# test-workflows
Testing workflows

## Example GitHub Actions Workflow

This repository contains an example GitHub Actions workflow located at `.github/workflows/example.yml`.

### Workflow Features

The example workflow demonstrates several key GitHub Actions features:

1. **Multiple Trigger Events**
   - Runs on pushes to `main` and `develop` branches
   - Runs on pull requests targeting the `main` branch

2. **Three Different Jobs**
   - **Test Job**: Basic environment setup and testing
   - **Build Job**: Creates build artifacts (depends on test job)
   - **Multi-OS Test**: Runs tests across Ubuntu, Windows, and macOS

3. **Common Actions Used**
   - `actions/checkout@v4`: Checks out the repository code
   - `actions/setup-node@v4`: Sets up Node.js environment
   - `actions/upload-artifact@v4`: Uploads build artifacts

4. **Key Concepts Demonstrated**
   - Job dependencies (`needs` keyword)
   - Matrix builds for multiple operating systems
   - Environment variables and GitHub context
   - Artifact creation and upload
   - Cross-platform shell scripting

### How to Use

The workflow will automatically run when:
- Code is pushed to the `main` or `develop` branch
- A pull request is opened targeting the `main` branch

You can also manually trigger the workflow from the GitHub Actions tab in the repository.
