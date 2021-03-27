---	
layout:     post	
title:      『Object-Oriented Project Analysis』 Derivation Of Expression	
subtitle:   『面向对象项目分析』 表达式求导    
date:       2021-03-24	   
author:     Coekjan 
header-img: img/post-bg-OO.jpg	
catalog:    true	
katex:    true    
tags:	
    - Object-Oriented Project Analysis  
---

## 项目背景与描述

识别某一文法的函数表达式, 根据求导法则进行求导, 对结果进行简化并输出. 形式化文法如下:

$$
\begin{aligned}
    \text{Expression}\rightarrow&\text{White [PlusMinus White] Term White}\\
    &\mid\text{Expression PlusMinus White Term White}\\
    \text{Term}\rightarrow&\text{[PlusMinus White] Factor}\\&\mid\text{Term White '*' White Factor}\\
    \text{Factor}\rightarrow&\text{VarFactor \textbar ConstFactor \textbar ExprFactor}\\
    \text{VarFactor}\rightarrow&\text{PowFuncFactor}\mid\text{TriFuncFactor}\\
    \text{ConstFactor}\rightarrow&\text{SignedInt}\\
    \text{ExprFactor}\rightarrow&\text{'(' Expression ')'}\\
    \text{TriFuncFactor}\rightarrow&\text{'sin' White '(' White Factor White ')' [White Index]}\\
    &\mid\text{'cos' White '(' White Factor White ')' [White Index]}\\
    \text{PowFuncFactor}\rightarrow&\text{'x' [White Index]}\\
    \text{Index}\rightarrow&\text{'**' White SignedInt}\\
    \text{SignedInt}\rightarrow&\text{[PlusMinus] Integer}\\
    \text{Integer}\rightarrow&\text{(0|1|2|...|9)\{0|1|2|...|9\}}\\
    \text{White}\rightarrow&\text{\{WhiteChar\}}\\
    \text{WhiteChar}\rightarrow&\text{'\textbackslash u0009' \textbar '\textbackslash u0020'}\\
    \text{PlusMinus}\rightarrow&\text{'+' \textbar '--'}
\end{aligned}
$$

其中的符号作如下解释:
* $\text{A}\mid\text{B}$ 表示选择 $\text{A}$ 和 $\text{B}$ 两者之一, 优先级最低
* $\lbrace\text{T}\rbrace$ 表示 $\text{T}$ 重复0, 1或多次
* $[\text{T}]$ 表示 $\text{T}$ 重复0或1次
* $(\text{T})$ 表示 $\text{T}$ 仅出现1次, 用于调整优先级
* $\text{'T'}$ 表示字符 $\text{T}$ 本身
* $0,1,2,\dotsm,9$ 表示这些数字本身

流程图如下:

$$
\begin{CD}
    \mathtt{String} @>\mathtt{Parser}>> \mathtt{Function} @. \mathtt{Function} @>\mathtt{toString}>>\mathtt{String}\\
    @. @V\mathtt{simplify}VV @A\mathtt{simplify}AA\\
    @. \mathtt{Function} @>\mathtt{diff}>> \mathtt{Function}
\end{CD}
$$

可见设计的**核心是 `Function` 类**, 这个类至少需要具备 `diff` , `simplify` , `toString` 等方法, 后文将详细分析.

此外, 识别文法的 `Parser` 也是相当重要的, 本项目将使用**递归下降算法**进行分析.

若以输出字符串的长度作为性能指标, 应当在化简 `simplify` 下功夫, 本文将介绍 `Function` **合并同类项**的编写.

## 抽象 `Function` 架构

考虑到文法中出现的函数类型:
1. 常数
2. 变量
3. 幂函数
4. 正弦函数
5. 余弦函数
6. 加法函数
7. 乘法函数

虽然出现的函数类型不多, 但其**组合嵌套情况数不胜数**, 因而要深思熟虑, 选择合适的架构. **笔者在探索中使用了如下的架构, 仅供读者参考**.

首先定义顶层**抽象**类 `Function` , 包含 `diff` , `simplify` , `toString` 等抽象方法:

```java
public abstract class Function {
    // ...
    public abstract Function diff();

    public abstract Function simplify();

    public abstract String toString();
    //...
}
```

> `simplify` 方法将在化简部分详述, 按下不表.

### `diff` 方法

该设计的最重要的特色就是**对于任何函数, 只需要统一继承了 `Function` 就可以只关注自身的求导方案, 完成任意的组合嵌套情况.**

基于这一点, 我们就可以为涉及的7种函数设计自身的求导方案.

以正弦函数 `SineFunction` 的求导方案为例, 解释这一架构的特色:

```java
public class SineFunction extends Function {
    private final Function inner;

    public SineFunction(Function inner) {
        this.inner = inner;
    }

    /**
     * [sin(F)]' = cos(F) * F'
     */
    public Function diff() {
        return new Multiplier(new CosineFunction(inner), inner.diff());
    }
    //...
}
```

无需关心 `inner` 的求导方案, 只需知道 `inner` 具备其求导的方案就可以完成该函数的求导.

### `toString` 方法

该设计还允许**对于任何函数, 只需要统一继承了 `Function` 就可以 *基本上* 关注自身的字符串化方法, 完成任意的组合嵌套情况.**

以加法函数 `Adder` 的 `toString` 为例, 解释这一特征:

```java
public class Adder extends Function {
    private final ArrayList<Function> functions = new ArrayList<>();

    //...

    @Override
    public String toString() {
        StringJoiner sj = new StringJoiner("+");
        functions.forEach(f -> sj.add(f.toString()));
        return sj.toString();
    }
}
```

无需关心一系列被加的 `functions` 如何字符串化, 只需知道 `functions` 中每一个 `Function` 都具备 `toString` 方法, 即可完成 `Adder` 的字符串化.

### 不可变对象

可以注意到, 本架构中 `diff` 与 `simplify` 方法都返回一个新的 `Function` , 致力于设计**不可变对象**. 使用不可变对象将降低逻辑复杂度, 不易出错.

## 正则表达式与递归下降法解析输入串

递归下降算法可用于快速编写形式化文法的解析器. 下面以一个更简单的文法入手, 使用递归下降解析文法.

$$
\begin{aligned}
    \text{E}&\rightarrow\text{T \textbar T '+' E}\\
    \text{T}&\rightarrow\text{F \textbar F '*' T}\\
    \text{F}&\rightarrow\text{num\textbar '(' E ')'}
\end{aligned}
$$

其中的符号解释如下:
* $\text{A}\mid\text{B}$ 表示选择 $\text{A}$ 和 $\text{B}$ 两者之一, 优先级最低
* $\text{'S'}$ 表示字符 $\text{S}$ 本身
* $\text{num}$ 是 `int` 范围内允许前导零的十进制正整数

### 正则表达式识别 `Token`

对于这样的文法, 我们首先要把字符串转化为 `Token` 流以方便分析. 为此, 可使用正则表达式提取 `Token` :

```java
private static final Pattern TOKEN_PATTERN = Pattern.compile(
    "(?<PLUS>\\+)|(?<MUL>\\*)|(?<NUM>\\d+)|(?<BRA>\\()|(?<KET>\\))|(?<ERR>.)"
);

// PLUS, MUL, NUM, BRA, KET are all 'Token.Type', but ERR is not.

private final ArrayList<Token> tokens = new ArrayList<>();

// ...

private void extractTokens() throws IllegalFuncString {
    Matcher m = TOKEN_PATTERN.matcher(source);
    lab : while (m.find()) {
        for (Token.Type t : Token.Type.values()) {
            if (m.group(t.toString()) != null) {
                tokens.add(/* ... */);
                continue lab;
            }
        }
        throw new IllegalFuncString(); // ERR detected
    }
}
```

### 递归下降分析

随后可以编写如下的递归下降解析方式(伪代码):

```pascal
PARSE_E() :
    term = PARSE_T()
    IF tokens END:
        EXIT term
    ELSE:
        ASSERT tokens[current] == '+'
        current = current + 1
        EXIT Adder(term, PARSE_E())

PARSE_T() :
    fact = PARSE_F()
    IF tokens END:
        EXIT fact
    ELSE:
        ASSERT tokens[current] == '*'
        current = current + 1
        EXIT Multiplier(fact, PARSE_T())

PARSE_F() :
    CASE tokens[current].type :
        NUM  =>
            num = tokens[current]
            current = current + 1
            EXIT num
        BRA  =>
            current = current + 1
            expr = PARSE_E()
            ASSERT tokens[current].type == KET
            EXIT expr
        OTHR =>                         // assert : program won't reach here.
            RAISE ERROR
```

这样, 通过合理的构造函数设置, 即可通过 `PARSE_E` 函数完成表达式模型的构建.

递归下降充分利用了**看下一个 `Token`** 的能力, 为将要到来的表达式做引导.

### 本项目中递归下降的注意要点

本项目中的文法存在左递归:

$$
\text{A}\rightarrow\text{A B \textbar C}
$$

可以使用如下手段消除左递归:

$$
\begin{aligned}
    \text{A}&\rightarrow\text{C \^A}\\
    \text{\^A}&\rightarrow\text{B \^A} \mid \varepsilon
\end{aligned}
$$

其中 $\varepsilon$ 是空串.

## 基本化简, "拆包装"与合并

### 基本化简

下面先讲其他5钟函数对象的化简:
1. 常数
2. 变量
3. 幂函数
4. 正弦函数
5. 余弦函数

最为平凡的化简是常数与变量的化简, 它们的化简是它们本身. 其余三个函数的化简也相对简单, 只需要讨论一些简化情形即可. 下面以幂函数为例:

```java
// ... inside PowerFunction
@Override
public Function simplify() {
    if (inner.equals(Constant.ZERO)) {
        return Constant.ZERO;
    } else if (inner.equals(Constant.ONE) || index.equals(BigInteger.ZERO)) {
        return Constant.ONE;
    } else {
        return new PowerFunction(inner.simplify(), index);
    }
}
// ...
```

**无需考虑其他函数的化简方案, 只需了解幂函数自身的化简方案即可完成该简化方法的编写.**

下面讲最后两个函数对象的基础化简:
1. 乘法函数
2. 加法函数

乘法函数内置了 `ArrayList<Function> functions` 字段, 逻辑意义为将这些函数乘起来. 化简时首先检查是否存在 `0` , 其次是消去 `1` . 重要的是, 做如上检查前必须对 `functions` 中每一个函数都化简再检查.

```java
// ... inside Multiplier
@Override
public Function simplify() {
    ArrayList<Function> simplifiedFunctions = new ArrayList<>(functions.size());
    for (Function f : functions) {
        Function sim = f.simplify();
        if (sim.equals(Constant.ZERO)) {
            return Constant.ZERO;
        } else if (!sim.equals(Constant.ONE)) {
            simplifiedFunctions.add(sim);
        }
    }
    if (simplifiedFunctions.isEmpty()) {
        return Constant.ONE;
    } else {
        return new Multiplier(simplifiedFunctions);
    }
}
// ...
```

加法函数的化简更简单, 消去 `0` 即可, 此处不再赘述.

### "拆包装"

拆包装主要是针对嵌套因子的, 例如: `1+(x+2)` , `1+(2*x)` 显然可以去括号. 关于这样的操作, **笔者称之为"拆包装"**.

#### 构造时"拆包装"

这一点可以通过安排合理的构造函数与对外接口直接实现. 下面以 `Multiplier` 为例解释实现过程.

```java
public class Multiplier extends Function {
    private final ArrayList<Function> functions = new ArrayList<>();

    public Multiplier(Function... funs) {
        this(Arrays.asList(funs));
    }

    public Multiplier(List<Function> funList) {
        funList.forEach(this::mult);
    }

    protected void mult(Function fun) {
        if (fun instanceof Multiplier) {
            mult((Multiplier) fun);
        } else {
            functions.add(fun);
        }
    }

    protected void mult(Multiplier multi) {
        functions.addAll(multi.functions);
    }

    // ...
}
```

当调用构造函数时, 就会自动检测参数类型, 若为 `Multiplier` 就直接"拆包装"合并 `functions` 列表中. `Adder` 的实现过程类似, 不再赘述.

#### 化简时"拆包装"

有时候是在化简过程中发现可以"拆包装", 例如: `x*(0+x**2)` 化简过程中可以去掉括号里的 `0` , 剩下 `x*(x**2)` 可以拆包装. 这一点可在基本化简的过程中实现. 下面对基本化简中的 `Multiplier` 的 `simplify` 方法进行补充:

```java
// ... inside Multiplier
@Override
public Function simplify() {
    // ... simFunctions = new List
    for (Function f : functions) {
        Function sim = f.simplify();
        if (sim instanceof Multiplier) {
            functions.addAll(((Multiplier) sim).functions); // Unpack
        } else {
            // ... other
        }
    }
    // ...
}
```

### 合并

**合并的核心问题是如何识别同类 `Function`**, 笔者在探索中重写 `Function` 类的 `equals` 和 `hashcode` 方法, 使得同类项在 `HashMap` 中位于同一位置, 实现同类项的识别与合并.

重写 `equals` 和 `hashcode` 的原则:
* 两个 `Function` 数学意义上相等, **当且仅当** `equals` 返回 `true` (**充要条件**)
* 两个 `Function` 数学意义上相等, 则两者具有相等的 `hashcode` 返回值 (**充分条件**); 两个 `Function` 数学意义上不相等, 则尽可能保证两者具有不同的 `hashcode` 返回值.

#### `Multiplier` 中的合并

只需遍历 `Multiplier` 的 `functions` 中每一个 `Function`:
* 常数因子在循环外专门记录. 一检测到常数因子, 就更新外部记录值
* 幂函数与幂函数嵌套三角函数, 使用 `HashMap<Function, BigInteger>` 存储**底数**与**指数**的对应关系. 一检测到幂函数(*幂函数嵌套三角函数, 也是幂函数对象*), 就将底数插入 `HashMap` 中, 在指数上作加法:
  ```java
  PowerFunction pow = (PowerFunction) f;
  map.put(
      pow.getInner(),
      map.getOrDefault(pow.getInner(), BigInteger.ZERO).add(pow.getIndex())
  );
  ```
* 嵌套的函数(如`Adder`)不支持幂运算, 应当单独记录.

遍历完成后, 再通过 `HashMap` 重建 `Multiplier` 即可完成指数合并.

#### `Adder` 中的合并

合并 `Adder` 前, 先把其中的 `Multiplier` 合并. 随后, 提取所有 `Multiplier` 的常系数, 形成 `Pair<Multiplier, Constant>`. 紧接着构建 `HashMap<Function, Constant>` 存储**项**与**系数**的对应关系. 将 `pair` 的第一元插入 `map` 中, 在系数上作加法.

遍历完成后, 再通过 `HashMap` 重建 `Adder` 即可完成系数合并.

## 随机数据生成与自动化测试

### 随机数据生成

可以使用类似递归下降的识别方式进行生成. 还是以如下文法为例:

$$
\begin{aligned}
    \text{E}&\rightarrow\text{T \textbar T '+' E}\\
    \text{T}&\rightarrow\text{F \textbar F '*' T}\\
    \text{F}&\rightarrow\text{num\textbar '(' E ')'}
\end{aligned}
$$

其中的符号解释如下:
* $\text{A}\mid\text{B}$ 表示选择 $\text{A}$ 和 $\text{B}$ 两者之一, 优先级最低
* $\text{'S'}$ 表示字符 $\text{S}$ 本身
* $\text{num}$ 是 `int` 范围内允许前导零的十进制正整数

笔者采用了 `Python` 代码来生成:

```python
max_depth = 4           # avoid stack overflow


def rand_E(depth=0):
    res = ''
    res += rand_T()
    if depth < max_depth and random.random() > 0.5:
        res += '+'
        res += rand_E(depth + 1)
    return res

def rand_T(depth=0):
    res = ''
    res += rand_F()
    if depth < max_depth and random.random() > 0.5:
        res += '*'
        res += rand_T(depth + 1)
    return res

def rand_F():
    res = ''
    if random.random() > 0.5:
        res += random.randint(1, 1 << 32)
    else:
        res += '(' + rand_E() + ')'
    return res
```

上述代码中**省略了用于避免嵌套过深的代码**, 读者若要使用上述代码, 请自行补充.

### 自动化测试

自动化测试是基于 `python` 第三方库 `SymPy` 完成的. 其基本流程如下:

![]({{ '/img/OO-DE-AutoTest-Process.svg' | prepend: site.baseurl}})

> 可以使用 `python` 的 `subprocess` 库完成运行 `java` 程序.

此外, `SymPy` 的 `equals` 方法解析多层的嵌套函数十分缓慢, 应使用**线性随机取点**的方法判定两函数相等与否.