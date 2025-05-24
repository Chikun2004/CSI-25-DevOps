# Create New Branch

 git checkout -b Aditya

# Make Changes

  adding a Hello World Code into it:-

  public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

# Commit Changes

git add .
git commmit -m "Implemented Hello World Program Into It"

# Push Changes

git push origin Aditya

# Create a Pull Request

gh pr create --base master --head feature-branch --title "Add feature X" --body "This PR adds feature X and fixes issue Y."


# Update Local Repository

git checkout master
git pull origin master

# Delete Branch (Optional)

git branch -d Aditya
