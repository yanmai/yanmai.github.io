# yanmai的个人博客

博客网址：[爱脑补](http://www.inaobu.com/)。欢迎光临！

## 添加文章的步骤

### 1、使用命令创建文章模板 `rake post title="<文章标题>"`，新文件被创建在 `_posts` 目录下。

> 也可以在 `_posts` 下直接创建文件，注意保持文件名格式为：`YYYY-MM-DD-title-name.md`, 然后 再从其他文件拷贝最上面七行代码。

### 2、检查文件名是否正确，`YYYY-MM-DD-<这里不能为空>.md`。如果为空，则需要添加单词或者拼音。如果是多个单词，请使用破折号 `-` 连接。

> 如果上面的 “文章标题” 中有英文单词，会自动提取用于创建文件名；否认需要自己添加。

### 3、打开刚刚创建的文件，在第二行填写标题和标题。如下：

```
title: "<这里就是标题>"
description: "<这里填写描述>"
category: "<这里填写类目，比如 Swift>"
```

### 4、最后，可以在这个文件的下面愉快写文章了。

## 本地运行

有时，我们需要对项目做一些小的调整，可以在本地运行该项目。

### 安装依赖

以此执行如下命令，安装所需要的依赖：

```
gem install bundler

bundle config mirror.https://rubygems.org https://gems.ruby-china.org

bundle --path=.bundle/gems
```

> 这里假设电脑上已经安装好了 Ruby 开发环境。

这些操作只需要执行一次即可。

### 启动项目

运行如下命令来启动项目：

```
bundle exec jekyll serve
```

稍等片刻，就可以访问： <http://127.0.0.1:4000/>。
