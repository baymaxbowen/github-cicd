# Github Action

## 什么是Github Action
[官方文档](https://docs.github.com/cn/actions)<br/>
Github Action 是 Github 推出的一个 `CI\CD` 服务<br/>
## 什么是 `CI\CD`
`CI\CD` 是指 **持续集成、持续交付、持续部署**<br/>
什么是**持续**,持续是指多次、频繁的进行
- **CI(持续集成)**, 是指开发在提交完代码到仓库后,自动进行代码构建,然后进行单元测试.<br/>
- **CD(持续交付)**,是指将构建好的产物部署到测试环境中,然后由测试人员进行测试,测试通过后再发布到真实环境.
- **CD(持续部署)**,是指持续集成后不需要测试人员进行干预直接部署到真实环境.
 > 持续集成的核心是**代码合并**执行的是**单元测试**,重点是**快速反馈**<br/>
 持续交付的核心是**可用的软件**执行的是**功能,系统测试等**,重点是软件质量达到交付标准<br/>
 持续部署太过理想化了,CI之后肯定会有人员干预

## Github Action 基本概念
 1) `workflow`(工作流程): 持续集成一次的过程,就是一个`workflow`.
 2) `job`(任务): 一个`workflow`由一个或多个`job`构成,是指一次集成可用完成多个任务.
 3) `step`(步骤):每个`job`由多个step构成,一步一步完成.
 4) `action`(动作):每个`step`可以依次执行一个或者多个`action`

## 发布一个`react` 项目到 Github Page
1) 创建一个Github密钥,因为部署项目到Github Page需要写权限
2) 创建一个react项目,在package.json文件中,加一个homepage字段<br/>```"homepage": "https://[username].github.io/demo"```
3) 在`.github/workflows` 的目录中创建一个workflow文件,比如ci.yml<br/>
```yml
name: GitHub Actions Build and Deploy Demo #workflow 命名
on:
  push:
    branches:
      - main  #在push到main分支的时候触发 jobs
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest #通过 Github 上的docker容器运行
    steps:
      - name: Checkout # 获取源代码
        uses: actions/checkout@master # 使用了别人的提供的action
        with: # 步骤的参数,设置版本、环境变量等
          persist-credentials: false 
      - name: Install and Build #构建和部署
        run: | 
          npm install
          npm run-script build
      - name: Deploy
        uses: JamesIves/github-pages-deploy-action@releases/v3
        with: #配置环境变量
          ACCESS_TOKEN: ${{ secrets.TOKEN }} # 申请的密钥
          BRANCH: gh-pages # 发布的分支
          FOLDER: dist #构建出来的目录
          BUILD_SCRIPT: npm install && npm run build #构建的脚本
```
4) 将代码推送到`main`分支,Github Action会自动运行,同时将构建产物发布至Github Page.