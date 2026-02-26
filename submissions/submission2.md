## Lab 2 — Version Control (Git)


## Task 1: Understanding Version Control Systems (Git objects)

### 1. Make a few commits in this repository

Run:


```sh
git add submissions/submission2.md
git commit -m "[lab2] test submission"
```

**RESULT:**
```
commit 46b87576aea264fdf7677e500885702af5fb0441 (HEAD -> solution/lab2, origin/solution/lab2)
Author: Vagif Khalilov <v.khalilov@innopolis.university>
Date:   Mon Feb 23 16:55:33 2026 +0300

    [lab2] test submission
```


### 2. Inspect commit / tree / blob with `git cat-file`

1) Pick a commit hash (use the latest one):

```sh
git cat-file -p 46b87576aea264fdf7677e500885702af5fb0441
```

**RESULT:**

```
tree 4228c584ca05c0f57b375fe00cc5a34ae7a9727d
parent 41ce254f2a50a42c377a1310bb2abf7ed431dd0b
author Vagif Khalilov <v.khalilov@innopolis.university> 1771854933 +0300
committer Vagif Khalilov <v.khalilov@innopolis.university> 1771854933 +0300
gpgsig -----BEGIN SSH SIGNATURE-----
 U1NIU0lHAAAAAQAAADMAAAALc3NoLWVkMjU1MTkAAAAgWVsjVNFk1dXfympsEaT4NMxW/0
 6jC01KG7DYnDBM+J4AAAADZ2l0AAAAAAAAAAZzaGE1MTIAAABTAAAAC3NzaC1lZDI1NTE5
 AAAAQAN5J/ta0crTnUKY0Xtx4Tqk0VjG1c7qPQ6N4adSj8kY1x6TW6zNVa0ac8hBCxOrKc
 wzYQjfh5kYxJWwpyRcCwc=
 -----END SSH SIGNATURE-----

[lab2] test submission
```

2) From that output, copy the `tree <TREE_HASH>` value and inspect it:

```sh
git cat-file -p 4228c584ca05c0f57b375fe00cc5a34ae7a9727d
```

**RESULT:**

```
100644 blob 5fa4cfc0b07958b54ab2e66bc4528a40d76e97cf    README.md
040000 tree c55a0c4f99e7e3cc6ca366dfdcc41953d1e957ff    app
100644 blob b9006b6317d05422caa8d292a810282644b984b3    lab1.md
100644 blob 2d91667659368994cb7ff71d689fc47b03bc0064    lab10.md
100644 blob 77e299c4cdb01bc31607bef4e2036b56c3368515    lab2.md
100644 blob ee82f41affad8007c973c86a36a1a765332c68a8    lab3.md
100644 blob 923c82cd690e7ba71fd20bce5f3cce764e4aec75    lab4.md
100644 blob 7fa74d0afa92c28f14b2987a27ea0e92e96f7967    lab5.md
100644 blob 5bcf6fd50e77ac7a5fcb442b7b2b445927cdb160    lab6.md
100644 blob 3447ddcd6810618a6294a2fced5d03876d50663a    lab7.md
100644 blob e75a33bed2bc6a6d76a6363731dbd711195090fc    lab8.md
100644 blob 8f6ac78dfeeabb3b755657b3dc752ea6ee219253    lab9.md
040000 tree bf3126b874ded144987264d82899bc8114fd3f00    submissions
```

3) From the tree output, pick a file entry and copy its blob hash, then inspect the blob:

```sh
git cat-file -p bf3126b874ded144987264d82899bc8114fd3f00
```

**RESULT:**

```
100644 blob a60687568ec738e691eb455dd06f5d6a2efcfbc6    submission1.md
100644 blob adf5d3a4a980bace75b16fb41c328c3b73e8e79c    submission2.md
```

### Short explanation
- **Commit object**: points to a tree (snapshot), stores author/committer info and message, and links to parent commit(s).
- **Tree object**: represents a directory listing (filenames + modes + hashes of blobs/other trees).
- **Blob object**: stores raw file content (bytes)

---

## Task 2: Practice with `git reset` and `reflog`

### 1. Create a practice branch

```sh
git checkout -b git-reset-practice
```

### 2. Create three commits on that branch

```sh
echo "First commit" > file.txt
git add file.txt
git commit -m "First commit"

echo "Second commit" >> file.txt
git add file.txt
git commit -m "Second commit"

echo "Third commit" >> file.txt
git add file.txt
git commit -m "Third commit"
```

Show state:
```
f7dd8a6 (HEAD -> git-reset-practice) Third commit
313d41e Second commit
b78c813 First commit
46b8757 (origin/solution/lab2, solution/lab2) [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
```

### 3. `git reset --soft` (keeps changes staged)

```sh
git reset --soft HEAD~1
git log --oneline -n 5
```

**RESULT:**
```
313d41e (HEAD -> git-reset-practice) Second commit
b78c813 First commit
46b8757 (origin/solution/lab2, solution/lab2) [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
f4904ae [lab1] minor fix2
```

`--soft` moves HEAD/branch back, but keeps the reverted commit changes in the **staging area**.

### 4. `git reset --hard` (discards changes)

```sh
git reset --hard HEAD~1
git log --oneline -n 5
```

**RESULT :**

```
b78c813 (HEAD -> git-reset-practice) First commit
46b8757 (origin/solution/lab2, solution/lab2) [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
f4904ae [lab1] minor fix2
dbb48e6 [lab1] minor fix
```

`--hard` moves HEAD/branch back **and** resets both staging area and working directory to match that commit

### 5. Use `git reflog` to recover

```sh
git reflog --date=iso
```

**RESULT:**

```
b78c813 (HEAD -> git-reset-practice) HEAD@{2026-02-23 17:24:10 +0300}: reset: moving to HEAD~1
313d41e HEAD@{2026-02-23 17:21:24 +0300}: reset: moving to HEAD~1
f7dd8a6 HEAD@{2026-02-23 17:20:56 +0300}: reset: moving to HEAD
f7dd8a6 HEAD@{2026-02-23 17:18:57 +0300}: commit: Third commit
313d41e HEAD@{2026-02-23 17:18:57 +0300}: commit: Second commit
b78c813 (HEAD -> git-reset-practice) HEAD@{2026-02-23 17:18:57 +0300}: commit: First commit
46b8757 (origin/solution/lab2, solution/lab2) HEAD@{2026-02-23 17:18:38 +0300}: checkout: moving from solution/lab2 to git-reset-practice
46b8757 (origin/solution/lab2, solution/lab2) HEAD@{2026-02-23 16:55:33 +0300}: commit: [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) HEAD@{2026-02-23 15:52:35 +0300}: checkout: moving from solution/lab1 to solution/lab2
```


Recover 46b8757 commit:

```sh
git reset --hard 46b8757
```

**RESULT:**
```
46b8757 (HEAD -> git-reset-practice, origin/solution/lab2, solution/lab2) [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
f4904ae [lab1] minor fix2
dbb48e6 [lab1] minor fix
be17070 [lab1] new signed commit
```


## Task 3: Visualizing Git commit history

### 1. Make 3 commits (on your current branch)

```sh
echo "Commit A" > history.txt
git add history.txt
git commit -m "Commit A"

echo "Commit B" >> history.txt
git add history.txt
git commit -m "Commit B"

echo "Commit C" >> history.txt
git add history.txt
git commit -m "Commit C"
```

### 2. Show commit graph

```sh
git log --oneline --graph --all
```

**RESULT:**

```
* 0a652b5 (HEAD -> solution/lab2) Commit C
* 8709f60 Commit B
* 32805ef Commit A
* 46b8757 (origin/solution/lab2, origin/git-reset-practice, git-reset-practice) [lab2] test submission
* 41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
* f4904ae [lab1] minor fix2
* dbb48e6 [lab1] minor fix
* be17070 [lab1] new signed commit
* a5e7c81 [lab1] final solution
* fc85200 [lab1] signed commit
* ad0978d (origin/master, origin/HEAD, master) Update lab10
* 70d6e4c Update lab10
```

### Commit messages list

**RESULT:**

```
0a652b5 (HEAD -> solution/lab2) Commit C
8709f60 Commit B
32805ef Commit A
46b8757 (origin/solution/lab2, origin/git-reset-practice, git-reset-practice) [lab2] test submission
41ce254 (origin/solution/lab1, solution/lab1) [lab1] final solution
f4904ae [lab1] minor fix2
dbb48e6 [lab1] minor fix
be17070 [lab1] new signed commit
a5e7c81 [lab1] final solution
fc85200 [lab1] signed commit
```

### Reflection
The commit graph helps me quickly understand where branches diverged/merged and in what order changes happened. This is useful in collaboration because it makes reviews, debugging, and release tracking easier.

---

## Task 4: Tagging a commit

### 1. Create and push a tag

```sh
git tag v1.0.0
git show --oneline -s v1.0.0
git push origin v1.0.0
```

**RESULT:**

```
0a652b5 (HEAD -> solution/lab2, tag: v1.0.0) Commit C
```

### What I created
- **Tag name**: `v1.0.0`
- **Associated commit hash**: 0a652b5

### Why tags are useful (short)
Tags mark important points in history (like releases), and they are often used for versioning, release notes, and CI/CD triggers.


