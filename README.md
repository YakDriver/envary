# envary
A project to make Pipenv work across many environmental needs based on platform, Python version, and a variety of custom environments (e.g., install, build, test).

# Setup

## Step 1: Write the YAML file

Put all package requirements for each environment in one file called `.envary.yml`. This YAML file will list a default environment and dependency information for each environment.

Version numbers of packages should be excluded as much as possible from `.envary.yml`. Pipfiles will be generated based on the YAML and version numbers can be manually and automatically adjusted (e.g., Dependabot) there.

For example, if you wanted to control Python package version numbers independently based on versions of Python, you could include the YAML below `.envary.yml`. It looks a little strange but would generate two Pipfiles where versions could be separately manually (or automatically) adjusted for each Python version.

```yaml
  - environment: test
    python_version:
    - equal: 2.6
      packages:
      - pytest
    - equal: 2.7
      packages:
      - pytest
```

An example `.envary.yml` file: https://github.com/YakDriver/envary/blob/master/.envary.yml

## Step 2: Generate Pipfiles

Run a command like `envary init`.

Envary will read the `.envary.yml` file and generate all the necessary Pipfiles (including locks). (Semi-complicated tree traverse.)

The parent directory is `.envary`, but every Pipfile gets its own subdirectory. All the Pipfiles should be included in version control. With separate Pipfiles, Dependabot can manage dependencies.

For example, the directory structure might look something like this for the YAML above:

```
project
│   .envary.yml
│
└───.envary
│   │
│   └───test-py-eq-2-6
│   |   │   Pipfile
│   |   │   Pipfile.lock
│   |
│   └───test-py-eq-2-7
│       │   Pipfile
│       │   Pipfile.lock
```


## Step 3: Manually massage Pipfiles

Avoid placing version numbers in `.envary.yml` as much as possible so that Dependabot can help manage versions of all the Pipfiles. Instead, manually and/or automatically massage versions in Pipfiles. Then run a command like `envary lock` to generate new lock files.

# Use

## Step 4: Use Envary

Run a command like `envary --env=install`.

Based on the platform, Python version, and environment provided (e.g., `install`), Envary will use the correct Pipfile to launch a pipenv (i.e., virtualenv) with the exact packages provided in the lock file.
