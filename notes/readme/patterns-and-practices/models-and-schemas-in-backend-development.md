---
icon: square-list
---

# Models & Schemas in Backend Development

* 🏗️ **Models** = Database structure & operations (SQLAlchemy)
* 📋 **Schemas** = Data validation & API interface (Pydantic)
* 🤝 **Together** = Clean, secure, and maintainable backend

***

### 🎯 Why This Matters

Think of models as your database architect and schemas as your API security guard. Together, they create a robust and reliable backend system.

### 🏛️ Models: The Database Blueprint

#### Purpose

* Define database table structure
* Handle database operations (CRUD)
* Manage relationships between tables

#### Example

```python
from sqlalchemy import Column, Integer, String, Float
from database import Base

class Product(Base):
    __tablename__ = "products"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    price = Column(Float)
    
    def calculate_discount(self, percentage):
        return self.price * (1 - percentage/100)
```

### 💂‍♂️ Schemas: The API Guardian

#### Purpose

* Validate incoming data
* Format outgoing responses
* Document API structure
* Transform data between API and database

#### Example

```python
from pydantic import BaseModel, Field
from typing import Optional

class ProductBase(BaseModel):
    name: str
    price: float = Field(..., gt=0)  # Price must be greater than 0

class ProductCreate(ProductBase):
    category: Optional[str] = None

class ProductResponse(ProductBase):
    id: int
    discount_price: Optional[float]

    class Config:
        orm_mode = True
```

### 🌉 The Bridge: Making Them Work Together

#### 1. Basic Flow

```python
@app.post("/products/", response_model=ProductResponse)
def create_product(product: ProductCreate):
    # 1. Pydantic validates incoming data
    # 2. Convert to database model
    db_product = Product(**product.dict())
    # 3. Save to database
    db.add(db_product)
    db.commit()
    # 4. Return response (automatically converted thanks to orm_mode)
    return db_product
```

#### 2. Advanced Features

**Nested Validation**

```python
class Category(BaseModel):
    name: str
    description: Optional[str]

class ProductWithCategory(ProductBase):
    category: Category  # Nested validation
```

**Custom Validators**

```python
from pydantic import validator

class ProductCreate(BaseModel):
    name: str
    price: float

    @validator('price')
    def price_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError('Price must be positive')
        return v
```

### 🎯 Best Practices

#### 1. File Structure

```
src/
├── models/
│   ├── __init__.py
│   ├── product.py
│   └── user.py
└── schemas/
    ├── __init__.py
    ├── product.py
    └── user.py
```

#### 2. Schema Inheritance

```python
# Base schema with common fields
class ProductBase(BaseModel):
    name: str
    price: float

# Input schema
class ProductCreate(ProductBase):
    category_id: int

# Output schema
class Product(ProductBase):
    id: int
    created_at: datetime
```

#### 3. Model Relationships

```python
class Category(Base):
    __tablename__ = "categories"
    id = Column(Integer, primary_key=True)
    products = relationship("Product", back_populates="category")

class Product(Base):
    __tablename__ = "products"
    category_id = Column(Integer, ForeignKey("categories.id"))
    category = relationship("Category", back_populates="products")
```

### 🎨 Pro Tips

1. **Always Use Type Hints**
   * Makes code more readable
   * Enables better IDE support
   * Catches errors early
2. **Leverage Pydantic Features**
   * Field validations
   * Custom validators
   * Config settings
3. **Keep It DRY**
   * Use inheritance for common fields
   * Create base classes for common functionality
   * Use mixins for shared behavior

### 🎉 When to Use What

#### Use Models For:

* Database structure definition
* Complex queries
* Data relationships
* Business logic related to data

#### Use Schemas For:

* API request/response formatting
* Input validation
* Data transformation
* API documentation

### 📝 Final Note

Remember: Models and schemas are your friends in maintaining clean, secure, and efficient backend code. Models handle how data lives in your database, while schemas ensure data integrity in your API interactions.
