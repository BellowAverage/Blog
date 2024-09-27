# React & Django Configuration

Created: September 12, 2024 1:48 PM

### Introduction

This note only records several **essential and often-overlooked steps** involved in the process of configuring the connection between `React` and `Django`.

### Environment

`Windows 10 Python 3.10.11` on `Windows PowerShell`

### Configuring Process

On `Windows PowerShell`, this must be executed to activate `venv`.

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
venv\Scripts\Activate.ps1
```

The `djangorestframework` framework collects many necessary `django` packages.

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple djangorestframework
```

Start the `django` project as usual.

```bash
django-admin startproject ReactDjango
python .\manage.py startapp app01
```

After installing `node.js`, run this.

```bash
npm cache clean --force
npm install -g npm@latest
npx create-react-app reactapp
# cd ./reactapp (change directory to wherever reactapp is)
# better just cut the whole reactapp directory to the django project root folder
# then run:
npm run build
```