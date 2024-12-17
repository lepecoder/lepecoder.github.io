---
title: vim代码格式化插件clang-format
date: 2017-12-13T12:13:00
tags: ['vim']
categories: 开发工具
---


### 安装vim-clang-format插件

项目地址：

[vim-clang-format](https://github.com/rhysd/vim-clang-format)



如果用Vundle管理vim插件，直接在`.vimrc`中添加一行

```

Plugin 'vim-clang-format'

```

运行`:PluginInstall`就可以自动安装插件



```

"配置

let g:clang_format#auto_format_on_insert_leave=1	"退出插入模式时自动格式化

```



**支持的语言**

- C

- C++

- Objective-C

- JavaScript

- java

- TypeScript

- Protobuf





**需要事先安装clang-format**



### clang-format



```bash

$ pacman -S clang-format

```





**配置信息**



```

BasedOnStyle (string)

这个样式用于所有没有特殊指定配置的选项。

 

这个选项仅在clang-format配置中支持(在-style='{...}' 和 .clang-format 文件里).

 

可能的值有:

 

LLVM：符合LLVM代码标准的样式

Google：符合谷歌的c++样式指南的样式

Chromium：符合Chromium样式指南的样式

Mozilla：符合Mozilla样式指南的样式

WebKit：符合WebKit的样式指南的样式

AccessModifierOffset (int)

访问修饰符的缩进或者向外伸展,例如 public:。

AlignAfterOpenBracket (BracketAlignmentStyle)

如果为真（true），在一个左圆括号后水平对齐参数

这会应用在圆括号，尖括号和中（方）括号。

可能的值有：

BAS_Align (在配置中: Align) 在左圆括号后调整参数, 例如:

someLongFunction(argument1,

                 argument2);

 

BAS_DontAlign (在配置中: DontAlign) 不调整, 换用 ContinuationIndentWidth, 例如:

someLongFunction(argument1,

    argument2);

 

BAS_AlwaysBreak (在配置中: AlwaysBreak) 在左圆括号后总是换行, 如果参数不能适应单行, 例如:

someLongFunction(

    argument1, argument2);

 

AlignConsecutiveAssignments (bool)

如果为真（true），连续调整多行

这将会调整连续行中的分配操作符。这将会导致像下面这样的格式：

int aaaa = 12;

int b    = 23;

int ccc  = 23;

 

AlignConsecutiveDeclarations (bool)

如果为真（true）, 校准连续的声明。

 

这将会校准连续多行的声明的名字。这将会导致像下面这样的格式：

int         aaaa = 12;

float       b = 23;

std::string ccc = 23;

 

AlignEscapedNewlinesLeft (bool)

如果为真（true）, 校准被忽略的新行距左边尽可能远。或者把它们放到列的最右边

 

AlignOperands (bool)

如果为真（true）, 水平对齐二进制和三元表达式的操作数。

具体来说，这将一个表达式的操作数对准一个需要被分割的多行的操作数, 例如:

int aaa = bbbbbbbbbbbbbbb +

          ccccccccccccccc;

 

AlignTrailingComments (bool)

如果为真（true）, 对齐注释。

 

AllowAllParametersOfDeclarationOnNextLine (bool)

即使BinPackParameters是假的，也允许将一个函数声明的所有参数移到下一行.

 

AllowShortBlocksOnASingleLine (bool)

允许将简单的语句块放到一个单行.

例如, 这允许 将 语句 “if (a) { return; }” 放到一个单行.

 

AllowShortCaseLabelsOnASingleLine (bool)

如果为真（true）, 很短的情况下的标签将会被放到单独的行。

 

AllowShortFunctionsOnASingleLine (ShortFunctionStyle)

取决于值, 语句“int f() { return 0; }”可以被放到一个单行。

可能的值有：

SFS_None (在配置中： None) 从不合并方法或函数到单独的一行。

SFS_Empty (在配置中： Empty) 仅合并空的函数。

SFS_Inline (在配置中： Inline) 仅合并类中定义的方法或函数. 意味着 “empty”.

SFS_All (在配置中： All) 合并所有的方法适应单行.

 

AllowShortIfStatementsOnASingleLine (bool)

如果为真（true）, 语句“if (a) return;” 能被放到单行。

 

AllowShortLoopsOnASingleLine (bool)

如果为真（true）, 语句“while (true) continue;” 能被放到单行。

 

AlwaysBreakAfterDefinitionReturnType (DefinitionReturnTypeBreakingStyle)

用于函数定义返回类型换行样式。这个选项是过时的并且被保留向后兼容。

可能的值有：

DRTBS_None (在配置中： None) 再返回类型后自动换行。PenaltyReturnTypeOnItsOwnLine 会被考虑到.

DRTBS_All (在配置中： All) 总是在返回类型后换行。

DRTBS_TopLevel (在配置中： TopLevel) 总是在返回类型的顶级函数后换行。

 

AlwaysBreakAfterReturnType (ReturnTypeBreakingStyle)

用于函数声明返回类型换行样式。

可能的值有：

 

RTBS_None (在配置中： None) 在返回类型后自动换行。“PenaltyReturnTypeOnItsOwnLine”会被考虑.

RTBS_All (在配置中： All) 再返回类型后总是换行。

RTBS_TopLevel (在配置中： TopLevel) 在方法的顶层的返回类型后总是换行。

RTBS_AllDefinitions (在配置中： AllDefinitions) 在方法定义中的返回类型后总是换行。

RTBS_TopLevelDefinitions (在配置中： TopLevelDefinitions) 在顶层定义的返回类型后总是换行。

 

AlwaysBreakBeforeMultilineStrings (bool)

如果为真（true）, 在多行字面量字符串前总是换行。

这个标志意味着使在文件中有多行字符串的情况看起来更一致。因此，如果字符串被“ContinuationIndentWidth”空格导致换行，它将会在行首生效。

 

AlwaysBreakTemplateDeclarations (bool)

如果为真（true）, 在模板声明“template<...>”后总是换行

 

BinPackArguments (bool)

如果为假（false）， 函数调用的参数要么是在同一行上，要么将在同一行上有一行。

 

BinPackParameters (bool)

如果为假（false）， 函数声明或函数定义的参数将都在同一行上，或各有一行。

 

BraceWrapping (BraceWrappingFlags)

控制单独的大括号换行事件。

如果“BreakBeforeBraces”设置为“BS_Custom”, 使用这个指定如何处理每个单独的括号的情况。否则，这是被忽略的。

嵌套结构的标志:

bool AfterClass 使类定义换行.

bool AfterControlStatement 使控制语句(if/for/while/switch/..)换行。

bool AfterEnum 使枚举定义换行。

bool AfterFunction 使方法定义换行。

bool AfterNamespace 使命名空间定义换行。

bool AfterObjCDeclaration 使OC定义(@autoreleasepool, interfaces, ..)换行。

bool AfterStruct 使结构定义换行。

bool AfterUnion 使共同体定义换行。

bool BeforeCatch 在catch之前换行。

bool BeforeElse 在else之前换行。

bool IndentBraces 缩进换行的大括号。

 

BreakAfterJavaFieldAnnotations (bool)

在JAVA文件中每个注释后换行。

 

BreakBeforeBinaryOperators (BinaryOperatorStyle)

使二进制操作符换行的方法。

可能的值有：

BOS_None (在配置中： None) 在操作符后换行。

BOS_NonAssignment (在配置中： NonAssignment) 在操作符没有被指定前换行。

BOS_All (在配置中： All) 在操作符前换行。

 

BreakBeforeBraces (BraceBreakingStyle)

用于大括号换行样式。

可能的值有：

BS_Attach (在配置中： Attach) 总是将大括号与上下文连在一起。

BS_Linux (在配置中： Linux) 像Attach一样, 但是在一个方法、命名空间或一个类定义的大括号之前换行

BS_Mozilla (在配置中： Mozilla) 像Attach一样, 但是在一个枚举、方法或记录定义前换行。

BS_Stroustrup (在配置中： Stroustrup) 像Attach一样,但是在方法定义、catch、和else前换行

BS_Allman (在配置中： Allman) 总是在大括号之前换行。

BS_GNU (在配置中： GNU) 总是在括号前中断，并添加一个额外的级别的缩进到控件语句的括号中，而不是类、函数或其他定义的括号中。

BS_WebKit (在配置中： WebKit) 像Attach一样, 但是在方法前换行。

BS_Custom (在配置中： Custom) 在“BraceWrapping”里配置每一个单独的大括号。

 

BreakBeforeTernaryOperators (bool)

如果为真（true）, 三元运算符将被放置在换行后。

 

BreakConstructorInitializersBeforeComma (bool)

总是在逗号和对齐逗号跟冒号前把构造函数初始化式换行。

 

BreakStringLiterals (bool)

当格式化时，总是对字面量字符串换行。

ColumnLimit (unsigned)

限制列。

列的限制为0意味着没有列限制。在这种情况下，clang-format将谨慎对待在声明中输入行的换行决定，除非与其他规则矛盾。

 

CommentPragmas (std::string)

一个固定的表达式，它描述了具有特殊意义的注释，不应该被分裂成行或以其他方式改变。

 

ConstructorInitializerAllOnOneLineOrOnePerLine (bool)

如果构造函数初始化器不适合在一行，把每个初始化放到单独的行。

 

ConstructorInitializerIndentWidth (unsigned)

使用构造函数初始化列表缩进的字符数。

 

ContinuationIndentWidth (unsigned)

新行缩进宽度。

 

Cpp11BracedListStyle (bool)

如果为真（true）,格式化大括号列表达到最适合c++11列表。

 

重要区别：-没有空格内的大括号列表。-大括号关闭前没有换行。与延续缩进缩进，不与块缩进。

从根本上讲，C++ 11大括号列表与函数调用格式化是一模一样的。如果大括号列表跟着一个名字（例如类型或变量名），clang-format的格式像是一个调用那个名字的函数的圆括号的“{}”。如果没有名称，则假定一个零长度的名称。

 

DerivePointerAlignment (bool)

如果为真（true）, 分析最常见的格式化文件中“&”和“\*”的对齐方式。pointeralignment则仅作为后备。

 

DisableFormat (bool)

完全禁止格式化。

 

ExperimentalAutoDetectBinPacking (bool)

如果为真（true）, clang-format检测函数调用和定义格式化为每行一个参数。

每个调用都可以被包装，每行一个或不确定的。如果是不确定的，例如完全在一行，但需要做出一个决定，clang-format分析文件中是否有其他被包装的事例和相应的行动。

注意：这是一个实验标志，可能会消失或被重命名。不要在配置文件中使用。你自己要为你的使用负责。

 

ForEachMacros (std::vector<std::string>)

一个宏，应解释为foreach循环而不是作为函数调用矢量。

这些都是预期形式的宏：

 

FOREACH(<variable-declaration>, ...)

  <loop-body>

在.clang-format 配置文件中, 这可以被设定为:

ForEachMacros: ['RANGES_FOR', 'FOREACH']

例如: BOOST_FOREACH.

 

IncludeCategories (std::vector<IncludeCategory>)

正则表达式表示不同的#include类别被用于#includes命令。

这些正则表达式与一个包含（包括< >或“）的文件的文件名相匹配。属于第一匹配正则表达式的值被分配，并且#include首先根据增加类别数然后在每个类别按字母的顺序排序。

 

如果正则表达式都不匹配，int_max分配类别。源文件的主要头引用自动获取类别0。因此，它通常是保持在#include开头（http://llvm.org/docs/CodingStandards.html#include-style）。然而，如果你有总是需要排在首位的头引用，你也可以分配负面的优先事项。

为了在.clang-format文件中配置这个, 请使用:

IncludeCategories:

  - Regex:           '^"(llvm|llvm-c|clang|clang-c)/'

    Priority:        2

  - Regex:           '^(<|"(gtest|isl|json)/)'

    Priority:        3

  - Regex:           '.\*'

    Priority:        1

 

IncludeIsMainRegex (std::string)

指定一个常用的可以在文件主要包括映射的正则表达式的表达式。

在猜测是否#include是“main”include（指定类别0，见上文），使用这个正则表达式允许后缀的头引用源。部分匹配完成，所以说：-“”意思是任意后缀，-“$”的意思是没有后缀

例如，如果配置”（_test）？$”，然后.h将被视为包括在a.cc和a_test.ccde中的“main”。

 

IndentCaseLabels (bool)

从switch语句缩进case标签一级。

当错误时，使用相同的缩进级别作为切换语句。switch语句的语句体总是缩进一级以上的case标签。

 

IndentWidth (unsigned)

用于缩进的列数。

 

IndentWrappedFunctionNames (bool)

缩进如果函数定义或声明后包的类型。

 

JavaScriptQuotes (JavaScriptQuoteStyle)

JavaScriptQuoteStyle 使用JavaScript字符串。

可能的值有：

JSQS_Leave (在配置中： Leave) 留下字符串原本的括号

JSQS_Single (在配置中： Single) 总是使用单括号

JSQS_Double (在配置中： Double) 总是使用双括号.

 

KeepEmptyLinesAtTheStartOfBlocks (bool)

如果为真（true）, 保持块的起始空行。

 

Language (LanguageKind)

这种格式针对的是语言。

可能的值有：

LK_None (在配置中： None) 不使用

LK_Cpp (在配置中： Cpp) 应该被用于C, C++, ObjectiveC, ObjectiveC++.

LK_Java (在配置中： Java) 应该被用于Java.

LK_JavaScript (在配置中： JavaScript) 应该被用于 JavaScript.

LK_Proto (在配置中： Proto) 应该被用于 Protocol 缓冲 (https://developers.google.com/protocol-buffers/).

LK_TableGen (在配置中： TableGen) 应该被用于 TableGen 代码.

 

MacroBlockBegin (std::string)

匹配宏指令的一个常用于开始一个块的表达式。

 

MacroBlockEnd (std::string)

匹配宏指令的一个常用于结束一个块的表达式。

 

MaxEmptyLinesToKeep (unsigned)

连续空行的最大数。

 

NamespaceIndentation (NamespaceIndentationKind)

用于命名空间的缩排。

可能的值有：

NI_None (在配置中： None) 在命名空间中不缩进。

NI_Inner (在配置中： Inner) 仅在内部命名空间缩进(嵌套在其他命名空间)。

NI_All (在配置中： All) 在所有的命名空间中缩进。

 

ObjCBlockIndentWidth (unsigned)

OC块中所拍的字符数。

 

ObjCSpaceAfterProperty (bool)

在OC中的@property后面添加一个空格。例如：使用“@property (readonly)”而不是“@property(readonly)”。

 

ObjCSpaceBeforeProtocolList (bool)

在OC协议列表前添加一个空格， 例如： 使用<Protocol>而不是<Protocol>。

 

PenaltyBreakBeforeFirstCallParameter (unsigned)

在调用小括号“（”后给一个方法调用换行的处罚。

 

PenaltyBreakComment (unsigned)

包含在一个注释中的每一个换行的处罚。

 

PenaltyBreakFirstLessLess (unsigned)

在第一个“<<”前的换行的处罚。

 

PenaltyBreakString (unsigned)

包含一个字面量的字符串中的每一个换行的处罚。

 

PenaltyExcessCharacter (unsigned)

每一个字符的列限制外的处罚。

 

PenaltyReturnTypeOnItsOwnLine (unsigned)

把一个方法返回类型放到函数的同一行。

 

PointerAlignment (PointerAlignmentStyle)

指针和引用的对其方式。

可能的值有：

PAS_Left (在配置中： Left) 指针左对齐。

PAS_Right (在配置中： Right) 指针右对齐。

PAS_Middle (在配置中： Middle) 指针中间对齐。

 

ReflowComments (bool)

如果为真（true）, clang-format 将会尝试将注释重新流布局。

 

SortIncludes (bool)

如果为真（true）, clang-format 将会分类#includes.

 

SpaceAfterCStyleCast (bool)

如果为真（true）, 可能在一个C样式描述后插入一个空格。

 

SpaceAfterTemplateKeyword (bool)

如果为真（true）, 在“template”关键字后插入一个空格。

 

SpaceBeforeAssignmentOperators (bool)

如果为假（false），移除分配操作符（=）前空格。

 

SpaceBeforeParens (SpaceBeforeParensOptions)

大括号之前定义空格的情况。

可能的值有：

SBPO_Never (在配置中： Never) 从不在圆括号之前加空格。

SBPO_ControlStatements (在配置中： ControlStatements) 仅在控制声明关键词（for/if/while···）的圆括号前面加空格。

SBPO_Always (在配置中： Always) 总在圆括号前面加空格, 除了语法规则禁止的（在方法中-像宏定义）或者 当其他样式规则定义过的(在一元运算符后,圆括号, 等等)

 

SpaceInEmptyParentheses (bool)

如果为真（true）, 可能会在“（）”中插入空格。

 

SpacesBeforeTrailingComments (unsigned)

单行注释前的空格数(// - comments)。

这不会影响块注释(/* - comments)，因为它们通常会有不同的使用模式和一些特殊情况。

 

SpacesInAngles (bool)

如果为真（true）, 将会在模板对齐列的“<>”中间插入空格。

 

SpacesInCStyleCastParentheses (bool)

如果为真（true）, 将会在C样式描述中插入空格。

 

SpacesInContainerLiterals (bool)

如果为真（true）, 将会在字面量容器中插入空格(例如 OC和Javascript的数组和字典字面量)。

 

SpacesInParentheses (bool)

如果为真（true）, 将会在“(”之后和“)”之前插入空格。

 

SpacesInSquareBrackets (bool)

如果为真（true）,将会在“[”之后和“]”之前插入空格。

 

Standard (LanguageStandard)

用这个标准格式化：例如：在LS_Cpp03中使用 A<A<int> > 而不是 A<A<int>>

可能的值有：

LS_Cpp03 (在配置中： Cpp03) 使用Use C++03统一语法。

LS_Cpp11 (在配置中： Cpp11) 使用C++11的特征(例如 A<A<int>>而不是A<A<int> >).

LS_Auto (在配置中： Auto) 基于输入自动检查。

TabWidth (unsigned)

用于制表符停止的列数。

 

UseTab (UseTabStyle)

在结果文件中使用制表符字符的方式。

可能的值有：

UT_Never (在配置中： Never) 从不使用制表符。

UT_ForIndentation (在配置中： ForIndentation) 仅缩排时使用制表符。

UT_Always (在配置中： Always) 使用标签时，我们需要填补的空白，至少从一个制表位到下一个。

```



配置文件位置`~/.clang-format`



>我的配置文件



```

#基于那个配置文件

BasedOnStyle: LLVM

#指针的*的挨着哪边

PointerAlignment: Right

#缩进宽度

IndentWidth: 4

# 连续的空行保留几行

MaxEmptyLinesToKeep: 1

# 在 @property 后面添加空格, \@property (readonly) 而不是 \@property(readonly).

ObjCSpaceAfterProperty: true

# OC block后面的缩进

ObjCBlockIndentWidth: 4

# 是否允许短方法单行

AllowShortFunctionsOnASingleLine: true

# 是否允许短if单行 If true, if (a) return; 可以放到同一行

AllowShortIfStatementsOnASingleLine: true

#注释对齐

AlignTrailingComments: true

# 换行的时候对齐操作符

#AlignOperands: true

# 中括号两边空格 [] 

SpacesInSquareBrackets: true

# 小括号两边添加空格

SpacesInParentheses : false

#多行声明语句按照=对齐

AlignConsecutiveDeclarations: true

#连续的赋值语句以 = 为中心对齐

AlignConsecutiveAssignments: true

#等号两边的空格

SpaceBeforeAssignmentOperators: true

# 容器类的空格 例如 OC的字典

SpacesInContainerLiterals: true

#缩进

IndentWrappedFunctionNames: true

#在block从空行开始

KeepEmptyLinesAtTheStartOfBlocks: true

#在构造函数初始化时按逗号断行，并以冒号对齐

BreakConstructorInitializersBeforeComma: true

#函数参数换行

AllowAllParametersOfDeclarationOnNextLine: true

#括号后添加空格

SpaceAfterCStyleCast: true

#tab键盘的宽度

TabWidth: 4

UseTab: Never



```



**此时打开vim，退出编辑模式时会自动格式化代码**
    