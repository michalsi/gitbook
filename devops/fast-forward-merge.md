---
icon: merge
---

# Fast-forward merge

### 🧠 Concept Overview

#### 1. **Fast-forward merge**

A **fast-forward merge** happens when the branch you’re merging _can be reached directly_ from the branch you’re merging into — i.e., there are **no new commits** on the target branch since the branching point.

In that case, Git just moves the branch pointer forward (no new merge commit is created).

***

#### 2. **Divergent branches**

Branches are **divergent** when both branches have new commits since they split.\
That’s when you can’t do a fast-forward merge, because the history has _diverged_. You’ll need a **merge commit** to reconcile them.

***

### 🧩 Visuals

#### Case 1: **Fast-forward merge**

```
A---B---C  (main)
           \
            D---E  (feature)
```

➡️ If no new commits are added to `main`, merging `feature` into `main` is simple:

```
git checkout main
git merge feature
```

Git just **moves `main` forward** to `E`:

```
A---B---C---D---E  (main, feature)
```

✅ No merge commit — just a _fast-forward_.

***

#### Case 2: **Divergent branches (non-fast-forward)**

If both `main` and `feature` have new commits:

```
A---B---C---F---G   (main)
           \
            D---E   (feature)
```

Now histories **diverged** at commit `C`.\
To merge:

```
git checkout main
git merge feature
```

Git must create a **merge commit** `M` to combine both histories:

```
A---B---C---F---G---M   (main)
           \         /
            D-------E   (feature)
```

🧩 This is called a **non-fast-forward merge**, because Git can’t simply move a pointer forward — it must integrate changes from both lines of development.

***

#### 📊 Concept Summary Table

| Scenario                       | Relationship between branches | Merge Type       | Result               |
| ------------------------------ | ----------------------------- | ---------------- | -------------------- |
| Branch can be reached directly | Linear                        | Fast-forward     | No merge commit      |
| Branches diverged              | Non-linear                    | Non-fast-forward | Merge commit created |
