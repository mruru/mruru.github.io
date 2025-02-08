使用 Quick SQL 可基于指定的简单文本快速创建关系数据模型。只需键入很少内容，就可以立即生成用于创建表、视图、触发器、索引结构以及示例数据的 SQL 命令。虽然无法替代正规的数据建模，但使用这种方式可以迅速生成 SQL 脚本，您以后可以根据需要不断改进它。

常见用例：

- 快速创建强大的数据模型
- 将精简 Quick SQL 另存为模型在以后调整或使用
- 轻松生成示例数据
- 使用提供的示例了解 SQL 语法



### 入门



在 SQL 面板中键入时，系统自动生成 SQL。在行开头输入父表名。添加列名，每行一个列名，列名跟在所属表的后面并使用一致缩进（2 个或更多空格）。系统自动生成主键。通过直接在父表下缩进子表来创建父/子关系，或者使用 `/fk` 列指令定义所需的任何外键关系。默认情况下，关系可防止删除引用的行。但是，要在删除父行时自动删除子行，请将 `/cascade` 指令添加到子表或子外键列。

非限定列默认为 `varchar2` 数据类型。要指定其他类型，请在列名后面添加 `num`、`int` 或 `date`，中间用空格分隔。如果未明确指定类型，系统可能会根据列名中的英语文本推断类型。例如，如果列名包含 "date" 一词，则为 `date` 数据类型，如果列名包含 "num" 或 "number" 一词，则为 `number` 数据类型。要指定特定 `varchar2` 长度，请输入 `vc*NN*`，其中 `*NN*` 是 `varchar2` 的长度。系统自动设置对象名称的格式，并将空格替换为下划线。因此，您可以在列名和表名中使用空格。

单击“设置”按钮可查看许多生成选项。您可以轻松添加“审计列”来跟踪创建和上次更新给定行的用户和时间。启用行版本列可简化丢失更新的检测，因为此列的值在每次更新行时自动递增 1。使用过程为每个表生成 PL/SQL 程序包 API，以便查询、插入、更新和删除行。

### 创建数据库对象



输入完精简 Quick SQL 并应用合适的设置后，您就可使用生成的 SQL。它可以创建数据库对象，并使用示例数据填充部分或所有表（可选）。按照以下步骤操作：

1. 从左侧窗格*操作*菜单中选择**保存模型**将精简 Quick SQL 另存为命名的模型，以便将来重用。
2. 输入模型名称。
3. 从右侧窗格 *SQL 脚本操作*菜单中选择**保存 SQL 脚本**将生成的输出另存为脚本。
4. 输入脚本名称。
5. 单击**检查并运行**导航到脚本编辑器。
6. 在脚本编辑器中，根据需要检查脚本内容。
7. 单击**运行**创建数据库对象。
8. 在脚本确认页上，单击**立即运行**。
9. 在结果页上，检查已处理、成功和出现错误的语句。如果报告了任何错误，请浏览结果页以查看特定 Oracle 错误消息。
10. （可选）单击**从脚本创建应用程序**转到创建应用程序向导，并根据在脚本中指定的表和视图自动生成页。



***注：***如果您希望将来更新精简 SQL，Oracle 建议您将精简 SQL 下载到计算机。如果您指定了任意设置，请导航到“Oracle SQL 输出”窗格底部，并将以 `# settings =` 开头的行复制到“精简 Quick SQL”窗格顶部。

从右侧窗格 *SQL 脚本操作*菜单中选择**下载**将 SQL 文件保存到您的计算机。

## 数据类型

| num, number                                 | NUMBER                                                       |
| ------------------------------------------- | ------------------------------------------------------------ |
| int, integer                                | INTEGER                                                      |
| d, date                                     | DATE                                                         |
| ts, timestamp                               | TIMESTAMP                                                    |
| tstz, tswtz, timestamp with local time zone | TIMESTAMP WITH LOCAL TIMEZONE                                |
| char, vc, varchar, varchar2, string         | VARCHAR2(4000)                                               |
| vcNNN                                       | VARCHAR2(NNN) NNN 表示介于 1 与 32767 之间的数字。           |
| vc(NNN)                                     | VARCHAR2(NNN) NNN 表示介于 1 与 32767 之间的数字。           |
| vc32k                                       | VARCHAR2(32767)                                              |
| clob                                        | CLOB                                                         |
| blob                                        | BLOB                                                         |
| json                                        | CLOB CHECK (<列名> IS JSON)                                  |
| file                                        | 添加 BLOB 列和 _FILENAME、_CHARSET、_MIMETYPE、_LASTUPD 列，这些列可增强通过浏览器下载文件的能力。 |

## 表指令

| /api                                    | 生成 PL/SQL 程序包 API，以便查询、插入、更新和删除数据。     |
| --------------------------------------- | ------------------------------------------------------------ |
| /audit                                  | 使用 Oracle 审计对更改进行审计（例如 `audit all on *table_name*`） |
| /auditcols, /audit cols, /audit columns | 使用触发器审计 `created`、`created_by`、`updated`、`updated_by` 列中的更改。 |
| /colprefix                              | 为表中的所有列名添加此值作为前缀。                           |
| /compress, /compressed                  | 压缩数据以优化空间。                                         |
| /insert NN                              | 插入 ***NN\*** (≤ 1000) 行示例数据（例如 `/insert 20`）      |
| /rest                                   | 启用使用 Oracle REST Data Services (ORDS) 自动访问 REST 数据 |
| /unique                                 | 生成表级别唯一约束条件（例如 `/unique state,city`）          |

## 列指令

| /idx, /index, /indexed       | 创建索引以提高搜索速度。                                     |
| ---------------------------- | ------------------------------------------------------------ |
| /unique                      | 使用唯一约束条件防止出现重复值。                             |
| /cascade                     | 删除父行时会自动删除行。                                     |
| /check                       | 列出只允许出现的值（区分大小写），例如 `/check Yes, No`      |
| /constant                    | 在生成数据时使用常数值（例如 `/constant NYC`）               |
| /default                     | 指定缺少列或列为空值时要插入的值。                           |
| /values                      | 列出生成数据的示例值（例如 `/values 1,2,3` 或 `/values Small,Large`） |
| /upper                       | 强制列值使用大写                                             |
| /lower                       | 强制列值使用小写                                             |
| /nn, /not null               | 需要提供使用 `not null` 约束条件的值。                       |
| /between                     | 将值约束在一个包含范围内（例如 `/between 1 and 100`）        |
| /hidden, /invisible          | 在 `select * from *table_name*` 的结果中隐藏值               |
| /references, /reference, /fk | 定义与另一个表的外键关系（例如 `/references *table_name*`）  |
| /pk                          | 在自动生成的主键未达到要求时标识主键。                       |
| --, [ comments ]             | 使用方括号或 -- 为模型添加注释                               |

## 视图

语法:

```
view [view_name] [table name] [table name]...
```

确保视图名称不包含空格，并且表名不包含空格。使用空格或逗号分隔表名。
示例:

```
    dept 
       dname
       loc
       emp
          ename
          job
    view dept_emp emp dept
    
```

## 设置



您可以在**设置**对话框中*以交互方式*定义 SQL 语法生成选项，也可以在精简 Quick SQL 中*明确*定义这些选项。在精简 Quick SQL 中包含设置可确保始终使用相同选项。例如，要为所有表名添加前缀 `test` 并为方案 `obe` 执行生成操作，请输入以下语句：

```
# settings = { prefix: "test", schema: "obe" }
```

或者，在单独行上输入每个设置可得到相同结果：

```
# prefix: "test"# schema: "obe"
```

***注：**设置必须从新行开始，以 `# settings = `开头，输入多个设置；或者以 `# `开头，每行输入一个设置。所有值不区分大小写。如果需要更加清楚，可以添加括号、空格或逗号。要在生成的脚本中包含所有设置，请使用 `# verbose: true`。*



| 设置              | 说明                                                         | 示例                                     | 默认值     |
| ----------------- | ------------------------------------------------------------ | ---------------------------------------- | ---------- |
| APEX              | 控制审计列触发器是否使用 APEX 可识别表达式设置 `created_by` 和 `updated_by` 列值：`coalesce(sys_context('APEX$SESSION','APP_USER'),user)`未启用时，将使用以下函数：`user` | # apex: true                             | false      |
| API               | 为每个表生成 PL/SQL 程序包 API，以便查询、插入、更新和删除其数据。 | # api: true                              | false      |
| AUDITCOLS         | 使用触发器审计每个表的 `created`、`created_by`、`updated`、`updated_by` 列中的更改。 | # auditcols: true                        | false      |
| COMPRESS          | 压缩每个表的数据以优化空间。                                 | # compress: true                         | false      |
| CREATEDBYCOL      | 覆盖表示行创建用户的默认审计列名。                           | # createdByCol: "created_by_user"        | created_by |
| CREATEDCOL        | 覆盖表示行创建时间的默认审计列名。                           | # createdCol: "created_date"             | created    |
| DATE              | 覆盖日期的默认数据类型。有效值如下：**date**, **timestamp**, **timestamp with time zone**, **TSWTZ**, **timestamp with local time zone**, **TSWLTZ**. | # date: "timestamp with local time zone" | date       |
| DB                | 为生成的 SQL 设置数据库版本兼容性。有效值：**19c**, **21c**, **23c** | # db: "19c"                              | 19c        |
| DROP              | 包括命令以在最初删除每个数据库对象。                         | # drop: true                             | false      |
| EDITIONABLE       | 在生成 PL/SQL 对象时, 包括触发器和程序包会使得它们可版本化。 | # editionable: true                      | false      |
| INSERTS           | 在设置为 false 时禁止生成所有示例数据。                      | # inserts: true                          | true       |
| LANGUAGE          | 选择生成示例数据使用的语言。支持的值：**EN**, **DE**, **KO**, **JA** | # language: "EN"                         | EN         |
| LONGVC            | 在配置为支持较长 varchar 的数据库中允许使用超过 4000 (≤ 32767) 的 `varchar2` 大小。 | # longVC: true                           | false      |
| PK                | 选择主键策略：标识、序列、SYS_GUID 或无。有效值：**guid**, **seq**, **identity**, **none** | # PK: "identity"                         | identity   |
| PREFIX            | 标识所有对象的公用前缀，前缀自动使用下划线分隔。             | # prefix: "foo"                          |            |
| PREFIXPKWITHTNAME | 为主键列添加表名作为前缀（例如 `EMPLOYEE_ID`）               | # prefixPKwithTname: true                | false      |
| GENPK             | 为每个表生成 `ID` 主键列。                                   | # genPK: false                           | true       |
| ROWKEY            | 在每个表中添加触发器分配的用作字母数字 ID 的 `ROW_KEY` 列。  | # rowkey: true                           | false      |
| ROWVERSION        | 在每个表中添加触发器分配的在更新时递增值的 `ROW_VERSION` 列。 | # rowVersion: true                       | false      |
| SCHEMA            | 使用可选的方案限定生成的 SQL 对象名称。                      | # schema: "scott"                        |            |
| SEMANTICS         | 将 `varchar2` 列长度语义设置为 `byte` 或 `char`，或使用默认值。**varchar2(4000)**, **varchar2(4000 byte)**, **varchar2(4000 char)** | # semantics: "char"                      |            |
| UPDATEDBYCOL      | 覆盖表示行上次更新用户的默认审计列名。                       | # updatedByCol: "updated_by_user"        | updated_by |
| UPDATEDCOL        | 覆盖表示行上次更新时间的默认审计列名。                       | # updatedCol: "updated_dt"               | updated    |

## 示例



| **产品销售** 定义包含 PRODUCTS、CUSTOMERS、CHANNELS、PROMOTIONS 和 SALES 表的星形方案。SALES 表具有对其他表的外键引用。标识数字列 (NUM)。为两列定义合法值 (/CHECK)。为所有表生成随机示例数据 (/INSERT)，并添加包含所有表的 SALES_V 联接视图。 | [加载模型](https://apex.oracle.com/pls/apex/#) |
| ----------------------------------------------- | --------------------------------------------------------- |
| **员工技能** 包括 DEPARTMENTS、EMPLOYEES 和 SKILLS 表，并使用缩进来标识父表和子表。指定 SQL 数据类型 (VC255)。生成十行随机数据 (/INSERT) 并提供示例值 (/VALUES)。定义使用检查约束条件的合法值 (/CHECK)。说明如何在方括号中添加列注释。 | [加载模型](https://apex.oracle.com/pls/apex/#) |
| **待办事项** 包括 ASSIGNEES 和 TODOS 表。定义 NOT NULL 约束条件 (/NN)。指定合法值 (/CHECK)。生成示例数据 (/INSERT)。添加审计列 (#auditcols: true)。启用创建定制待办事项系统（分钟）。 | [加载模型](https://apex.oracle.com/pls/apex/#) |
| **部门和员工** 包括 DEPARTMENTS 和 EMPLOYEES 表以及额外的 EMP_V 联接视图。生成随机数据 (/INSERT)。添加 NOT NULL 约束条件 (/NN)。使用缩进列表定义表、列和子表。说明如何自动添加主键列和外键列。 | [加载模型](https://apex.oracle.com/pls/apex/#) |
| **项目管理** 定义包含 PROJECTS 父表和四个子表（MILESTONES、LINKS、ATTACHMENTS 和 ACTION_ITEMS）的项目管理方案。添加视图 PROJECT_MS（联接项目和里程碑）和 PROJECT_AI（组合项目和操作项）。指定合法值 (/CHECK)、示例值 (/VALUES)，并生成示例行 (/INSERT)。在 # 符号后面采用 SQL 语法指令添加审计列 (auditcols: true)。 | [加载模型](https://apex.oracle.com/pls/apex/#) |