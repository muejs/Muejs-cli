# muejs-cli 命令行工具


| **描述**            | **修改日期**    |
|:-------------------|-------------------:|
|更新默认的源为 gitee 国内会快很多     |2020-04-13    |


## 一、简介

> [muecli](https://github.com/muejs/muejs-cli) 是[Mue Webapp交互框架](http://www.muejs.com) 的cli命令工具, 用于快速生成指定平台与模板最新的工程文件.

*通过命令行构建,有以下好处:*

1. 自动获取最新的mue模板工程;
2. 可以指定mue对应的平台版本;
3. 可以指定mue的单页模板.  [mue模板图片预览](https://github.com/muejs/Mue-Template/) ;
4. 拥有服务器并支持接口跨域调试;
5. 自动编译压缩混淆文件,便于打包部署的安全;
6. 支持ES6编译;
7. 支持sass编译;
8. 支持less编译;
9. 减少对工具的依赖


## 二、安装muejs命令行工具

> 需要先安装 [node环境](https://nodejs.org/zh-cn/) 才能使用npm命令. 如果下载缓慢,建议先安装[淘宝的npm镜像](https://npm.taobao.org/), 把以下命令的`npm`换成`cnpm`;

*windows:* 需要右键使用管理员打开终端或者安装 `powershell`.
```bash
npm install -g muecli
```

*mac: 打开终端, 需要权限,加上sudo,还要输入密码回车确认*
```bash
$ sudo npm install -g muecli
```

## 三、初始化工程目录:

![mue 创建工程预览](http://www.muejs.com/docs/images/router/muejs-create-demo_low.gif)


### 3.1 创建默认Webapp工程, 新版的muejs会针对不同的node版本,创建不同的工程,但是默认不会主动创建sass的编译支持.

```bash

# * 创建webapp工程 (demo 为工程名称, 如果没有,则在当前目录)
$ mue create demo

# * 进入工程目录
$ cd demo

# * 安装依赖
$ npm install

# * 自动打开浏览器并监听js,scss,html等文件的修改
$ npm run dev


# 非必须命令,编译工程,生成dist目录,压缩脚本,样式,图片,用于发布打包的安全, 会清空之前目录,重新根据`src`目录生成. 另外, es6 的模块化 import ,Objece.assign 等方法不要使用, 会导致编译后在部分手机无法运行

$ npm run build

```

#### 3.1.1 如果需要工程对sass或者less支持

在刚刚创建的demo工程里面再次执行以下命令
```bash
$ mue create -p sass
$ mue create -p less
```

**注意:** `npm run dev`使用这个命令, 样式的修改都需要在 `src/scss/style.scss` 文件. 如果直接修改`src/css/style.css`,需要删除`src/scss`目录,避免style.css被覆盖.

> 安装时, 如果报 sass 错误, 请先执行 `npm remove node-sass` 然后再安装依赖.

> 如 github 下载缓慢, 可以使用新的命令 `mue create -f gitee` 使用码云的下载源. mue 需要更新到 0.6.6 以上才有.


### 3.2 自动打开默认浏览器,  修改src 目录的相同文件,就会生成对应的dist文件,用于预览.  

`http://localhost:port`

`port`端口自动生成, 一个端口对应一个工程, 如需更改同样是在`app.json` 配置.


### 3.3 工程模板预览

**默认模板预览**  更多模板点击这里 [Mue模板图片预览](https://github.com/muejs/Mue-Template/)

<img src="https://raw.githubusercontent.com/muejs/Mue-Template/master/preview.png" alt="">


## 四、接口跨域及配置

### 接口跨域

打开根目录下的 `app.json` ,里面有个 `proxy` 的对象, key值为接口的目录名, `target` 为域名的host.

假设请求的接口地址为: http://www.muejs.com/api/getDetail/id/123

需要这样配置 proxy :

```js
{
...
"proxy": {
    "/api": {
      "target": "http://www.muejs.com",  
      "changeOrigin":true  
    }
  }
...
}
```

js: 脚本请求使用相对路径, 为了后面更改为正式地址, 建议可以把url部分作为配置项.

```js
var apiUrl = "";

mue.ajax({
    url: apiUrl+ "api/getDetail/id/123"
}).then(function(res){

})
```

### 服务器的配置说明

打开根目录下的 `app.json ,里面有个 devServer 的对象.
```js
{
  "distServer": {
    "root": "dist",                   // 编译的目录
    "livereload": true,               // 修改自动刷新
    "port": 2149                      // 端口号,默认第一次随机自动生成
  }
}

```

## 五、命令列表

注意: 中括号为可选,如果没有采用默认

### mue 命令列表

| **命令行**   | **描述**           |
|:------------- |:-------------------|
| `mue -v`       |查看当前工具的版本    |
| `mue -h`       |命令帮助信息    |
| `mue create `  |在当前目录创建webapp默认工程    |
| `mue create [projectName] [version] [-t templateName] [-p platformName] [-m moduleName] [-f from]`       |创建工程,支持指定版本,指定模板,指定平台,相同目录下会覆盖    |
| `mue update` | 在当前项目更新 mue为最新webapp版本,只修改mue.css,mue.js不覆盖项目其它内容    |
| `mue update [projectName] [version] [-p platformName] [-d dev] [-f from]` | 更新mue为某个版本,某个平台, -d 更新为最新的工程模式(dev)    |
| `mue list`       |显示可用的版本    |
| `mue list-template`       |显示可用的模板列表 [模板图片预览](https://github.com/muejs/Mue-Template/)    |
| `mue list-platform`       |显示可用的平台列表    |
| `mue clear`       |清除下载的模板缓存    |
| `mue create -m 模块名 `  新     | 创建新的模块  m = module  |
| `mue create -f 数据源 `  新     | 创建工程来自 gitee 或者 github , 默认是 github  f = from    |

### NPM 命令列表

| **命令行**   | **描述**           |
|:------------- |:-------------------|
| `npm run build` | 编译成可以打包的文件,默认服务器根路径是"dist",所以需要先编译    |
| `npm run dev` | 启动服务并打开默认浏览器,支持接口跨域, 并且会自动监听脚本,scss文件,html文件的修改编译    |
| `npm run server` | 启动默认8000端口的服务,并且默认支持了接口跨域,需要在app.json配置对应的接口地址    |


## 六、命令示例
### 创建某个模板工程 ( main-tab 为模板名称)
可以先查看有什么模板 `mue list-template`, [模板图片预览](https://github.com/muejs/Mue-Template/)

```bash
# 查看有什么模板
$ mue list-template

# 进入当前工程 demo
$ cd demo

# 替换main模板
$ mue create -t main-tab

# 新增login模板
$ mue create -t page-login

# 新增一个新模块 apps 
$ mue create -m apps

# 新增一个新子模块 apps/meeting 
$ mue create -m apps/meeting

```

**效果预览**  
<img src="https://raw.githubusercontent.com/muejs/Mue-Template/master/templates/main-tab/preview.png" alt="">
> <strong style="color:red">注意:</strong>
1. 同一个工程可以多次创建模板;
模板名以 `main-`开头 会覆盖 main 模块, 例如: 模板名 `main-tab` 预览地址 `index.html`
模板名以 `page-`开头 会新增页面, 例如: 模板名 `page-login` 预览地址 `index.html#pages/login/login`;
2. `main-`开头的模板会覆盖main页面, `page-`开头的模板是新增页面;
3. 同一个工程只能创建一个平台, 多次创建会相互覆盖;


### 创建指定平台工程 ( dcloud 为平台名称 )

可以先查看有什么平台选择 `mue list-platform`
> <strong style="color:red">注意:</strong>
1. 目前已经支持以下打包平台 cordova,bingotouch,dcloud,apicloud,appcan,微信 等;
2. 不同平台对应的文件会有些许不同, 绑定原生后退的方法也不同, 不指定平台时, 默认是webapp平台, 可以在微信及webkit浏览器内核预览.

以下内容都是以进入 demo 工程示例.

```bash

# 创建Dcloud平台的应用
$ mue create -p dcloud

```

### 创建dcloud平台下的某个模板工程

```bash

$ mue create -t sidebar -p dcloud

```

### 创建sass编译工程

```bash

$ mue create -p sass

```

### 创建less编译工程

```bash

$ mue create -p less

```

### 创建指定版本工程
> 可以先查看有什么版本 `mue list`

```bash
mue create v1.0
```


### 更新工程为最新mue版本

```bash
mue update
```


### 创建多页工程

```bash
mue update -p mpa
```


### 更新工程为最新npm开发模式 node10以上使用以下方式更新

```bash
mue update -d
```
也可以指定更新对应的node版本
```bash
mue update -d node8
mue update -d node8-sass
mue update -d node10
mue update -d node10-sass
```

### 创建新模块

模块的访问路径: 默认: `index.html#pages/article/index`

```bash
mue create -m article
```


## 七、目录说明:

**单页应用包文件夹说明:**

| **路径**   | **描述**           |
|:------------- |:-------------------|
| gulpfile.js     |入口文件    |
| package.json    |npm依赖配置文件    |
| app.json    |入口文件    |
| dist/     | 编译打包最终要部署的目录    |
| src/index.html     |入口文件    |
| src/index.js       |入口的脚本    |
| src/css/mue.css  |mue库的样式文件    |
| src/css/style.css  | 当前应用的样式文件    |
| src/font/         |字体图标目录    |
| src/images/       |应用图片目录    |
| src/scss/       | 样式源文件,样式最好放这里可以分模块管理    |
| src/js/zepto.js  | mue的依赖库  |
| src/js/plugins/fastclick.js  |  移动端快速点击的插件   |
| src/js/mue.js       |  mue交互控件库   |
| src/pages/      | 模块目录    |
| src/pages/main       | 默认 main 模块    |
| src/pages/main/main.html      | 默认 main 模块模板    |
| src/pages/main/main.js      | 默认 main 模块定义脚本    |

## 八、多个mue工程共享`node_modules`模块目录

> 现在每次创建一个工程以后, 每次都需要执行 `npm install` 特别的繁琐, 通过以下步骤, 可以创建一个mue的相关工程共享 `node_modules`

1. 升级mue 2.0
2. 创建mue工程目录, 作为所有工程目录 `mue create mue-projects`, 删除 <del>src目录,app.json</del> ,只保留 `package.json, gulpfile.js `
3. `cd mue-projects` 进入目录
4. `npm install` 安装依赖模块
5. `mue create project1` 创建带工程名的工程
6. `npm run dev-project1` 运行服务预览 或者 `npm run build-project1` 编译打包

```bash
# 创建 mue-projects 文件夹作为公共的mue应用目录
mue create mue-projects

# 进入这个目录
cd mue-projects/

# 安装依赖
npm install

# 创建 project1 工程
mue create project1

# 运行预览
npm run dev-project1

# 编译打包
npm run build-project1
```
> project1/gulpfile.js, project1/package.json 这两个文件则不需要了

## 九、更改数据源

> 国内部分地区创建工程时,个别反应,创建的过程过于缓慢. 现在我们新增一个命令用于指定数据源, 默认是在 `github`, 你可以通过 `-f` 命令,更改到 `gitee`

```bash
# 从 gitee 的最新版本创建默认工程. 
mue create -f gitee
mue create -f github

# 创建一次以后,在没有新版本的时候, 创建其它模板, 无需再加入这个 `-f` from命令. 
mue create -t main-tab
```

## 十、创建完整案例参考

```bash

# 创建163案例
mue create -t case-163
```


## 十一、创建皮肤

1. 创建主色皮肤

> `npm run dev` 主色皮肤运行以后就会覆盖掉 原本的色系
```bash

# 先让你的文件支持sass
mue create -p sass

# 创建一个红色皮肤, 在__variables.scss 文件里面通过 $main-color 修改为其它主色调, 会把头部等样式替换
mue create -p skin

```

2. 创建深夜模式皮肤

> 深色皮肤跟主色皮肤的区别在于很多配色是相反的, 一般不需要修改, 运行以后,会在 css/mue-skindeep.css 生成样式文件,需要在 index.html 主动引入或者通过脚本控制切换.

```bash

# 先让你的文件支持sass
mue create -p sass

# 创建一个深色皮肤
mue create -p skindeep
```



## 十二、疑难问题

1. 保存代码不会自动更新？

> 答：工程有使用 `npm run dev` 以后,会默认支持热更新, 工程只能在英文名路径下. 

2. 找不到全局变量? 

> 答: 在node10以上创建的新工程, 默认对es6编译更加友好, 但当执行了 `npm run build`命令以后, 这个编译把脚本变成了闭包, 工程原本的全局变量, 变成了局部变量, 公共脚本用`var`声明的全局变量都无法找到. 有4种解决办法, 
  1. 最简单的方法是使用 window.xxx 来替代公共方法的 var 声明. 
  2. 一开始就使用 loader.define 定义这个模块, 然后在每个模块需要用的时候引入, 这样就不会有这个问题了. 
  3. 把文件命名为 `xxx.min.js` 就会跳过编译. 但这样就要求里面不能有es6的写法, 因为打包以后对es6支持不好. 
  4. 1.6.2 新增了一个新的定义方式, 比较推荐.
  > 4.1. common.js 使用这个方法构建全局方法
  
    ```js
    loader.global(function(global){
        // global: 为上次执行的依赖
        return {
            test: function(){
                console.log("test");
            },
            test2: function(){
                console.log("test2");
            }
        }
    })
    ```

  >  4.2. 推荐: 局部调用
    
    ```js
    loader.define(function(require,export,module,global){
        // 调用test方法
        global.test();

    })
    ```

  > 4.3 全局调用: 
    `loader.globals.test();`
