---
title: Git多仓库同步
date: '2023-06-08 10:05:38'
toc: true
indexing: false
abbrlink: 29ad
categories:
  - Tool
  - Git
---

# 场景描述

现在有个项目，该项目需要同步到GitHub上，同时也需要同步到公司本地的Gitlab上。

# 解决方案

假设我们已经创建好Github项目和对应的Gitlab项目。在 `.git` 目录下的 `config` 文件可以看到GitHub的远程地址。

![](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306071226664.png)

**新增GitLab：**

![](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306071230884.png)

复制GitLab的HTTP连接，使用下面命令来增加仓库地址。

```sh
git remote add gitlab http://172.16.5.8/neumann/gobinscan.git
```

![](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306071236187.png)

# 同步方法

- 同步到Github

  ```sh
  git push origin main
  ```

- 同步到Gitlab

  ```sh
  git push gitlab main
  ```

  