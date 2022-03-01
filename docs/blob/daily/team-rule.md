## 团队规范

每个研发团队都需要有自己的代码规范,这样才能够统一每个人书写代码格式,减少CodeReview的时间,提前检查出一些不符合代码规范的代码.一般前端检查代码规范的组合拳都是:eslint+lint-staged-husky+prettier.我们接下来介绍一下每个工具的作用.

----------------
## [eslint](https://eslint.bootcss.com/docs/user-guide/getting-started)
1. 官方解读:ESLint 是在 ECMAScript/JavaScript 代码中识别和报告模式匹配的工具，它的目标是保证代码的一致性和避免错误.
2. 实现:
- ESLint 使用 Espree 解析 JavaScript。
- ESLint 使用 AST 去分析代码中的模式
- ESLint 是完全插件化的。每一个规则都是一个插件并且你可以在运行时添加更多的规则。
3. 配置
```js
.eslintrc
{
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"],
        ...
    }
}
```
4. 那么我们可以从以上几点得知如果我们在ESlint的配置中配置了相关的规则,那么eslint就会通过Espree把我们的代码解析成AST对象之后,在根据配置中的规则来查看是否匹配规则,不匹配则根据off/warn/error来做对应的提示.




## [prettier](https://www.prettier.cn/)
1. 官方解读:Prettier is an opinionated code formatter.简单的来说Prettier是一个固执己见的代码格式化程序.
2. 实现:
- 也是利用解析器把代码解析成AST,然后AST进行遍历,最后根据规则整合代码
3. 配置
```js
.prettierrc.js
module.exports = {
  printWidth: 100, // 一行的字符数，如果超过会进行换行，默认为80，官方建议设100-120其中一个数
  tabWidth: 2, // 一个tab代表几个空格数，默认就是2
  useTabs: false, // 启用tab取代空格符缩进，默认为false
  semi: true, // 结尾加上分号, 默认true
  ...
};
```
4. Prettier就是利用你在配置中配置的选项来格式话你的代码,比如长度,标点符号...


## [lint-staged](https://www.npmjs.com/package/lint-staged)
1. 官方解读:Run linters against staged git files and don't let 💩 slip into your code base!意思就是对暂存的git文件跑linter,防止一些不好的代码溜进你的代码仓库.
2. 实现(https://segmentfault.com/a/1190000019466459)
3. 配置
```js
.lintstagedrc 
{
  "src/**": [
      "prettier --write",
      "eslint --fix",
      "git add ."
    ]
}
```
4. 就是在你给出需要校验文件的范围,然后执行对应的命令,比如prettier --write来帮你做一些代码的格式化


## [husky](https://typicode.github.io/husky/#/?id=bypass-hooks)
1. Husky improves your commits and more 🐶 woof! You can use it to lint your commit messages, run tests, lint code, etc... when you commit or push. Husky supports all Git hooks. husky是一个Git Hook工具.就是提交的时候会触发一些git的钩子,在钩子中做一些你想做的事情(简化了Git hooks的操作)
2. 配置:
```js
.package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged", 
    }
  }
}
```
3. 就是在你提交代码的时候,在不同时期的Hook里面做一些操作.比如commit-msg,pre-commit这些hook来帮助你做一些校验.

## 总结
总的来说,eslint负责代码的质量和规范(比如一些变量声明,单双引号等规范),prettier是负责代码的格式(行宽,结尾使用分号等),lint-staged是负责寻找指定范围内git本地暂存的代码,然后运行对应的指令,husky是在你提交代码的时候执行git的钩子,然后执行对应的脚本命令.


