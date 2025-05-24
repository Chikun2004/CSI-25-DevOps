# Branching in Git

Branching allows you to create independent lines of development.

Use branches to work on features or fixes without affecting the main code (master or main).

# Create a Branch 

git checkout -b Node

# Switch between branches

git checkout master
git checkout Node

# Merging in Git

Merge incorporates changes from one branch into another.

# Merge Node into master

git checkout master
git merge Node

# Resolving Merge Conflicts

When Git can’t automatically merge changes because the same lines were modified differently, you get a merge conflict.

Steps to resolve:

1.Git will mark conflicting files as unmerged.

2.Open conflicted files. You will see conflict markers like:

<<<<<<< HEAD
Your changes on master
=======
Changes from Node
>>>>>>> Node

3.Manually edit the file to resolve conflicts by choosing or combining changes.

4.Mark the conflict resolved by adding the file:

git add path/to/conflicted-file

5.Complete the merge by committing:

git commit 