# Software development in MF-DAS

With this tutorial we want to guide you through a tested and functional way of starting your new Python project in MF-DAS. This will hopefully help you and others in understanding and modifying the software you created :)

## Agenda
- Learn how to create, build and install a Python package
- Learn about the GitHub Flow
- Test Driven Development and testing with pytest
- Documenting code
- Publishing a Python package
- Static code analysis tools
- Design by Contract using Deal
Every lesson also include elements of CI/CD since we are going to **automate** all the steps.

## Create, build and install a Python package
This is the the structure we conceive the modern one for a Python project inside MF-DAS

```
├── your_project
│   ├── LICENSE (empty)
│   ├── pyproject.toml
│   ├── setup.cfg (empty)
│   ├── README.md (empty)
│   ├── src/
│   │   ├── example_package/
│   │   │   ├── __init__.py (empty)
│   │   │   ├── example.py
│   ├── tests/
│   │   ├── test_main.py (empty)
```

To start, create these directories and empty files. Tip: if you're working from linux-like terminal, you can create a new empty file with:

```
touch new_file.extension
```

Let's then populate the project configuration file *pyproject.toml*. We will set it to initially install numpy and pandas:

```
[project]
name = "hello-world"
version = "1.0.0"
description "My first Python package"
requires-python = ">=3.8"
authors = [
  {name = "John Doe", email = "john@example.com"},
]
dependencies = [
  "numpy",
  "pandas"
]
```

If your project is structured as above, then build it with:

```
python3 -m pip install --upgrade build
python3 -m build
```

This last command builds the binary distribution package of your project, called wheel. Wheels are a packaging standard that allows for faster installations and a more efficient distribution process of your software.

So we are now ready to install the wheel with pip install:

```
pip install dist/hello_world-1.0.0-py3-none-any.whl
```

Great, you created your package! Let's now create the package version 1.0.1, which acctually contain some code to run :)

Let's put an hello world function inside src/example_package/example.py:

```
def hello():
	print("Hello world!")
```

Now let's update the version of our package in the pyproject.toml file:

```
...
version = "1.0.1"
...
```

Build the new package

```
python3 -m build
```

And install it (the old version is automatically disinstalled):

```
pip install dist/hello_world-1.0.1-py3-none-any.whl
```

Great! Let's now test our package. Open the python3 console:

```
python3
```

Import our module and run the hello function:

```
>>> from example_package import example
>>> example.hello()
Hello world!
```

## The GitHub Flow

Version control enables collaborative development because it:
- Tracks who made changes in the code and what they are
- Allows you to revert back to a previous version of the code
- Allows parallel versions of the software to be developed at the same time and then merged to a single version later

**Git** seems to have eliminated all other version control systems and is pretty much the only game in town. Some terminology:

- *repository*: source code along with the full change history
- *clone*: your own copy of the repository
- *fork*: a copy of the repository that is being independently maintained
- *branch*: a divergent change history that is maintained in parallel with other branches
- *commit*: a smallest unit of change in the source code
- *merge*: incorporate changes from one branch into another one
- *conflict*: occurs when two branches have changes to the same file
- *pull (merge) request*: a request to incorporate changes from a branch to the main branch of the project
- *check out*: update to your local copy of the repository
- *rebase*: incorporate changes made in the main branch to your branch

### Initialize Your GitHub Repository

GitHub is a cloud-based platform that provides hosting for Git repositories, enabling version control, collaborative software development and project management, through their servers. 
To store our Git repository on GitHub we first need to setup authentification correctly. Right now easiest seems to be through the [use of an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

After setting up authentification, create a [new *empty* repository on GitHub](https://github.com/new). Do NOT add any file (e.g. README, .gitignore, etc.) from the web platform.

Now, from the root folder of your project:

```
git init -b main
git add .
git commit -m "first commit"
git remote add origin git@github.com:/yourusername/name-of-your-github-repo.git
git push -u origin main
```

Congratulations, you just pushed on GitHub your Python package. To finish this section, let's just add a small modification to the package and push the changes to GitHub. We will simply create a README.md file for our package:

```
echo "# Hello World package" > README.md
```

Then:

```
git add README.md
git commit -am "Created README.md"
git push
```

### Setting Up CI/CD for Your GitHub Repository

CI/CD stands for Continous Integration/Continous Deployment. CI/CD involves the automated testing, building, and deployment processes integrated directly into a GitHub repository. When a developer makes changes to the Python package's code, GitHub CI/CD automatically triggers a series of actions, including running tests to ensure the code's integrity and functionality. If the tests pass, the CI/CD pipeline proceeds to build the package and, in a Continuous Deployment setup, deploys it to a specified environment. This automated workflow helps catch errors early in development, ensures consistent code quality, and facilitates efficient and reliable delivery of updates to users. For Python packages, GitHub CI/CD is crucial as it streamlines the development lifecycle, reduces manual intervention, and enables swift and reliable delivery of improvements or new features to end-users.

In order to set up CI/CD for our package, let move to our repository webpage on GitHub. Then click "Actions" in the repo header.So *Configure* Python package:
