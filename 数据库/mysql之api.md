## 第1章：一般信息

**目录**

- [1.1.  关于本手册](introduction.html#manual-info)

- [1.2.  本手册采用的惯例](introduction.html#manual-conventions)

- [1.3. MySQL  AB概述](introduction.html#what-is-mysql-ab)

- [1.4.  MySQL数据库管理系统概述](introduction.html#what-is)

  [1.4.1.  MySQL的历史](introduction.html#history) [1.4.2.  MySQL的的主要特性](introduction.html#features) [1.4.3.  MySQL稳定性](introduction.html#stability) [1.4.4.  MySQL表最大能达到多少](introduction.html#table-size) [1.4.5.  2000年兼容性](introduction.html#year-2000-compliance)

- [1.5.  MaxDB数据库管理系统概述](introduction.html#maxdb)

  [1.5.1.  什么是MaxDB？](introduction.html#maxdb-overview) [1.5.2.  MaxDB的历史](introduction.html#maxdb-history) [1.5.3.  MaxDB的特性](introduction.html#maxdb-features) [1.5.4.  许可和支持](introduction.html#maxdb-licensing) [1.5.5.  MaxDB和MySQL之间的特性差异](introduction.html#maxdb-mysql-differences) [1.5.6.  MaxDB和MySQL之间的协同性](introduction.html#maxdb-mysql-interoperability) [1.5.7.  与MaxDB有关的链接](introduction.html#maxdb-links)

- [1.6.  MySQL发展大事记](introduction.html#roadmap)

  [1.6.1.  MySQL 5.1的新特性](introduction.html#mysql-5-1-nutshell)

- [1.7. MySQL信息源](introduction.html#mysql-information-sources)

  [1.7.1.  MySQL邮件列表](introduction.html#questions) [1.7.2.  IRC（在线聊天系统）上的MySQL社区支持](introduction.html#irc) [1.7.3.  MySQL论坛上的MySQL社区支持](introduction.html#forums)

- [1.8.  MySQL标准的兼容性](introduction.html#compatibility)

  [1.8.1.  MySQL遵从的标准是什么](introduction.html#standards) [1.8.2.  选择SQL模式](introduction.html#sql-mode) [1.8.3.  在ANSI模式下运行MySQL](introduction.html#ansi-mode) [1.8.4.  MySQL对标准SQL的扩展](introduction.html#extensions-to-ansi) [1.8.5.  MySQL与标准SQL的差别](introduction.html#differences-from-ansi) [1.8.6.  MySQL处理约束的方式](introduction.html#constraints)



MySQL®软件提供了十分快速的多线程、多用户、牢靠的SQL（结构化查询语言）数据库服务器。  MySQL服务器定位于任务关键型、重负荷生产系统，并能嵌入在大量部署的软件中。MySQL是MySQL AB的注册商标。

MySQL软件采用双许可方式。用户可根据GNU通用公共许可（http://www.fsf.org/licenses/）条款，将MySQL软件作为开放源码产品使用，或从MySQL  AB公司购买标准的商业许可证。关于我方许可策略的更多信息，请参见http://www.mysql.com/company/legal/licensing/。

在下面的清单中，介绍了本手册感兴趣的一些特殊部分。

- 关于MySQL数据库服务器的讨论，请参见[1.4.2 “MySQL的的主要特性”](introduction.html#features)。
- 关于安装说明，请参见[第2章：安装MySQL](installing.html)。 
- 关于将MySQL数据库软件移植到新体系结构或操作系统方面的技巧，请参见[附录](porting.html)[E：*移植到其他系统*](porting.html)。
- 关于从5.0版向上升级的更多信息，请参见[2.10.1节，“从5.0版升级”](installing.html#upgrading-from-5-0)。
- 关于MySQL数据库服务器的教程说明，请参见[第3章：教程](tutorial.html)。 
- 关于SQL示例和标准程序信息，请参见标准程序目录（在本发布版中，为sql-bench）。
- 关于新特性和缺陷更正的历史信息，请参见[附录](news.html)[D：*MySQL变更史*](news.html)。
- 关于当前已知缺陷和错误特性的列表，请参见[A.8 “MySQL中的已知事宜”](problems.html#bugs)。
- 关于未来计划，请参见[1.6 “MySQL发展大事记”](introduction.html#roadmap)。
- 关于本项目所有贡献人的清单，请参见 [附录C：感谢](credits.html)。

**重要说明：** 

请将错误报告（通常称为缺陷）以及问题和评论发送到http://bugs.mysql.com。请参见[1.7.1.3 “如何通报缺陷和问题”](introduction.html#bug-reports)。

如果在MySQL服务器中发现敏感的安全缺陷，请使用电子邮件立刻通知我们：[security@mysql.com](mailto:security@mysql.com)。

## 1.1. 关于本手册



这是关于5.1版至5.1.2-alpha版MySQL数据库系统的参考手册。该手册不适用于旧版本MySQL软件，这是因为在MySQL  5.1和以前的版本存在很多功能性差异和其他差异。如果正在使用MySQL软件的较旧版本，请参阅*MySQL  5.0参考手册*，该手册涵盖了MySQL 5.0，或参阅*MySQL  4.1参考手册*，该手册涵盖了MySQL  3.22、3.23、4.0和4.1系列。在手册的文本中，通过引用发布版本号（5.1.x），注明了MySQL 5.1的二级版本。

由于本手册是作为参考手册而编制的，在本手册中未提供关于SQL或关联数据库概念的一般说明。在本手册中，也不包含如何使用操作系统或命令行解释器方面的信息。

MySQL数据库软件始终在发展，参考手册也会相应地频繁更新。本手册的最新版本以在线方式提供，请使用http://dev.mysql.com/doc/上的搜索表单。也提供多重其他格式，包括HTML、PDF、和Windows  CHM版本。

主要文档是[DocBook](http://docbook.org/)  XML文件的集合。对于HTML版本和其他格式，它们是使用[DocBook XSL stylesheets](http://docbook.sourceforge.net/release/xsl/current/doc/reference.html)自动生成的。

如果你有任何关于本手册应增加内容或更正内容方面的建议，请将其发送给文档编制团队：[docs@mysql.com](mailto:docs@mysql.com)。

本手册最初是由David  Axmark和Michael “Monty”  Widenius编制的。由MySQL文档编制团队负责维护，团队成员包括Paul DuBois、Stefan Hinz、Mike Hillyer和Jon  Stephens。关于中多其他贡献人，请参见[附录C：感谢](credits.html)。

本手册的版权归瑞典公司MySQL AB所有。MySQL®和MySQL徽标均是MySQL  AB的注册商标。本手册中引用的其他商标和注册商标是相应所有人的财产，在本手册中仅将其用于辨识目的。

## 1.2. 本手册采用的惯例

本手册采用了特定的印刷惯例： 

·      这类风格的文本用于SQL语句，数据库、表和列名称，C和Perl代码，以及环境变量。例如：  要想重新加载授权表，请使用FLUSH PRIVILEGES语句。

**这类风格的文本**用于指明键入的数如信息。

·      **这类风格的文本**用于指明可执行程序和脚本的名称，例如**mysql**（MySQL命令行客户端程序）和**mysqld**（MySQL服务器执行程序）。

·      *这类风格的文本*用于变量输入，应使用你选择的值替换它。

·     文件名和目录名采取下述方式：  “全程my.cnf位于目录/etc下”。

·     字符序列采取下述方式： “要想使用通配符，请使用字符%”。

·     *这类风格的文本*用于强调。

·      **这类风格的文本**用于表头，并用于传递强调信息。

当出现准备在特定程序中执行的命令时，该程序将由位于命令前的提示符指明。例如，shell>指明命令将从注册外壳程序中执行，mysql>指明命令将从**mysql客户端**程序中执行： 

```
shell> type a shell command here（在此输入shell命令）
mysql> type a mysql statement here（在此输入mysql语句）
```

“shell”是命令解释程序。在Unix平台上，它通常是程序，如**sh**、**csh**或**bash**。在Windows平台下，等效程序为**command.com**或**cmd.exe**，通常运行在控制台窗口中。

输入示例中显示的命令或语句时，不要输入示例中给出的提示符。

数据库、表和；列名必须代入语句中。为了指明该代入是必要的，在本手册中使用了*db_name*、*tbl_name*和*col_name。*例如，你将见到如下所示的语句： 

```
mysql> SELECT col_name FROM db_name.tbl_name;
```

这意味着，如果你输入了类似的语句，应提供你的数据库、表和列名，如下例所示： 

```
mysql> SELECT author_name FROM biblio_db.author_list;
```

SQL关键字不区分大小写，因此即可为大写也可为小写。在本手册中采用大写。

在语法介绍中，方括号（“[”和“]”）用于指明可选字或子句。例如，在下面的语句中，IF EXISTS是可选的： 

```
DROP TABLE [IF EXISTS] tbl_name
```

当某一语法成分由多个可选项组成时，可选项应用竖线“|”分开。当可能选择一组选择中的某一成员时，可选项将列在方括号（“[”和“]”）中。

```
TRIM([[BOTH | LEADING | TRAILING] [remstr] FROM] str)
```

当必须选择一组选择中的某一成员时，可选项将列在大括号（“{”和“}”）中。

```
{DESCRIBE | DESC} tbl_name [col_name | wild]
```

省略号（…）表明省略了语句的某一选择，通常是为了提供复杂语法的简短表述。例如，INSERT ...  SELECT是后跟SLECT语句的INSERT语句的简短形式。

省略号还能指明语句的前部分语法元素可重复。在下面的示例中，可给定多个*reset_option*值，第1个值后每一个可由逗号分开： 

```
RESET reset_option [,reset_option] ...
```

对于用来设置shell变量的命令，采用Bourne shell语法给出。例如，用于设置环境变量的序列和运行命令的序列，与下述Bourne  shell语法给出的类似： 

```
shell> VARNAME=value some_command
```

如果你正在使用**csh**或**tcsh**，必须使用略有不同的命令。应执行与下例所示类似的序列： 

```
shell> setenv VARNAME value
shell> some_command
```

## 1.3. MySQL  AB概述



MySQL AB是由MySQL创始人和主要开发人创办的公司。MySQL AB最初是由David Axmark、Allan  Larsson和Michael“Monty”Widenius在瑞典创办的。

我们致力于开发MySQL数据库软件，并向新用户宣传推广它。MySQL  AB拥有MySQL源代码、MySQL徽标和（注册）商标、以及本手册的版权。请参见

[1.4 “MySQL数据库管理系统概述”](introduction.html#what-is)。

MySQL的核心价值取向指明了我们对MySQL和开发源码的贡献。

这些核心价值取向规定了MySQL AB与MySQL服务器软件的协作方式： 

·     成为世界上最好和使用最广泛的数据库。

·     面向所有人，而且所有人均能支付得起。

·     使用简单。

·     在保持快速和安全的同时不断改进。

·     使用和改进充满乐趣。

·     不存在缺陷。

这就是MySQL AB公司及其雇员的核心价值取向。

·     我们同意开放源码理念，并支持开放源码群。

·     我们的目标是成为最佳公民。

·     我们倾向于那些与我们有共同价值取向和思想倾向的合作伙伴。

·     我们将回复电子邮件并提供支持。

·     我们是一家与其他方联系在一起的“虚拟”公司。

·     我们反对软件专利。

在MySQL的网站（http://www.mysql.com/）上，给出了关于MySQL和MySQL的最新信息。

顺便提及一下，公司名中的“AB”是瑞典语“aktiebolag”或“股份公司”的首字母缩写。可将其翻译为“MySQL有限公司”。事实上，MySQL有限公司和MySQLGmbH均是MySQL  AB子公司的名称。它们分别位于美国和德国。

## 1.4. MySQL数据库管理系统概述

- [1.4.1.  MySQL的历史](introduction.html#history)
- [1.4.2.  MySQL的的主要特性](introduction.html#features)
- [1.4.3.  MySQL稳定性](introduction.html#stability)
- [1.4.4.  MySQL表最大能达到多少](introduction.html#table-size)
- [1.4.5.  2000年兼容性](introduction.html#year-2000-compliance)



MySQL是最流行的开放源码SQL数据库管理系统，它是由MySQL AB公司开发、发布并支持的。MySQL  AB是由多名MySQL开发人创办的一家商业公司。它是一家第二代开放源码公司，结合了开放源码价值取向、方法和成功的商业模型。

在MySQL的网站（http://www.mysql.com/）上，给出了关于MySQL和MySQL的最新信息。

·     MySQL是一种数据库管理系统。

数据库是数据的结构化集合。它可以是任何东西，从简单的购物清单到画展，或企业网络中的海量信息。要想将数据添加到数据库，或访问、处理计算机数据库中保存的数据，需要使用数据库管理系统，如MySQL服务器。计算机是处理大量数据的理想工具，因此，数据库管理系统在计算方面扮演着关键的中心角色，或是作为独立的实用工具，或是作为其他应用程序的组成部分。

- MySQL是一种关联数据库管理系统。

  关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大的仓库内。这样就增加了速度并提高了灵活性。MySQL的SQL指得是“结构化查询语言”。SQL是用于访问数据库的最常用标准化语言，它是由ANSI/ISO  SQL标准定义的。SQL标准自1986年以来不断演化发展，有数种版本。在本手册中，“SQL-92”指得是1992年发布的标准，“SQL:1999”指得是1999年发布的标准，“SQL:2003”指得是标准的当前版本。我们采用术语“SQL标准”标示SQL标准的当前版本。

- MySQL软件是一种开放源码软件。

  “开放源码”意味着任何人都能使用和改变软件。任何人都能从Internet下载MySQL软件，而无需支付任何费用。如果愿意，你可以研究源码并进行恰当的更改，以满足你自己的需求。MySQL软件采用了GPL（GNU通用公共许可证），http://www.fsf.org/licenses/，定义了在不同情况下可以用软件作的事和不可作的事。如果你对GPL不满意，或需要在商业应用程序中嵌入MySQL代码，可从我方购买商业许可版本。更多信息，请参见MySQL许可概述（http://www.mysql.com/company/legal/licensing/）。 

- MySQL数据库服务器具有快速、可靠和易于使用的特点。

  如果它正是你所寻找的，不妨一试。MySQL服务器还有一套实用的特性集合，这些特性是通过与我们用户的密切合作而开发的。在我们的基准测试主页上，给出了MySQL服务器和其他数据库管理器的比较结果。请参见[7.1.4 “MySQL基准套件”](optimization.html#mysql-benchmarks)。

  MySQL服务器最初是为处理大型数据库而开发的，与已有的解决方案相比，它的速度更快，多年以来，它已成功用于众多要求很高的生产环境。尽管MySQL始终在不断发展，但目前MySQL服务器已能提供丰富和有用的功能。它具有良好的连通性、速度和安全性，这使的MySQL十分适合于访问Internet上的数据库。 

- MySQL服务器工作在客户端/服务器模式下，或嵌入式系统中。 

  MySQL数据库软件是一种客户端/服务器系统，由支持不同后端的1个多线程SQL服务器，数种不同的客户端程序和库，众多管理工具和广泛的应用编程接口API组成。

  我们还能以嵌入式多线程库的形式提供MySQL服务器，你可以将其链接到你的应用程序，从而获得更小、更快、和更易管理的产品。

- 有大量可用的共享MySQL软件。

  你所喜欢的应用程序和语言均支持MySQL数据库服务器，这种情况十分可能。 



“MySQL”的正式发音是“My Ess Que  Ell”（而不是“my sequel”）,但我们并不介意你的发音方式是“my sequel”或其他当地方式。 

### 1.4.1. MySQL的历史



我们最初的出发点是，使用mSQL来连接我们的表，这类表采用了我们的快速低层面（ISAM）子程序。然而，经过一些测试后，我们得出结论，mSQL的速度或灵活性不足以满足我们的要求。其结果是，为我们的数据库提供了新的SQL接口，但API接口与mSQL的几乎一样。设计该API的目的在于，允许将为mSQL编写的第三方代码方便地移植到MySQL。

MySQL名称的起源不明。10多年来，我们的基本目录以及大量库和工具均采用了前缀“my”。不过，共同创办人Monty  Widenius的女儿名字也叫“My”。时至今日，MySQL名称的起源仍是一个迷，即使对我们也一样。

MySQL  Dolphin（我方徽标）的名称为“Sakila”，它是由MySQL AB公司的创办人从用户在“Dolphin命名”比赛中提供的众多建议中选定的。该名称是由来自非洲斯威士兰的开放源码软件开发人Ambrose  Twebaze提出的。根据Ambrose的说法，按斯威士兰的本地语言，女性化名称Sakila源自SiSwati。Sakila也是坦桑尼亚、Arusha地区的一个镇的镇名，靠近Ambrose的母国乌干达。

### 1.4.2. MySQL的的主要特性



下面介绍了MySQL数据库软件的一些重要特性。关于当前特性和即将提供特性的更多信息，，请参见[1.6节，“MySQL发展大事记”](introduction.html#roadmap) 。 

·     内部构件和可移植性

o    使用C和C++编写 

o    用众多不同的编译器进行了测试 

o    能够工作在众多不同的平台上。请参见[2.1.1 “MySQL支持的操作系统”](installing.html#which-os)。

o    使用GNU Automake、Autoconf和Libtool进行移植。

o     提供了用于C、C++、Eiffel、Java、Perl、PHP、Python、Ruby和Tcl的API。请参见[第25章：API和库](apis.html)。

o    采用核心线程的完全多线程 如果有多个CPU，它能方便地使用这些CPU。

o    提供了事务性和非事务性存储引擎。

o     使用了极快的“B树”磁盘表（MyISAM）和索引压缩。

o     添加另一个存储引擎相对简单。如果打算为内部数据库添加一个SQL接口，该特性十分有用。

o    极快的基于线程的内存分配系统。

o    通过使用优化的“单扫描多连接”，能实现极快的连接。

o    存储器中的哈希表用作临时表。

o     SQL函数是使用高度优化的类库实现的，运行很快。通常，在完成查询初始化后，不存在存储器分配。

o    采用Purify（商业内存溢出检测器）以及GPL工具Valgrind（http://developer.kde.org/~sewardj/）测试了MySQL代码。

o     服务器可作为单独程序运行在客户端/服务器联网环境下。它也可作为库提供，可嵌入（链接）到独立的应用程序中。这类应用程序可单独使用，也能在网络环境下使用。

- 列类型 

  - 众多列类型：  带符号/无符号整数，1、2、3、4、8字节长，FLOAT，DOUBLE，CHAR，VARCHAR，TEXT，BLOB，DATE，TIME，DATETIME，TIMESTAMP，YEAR，SET，ENUM，以及OpenGIS空间类型。请参见[第11章：列类型](column-types.html)。 
  - 定长和可变长度记录。

- 语句和函数

  - 在SELECT和查询的WHERE子句中，提供完整的操作符和函数支持。例如：

    ```
    mysql> SELECT CONCAT(first_name, ' ', last_name)
        -> FROM citizen
        -> WHERE income/dependents > 10000 AND age > 30;
    ```

  - 对SQL GROUP BY和ORDER  BY子句的全面支持。支持聚合函数(COUNT(), COUNT(DISTINCT  ...)，AVG()，STD()，SUM()，MAX()，MIN()和GROUP_CONCAT())。

  - 支持LEFT OUTER JOIN和RIGHT OUTER  JOIN，采用标准的SQL和ODBC语法。

  - 按照标准SQL的要求，支持表别名和列别名。

  - DELETE、INSERT、REPLACE和UPDATE返回更改（影响）的行数。连接到服务器时，可通过设置标志返回匹配的行数。

  - MySQL的SHOW命令可用于检索关于数据库、数据库引擎、表和索引的信息。EXPLAIN命令可用于确定优化器处理查询的方式。

  - 函数名与表名或列名不冲突。例如，ABS是有效的列名。唯一的限制在于，调用函数时，函数名和随后的符号“(”之间不得有空格。请参见[9.6 “MySQL中保留字的处理”](language-structure.html#reserved-words)。 

  - 可以将不同数据库的表混合在相同的查询中（就像MySQL 3.22中那样）。

- 安全

  - 十分灵活和安全的权限和密码系统，允许基于主机的验证。连接到服务器时，所有的密码传输均采用加密形式，从而保证了密码安全。 

- 可伸缩性和限制 

  - 处理大型数据库：  我们使用了MySQL服务器和含5千万条记录的数据库。我们还听说，有些用户将MySQL用于含60000个表和约50亿行的数据库。 
  - 每个表可支持高达64条索引（在MySQL  4.1.2之前为32条）。每条索引可由1～16个列或列元素组成。最大索引宽度为1000字节（在MySQL  4.1.2之前为500）。索引可使用具备CHAR、VARCHAR、BLOB或TEXT列类型的列前缀。

- 连接性

  - 在任何平台上，客户端可使用TCP/IP协议连接到MySQL服务器。在Windows系统的NT系列中（NT、2000、XP或2003），客户端可使用命名管道进行连接。在Unix系统中，客户端可使用Unix域套接字文件建立连接。
  - 在MySQL  4.1和更高的版本中，如果是以“--shared-memory”选项开始，Windows服务器还支持共享内存连接。客户端可使用“--protocol=memory”选项，通过共享内存建立连接。
  - Connector/ODBC (MyODBC)接口为使用ODBC（开放式数据库连接性）连接的客户端程序提供了MySQL支持。例如，可以使用MS  Access连接到你的MySQL服务器。客户端可运行在Windows或Unix平台上。提供了MyODBC源。支持所有的ODBC  2.5函数，以及众多其他函数。请参见[第26章：连接器](connectors.html)。
  - Connector/J接口为使用JDBC连接的Java客户端程序提供了MySQL支持。客户端可运行在Windows或Unix平台上。提供了Connector/J源码。请参见[第26章：连接器](connectors.html)。 

- 本地化

  - 服务器可使用多种语言向客户端提供错误消息。请参见[5.10.2节，“设置错误消息语言”](database-administration.html#languages)。
  - 对数种不同字符集的全面支持，包括latin1  (cp1252)、german、big5、ujis等。例如，在表名和列名中允许使用斯堪的纳维亚字符‘å’、‘ä’和‘ö’。从MySQL  4.1开始，提供了Unicode支持。 
  - 所有数据均以所选的字符集保存。正常字符串列的比较不区分大小写。
  - 分类是根据所选的字符集（默认情况下，使用瑞典校对）进行的。启动MySQL服务器时，可更改该项设置。要想查看高级分类的示例，请参见Czech分类代码。MySQL服务器支持众多不同的字符集，这类字符集可在编译时和运行时指定。

- 客户端和工具

  - MySQL服务器提供了对SQL语句的内部支持，可用于检查、优化和修复表。通过**mysqlcheck**客户端，可在命令行上使用这类语句。MySQL还包括**myisamchk**，这是一种很快的命令行实用工具，可用于在MyISAM表上执行这类操作。请参见[第5章：数据库管理](database-administration.html)。 
  - 对于所有MySQL程序，均能通过“-help”或“-?”选项调用，以获取联机帮助信息。

### 1.4.3. MySQL稳定性



本节回答了如下问题：“MySQL服务器有多稳定？”，以及“在本项目中我能依靠MySQL服务器吗”？  我们将尝试阐明这类问题，并回答很多潜在用户关心的某些重要问题。本节所给出的信息基于通过邮件列表收集的数据，在确定问题和通报使用类型方面，邮件列表十分有用。

最初的代码可回溯至20世纪80年代初。它提供了稳定的编码基数，最初存储引擎使用的ISAM表格式仍保持向后兼容性。在MySQL  AB公司的前身TcX，自1996年中期以来，MySQL代码在多个项目中工作良好，未出现任何问题。当MySQL数据库软件首次向更广泛的公众发布时，我们的用户很快发现了一些未经测试的代码段。自那以后，尽管每个新版本具有很多新的特性，但每次新发布的版本均存在少量的移植性问题。

每次发布的MySQL服务器均是可用的。仅当用户尝试源自“灰色区域”的代码时才会出现问题。当然，新用户不了解“灰色区域”是什么。因此，在本节中，我们介绍了目前已知的这类区域。本节所作的介绍主要针对MySQL服务器3.23版和更高版本。在最新的版本中，更正了所有已知和通报的缺陷，但“缺陷”一节所列的除外，这类缺陷与设计有关。请参见[A.8节，“MySQL中的已知事宜”](problems.html#bugs)。

MySQL服务器采用了多层设计和独立模块。在此列出了一些较新的模块，并指明了它们的测试情况。

·     Replication（稳定）

大量使用复制功能的服务器均处于生产模式下，结果良好。在MySQL 5.x中，将继续增强复制功能。

·     InnoDB表（稳定）

自3.23.49版以来，InnoDB事务存储引擎一直很稳定。InnoDB正用于大型、重负荷生产系统。

·     BDB表（稳定）

Berkeley DB码十分稳定，但在MySQL服务器中，我们仍在改进BDB事务存储引擎。

·     全文本搜索（稳定）

全文本搜索的使用范围十分广泛。在MySQL 4.0和4.1中，增加了重要的特性增强。

·     MyODBC 3.51（稳定） 

MyODBC 3.51采用了ODBC SDK  3.51，并广泛用于生产活动中。某些出现的情况看上去与应用程序相关，与ODBC驱动程序或底层数据库服务器无关。

### 1.4.4. MySQL表最大能达到多少



MySQL 3.22限制的表大小为4GB。由于在MySQL  3.23中使用了MyISAM存储引擎，最大表尺寸增加到了65536TB（2567 –  1字节）。由于允许的表尺寸更大，MySQL数据库的最大有效表尺寸通常是由操作系统对文件大小的限制决定的，而不是由MySQL内部限制决定的。

InnoDB存储引擎将InnoDB表保存在一个表空间内，该表空间可由数个文件创建。这样，表的大小就能超过单独文件的最大容量。表空间可包括原始磁盘分区，从而使得很大的表成为可能。表空间的最大容量为64TB。

在下面的表格中，列出了一些关于操作系统文件大小限制的示例。这仅是初步指南，并不是最终的。要想了解最新信息，请参阅关于操作系统的文档。

| **操作系统**             | **文件大小限制**            |
| ------------------------ | --------------------------- |
| Linux 2.2-Intel 32-bit   | 2GB (LFS: 4GB)              |
| Linux 2.4+               | (using ext3 filesystem) 4TB |
| Solaris 9/10             | 16TB                        |
| NetWare w/NSS filesystem | 8TB                         |
| win32 w/ FAT/FAT32       | 2GB/4GB                     |
| win32 w/ NTFS            | 2TB（可能更大）             |
| MacOS X w/ HFS+          | 2TB                         |

在Linux  2.2平台下，通过使用对ext2文件系统的大文件支持（LFS）补丁，可以获得超过2GB的MyISAM表。在Linux  2.4平台下，存在针对ReiserFS的补丁，可支持大文件（高达2TB）。目前发布的大多数Linux版本均基于2.4内核，包含所有所需的LFS补丁。使用JFS和XFS，petabyte（千兆兆）和更大的文件也能在Linux上实现。然而，最大可用的文件容量仍取决于多项因素，其中之一就是用于存储MySQL表的文件系统。

关于Linux中LFS的详细介绍，请参见Andreas Jaeger的“Linux中的大文件支持”页面：http://www.suse.de/~aj/linux_lfs.html。

Windows用户请注意： FAT和VFAT (FAT32)不适合MySQL的生产使用。应使用NTFS。

在默认情况下，MySQL创建的MyISAM表允许的最大尺寸为4GB。你可以使用SHOW TABLE  STATUS语句或**myisamchk -dv**  ***tbl_name\***检查表的最大尺寸。请参见[13.5.4节，“SHOW语法”](sql-syntax.html#show)。

如果需要使用大于4GB的MyISAM表（而且你的操作系统支持大文件），可使用允许AVG_ROW_LENGTH和MAX_ROWS选项的CREATE  TABLE语句。请参见[13.1.5节，“CREATE  TABLE语法”](sql-syntax.html#create-table)。创建了表后，也可以使用ALTER TABLE更改这些选项，以增加表的最大允许容量。请参见[13.1.2节，“ALTER TABLE语法”](sql-syntax.html#alter-table)。

处理MyISAM表文件大小的其他方式：

·      如果你的大表是只读的，可使用**myisampack**压缩它。**myisampack**通常能将表压缩至少50％，因而，从结果上看，可获得更大的表。此外，**myisampack**还能将多个表合并为1个表。请参见[8.2节，“myisampack：生成压缩、只读MyISAM表”](client-side-scripts.html#myisampack)。

·      MySQL包含一个允许处理MyISAM表集合的MERGE库，这类MyISAM表具有与单个MERGE表相同的结构。请参见[15.3节，“MERGE存储引擎”](storage-engines.html#merge-storage-engine)。

### 1.4.5. 2000年兼容性



MySQL服务器本身不存在2000年（Y2K）兼容性问题： 

·      MySQL服务器采用了Unix的时间功能，对于TIMESTAMP值，可处理的日期至2037年。对于DATE和DATETIME值，可接受的日期可至9999年。

·      所有的MySQL日期函数均是在1个源文件sql/time.cc中实现的，并经过了恰当编码以确保2000年安全。

·     在MySQL  3.22和以后的版本中，YEAR列类型能够在1个字节内保存0年以及1901～2155年，并能使用两位或四位数字显示它们。所有的两位数字年份均被视为介于1970～2069年之间，这意味着，如果你在YEAR列中保存了01，MySQL服务器会将其当作2001年。

通过下面的简单演示示例，表明MySQL服务器在处理直至9999年的DATE或DATETIME值方面不存在问题，在处理2030年以前的TIMESTAMP值方面也不存在问题： 

```
mysql> DROP TABLE IF EXISTS y2k;
Query OK, 0 rows affected (0.01 sec)
 
mysql> CREATE TABLE y2k (date DATE,
    ->                   date_time DATETIME,
    ->                   time_stamp TIMESTAMP);
Query OK, 0 rows affected (0.01 sec)
 
mysql> INSERT INTO y2k VALUES
    -> ('1998-12-31','1998-12-31 23:59:59',19981231235959),
    -> ('1999-01-01','1999-01-01 00:00:00',19990101000000),
    -> ('1999-09-09','1999-09-09 23:59:59',19990909235959),
    -> ('2000-01-01','2000-01-01 00:00:00',20000101000000),
    -> ('2000-02-28','2000-02-28 00:00:00',20000228000000),
    -> ('2000-02-29','2000-02-29 00:00:00',20000229000000),
    -> ('2000-03-01','2000-03-01 00:00:00',20000301000000),
    -> ('2000-12-31','2000-12-31 23:59:59',20001231235959),
    -> ('2001-01-01','2001-01-01 00:00:00',20010101000000),
    -> ('2004-12-31','2004-12-31 23:59:59',20041231235959),
    -> ('2005-01-01','2005-01-01 00:00:00',20050101000000),
    -> ('2030-01-01','2030-01-01 00:00:00',20300101000000),
    -> ('2040-01-01','2040-01-01 00:00:00',20400101000000),
    -> ('9999-12-31','9999-12-31 23:59:59',99991231235959);
Query OK, 14 rows affected (0.01 sec)
Records: 14  Duplicates: 0  Warnings: 2
 
mysql> SELECT * FROM y2k;
+------------+---------------------+----------------+
| date       | date_time           | time_stamp     |
+------------+---------------------+----------------+
| 1998-12-31 | 1998-12-31 23:59:59 | 19981231235959 |
| 1999-01-01 | 1999-01-01 00:00:00 | 19990101000000 |
| 1999-09-09 | 1999-09-09 23:59:59 | 19990909235959 |
| 2000-01-01 | 2000-01-01 00:00:00 | 20000101000000 |
| 2000-02-28 | 2000-02-28 00:00:00 | 20000228000000 |
| 2000-02-29 | 2000-02-29 00:00:00 | 20000229000000 |
| 2000-03-01 | 2000-03-01 00:00:00 | 20000301000000 |
| 2000-12-31 | 2000-12-31 23:59:59 | 20001231235959 |
| 2001-01-01 | 2001-01-01 00:00:00 | 20010101000000 |
| 2004-12-31 | 2004-12-31 23:59:59 | 20041231235959 |
| 2005-01-01 | 2005-01-01 00:00:00 | 20050101000000 |
| 2030-01-01 | 2030-01-01 00:00:00 | 20300101000000 |
| 2040-01-01 | 2040-01-01 00:00:00 | 00000000000000 |
| 9999-12-31 | 9999-12-31 23:59:59 | 00000000000000 |
+------------+---------------------+----------------+
14 rows in set (0.00 sec)
```

最后2个TIMESTAMP列的值为0，这是因为年份值（2040，9999）超出了TIMESTAMP的最大范围。TIMESTAMP数据类型用于保存当前时间，在32位机器上，支持的取值范围是19700101000000～20300101000000（带符号值）。在64位机器上，TIMESTAMP能处理的值达2106（无符号值）。

尽管MySQL服务器本身不存在2000年安全问题，但如果使用了存在Y2K问题的应用程序，也会遇到问题。例如，很多早期的应用程序采用2位数值（两可性）而不是4位数值来保存和处理年份数据。这类问题可能会被使用“00”或“99”的应用程序合并为“丢失”值指示符。很不幸，这类问题或许很难更正，这是因为不同的应用程序是由不同的程序员编写的，每位程序员可能使用了不同的惯例集和日期处理函数。

因此，尽管MySQL服务器不存在Y2K问题，但应用程序须提供无歧义的输入值。关于MySQL服务器在处理含2位年份数值的两可性日期输入数据方面的作用，请参见[11.3.4节，“Y2K事宜和日期类型”](column-types.html#y2k-issues) 。

## 1.5. MaxDB数据库管理系统概述

- [1.5.1.  什么是MaxDB？](introduction.html#maxdb-overview)
- [1.5.2.  MaxDB的历史](introduction.html#maxdb-history)
- [1.5.3.  MaxDB的特性](introduction.html#maxdb-features)
- [1.5.4.  许可和支持](introduction.html#maxdb-licensing)
- [1.5.5.  MaxDB和MySQL之间的特性差异](introduction.html#maxdb-mysql-differences)
- [1.5.6.  MaxDB和MySQL之间的协同性](introduction.html#maxdb-mysql-interoperability)
- [1.5.7.  与MaxDB有关的链接](introduction.html#maxdb-links)

MaxDB是一种大型高效的企业数据库。数据库管理通过了SAP认证。

MaxDB是数据库管理系统的新名称，以前称为SAP DB。2003年，SAP AG和MySQL  AB确立了合作伙伴关系，并将数据库系统重命名为MaxDB。自此以后，MaxDB的开发一直由SAP开发者团队负责，就像以前那样。

MySQL  AB与MaxDB团队在SAP处保持着密切的合作，以不断改进MaxDB产品。两者的联合努力包括：开发新的固有驱动程序，以便能够在开发源码社区中更有效地使用MaxDB，并不断改善各种文档，以拓展MaxDB的用户基数。此外，MySQL和MaxDB数据库的协同性也被视为一项重要因素，例如，新的MaxDB同步管理器支持从MaxDB到MySQL的数据同步。

MaxDB数据库管理系统和MySQL数据库管理系统未共享公用编码基数。MaxDB和MySQL数据库管理系统是由MySQL  AB公司提供的独立产品。

MySQL AB为MaxDB提供了全面的专业服务组合。

### 1.5.1. 什么是MaxDB？

MaxDB是兼容ANSI SQL-92（入门级）、由SAP AG提供的关联数据库管理系统（RDBMS)，也可由MySQL  AB提供。MaxDB能够满足企业级应用的要求：  安全性，可伸缩性，高度并行性，以及强大的性能。它能运行在所有主要的操作系统下。多年的经历表明，它能运行，并能在24x7的运作中运行数以TB计的数据。

数据库开发是于1977年在柏林技术大学作为一个研究项目开始的。在20世纪80年代早期，它发展成为数据库产品，随后归Nixdorf、Siemens  Nixdorf、Software AG所有，目前归SAP AG所有。在这一发展历程中，它先后被命名为VDN、Reflex、Supra  2、DDB/4、Entire SQL-DB-Server和ADABAS D。1997年，SAP从软件AP手中接管了该软件，并将其重新命名为SAP  DB。自2000年10月起，依GNU通用公共许可的名义发布了众多的SAP DB源码（请参见[附录J：GNU通用公共许*可*](gpl-license.html)）。

2003年，SAP AG和MySQL  AB确立了合作伙伴关系，并将数据库系统重命名为MaxDB。

### 1.5.2. MaxDB的历史

MaxDB的历史可追溯至SAP DB、SAP AG的DBMS，也就是说，MaxDB是SAP  DB的重命名和增强版本。多年来，MaxDB已成功用于mySAP业务套件的小型、中性和大型安装实例，以及需要企业级DBMS的其他要求苛刻的SQL应用（涉及用户数、事务工作量、以及数据库的大小）。

除了第三方数据库系统外，如Oracle、Microsoft SQL Server以及IBM DB2，SAP DB意味着另一种选择。2000年10月，SAP  AG依 GNU GPL许可（请参见[附录J：GNU通用公共许*可*](gpl-license.html)）发布了SAP DB，从而使得其成为开放源码软件。

目前，MaxDB已被世界各地约3500个SAP客户使用。不仅如此，在SAP的IT部门内，大多数安装在Unix和Linux平台上的DBMS均依赖于MaxDB。MaxDB正转向重负荷联机事务处理（OLTP），用户数以千计，数据库的大小从数百GB到数TB。

2003年，SAP和MySQL确立了合作伙伴关系，并达成了开发合作协议。作为其结果，自7.5版发布以来（2003年11月），SAP的数据库系统SAP  DB以MySQL的MaxDB名义提供。

MaxDB 7.5版是SAP DB 7.4编码基数的直接改进。因此，MaxDB软件7.5版可用于SAP DB 7.2.04版和更高版本的直接升级。

与以往相同，目前，位于SAP AG的前SAP DB开发团队仍负责MaxDB的开发和支持。MySQL  AB与位于SAP的MaxDB团队密切合作，致力于改进MaxDB产品，请参见[1.5节，“MaxDB数据库管理系统概述”](introduction.html#maxdb)。SAP AG和MySQL  AB均负责MaxDB的销售和分发。MaxDB和MySQL服务器的提升促进了企业协作，从而使得两种产品系列均从中受益。

与SAP解决方案一起提供之前，或放在MySQL站点供下载之前，MaxDB受SAP AG全面质量保证计划的控制。

### 1.5.3. MaxDB的特性

MaxDB是一种大型、通过SAP认证的开放源码数据库，可用于OLTP和OLAP，它具有高的可靠性、可用性和可伸缩性，以及相当完善的特性集。它定位于大型mySAP商业套件环境，以及需要最大企业级数据库功能的其他应用，此外，它还补充了MySQL数据库服务器。

MaxDB是采用客户端/服务器模式运作的产品。开发它的目的在于满足OLTP和数据仓库/OLAP/决策支持方面的安装需求。**优点：** 

·     **简单的配置和管理：**  基于GUI（图形化用户界面）的安装管理器和数据库管理器，可作为DBMS操作的单个管理工具。

·      **不间断操作，无需计划的停机时间，也不需要持久性维护**：自动空间管理，无需重组。

·      **精心设计的备份和恢复能力**：联机备份和增量备份，恢复向导以指导你完成整个恢复步骤。

·     **支持大量用户，数TB的数据库大小，以及苛刻的工作量要求：**  高的可靠性，性能和可伸缩性

·     **高可用性：** 簇支持，待机配置，热待机配置



### 1.5.4. 许可和支持

使用MySQL  AB提供的其他产品的许可证，可使用MaxDB。因此，可在GNU通用公共许可下以及商业许可下使用MaxDB。关于许可的更多信息，请访问http://www.mysql.com/company/legal/licensing/。

MySQL  AB负责为非SAP客户提供MaxDB技术支持。MaxDB支持可在各种层面上提供（基本，银质和金质），将无限的电子邮件/Web支持扩展为对业务关键系统的全天候电话支持。

当MaxDB与Sap应用程序（如SAP NetWeaver和mySAP商业套件）一起使用时，MySQL  AB还能为其提供许可证和支持。关于能满足您需求的许可和支持方面的更多信息，[请联系](http://www.mysql.com/company/contact/)MySQL AB。

我们也提供咨询和培训服务。MySQL将定期提供MaxDB课程，关于课程表，请参见http://www.mysql.com/training/。

### 1.5.5. MaxDB和MySQL之间的特性差异

MaxDB是MySQL AB公司通过SAP认证的数据库。MaxDB数据库服务器补充了MySQL  AB产品系列。某些MaxDB特性在MySQL数据库服务器上不可用，反之亦然。

下面简要介绍了MaxDB和MySQL的主要差别，但并不完全。

·      MaxDB是采用客户端/服务器模式运作的系统。MySQL能够作为客户端/服务器系统运行，也能作为嵌入式系统运行。

·     MaxDB或许不能运行在MySQL支持的所有平台上。

·      MaxDB采用了针对客户端／服务器通信的专有网络协议。MySQL采用了TCP/IP（采用或未采用SSL加密）、套接字协议（类似Unix的系统下）或命名管道（Windows  NT系列下）。

·      MaxDB支持存储程序。对于MySQL，在5.0版本中实现了存储程序。MaxDB还支持通过SQL扩展进行的触发程序编程，该功能计划在MySQL  5.1中实现。MaxDB包含针对存储程序语言的调试程序，能够将多个嵌套式触发程序串联在一起，而且每个动作和行均支持多个触发程序。

·      MaxDB的发布采用了基于文本、图形或Web的用户界面。MySQL的发布仅采用基于文本的用户界面：图形化用户界面（MySQL控制中心、MySQL管理器）与主发布版本是单独提供的。针对MySQL的基于Web的用户界面是由第三方提供的。

·      MaxDB支持多种也被MySQL支持的编程接口。为了使用MaxDB进行开发，还提供了MaxDB  ODBC驱动程序，SQL数据库连通（SQLDBC），JDBC驱动程序，Perl和Python模块，以及MaxDB PHP扩展（通过使用PHP来访问MySQL  MaxDB数据库）。第三方编程接口： 支持OLE DB、ADO、DAO、RDO、以及.NET和ODBC。MaxDB支持嵌入式SQL和C/C++。

·     MaxDB包含MySQL不具备的管理特性：  按时间、事件和告警进行规划安排，并能在达到告警阈值时将消息发送给数据库管理器。

### 1.5.6. MaxDB和MySQL之间的协同性

MaxDB和MySQL是独立的数据库管理服务器。系统间的协同性是可能的，通过相应的方式，系统能够彼此交换数据。要想在MaxDB和MySQL之间交换数据，可使用系统的导入和导出工具，或MaxDB同步管理器。对于导入和导出工具，可在手动模式下传输数据（很少出现）。MaxDB同步管理器提供了更快的数据传输功能。

MaxDB装载器可用于导出数据和对象定义。装载器能够以MaxDB内部二进制格式和文本格式（CSV）导出数据。对于以文本格式从MaxDB导出的数据，可使用**mysqldump**数据库备份程序将其重新导入到MySQL中。要想将MySQL数据导入到MaxDB，可使用**mysqldump**创建INSERT语句或SELECT  ... INTO OUTFILE语句以创建文本文件（CSV）。使用MaxDB装载器装载由MySQL生成的数据文件。

可以使用MaxDB装载器和MySQL工具**mysqldump**，在系统间交换数据定义。由于两种系统使用的SQL“方言”略有差异，而且MaxDB拥有目前尚不被MySQL支持的特性（如SQL约束），我们建议以手动方式调整定义文件。**Mysqldump**工具提供了“--compatible-name  = maxdb”选项来生成与MaxDB兼容的输出，以便使移植更为简单。

作为MaxDB  7.6的组成部份，发布了MaxDB同步管理器。同步管理器支持数个MaxDB实例间的异步复制。但是，也设计规划了协同特性，因此，同步管理器支持复制到MySQL服务器的操作，以及来自MySQL服务器的复制操作。

在首次发布的版本中，同步管理器支持将数据插入到MySQL。这意味着，在开始时仅支持从MaxDB到MySQL的复制。在2005年的安排中，将增加把数据从MySQL服务器导出到同步管理器的功能，因而增加了对从MySQL到MaxDB的复制支持。

### 1.5.7. 与MaxDB有关的链接

关于MaxDB信息的主页位于http://www.mysql.com/products/maxdb。在该页面上，详细介绍了MaxDB数据库管理系统的特性，并提供了指向文档的多个超级链接。

除了本章给出的介绍外，MySQL参考手册不含任何MaxDB文档。MaxDB有自己的文档，称为MaxDB库。MaxDB库可从下述网址获得：http://dev.mysql.com/doc/maxdb/index.html。

MySQL AB运行着一个关于MaxDB的社区邮件列表，请参见http://lists.mysql.com/maxdb。该列表给出了生动活泼的社区讨论。很多核心开发人员均为其提供了相应的贡献。产品发布将被发送至该列表。

MaxDB的Web论坛网址是http://forums.mysql.com/。该论坛主要处理关于MaxDB的问题，而不是关于SAP应用程序的问题。

## 1.6. MySQL发展大事记

- [1.6.1.  MySQL 5.1的新特性](introduction.html#mysql-5-1-nutshell)

在本节中，介绍了MySQL发展历程中的重要事件，包括各种MySQL版本中已实现的主要特性或规划中的特性。在下节中，介绍了各发布系列的相关信息。

当前的生产版本系列是MySQL 5.0，据称它能稳定地用于生产环境，如2005年10月发布的5.0.15版。以前的生产版本系列是MySQL  4.1，据称它也能稳定地用于生产环境，如2004年10月发布的4.1.7版。“生产状态”意味着未来的5.0和4.1开发仅限于修正缺陷。对于较早的MySQL  4.0和3.23系列，仅会对关键缺陷进行更正。

对于MySQL 5.0和5.1系列，相关的MySQL开发正在积极进行当中，并会为后者增加新的特性。

从1个版本系列升级到下一个版本系列之前，请参见[2.10节，“升级MySQL”](installing.html#upgrade)的介绍。

在下面的表格中，归纳了要求最迫切的特性，以及实施了这些特性或计划实施这些特性的版本： 

| **特性**                     | **MySQ系列**                    |
| ---------------------------- | ------------------------------- |
| Foreign keys                 | 3.23（针对InnoDB存储引擎）      |
| Unions                       | 4.0                             |
| Subqueries                   | 4.1                             |
| R-trees                      | 4.1（针对MyISAM 存储引擎）      |
| Stored procedures            | 5.0                             |
| Views                        | 5.0                             |
| Cursors                      | 5.0                             |
| XA transactions              | 5.0                             |
| Foreign keys                 | 5.1（在3.23中实施，对于InnoDB） |
| Triggers                     | 5.0和5.1                        |
| Full outer joins             | 5.1                             |
| Constraints                  | 5.1（在3.23中实施，对于InnoDB） |
| Partitioning                 | 5.1                             |
| Pluggable Storage Engine API | 5.1                             |
| Row-Based Replication        | 5.1                             |

### 1.6.1. MySQL  5.1的新特性

关于我们打算在MySQL 5.1中增加的特性列表，请参见[1.6节，“MySQL发展大事记”](introduction.html#roadmap)。随着5.1版的不断发展，我们将在本节增加更多详细信息。

另请参见[第18章：](partitioning.html)[*分区*](partitioning.html)。

## 1.7. MySQL信息源

- [1.7.1.  MySQL邮件列表](introduction.html#questions)
- [1.7.2.  IRC（在线聊天系统）上的MySQL社区支持](introduction.html#irc)
- [1.7.3.  MySQL论坛上的MySQL社区支持](introduction.html#forums)

### 1.7.1. MySQL邮件列表

- [1.7.1.1.  MySQL邮件列表](introduction.html#mailing-list)
- [1.7.1.2.  请教问题或通报缺陷](introduction.html#asking-questions)
- [1.7.1.3.  如何通报缺陷和问题](introduction.html#bug-reports)
- [1.7.1.4.  在邮件列表上回答问题的指南](introduction.html#answering-questions)



在本节中介绍了MySQL邮件列表，并给出了使用邮件列表的指南。订购邮件列表后，将以电子邮件消息的形式收到所有已记录的信息。你也可以将自己的问题和解答发送至邮件列表。 

#### 1.7.1.1. MySQL邮件列表



要想订购本节所介绍的邮件列表或取消订购，请访问http://lists.mysql.com/。对于大多数邮件列表，可选择能够获取单独消息的正规列表版本，或选择按天发布的包含大量消息的文摘版本。

不要将订购信息或取消订购的信息发送到邮件列表，原因在于，这类消息将自动分发给数千位其他用户。

在你的所在地，可能有很多MySQL邮件列表的订户。如果是这样，该地点可能会有本地邮件列表，这样，从lists.mysql.com发出的消息将被传播到本地列表。在这类情形下，请与你的系统管理员联系，添加或删除本地MySQL列表。

如果希望将邮件列表的信息传送到邮件程序的邮箱中，请根据消息标题设置过滤器。可以使用列表ID： 或投递至： 识别列表消息的标题。

MySQL列表包含： 

·     通告

该列表用于通告新的MySQL版本和相关程序。这是1种低容量列表，所有的MySQL用户均应订购它。

·     mysql 

这是关于一般MySQL讨论的主要列表。请注意，对于某些主题来说，在更专门的列表中会得到更好地讨论。如果将问题张贴到了错误的列表，可能不会得到回答。

·     缺陷

该列表面向那些希望随时了解自上次MySQL版本发布以来已通报事宜的人员，或希望积极参与缺陷寻找和更正进程的人员。请参见[1.7.1.3节，“如何通报缺陷和问题”](introduction.html#bug-reports)。

·     内部构件

该列表面向那些与MySQL代码打交道的人员。它也是讨论MySQL开发并张贴补丁的论坛。

·     mysqldoc 

该列表面向那些与MySQL文档打交道的人员： MySQL AB公司的人员，译者，以及其他社区成员。

·     基准

该列表面向任何对性能事宜感兴趣的人员。讨论主要集中在数据库性能方面（不限于MySQL），也包括更广的类别，如内核性能、文件系统、磁盘系统等。

·     packagers（包装程序）

该列表主要讨论包装和分发MySQL方面的问题。这是供分发版维护人员交流MySQL打包事宜的论坛，为的是确保在所有支持的平台和操作系统上，MySQL的外观和感觉尽可能类似。

·     java 

该列表主要讨论MySQL服务器和Java方面的问题。它主要讨论JDBC驱动程序，包括MySQL Connector/J。

·     win32 

该列表涵盖了在Microsoft操作系统环境下（如Windows 9x, Me, NT, 2000,  XP和2003）与MySQL软件有关的所有主题，

·     myodbc 

该列表涵盖了与使用ODBC连接到MySQL服务器有关的所有主题。

·     gui-tools 

该列表涵盖了与MySQL GUI工具有关的所有主题，包括MySQL管理员以及MySQL控制中心图形客户端。

·     cluster 

该列表主要讨论MySQL簇。

·     dotnet 

该列表主要讨论MySQL服务器和.NET平台方面的问题。它与MySQL Connector/Net提供人的关系最密切。

·     plusplus 

该列表涵盖了使用C++ API进行MySQL编程的所有主题。

·     perl 

该列表涵盖了与Perl对MySQL支持、以及DBD::mysql有关的所有主题。

如果无法从MySQL邮件列表或论坛获得问题解答，一种选择是购买MySQL AB的支持服务。这样，你就能与MySQL开发人员直接联系。

下面介绍了一些英语以外其他语言的MySQL邮件列表。这些邮件列表不是由MySQL AB运营的。

·     <[mysql-france-subscribe@yahoogroups.com](mailto:mysql-france-subscribe@yahoogroups.com)> 

法语邮件列表。

·     <[list@tinc.net](mailto:list@tinc.net)> 

朝鲜语邮件列表。发送电子邮件订购mysql [your@email.address](mailto:your@email.address)。

·     <[mysql-de-request@lists.4t2.com](mailto:mysql-de-request@lists.4t2.com)> 

德语邮件列表。发送电子邮件订购mysql-de [your@email.address](mailto:your@email.address)。在http://www.4t2.com/mysql/站点上，可找到关于该邮件列表的更多信息。

·     <[mysql-br-request@listas.linkway.com.br](mailto:mysql-br-request@listas.linkway.com.br)> 

葡萄牙语邮件列表。发送电子邮件订购mysql-br [your@email.address](mailto:your@email.address)。

·     <[mysql-alta@elistas.net](mailto:mysql-alta@elistas.net)> 

西班牙语邮件列表。发送电子邮件订购mysql [your@email.address](mailto:your@email.address)。

#### 1.7.1.2. 请教问题或通报缺陷



张贴缺陷报告或问题之前，请： 

·     首先搜索MySQL在线手册，http://dev.mysql.com/doc/。我们经常更新该手册，以使该手册保持最新，其中包含相应的解决方案和新发现的问题。变更史（http://dev.mysql.com/doc/mysql/en/News.html）可能更有用，原因在于，在较新的版本中可能包含对你所提出问题的解决方案。

·     搜索缺陷数据库，http://bugs.mysql.com/，查找该缺陷是否已通报或更正。

·     搜索MySQL邮件列表档案，http://lists.mysql.com/。

·     你也可以使用http://www.mysql.com/search/来搜索MySQL AB网站上的所有网页（包含手册）。

如果无法在手册或档案中找到答案，请与本地MySQL专家协商。如果仍无法获得解答，在与我们联系之前，请按照介绍发送电子邮件至MySQL邮件列表，具体内容见下一节。

#### 1.7.1.3. 如何通报缺陷和问题



通报缺陷的正常地址是http://bugs.mysql.com/，它也是我方缺陷数据库的地址。这是1个公共数据库，任何人都能浏览它并进行相应的搜索。如果登录到系统，可输入新的报告。

编写良好的缺陷报告需要耐心，但在第1时间正确地完成它不仅能节省我们的时间，也能节省你自己的时间。良好的缺陷报告应包含对缺陷的完整测试情况，以便我们能够在下个版本中更正该缺陷。本节介绍的内容用于帮助你正确地编写报告，从避免将你的时间浪费在对我们帮助不大或没有帮助的事上， 

我们鼓励任何人使用**mysqlbug**脚本来生成缺陷报告（或通报问题）。**Mysqlbug**可在脚本目录下找到（源码分发版），也能在MySQL安装目录的bin子目录下找到（二进制分发版）。如果不能使用**mysqlbug**（例如，如果你正在Windows平台上运行），应包括本节所述的所有必要信息（更重要的是，应介绍操作系统和MySQL版本），这点十分重要。

通过自动确定下述信息，**mysqlbug**脚本能够帮助你生成报告，但是，如果遗漏了某些重要事项，请将其包含在消息中。请认真阅读本节，并确保在你的报告中包含了本节所述的所有信息。

在张贴问题前，最好使用MySQL服务器的最新生产版或开发版对问题进行测试。通过在所含的测试范例上使用“mysql test <  script_file”，或运行缺陷报告中所含的Shell或Perl脚本，任何人均应能重复该缺陷。

对于在缺陷数据库（http://bugs.mysql.com/）中张贴的所有缺陷，均会被纳入或记录在下一个MySQL版本中。如果只需要少量更改就能更正问题，我们或许会给出更正该问题的补丁。

如果发现MySQL中存在敏感的安全缺陷，请发送电子邮件至[security@mysql.com](mailto:security@mysql.com)。

如果有1份可重复的缺陷报告，请将其提交到缺陷数据库，[http://bugs.mysql.com/](???)。注意，即使在该情况下，也应首先运行**mysqlbug**脚本以找出与你的系统有关的信息，这是一个不错的习惯。对于任何我们能再现的缺陷，在下一个MySQL版本中修正它的机会很大。

要想通报其他问题，请使用MySQL邮件列表。

请注意，我们可能会对包含过多信息的消息做出响应，但不太会对包含过少信息的消息做出回应。人们常会省略掉一些事实，因为他们认为自己知道了故障的原因，并想当然地认为这类细节无关紧要。良好的原则是：  如果你对陈述某事犹豫不定，请陈述之。如果我们要求你提供初始报告中缺少的信息，在报告中编写多行信息源比等候回复要快，麻烦也更小。

在缺陷报告中，最常犯的错误包括：（a）未包含所使用MySQL的版本号，以及（b）未完全描述安装了MySQL服务器的平台（包括平台类型，以及版本号）。这是高度相关的信息，如果没有它，99％的缺陷报告无用。我们遇到这类问题，“为何它对我没用”？  随后，我们发现在该MySQL版本中，所请求的特性尚未实施，或在较新的MySQL版本中已更正了报告中描述的缺陷。有些时候，错误与平台相关，在这类情况下，如果不知道操作系统和平台的版本号，我们几乎不可能更正任何问题。

如果你是从源码编译MySQL的，如果与问题有关，还应提供有关编译器的信息。问题经常出在编译器，但人们却认为问题与MySQL有关。大多数编译均处于不断的开发过程中，并会变得越来越好。为了确定问题是否与你的编译器有关，我们需要知道你所使用的编译器。注意，所有的编译问题均应被当作缺陷并予以通报。

在你的报告中包含良好的问题描述时，报告最有帮助。也就是说，应给出示例，指明导致问题的所有事项，并准确描述问题本身。最好的报告应包含完整的示例，这类示例应阐明再现缺陷或问题的方式。请参见[E.1.6节，“如果出现表崩溃，请生成测试案例”](porting.html#reproduceable-test-case)。 

如果程序产生了错误消息，也应将其包含在你的报告中，这点很重要。如果我们打算使用程序搜索档案，最好是通报的错误消息与程序生成的错误消息准确匹配。（即使是字母的大小写也应考虑在内）。永远不要尝试从记忆中再现错误消息，而是应将整个消息拷贝并粘贴到报告中。

如果遇到与Connector/ODBC (MyODBC)有关的问题，请生成1份跟踪文件，并与报告一起发送给我们。请参见[26.1.1.9节，“如何通报MyODBC问题或缺陷”](connectors.html#myodbc-bug-report)。

请记住，很多阅读你报告的人员会使用80列的显示器。使用**mysql**命令行工具生成报告或示例时，如果输出内容可能会超过这类显示器的可用宽度，应使用“--vertical”选项（或“\G”语句终结符），例如EXPLAIN  SELECT语句，请参见本节后面给出的示例。

请在你的报告中包含下述信息： 

·     你所使用的MySQL分发版的版本号（例如MySQL  4.0.12）。通过执行**mysqladmin  version**，即可了解正在运行版本。**Mysqladmin**程序位于MySQL安装目录的bin子目录下。

·     出现问题的机器的制造商和型号。

·      操作系统的名称和版本。如果你使用的是Windows操作系统，通常能通过双击“我的电脑”图标并点击“帮助/关于Windows”菜单来了解操作系统的名称和版本。对于大多数Unix操作系统，可通过执行命令uname  –a获取这类信息。

·      某些时候，内存容量（实际内存和虚拟内存）也有关系。如果怀疑它，也应包含这类数值。

·      如果你正在使用的是MySQL软件的源码分发版，还须提供所使用编译器的名称和版本。如果使用的是二进制分发版，需要提供其名称。

·      如果在编译过程中出现问题，应给出准确的错误消息，出错文件中的不良代码，以及该代码附近的数行内容。

·      如果**mysqld**停止运行，还应通报导致**mysqld**崩溃的查询。通常，能够通过运行启用了查询日志功能的**mysqld**找出它，然后在**mysqld**崩溃后查找日志。请参见[E.1.5节，“使用日志文件找出mysqld中的错误原因”](porting.html#using-log-files)。

·     如果数据库表与问题有关，还应包含**mysqldump  --no-data** ***db_name\***  ***tbl_name\***的输出。这是一种了解数据库中表相关信息的简单易行而且功能强大的方式。该信息能帮助我们建立与你所遇到的情况相匹配的场景。

·     对于与SELECT语句的速度有关的缺陷或问题，总应包含“EXPLAIN  SELECT ...”的输出，以及SELECT语句生成的行数（至少）。对于每个涉及的表，应包含SHOW CREATE TABLE  *tbl_name*的输出。你所提供的关于具体情况的信息越多，得到帮助的可能性就越大。

下面给出了一个良好缺陷报告的示例。应使用**mysqlbug**脚本张贴它。本例采用了**mysql**命令行工具。对于输出内容可能会超过80列显示器可用宽度的语句，应使用“\G”语句终结符。

```
mysql> SHOW VARIABLES;
mysql> SHOW COLUMNS FROM ...\G
       <output from SHOW COLUMNS>
mysql> EXPLAIN SELECT ...\G
       <output from EXPLAIN>
mysql> FLUSH STATUS;
mysql> SELECT ...;
       <A short version of the output from SELECT,
       including the time taken to run the query>
mysql> SHOW STATUS;
       <output from SHOW STATUS>
```

·      如果在运行**mysqld**时出现错误或问题，应提供导致异常的输入脚本。该脚本应包含任何所需的源文件。越能再现具体情况的脚本越好。如果能够创建可再现的测试范例，请将其张贴到http://bugs.mysql.com/，它将得到优先对待。

如果你不能提供脚本，至少应在你的邮件中包含**mysqladmin variables extended-status  processlist**的输出，以提供关于系统执行情况的某些信息。

·      如果不能生成包含数行内容的测试范例，或者如果测试表过大以至于无法发送到邮件列表（超过10行），应使用**mysqldump**转储表，并创建描述问题的README文件。

使用**tar**和**gzip**或**zip**创建文件的压缩包档案，并使用FTP将档案传输到[ftp://ftp.mysql.com/pub/mysql/upload/](ftp://ftp.mysql.com/pub/mysql/upload/)。然后将问题提交到我们的缺陷数据库中，http://bugs.mysql.com/。

·      如果你认为MySQL服务器生成了奇怪的查询结果，不仅应包含结果，还应给出你对该结果的看法，以及支持观点的基础。

·      提供问题的示例时，最好使用实际情况下已有的变量名、表名等，而不是新名称。问题可能与变量名或表名有关。或许这类情况很罕见，但安全总比道歉强。归根结底，对你来说，提供关于实际情况的示例要简单些，当然对我们也更好。如果你的数据不打算展示给其他人，请使用FTP将其传输到[ftp://ftp.mysql.com/pub/mysql/upload/](ftp://ftp.mysql.com/pub/mysql/upload/)。如果信息是高度保密的，而且你甚至不打算向我们展示，请使用其他名称给出示例，但请注意，这应是最后的选择。

·      如果可能，应包含相关程序的所有选项。例如，应指明启动**mysqld**服务器时使用的选项，以及用来运行MySQL客户端程序的选项。对于程序（如**mysqld**和**mysql）选项以及configure脚本的选项，通常是解答问题的关键，关系十分密切。**包含它们总不是坏主意。如果使用了任何模块，如Perl或PHP等，还应给出它们的版本。

·      如果你的问题与权限系统有关，请给出**mysqlaccess**的输出，**mysqladmin  reload**的输出，以及进行连接时获得的所有错误消息。测试权限时，首先应运行**mysqlaccess**。接下来，执行**mysqladmin  reload  version**，并与导致问题的程序相连。**mysqlaccess**可在MySQL安装目录的bin子目录下找到。

·      如果你有关于某一缺陷的补丁，也请将它包含在内。但不要认为该补丁是我们所需的全部，如果未提供补丁所更正缺陷的必要信息（如测试范例），不要假定我们会使用它。我们可能会通过补丁发现问题，或者不能理解该补丁，如果是这样，我们不会使用该补丁。

如果我们不能准确核实补丁的目的，将不会使用它。测试范例会对我们有所帮助。请指明该补丁能处理所有的问题。如果我们发现补丁不能工作的临界情况（即使很罕见），它可能是无用的。

·      关于缺陷是什么、出现原因、以及缺陷导因的猜测通常是错的。即使是MySQL团队，在未使用调试器判定缺陷真实原因的情况下，也不能妄加猜测。

·      请在你的缺陷报告中指明，你已参阅了参考手册并寄出了档案，以便让其他人知道你已作了自行解决问题的尝试。

·      如果遇到解析错误，请仔细检查语法。如果不能找出错误出现在那里，很可能是因为你使用的MySQL服务器版本不支持你使用的语法。如果你使用的是http://dev.mysql.com/doc/上提供的当前版本和手册，不要包含你所使用的语法，MySQL服务器不支持你的查询。在这种情况下，唯一的选择是自行实施语法，或发送电子邮件至<[licensing@mysql.com](mailto:licensing@mysql.com)>，并寻求实施方案。

如果手册中涵盖了你所使用的语法，但你使用的是旧版本MySQL服务器，请检查MySQL变更史，以查看语法的实施时间。在这种情况下，可以选择升级到较新的MySQL服务器版本。请参见[附录D：MySQL变更史](news.html)。

·     如果问题在于数据崩溃，或访问特殊表时出错，首先应使用CHECK  TABLE和REPAIR  TABLE或**myisamchk**进行检查并尝试修复。请参见[第5章：数据库管理](database-administration.html)。

如果你使用的操作系统是Windows，请使用SHOW VARIABLES LIKE  'lower_case_table_names'命令核实“lower_case_table_names”的值。

·      如果经常获得崩溃的表，请尝试找出发生的时间和原因。在这种情况下，MySQL数据目录下的错误日志可能会包含关于它的一些信息。（这是名称中包含.err后缀的文件）。请参见[5.11.1节，“错误日志”](database-administration.html#error-log)。在你的缺陷报告中，请包含该文件提供的相关信息。如果在更新期间，未杀死更新进程，正常情况下，**mysqld**不会造成表损坏。如果你能够找到**mysqld**停止的原因，我们会更容易地为你提供更正它的补丁。请参见[A.1节，“如何确定导致问题的原因”](problems.html#what-is-crashing)。

·      如果可能，请下载并安装最新版本的MySQL服务器，并检查你的问题是否得到解决。所有版本的MySQL软件均经过彻底测试，并应能无故障运行。我们致力于尽可能地向后兼容，你也应能够毫不困难地在不同的MySQL版本间进行切换。请参见[2.1.2节，“选择要安装的MySQL分发版”](installing.html#which-version)。

如果你是享受支持服务的客户，请将缺陷报告交叉张贴在[mysql-support@mysql.com](mailto:mysql-support@mysql.com)，以获得更高的优先级，并将其张贴到恰当的邮件列表，以查看是否有人遇到了类似问题（或解决了问题）。

关于通报MyODBC中存在缺陷的更多信息，请参见[26.1.1.9节，“如何通报MyODBC问题或缺陷”](connectors.html#myodbc-bug-report)。

关于某些常见问题的解决方案，请参见[附录](problems.html)[A：*问题和常见错误*](problems.html)。

将答案单独发送给你而不是发送到邮件列表时，良好的礼节是，对回答进行归纳总结并将结果发送到邮件列表，以便其他人也能从你所收到、并解决了问题的回应中受益。

#### 1.7.1.4. 在邮件列表上回答问题的指南



如果你认为自己的解答会引起广泛关注，可以将其张贴到邮件列表，而不是直接回复请教的个人。尽量使你的解答具有普遍性，以便除初始发起人之外的其他人也能从中受益。将解答张贴到邮件列表时，请确认你的解答不是已有答案的复制品。

在你的回复中，应尽量归纳问题的基本部分，没有必要一定引用全部初始信息。

在要在打开HTML模式的情况下从浏览器张贴邮件信息。很多用户不使用浏览器来阅读邮件。

### 1.7.2. IRC（在线聊天系统）上的MySQL社区支持



除了各种MySQL邮件列表外，在IRC（在线聊天系统）上，也能发现有经验的社区成员。以下是目前我们已知的最好的网络／渠道： 

·      **Freenode**（请参见http://www.freenode.net/以查找服务器信息）

o     #mysql，主要针对MySQL问题，也欢迎其他数据库和一般的SQL问题。与MySQL一起使用PHP、Perl或C方面的问题也很常见。

如果你正在寻找URC客户端软件，以便连接到IRC网络，请访问xChat（http://www.xchat.org/）。X-Chat（GPL许可）即能用于Unix平台，也适用于Windows平台（免费的面向Windows的X-Chat可从站点http://www.silverex.org/download/上下载）。

### 1.7.3. MySQL论坛上的MySQL社区支持



最新的社区支持资源是位于下述站点的论坛：http://forums.mysql.com。

有各种可用论坛，分为以下大类： 

·     移植

·     MySQL用法

·     MySQL连接器

·     编程语言

·     工具

·     第三方应用程序

·     存储引擎

·     MySQL技术

·     SQL标准

·     业务

## 1.8. MySQL标准的兼容性

- [1.8.1.  MySQL遵从的标准是什么](introduction.html#standards)
- [1.8.2.  选择SQL模式](introduction.html#sql-mode)
- [1.8.3.  在ANSI模式下运行MySQL](introduction.html#ansi-mode)
- [1.8.4.  MySQL对标准SQL的扩展](introduction.html#extensions-to-ansi)
- [1.8.5.  MySQL与标准SQL的差别](introduction.html#differences-from-ansi)
- [1.8.6.  MySQL处理约束的方式](introduction.html#constraints)



在本节中，介绍了MySQL与ANSI/ISO  SQL标准的关系。MySQL服务器有很多对SQL标准的扩展之处，这里介绍了它们是什么，以及使用它们的方法。你也能了解关于MySQL服务器缺失功能的信息，以及如何处理某些差异的方法。

SQL标准自1986年以来不断演化发展，有数种版本。在本手册中，“SQL-92”指得是1992年发布的标准，“SQL:1999”指得是1999年发布的标准，“SQL:2003”指得是标准的当前版本。我们采用术语“SQL标准”标示SQL标准的当前版本。

我们的目标是在没有良好理由的情况下不限制MySQL服务器的可用性。即使我们没有足够的资源就每种可能的应用进行开发，我们始终愿意帮助那些在新领域使用MySQL服务器的人员，并向他们提供建议。

对于该产品，我们的一项主要目标是，继续致力于与SQL标准的兼容性，但不以牺牲速度和可靠性为代价。如果它们能显著增加拥有大量用户基数的MySQL服务器的可用性，我们无惧于为SQL添加扩展，也无惧于为非SQL特性提供支持。MySQL服务器4.0中的HANDLER接口即是该策略的例子。请参见[13.2.3节，“HANDLER语法”](sql-syntax.html#handler)。

我们将继续支持事务性和非事务性数据库，以满足任务关键型全天候应用，以及高负载Web或日志应用。

MySQL服务器最初是为小型计算机系统上中等规模的数据库设计的（100万-1亿行，或每个表的大小为100MB）。目前，MySQL服务器能处理TB级别的数据库，也能在针对便携式设备或嵌入式设备的精简版本中使用。MySQL服务器的精简设计使得双向开发成为可能，不会在源码树中产生任何冲突。

目前，我们并未定位于实时支持，虽说MySQL复制特性提供了强大的功能。

在众多第三方簇解决方案中均有数据库簇支持特性，自4.1.2版以来，对于我们所需的NDB簇技术集成方案，同样请参见[第17章：](ndbcluster.html)[*MySQL簇*](ndbcluster.html)。

我们也正着手在数据库服务器中提供XML支持。

### 1.8.1. MySQL遵从的标准是什么

我们致力于支持全套ANSI/ISO SQL标准，但不会以牺牲代码的速度和质量为代价。

ODBC级别0-3.51。

### 1.8.2. 选择SQL模式

MySQL服务器能够工作在不同的SQL模式下，并能针对不同的客户端以不同的方式应用这些模式。这样，应用程序就能对服务器操作进行量身定制以满足自己的需求。

这类模式定义了MySQL应支持的SQL语法，以及应该在数据上执行何种确认检查。这样，就能在众多不同的环境下、与其他数据库服务器一起更容易地使用MySQL。

可以使用“--sql-mode="modes"”选项，通过启动**mysqld**来设置默认的SQL模式。从MySQL  4.1开始，也能在启动之后，使用ET [SESSION|GLOBAL]  sql_mode='modes'语句，通过设置sql_mode变量更改模式。

关于设置服务器模式的更多信息，请参见[5.3.2节，“SQL服务器模式”](database-administration.html#server-sql-mode)。

### 1.8.3. 在ANSI模式下运行MySQL



你可以使用“--ansi”启动选项，要求**mysqld**使用ANSI模式。请参见[5.3.1节，“**mysqld**命令行选项”](database-administration.html#server-options)。

在ANSI模式下运行服务器与使用该选项启动它的效果一样（在一行上指定“--sql_mode”值）： 

```
--transaction-isolation=SERIALIZABLE
--sql-mode=REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,
IGNORE_SPACE
```

在MySQL 4.1中，能够用下述两条语句实现相同的效果（在一行上指定“sql_mode”值）： 

```
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SET GLOBAL sql_mode = 'REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,
IGNORE_SPACE';
```

请参见[1.8.2节，“选择SQL模式”](introduction.html#sql-mode)。

在MySQL 4.1.1中，也能用下述语句设置sql_mode选项： 

```
SET GLOBAL sql_mode='ansi';
```

在本例中，将sql_mode变量的值设置为与ANSI模式相关的所有选项。你可以检查其结果，如下所示： 

```
mysql> SET GLOBAL sql_mode='ansi';
mysql> SELECT @@global.sql_mode;
        -> 'REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,
            IGNORE_SPACE,ANSI';
```

### 1.8.4. MySQL对标准SQL的扩展



MySQL服务器包含一些其他SQL  DBMS中不具备的扩展。注意，如果使用了它们，将无法把代码移植到其他SQL服务器。在某些情况下，你可以编写包含MySQL扩展的代码，但仍保持其可移植性，方法是用“/*...  */”注释掉这些扩展。在本例中，MySQL服务器能够解析并执行注释中的代码，就像对待其他MySQL语句一样，但其他SQL服务器将忽略这些扩展。例如： 

```
SELECT /*! STRAIGHT_JOIN */ col_name FROM table1,table2 WHERE ...
```

如果在字符“!”后添加了版本号，仅当MySQL的版本等于或高于指定的版本号时才会执行注释中的语法： 

```
CREATE /*!32302 TEMPORARY */ TABLE t (a INT);
```

这意味着，如果你的版本号为3.23.02或更高，MySQL服务器将使用TEMPORARY关键字。

下面按类别介绍了各种MySQL扩展。

·     磁盘上的数据组织

MySQL服务器会将每个数据库映射到MySQL数据目录下的1个目录中，并将数据库中的表映射到数据库目录下的文件名。它具有下述含义： 

o    如果操作系统的文件名区分大小写（如大多数Unix系统），当MySQL服务器运行在这类操作系统上时，数据库名和表名也区分大小写。请参见[9.2.2节，“识别符大小写敏感性”](language-structure.html#name-case-sensitivity)。

o     你可以使用标准的系统命令来备份、重命名、移动、删除、并拷贝由MyISAM或ISAM存储引擎管理的表。例如，要想重命名MyISAM表，可重命名表对应的.MYD、.MYI、以及.frm文件。

数据库、表、索引、列或别名能够以数字开头（但或许不能全部由数字构成）。

·     通用语言语法

o     可以使用“””或“’”括住字符串，而不仅是“’”。

o    在字符串中使用“\”作为转义字符。

o     在SQL语句中，可以使用“*db_name.tbl_name*”语法访问不同数据库中的表。某些SQL服务器提供了相同的功能，但调用该用户空间除外。MySQL服务器不支持表空间，如下述语句中使用的那样：  CREATE TABLE ralph.my_table...IN my_tablespace. 

·     SQL语句的语法

o    ANALYZE  TABLE，CHECK TABLE，OPTIMIZE  TABLE，以及REPAIR TABLE语句。

o    CREATE DATABASE和DROP  DATABASE语句。请参见[13.1.3节，“CREATE DATABASE语法”](sql-syntax.html#create-database)。

o    DO语句。

o    EXPLAIN SELECT获取如何联合表的介绍。

o     FLUSH和RESET语句。

o    SET语句。请参见[13.5.3节，“SET语法”](sql-syntax.html#set-option)。

o     SHOW语句。请参见[13.5.4节，“SHOW语法”](sql-syntax.html#show)。

o    使用LOAD DATA  INFILE。在很多情况下，该语法与Oracle的LOAD DATA INFILE兼容。请参见[13.2.5节，“LOAD DATA INFILE语法”](sql-syntax.html#load-data)。

o    RENAME TABLE的使用。请参见[13.1.9节，“RENAME TABLE语法”](sql-syntax.html#rename-table)。

o     使用REPLACE取代DELETE +  INSERT。请参见[13.2.6节，“REPLACE语法”](sql-syntax.html#replace)。

o    在ALTER TABLE语句中使用CHANGE  col_name、DROP col_name、或DROP  INDEX、IGNORE或RENAME。在ALTER  TABLE语句中使用多个ADD、ALTER、DROP或CHANGE子句。请参见[13.1.2节，“ALTER TABLE语法”](sql-syntax.html#alter-table)。

o    使用索引名，字段前缀上的索引，并在CREATE  TABLE语句中使用INDEX或KEY。请参见[13.1.5节，“CREATE TABLE语法”](sql-syntax.html#create-table)。

o    与CREATE  TABLE一起使用TEMPORARY或IF NOT EXISTS。

o    与DROP TABLE一起使用IF EXISTS。

o    使用单个DROP TABLE语句，能够舍弃多个表。

o    UPDATE和DELETE语句的ORDER  BY和LIMIT子句。

o    INSERT INTO ... SET col_name =  ... syntax. 

o     INSERT和REPLACE语句的DELAYED子句。

o     INSERT、REPLACE、DELETE和UPDATE语句的LOW_PRIORITY子句。

o    在SELECT语句中使用INTO  OUTFILE和STRAIGHT_JOIN。请参见[13.2.7节，“SELECT语法”](sql-syntax.html#select)。

o     SELECT语句中的SQL_SMALL_RESULT选项。

o    不需要在GROUP  BY部分命名所有选择的列。对于某些十分特殊但相当正常的查询，它能提供更好的性能。请参见[12.10节，“与GROUP  BY子句同时使用的函数和修改程序``”](functions.html#group-by-functions-and-modifiers)。

o    可以与GROUP BY一起指定ASC和DESC。

o    能够在带有“:=”赋值操作符的语句中设置变量。

```
o                     mysql> SELECT @a:=SUM(total),@b=COUNT(*),@a/@b AS avg
o                         -> FROM test_table;
o                     mysql> SELECT @t1:=(@t2:=1)+@t3:=4,@t1,@t2,@t3;
```

·     列类型

o     列类型MEDIUMINT、SET、ENUM、以及不同的BLOB和TEXT类型。

o     列属性AUTO_INCREMENT、BINARY、NULL、UNSIGNED以及ZEROFILL。

·     函数和操作符

o     为了使其他SQL环境下的用户更容易入手，MySQL服务器对很多函数均支持别名特性。例如，所有的字符串函数均支持标准SQL语法和ODBC语法。

o     MySQL服务器能够理解“||”和“&&”操作符，将其当作逻辑OR和AND，就像在C编程语言中那样。在MySQL服务器中，||和OR是同义词，&&和AND也是同义词。由于采用了该优异的语法体系，MySQL服务器不支持SQL针对字符串连接的“||”操作符，而采用了CONCAT()取而代之。由于CONCAT()能够接受任意数目的参量，很容易将使用“||”操作符的情况转换为MySQL服务器支持的类型。

o    请在有多于1个元素的场合下使用COUNT(DISTINCT  list)。

o     默认情况下，所有的字符串比较均区分大小写，其分类顺序由当前字符集确定（默认为cp1252  Latin1）。如果你不喜欢该点，应使用BINARY属性或BINARY  cast声明列，这样，就会使用基本的字符代码值进行比较，而不是词汇顺序。

o     “%”操作符等同于MOD()。也就是说“N %  M”等同于MOD(N,M)。Cyuyan的程序员支持“%”，而且它也是为了兼容PostgreSQL而使用的。

o     在列比较中，可在SELECT语句的FROM左侧使用=、<>、<=、<、>=、>、<<、>>、<=>、AND、OR或LIKE操作符。例如： 

```
o                     mysql> SELECT col1=1 AND col2=2 FROM tbl_name;
```

o     返回最近AUTO_INCREMENT值的LAST_INSERT_ID()函数。请参见[12.9.3节，“信息函数”](functions.html#information-functions)。

o    允许在数值列上使用LIKE。

o    REGEXP和NOT  REGEXP扩展了常规的表达式操作符。

o     具有1个或2个以上参量的CONCAT()或CHAR()。（在MySQL服务器中，这些函数可以有任意数目的参量）。

o     BIT_COUNT()、CASE、ELT()、FROM_DAYS()、FORMAT()、IF()、PASSWORD()、ENCRYPT()、MD5()、ENCODE()、DECODE()、PERIOD_ADD()、PERIOD_DIFF()、TO_DAYS()、以及WEEKDAY()函数。

o     使用TRIM()来调整子字符串。标准SQL仅支持单个字符的删除。

GROUP  BY函数STD()、BIT_OR()、BIT_AND()、BIT_XOR()、以及GROUP_CONCAT()。请参见[12.10节，“与GROUP  BY子句同时使用的函数和修改程序``”](functions.html#group-by-functions-and-modifiers)。

### 1.8.5. MySQL与标准SQL的差别

- [1.8.5.1. 子查询](introduction.html#ansi-diff-subqueries)
- [1.8.5.2. SELECT INTO  TABLE](introduction.html#ansi-diff-select-into-table)
- [1.8.5.3. 事务和原子操作](introduction.html#ansi-diff-transactions)
- [1.8.5.4.  存储程序和触发程序](introduction.html#ansi-diff-triggers)
- [1.8.5.5. 外键](introduction.html#ansi-diff-foreign-keys)
- [1.8.5.6.  视图](introduction.html#ansi-diff-views)
- [1.8.5.7.  ‘--’作为注释起始标记](introduction.html#ansi-diff-comments)

我们试图使MySQL服务器遵从ANSI SQL标准和ODBC SQL标准，但在某些情况下MySQL服务器执行的操作有所不同： 

·      对于VARCHAR列，存储值时删除了尾部空间。（在MySQL 5.0.3中更正）。请参见[A.8节，“MySQL中的已知事宜”](problems.html#bugs)。

·      在某些情况下，定义表或更改其结构时，将CHAR列转换为VARCHAR列。（在MySQL  5.0.3中更正）。请参见[13.1.5.1节，“沉寂的列规格变更”](sql-syntax.html#silent-column-changes)。

·      删除表时，不自动取消关于表的权限。必须明确发出REVOKE语句，以撤销针对表的权限。请参见[13.5.1.3节，“GRANT和REVOKE语法”](sql-syntax.html#grant)。

·      CAST()函数不支持对REAL或BIGINT的抛弃。请参见[12.8节，“Cast函数和操作符”](functions.html#cast-functions)。

·     标准SQL要求，SELECT语句中的HAVING子句能够引用GROUP  BY子句中的列。在MySQL 5.0.2之前，不能完成该功能。

#### 1.8.5.1. 子查询

MySQL  4.1支持子查询和导出表。“子查询”指的是嵌套在另一语句中的SELECT语句。“导出表”（未命名视图）是另一语句的FROM子句中的子查询。请参见[13.2.8节，“Subquery语法”](sql-syntax.html#subqueries)。

从MySQL 4.1版起，可以使用联合或其他方法重写大多数子查询。关于如何完成该任务的更多信息，请参见[13.2.8.11节，“对于较早的MySQL版本，采用联合方法重写子查询”](sql-syntax.html#rewriting-subqueries)。

#### 1.8.5.2. SELECT INTO  TABLE



MySQL服务器不支持Sybase SQL扩展： SELECT ... INTO TABLE  ....。但MySQL服务器支持标准的SQL语法INSERT INTO ... SELECT ...，它基本上相同。请参见[13.2.4.1节，“INSERT ... SELECT语法”](sql-syntax.html#insert-select)。

```
INSERT INTO tbl_temp2 (fld_id)
    SELECT tbl_temp1.fld_order_id
    FROM tbl_temp1 WHERE tbl_temp1.fld_order_id > 100;
```

作为备选方式，可以使用SELECT INTO OUTFILE ...或CREATE TABLE ...  SELECT。

从5.0版开始，MySQL支持SELECT ...  INTO，以及用户变量。在使用光标和局部变量的存储程序中也可以使用相同的语法。请参见[20.2.9.3节，“SELECT ...  INTO语句”](stored-procedures.html#select-into-statement)。

#### 1.8.5.3. 事务和原子操作



MySQL服务器（3.23至该系列的最高版本，所有4.0版本，以及更高版本）支持采用InnoDB和BDB事务存储引擎的事务。InnoDB提供了全面的ACID兼容性。请参见[第15章：](storage-engines.html)[*存储引擎和表类型*](storage-engines.html)。

MySQL服务器中的其他非事务性存储引擎（如MyISAM）遵从不同的数据完整性范例，称之为“原子操作”。按照事务术语，MyISAM表总能高效地工作在AUTOCOMMIT=1模式下。原子操作通常能提供可比较的完整性以及更好的性能。

由于MySQL服务器支持两种范例，因而你能决定是否利用原子操作的速度更好地服务于你的应用程序，或使用事务特性。该选择可按表进行。

正如所阐述的那样，事务性和非事务性表类型之间的权衡主要取决于性能。事务性表对内存和磁盘空间的要求更高，CPU开销也更大。另一方面，多种事务性表类型，如InnoDB，也能提供很多显著特性。MySQL服务器的模块化设计允许同时使用不同的存储引擎，以满足不同的要求，并在所有情形下，提供最佳性能。

但是，即便使用非事务性MyISAM表，你将如何使用MySQL服务器的特性来保持严格的完整性呢？这些特性与事务性表类型相比又如何呢？ 

\1.   如果应用程序采用了特定的编写方式，依赖于在关键情况下能够调用ROLLBACK而不是COMMIT，那么事务性类型更方便。使用事务，还能确保未完成的更新或崩溃的活动不被提交到数据库，能为服务器提供自动回滚的机会，并保存你的数据库。

如果使用非事务性表，MySQL服务器几乎在所有情况下均允许你解决潜在的问题，方式是在更新前进行简单检查，并运行检查数据库一致性的简单脚本，如果出现不一致性，该脚本能自动修复它或给出告警。注意，仅使用MySQL日志或增加额外日志，通常能完美地更正表，同时不会造成数据完整性损失。

\2.   在很多情况下，能够对关键的事务更新进行重写，使之成为“原子”类型。一般而言，所有由事务解决的完整性问题均能用LOCK  TABLES或原子更新解决，从而确保了服务器不会自动中断，后者是事务性数据库系统的常见问题。

\3.   为了安全使用MySQL服务器，无论是否使用事务性表，仅需启用备份和二进制日志功能。这样，你就能解决使用其他事务性数据库系统时遇到的任何问题。无论使用的数据库系统是什么，启用备份总是个好主意。

事务范型有自己的优点和不足之处。很多用户和应用程序开发人员喜欢这类简单性，在出现问题时或必要时，通过代码解决问题。但是，即使你是原子操作范型的新手，或更熟悉事务，也请考虑非事务性表的速度益处，与经过优化调整的最快的事务性表相比，它的速度快3～5倍。

在完整性具有最高重要性的情况下，即使是对非事务性表，MySQL也能提供事务级别的可靠性和安全性。如果使用LOCK  TABLES锁定了表，所有更新均将被暂时中止直至完整性检查完成。如果你获得了对某一表的READ  LOCAL锁定（与写锁定相对），该表允许在表尾执行并行插入，当其他客户端执行插入操作时，允许执行读操作。新插入的记录不会被有读锁定属性的客户端看到，直至解除了该锁定为止。使用INSERT  DELAYED，能够将插入项置于本地队列中，直至锁定解除，不会让客户端等待插入完成。请参见[13.2.4.2节，“INSERT DELAYED语法”](sql-syntax.html#insert-delayed)。

从我们赋与其名称的意义上，“原子”绝非不可思议的。它仅意味着，你能确信在每个特性更新运行的同时，其他用户不能干涉它，而且不会出现自动回滚（如果你不小心，对于事务性表，这种情况可能发生）。MySQL服务器还能保证不存在脏读。

下面列出了使用非事务性表的一些技术： 

·     对于需要事务的循环，通常能使用LOCK  TABLES进行编码，不需要光标来更新正在处理的记录。

·     要想避免使用ROLLBACK，可采取下述策略： 

\1.  使用LOCK TABLES锁定所有希望访问的表。

\2.  执行更新前，测试必须为真的条件。

\3.  如果一切正常，执行更新。

\4.  使用UNLOCK TABLES解除锁定。

与使用具有回滚可能性的事务性表相比，它通常具有更快的速度，虽然并非始终如此。该解决方案唯一不能处理的情形是，在更新中途杀死了线程。在这种情况下，将释放所有锁定，但某些更新可能尚未执行。

·      也可以使用函数在单一操作中更新记录。采用下述技术，能获得效率很高的应用程序。

o    根据其当前值更改列。

o    仅更新出现实际变化的列。

例如，当我们更新某些客户信息时，仅更新已更改的客户数据，与原始行相比，仅测试已更改的数据或依赖于已更改数据的数据是否未出现变化。对于已更改数据的测试，它是通过UPDATE语句的WHERE子句完成的。如果记录未更新，将向客户端发出消息：  “一些你改变的数据已被其他用户更改”。接下来，我们在窗口中给出了旧行和新行，以便用户决定使用哪个版本。

这给出了与列锁定类似的结果，但效果更好，使用相对于其当前值的值，仅更新了某些列。这意味着，典型的UPDATE语句与下面给出的类似： 

```
UPDATE tablename SET pay_back=pay_back+125;
 
UPDATE customer
  SET
    customer_date='current_date',
    address='new address',
    phone='new phone',
    money_owed_to_us=money_owed_to_us-125
  WHERE
    customer_id=id AND address='old address' AND phone='old phone';
```

它很有效，即使其他客户端更改了pay_back或money_owed_to_us列中的值，也能使用。

·     在很多情况下，用户希望将LOCK  TABLES和／或ROLLBACK用于管理唯一ID。可以在不使用锁定功能或回滚的情况下，使用AUTO_INCREMENT列以及LAST_INSERT_ID()  SQL函数或mysql_insert_id() C API函数，更有效地处理之。请参见[12.9.3节，“信息函数”](functions.html#information-functions)。请参见[25.2.3.36节，“mysql_insert_id()”](apis.html#mysql-insert-id)。

我们通常能使用代码来处理行级锁定方面的需求。在某些情况下，实际上不需要它，InnoDB表支持行级锁定。通过MyISAM表，能够在表中使用标志列，并完成类似下面的操作： 

```
UPDATE tbl_name SET row_flag=1 WHERE id=ID;
```

如果找到行，而且原始行中的row_flag不是1，对于受影响的行数，MySQL返回1。

你可以认为MySQL将前述查询更改为： 

```
UPDATE tbl_name SET row_flag=1 WHERE id=ID AND row_flag <> 1;
```

#### 1.8.5.4. 存储程序和触发程序



对于MySQL，在5.0版本中实现了存储程序。请参见[第20章：](stored-procedures.html)[*存储程序和函数*](stored-procedures.html)。

从5.0.2版开始，在MySQL中实现了基本的触发器功能，计划在MySQL 5.1中进一步发展它。请参见[第21章：](triggers.html)[*触发程序*](triggers.html)。

#### 1.8.5.5. 外键



在MySQL服务器3.23.44和更高版本中，InnoDB存储引擎支持对外键约束的检查功能，这些约束包括CASCADE、ON  DELETE和ON UPDATE。请参见[15.2.6.4节，“FOREIGN  KEY约束”](storage-engines.html#innodb-foreign-key-constraints)。

对于InnoDB之外的其他存储引擎，MySQL服务器能够解析CREATE TABLE语句中的FOREIGN  KEY语法，但不能使用或保存它。未来将进行扩展，能够将这类信息保存到表规范文件中，以便能被**mysqldump**和ODBC检索。稍后，还将为MyISAM表实现外键约束。

外键增强为数据库开发人员提供了多项益处： 

·     假定关联设计恰当，外键约束使得程序员更难将不一致性引入数据库。

·      数据库服务器具有集中式约束检查功能，因而没有必要在应用程序一侧执行这类检查。这样，就消除了不同应用程序使用不同方式检查约束的可能性。

·     使用级联更新和删除，简化了应用程序代码。

·     设计恰当的外键有助于以文档方式记录表间的关系。

请记住，这些好处是以数据库服务器为执行必要检查而需的额外开销为代价的。服务器额外检查会影响性能，对于某些应用程序，该特性不受欢迎，应尽量避免。（出于该原因，在一些主要的商业应用程序中，在应用程序级别上实施了外键逻辑）。

MySQL允许数据库开发人员选择要使用的方法。如果你不需要外键，并希望避免与强制引用完整性有关的开销，可选择另一种表类型取而代之，如MyISAM。（例如，MyISAM存储引擎为仅执行INSERT和SELECT操作的应用程序提供了极快的性能，这是因为插入能和检索同时进行）。请参见[7.3.2节，“表锁定事宜”](optimization.html#table-locking)。

如果你不打算利用引用完整性检查具备的优点，请记住下述要点： 

·      不存在服务器端外键关联检查时，应用程序本身必须处理这类关联事宜。例如，将行按恰当顺序插入表时应谨慎，并应避免产生孤立的子记录。必须能够在多记录插入操作期间更正出现的错误。

·     如果ON  DELETE是应用程序所需的唯一引用完整性功能，请注意，从MySQL服务器4.0起，可以使用多表DELETE语句，用单一语句从多个表中删除行。请参见[13.2.1节，“DELETE语法”](sql-syntax.html#delete)。

·     从具有外键的表删除记录时，在缺少ON  DELETE的情况下，一种解决方式是为应用程序增加恰当的DELETE语句。实际上，它与使用外键同样快，而且移植性更好。

注意，使用外键在某些情况下会导致问题。

·      外键支持能处理很多引用完整性事宜，但仍需要仔细设计键的关系，以避免循环规则或不正确的级联删除组合。

·      DBA需要创建关联拓扑，这会使从备份中恢复单独表变得困难，该类情形并不罕见。（加载依赖其他表的表时，MySQL允许你临时禁止外键检查，从而降低了该难度）。请参见[15.2.6.4节，“FOREIGN  KEY约束”](storage-engines.html#innodb-foreign-key-constraints)。在MySQL  4.1.1以前。重新加载时，**mysqldump**能够生成自动利用该性能的转储文件。

注意，SQL中的外键用于检查和强制引用完整性，而不是联合表。如果打算用SELECT语句获取多个表的结果，可在表之间执行联合操作： 

```
SELECT * FROM t1, t2 WHERE t1.id = t2.id;
```

请参见[13.2.7.1节，“JOIN语法”](sql-syntax.html#join)。请参见[3.6.6节，“使用外键”](tutorial.html#example-foreign-keys)。

ODBC应用程序常使用不带“ON DELETE ...”的FOREIGN  KEY语法来生成自动WHERE子句。

#### 1.8.5.6. 视图



在MySQL服务器5.0版中实现了视图功能（包括可更新视图）。在5.0.1和更高版本中，提供了二进制版的视图功能。请参见[第22章：](views.html)[*视图*](views.html)。

View（视图）十分有用，它允许用户像单个表那样访问一组关系（表），而且仅允许对它们的这类访问。视图也能限制对行的访问（特定表的子集）。对于列控制的访问，可使用MySQL服务器中的高级权限系统。请参见[5.7节，“MySQL访问权限系统”](database-administration.html#privilege-system)。

在设计视图的过程中，我们的宏伟目标是，在SQL的范围内尽可能与关联数据库系统的“Codd's Rule  #6”兼容。“所有理论上可更新的视图，实际上也应是可更新的”。

#### 1.8.5.7. ‘--’作为注释起始标记



一些其他SQL数据库采用“--”作为注释开始标志。MySQL服务器采用“#”作为注释起始字符。对于MySQL服务器，也能使用C风格的注释：/*该处为注释*/。请参见[9.5节，“注释语法”](language-structure.html#comments)。

MySQL服务器3.23.3和更高版本支持“--”注释风格，但要求注释后面跟1空格（或控制字符，如新行）。之所以要求使用空格，是为了防止与自动生成SQL查询有关的问题，它采用了类似下面的代码，其中，自动为“!payment!”插入“payment”的值： 

```
UPDATE account SET credit=credit-!payment!
```

考虑一下，如果“payment”的值为负数如“-1”时会出现什么情况： 

```
UPDATE account SET credit=credit--1
```

在SQL中“credit--1”是合法的表达式，但是，如果“--1”被解释为注释开始，部分表达式将被舍弃。其结果是，表达式的意义与预期的意义完全不同。

```
UPDATE account SET credit=credit
```

该语句不会对值作任何更改！这表明，允许注释以“--”开始会产生严重后果。

采用MySQL服务器3.23.3和更高版本中的这类注释方法，“credit--1”实际上很安全。

另一个安全特性是，**mysql**命令行客户端将删除所有以“--”开头的行。

仅当使用高于3.23.3的MySQL时，下述信息才有意义： 

如果有1个文本文件形式的SQL程序，该文件包含“--”注释，应按下述方式使用**replace**实用工具，将其转换为使用“#”字符的注释： 

```
shell> replace " --" " #" < text-file-with-funny-comments.sql \
         | mysql db_name
```

而不是通常的： 

```
shell> mysql db_name < text-file-with-funny-comments.sql
```

你也可以编辑注释文件，将“--”注释更改为“#”注释： 

```
shell> replace " --" " #" -- text-file-with-funny-comments.sql
```

使用下述命令将其改回去： 

```
shell> replace " #" " --" -- text-file-with-funny-comments.sql
```

### 1.8.6. MySQL处理约束的方式

- [1.8.6.1. PRIMARY  KEY和UNIQUE索引约束](introduction.html#constraint-primary-key)
- [1.8.6.2. 对无效数据的约束](introduction.html#constraint-invalid-data)
- [1.8.6.3.  ENUM和SET约束](introduction.html#constraint-enum)



使用MySQL，你可以使用允许回滚的事务表，以及不允许回滚的非事务表。因此，在MySQL中的约束处理功能与其他DBMS中的略有不同。在非事务性表中插入或更新大量行时，当出现错误以至于不能回滚所作的变更时，必须处理该情况。

其基本原理在于，在解析将要执行的语句的同时，MySQL服务器会尽量为检测到的问题生成错误信息，并会在执行语句的同时尽量恢复出现的错误。在大多数情况下我们均是这样作的，但不包括所有情况。

出现错误时，MySQL可选择中途中止语句，或尽可能恢复并继续执行语句。默认情况下，服务器将采取后一种路线。这意味着，服务器可能会强制将非法值变为最接近的合法值（例如）。

从MySQL  5.0.2开始，提供了数种SQL模式，使用它们，能够对如何接受可能为不良数据值的方式进行更好的控制，也能在出现错误时，对是否继续执行语句或放弃语句进行控制。使用这些选项，能够将MySQL服务器配置为更为传统的风格，类似于拒绝不恰当输入的其他DBMS。可以在运行时设置SQL模式，这样，各客户端就能选择与其需求最为贴切的行为。请参见[5.3.2节，“SQL服务器模式”](database-administration.html#server-sql-mode)。

在以下部分，介绍了使用不同约束类型的情况。

#### 1.8.6.1. PRIMARY  KEY和UNIQUE索引约束



通常情况下，当你试图INSERT或UPDATE会导致主键、唯一键或外键冲突的行时，将出现错误。如果你正在使用事务性存储引擎时，如InnoDB，MySQL会自动回滚语句。如果你正在使用非事务性存储引擎，MySQL将在出错的行上停止执行语句，剩余的行也不再处理。

如果你希望忽略这类键冲突，可使用MySQL支持的、用于INSERT和UPDATE的IGNORE关键字。在这种情况下，MySQL将忽略任何键冲突，并继续处理下一行。请参见[13.2.4节，“INSERT语法”](sql-syntax.html#insert)。请参见[3.2.10节，“UPDATE语法”](sql-syntax.html#update)。

使用mysql_info() C API函数，能够获取关于实际插入或更新行数的信息。请参见[25.2.3.34节，“mysql_info()”](apis.html#mysql-info)。在MySQL  4.1和更高版本中，也能使用SHOW WARNINGS语句。请参见[13.5.4.22节，“SHOW WARNINGS语法”](sql-syntax.html#show-warnings)。

目前，只有InnoDB表支持外键。请参见[15.2.6.4节，“FOREIGN  KEY约束”](storage-engines.html#innodb-foreign-key-constraints)。计划在MySQL 5.1中实施对MyISAM表的外键支持。

#### 1.8.6.2. 对无效数据的约束



在MySQL 5.0.2之前，MySQL对非法或不当值并不严厉，而且为了数据输入还会强制将它们变为合法值。在MySQL  5.0.2和更高版本中，保留了以前的默认行为，但你可以为不良值选择更传统的处理方法，从而使得服务器能够拒绝并放弃出现不良值的语句。本节介绍了MySQL的默认行为（宽大行为），新的严格的SQL模式，以及它们的区别。

如果你未使用严格模式，下述情况是真实的。如果将“不正确”的值插入到列，如将NULL值插入非NULL列，或将过大的数值插入数值列，MySQL会将这些列设置为“最可能的值”，而不是生成错误信息。

·      如果试图将超范围的值保存到数值列，MySQL服务器将保存0（最小的可能值）取而代之，或最大的可能值。

·     对于字符串，MySQL或保存空字符串，或将字符串尽可能多的部分保存到列中。

·     如果打算将不是以数值开头的字符串保存到数值列，MySQL将保存0。

·      MySQL允许将特定的不正确日期值保存到DATE和DATETIME列（如“2000-02-31”或“2000-02-00”）。其观点在于，验证日期不是SQL服务器的任务。如果MySQL能保存日期值并准确检索相同的值，MySQL就能按给定的值保存它。如果日期完全不正确（超出服务器能保存的范围）将在列中保存特殊的日期值“0000-00-00”取而代之。

·      如果试图将NULL值保存到不接受NULL值的列，对于单行INSERT语句，将出现错误。对于多行INSERT语句或INSERT  INTO ...  SELECT语句，MySQL服务器会保存针对列数据类型的隐含默认值。一般情况下，对于数值类型，它是0，对于字符串类型，它是空字符串('')，对于日期和时间类型是“zero”。在[13.1.5节，“CREATE  TABLE语法”](sql-syntax.html#create-table)一节中，讨论了隐含的默认值。

·      如果INSERT语句未为列指定值，如果列定义包含明确的DEFAULT子句，MySQL将插入默认值。如果在定义中没有这类DEFAULT子句，MySQL会插入列数据类型的隐含默认值。

采用前述规则的原因在于，在语句开始执行前，无法检查这些状况。如果在更新了数行后遇到这类问题，我们不能仅靠回滚解决，这是因为存储引擎可能不支持回滚。中止语句并不是良好的选择，在该情况下，更新完成了“一半”，这或许是最差的情况。对于本例，较好的方法是“仅可能做到最好”，然后就像什么都未发生那样继续。

在MySQL  5.0.2和更高版本中，可以使用STRICT_TRANS_TABLES或STRICT_ALL_TABLES  SQL模式，选择更严格的处理方式。请参见[5.3.2节，“SQL服务器模式”](database-administration.html#server-sql-mode)。

STRICT_TRANS_TABLES的工作方式： 

·      对于事务性存储引擎，在语句中任何地方出现的不良数据值均会导致放弃语句并执行回滚。

·      对于非事务性存储引擎，如果错误出现在要插入或更新的第1行，将放弃语句。（在这种情况下，可以认为语句未改变表，就像事务表一样）。首行后出现的错误不会导致放弃语句。取而代之的是，将调整不良数据值，并给出告警，而不是错误。换句话讲，使用STRICT_TRANS_TABLES后，错误值会导致MySQL执行回滚操作，如果可以，所有更新到此为止。

要想执行更严格的检查，请启用STRICT_ALL_TABLES。除了非事务性存储引擎，它与STRICT_TRANS_TABLES等同，即使当不良数据出现在首行后的其他行，所产生的错误也会导致放弃语句。这意味着，如果错误出现在非事务性表多行插入或更新过程的中途，仅更新部分结果。前面的行将完成插入或更新，但错误出现点后面的行则不然。对于非事务性表，为了避免这种情况的发生，可使用单行语句，或者在能接受转换警告而不是错误的情况下使用STRICT_TRANS_TABLES。要想在第1场合防止问题的出现，不要使用MySQL来检查列的内容。最安全的方式（通常也较快）是，让应用程序负责，仅将有效值传递给数据库。

有了严格的模式选项后，可使用INSERT IGNORE或UPDATE  IGNORE而不是不带IGNORE的INSERT或UPDATE，将错误当作告警对待。

#### 1.8.6.3. ENUM和SET约束

ENUM和SET列提供了定义仅能包含给定值集合的列的有效方式。但是，从MySQL  5.0.2起，ENUM和SET不是实际约束。其原因与不重视NOT  NULL的原因一样。请参见[1.8.6.2节，“对无效数据的约束”](introduction.html#constraint-invalid-data)。

ENUM列总有1个默认值。如果未指定默认值，对于包含NULL的列，默认值为NULL；否则，第1个枚举值将被当作默认值。

如果在ENUM列中插入了不正确的值，或者，如果使用IGNORE将值强制插入了ENUM列，会将其设置为保留的枚举值0，对于字符串情形，将显示为空字符串。请参见[11.4.4节，“ENUM类型”](column-types.html#enum)。

如果在SET列中插入了不正确值，该值将被忽略。例如，如果列能包含值“a”、“b”和“c”，并赋值“a,x,b,y”，结果为“a,b”。请参见[11.4.5节，“SET类型”](column-types.html#set)。

从5.0.2开始，可以对服务器进行配置，以使用严格的SQL模式。请参见[5.3.2节，“SQL服务器模式”](database-administration.html#server-sql-mode)。启用严格模式后，ENUM或SET列的定义可作为对输入至列的值的约束。如果值不满足下述条件，将出现错误： 

·      ENUM值必须是在列定义中给出的值之一，或内部的数字等同物。该值不能是错误值（即，0或空字符串）。对于定义为ENUM('a','b','c')的列，诸如''、'd'和'ax'等，均是非法的，并将被拒。

·      SET值必须是空字符串，或由1个或多个在列定义中给出的且用逗号隔开的值组成。  对于定义为SET('a','b','c')的列，诸如'd'和'a,b,c,d'等，均是非法的，并将被拒。

如果使用了INSERT IGNORE或UPDATE  IGNORE，在严格模式下，可抑制无效值导致的错误。在这种情况下，将生成警告而不是错误。对于ENUM，值将作为错误成员(0)插入。对于SET，会将给定值插入，但无效的子字符串将被删除。例如，'a,x,b,y'的结果是'a,b'，就像前面介绍的那样。

------

这是MySQL参考手册的翻译版本，关于MySQL参考手册，请访问[dev.mysql.com](http://dev.mysql.com/doc/mysql/en)。 原始参考手册为英文版，与英文版参考手册相比，本翻译版可能不是最新的。