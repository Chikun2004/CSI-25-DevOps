# Undo the Last Commit (Keep changes locally)

git reset --soft HEAD~1

# Undo the Last Commit and Discard Changes

git reset --hard HEAD~1

# Undo the Last Commit and Push the Undo to Remote

git reset --hard HEAD~1
git push origin HEAD --force

# Remove the Last Created File From Remote Repo

git rm path/to/filename
git commit -m "Remove unwanted file"
git push origin master

