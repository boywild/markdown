[TOC]
# Nvm安装

### Mac下的安装

```js
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash
```

复制上述代码，到`Mac`的`terminal`中执行，就会安装`nvm`,安装完成后，还暂时不能用，需要复制它提示的两行代码（就是下图拿箭头标出来的两行代码）来配置环境变量：

```js
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

![](https://ws1.sinaimg.cn/large/aaf377eegy1fsrwdttr8ij20vo0fczog.jpg)

在`terminal`查找上述代码出现的地方，附近会有提示让你重新启动`terminal`，重新启动`terminal`，然后输入红色箭头部分的代码并执行


### 网上另外的一种方法
切换到`~/.nvm($HOME)`下面(快速的切换：直接输入`cd`回车)，查看是否有`.bash_profile`,如果没有的话就创建！

创建`.bash_profile`命令
>touch .bash_profile

打开
>open .bash_profile

这时候把提示的内容`copy`进去就可以了！

更新刚配置的环境变量
>source .bash_profile

完成以后 输入
>nvm


出现如下情况则表明安装成功

![](https://ws1.sinaimg.cn/large/aaf377eegy1fsrwi9ut31j21re11ing4.jpg)


### Windows 下安装

## 修改nvm镜像地址
### Mac下修改
在~/.nvm目录下打开`.bash_profile`添加下面到最后，
>export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node

保存退出，执行更新命令
>source .bash_profile

执行如下命令
>echo $NVM_NODEJS_ORG_MIRROR

能看到输出`https://npm.taobao.org/mirrors/node`即表示环境已经生效。

### Windows下修改


## 使用 .nvmrc 文件配置项目所使用的 node 版本

```js
cd <项目根目录>     #进入项目根目录
echo 4 > .nvmrc   #添加 .nvmrc 文件
nvm use           #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v           #查看 node 是否切换为对应版本
```

## Nvm常用命令

```js
nvm --help                          显示所有信息
nvm --version                       显示当前安装的nvm版本
nvm install [-s] <version>          安装指定的版本，如果不存在.nvmrc,就从指定的资源下载安装
nvm install [-s] <version>  -latest-npm 安装指定的版本，平且下载最新的npm
nvm uninstall <version>             卸载指定的版本
nvm use [--silent] <version>        使用已经安装的版本  切换版本
nvm current                         查看当前使用的node版本
nvm ls                              查看已经安装的版本
nvm ls  <version>                   查看指定版本
nvm ls-remote                       显示远程所有可以安装的nodejs版本
nvm ls-remote --lts                 查看长期支持的版本
nvm install-latest-npm              安装罪行的npm
nvm reinstall-packages <version>    重新安装指定的版本
nvm cache dir                       显示nvm的cache
nvm cache clear                     清空nvm的cache
nvm alias default <version>         指定一个默认的node版本
```