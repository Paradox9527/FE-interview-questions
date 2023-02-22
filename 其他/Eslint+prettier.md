### Eslint+prettier

#### 概念

Eslint是一个开源的JavaScript的linting工具，将JavaScript代码解析成AST（抽象语法树），然后通过AST来分析我们代码，从而给我们提示；提示**代码质量问题**和**代码风格问题**

#### ESlint基础使用步骤

##### 1.使用node初始化创建项目，生成package.json文件；在某个文件夹下，使用终端，输入指令

```javascript
npm init // 使用node初始化创建项目，版本最好>=14
```

##### 2.添加项目目录

可以创建一个项目的名字命名的文件夹，这里命名为src；

##### 3.安装Eslint

```javascript
npm install --save-install eslint // 终端使用该指令安装eslint依赖
```

安装完成后可以在package.json中配置调试脚本；

- scripts属性是脚本，使用方法是在 cmd 窗口中输入 npm run 指令 的形式，如：npm run lint:create；运行该指令，完成eslint创建
- "lint:create": "eslint --init" 这个脚本是为了生成 .eslintrc.js 文件，后续的一系列规则需要在这个文件中配置，所以我们需要初始化创建它；
- "lint": "eslint src/**" 使用该指令可以检验src路径下所有js代码；（后续添加）

##### 4.使用npm运行脚本；

```javascript
npm run lint:create // 需要提前在.eslintrc.js文件中配置好
```

##### 5.用eslint来检测一下我们的js文件并且修复

```javascript
npm run lint // 需要提前在.eslintrc.js文件中配置好
```

eslint的规则以及相关配置参数详细可以见http://eslint.cn/docs/rules/

例子：当报错一个变量未定义并且不想要代码之间存在空行

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1669802622814-71e80ed3-15b5-49a1-8bf1-d86e16cf47e3.png)

可以通过在源文件中添加注释或者在配置文件中添加globals对象，避免这个报错，因此我们的项目中存在大量的全局变量，要写很多才能防止这类报错

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1669802622865-66968d7a-18b3-496c-8e23-28ed00bb489f.png)

```javascript
{
    globals: {
        "var1": "writable", // 允许重写变量
        "var2": "readonly" // 不允许重写变量
         View: true, // 例子：将View作为全局变量
    },
    "no-multiple-empty-lines": [ // 不运行空行
			2,
			{
			  max: 0,
			},
		],
}
```

命令修复js  修复某个目录下的全部文件，需要配置.eslintrc.js中的rules

```javascript
// package.json文件中
scripts属性增加一个脚本
"scripts" : {
  "fix":"eslint --fix src/**"
}
// .eslintrc.js
"rules": {
  "quotes": 2, // 双引号
  "semi": 1, // 末尾有分号
  "no-console": 1, // 不能有输出打印语句
}
```

运行后只能修复一些代码风格上的问题，语法问题需要自我检查修复（代码风格格式化采用prettier）

##### 6.用ESlint来检测html文件

①安装插件

```javascript
npm install --save-dev eslint-plugin-html
```

②配置.eslintrc.js

```javascript
{
    "plugins": [
        "html"
    ]
}
```

##### 7.其他(ESlint补充项)

###### Ⅰ.忽略代码检查

在html页面，暂时不需要检查script标签，可以使用<!-- eslint-disable -->注释

单行不需要检查，可以添加// eslint-disable-line的注释  **使用时切记不要写中文的注释了**

```javascript
<!-- eslint-disable --> // 下面的script标签不启用eslint
<script>
  var foo = 1
</script>
<!-- eslint-enable --> // 下面的script标签启用eslint
const apple = "apple";  // eslint-disable-line 单行不检查语法
```

###### Ⅱ.vscode插件ESlint

搜索ESlint，安装插件，针对vscode，打开settings.json 添加下面语句

```javascript
"editor.formatOnSave": true, // 保存时格式化
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
},
"eslint.alwaysShowStatus": true, // 总是显示eslint状态
```

可以在命令行上实时打印语法问题

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1670236098964-23b7ba6a-7f71-4535-b7dd-caa3acc75246.png)

ESlint语法相关的规则很多，需要针对我们项目一点点完善规则，例如我们项目中存在大量的相同的全局变量，可以在使用的过程中一点点加入到globals中；方法就是看ESlint的报错，根据报错的规则代码，配置对应的参数；比如像上图中的no-unused-vars意思就是出现了未使用的变量，这时候可以添加以下规则解决这个报错

```javascript
"no-unused-vars": [2, {
			// 允许声明未使用变量
			"vars": "local",
			// 参数不检查
			"args": "none"
}],
// 全局变量
globals: {
    View: true,
		initView: true,
		initUtil: true,
		setView: true,
		PageUtil: true,
		AreaView:true,
		RecycleViewM: true,
		UserModel: true,
}
```

###### Ⅲ.忽略检查文件

创建.eslintignore文件，添加文件，使其被eslint插件忽略检查

```javascript
# test 注释的使用 以#开头
/src/js/*.js  某个目录下全部的js文件
.eslintrc.js  指定文件名
.prettierrc.json
/src/css/
/src/images/
```

###### Ⅳ.注释的使用（需要自我优化）

注释是让团队之间的小伙伴了解代码写法和目的唯一途径，对于一些看似无关紧要的代码时，由于记忆不深刻，注释就变得尤为重要；

写注释时要注意：不要写代码干了什么，而要写代码为什么要这么写，背后的考量是什么。

```javascript
// 例子
var offset = 0, includeLabels = true;
if (includeLabels) {
  // 偏移量offset赋值20  =====> 不推荐的注释
  offset = 20;  // 如果这个includeLabels为true，必须要给这个标签一个20px的偏移量；因为不赋值会引起bug
}
```

###### Ⅴ.排查es6语法

.eslintrc.js 配置参数解析器选择5，env参数es6为false

```javascript
module.exports = {
  env: {
    "es6": false
  }
  parserOptions: {
		"ecmaVersion": 5
	},
}
```

**注意**：有一定局限性，检查出一个问题后，会检查不出第二个第三个，需要逐一修复；

#### Eslint + Prettier结合使用

ESlint主要解决了代码质量问题，代码风格问题其实是靠Airbnb JavaScript style Guide规划的，并没有完完全全做完，并且配置项参数多且复杂。风格问题其实主次不高，不会引起bug只是不好看，这时候就可以使用Prettier，prettier是一个有主见（偏见）的代码格式化工具(opinionated code formatter)。

ESlint + Prettier结合使用，我们需要通过安装eslint-config-prettier，这个工具禁止了一些不必要的以及和Prettier相冲突的ESlint规则，同时还要安装eslint-plugin-prettier，这个插件是将prettier最为ESlint的规则来使用，这样当代码不符合prettier规范时，会报一个ESlint的错误

Prettier官网（可以在线格式化）：https://www.prettier.cn/

**基础使用**：

##### 1.安装依赖

```javascript
npm install --save-dev prettier  // prettier的本体
// 结合ESlint和Prettier的插件：
npm install --save-dev eslint-config-prettier // 用于解决eslint与prettier共用时发生的一些冲突
npm install --save-dev eslint-plugin-prettier // 将Pretier作为ESLint规则运行，并将差异作为ESLint报错。
```

##### 2.解决冲突，模板配置

①创建prettier配置文件，可用于自定义一些配置项

创建.prettierrc.js或者prettier.config.js文件，也可以是.prettierrc.json，json不需要exports；

配置文件中不要带注释

```javascript
// 基础的配置项例子
module.exports = {
    printWidth: 200,
		tabWidth: 4,
		useTabs: true,
		singleQuote: false,
		semi: true,
		trailingComma: "none",
		insertPragma: false,
		bracketSameLine: false,
		bracketSpacing: true,
		htmlWhitespaceSensitivity: "ignore",
		singleAttributePerLine: false,
		embeddedLanguageFormatting: "auto",
		quoteProps: "as-needed",
		requirePragma: false,
		endOfLine: "auto"
}
```

创建配置JSON文件

②配置.eslintrc.js文件将两者结合使用，并且解决冲突；

```javascript
{
    "extends": ["eslint:recommended","prettier"], // 写在继承中，这是eslint-config-prettier 关闭一些eslint的配置
    "plugins": ["prettier","html"], // 这是eslint-plugin-prettier  写在插件里
    "rules": {
    "prettier/prettier": "warn" // 会将一些prettier的规则以eslint形式提示
    }
}
```

**注**：还有一种简化的写法，官方的推荐配置

```javascript
{
    "extends": ["plugin:prettier/recommended"] // plugins中就不用写了，添加到数组的最后一个元素，以防被盖了
}
```

配置完后重启，使用上面eslint 的命令检查，就会打印一些prettier的warn。这里可能会存在一个问题，![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1670235373860-7290b6eb-d54d-4fd0-a579-51c3bd4622f6.png)

由于编码导致，可以使用编译器右下角，更换编码格式为utf-8即可

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1670235761715-1efdf569-0c0d-478a-96af-6261da5b485d.png)



当在eslint中写了semi:1 要加分号，而在prettier中设置了不需要分号；就会发生冲突；会提示出来，然后取舍一下，删去某一个；

③也可以像eslint一样，创建.prettierignore文件，以实现忽略一些文件的格式化

##### 3.如何修复？

在package.json文件中配置修复的脚本指令

```javascript
"scripts": {
    "write": "prettier --write src/**"
},
```

##### 4.常用自定义配置项

```javascript
module.exports = {
    printWidth: <int>  // 一行最大的字符数，超过就换行，默认80
    tabWidth: <int> // 缩进 默认2
    useTabs: <bool> // 使用制表符而不是空格
    semi: <bool> //  末尾是否有分号
    singleQuote: <bool> // 是否使用单引号
    quoteProps:"<as-needed|consistent|preserve>"  // 对象中属性的引号 分别代表 1.属性需要时加；2.对象中至少有一个属性需要引号，所有属性加 3.遵循原则，遵循输入的引号
    trailingComma: "<es5|none|all>" // 为多行数组的非末尾行添加逗号(单行数组不需要逗号)，配置项：none：不需要逗号；es5：在es5中生效的逗号（对象，数组等）；all：任何可以添加逗号的地方
    bracketSpacing: <bool> // 括号空格，在对象字面量和括号之间添加空格
    bracketSameLine: <bool> //  将多行元素的 > 放置于最后一行的末尾，而非换行
    arrowParens: "<always|avoid>" // 箭头函数，当只有一个参数的时候去除括号
    singleAttributePerLine: <bool>  // 在HTML、Vue和JSX中强制每行使用一个属性。
    embeddedLanguageFormatting: "off"   // 控制Pretier是否格式化文件中嵌入的引用代码。 auto，如果prettier能够自动识别到嵌入的代码，就将其格式化
    proseWrap: "<always|never|preserve>" //  "always" - 超出长度自动折行 "never" - 用不折行  "preserve"- 使用默认的折行标准
    endOfLine: "<lf|crlf|cr|auto>" // 换行符使用啥
} 
```

添加注释// prettier-ignore 可以忽略格式化 或者按下方的方式添加开始结束标签

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1670305201742-77cd680f-265e-4a2a-8575-0c399404dcfe.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/28662297/1670305227037-38ba5f6a-29dc-4cb3-a541-529ec753a3df.png)

##### 5.vscode插件的一些设置实现保存并且自动格式化

搜索插件prettier code formatter安装即可，settings.json中再添加以下配置，作用是修改默认的格式化工具为prettier

```javascript
"[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
}，
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.formatOnSave": true,
```

#### eslint插件的使用

##### 1.拼写检查(eslint-plugin-spellcheck)

https://github.com/aotaduy/eslint-plugin-spellcheck（详细说明）

```javascript
// 1.安装依赖
npm install --save-dev eslint-plugin-spellcheck

// 2.在.eslintrc.js添加配置
module.exports = {
  plugins: ["spellcheck"],
  rules: {
    "spellcheck/spell-checker": [warn]
    // 也可以配置规则写了下面这条，上面的这条就不要写了
    "spellcheck/spell-checker": [1,
    	{
        "skipWords": ["Childs"]
    	}
    ]
  }
}

// 3.相关rules配置
"comments": <<Boolean>> default: true // 检查注释中的拼写
"strings": <<Boolean>>, default: true // 检查字符串内的拼写
"identifiers": <<Boolean>>, default: true // 检查标识符内的拼写
"ignoreRequire": <<Boolean>>, default: false // 从拼写检查中排除“require（）”导入。用于排除 NPM 包名称误报。
"enableUpperCaseUnderscoreCheck": <<Boolean>>, default: false // 排除选中用下划线分隔的大写单词。例如，“SEARCH_CONDITIONS_LIMIT”
"templates": <<Boolean>>, default: true // 检查ES6模板中的拼写(eslint需要开启解析器为ES6）
"lang": <<String>>, default: "en_US" // 选择要使用的语言。选项包括：“en_US”、“en_CA”、“en_AU”和“en_GB”
"langDir": <<String>>, default: "" // 语言文件目录路径。默认情况下使用插件目录。
"skipWords": <<Array Of Strings>> default: [] // 不会检查的单词数组。
"skipIfMatch": <<Array Of Strings>> default: [] // 正则表达式数组 插件将尝试匹配 js 节点元素值（标识符、注释、字符串、字符串模板等），并且不会检查整个节点内容 如果匹配，请在注释中小心，因为如果注释的一部分匹配，则不会检查整个注释，字符串也是如此。
即： “^[-w]+/[-w.]+$“ 将忽略 MIME 类型。
"skipWordIfMatch": <<Array Of Strings>> default: [] // 正则表达式数组 该插件将尝试匹配节点中找到的每个单词（标识符、注释、字符串、字符串模板等），并且不会检查匹配的单个单词。
即： “^[-w]+/[-w.]+$“ 将忽略 MIME 类型。
"minLength": <<Number>> default: 1 // 字符量小于 minLength 的单词将不会被拼写检查。
```

#### eslint+prettier epg配置插件使用

已将eslint与prettier配置结合在一起作为插件包分享到npm

##### 1.初始化目录

按照第一节基础使用中使用npm初始化目录，产生package.json文件

##### 2.安装插件包

```javascript
npm i eslint-config-epgconfig
```

npm网址：https://www.npmjs.com/package/eslint-config-epgconfig

##### 3.创建.eslintrc.js并配置

```javascript
module.exports = {
    "extends": "eslint-config-epgconfig"
}
// 如上配置即可
```

由于我们的项目涉及跨js文件的全局变量，基础配置需要一步步完善，若出现了一些全局变量未定义的报错，可以在.eslintrc.js文件中添加全局变量

例：类似于此类（no-undef）的错误![img](https://cdn.nlark.com/yuque/0/2023/png/28662297/1675235286263-61bf8628-f21c-477d-8af9-5a7963a23d79.png)

解决措施：

```javascript
module.exports = {
    "extends": "eslint-config-epgconfig",
  	"globals" : {
        log_view:"writable"// 代表可重写， readonly: 代表只读不可重写；在js添加globals
    }
}
// 备注：若采用在文件里添加globals注释的方式忽略该错误的话，默认是只读
```

##### 4.修复

###### 1.简单语法自动修复（有一定局限性）

package.json中配置脚本命名

```javascript
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

运行npm run lint:fix

**备注**：最好能够单文件手动修复（自动修复仅能修复js文件），代码风格建议手动修复

###### 2.代码风格修复

建议手动修复，风格上的报错，如果编译器为VScode，则为黄色下划线

代码风格配置参数详细说明：https://www.prettier.cn/docs/options.html#tabs

```javascript
{
  "arrowParens": "always",  // 箭头函数只有一个参数时包含括号
  "bracketSameLine": false,  
  "bracketSpacing": true,  // 对象属性与括号之间打印空格
  "embeddedLanguageFormatting": "auto", // 自动识别并格式嵌入式代码
  "htmlWhitespaceSensitivity": "css", // 为 HTML，Vue，Angular 和 Suppherbars 指定全局空白的灵敏度 “css” - 尊重 CSS 显示属性的默认值。 处理方式类似于严格 strict属性值
  "insertPragma": false, // prettier 可以在文件顶部插入特殊的@Format 标记，指定文件已格式化文件
  "jsxSingleQuote": false, // 使用单引号而不是 JSX 中的双引号
  "printWidth": 200, // 指定行字符串显示长度
  "proseWrap": "preserve", // preserve，什么都不做，不自动换行，让散文块保持原样
  "quoteProps": "as-needed", // 仅在必需的对象属性周围添加引号
  "requirePragma": false, // 可以仅限于文件顶部的仅格式化包含特殊注释的文件(在文件头部需要引入注释https://blog.csdn.net/weixin_44808483/article/details/118113753)
  "semi": true, // 需要分号
  "singleQuote": false, // 不使用单引号
  "tabWidth": 4, // 缩进4个空格
  "trailingComma": "es5", // 在 ES5（对象，阵列等）有效的尾随逗号
  "useTabs": true, // 使用制表符而不是空格缩进行
  "vueIndentScriptAndStyle": false // 不要在 Vue 文件中缩进脚本和样式标签
}
```

例子1：缩进

统一采用制表符缩进，且长度为4；

Vscode修改就可以一次性修改代码的缩进：

1.点击右下角（你的可能显示的是空格4）

![img](https://cdn.nlark.com/yuque/0/2023/png/28662297/1675235876979-d4b58df9-1a07-4bf8-89c7-c8ece0057b79.png)

2.选择将缩进转换为制表符![img](https://cdn.nlark.com/yuque/0/2023/png/28662297/1675235912927-5ce4bb32-1dbe-4e77-aace-25ac599c8555.png)

例子2：忽略格式化

在html中，可能会用到适配器相关，格式已经有了一定的统一；可以选择忽略

如下图中添加注释，即可忽略此段代码

![img](https://cdn.nlark.com/yuque/0/2023/png/28662297/1675236139029-5b0fb6ab-3214-489f-86b6-895f74957cd3.png)

其余修复，可以根据问题修复

例子3：![img](https://cdn.nlark.com/yuque/0/2023/png/28662297/1675236476830-35afe9b9-b021-4fe2-9a91-8bc9b6905c7c.png)1.Delete `␍⏎`

指删除换行符，表示此处多了一个换行

2.Insert `·`

指插入一个空格，此处需要空一格

3.Replace `(g_param.smallPlayIndex·/·10)` with `g_param.smallPlayIndex·/·10`

指将(g_param.smallPlayIndex / 10) 替换为g_param.smallPlayIndex / 10 即去除括号

综上所述：一些格式，语法的问题可以根据提示去手动修改（建议！！！），自动修复存在局限