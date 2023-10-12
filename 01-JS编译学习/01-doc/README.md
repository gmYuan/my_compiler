## 思维导图

1 需求分析==> 2 编译器工作流
  - 2.1 解析==> 词法分析
  - 2.2 解析==> 语法分析 


2.1 词法分析
  - 使用 esprima库进行JSX词法分析 + 使用estraverse-fb库进行 JSX的AST 节点遍历，具体代码，见[库词法分析-tokenizer](./01-3-词法分析-tokenizer.js)

  - 使用 自定义状态机对【"10+20"】进行词法分析，具体见 [自定义状态机的简单词法分析](./02-4-状态机实现-lexier.js)

  - 使用 自定义状态机对 简单JSX进行词法分析，具体见 [自定义状态机的JSX词法分析](../src/tokenizer.js)


04— 语法分析原理概述 和 流程图分析概述（以简单的加法和乘法 四则运算为例）

05~06- 语法分析具体实现准备，以简单 四则运算为例
  - parse(source)==>
    - tokenReader = tokenize(script) + toAST(tokenReader)==> return AST
      - 05- 通过正则进行词法分析，具体见 [正则词法分析](../formula/index.js) 

      - 06- 创建ASTNode类 && rootNode，具体见 [toAST实现导入依赖和主入口函数](../formula/toAST.js)


07 语法分析实现
  - 根据 词法分析结果tokenReader + 递归下降算法，生产四则运算的 AST结构
  - 根据 AST结构进行 定义操作运算，得出计算结果
  - 具体代码，见 [四则运算计算过程](../formula/index.js)


08- 实现运算支持 减法和小括号，以完善 生成AST的运算功能
  - 之前只支持加法和乘法的问题，在于其 结合性顺序不正确
  - 修改方法是: 增加并定义好 四则运算的优先级
  - 具体见 [toAST实现-debug流程](../formula/toAST.js)



## 参考文档：

[在线文档](http://www.zhufengpeixun.com/strong/html/103.15.webpack-compiler2.html#t105.%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90)




结合性错了

结合性
左结合
1+2+3

2*3*4


右结合
a=b=c


现在这种写法有一个本质的错误!!
运算符有结合性 乘法和除法 从左往右结合
加法和减法也是
本来我们的文法结构应该是这样的写
如果你的文法是这样的写的,就会结合性是正常的,计算顺序是从左往后算的
左递归?
2+3+4
((2+3)+4)
add ->  add|add+multiple
((2*3)*4)
multiple -> multiple|multiple*NUMBER

如果是如何写样写的,又会现左递归的问题 

如果改成这样,就不会出现左递归
但是结合性又不对了
2+3+4
(2+(3+4)) 计算顺序就错了
add ->  multiple|multiple+add
multiple -> NUMBER|NUMBER*multiple

//TODO 非常复杂的解决方案?



1. 如何解决左递归和结合性的问题?
2. 写一个命令行工具,你可以命令里实时输入表达式,然后计算结果输出 ,完美支持加减乘除和括号
3. 转换器 把jsx语法树转换成js语法树
4. 生成器 把js语法树重新生成源代码