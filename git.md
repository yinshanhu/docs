### git submodule

#### 添加子模块

添加一个远程仓库项目 `https://github.com/xxxxxx/GWToolkit.git` 子模块到一个已有主仓库项目中。代码形式是 `git submodule add <url> <repo_name>`， 如下面的例子：`<repo_name>` 非必须，默认以仓库名命名。

```
git submodule add https://github.com/xxxxxx/GWToolkit.git
```

这时，会看到一个名为 `GWToolkit` 的文件夹在主仓库目录中。


*如果你是旧版 Git 的话，你会发现 `./GWToolkit` 目录中是空的，你还需要在执行一步「更新子模块」，才可以把远程仓库项目中的内容下载下来。*

```
git submodule update --init --recursive
```

#### 删除子模块

**删除子模块比较麻烦，需要手动删除相关的文件，否则在添加子模块时有可能出现错误** 同样以删除 GWToolkit 子模块仓库文件夹为例：

1. 删除子模块文件夹

```
git rm --cached GWToolkit
rm -rf GWToolkit
```

2. 删除 `.gitmodules` 文件中相关子模块的信息，类似于：

```
[submodule "GWToolkit"]
        path = GWToolkit
        url = https://github.com/xxxxxx/GWToolkit.git
```

3. 删除 `.git/config` 中相关子模块信息，类似于：

```
[submodule "GWToolkit"]
        url = https://github.com/xxxxxx/GWToolkit.git
        active = true
```

4. 删除 .git 文件夹中的相关子模块文件

```
rm -rf .git/modules/GWToolkit
```

#### 更新子模块

> 更新项目内子模块到最新版本：

```
git submodule update
```

> 更新子模块为远程项目的最新版本

```
git submodule update --remote
```


#### 拉取主项目和所有子项目

```
git clone --recursive <project url>
```