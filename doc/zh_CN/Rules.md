
# 规则

本文档包含对所有规则的描述，它们的检查对象，以及一些违反规则的文档示例和修正后的文档示例。

<a name="md001"></a>

## MD001 - 标题应当逐级使用

标签：标题

别名：多级标题

当你在markdown文档中越级使用标题会触发本条规则，实例如下：

```markdown
# 标题1

### 标题3

我们在该文档中跳过了二级标题
```

当我们使用多级标题的时候，嵌套的标题应当每次逐级增加：

```markdown
# 标题1

## 标题2

### 标题3

#### 标题4

## 另一个标题2

### 另一个标题3
```

<a name="md002"></a>

## MD002 - 第一个标题应当是一级标题

标签：标题

别名：首标题-h1

参数: level (number; 默认为1)

> 注意: *MD002现已被弃用并默认关闭。*
> [MD041/首行标题](#md041) 提供了一个改进实现.

该规则旨在确保文档以一级标题为开始，并当文档的第一个标题不是h1标题时被触发：

```markdown
## 这不是一个H1标题

### 另一个标题
```

文档中的第一个标题应该是一个h1标题：

```markdown
# 以一个H1标题开始

## 然后为分段使用一个H2标题
```

注意：`level`参数可以用来改变首级标题（例：更改为h2），以防止h1在外边被添加。

<a name="md003"></a>

## MD003 - 标题样式

标签：标题

别名：标题样式

参数：style ("consistent", "atx", "atx_closed", "setext",
"setext_with_atx", "setext_with_atx_closed"; default "consistent")

当在同一个文档中，使用不同的标题样式（atx，setext，以及‘closed’ atx）会触发该规则：

```markdown
# ATX样式的H1标题

## Closed ATX样式的H2标题 ##

Setext样式的H1标题
===============
```

在同一个文档中，使用一致的标题样式：

```markdown
# ATX样式的H1标题

## ATX样式的H2标题
```

setext_with_atx和setext_with_atx_closed文档样式允许在文档中使用带有setext样式标题的atx-style的3级标题或者更多的标题：

```markdown
Setext样式的H1标题
===============

Setext样式的H2标题
---------------

### ATX样式的H3标题
```

注意：配置标题样式可以使用某个特定的样式（atx，atx_closed，setext，setext_with_atx，setext_with_atx_closed），或者简单地要求在一个文档内使用一致的样式。

<a name="md004"></a>

## MD004 - 无序列表样式

标签：项目符号，无序列表

别名：ul-style

参数：style ("consistent", "asterisk", "plus", "dash", "sublist"; default
"consistent")

在文档中，当无序列表项目使用的符号与配置的无序列表样式不符时触发该条规则：

```markdown
* 项目1
+ 项目2
- 项目3
```

通过在文档中使用已配置的列表项样式修复本问题：

```markdown
* 项目1
* 项目2
* 项目3
```

列表项的配置样式可以使用一种特定的符号（星号，加号，破折号），可以要求在文档中使用一致的符号，或者子列表内部使用一致的，且与其父列表不同符号：

例如，下面子列表使用的样式就是有效的，因为最外侧缩进的项使用星号，中间缩进的项目使用加号，以及最内侧的项目使用破折号：

```markdown
* 项目1
  + 项目2
    - 项目3
  + 项目4
* 项目4
  + 项目5
```

<a name="md005"></a>

## MD005 - 同级列表项中的不一致缩进

标签：列表项目，无序列表，缩进

别名：列表缩进

当列表项被解析为同一级，但没有使用一致的缩进时，触发该规则：

```markdown
* 项目1
  * 嵌套的项目1
  * 嵌套的项目2
   * 一个编排不整齐的项目
```

通常该规则由于打字错误被触发。修正列表的缩进以解决该问题：

```markdown
* 项目1
  * 嵌套的项目1
  * 嵌套的项目2
  * 嵌套的项目3
```

顺序列表的标记通常是左对齐，使得所有的项目是相同的起始列：

```markdown
...
8. 项目
9. 项目
10. 项目
11. 项目
...
```

本规则也支持列表符号的右对齐，使得所有的项目是相同的列结束：

```markdown
...
 8. 项目
 9. 项目
10. 项目
11. 项目
...
```

<a name="md006"></a>

## MD006 - 考虑将无序列表的起始位置放在行首

标签：着重号，无序列表，缩进

别名：无序列表起始于左侧

当顶级列表没有以行首为开始时触发该规则：

```markdown
一些文本

  * 列表项
  * 列表项
```

修复该问题，确保顶级列表项目没有缩进：

```markdown
一些文本

* 列表项目
* 列表项目
```

原理：在行首起始一个列表意味着当使用编辑器的缩进函数或者tab键缩进时，嵌套的列表项目可以缩进相同的数目。一个列表的起始缩进一个空格意味着第一个嵌套的列表的缩进少于第二级的列表（当使用4空格的tabs时是3个字符，或者使用2空格tabs时是1个空格）。

注意：该规则也会在下面的场景被触发，因为无序子列表无法被解析器识别。外边的有序列表也要求不要嵌套3个字符，它反而创建了一个顶级的无序列表。

```markdown
1. 列表项
  - 列表项
  - 列表项
1. 列表项
```

<a name="md007"></a>

## MD007 - 无序列表缩进

标签：着重号，无序列表，缩进

别名：ul-indent

参数：indent (number; 默认为2)

当列表项没有按照配置的空格数（默认为2）进行缩进时触发该规则。

例如：

```markdown
* 列表项
   * 嵌套列表项缩进了3个空格
```

修正后的例子：

```markdown
* 列表项
  * 嵌套列表项缩进了3个空格
```

原理（2空格缩进）：当在列表符号后使用单个空格时，使用2个空格的缩进能够允许嵌套列表的内容与父列表内容的开始保持一致。

原理（4空格缩进）：诸如code block是中使用这样的缩进，编辑器更易实现。参见<https://www.cirosantilli.com/markdown-style-guide/#indentation-of-content-inside-lists>获得更多信息。

此外，要求4空格缩进是一个多种markdown解析器之间的兼容问题。参见<http://support.markedapp.com/discussions/problems/21-sub-lists-not-indenting>获得问题描述。

注意：本规则只有在父列表全部是无序时才会应用到子列表（否则，有序列表的额外缩进会与该规则冲突）。

<a name="md009"></a>

## MD009 - 后续空格

标签：空格

别名：禁止后续空格

参数：br_spaces, list_item_empty_lines (number; 默认为2, boolean; 默认为false)

当任何行以一场的空格结束时，触发该规则。从行尾删除后续的空格以修复该问题。

参数`br_spaces`允许在为该规则设置例外的特定数目的后续空格，典型的是用于插入一个明确的换行标志。默认参数使用2个空格表示一个硬换行（\<br>元素）。

注意：你必须将`br_spaces`的值设置为>=2使得该参数生效。将`br_spaces`设置为1时变现与0相同，即禁止任何的后续空格。

在一个列表项中使用空格编排一个空白行通常是不必要的，但是一些解析器需要这样做。将参数`list_item_empty_lines`设置为`true`允许这样的情况：

```markdown
- 列表项文本
  [2个空格]
  列表项文本
```

<a name="md010"></a>

## MD010 - 硬tabs

标签：空格，硬tabs

别名：禁止硬tabs

参数：code_blocks (boolean; 默认为true)

当任一行使用硬tabs字符而不是空格进行缩进时，会触发该规则。使用空格来替换硬tabs字符以修复该问题。

例如：

```markdown
一些文本

	* 对列表项使用硬tabs字符进行缩进
```

修正后的例子：

```markdown
一些文本

    * 使用空格对列表项进行缩进
```

你可以选择为代码块排除这条规则。将`code_blocks`参数设置为`false`以实现目的。代码块默认被包括在内，因为工具对tabs的处理经常是不一致的（例：使用4空格 vs. 使用8空格）。

<a name="md011"></a>

## MD011 - 链接语法颠倒

标签：链接

别名：禁止颠倒链接

在文本中使用了链接，但是语法看起来被颠倒了（`[]`和`()`的顺序颠倒了），就会触发这条规则：

```markdown
(不正确的链接语法)[https://www.example.com/]
```

修复这个问题，交换`[]`和`()`的顺序：

```markdown
[正确的链接语法](https://www.example.com/)
```

注意：[Markdown Extra](https://en.wikipedia.org/wiki/Markdown_Extra)-样式的脚注不会触发该规则：

```markdown
For (example)[^1]
```

<a name="md012"></a>

## MD012 - 多个连续空白行

标签：空格，空白行

别名：禁止多个空白行

参数：maximum (number; 默认1)

当文档中有连续的多个空白行会触发这个规则：

```markdown
这里是一些文本


这里是一些更多的文本
```

删除非法的行以修正该问题：

```markdown
这里是一些文本

这里是一些更多的文本
```

注意：代码块中存在的连续空白行不会触发这条规则。

注意：参数`maximum`能够用来配置连续空白行的最大数目。

<a name="md013"></a>

## MD013 - 行长度

标签：line_length

别名：line-length

参数：line_length, heading_line_length, code_block_line_length, code_blocks, tables, headings, headers (number; 默认80, boolean; 默认true)

> 如果没有提供`headings`，则会使用`headers`（已弃用）。

当有行的长度超出配置的`line_length`参数（默认：80字符）会触发该规则。
将一行分割成多行以修正该问题。
使用`heading_line_length`参数为标题设置不同的最大长度。
使用`code_block_line_length`参数为代码块设置不同的最大长度。

当某行没有空格且超出了配置的行的长度，这种情况是该规则的一个例外。
这使你不需要对诸如长URLs等项目进行从中间进行换行。

你可以选择将代码块、表格或者标题排除在该规则之外。
将`code_blocks`、`tables`或`headings`参数设置为false以实现。

代码块默认被包含在该规则中，因为这通常是文档可读性的要求，暂时与代码规则相兼容。
然而一些语言并不合适于短行。

<a name="md014"></a>

## MD014 - 没有显示输出而在命令前使用美元符号

标签：code

别名：commands-show-output

当在代码块中展示输入的shell命令，并且shell命令被美元符号（$）引领着，会触发该规则：

```markdown
$ ls
$ cat foo
$ less bar
```

上述情况的美元符号不是必要的，应当不被包含：

```markdown
ls
cat foo
less bar
```

然而，当需要区分输入的命令与命令的输出时，是一个例外的情况，如下面的例子：

```markdown
$ ls
foo bar
$ cat foo
Hello world
$ cat bar
baz
```

原理：当美元符号不必要时被忽略，可以更易复制粘贴以及阅读。
参见<https://www.cirosantilli.com/markdown-style-guide/#dollar-signs-in-shell-code>获得更多信息。

<a name="md018"></a>

## MD018 - 在atx风格的标题中hash之后没有空格

标签：headings, headers, atx, spaces

别名：no-missing-space-atx

在atx风格的标题中，当hash字符后缺少空格时会触发这条规则：

```markdown
#标题1

##标题2
```

使用单空格将标题文本与hash字符分离以修正该问题：

```markdown
# 标题1

## 标题2
```

<a name="md019"></a>

## MD019 - 在atx风格的标题中的hash后使用了多个空格

标签：headings, headers, atx, spaces

别名：no-multiple-space-atx

在atx风格的标题中，使用多个空格去分割标题文本和hash字符时会触发这条规则：

```markdown
#  标题1

##  标题2
```

用一个空格来分割标题文本和hash字符以修正该问题：

```markdown
# 标题1

## 标题2
```

<a name="md020"></a>

## MD020 - 闭合atx风格标题hashes内没有空格

标签：headings, headers, atx_closed, spaces

别名：no-missing-space-closed-atx

当闭合atx风格标题内的hashes缺失了空格时会触发这条规则：

```markdown
#标题1#

##标题2##
```

使用一个空格来分隔标题文本与hash字符以修正该问题：

```markdown
# 标题1 #

## 标题2 ##
```

注意：标题的任何一边缺失空格都会触发该规则。

<a name="md021"></a>

## MD021 - 闭合atx风格标题hashes内有多个空格

标签：headings, headers, atx_closed, spaces

别名：no-multiple-space-closed-atx

当闭合atx风格的标题中使用了多个空格来分隔标题文本与hash字符时会触发这条规则：

```markdown
#  标题1  #

##  标题2  ##
```

使用单个空格来分隔标题文本与hash字符以修复这个问题：

```markdown
# 标题1 #

## 标题2 ##
```

注意：标题的任何一边包含多个空格都会触发该规则。

<a name="md022"></a>

## MD022 - 标题应当被空行包围

标签：headings, headers, blank_lines

别名：blanks-around-headings, blanks-around-headers

参数：lines_above, lines_below (number; 默认1)

当标题行（任何样式）的前一行或者后一行没有至少一个空行时触发该规则：

```markdown
# 标题1
一些文本

一些更多的文本
## 标题2
```

确保标题的前面和后面都有一个空行（除非标题位于文档的开始处或者结束处）以修正该问题：

```markdown
# 标题1

一些文本

一些更多的文本

## 标题2
```

原理：除去一些审美的原因外，一些解析器，包括kramdown，如果标题后没有空行会无法被解析，并且会被解析成常规文本。

参数`lines_above`和`lines_below`可以用来指定标题行前和行后的空行数目（包括0）。

注意：如果参数`lines_above`或`lines_below`被修改成需要多于一个空行时，
[MD012/no-multiple-blanks](#md012)也应当被修改。

<a name="md023"></a>

## MD023 - 标题应当在行首开始

标签：headings, headers, spaces

别名：heading-start-left, header-start-left

当标题被一个或多个空格进行缩进时会触发该规则：

```markdown
一些文本

  # 缩进的标题
```

确保所有的标题都在行首开始以修正该问题：

```markdown
一些文本

# 标题
```

原理：标题不在行首开始会使得其无法被解析为标题，并且反而会被识别为普通文本。

<a name="md024"></a>

## MD024 - 多个标题有相同内容

标签：headings, headers

别名：no-duplicate-heading, no-duplicate-header

参数：siblings_only, allow_different_nesting (boolean; 默认`false`)

当文档中多个标题的文本内容一样时会触发该规则：

```markdown
# 一些文本

## 一些文本
```

确保每个标题的内容不一样以修正该问题：

```markdown
# 一些文本

## 一些更多的文本
```

原理：一些markdown解析器基于标题名为标题生成锚点，
标题拥有相同的内容会因此造成一些问题。

如果参数`siblings_only`（或者`allow_different_nesting`）设置为`true`，
就允许非同胞标题使用相同标题（通常在修改日志中使用）：

```markdown
# 修改日志

## 1.0.0

### Features

## 2.0.0

### Features
```

<a name="md025"></a>

## MD025 - 同一个文档中有多个顶级标题

标签：headings, headers

别名：single-title, single-h1

参数：level, front_matter_title (number; 默认 1, string; 默认 "^\s*title:")

当一个顶级标题在使用中（文件的第一行是一个h1标题），并且文档中还有更多的h1标题时，
会触发这条规则:

```markdown
# 顶级标题

# 另一个顶级标题
```

组织你的文档以使得文档只有一个h1标题作为题目，后面的标题使用h2级或者更低级的标题，以修正该问题：

```markdown
# 题目

## 标题

## 另一个标题
```

原理：顶级标题是文件的第一行的h1标题，并被作为文档的题目。
如果这个惯例被使用，文档中就不能有多个题目，整个文档应该被这个标题所包含。

注意：参数`level`可以被用来修改顶级标题（例如：修改为h2）以防止h1在外部被添加。

如果[YAML](https://en.wikipedia.org/wiki/YAML)前面包含一个`title`属性（通常使用在博客中）而面临问题，
这条规则会将其视为顶级标题并会对其后的顶级标题报出警告。
在前面使用不同属性名，使用属性`front_matter_title`修改正则表达式文本。
将`front_matter_title`修改为`""`以关闭该规则。

<a name="md026"></a>

## MD026 - 标题后续的标点符号

标签：headings, headers

别名：no-trailing-punctuation

参数：punctuation (string; 默认 ".,;:!?。，；：！？")

任何标题所在行的最后一个字符是一个普通或者全角字符时触发该规则：

```markdown
# This is a heading.
```

移除最后的任何标点符号以修正该问题：

```markdown
# This is a heading
```

注意：标点参数可以用来修改哪些字符在标题的末尾可以被分为标点符号。
例如，你可以将该参数设置为`".,;:!"`以允许标题中使用问号，比如在FAQ中使用。

<a name="md027"></a>

## MD027 - 块引用符号后有多个空格

标签：blockquote, whitespace, indentation

别名：no-multiple-space-blockquote

当块引用中的引用符号（`>`）后有不止一个空格时触发该规则：

```markdown
>  这是一个错误不好的块引用缩进
>  应当只有一个空格。
```

移除多余的空格以修正该问题：

```markdown
> 这是正确的块引用
> 缩进。
```

<a name="md028"></a>

## MD028 - 块引用内的空行

标签：blockquote, whitespace

别名：no-blanks-blockquote

当两个引用块仅被一个空行分隔时触发该规则：

```markdown
> 这是一个块引用
> 其后跟随着

> 这个块引用。可惜
> 在一些解析器中，它们会被视为同一个引用块。
```

确保任何相邻的块引用中间有一些文本，以修正该问题：


```markdown
> 这是一个块引用。

并且Jimmy也说：

> 这也是一个块引用。
```

或者，如果它们应该是同一个引用块，在空行的开始加上一个引用符号：

```markdown
> 这是一个块引用。
>
> 这是同一个块引用。
```

原理：一些markdown解析器会将被一个或多个空行分隔的两个块引用视为同一个引用块，
而另一些将它们视为不同的引用块。

<a name="md029"></a>

## MD029 - 有序列表项前缀

标签：ol

别名：ol-prefix

参数：style ("one", "ordered", "one_or_ordered", "zero"; 默认 "one_or_ordered")

当列表项要么没有以‘1.’开始，要么没有使用递增有序数字的前缀（取决于配置的样式）时，触发该规则。
一种少见的为所有项目使用前缀‘0.’的样式也是被支持的。

如果样式被配置为‘one’时，例子中的列表是有效的：

```markdown
1. 做这个。
1. 做那个。
1. 完成。
```

如果样式被配置为‘ordered’时，例子中的列表是有效的：

```markdown
1. 做这个。
2. 做那个。
3. 完成。
```

当样式被配置为‘one_or_ordered’时，上面的两个例子都是有效的。

如果样式被配置为‘zero’时，例子中的列表是有效的：

```markdown
0. 做这个。
0. 做那个。
0. 完成。
```

在所有样式中，下面的例子都是无效的：

```markdown
1. Do this.
3. Done.
```

本规则支持为0-前缀的有序列表项使用统一的缩进：

```markdown
...
08. 项目
09. 项目
10. 项目
11. 项目
...
```

<a name="md030"></a>

## MD030 - Spaces after list markers

Tags: ol, ul, whitespace

Aliases: list-marker-space

Parameters: ul_single, ol_single, ul_multi, ol_multi (number; default 1)

This rule checks for the number of spaces between a list marker (e.g. '`-`',
'`*`', '`+`' or '`1.`') and the text of the list item.

The number of spaces checked for depends on the document style in use, but the
default is 1 space after any list marker:

```markdown
* Foo
* Bar
* Baz

1. Foo
1. Bar
1. Baz

1. Foo
   * Bar
1. Baz
```

A document style may change the number of spaces after unordered list items
and ordered list items independently, as well as based on whether the content
of every item in the list consists of a single paragraph, or multiple
paragraphs (including sub-lists and code blocks).

For example, the style guide at
<https://www.cirosantilli.com/markdown-style-guide/#spaces-after-list-marker>
specifies that 1 space after the list marker should be used if every item in
the list fits within a single paragraph, but to use 2 or 3 spaces (for ordered
and unordered lists respectively) if there are multiple paragraphs of content
inside the list:

```markdown
* Foo
* Bar
* Baz
```

vs.

```markdown
*   Foo

    Second paragraph

*   Bar
```

or

```markdown
1.  Foo

    Second paragraph

1.  Bar
```

To fix this, ensure the correct number of spaces are used after list marker
for your selected document style.

<a name="md031"></a>

## MD031 - Fenced code blocks should be surrounded by blank lines

Tags: code, blank_lines

Aliases: blanks-around-fences

This rule is triggered when fenced code blocks are either not preceded or not
followed by a blank line:

````markdown
Some text
```
Code block
```

```
Another code block
```
Some more text
````

To fix this, ensure that all fenced code blocks have a blank line both before
and after (except where the block is at the beginning or end of the document):

````markdown
Some text

```
Code block
```

```
Another code block
```

Some more text
````

Rationale: Aside from aesthetic reasons, some parsers, including kramdown, will
not parse fenced code blocks that don't have blank lines before and after them.

<a name="md032"></a>

## MD032 - Lists should be surrounded by blank lines

Tags: bullet, ul, ol, blank_lines

Aliases: blanks-around-lists

This rule is triggered when lists (of any kind) are either not preceded or not
followed by a blank line:

```markdown
Some text
* Some
* List

1. Some
2. List
Some text
```

To fix this, ensure that all lists have a blank line both before and after
(except where the block is at the beginning or end of the document):

```markdown
Some text

* Some
* List

1. Some
2. List

Some text
```

Rationale: Aside from aesthetic reasons, some parsers, including kramdown, will
not parse lists that don't have blank lines before and after them.

Note: List items without hanging indents are a violation of this rule; list
items with hanging indents are okay:

```markdown
* This is
not okay

* This is
  okay
```

<a name="md033"></a>

## MD033 - Inline HTML

Tags: html

Aliases: no-inline-html

Parameters: allowed_elements (array of string; default empty)

This rule is triggered whenever raw HTML is used in a markdown document:

```markdown
<h1>Inline HTML heading</h1>
```

To fix this, use 'pure' markdown instead of including raw HTML:

```markdown
# Markdown heading
```

Rationale: Raw HTML is allowed in markdown, but this rule is included for
those who want their documents to only include "pure" markdown, or for those
who are rendering markdown documents in something other than HTML.

Note: To allow specific HTML elements, use the 'allowed_elements' parameter.

<a name="md034"></a>

## MD034 - Bare URL used

Tags: links, url

Aliases: no-bare-urls

This rule is triggered whenever a URL is given that isn't surrounded by angle
brackets:

```markdown
For more information, see https://www.example.com/.
```

To fix this, add angle brackets around the URL:

```markdown
For more information, see <https://www.example.com/>.
```

Rationale: Without angle brackets, the URL isn't converted into a link in many
markdown parsers.

Note: if you do want a bare URL without it being converted into a link,
enclose it in a code block, otherwise in some markdown parsers it _will_ be
converted:

```markdown
`https://www.example.com`
```

<a name="md035"></a>

## MD035 - Horizontal rule style

Tags: hr

Aliases: hr-style

Parameters: style ("consistent", "---", "***", or other string specifying the
horizontal rule; default "consistent")

This rule is triggered when inconsistent styles of horizontal rules are used
in the document:

```markdown
---

- - -

***

* * *

****
```

To fix this, ensure any horizontal rules used in the document are consistent,
or match the given style if the rule is so configured:

```markdown
---

---
```

Note: by default, this rule is configured to just require that all horizontal
rules in the document are the same, and will trigger if any of the horizontal
rules are different than the first one encountered in the document. If you
want to configure the rule to match a specific style, the parameter given to
the 'style' option is a string containing the exact horizontal rule text that
is allowed.

<a name="md036"></a>

## MD036 - Emphasis used instead of a heading

Tags: headings, headers, emphasis

Aliases: no-emphasis-as-heading, no-emphasis-as-header

Parameters: punctuation (string; default ".,;:!?。，；：！？")

This check looks for instances where emphasized (i.e. bold or italic) text is
used to separate sections, where a heading should be used instead:

```markdown
**My document**

Lorem ipsum dolor sit amet...

_Another section_

Consectetur adipiscing elit, sed do eiusmod.
```

To fix this, use markdown headings instead of emphasized text to denote
sections:

```markdown
# My document

Lorem ipsum dolor sit amet...

## Another section

Consectetur adipiscing elit, sed do eiusmod.
```

Note: This rule looks for single line paragraphs that consist entirely
of emphasized text. It won't fire on emphasis used within regular text,
multi-line emphasized paragraphs, or paragraphs ending in punctuation
(normal or full-width). Similarly to rule MD026, you can configure what
characters are recognized as punctuation.

<a name="md037"></a>

## MD037 - Spaces inside emphasis markers

Tags: whitespace, emphasis

Aliases: no-space-in-emphasis

This rule is triggered when emphasis markers (bold, italic) are used, but they
have spaces between the markers and the text:

```markdown
Here is some ** bold ** text.

Here is some * italic * text.

Here is some more __ bold __ text.

Here is some more _ italic _ text.
```

To fix this, remove the spaces around the emphasis markers:

```markdown
Here is some **bold** text.

Here is some *italic* text.

Here is some more __bold__ text.

Here is some more _italic_ text.
```

Rationale: Emphasis is only parsed as such when the asterisks/underscores
aren't completely surrounded by spaces. This rule attempts to detect where
they were surrounded by spaces, but it appears that emphasized text was
intended by the author.

<a name="md038"></a>

## MD038 - Spaces inside code span elements

Tags: whitespace, code

Aliases: no-space-in-code

This rule is triggered on code span elements that have spaces right inside the
backticks:

```markdown
` some text `

`some text `

` some text`
```

To fix this, remove the spaces inside the codespan markers:

```markdown
`some text`
```

Note: A single leading or trailing space is allowed if used to separate codespan
markers from an embedded backtick:

```markdown
`` ` embedded backtick``
```

<a name="md039"></a>

## MD039 - Spaces inside link text

Tags: whitespace, links

Aliases: no-space-in-links

This rule is triggered on links that have spaces surrounding the link text:

```markdown
[ a link ](https://www.example.com/)
```

To fix this, remove the spaces surrounding the link text:

```markdown
[a link](https://www.example.com/)
```

<a name="md040"></a>

## MD040 - Fenced code blocks should have a language specified

Tags: code, language

Aliases: fenced-code-language

This rule is triggered when fenced code blocks are used, but a language isn't
specified:

````markdown
```
#!/bin/bash
echo Hello world
```
````

To fix this, add a language specifier to the code block:

````markdown
```bash
#!/bin/bash
echo Hello world
```
````

<a name="md041"></a>

## MD041 - First line in file should be a top level heading

Tags: headings, headers

Aliases: first-line-heading, first-line-h1

Parameters: level, front_matter_title (number; default 1, string; default "^\s*title:")

This rule is intended to ensure documents have a title and is triggered when
the first line in the file isn't a top level (h1) heading:

```markdown
This is a file without a heading
```

To fix this, add a top level heading to the beginning of the file:

```markdown
# File with heading

This is a file with a top level heading
```

Note: The `level` parameter can be used to change the top level (ex: to h2) in cases
where an h1 is added externally.

If [YAML](https://en.wikipedia.org/wiki/YAML) front matter is present and contains a
`title` property (commonly used with blog posts), this rule will not report a
violation. To use a different property name in front matter, specify the text
of a regular expression via the `front_matter_title` parameter. To disable the
use of front matter by this rule, specify `""` for `front_matter_title`.

<a name="md042"></a>

## MD042 - No empty links

Tags: links

Aliases: no-empty-links

This rule is triggered when an empty link is encountered:

```markdown
[an empty link]()
```

To fix the violation, provide a destination for the link:

```markdown
[a valid link](https://example.com/)
```

Empty fragments will trigger this rule:

```markdown
[an empty fragment](#)
```

But non-empty fragments will not:

```markdown
[a valid fragment](#fragment)
```

<a name="md043"></a>

## MD043 - Required heading structure

Tags: headings, headers

Aliases: required-headings, required-headers

Parameters: headings, headers (array of string; default `null` for disabled)

> If `headings` is not provided, `headers` (deprecated) will be used.

This rule is triggered when the headings in a file do not match the array of
headings passed to the rule. It can be used to enforce a standard heading
structure for a set of files.

To require exactly the following structure:

```markdown
# Head
## Item
### Detail
```

Set the `headings` parameter to:

```json
[
    "# Head",
    "## Item",
    "### Detail"
]
```

To allow optional headings as with the following structure:

```markdown
# Head
## Item
### Detail (optional)
## Foot
### Notes (optional)
```

Use the special value `"*"` meaning "one or more unspecified headings" and set
the `headings` parameter to:

```json
[
    "# Head",
    "## Item",
    "*",
    "## Foot",
    "*"
]
```

When an error is detected, this rule outputs the line number of the first
problematic heading (otherwise, it outputs the last line number of the file).

Note that while the `headings` parameter uses the "## Text" ATX heading style for
simplicity, a file may use any supported heading style.

<a name="md044"></a>

## MD044 - Proper names should have the correct capitalization

Tags: spelling

Aliases: proper-names

Parameters: names, code_blocks (string array; default `null`, boolean; default `true`)

This rule is triggered when any of the strings in the `names` array do not have
the specified capitalization. It can be used to enforce a standard letter case
for the names of projects and products.

For example, the language "JavaScript" is usually written with both the 'J' and
'S' capitalized - though sometimes the 's' or 'j' appear in lower-case. To enforce
the proper capitalization, specify the desired letter case in the `names` array:

```json
[
    "JavaScript"
]
```

Set the `code_blocks` parameter to `false` to disable this rule for code blocks.

<a name="md045"></a>

## MD045 - Images should have alternate text (alt text)

Tags: accessibility, images

Aliases: no-alt-text

This rule is triggered when an image is missing alternate text (alt text) information.
Alternate text is important for accessibility, describing the content of an image for
people who may not be able to see it.

Alternate text is commonly specified inline as:

```markdown
![Alternate text](image.jpg)
```

Or with reference syntax as:

```markdown
![Alternate text][ref]

...

[ref]: image.jpg "Optional title"
```

Guidance for writing alternate text is available from the [W3C](https://www.w3.org/WAI/alt/),
[Wikipedia](https://en.wikipedia.org/wiki/Alt_attribute), and
[other locations](https://www.phase2technology.com/blog/no-more-excuses-definitive-guide-alt-text-field).

<a name="md046"></a>

## MD046 - Code block style

Tags: code

Aliases: code-block-style

Parameters: style ("consistent", "fenced", "indented"; default "consistent")

This rule is triggered when unwanted or different code block styles are used in
the same document.

In the default configuration this rule reports a violation for the following document:

    Some text.

        # Indented code

    More text.

    ```ruby
    # Fenced code
    ```

    More text.

To fix violations of this rule, use a consistent style (either indenting or code fences).

The specified style can be specific (`fenced`, `indented`) or simply require that usage
be consistent within the document (`consistent`).

<a name="md047"></a>

## MD047 - 文件应当以一个单独的换行符为结束

标签：blank_lines

别名：single-trailing-newline

当文件的结束处没有一个单独的换行符时触发该规则。

触发改规则的例子：

```markdown
# 标题

这个文件没有以换行符为结束。[EOF]
```

为文件的最后新增一个换行符以修正该问题：

```markdown
# 标题

这个文件以一个换行符为结束。
[EOF]
```
