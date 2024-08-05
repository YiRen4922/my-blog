---
title: hexo+github搭建博客
date: 2024-08-02 22:27:15
tags:
typora-root-url: hexo-github搭建博客
---

# hexo+Github Pages 和 actions搭建博客

## 前提条件

1. Node.js

   1. 官网：https://nodejs.org/zh-cn

   2. 下载安装

   3. 测试安装成功
      
      ```shell
      node -v
      ```
      
   4. 返回版本为成功

2. npm

   1. 安装node时附带安装

   2. 测试安装成功

      ```shell
      npm -v
      ```

   3. 返回版本为成功

3. git

   1. 官网：https://git-scm.com/downloads

   2. 下载安装测试安装成功

   3. 测试安装成功
      
      ```shell
      git -v
      ```
      
   4. 返回版本为成功

4. github账号

   1. 官网：https://github.com
   2. 自行注册（有时不能访问）
      1. 用steam++加速
      2. 挂梯子

## 安装hexo以及初始化

1. 安装hexo

   ```shell
   npm install -g hexo-cli
   ```

2. 验证安装

   ```shell
   hexo -v
   ```

3. 初始化
   找到合适的路径打开Windows Power Shell

   ```shell
   mkdir blog
   cd blog
   # hexo初始化
   hexo init
   # 安装hexo的依赖
   npm install
   # 生成静态文件，默认有一篇hello world的博客
   hexo generate
   # 本地启动
   hexo server
   ```

   访问本地：http://localhost:4000

   如下：

![img](QQ_1722657455049.png)

初始化成功

4. 发布到Github Pages需要下载插件

   ```shell
   npm install hexo-deployer-git --save
   ```

## 配置git

1. 配置用户名邮箱

   ```shell
   git config --global user.name "你的github用户名"
   git config --global user.email "你的gihub邮箱"
   ```

2. 查看配置是否成功

   ```shell
   git config --list
   ```

3. 配置ssh

   ```shell
   ssh-keygen -t rsa -C "你的gihub邮箱"
   ```

   三次Enter不设密码

   密钥分为公钥和私钥，位于/c/Users/Tinywan/.ssh/

   带.pub后缀的是公钥（id_rsa.pub）

4. 添加公钥到github

   ```shell
   cat /c/Users/Tinywan/.ssh/id_rsa.pub
   ```

   将展示出来的公钥内容一字不落的复制到gihub的ssh中

   ![image-20240803131346479](image-20240803131346479.png)

## github和hexo配置

1. github新建仓库

   ![image-20240803131815244](image-20240803131815244.png)

2. hexo配置

   1. 回到/blog/目录下，修改_config.yml最后几行，这里我用vim，用其他也可以（vscode，记事本）

      ```shell
      vim _config.yml
      ```

      原来![img](QQ_1722662611435.png)

      修改为（你自己的仓库地址，可从github复制）

      ![img](QQ_1722662678623.png)

      ```yaml
      deploy:
        type: git
        repo: https://github.com/YiRen4922/my-blog.git
        branch: gh-pages
      ```

   2. 还有添加资源地址（用来加载css等文件）

      ![img](QQ_1722693495529.png)

      ```yaml
      url: https://yiren4922.github.io/my-blog
      root: /my-blog/
      ```

   3. 修改生成新博客md文档时会生成同名文件夹（用来存储博客图片）
      ![img](QQ_1722693742144.png)

      ```yaml
      post_asset_folder: true
      marked:
        prependRoot: true
        postAsset: true
      ```

3. 配置github actions（用来当main分支push时编译静态文件，并推送到github pages）

   1. 本地

      在/blog/目录下

      ```shell
      cd .github
      mkdir workflows
      cd workflows
      touch deploy.yml
      ```

      新建了一个*.yml*后缀的文件，将下方代码复制进去，然后修改node版本为你对应的版本

      ```yaml
      name: Deploy Hexo to GitHub Pages
      
      on:
        push:
          branches:
            - main  # 当推送到 main 分支时触发工作流
      
      jobs:
        build:
          runs-on: ubuntu-latest  # 使用最新的 Ubuntu 操作系统运行工作流
      
          steps:
            - name: Checkout code
              uses: actions/checkout@v4  # 检出当前的代码仓库
      
            - name: Setup Node.js
              uses: actions/setup-node@v4  # 设置 Node.js 环境
              with:
                node-version: '20'  # 设置 Node.js 版本为 20
      
            - name: Cache node modules
              uses: actions/cache@v4  # 缓存 node_modules 以加快后续构建速度
              with:
                path: ~/.npm
                key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
                restore-keys: |
                  ${{ runner.os }}-node-
      
            - name: Setup Hexo
              run: npm install hexo-cli@latest -g  # 全局安装最新的 Hexo CLI
      
            - name: Install dependencies
              run: npm install  # 安装项目依赖
      
            - name: Configure Git
              run: |
                git config --global user.name "你的用户名"
                git config --global user.email "你的邮箱"
      
            - name: Generate static files
              run: npx hexo generate  # 生成静态文件
      
            - name: Deploy to GitHub Pages
              env:
                GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # 使用 GitHub 提供的 GITHUB_TOKEN 进行身份验证
              run: |
                echo "machine github.com login ${{ secrets.GH_TOKEN }}" > ~/.netrc
                echo "machine api.github.com login ${{ secrets.GH_TOKEN }}" >> ~/.netrc
                npx hexo deploy  # 部署静态文件到 GitHub Pages
      ```

   2. github端

      github actions在我理解中就是一个虚拟机，可以运行一些脚本命令，使得可以部署静态文件，既然是虚拟机那么访问github自然需要身份验证，可以添加密钥，也可以用token验证身份，这里采用token的方式。

      ![image-20240803221954778](image-20240803221954778.png)

      在这里新建一个token，token的过期时间以自己选择，复制你的token，到之前在github新建的空仓库。

      ![image-20240803222256032](image-20240803222256032.png)

      点击新建，注意名字要和github actions中的名字一样GH_TOKEN

      ![image-20240803222512304](image-20240803222512304.png)

      至此，配置完成

## 写文章并提交

用hexo new blog_name命令，本地新建博文

```shell
hexo new blog_name
```

博文的位置在\blog\source\_posts\下，同时还会生成一个同名文件夹里面用来存放图片，md文档可以用常规的图片添加语法添加，同时编译出的静态文件也可以正常加载图片。

编辑好博文后

```shell
git add *
git commit -m "first commit"
git remote add origin "你的gtihub仓库路径"
git push -u origin main
```

之后每次提交博文就是

```shell
hexo new
# 写文章
git add *
git commit -m "巴拉巴拉"
git push
```

然后github actions就会编译出静态文件并自动部署。

