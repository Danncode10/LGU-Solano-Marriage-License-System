# Git Commit Message Format

This project adheres to the Conventional Commits specification for git commit messages. This ensures a standardized commit history, which can be used for automated changelog generation, semantic versioning, and clearer understanding of changes.

## Format:

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

### Commit Message Structure:

*   **`<type>`:** Describes the kind of change that this commit is providing.
    *   `feat`: A new feature
    *   `fix`: A bug fix
    *   `docs`: Documentation only changes
    *   `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semicolons, etc.)
    *   `refactor`: A code change that neither fixes a bug nor adds a feature
    *   `perf`: A code change that improves performance
    *   `test`: Adding missing tests or correcting existing tests
    *   `build`: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
    *   `ci`: Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
    *   `chore`: Other changes that don't modify src or test files
    *   `revert`: Reverts a previous commit

*   **`<scope>` (optional):** A noun describing the section of the codebase affected. For example: `parser`, `compiler`, `api`, `auth`, `ui`, `docs`.

*   **`<short summary>`:** A very brief description of the change.
    *   Use the imperative, present tense: "change" not "changed" nor "changes"
    *   Don't capitalize the first letter
    *   No period (`.`) at the end

*   **`[optional body]`:** A longer explanation of the commit message.
    *   Use the imperative, present tense.
    *   Explain *what* and *why* the changes were made.

*   **`[optional footer(s)]`:** Used for referencing issues or breaking changes.
    *   `BREAKING CHANGE:`: Indicate backward-incompatible changes.
    *   `Closes #<issue-number>`: Reference a GitHub issue that the commit closes.

## Example:

```
feat(authentication): add user login functionality

This commit introduces the user login functionality, allowing users to authenticate
using their email and password. It integrates with AWS Cognito for secure
authentication.

Closes #123
```

This documentation ensures that all contributors maintain a consistent and readable commit history.

### Prompt:
```
From the changes, generate a Git commit message following the format in docs/git_commit_format.md.
Separate git add commands if needed for selective staging.
Ensure the commit message is clean, concise, and follows the workflow guidelines.
Dont run in terminal, just give me the message
```