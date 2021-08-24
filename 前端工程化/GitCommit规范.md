[参考文章](https://zhuanlan.zhihu.com/p/182553920)

### commit message 格式

```
<type>(scope): <subject>
```

### type

表示commit的类别,只允许是使用下面的标识

+ **feat**: 新功能(feature)
+ fix: 适合一次提交直接修改的BUG
+ to: 适合多次提交修改的BUG, 最终修复问题提交时使用fix
+ docs: 文档
+ style: 格式(不影响代码运行的变动)
+ refactor: 重构(既不是新功能, 也不是修改Bug的代码变动)
+ perf: 优化相关, 比如 提升性能 体验
+ test: 增加测试
+ chore: 构建过程或辅助工具的变动
+ revert: 回滚到上一个版本
+ merge: 代码合并
+ sync: 同步主线或分支的Bug



## 使用工具

[相关文章](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

```shell
# 全局安装
npm install -g commitizen

# 在项目中
commitizen init cz-conventional-changelog --save --save-exact

# git commit 使用 git cz
```

git cz 命令的使用

```shell
git cz

cz-cli@4.1.2, cz-conventional-changelog@3.2.0

#指定commit的类型，约定了feat、fix两个主要type，以及docs、style、build、refactor、revert五个特殊type
? **Select the type of change that you're committing:** fix:   A bug fix

#用于描述改动的范围，格式为项目名/模块名
? **What is the scope of this change (e.g. component or file name): (press enter t**
**o skip)** index.html

#对改动进行简短的描述
? **Write a short, imperative tense description of the change (max 83 chars):**
 (11) add a blank

#对改动进行长的描述
? **Provide a longer description of the change: (press enter to skip)**

#是破坏性的改动吗
? **Are there any breaking changes?** No

#影响了哪个issue吗，如果选是，接下来要输入issue号
? **Does this change affect any open issues?** No
```



