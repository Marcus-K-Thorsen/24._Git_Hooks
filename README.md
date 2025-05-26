# Git Hooks

## What are Git Hooks?

Git hooks are scripts that Git runs automatically at certain points in your workflow, such as before you commit or push code. They allow you to automate tasks like checking code style, running tests, or displaying messages. Git hooks are local to your repository and are not shared with others unless you distribute them separately.

---

## Step-by-Step: Creating a Simple Pre-commit Hook

**This guide is for the `24._Git_Hooks` folder and uses Windows PowerShell.**

1. **Open PowerShell** and navigate to your `24._Git_Hooks` folder (this is your "project root" for this exercise):
    ```powershell
    cd "C:\Users\Marcus\Desktop\KEA\KEA_Studie\System Integration\Marcus_K_Thorsen_System_Integration\24._Git_Hooks"
    ```

2. **Initialize a Git repository** (if you haven't already, in the `24._Git_Hooks` folder):
    ```powershell
    git init
    ```

3. **Navigate to the hooks folder** (inside the `.git` folder in `24._Git_Hooks`):
    ```powershell
    cd .git\hooks
    ```

4. **Create or edit the `pre-commit` file** (still in `.git\hooks`):
    - If a `pre-commit.sample` file exists, rename it:
      ```powershell
      mv pre-commit.sample pre-commit
      ```
    - Or create a new file:
      ```powershell
      notepad pre-commit
      ```
    - Add the following lines and save:
      ```sh
      #!/bin/sh
      echo "Hello, world!"
      ```

5. **(Optional) Make the hook executable:**  
   On Windows, this step is usually not needed, but if using Git Bash:
    ```sh
    chmod +x pre-commit
    ```

6. **Test your hook:**
    - Go back to your `24._Git_Hooks` folder:
      ```powershell
      cd ../..
      ```
    - Make a change to any file in `24._Git_Hooks`, then run:
      ```powershell
      git add .
      git commit -m "Test pre-commit hook"
      ```
    - You should see `Hello, world!` printed before the commit completes.

---

## How to Change the Message

To change what is echoed when you commit, simply edit the `.git/hooks/pre-commit` file (inside `24._Git_Hooks/.git/hooks/`) and replace `"Hello, world!"` with any message you want. For example:

```sh
#!/bin/sh
echo "Don't forget to check your code style!"
```

Save the file. The next time you commit in the `24._Git_Hooks` folder, your new message will appear.

---

**Summary:**  
- Git hooks let you automate actions in your Git workflow.
- The `pre-commit` hook can be used to display messages or run checks before a commit.
- Edit the `pre-commit` file to change the message or add more commands.
---


## Step-by-Step: Adding Linting with StandardJS

**This will make sure you can't commit JavaScript files with formatting errors in the `24._Git_Hooks` folder.**

1. **Go to your `24._Git_Hooks` folder in PowerShell:**
    ```powershell
    cd "C:\Users\Marcus\Desktop\KEA\KEA_Studie\System Integration\Marcus_K_Thorsen_System_Integration\24._Git_Hooks"
    ```

2. **Install StandardJS locally:**
    ```powershell
    npm install --save-dev standard
    ```

3. **Edit your `.git/hooks/pre-commit` file** (open it in Notepad or VS Code):
    ```sh
    #!/bin/sh
    # Find all staged JS files and lint them with StandardJS
    STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.jsx?$')
    if [ "$STAGED_FILES" = "" ]; then
      exit 0
    fi

    npx standard $STAGED_FILES
    if [ $? -ne 0 ]; then
      echo "StandardJS found formatting errors. Commit aborted."
      exit 1
    fi
    ```

4. **(Optional) Make the hook executable (if using Git Bash):**
    ```sh
    chmod +x .git/hooks/pre-commit
    ```

5. **Test the linting hook:**
    - Create a JavaScript file in `24._Git_Hooks` with a formatting error (e.g., `bad.js`).
    ```js
    const foo = 'bar'
    console.log(foo )
    ```
    - Stage it:
      ```powershell
      git add bad.js
      ```
    - Try to commit:
      ```powershell
      git commit -m "Test lint hook"
      ```
    - The commit should be blocked if there are lint errors, and you’ll see a message.
    - Run the fix command:
      ```powershell
      npx standard --fix
      ```
    - Then try and commit after the linting has been fixed.

---

## How it works

- When you try to commit in the `24._Git_Hooks` folder, the hook runs StandardJS on all staged JS files.
- If there are formatting errors, the commit is stopped and you see an error message.
- Fix the errors (or run `npx standard --fix`), then try again.

---

# Git hooks pros and cons

**Pros**
- <details><summary>Early Error Detection / Immediate Feedback</summary>Hooks can run checks or tests before code is committed, so you catch mistakes right away instead of later in the process.</details>
- <details><summary>Prevents bad commits from being checked in</summary>Pre-commit hooks can block commits that don't meet your standards (e.g., failing tests or lint errors), keeping your repository cleaner.</details>
- <details><summary>Customizable Automation</summary>You can automate repetitive tasks (like formatting code or running scripts) to save time and ensure consistency.</details>
- <details><summary>Encourages Best Practices</summary>By enforcing checks, hooks help teams follow coding standards and workflows automatically.</details>

**Cons**
- <details><summary>Local Only</summary>Hooks are not shared with others by default, so each developer must set them up manually unless you use extra tools.</details>
- <details><summary>Manual Configuration</summary>Setting up and maintaining hooks requires manual steps, which can be forgotten or misconfigured.</details>
- <details><summary>Bypassable (with the `--no-verify` flag)</summary>Anyone can skip hooks by adding `--no-verify` to their git command, so enforcement is not absolute.</details>
- <details><summary>Cross-Platform Issues</summary>Hooks are often written as shell scripts, which may not work the same on Windows, macOS, and Linux.</details>
- <details><summary>Not Version Controlled</summary>Hooks are stored in the `.git/hooks` folder, which is not tracked by Git, so they don't travel with your repository unless you use special tools.</details>
- <details><summary>Can Slow Down Commits</summary>If hooks run time-consuming tasks, they can make the commit process slower and frustrate developers.</details>