# github-actions-tutorial
Github Actions教程

教程全文在: https://kalasearch.com/github-actions-simple-tutorial/

请Fork本repo，然后执行

```git clone git@github.com:YOUR_NAME/github-actions-tutorial.git
cd github-actions-tutorial

touch empty.txt # 向文件夹里添加一个空文件
git add empty.txt
git commit -m  "Add empty file"
git push origin master

```

即可在你fork出的repo中看到Actions的输出



### Workflow YAML文件
请参考.github/workflow/deploy.yml

这里的deploy.yml命名不是必须，可替换。

本repo中的deploy.yml文件内容为

```
name: Python Service

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest #构建环境，如无特殊需要可以默认为ubuntu-latest

    steps:  # 步骤
    - uses: actions/checkout@v1    # 注释1
    - name: Set up Python 3.7      # 使用python特定版本
      uses: actions/setup-python@v1
      with:
        python-version: 3.7        # 注释2
    - name: Test                   # 第二步
      run: |
        ls -all -h                 # 可以直接运行普通的linux命令
        pwd                        # 注意此行输出，actions运行的默认路径是你的repo根目录
        python -m pip install --upgrade pip       # 可直接运行普通的python命令
        python ./python-service/test.py             # 开始测试
    - name: Build
      run: |
        python ./python-service/build.py
    - name: Deploy
      run: |
        python ./python-service/deploy_service.py

```

注释1：默认必须提供的`checkout action`标准操作（见注释一）。这一步是使用一个标准的action来告诉github actions你的代码在哪里（详情见: [Checkout Action文档])。

注释2：这是为`actions/setup-python@v1`这个第三方action提供它需要的操作，即python版本



