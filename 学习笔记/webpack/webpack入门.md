#### 什么是webpack
> WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。
#### 为什要使用WebPack

现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法
- 模块化，让我们可以把复杂的程序细化为小的文件;
- 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能转换为JavaScript文件使浏览器可以识别；
- Scss，less等CSS预处理器
- ...

这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别,而手动处理又是非常繁琐的，这就为WebPack类的工具的出现提供了需求。

#### Webpack的工作方式
> 把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

![image](https://segmentfault.com/img/remote/1460000007045085)

<br>
---

#### webpack的安装

```
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
```
#### 正式使用Webpack前的准备
1. 在项目文件夹中创建一个**package.json**文件，这是一个标准的npm说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等等。在终端中使用npm init命令可以自动创建这个package.json文件

```
npm init
```
输入这个命令后，终端会问你一系列诸如项目名称，项目描述，作者等信息，不过不用担心，如果你不准备在npm中发布你的模块，这些问题的答案都不重要，回车默认即可。

2. package.json文件已经就绪后，我们在本项目中安装Webpack作为依赖包

构建项目代码,目录如下
![image](https://segmentfault.com/img/remote/1460000007045086)

在命令行中进行webpack打包

```
# webpack非全局安装的情况
node_modules/.bin/webpack app/main.js -o public/bundle.js
# 全局安装情况
webpack app/main -o public/bundle.js
```
> 这里使用原教程中的语句是无法运行的,因为webpack更高的版本上更换了命令 更换的命令需要在入口文件(entry)后面加入 **-o**

```
# {extry file}出填写入口文件的路径，本文中就是上述main.js的路径，
# {destination for bundled file}处填写打包文件的存放路径
# 填写路径的时候不用添加{}
webpack {entry file} -o {destination for bundled file}
```

#### 使用配置文件进行打包
> 定义一个配置文件，这个配置文件其实也是一个简单的JavaScript模块，我们可以把所有的与打包相关的信息放在里面。

在当前项目文件夹的根目录下新建一个名为**webpack.config.js**的文件，在其中写入如下所示的简单配置代码，++目前的配置主要涉及到的内容是入口文件路径和打包后文件的存放路径。++

```
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
```
> **注：**“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。

有了这个配置之后，再打包文件，只需在终端里运行webpack(非全局安装需使用node_modules/.bin/webpack)命令就可以了，这条命令会自动引用webpack.config.js文件中的配置选项，示例如下：
![微信截图_20181208154915](79EB33797A9E4AE4A9F542BD366376A6)

#### 更快捷的执行打包任务
在命令行中输入命令需要代码类似于node_modules/.bin/webpack这样的路径其实是比较烦人的，不过值得庆幸的是npm可以引导任务执行，对npm进行配置后可以在命令行中使用简单的npm start命令来替代上面略微繁琐的命令。在package.json中对scripts对象进行相关设置即可，设置方法如下。

```
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack" // 修改的是这里，JSON文件不支持注释，引用时请清除
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "3.10.0"
  }
}
```
> **注**：package.json中的script会安装一定顺序寻找命令对应位置，本地的node_modules/.bin路径就在这个寻找清单中，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

npm的start命令是一个**特殊的**脚本名称，其特殊性表现在，在命令行中使用npm start就可以执行其对于的命令，**如果对应的此脚本名称不是start，想要在命令行中运行时，需要这样用npm run {script name}如npm run build**