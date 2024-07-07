# Fullstack Project Repository

This repository serves as a unified project that combines two independent repositories: `frontend-repo` and `backend-repo`. It includes a shared configuration file (`config.yaml`) that both submodules can read from.

## Steps to Set Up the Project

### 1. Create Independent Repositories

#### `frontend-repo`

1. **Create the repository on GitHub**.
2. **Add `client.py`**:
    ```python
    import yaml
    import os
    config_path = os.path.join(os.path.dirname(
    os.path.abspath(__file__)), '..', 'config.yaml')
    with open(config_path, 'r') as file:
        config = yaml.safe_load(file)

    if config.get('run_localhost'):
        print("Client running on localhost")
    else:
        print("Client running remotely")


    ```
3. **Commit and push the changes**.

#### `backend-repo`

1. **Create the repository on GitHub**.
2. **Add `server.py`**:
    ```python
    import yaml
    import os
    config_path = os.path.join(os.path.dirname(
    os.path.abspath(__file__)), '..', 'config.yaml')
    with open(config_path, 'r') as file:
        config = yaml.safe_load(file)

    if config.get('run_localhost'):
        print("Server running on localhost")
    else:
        print("Server running remotely")
    ```
3. **Commit and push the changes**.

### 2. Create `fullstack-repo` and Add Submodules

1. **Create `fullstack-repo` on GitHub**.
2. **Add `frontend-repo` and `backend-repo` as submodules**:
    ```bash
    git submodule add https://github.com/AbdullahAl1/frontend-repo.git
    git submodule add https://github.com/AbdullahAl1/backend-repo.git
    git submodule update --init --recursive
    ```
3. **Create the `config.yaml` file**:
    ```yaml
    # config.yaml
    run_localhost: true
    ```

### 3. Automate Submodule Updates with GitHub Actions

1. **Create a GitHub Actions Workflow(TESTING)**:
    - Add a `.github/workflows/update_submodules.yml` file in `fullstack-repo`:
    ```yaml
    name: Update Submodules

    on:
      push:
        branches:
          - main

    jobs:
      update-submodules:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout repository
          uses: actions/checkout@v2
          with:
            submodules: true

        - name: Update submodules
          run: |
            git submodule update --remote --merge
            git add .
            git commit -m "Update submodules"
            git push
          env:
    ```
