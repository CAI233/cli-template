本篇文章都是干货～ 带你step by step手把手教你搭建一个CLI

//学习 https://blog.csdn.net/Wjangia/article/details/130053704

//学习 https://zhuanlan.zhihu.com/p/630328885?utm_id=0

//学习 https://juejin.cn/post/7208847676404990009

//学习  https://juejin.cn/post/6914556810129539085

//学习 https://blog.csdn.net/AS_TS/article/details/126579860?spm=1001.2101.3001.6650.9&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-126579860-blog-106123839.235%5Ev38%5Epc_relevant_anti_vip&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-126579860-blog-106123839.235%5Ev38%5Epc_relevant_anti_vip&utm_relevant_index=10

//学习 https://blog.csdn.net/qq_43402051/article/details/128180243?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-5-128180243-blog-126579860.235^v38^pc_relevant_anti_vip&spm=1001.2101.3001.4242.4&utm_relevant_index=8

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f83f96d306c343d99801fb7ddbbb3900~tplv-k3u1fbpfcp-watermark.image?)

//学习 https://juejin.cn/post/6898575518804017160?from=search-suggest
//学习 https://juejin.cn/post/6844903565287243784?from=search-suggest
### 一、背景：
过去的半年里，因为工作业务的需要，自己陆续写了两个脚手架，且都在前端组内业务中应用起来了，因此也受到了主管肯定，顺便申请了专利，其中一个已经申请专利成功，另外一个申请中（等申请成功再写篇文章分享）...

脚手架通常作为**中高级前端必备的技能**。文章较长，建议收藏～

本文**纯干货**，通过本文，你可以get怎么开发一个**高大上**的脚手架
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bf429dae8bdf4c739134566d611023b8~tplv-k3u1fbpfcp-watermark.image?)

### 二、什么是脚手架，能解决什么问题：
脚手架也常被称为`CLI`，全称`command-line interface`，翻译为命令行界面。前端脚手架CLI，也就是一个命令行工具

再日常开发中，我们经常能碰见这三类脚手架
- **常见开源脚手架**
    
    如`vue-cli`、`create-react-app`，大家再为熟悉不过了，这两个脚手架都提供了**搭建-运行-构建**项目的能力，属于“大型脚手架”；我们每天都在用的`npm`也是个脚手架

- **企业内脚手架**
    
    一般企业中，需要订制一套符合组内业务的脚手架，且跟公司的一些基础服务打通，上下游的衔接，内置的工具集合等，一般基于**常见开源脚手架**二次封装

- **其他脚手架**
    
    还有一大类，针对某一业务痛点，手动解决较复杂、成本高、效率低下，设计为自动的脚手架实现

通过以上举例，我们不难发现脚手架可以解决以下问题：
-  减少重复性工作
-  团队统一开发风格，规范项目开发目录结构，便于跨团队合作以及后期维护，降低新人上手成本
-  提供一键前端项目的创建、配置、本地开发、插件扩展等功能，让开发着更多时间专注于业务

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bcf0608654524ec6b1ed9a4f5455164f~tplv-k3u1fbpfcp-watermark.image?)

### 三、如何搭建一个脚手架

#### 3.1 设计脚手架功能
首先在开始之前，需要先设计脚手架，就拿申请专利成功的 [npm-package-privatify](https://www.npmjs.com/package/npm-package-privatify)举例，（这个脚手架具体解决了啥问题不需要关注，只是方便举例～）

- 第一步，给自己的脚手架取一个名字，比如我这里的`privatify`

- 第二步，这个脚手架提供的命令列表，比如跟`vue create  [options] <app-name>`一样提供一个初始化项目的命令，比如跟`vue add [options] <plugin> [pluginOptions]`提供一个添加插件一样的操作命令...

    当然还有基本的`--version`查看版本,`--help`提供命令列表，如下是privatify设计的命令列表
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3120bb5a6a7e4d279acbee1318f5240c~tplv-k3u1fbpfcp-watermark.image?)

#### 3.2 依赖的基础包
都说站在巨人的肩膀上，一个好的包能让你事半功倍，搭建一个基础的脚手架需要依赖以下包
| 包名 | 用途 |
| --- | --- |
| commander |  命令行工具，读取命令行命令，知道用户想要做什么 |
| inquirer |  交互式命令工具，给用户提供一个提问流方式 |
| chalk | 颜色插件，用来修改命令行输出样式，通过颜色区分info、error日志，清晰直观
| ora | 用于显示加载中的效果，类似于前端页面的loading效果，想下载模版这种耗时的操作，有了loading效果，可以提示用户正在进行中，请耐心等待
| globby | 用于检索文件 |
| fs-extra | node fs文件系统模块的增强版 |
| pacote | 获取node包最新版本等信息 |
| handlebars | 提供了必要的功能，使你可以高效地构建语义化模板 |


#### 3.3 脚手架代码实现
我是用`TypeScript+Node`搭建，仓库地址：https://github.com/zxyue25/cli-template.git 建议clone下来
##### （1）目录结构
```md
cli-template
├─ .gitignore
├─ README.md
├─ build // 打包后文件夹
├─ project-template // 初始化项目模版
├─ bin.js // 生产环境执行文件入口，具体见下
├─ bin-local.js // 本底调试执行文件入口，具体见下
├─ package.json // 配置文件，具体见下
├─ src
│  ├─ commands // 命令文件夹
│  │  ├─ create.ts // create命令
│  │  ├─ scope.ts // scope命令
│  │  ├─ package.ts // package命令
│  │  └─ utils // 公共函数
│  ├─ index.ts // 入口文件
│  └─ lib // 公共第三方包
│     ├─ consts.ts // 常量
│     ├─ index.ts
│     ├─ logger.ts // 控制台颜色输出
│     └─ spinner.ts // 控制台loading
├─ tsconfig.json // TypeScript配置文件
└─ tslint.json // tslint配置文件
```
##### （2) package.json
- `1、npm init` 初始化package.json
- `2、npm i typescript ts-node tslint rimraf -D` 安装开发依赖
- `3、npm i typescript chalk commander execa fs-extra globby handlebars inquirer ora pacote` 安装生产依赖
- `scripts配置clear、build、publish、lint命令`，具体使用最后发包会讲到

完成的`package.json`配置文件如下

```json
{
  "name": "cli-template",
  "version": "0.0.2",
  "description": "cli模版仓库",
  "main": "./build",
  "scripts": {
    "clear": "rimraf build",
    "build": "npm run clear && tsc",
    "publish": "npm run build && npm publish",
    "lint": "tslint ./src/**/*.ts --fix"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/zxyue25/cli-template.git"
  },
  "bin": {
    "privatify": "./bin.js",
    "privatify-local": "./bin-local.js"
  },
  "files": [
    "build",
    "bin.js"
  ],
  "keywords": [
    "cli",
    "node",
    "typescript",
    "command"
  ],
  "author": "zxyue25",
  "license": "ISC",
  "devDependencies": {
    "rimraf": "^3.0.2",
    "ts-node": "^10.3.0",
    "tslint": "^6.1.3",
    "typescript": "^4.4.4"
  },
  "dependencies": {
    "chalk": "^4.1.2",
    "commander": "^8.2.0",
    "execa": "^5.1.1",
    "fs-extra": "^10.0.0",
    "globby": "^11.0.4",
    "inquirer": "^8.2.0",
    "leven": "^4.0.0",
    "ora": "^5.4.1",
    "pacote": "^12.0.2"
  }
}
```
重点需要关注`bin字段`和`files字段`
- `bin字段`见下面（2）
- `files字段`即npm的白名单，如下图，npm官方解释，也就是说发包后需要包括哪些文件，不配置的话默认发布全部文件，这自然是不好的，你想别人看到你的源码嘛？所以这里我们配置了`"files": [
    "build",
    "bin.js"
  ]`，包括build跟bin.js，src文件夹不在白名单内
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/97feac18e8a24463b56b32c69d6674af~tplv-k3u1fbpfcp-watermark.image?)
##### （3) tsconfig.json配置文件
如果一个目录下存在一个`tsconfig.json`文件，那么它意味着这个目录是TypeScript项目的根目录。 `tsconfig.json`文件中指定了用来编译这个项目的根文件和编译选项，按照如下配置即可
> 不熟悉typescript的可以先跳过，理解为我们利用typescript编写，需要配置一个配置文件即可
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "es2017",
    "module": "commonjs",
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "alwaysStrict": true,
    "sourceMap": false,
    "noEmit": false,
    "noEmitHelpers": false,
    "importHelpers": false,
    "strictNullChecks": false,
    "allowUnreachableCode": true,
    "lib": ["es6"],
    "typeRoots": ["./node_modules/@types"],
    "outDir": "./build", // 重定向输出目录
    "rootDir": "./src" // 仅用来控制输出的目录结构
  },
  "exclude": [ // 不参与打包的目录
    "node_modules",
    "build",
    "**/*.test.ts",
    "temp",
    "scripts",
    "./src/__tests__"
  ],
  "esModuleInterop": true,
  "allowSyntheticDefaultImports": true,
  "compileOnSave": false,
  "buildOnSave": false
}
```
##### （4）bin.js和npm link本地调试
```json
  "bin": {
    "privatify": "./bin.js",
    "privatify-local": "./bin-local.js"
  }
```
该字段是**定义命令名(也就是你脚手架的名字)和关联的执行文件**，行首加入一行 `#!/usr/bin/env node` 指定当前脚本由node.js进行解析

生产环境
```js
// bin.js
#!/usr/bin/env node   
require('./build')
```
本地调试
```js
// bin-local.js
#!/usr/bin/env node   
require('ts-node/register')
require('./src')
```
看到这里，大家可能有些疑问，为什么要定两个呢，这是因为便于咱们本地调试脚手架，生成环境下，直接指向`build`文件夹，但本地调试的时候，你肯定不想改一句代码，编译打包到build，再调试下吧，`ts-node/register`提供了即时编译的能力，指向`src`即可


执行`npm link`，它默认会去读取`package.json的bin`字段，执行`privatify-local`可以看到正确的执行了`src/index.ts`中的内容
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ebd46f750c7485ea19f4a58e4922a4e~tplv-k3u1fbpfcp-watermark.image?)

##### （5）核心代码实现
`src/index.ts`为入口文件，`src/commands/*.*s`下放具体命令文件，这里我们根据`3.1`中设计的脚手架的命令，创建`create.ts`、`scope.ts`、`package.ts`文件
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad1f69b10dfe40eba5dcd0ec65c2c134~tplv-k3u1fbpfcp-watermark.image?)

回顾如下
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3120bb5a6a7e4d279acbee1318f5240c~tplv-k3u1fbpfcp-watermark.image?)

可以看到，每个命令都包括`command(命令)`、`description(描述)`、还有一个`执行函数(action)`，部分还支持`option(参数)`

在`src/commands/*.*s`下都写成如下形式，如`create.ts`
```ts
// src/commands/create.ts
const action = (projectName) => { 
     console.log('projectName:', projectName)
}
export default {
    command: 'create <registry-name>',
    description: '创建一个npm私服仓库',
    optionList: [['--context <context>', '上下文路径']],
    action,
}
```
`src/index.ts`代码实现如下
```js
// src/index.ts
import * as globby from 'globby'
import * as commander from 'commander'
import * as path from 'path'
import { error, chalk, fs, info } from './lib'
import * as pacote from 'pacote'
const { program } = commander

let commandsPath = []
let pkgVersion = ''
let pkgName = ''

// 获取src/command路径下的命令
const getCommand = () => {
  commandsPath =
    (globby as any).sync('./commands/*.*s', { cwd: __dirname, deep: 1 }) || []
  return commandsPath
}

// 获取当前包的信息
const getPkgInfo = () => {
  const jsonPath = path.join(__dirname, '../package.json')
  const jsonContent = fs.readFileSync(jsonPath, 'utf-8')
  const jsonResult = JSON.parse(jsonContent)
  pkgVersion = jsonResult.version
  pkgName =  jsonResult.name
}

// 获取最新包最新版本
const getLatestVersion = async () => {
    const manifest = await pacote.manifest(`${pkgName}@latest`)
    return manifest.version
}

function start() {
  getPkgInfo()
  const commandsPath = getCommand()
  program.version(pkgVersion)
  commandsPath.forEach((commandPath) => {
    const commandObj = require(`./${commandPath}`)
    const { command, description, optionList, action } = commandObj.default
    const curp = program
      .command(command)
      .description(description)
      .action(action)

    optionList &&
      optionList.map((option: [string]) => {
        curp.option(...option)
      })
  })

  program.on('command:*', async ([cmd]) => {
    program.outputHelp()
    error(`未知命令 command ${chalk.yellow(cmd)}.`)
    const latestVersion = await getLatestVersion() 
    if(latestVersion !== pkgVersion){
      info(`可更新版本，${chalk.green(pkgVersion)} -> ${chalk.green(latestVersion)}`)
    }
    process.exitCode = 1
  })

  program.parseAsync(process.argv)
}

start()
```
在`src/index.ts`入口文件利用`globby`查找`src/commands/*.*s`下的命令，也就是下面的`getCommand函数`，将读到的**command(命令)**、**description(描述)**、**执行函数(action)**，**option(参数)** 利用`commander包`，放入`commander.program.command(command)
      .description(description)
      .action(action)
      .option(option)`即可
      
> commander官网，命令行工具，提供了非常强大的功能 https://github.com/tj/commander.js/blob/master/Readme_zh-CN.md

执行，`privatify-local create name1`可以看到
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/524d4cb0944744c8ad2bbdd370da1641~tplv-k3u1fbpfcp-watermark.image?)

`program.version(pkgVersion)`是命令`--version`，我们通过下面的`getPkgInfo函数`读取**package.json**文件的**version**字段来获取版本
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0bb5b37821794523aeea723e24185faf~tplv-k3u1fbpfcp-watermark.image?)

另外当你输入的命令不存在时，应该提示用户，`commander包`提供了`program.outputHelp()`实现

且当你升级了脚手架之后，自然希望用户更新本地的脚手架到最新版，利用`pacote包`可获取指定包版本，并提示更新信息
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d151ce630fc45648d54279c7a40a1a9~tplv-k3u1fbpfcp-watermark.image?)
##### （6）公共第三方依赖封装
脚手架属于交互式命令行，涉及到界面的友好提示，成功或失败的颜色、文案等，loading，图标等将此统一封装在路径`src/lib`下，具体不展开，比较基本的封装

##### （7）初始化项目模版实现
逻辑大致与`create [options] <app-name>`一样，通过交互式初始化一个项目，利用`inquirer包`进行交互，询问用户`package.json`的**name**、**description**、**author**字段，拿到字段后，开始下载项目模版，**项目模版存在的位置有两种实现方式**，如下
- 第一种是和脚手架打包在一起，在安装脚手架的时候就会将项目模板存放在全局目录下了，这种方式每次创建项目的时候都是从本地拷贝的速度很快，但是项目模板自身**升级比较困难**。
- 第二种是将项目模板存在远端仓库（比如 gitlab 仓库），这种方式每次创建项目的时候都是通过某个地址**动态下载**的，项目模板更新方便
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3db448fe7eb144d1acb0154f1f6562a5~tplv-k3u1fbpfcp-watermark.image?)
我们这里先用**第一种**，项目模版放在`project-template`路径下，第二种下面讲
```json
// project-template/package.json
{
  "name": "{{ name }}",
  "version": "1.0.0",
  "description": "{{ description }}",
  "main": "index.js",
  "author": "{{ author }}",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "license": "ISC",
  "dependencies": {
    "npm-package-privatify": "^1.1.4"
  }
}
```

再利用`handlebars包`进行模版替换，也就是把上面的`{{ name }}`、`{{ description }}`、`{{ author }}`换成用户输入的


```js
// // src/commands/create.ts
import * as path from 'path'
import * as handlebars from 'handlebars'
import * as inquirer from 'inquirer'
import {
  chalk,
  execa,
  fs,
  startSpinner,
  succeedSpiner,
  warn,
  info,
} from '../lib'

// 检查是否已经存在相同名字工程
export const checkProjectExist = async (targetDir) => {
  if (fs.existsSync(targetDir)) {
    const answer = await inquirer.prompt({
      type: 'list',
      name: 'checkExist',
      message: `\n仓库路径${targetDir}已存在，请选择`,
      choices: ['覆盖', '取消'],
    })
    if (answer.checkExist === '覆盖') {
      warn(`删除${targetDir}...`)
      fs.removeSync(targetDir)
    } else {
      return true
    }
  }
  return false
}

export const getQuestions = async (projectName) => {
  return await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: `package name: (${projectName})`,
      default: projectName,
    },
    {
      type: 'input',
      name: 'description',
      message: 'description',
    },
    {
      type: 'input',
      name: 'author',
      message: 'author',
    },
  ])
}

export const cloneProject = async (targetDir, projectName, projectInfo) => {
  startSpinner(`开始创建私服仓库 ${chalk.cyan(targetDir)}`)
  // 复制'private-server-boilerplate'到目标路径下创建工程
  await fs.copy(
    path.join(__dirname, '..', '..', 'private-server-boilerplate'),
    targetDir
  )

  // handlebars模版引擎解析用户输入的信息存在package.json
  const jsonPath = `${targetDir}/package.json`
  const jsonContent = fs.readFileSync(jsonPath, 'utf-8')
  const jsonResult = handlebars.compile(jsonContent)(projectInfo)
  fs.writeFileSync(jsonPath, jsonResult)

  // 新建工程装包
  execa.commandSync('npm install', {
    stdio: 'inherit',
    cwd: targetDir,
  })

  succeedSpiner(
    `私服仓库创建完成 ${chalk.yellow(projectName)}\n👉 输入以下命令开启私服:`
  )

  info(`$ cd ${projectName}\n$ sh start.sh\n`)
}

const action = async (projectName: string, cmdArgs?: any) => {
    console.log('projectName:', projectName)
}

export default {
  command: 'create <registry-name>',
  description: '创建一个npm私服仓库',
  optionList: [['--context <context>', '上下文路径']],
  action,
}
```
当创建的文件存在时，提醒用户，并提供**覆盖**跟取消**选项**，执行效果如图
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b1f97143b7f04c94bd07091ccb0faa4c~tplv-k3u1fbpfcp-watermark.image?)

可以看到创建了项目`name`，且`package.json`是用户输入的值
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c45a2e3312bf45b6a59f485b931daae2~tplv-k3u1fbpfcp-watermark.image?)

第二种也比较简单，利用`download-git-repo包`，下载远程仓库地址即可，前提是你需要新建一个模版仓库，例如如下
> 需要注意的是，远程仓库地址，不是git clone的地址，而是需要稍微调整下
比如git仓库地址是https://github.com/zxyue25/vue-demo-cli-templateA.git -> https://github.com:zxyue25/vue-demo-cli-templateA#master
```js
import * as download from 'download-git-repo'
// https://github.com/zxyue25/vue-demo-cli-templateA.git
download("https://github.com:zxyue25/vue-demo-cli-templateA#master", projectName, {clone: true}, err => {
  if(err){
    spinner.fail()
    return 
  }
  spinner.succeed()
  inquirer.prompt([
    {
        type: 'input',
        name: 'name',
        message: '请输入项目名称'
    },
    {
        type: 'input',
        name: 'description',
        message: '请输入项目简介'
    },
    {
        type: 'input',
        name: 'author',
        message: '请输入作者姓名'
    },
]).then((answers) => {
    const packagePath = `${projectName}/package.json`
    const packageContent = fs.readFileSync(packagePath,'utf-8')
    //使用handlebars解析模板引擎
    const packageResult = handlebars.compile(packageContent)(answers)
    //将解析后的结果重写到package.json文件中
    fs.writeFileSync(packagePath,packageResult)
    console.log(logSymbols.success, chalk.yellow('初始化模板成功'))
  })
})
```

##### （8）发包
到此，脚手架基本的搭建与开发就完成了，发布到npm
- `1、npm run lint` 校验代码，毕竟都发包了，避免出现问题
- `2、npm run build` typescript打包
- `3、npm publish` 发布到npm
发包完成后，安装检查
```shell
npm i npm-package-privatify -g
privatify
```
### 四、写在最后
脚手架的重点是帮助降本增效，比脚手架基本开发技能更重要的是脚手架的设计思维，而这些思维一般都是需要我们对日常开发中遇到的痛难点的思考而形成的，本项目完整代码请戳:[zxyue25/github](https://github.com/zxyue25/cli-template.git)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ed84ca7466f545a588946deaf4f85d88~tplv-k3u1fbpfcp-watermark.image?)

> 编写本文，虽然花费了一定时间，但是在这个过程中，我也学习到了很多知识，谢谢各位小伙伴愿意花费宝贵的时间阅读本文，如果本文给了您一点帮助或者是启发，请不要吝啬你的赞和Star，您的肯定是我前进的最大动力
