# Git practice

## Setup VSCode

1. Ensure that you install the `gitgraph` extension

## Steps

1. Fork this repository to your own Github account
2. Clone the forked repository twice.

    ```bash
    $ git clone https://github.com/<YOUR_GITHUB_NAME>/calculator-git-pracitce calculator-1
    $ git clone https://github.com/<YOUR_GITHUB_NAME>/calculator-git-pracitce calculator-2
    ```

3. Open the 2 local repository with `vscode`
4. You can see that `calculator.js` provides 2 function `sum` and `minus`
    1. `sum` - Takes in 2 number and sum them together (e.g. `sum(2,3) = 5`)
    2. `minus` - Takes in 2 number, x and y, and minus y from x (e.g. `minus(5,3) = 2`)
5. If you observe their implementation, it is currently unnecessarily complicated, let us modify it to simplify the implementation.
    1. Do pay attention to which repository (`calculator-1` or `calculator-2`) you are supposed to perform the operation.
6. (`calculator-1`) Let's modify `sum` to make use of the `+` operator:

    ```
    function sum(x, y) {
        return x + y;
    }
    ```

7. (`calculator-1`) Stage this change, and add a new commit "Make `sum(x, y)` use + operator"
    1. To stage:
        1. Either enter `git add calculator.js` OR
        2. Use VSCode's built in source control
    2. To commit:
        1. Either enter `git commit -m "Make...."` OR
        2. Use VSCode's built in source control
8. (`calculator-1`) Let's also modify the `minus` function to make use of the `-` operator.

    ```js
    function minus(x, y) {
        return x - y;
    }
    ```

9. (`calculator-1`) Similarly, stage the changes and make a commit.
10. (`calculator-1`) Open up gitgraph, and you should observe that you have 3 commits in total
    1. initial
    2. Make add ...
    3. Make minus ...
11. (`calculator-1`) Type the following to view the remotes configured on your local repository

    ```bash
    git remote -v
    ```

    You should observe that there are 2 remote, 1 for push and 1 for pull, but both are named `origin` and are targeting at the same github url.

12. (`calculator-1`) Push the changes onto `origin` at the master branch.

    ```bash
    git push origin master

    ```

13. (`calculator-2`) Move to `calculator-2`
14. (`calculator-2`) Open up gitgraph, you should observe that there are 3 commits
    1. Initial (master)
    2. Make add....
    3. Make minus.... (origin/master)
15. (`calculator-2`) Suppose we like the way the the `add` is done. Let us merge our `master` branch with the `Make add....` commit.

    ```bash
    git merge <COMMIT_HASH>
    ```

    > Find out what is the <COMMIT_HASH> of the `Make add....` commit
    > It is a combination of alphabets and numbers (e.g. jd7s6a9nd9s8...)

    Alternatively, you can right click on the particular commit in `gitgraph` and press merge.

16. (`calculator-2`) You should observe that there are still 3 commits
    1. Initial
    2. Make add.... (master)
    3. Make minus.... (origin/master)
17. (`calculator-2`) Recall in the video that it is mentioned that merging will create a new commit, what you observed is a fast-forward merge, [you can read more about it here](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging).
18. (`calculator-2`) Let us now modify the `minus` function. We know that we can rewrite a subtraction in the following way:

    ```
    x - y = x + (-y)
    ```

19. (`calculator-2`) Let us modify the `minus` function so that we are adding `x` with `-y` using the `sum` function.

    ```js
    function minus(x, y) {
        return sum(x, -y);
    }
    ```

20. (`calculator-2`) Save the changes, stage and commit with message "Use `sum` to do `minus`".
21. (`calculator-2`) Head back to `gitgraph` and you should observe the following commits

    ```
    initial - Make add... - Use sum to do minus (master)
            \
             - Make minus... (origin/master)
    ```

22. (`calculator-2`) Try pushing the changes to origin now.

    ```
    git push origin master
    ```

    You should receive an error mentioning `"not up-to-date"`

23. (`calculator-2`) This tells us that we need to pull the changes from origin first. (Error will occur, it's expected)

    > Recall that `pull` is a combination of `fetch` and `merge`

    ```
    git pull origin master
    ```

24. (`calculator-2`) You should observe a message mentioning a "Conflict". The reason is because there are now 2 possible `minus` implementation and git is not able to resolve it automatically.
25. (`calculator-2`) let's open the `calculator.js` file and you should observe something of the following:

    ```git
    function minus(x, y) {
    >>>>>>>> HEAD
        return sum(x, -y);
    ========
        return x - y;
    >>>>>>>> origin/master
    }
    ```

26. (`calculator-2`) This shows you the 2 possible implementation of `minus` and you are to manually fix it by modifying the file. Let's modify the file such that we use the `x - y` implementation.
    1. Remove the lines with `>>>>>>` and `========`
    2. Remove the line with `sum(x, -y)`
27. (`calculator-2`) Save the file, stage the change and commit (without -m).
28. (`calculator-2`) Return back to `gitgraph`, you should observe the following commits:

    ```
    initial - Make add... - Use sum to do minus (master) - Merge ...
            \                                             /
             - Make minus... (origin/master) ------------
    ```

    That is, a new merge commit has been created.

29. (`calculator-2`) Now push the changes back to the remote.
30. (`calculator-1`) Head back to `calculator-1`
31. Open up `gitgraph` you should now observe a graph with all the relevant commits, if not, do a fetch.
32. Check your understanding & further exploration:
    1. Is `calculator-1`'s master branch `ahead` or `behind` origin's master branch? (Ans: ahead)
    2. If I make another commit in `calculator-1` before pulling, how will the graph look like? Ans:
        ```
        initial - Make add... ------ Make minus... - new Commit (master)
                \                                         \
                    - Use sum to do minus (origin/master) - Merge ...
        ```
    3. The video mentioned that "commits are like save points in a game", How do we load the version on `initial`? Or, how do we move `HEAD` back to initial? (Ans: `git checkout <INITIAL_COMMIT_HASH>`)
    4. The video mentioned that "we can add labels to commits (e.g. master)", how do we add a new label, "hello", to our current commit? (Ans: git branch hello)
        1. You should be able to see another label on your current commit on `gitgraph`.
