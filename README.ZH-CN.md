# The ESTree Spec

很久以前，一位[令人信任的 Mozilla 工程师](http://calculist.org)在 Firefox 上创建了一种 API，该 API 将 SpiderMonkey engine 的 JavaScript 解析器变成了 JavaScript 的 API。这位工程师记录了[它的产生过程](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey/Parser_API)，而且这种格式也作为了操作 JavaScript 源码的通用语法。

同时 JavaScript 正在发展。这个项目将作为参与构建和使用这些工具来帮助开发这种格式的人的社区标准，便于了解 Javascript 的发展。

# AST 描述符语法

这种规格使用一种自定义的语法来描述它的结构。例如，在写作的时候， 'es2015.md' 就包含了一个 `Program` 的描述，如下：

```js
extend interface Program {
    sourceType: "script" | "module";
    body: [ Statement | ModuleDeclaration ];
}
```

# ESTree 指导委员会

* [Nicholas C. Zakas](https://github.com/nzakas) ([ESLint](https://github.com/eslint))
* [Ingvar Stepanyan](https://github.com/rreverser) ([Acorn](https://github.com/acornjs/acorn))
* [Junliang Huang](https://github.com/JLHwung) ([Babel](https://github.com/babel))

# 版权和许可

Copyright Mozilla Contributors and ESTree Contributors.

Licensed under [Creative Commons Sharealike](https://creativecommons.org/licenses/by-sa/2.5/).

# 设计哲学

建议添加和改动都必须遵循下面的这些准则：

1. **Backwards compatible:** 除非有巨大的支持需要更改，否则不考虑对现有的结构进行非添加性质的改动。([eg. #65](https://github.com/estree/estree/issues/65))

2. **Contextless:** Nodes 应该和它的父节点没有任何关联的信息。如，`FunctionExpression` 不应该知道它自己是否是一个 concise 函数。(eg. [#5](https://github.com/estree/estree/issues/5))

3. **Unique:** 信息不能重复。如，如果从 `value` 中可以看出类型的表述，则 `kind` 这样的属性不应出现在 `Literal` 中。(eg. [#61](https://github.com/estree/estree/issues/61))

4. **Extensible:** 新的 nodes 需要便于将来添加新的 spec 描述。这也意味着拓展 node 类型的覆盖范围。如， `NewTarget` 上的 `MetaProperty` 就覆盖了未来支持的元属性。 (eg. [#32](https://github.com/estree/estree/pull/32))

# 致谢

ESTree 这些年间得益于人们的贡献。我们由衷的感谢这些人对该项目的重大贡献：

[Sebastian McKenzie](https://github.com/sebmck) ([Babel](https://github.com/babel/babel)), Kyle Simpson ([@getify](https://github.com/getify)), [Mike Sherov](https://github.com/mikesherov) ([Esprima](https://github.com/jquery/esprima)), [Ariya Hidayat](https://github.com/ariya) ([Esprima](https://github.com/jquery/esprima)), [Adrian Heine](https://github.com/adrianheine) ([Acorn](https://github.com/acornjs/acorn)), [Dave Herman](https://github.com/dherman) (SpiderMonkey), Michael Ficarra ([@michaelficarra](https://github.com/michaelficarra)).
