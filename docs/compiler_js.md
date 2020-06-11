# 作用域与闭包 - 最简解释器实现（转）
如果你对 JS 闭包的实现有兴趣，就跟着这篇文章一起实现这一个最简单的解释器来重新认识一下闭包。

## 开始之前
如果是从头开始实现一个解释器，避免不了从词法分析和语法分析开始。我们对这部分工作没有兴趣，所以直接使用现有的分析器 acorn 。 acorn 是一款比较流行的分析器，babel 底层依赖的就是 [acorn](https://github.com/acornjs/acorn)。 另外，esprima 也是一个不错的选择。这儿我们选择 acorn，还有一个经常会用到的在线工具 astexplorer。 acorn 解析得到的抽象语法树符合 estree 的规范，项目中也依赖了 @types/estree。

在此之前，希望你大概明白 AST 抽象语法树是怎么回事，知道如何遍历一棵树。

这儿选择 Typescript，在开发中也随时可以查看特定节点的数据类型，代码也比较容易阅读。 测试使用 Jest，运行起来比较简单。即使并不追求 TDD，测试也可以帮助我们持续迭代。建议配置在 IDE 中运行和调试测试。

## 算术表达式解析
我们从最简单的算术表达式开始，这节的目的就是对算术表达式进行求值：给定任意的算术表达式，形如 1 + 2 * 3 - 4 / 5 我们能够正确得到结果。

## AST 抽象语法树
在开始之前，我们需要先打开 [astexplorer](https://astexplorer.net/)，对 AST 有一个感性的认识。当左侧的表达式为 1 + 2 时，右侧的得到的 AST 是这样子的 （这儿以 JSON 形式表示）：
```json
{
  "type": "Program",
  "body": [
    {
      "type": "ExpressionStatement",
      "expression": {
        "type": "BinaryExpression",
        "left": {
          "type": "Literal",
          "value": 1
        },
        "operator": "+",
        "right": {
          "type": "Literal",
          "value": 2
        }
      }
    }
  ]
}
```
这儿移除了标注代码位置的数据，以及字面量的 raw 属性，其中 raw 表示原始字符串。如果将右操作数改为 16 进制形式 1 + 0x2，我们就会发现 raw 为 '0x2'。 这棵语法树根节点表示一段程序，其 body 属性是一个数组表示可能有很多条表达式语句。对于二元表达式来说，有左右两个节点。这儿的左右节点是字面量。 我们改动一下左侧内容为 1 + 2 * 3 我们看到，右侧的节点成为了一个二元表达式，而这个新的二元表达式的两个左右节点分别为字面量 2 和 字面量 3。

## 代码实现
我们给自己的解释器起一个名字，随便起一个就叫 Z2（V8）吧。我们可以先写一个简单的测试：
```javascript
it('should return sum when adding two numbers', () => {
  const z2 = new Z2();
  expect(z2.run('3 + 4')).toBe(7);
});
```
我们借助 `acorn` 得到 AST，之后遍历这棵语法树得到执行结果就可以了。对每个节点类型，我们在一个单独的函数中处理。整个的文件结构：
```javascript
import { Parser } from 'acorn';
class Z2 {
  run(code: string) {
    const program: Node = Parser.parse(code);
    return this.evaluate(program);
  }

  evaluate(node: ESTree.Node) {
    switch (node.type) {
      case 'Program':
        return this.evaluateProgram(node);
      case 'ExpressionStatement':
        return this.evaluateExpressionStatement(node);
      case 'BinaryExpression':
        return this.evaluateBinaryExpression(node);
      case 'Literal':
        return this.evaluateLiteral(node);
      default:
        throw new Error(`Unknown node type: ${node.type}`);
    }
  }
}
```
现在我们只需要实现单独的各个节点处理函数就可以了。各个节点只关注当前的节点操作就可以了，对 `evaluateBinaryExpression` 来说，我们需要递归的调用 `this.evaluate` 以对子节点进行求值。

```javascript
evaluateBinaryExpression(node: ESTree.BinaryExpression) {
  const { operator } = node;
  const left = this.evaluate(node.left);
  const right = this.evaluate(node.right);
  switch (operator) {
    case '+':
      return left + right;
    case '-':
      return left - right;
    case '*':
      return left * right;
    case '/':
      return left / right;
    default:
      throw new Error(`Unknown operator: ${node.type}`);
  }
}
```
而字面量的处理就更简单了
```javascript
evaluateLiteral(node: ESTree.Literal) {
  return node.value;
}
```
现在可以可以测试一下，对于混合的算术运算，Z2 依然返回正确的结果。
```javascript
it('should return correct value given mixed math expression', () => {
  const z2 = new Z2();
  expect(z2.run('3 + 4 * 2')).toBe(11);
  expect(z2.run('8 + 12 / 3 + 4 * 2')).toBe(20);
  expect(z2.run('1 + 2 * 3 - 4 / 5')).toBe(6.2);
});

```
当然即使有括号改变优先级，一样不会影响程序的正确性，因为 acorn 已经给我们返回了正确的语法树。
```javascript
it('should return correct value given expression include parentheses', () => {
  const z2 = new Z2();
  expect(z2.run('(3 + 4) * 2')).toBe(14);
});
```
## 变量和环境
这次我们尝试引入变量，我们的代码也很简单。我们可以添加一条新的测试用例：
```javascript
it('should calculate correct value given expression with variable', () => {
 const z2 = new Z2();
 expect(z2.run(`
   var a = 3;
   a * 9;
 `)).toBe(27);
});
```
我们在 astexplorer 查看一下这两个代码的语法树：第一部分是变量声明；第二部分的表达式没有太大变化，只是左侧的操作数类型是标识符 { type: 'Identifier', name: 'a' }，右侧依然是字面量。 也就是说在对表达式求值时，我们知道的只有标识符的名称 a，所以，我们需要从一个地方获取到标识符的值，简单的理解为变量环境 Environment。 现在这个变量环境只是一个符号表，即便如此，我们还是创建一个单独的类吧。
```javascript
class Environment {
 private symbolTable = {};

 set(name, value) {
   this.symbolTable[name] = value;
 }

 get(name: string) {
   return this.symbolTable[name];
 }
}
```
在 Z2 run() 中每次运行代码之前，都先创建一个新的环境 this.env = new Environment();。接下来的工作就是添加对变量声明和标识符的处理函数，
```javascript
evaluateVariableDeclaration(node) {
 node.declarations.forEach((declaration) => {
   if (declaration.id.type === 'Identifier') {
     this.env.set(declaration.id.name, this.evaluate(declaration.init));
   }
 });
}
```

变量声明是一个数组因为我们可以同时声明多个变量，这儿我们只处理左侧类型是标识符的变量声明。暂时不考虑形如 const { name, id } = user; 复杂的声明。 我们做的也很简单，只是将变量值存放在环境中。 至于标识符的处理就更简单了，只需读取变量的值。

```javascript
evaluateIdentifier(node: ESTree.Identifier) {
 return this.env.get(node.name);
}
```
同样的方式，我们添加对赋值语句的支持，同时添加一些测试用例。

## 函数和闭包
接下来我们要引入函数和闭包。我们先来考察一下最最简单的函数声明和调用：

```javascript
var a = 3;
function outer() {
  var a = 5;
  return a;
}
outer();
```

相比较于变量声明，函数声明包含的属性有点多，不过我们先只关注最基本的情况，其中最重要的是参数 params 和函数体 body，暂时不考虑 params，在解释函数声明时，同样的我们需要把函数存放到上下文中， 简单起见，我们只是把整个节点存放进去。对于函数调用，这是一个 CallExpression 我们需要做的就是根据标识符名称找到我们存放的函数对象（现在只是一个节点），然后解释执行函数。 在上面的例子中，两个同名变量是不同的，函数内部是一个单独的环境。在进入函数之前，我们需要创建一个新的环境，将当前环境设置为新环境，在函数调用结束之后，我们需要恢复原来的环境。

实现代码如下，同时我们还要添加对 ReturnStatement 和 BlockStatement 的处理函数。

```javascript
evaluateFunctionDeclaration(node: ESTree.FunctionDeclaration) {
  this.env.init(node.id.name, node);
}
evaluateCallExpression(node: ESTree.CallExpression) {
  const fnNode = this.env.get(node.callee.name);
  const env = new Environment();
  const currentEnv = this.env; // 保存调用者环境
  this.env = env;
  const result = this.evaluate(fnNode.body);
  this.env = currentEnv; // 恢复调用者环境
  return result;
}
```

## 闭包的处理
我们希望函数内部可以访问函数外部的变量，我们添加一个测试用例：
```javascript
it('should be able to access outer variable in function', () => {
  const z2 = new Z2();
  expect(z2.run(`
    var a = 3;
    function outer() {
      return a;
    }
    outer();
  `)).toBe(3);
});
```
在上面的实现中，我们创建的 Environment 是一个全新的。为了访问到外部的环境变量，我们做需要一点点的变通：把当前的环境作为外部环境引用传进去。 const env = new Environment(this.env); 我们的思路是：在查找标识符的时候，也就是 Environment.get(name) 函数实现中，先在当前的环境查，如果查找不到，我们就在外部查。 修改后的 Environment 类：

```javascript
class Environment {
  private outer: Environment;

  private symbolTable = {};

  constructor(outer: Environment = null) {
    this.outer = outer;
  }

  set(name, value) {
    this.symbolTable[name] = value;
  }

  get(name) {
    if (this.symbolTable[name] !== undefined) {
      return this.symbolTable[name];
    }
    if (this.outer) {
      return this.outer.get(name);
    }
    return undefined;
  }
}
```

修改代码通过上面的测试用例。

现在我们思考一下在我们创建一个新的变量环境时，是否应该传入当前的变量环境。的确，我们这样做通过了测试。但是，我们得思考一下，这意味着什么？ 这意味着，一个函数中的变量依赖于调用者环境。而一个函数可能在几十个地方被调用，而我们根本无法得知函数里面的外部环境变量究竟是在哪儿定义的。

我们来看看实际当中我们希望的外部环境是什么？现在我们添加一个闭包的测试用例，当然这个测试现在不会通过。

```javascript
var b = 5;
function outer() {
 var b = 10;
 function inner() {
   var a = 20;
   return a + b;
  }
 return inner;
}
var fn = outer();
fn();
```

我们对闭包并不陌生，上面这个的这个例子中，fn 只是 inner 的引用，在我们调用 fn 时，我们希望外部环境是 inner 所在的环境，也就是说我们希望将函数声明时的环境作为外部环境。 修改代码，在函数声明时保存整个函数和当时的变量环境。

```javascript
evaluateFunctionDeclaration(node: ESTree.FunctionDeclaration) {
  this.env.init(node.id.name, this.createFunction(node));
}
createFunction(node) {
  return {
    parentEnv: this.env,
    node,
  };
}
```

在函数调用时将其作为外部环境
```javascript
const fn = this.env.get(node.callee.name);
const env = new Environment(fn.parentEnv);
```
修改代码通过上面的测试用例。现在我们再来看闭包的话，我们发现所谓的闭包就是函数和它所在的环境。 完整的代码见 https://github.com/fedeoo/learning-ecma262/tree/master/src

## 总结
闭包已经被大家讲烂了，个人觉得理解一个东西最好的办法就是实现一遍。简单的解释器非常容易实现（如果不需要做语法分析），建议大家读完之后自己实现一遍，也就 100 多行代码。 这儿并没有遵照 ECMAScript 规范来实现，因为要引入不少概念，比如运行时返回的是 CompletionRecord 类型，解析标识符得到的是 Reference 类型。 如果想严格的按照规范实现或者学习规范，可以参考项目 engine262。 针对想从头开始写解释器的同学，建议阅读 esprima 源码。除非你是在学编译原理，本人觉得没有必要从头开始，事实上已经有不错的工具，根据产生式来直接生成语法树了。

[作者博客](https://luckyshark.me/archives/)