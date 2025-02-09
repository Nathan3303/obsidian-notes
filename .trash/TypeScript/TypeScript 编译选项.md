## 自动编译文件

编译文件时，添加 `-w` 或 `--watch` 参数能够使 TypeScript 编译器自动监视文件变化，并在变化后自动对文件进行重编译
示例：`tsc xxx.ts -w` 或 `tsc xxx.ts --watch`

## 自动编译整个项目

直接在项目根目录中直接指定 tsc 命令，则编译器会将整个项目中的所有 .ts 文件都编译为 .js 文件，并各自与 .ts 文件同目录。
但是直接使用 tsc 命令需要在根目录下创建 ts 编译器的配置文件 tsconfig.json，并在其中添加一些编译选项。

### 配置选项

-   include
    -   定义：期望被编译文件所在的目录
    -   默认值：`["**/*"]`
        -   `**` 表示任意目录
        -   `*` 表示任意文件
    -   示例：
        ```json
        "include": ["src/**/*", "tests/**/*"]
        ```
-   exclude
    -   定义：需要排除在外的目录
    -   默认值：`["node_modules", "bower_components", "jspm_packages"]`
    -   示例：
        ```json
        "exclude": ["./src/hello/**/*"]
        ```
-   extends
    -   定义：被继承的配置文件
    -   描述：当前配置文件中会自动包含 config 目录下 base.json 中的所有配置信息
    -   示例：
        ```json
        "extends": "./configs/base"
        ```
-   files
    -   定义：指定被编译文件的列表，只有需要编译的文件少时才会用到
    -   示例：
        ```json
		"files": [
			"core.ts",
			"sys.ts",
			"scanner.ts",
			// ...
		]
        ```
-   compilerOptions
    -   定义：编译选项
    -   描述：compilerOptions 包含多个子选项，完成对编译的设置
    -   部分子选项解释：
        -   target
            -   定义：设置 TypeScript 编译的目标版本
            -   可选值：ES3（默认）、ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext
        -   lib
            -   定义：指定代码运行时所包含的库
            -   可选值：ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost 等
        -   module
            -   定义：指定所要使用的模块化规范
            -   可选值：none, commonjs, ES6/ES2015, amd, system, umd, es6, es2020, esnext
        -   outDir
            -   定义：指定编译完成后文件的输出目录
        -   outFile
            -   定义：将所有全局作用域中的代码编译合并到的一个文件中
            -   注意事项：当使用了模块化语句后，只有符合 amd 和 system 模块化规范的代码能够合并到一个文件中
        -   allowJs
            -   定义：是否对 .js 文件进行编译
            -   默认值：false
        -   checkJs
            -   定义：是否检查 .js 文件中代码的语法规范
            -   默认值：false
        -   removeComments
            -   定义：是否移除注释内容
            -   默认值：false
        -   noEmit
            -   定义：不生成编译后的文件
            -   默认值：false
        -   noEmitOnError
            -   定义：当有错误时不生成编译后的文件
            -   默认值：false
        -   strict
            -   定义：所有严格检查的总开关
            -   描述：若该项设置为 true，则所有严格检查的相关项全部为 true
            -   默认值：false
        -   alwaysStrict
            -   定义：保持 JavaScript 严格模式的开启
            -   默认值：false
        -   noImplicitAny
            -   定义：不允许隐式指定 any 类型
            -   默认值：false
        -   noImplicitThis
            -   定义：不允许使用不明确的 this 对象
            -   默认值：false
        -   strictNullChecks
            -   定义：严格检查 null 值
            -   默认值：false
    -   配置示例：
        ```json
        "compilerOptions": {
            "target": "es6",
            "module": "es2015",
            // "lib": ["dom"]
            "outDir": "./dist",
            // "outFile": "./dist/app.js"
            "allowJs": true,
            "checkJs": true,
            "removeComments": true,
            "noEmitOnError": true,
            "strict": true,
            "alwaysStrict": true,
            "noImplicitAny": true,
            "noImplicitThis": true,
            "strictNullChecks": true,
        }
        ```
