---
title: 《高性能 MySQL》学习笔记 第四章
date: 2020-02-16 17:34:36
tags: 《高性能 MySQL》学习笔记
---
# 第四章 Schema 与数据类型优化

## 临时表

MySQL 临时表用于保存一些临时数据。临时表只在当前连接可见，当关闭连接时，MySQL 会自动删除表并释放所有空间。

* 外部临时表

  通过 `CREATE TEMORPARY TABLE` 创建的临时表称为外部临时表。这种临时表的命名可与非临时表相同。同名后，非临时表对当前回话不可见，直到临时表被删除。

* 内部临时表

  内部临时表是一种特殊的轻量级临时表，由 MySQL 自动创建，用来存储某些操作的中间结果，达到性能优化的目的。该表一般是 Memory 表。如果中间结果太大超出了 Memory 的限制，或者含有 BLOB 或 TEXT 字段，则临时表会转换为 MyISAM 表。

## 对象关系映射 Object Relation Mapping 简称 OMP

面向对象的开发方法是当今企业级应用开发环境中的主流方法，关系数据库是企业级应用环境中永久存放数据的主流数据存储系统。对象和关系数据是业务实体的两种表现方式。业务实体在内存中表现为对象，在数据库中表现为关系数据。内存中的对象之间存在关联和继承关系，而在数据库中，关系数据无法直接表达多对多的关联和继承关系。因此，对象-关系映射系统一般以中间件的形式存在，主要实现程序对象到关系数据库数据的映射。

## 实体-属性-值模型 Entity-attribute-value model EAV

> [Entity-attribute-value mode-wiki]([https://en.wikipedia.org/wiki/Entity%E2%80%93attribute%E2%80%93value_model](https://en.wikipedia.org/wiki/Entity–attribute–value_model))

EAV 模型是一种数据模型。主要用于以较高的空间利用率编码一种特殊实体，该实体在整体上包含很多属性，但是具体到某一个个体时，只有有限个的属性有值，其他的属性都为空。这种实体与数学上的稀疏矩阵很相似。

EAV 表通常是瘦长的。瘦表示表的列很少，长表示表的行很多。

EAV 表通常有三列

 - entity 表示被描述的实体，该列一般是关联到实体定义表的外键。
 - attribute 该列一般是关联到一个属性定义表的外键。该属性定义表用来定义该属性，表格可能包含如下几种列：属性ID、属性名、属性描述、数据类型和用于辅助输入验证的列，比如，最大字符串长度、正则表达式或这一个包含有效数据的集合。
 - value 该列存放属性的值

使用 EAV 模型的两个例子：医院的病历记录，超市的销售数据。

EAV 使用了行模型。行模型表示一个实体被记录在很多行而不是很多列中，每行通常有三列：entity、attribute 和 value。

## 范式

> [常见范式设计](https://www.zhihu.com/question/24696366)

* 依赖：若在一张表中，在属性（或属性组）X 的值确定的情况下，必定能确定属性 Y 的值，那么就可以说 Y 函数依赖于 X，记作 $X \to Y$。
* 完全函数依赖：在一张表中，若 $X \to Y$，且对于 X 的任何真子集 $X'$，$X' \to Y$ 不成立，那么我们称 Y 完全依赖于 X，记作 $X\stackrel{F}{\to}Y$。
* 部分函数依赖：Y 函数依赖于 X，同时 Y 不完全依赖于 X，则称 Y 部分依赖于 X，记作 $X\stackrel{P}{\to}Y$。
* 传递函数依赖：Z 函数依赖于 Y，且 Y 函数依赖于 X，且 Y 不包含于 X，且 X 不函数依赖于 Y，则称 Z 传递依赖于 X，记作 $X\stackrel{T}{\to}Z$。
* 码：设 K 为某表中的一个属性或属性组，若除 K 之外的所有属性都有完全函数依赖于 K，那么我们称 K 为候选码，简称为码。通常可以这样理解：假如 K 确定的情况下，该表除 K 之外的所有属性的值也就确定，那么 K 就是码。一张表中可以有超过一个码，通常选择其中的一个码作为主码。
* 主属性：包含在任何一个码中的属性为主属性
* 判断表格符合那一个范式步骤
  1. 找出表中的所有码
  2. 根据第一步的码，找出所有主属性
  3. 除去所有的主属性，剩下的都是非主属性
  4. 查看函数依赖

1. 第一范式

   所有属性都是不可分割的原子值。

   数据库表的每一列都是不可分割的原子数据项，不能是集合，数组，记录等非原子数据项。

2. 第二范式

   在第一范式的基础上，要求非主属性都要==完全==依赖于码。

3. 第三范式

   任何非主属性不依赖于其他非主键属性。

   第三范式是在第二范式的基础上建立起来的，消除了非主属性对于码的传递函数依赖。

4. BC 范式

   在 3NF 的基础上消除主属性对于码的部分与传递函数依赖。

## 选择数据类型的原则

1. 更小的通常更好

    更小的数据类型通常占用更少的存储空间，处理时需要的 CPU 周期更少。

   同时要确保没有低估需要存储的值的范围。在 schema 中的多个地方增加数据类型是一个代价很高的操作。

2. 简单就好

   简单的数据类型操作通常需要更少的 CPU 周期。

   使用 MySQL 内建的数据类型存储日期和时间，而不是使用字符串。

   使用整形存储 IP 地址。

3. 尽量避免 NULL

   可为 NULL 是列的默认属性。

   通常情况下最好指定 NOT NULL，除非真的需要存储 NULL 值。

   包含 NULL 的列，对 MySQL 来说更难优化。

   可为 NULL 的列使得索引、索引统计和值比较都更加复杂。

   但是，通常把可为 NULL 的列改为 NOT NULL 带来的性能提升比较小。所以，在调优时，没有必要首先修改这种情况。但是，在设计表格时要尽量避免 NULL。

4. 第一步，确定适合的大类型：数字、字符串、时间等。然后确定具体类型。MySQL 为了兼容性支持很多基本数据类型的别名，例如 INTEGER、BOOL 以及 NUMERIC。这些别名不会影响性能。

## 整数类型

|   类型    | 位数  |
| :-------: | :---: |
|  TINYINT  | 8 位  |
| SMALLINT  | 16 位 |
| MEDIUMINT | 24 位 |
|    INT    | 32 位 |
|  BIGINT   | 64 位 |



可存储的值的范围：$-2^{(N-1)}\thicksim2^{(N-1)}-1$，N 为位数。

有可选的 UNSIGNED 属性，表示无符号整数。可存储的值的范围：$0 \thicksim 2^N-1$，N   为位数。

数据类型决定整数是如何存储的。而整数计算一般使用 64 位的 BIGINT 整数，即使在 32 位环境中也是如此。一些聚合函数是例外，它们使用 DECIMAL 或 DOUBLE 进行计算。

可以为整数类型指定宽度，例如 INT(11)。这对大多数应用没有意义，这不会改变值的合法范围，只是规定了 MySQL 的一些交互工具用来显示字符的个数。对于存储和计算来讲 INT(1) 与 INT(20) 没有区别。

## 实数类型

|  类型   |  位数  |
| :-----: | :----: |
|  FLOAT  | 32 位  |
| DOUBLE  | 64 位  |
| DECIMAL | 128 位 |

DECIMAL(a,b)

参数说明：a 指定小数点左边和右边可以存储的十进制数字的最大个数。b 指定小数点右边可以存储的十进制数字的最大个数。

DECIMAL 只是一种存储格式，在实际计算中，DECIMAL 会转换为 DOUBLE。

CUP 不支持 DECIMAL 的直接运算，DECIMAL 的运算要在服务器层实现。相对而言，CUP 原生支持的浮点数运算更快。

MySQL 使用 DOUBLE 作为内部浮点计算的类型。

推荐只指定数据类型，不指定精度。

尽量只在对小数进行精确计算时才使用 DECIMAL，例如财务数据。

在数据量比较大时，可以使用 BIGINT 代替 DECIMAL，只需要将 DECIMAL 乘以合适的倍数将其转换为整数即可。这样可以避免浮点数计算精度问题和 DECIMAL 计算代价大的问题。

## 字符串类型

MySQL 4.1 开始，每个字符串列可以自定义自己的字符集和排序规则，这些东西会很大程度上影响性能。

### VARCHAR 和 CHAR 类型

 这两个数据类型在磁盘和内存中的存储方式与存储引擎的具体实现有关。以下以 InnoDB 和 MyISAM 在磁盘上的存储为例。

* VARCHAR

  用于存储可变长的字符串。

  比定长类型节省空间，因为它仅使用必要的空间。

  例外：如果表使用 ROW_FORMAT = FIXED 创建，每一行都会使用定长储存，这会很浪费存储空间。

  VARCHAR 需要使用额外的字节记录字符串的长度，如果列的最大长度小于等于 255 字节，则使用 1 个字节记录，否则使用 2 字节。

  由于行是变长的，在 UPDATE 时可能使行变得比原来长，这会导致额外的工作。如果一个行占用的空间增长，并且在页内没有更多的空间存储，在这种情况下，不同的存储引擎的处理方式不同。InnoDB 会分裂页来使行可以放入页内，MyISAM 会将行拆分成不同的片段存储。

  适用 VARCHAR 的情况：

  1. 字符串列的最大长度比平均长度大很多
  2. 列的更新很少
  3. 使用了像 UTF-8 这样的复杂字符集，每个字符都使用不同的字节进行存储。

  MySQL 5.0 及以上在存储和检索时会保留末尾空格。

  InnoDB 会把过长的 VARCHAR 存储为 BLOB。

* CHAR

  CHAR 是定长的。MySQL 总是根据定义的字符串长度为 CHAR 分配足够的空间。

  存储时，MySQL 会删除所有的末尾空格。

  CHAR 值会根据需要使用空格进行填充以便进行比较。

  适用 CHAR 的情况：

  1. 很短的字符串。
  2. 所有值都接近同一长度的字符串。比如 MD5 值。
  3. 对于经常变更的数据，CHAR 比 VARCHAR 更有效率，因为不容易产生碎片。

填充和截取空格的行为在不同的存储引擎中都是一样的，因为这是在服务器层处理的。

### BINARY 和 VARBINARY

与 CHAR 和 VARCHAR 很像。

这两个数据类型存储的是二进制的字符串，即存储的是字节码而不是字符。

填充时使用 \0 而不是空格。

二进制比较比字符比较简单很多，也就快很多。

### 慷慨是不明智的 最好的策略是只分配真正需要的空间

### BLOB 和 TEXT 类型

用于存储很大的数据。

BLOB 存储二进制数据，没有字符集和排序规则。

TEXT 存储字符数据，有字符集和排序规则。

|    字符类型     |   二进制类型    |
| :-------------: | :-------------: |
|    TINYTEXT     |    TINYBLOB     |
| SMALLTEXT(TEXT) | SMALLBLOB(BLOB) |
|   MEDIUMTEXT    |   MEDIUMBLOB    |
|    LONGTEXT     |    LONGBLOB     |

InnoDB 在 BLOB 或 TEXT 太大时，使用专用的外部存储区域存储，在行内只存储一个指向外部存储区域的指针。

只对每列最前的 max_sort_lenght 字节做排序而不是对整个字符串。

不能将 BLOB 和 TEXT 全部长度的字符串进行索引，也不能使用这些索引消除排序。

### 使用枚举类型代替字符串类型






