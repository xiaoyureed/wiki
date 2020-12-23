---
title: Github Actions demos
---

http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html

<!-- more -->

## concept

（1）workflow （工作流程）：持续集成一次运行的过程，就是一个 workflow。configured with a yml file located in ./github/workflows/, every workflow has a single file

（2）job （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。

（3）step（步骤）：每个 job 由多个 step 构成，一步步完成。

（4）action （动作）：每个 step 可以依次执行一个或多个命令（action）

## yml config

```yml
# This is a basic workflow to help you get started with Actions
# name can be specified  by yourself and  it will display in actions tab
# optional , and if ignored , the file name will be take as its value
name: CI

# Controls when the action will run.
# conditions
#eg:
# on: push
# on: [push, pull_request]

# specify a branch
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    # it's  an array
    branches: [main]
  pull_request:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
# jobs.<job_id>.name
jobs:
  # This workflow contains a single job called "build"
  #the  key  string   can  be specified  yourself
  build:
    name: xxxx
    #jobs.<job_id>.needs:  指定当前任务的依赖关系，即运行顺序
    needs:
    # The type of runner that the job will run on
    #ubuntu-latest，ubuntu-18.04或ubuntu-16.04
    #windows-latest，windows-2019或windows-2016
    #macOS-latest或macOS-10.14
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    #jobs.<job_id>.steps , is  an array , which has three property : name (自定义名称), run(该步骤运行的命令或者 action) , env (该步骤所需的环境变量)
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      #   use the third party actions
      # 语法: 可以指定版本 , 分支
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
  my-job:
    name: My Job
    runs-on: ubuntu-latest
    steps:
      - name: Print a greeting
        env:
          MY_VAR: Hi there! My name is
          FIRST_NAME: Mona
          MIDDLE_NAME: The
          LAST_NAME: Octocat
        run: |
          echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```

## apply for a github api  access token 

https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token





