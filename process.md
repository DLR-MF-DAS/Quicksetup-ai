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
