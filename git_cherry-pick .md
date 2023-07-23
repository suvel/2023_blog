# Git cherry-pick 🍒👌

cherry-pick is somthing I explored and using very recently. I came accross this feture of git when I had to promote certain commits from the develop to qa branch.

Let us assume we have multiple commits in **main** branch and we have to promote **c1** and **c3** to **qa**.

1. Go to **main brach**
2. Perform git log to get commit details, use the below command
   ```
   git log
   ```
   or use the below command , which is a prettier verision of the above command.
   ```
   git log --pretty=format:"%h%x09%an%x09%s"
   ```
   ![image](https://github.com/suvel/2023_blog/assets/40818146/fbf15e5d-207b-4a50-89fa-fe8be5510383)

  Copy or make a note of the 7dig number in the first column for the commit(s) you want to perform cherry-pick

3. check out to the **feture branch**
4. Perform cherry-pick
   ```
   git cherry-pick <commit-id>
   ```
   To verify, perform `git log` you should be seeing the commit under the feture branch.


## Reference
1. [Doc how to use git cherry-pick](https://www.atlassian.com/git/tutorials/cherry-pick).
