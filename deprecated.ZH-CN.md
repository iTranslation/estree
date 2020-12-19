<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
This document specifies deprecated extensions to the ESTree API that were at one point supported in Mozilla's SpiderMonkey JavaScript engine for features that were experimental or came from defunct standards.

本文档指定了不赞成使用的ESTree API扩展，这些扩展在 Mozilla 的 SpiderMonkey JavaScript 引擎中曾经受过支持，其功能是试验性的或来自已废止的标准。

- [Functions](#functions)
- [Statements](#statements)
  - [ForInStatement](#forinstatement)
  - [LetStatement](#letstatement)
  - [SwitchStatement](#switchstatement)
  - [TryStatement](#trystatement)
- [Expressions](#expressions)
  - [ComprehensionExpression](#comprehensionexpression)
  - [GeneratorExpression](#generatorexpression)
  - [GraphExpression](#graphexpression)
  - [GraphIndexExpression](#graphindexexpression)
  - [LetExpression](#letexpression)
- [Clauses](#clauses)
  - [CatchClause](#catchclause)
  - [ComprehensionBlock](#comprehensionblock)
- [Miscellaneous](#miscellaneous)
  - [BinaryOperator](#binaryoperator)
- [E4X](#e4x)
  - [Declarations](#declarations)
    - [XMLDefaultDeclaration](#xmldefaultdeclaration)
  - [Expressions](#expressions-1)
    - [XMLAnyName](#xmlanyname)
    - [XMLQualifiedIdentifier](#xmlqualifiedidentifier)
    - [XMLFunctionQualifiedIdentifier](#xmlfunctionqualifiedidentifier)
    - [XMLAttributeSelector](#xmlattributeselector)
    - [XMLFilterExpression](#xmlfilterexpression)
    - [XMLElement](#xmlelement)
    - [XMLList](#xmllist)
  - [XML](#xml)
    - [XMLEscape](#xmlescape)
    - [XMLText](#xmltext)
    - [XMLStartTag](#xmlstarttag)
    - [XMLEndTag](#xmlendtag)
    - [XMLPointTag](#xmlpointtag)
    - [XMLName](#xmlname)
    - [XMLAttribute](#xmlattribute)
    - [XMLCdata](#xmlcdata)
    - [XMLComment](#xmlcomment)
    - [XMLProcessingInstruction](#xmlprocessinginstruction)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Functions

```js
extend interface Function {
    body: BlockStatement | Expression;
    expression: boolean;
}
```

如果 `expression` 的标志为 true ，则这个函数是一个 [expression closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/1.8#Expression_closures_%28Merge_into_own_page.2fsection%29) ，其 body 字段为一个表达式。

# Statements

## ForInStatement

```js
extend interface ForInStatement {
   each: boolean;
}
```

如果 `each` 为 true ，这是一个 `for each`/`in` 语句。

## LetStatement

```js
interface LetStatement <: Statement {
    type: "LetStatement";
    head: [ VariableDeclarator ];
    body: Statement;
}
```

`let` 语句。

## SwitchStatement

```js
extend interface SwitchStatement {
    lexical: boolean;
}
```

`lexical` 标志为 metadata，它标志着 `switch` 语句是否包含了未声明的 `let` （因此这将需要引入新的词法作用域）。 

## TryStatement

```js
extend interface TryStatement {
    handlers: [ CatchClause ];
    guardedHandlers: [ CatchClause ];
}
```

`handlers` 的 `length` 属性可能是任何非负的整数。

# Expressions

## ComprehensionExpression

```js
interface ComprehensionExpression <: Expression {
    type: "ComprehensionExpression";
    body: Expression;
    blocks: [ ComprehensionBlock ];
    filter: Expression | null;
}
```

An array comprehension. The `blocks` array corresponds to the sequence of `for` and `for each` blocks. The optional `filter` expression corresponds to the final `if` clause, if present.

## GeneratorExpression

```js
interface GeneratorExpression <: Expression {
    type: "GeneratorExpression";
    body: Expression;
    blocks: [ ComprehensionBlock ];
    filter: Expression | null;
}
```

一个 generator 表达式。作为数组 comprehensions ，`blocks` 数组对应到 `for` 和 `for each` 块的序列，可选的 `filter` 表达式则对应到最后的 `if` 子句，如果存在的话。

## GraphExpression

```js
interface GraphExpression <: Expression {
    type: "GraphExpression";
    index: uint32;
    expression: Literal;
}
```

一个 graph 表达式。又名  "sharp literal,"， 如 `#1={ self: #1# }`。

## GraphIndexExpression

```js
interface GraphIndexExpression <: Expression {
    type: "GraphIndexExpression";
    index: uint32;
}
```

一个 graph index 表达式，又名 "sharp variable,"，如 `#1#` 。

## LetExpression

```js
interface LetExpression <: Expression {
    type: "LetExpression";
    head: [ VariableDeclarator ];
    body: Expression;
}
```

一个 `let` 表达式。

# Clauses

## CatchClause

```js
extend interface CatchClause {
    guard: Expression | null;
}
```

可选的 `guard` 属性对应于绑定变量的可选的表达式 guard 。

## ComprehensionBlock

```js
interface ComprehensionBlock <: Node {
    type: "ComprehensionBlock";
    left: Pattern;
    right: Expression;
    each: boolean;
}
```

一个 `for` 或者 `for each` 块在一个数组 comprehension 或是 generator 表达式中。

# Miscellaneous

## BinaryOperator

```js
extend enum BinaryOperator {
    ".."
}
```

`".."` token 是 E4X-specific 。

# E4X

E4X 由 [ECMA-357](http://www.ecma-international.org/publications/standards/Ecma-357.htm) 但已成为废弃的标。它在 SpiderMonkey 中实施了数年，但在[Firefox 21 中已经被移除](https://bugzilla.mozilla.org/show_bug.cgi?id=788293)。

## Declarations

### XMLDefaultDeclaration

```js
interface XMLDefaultDeclaration <: Declaration {
    type: "XMLDefaultDeclaration";
    namespace: Expression;
}
```

一个 `default xml namespace` 声明。

## Expressions

### XMLAnyName

```js
interface XMLAnyName <: Expression {
    type: "XMLAnyName";
}
```

特定的 E4X 通配的伪标识符（pseudo-identifier）`*`。

### XMLQualifiedIdentifier

```js
interface XMLQualifiedIdentifier <: Expression {
    type: "XMLQualifiedIdentifier";
    left: Identifier | XMLAnyName;
    right: Identifier | Expression;
    computed: boolean;
}
```

一个 E4X 限定标识符。如，一个 pseudo-identifier 使用的是命名空间分隔符 `::`。如果这个限定标识符有一个组合的名称（如，`id::[expr]`），那 `computed` 属性为 `true` 和 `right` 属性将为一个表达式。

### XMLFunctionQualifiedIdentifier

```js
interface XMLFunctionQualifiedIdentifier <: Expression {
    type: "XMLFunctionQualifiedIdentifier";
    right: Identifier | Expression;
    computed: boolean;
}
```

一个由 `function` 关键字限定的 E4X 标识符。如  `function::id` （此功能是非标准的SpiderMonkey扩展）

### XMLAttributeSelector

```js
interface XMLAttributeSelector <: Expression {
    type: "XMLAttributeSelector";
    attribute: Expression;
}
```

一个 E4X 属性选择表达式。如，`@` 表达式

### XMLFilterExpression

```js
interface XMLFilterExpression <: Expression {
    type: "XMLFilterExpression";
    left: Expression;
    right: Expression;
}
```

一个 E4X 列表过滤的表达式。如，`expr.(expr)` 形式的表达式。

### XMLElement

```js
interface XMLElement <: XML, Expression {
    type: "XMLElement";
    contents: [ XML ];
}
```

代表一个 XML 元素的列表的 E4X literal 。

### XMLList

```js
interface XMLList <: XML, Expression {
    type: "XMLList";
    contents: [ XML ];
}
```

代表 XML 元素的列表的 E4X literal 。

## XML

```js
interface XML <: Node { }
```

XML data.

XML 数据。

### XMLEscape

```js
interface XMLEscape <: XML {
    type: "XMLEscape";
    expression: Expression;
}
```

一个转义的 JavaScript 表达式的 XML 数据。

### XMLText

```js
interface XMLText <: XML {
    type: "XMLText";
    text: string;
}
```

Literal XML 内容。

### XMLStartTag

```js
interface XMLStartTag <: XML {
    type: "XMLStartTag";
    contents: [ XML ];
}
```

一个 XML 的 start 标签。

### XMLEndTag

```js
interface XMLEndTag <: XML {
    type: "XMLEndTag";
    contents: [ XML ];
}
```

一个 XML 的 end 标签。

### XMLPointTag

```js
interface XMLPointTag <: XML {
    type: "XMLPointTag";
    contents: [ XML ];
}
```

一个 XML 的 point 标签。

### XMLName

```js
interface XMLName <: XML {
    type: "XMLName";
    contents: string | [ XML ];
}
```

XML 的名称

### XMLAttribute

```js
interface XMLAttribute <: XML {
    type: "XMLAttribute";
    value: string;
}
```

XML 的属性值

### XMLCdata

```js
interface XMLCdata <: XML {
    type: "XMLCdata";
    contents: string;
}
```

XML 的 CDATA 节点

### XMLComment

```js
interface XMLComment <: XML {
    type: "XMLComment";
    contents: string;
}
```

XML 注释

### XMLProcessingInstruction

```js
interface XMLProcessingInstruction <: XML {
    type: "XMLProcessingInstruction";
    target: string;
    contents: string | null;
}
```

XML 处理指令
