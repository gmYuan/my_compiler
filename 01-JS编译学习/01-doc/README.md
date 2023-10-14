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


09- 生成AST语法树
  - 通过tokenizer对JSX进行词法分析==> 分词为一个个的token
  - 通过walk递归函数 + AST树形结构对象，对不同类型的token进行对象化，形成AST
  - 具体代码，见 [parse-AST构造过程](../src/index.js)


10- AST语法树的遍历
  - 对每个AST节点进行遍历，具体见 [transfromer- traverse子过程](../src/transformer.js)


11- AST语法树的转化
  - 通过提供traverse的visitor选项方法，来对AST的对象节点树，进行新旧节点对象的替换
  - 具体代码，见 [transfromer过程](../src/transformer.js)


12- 代码生成
  - 本质上，还是递归下降算法的实现 ，具体见 [codeGenerator](../src/index.js)


13- 优先级结合性
  - 优先级-结合性含义
  - 正确文法规则：先低后高，由低优先级--> 高优先级

14- 解决左递归和结合性的矛盾
  - 改进递归下降的原理==> 不包含自身递归的下降算法


## 参考文档：

[在线文档](http://www.zhufengpeixun.com/strong/html/103.15.webpack-compiler2.html#t105.%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90)