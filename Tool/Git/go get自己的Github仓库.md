# go get自己的Github仓库

## 创建Github仓库

使用一般的方法创建一个仓库，这里新建一个名为`gobinscan`的仓库。仓库可以设置为公开或者私有。

![image-20230627191307518](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306271913986.png)



## 编辑项目代码

如果想要将代码作为go仓库使用，需要在mod中指明module为github的地址。

例如现在仓库目前module为`gobinscan`。

将`gobinscan`更新为`github.com/neumannlyu/gobinscan`。

1. 将`go.mod`中的`module` 更新为 `github.com/neumannlyu/gobinscan`。
2. 将其他的导入项都修改为``github.com/neumannlyu/gobinscan`。



## 上传到Github

```
go remote add github https://github.com/neumannlyu/gobinscan.git

// 可选
git remote rm origin

git remote add origin https://github.com/neumannlyu/gobinscan.git

git push -u origin main
```



## 发布版本

因为之前已经创建过0.0.1和0.0.2，新建一个Release，标签和标题都为`v0.0.3`。

**注意：**千万不要使用重复的标签值，即使将标签删除了，go get很有可能仍是老版本，即使将项目删除后再创建仍不能使用这些已经使用的标签。



## 使用模块

新建一个测试工程。

如果直接使用 go get是没有效果的。

```
neumann@neumanndeMBP TestScan % go get github.com/neumannlyu/gobinscan@v0.0.3
go: downloading github.com/neumannlyu/gobinscan v0.0.3
go: github.com/neumannlyu/gobinscan@v0.0.3: verifying module: github.com/neumannlyu/gobinscan@v0.0.3: reading https://sum.golang.org/lookup/github.com/neumannlyu/gobinscan@v0.0.3: 404 Not Found
        server response:
        not found: github.com/neumannlyu/gobinscan@v0.0.3: invalid version: git ls-remote -q origin in /tmp/gopath/pkg/mod/cache/vcs/b6a8340b79e942dae055256e79ae944fa65029e2c23b324d28e9c5b8e32db576: exit status 128:
                fatal: could not read Username for 'https://github.com': terminal prompts disabled
        Confirm the import path was entered correctly.
        If this is a private repository, see https://golang.org/doc/faq#git_https for additional information.
```

因为我们这里设置的是私有仓库，我们不能够直接获取。

我们需要先设置私有仓库地址，然后用ssh（需要先提前上传公钥到github上）连接来代替原本的https连接。

```
1.go env -w GOPRIVATE="github.com"
2.git config --global url."git@github.com:neumannlyu/gobinscan.git".insteadof "https://github.com/neumannlyu/gobinscan.git"
```

之后就可以愉快地使用`go get` 了。

