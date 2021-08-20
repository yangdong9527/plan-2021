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

