# Precommit-hook-test

- Install trufflehog
  ```bash
  $ brew install trufflehog
  ```

- Setup a python virtual environment and install dependencies
  ```bash
  # setup
  $ python3 -m venv .venv

  # activate
  $ source .venv/bin/activate

  # Optionally upgrade pip if needed
  $ pip install --upgrade pip

  # install pre-commit package
  $ pip install pre-commit
  ```

- Create a `.pre-commit-config.yaml` file in the root of your directory with contents below. This holds all the necessary steps/hooks that run on every commit. Feel free to modify hooks according to your project specifications. See creating new hooks [guide](https://pre-commit.com/#new-hooks) for more.
  ```yaml
  repos:
  -   repo: https://github.com/pre-commit/pre-commit-hooks
      rev: v2.3.0
      hooks:
      -   id: check-yaml
      -   id: end-of-file-fixer
      -   id: trailing-whitespace

  -   repo: https://github.com/psf/black
      rev: 22.10.0
      hooks:
      -   id: black
          stages: ["commit", "push"]

  -   repo: local
      hooks:
      -   id: trufflehog
          name: TruffleHog
          description: Detect secrets in your data.
          entry: bash -c 'trufflehog git file://. --since-commit HEAD --fail'
          language: system
          stages: ["commit", "push"]
  ```

- Install the git hook scripts
  ```bash
  $ pre-commit install
  ```
  *notice that a pre-commit file has now been created in `.git/hooks/pre-commit`*

- Test committing a file to see the pre-commithook in action.
