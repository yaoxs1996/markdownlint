
# 规则

本文档包含对所有规则的描述，它们的检查对象，以及一些违反规则的文档示例和修正后的文档示例。

<a name="md001"></a>

## MD001 - 标题应当逐级使用

标签：headings, headers

别名：heading-increment, header-increment

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

标签：headings, headerd

别名：first-headings-h1, first-header-h1

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

标签：headings, headers

别名：heading-style, header-style

参数：style ("consistent", "atx", "atx_closed", "setext",
"setext_with_atx", "setext_with_atx_closed"; 默认 "consistent")

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

标签：bullet, ul

别名：ul-style

参数：style ("consistent", "asterisk", "plus", "dash", "sublist"; 默认
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

标签：bullet, ul, indentation

别名：list-indent

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

标签：bullet, ul, indentation

别名：ul-start-left

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

标签：bullet, ul, indentation

别名：ul-indent

参数：indent (number; 默认 2)

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

标签：whitespace

别名：no-trailing-spaces

参数：br_spaces, list_item_empty_lines (number; 默认 2, boolean; 默认 false)

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

标签：whitespace, hard_tab

别名：no-hard-tabs

参数：code_blocks (boolean; 默认 true)

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

标签：links

别名：no-reversed-links

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

标签：whitespace, blank_lines

别名：no-multiple-blanks

参数：maximum (number; 默认 1)

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

## MD030 - 列表符号后的空格

标签：ol, ul, whitespace

别名：list-marker-space

参数：ul_single, ol_single, ul_multi, ol_multi (number; 默认 1)

该规则检查列表符号（例如：‘`-`’，‘`*`’，‘`+`’或者‘`1.`’）与列表项文本间的空格数量。

检查的空格数量取决于使用中的文档配置，默认为列表符号后使用1个空格：

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

文档配置可以分别独立地修改无序列表和有序列表后的空格数量，
同样也基于每个列表项的内容是否包括单个或多个段落（包括子列表和代码块）。

例如，<https://www.cirosantilli.com/markdown-style-guide/#spaces-after-list-marker>
中的配置指南指出，如果列表中的每个列表项是一个单独的段落，应该为列表符号使用1个空格，
如果列表中的内容为多个段落，应该使用2个或者3个空格（分别对应有序列表和无序列表）：

```markdown
* Foo
* Bar
* Baz
```

对比

```markdown
*   Foo

    Second paragraph

*   Bar
```

或者

```markdown
1.  Foo

    Second paragraph

1.  Bar
```

根据你所选择的文档配置，确保列表符号后使用了正确数量的空格以修正该问题。

<a name="md031"></a>

## MD031 - 代码块应该被空行包围

标签：code, blank_lines

别名：blanks-around-fences

当代码块的前面或者后面没有空行时触发该规则：

````markdown
一些文本
```
代码块
```

```
另一个代码块
```
一些更多的文本
````

确保所有的代码块的前后都有空行（除非代码块位于文档的开始或结束位置）以修复该问题：

````markdown
一些文本

```
代码块
```

```
另一个代码块
```

一些更多的文本
````

原理：除了一些审美原因外，一些解析器，包括kramdown，如果代码块的前后没有空行就无法被解析。

<a name="md032"></a>

## MD032 - 列表项应该被空行包围

标签：bullet, ul, ol, blank_lines

别名：blanks-around-lists

当列表（任何种类）的前面或者后面都没有空行时，触发该规则：

```markdown
一些文本
* 一些
* 列表

1. 一些
2. 列表
一些文本
```

确保所有的列表的前后都有空行（除非列表位于文档的开始或者结束位置）以修正给问题：

```markdown
一些文本

* 一些
* 列表

1. 一些
2. 列表

一些文本
```

原理：除了审美的原因外，一些解析器，包括kramdown，如果列表的前后没有空行时会无法被解析。

注意：列表项没有悬挂缩进是对该规则的违反；列表项应该悬挂缩进：

```markdown
* 这
不行

* 这
  可以
```

<a name="md033"></a>

## MD033 - 内联HTML

标签：html

别名：no-inline-html

参数allowed_elements (array of string; 默认为空)

无论何时在markdown文档中使用原生HTML时触发该规则：

```markdown
<h1>内联的HTML标题</h1>
```

使用纯净的markdown代替包含原生HTML以修正该问题：

```markdown
# Markdown标题
```

原理：markdown允许使用原生HTML，但是该规则适合于那些希望他们的文档中只包含“纯净”的markdown，
或者将markdown文档转换成非HTML的用户。

注意：使用参数`allowed_elements`来允许使用一些特定的HTML元素。

<a name="md034"></a>

## MD034 - 使用裸露的URL

标签：links, url

别名：no-bare-urls

任何时候，当给定的URL没有被尖括号包住时，会触发该规则：

```markdown
获得更多的信息，参见https://www.example.com/.
```

为URL添加上尖括号以修正该问题：

```markdown
获得更多的信息，参见<https://www.example.com/>.
```

原理：很多markdown解析器无法将没有尖括号的URL转换成链接。

注意：如果你希望裸露的URL不会被识别为链接，将其写入代码块中，否则某些解析器 _会_ 将其转化成链接：

```markdown
`https://www.example.com`
```

<a name="md035"></a>

## MD035 - 水平线样式

标签：hr

别名：hr-style

参数：style ("consistent", "---", "***", 或者指定其他的水平线字符串; 默认 "consistent")

当文档中使用的水平线样式不一致时触发该规则：

```markdown
---

- - -

***

* * *

****
```

确保文档中使用的任何水平线是一致的，或者匹配规则配置的给定样式以修正该问题：

```markdown
---

---
```

注意：默认情况下，该规则被配置为只要文档中的所有水平线是一致的，如果任何一个水平线与文档中的第一个水平线的样式不同会触发规则。
如果你想将规则配置为匹配一个特定的样式，将给定的`style`参数选项设置为确定的水平线文本字符串。

<a name="md036"></a>

## MD036 - 用强调来代替标题

标签：headings, headers, emphasis

别名：no-emphasis-as-heading, no-emphasis-as-header

参数：punctuation (string; 默认 ".,;:!?。，；：！？")

该规则检查没有使用标题，而是使用强调（即：粗体或者斜体）来分隔段落：

```markdown
**我的文档**

Lorem ipsum dolor sit amet...

_另一个部分_

Consectetur adipiscing elit, sed do eiusmod.
```

使用markdown标题而不是强调文本来分隔段落以修正该问题：

```markdown
# 我的文档

Lorem ipsum dolor sit amet...

## 另一个部分

Consectetur adipiscing elit, sed do eiusmod.
```

注意：该规则查找完全由强调文本组成的单行段落。
不会影响普通文本中的强调、多行强调段落，或者以标点符号（普通符号或者全角符号）结束的段落。
与规则MD026一样，你可以配置什么字符被识别为标点符号。

<a name="md037"></a>

## MD037 - 强调标号中的空格

标签：whitespace, emphasis

别名：no-space-in-emphasis

使用强调标号（粗体，斜体）时，标号与文本间有空格时触发该规则：

```markdown
这是一些 ** 粗体 ** 文本。

这是一些 * 斜体 * 文本。

这是一些更多的 __ 粗体 __ 文本。

这是一些更多的 _ 斜体 _ 文本。
```

移除强调标号中的空格以修正该问题：

```markdown
这是一些 **粗体** 文本。

这是一些 *italic* 文本。

这是一些更多的 __bold__ 文本。

这是一些更多的 _italic_ 文本。
```

原理：强调只会在星号/下划线没有完全被空格包围时被解析。
该规则视图检测它们哪里被空格包围，但是看起来强调文本被作者有意设计如此。

<a name="md038"></a>

## MD038 - 代码块元素中的空格

标签：whitespace, code

别名：no-space-in-code

代码块元素的反引号中有空格时触发该规则：

```markdown
` 一些文本 `

`一些文本 `

` 一些文本`
```

移除代码块标号中的空格以修正该问题：

```markdown
`一些文本`
```

注意：允许使用一个单独的开头或结尾的空格来分隔代码块标号和内嵌反引号：

```markdown
`` ` 内嵌的反引号``
```

<a name="md039"></a>

## MD039 - 链接文本中的空格

标签：whitespace, links

别名：no-space-in-links

链接中有空格围绕着链接文本时触发该规则：

```markdown
[ 一个链接 ](https://www.example.com/)
```

移除围绕链接文本的空格以修正该问题：

```markdown
[一个链接](https://www.example.com/)
```

<a name="md040"></a>

## MD040 - 代码块应该指明语言

标签：code, language

别名：fenced-code-language

当使用代码块时没有指明语言会触发该规则：

````markdown
```
#!/bin/bash
echo Hello world
```
````

为代码块添加语言说明符以修正该问题：

````markdown
```bash
#!/bin/bash
echo Hello world
```
````

<a name="md041"></a>

## MD041 - 文件的第一行应该是顶级标题

标签：headings, headers

别名：first-line-heading, first-line-h1

参数：level, front_matter_title (number; 默认 1, string; 默认 "^\s*title:")

该规则意图确保文档有一个标题，并当文件的第一行不是顶级（h1）标题时被触发：

```markdown
这是一个没有标题的文件
```

在文件的开头添加一个顶级标题修正该问题：

```markdown
# 有标题的文件

这是一个有顶级标题的文件
```

注意：参数`level`可以用来修改顶级标题（例如：改为h2），以防止h1在外部被添加。

如果[YAML](https://en.wikipedia.org/wiki/YAML)前面包含一个`title`属性（通常使用在博客中）而面临问题，
这条规则会将其视为顶级标题并会对其后的顶级标题报出警告。
在前面使用不同属性名，使用属性`front_matter_title`修改正则表达式文本。
将`front_matter_title`修改为`""`以关闭该规则。

<a name="md042"></a>

## MD042 - 禁止空白链接

标签：links

别名：no-empty-links

当遇到一个空白链接时触发该规则：

```markdown
[一个空白链接]()
```

为链接提供一个目标地址修正该问题：

```markdown
[有效的链接](https://example.com/)
```

空白片段会触发该规则：

```markdown
[空白片段](#)
```

但是非空片段不会触发规则：

```markdown
[有效的片段](#fragment)
```

<a name="md043"></a>

## MD043 - 必需的标题结构

标签：headings, headers

别名：required-headings, required-headers

参数：headings, headers (array of string; 默认 `null` 关闭)

> 如果没有提供`headings`，将使用`headers`（已弃用）。

当文件中的标题没有匹配规则中的标题数组时触发该规则。该规则可以为一组文件强制使用一个标准的标题结构。

要准确地要求下列的结构：

```markdown
# Head
## Item
### Detail
```

将参数`headings`设置为：

```json
[
    "# Head",
    "## Item",
    "### Detail"
]
```

要允许和下列结构一样的可选标题：

```markdown
# Head
## Item
### Detail (optional)
## Foot
### Notes (optional)
```

使用特殊值`"*"`意味着“一个或多个未指明的标题”，并将参数`headings`设置为：

```json
[
    "# Head",
    "## Item",
    "*",
    "## Foot",
    "*"
]
```

当检测到错误时，该规则输出第一个有问题标题的行号（否则会输出文件中的最后一个问题标题的行号）。

注意，为了方便起见，尽管参数`headings`使用“## Text”这样的ATX样式的标题，但是文件可以使用任何支持的标题样式。

<a name="md044"></a>

## MD044 - 恰当的名字应该有正确的大写

标签：spelling

别名：proper-names

参数：names, code_blocks (string array; 默认 `null`, boolean; 默认 `true`)

当数组`names`中的任何字符串没有特定的大小写时触发该规则。该规则可以用来强制项目和产品名称的标准写法。

例如，编程语言“JavaScript”经常将‘J’与‘S’大写——尽管有时也会小写‘j’和‘s’。
为了强制正确的大小写，把期望的写法列入`names`数组中。

```json
[
    "JavaScript"
]
```

将参数`code_blocks`设置为`false`为代码块关闭该规则。

<a name="md045"></a>

## MD045 - 图像应该有替换文本（alt text）

标签：accessibility, images

别名：no-alt-text

当图片缺失替换文本（alt text）信息时触发该规则。对于可访问性，替代文本非常重要，为无法看到图片的人提供图像内容的描述。

替换文本通常如下的内联指定：

```markdown
![替换文本](image.jpg)
```

或者参考如下的语法：

```markdown
![替换文本][ref]

...

[ref]: image.jpg "可选标题"
```

书写替换文本的指南可以从[W3C](https://www.w3.org/WAI/alt/)，
[Wikipedia](https://en.wikipedia.org/wiki/Alt_attribute)以及
[其他站点](https://www.phase2technology.com/blog/no-more-excuses-definitive-guide-alt-text-field)获得。

<a name="md046"></a>

## MD046 - 代码块样式

标签：code

别名：code-block-style

参数：style ("consistent", "fenced", "indented"; 默认 "consistent")

当同一个文档内使用了多余的或者不同的代码块样式时触发该规则。

在默认的配置下，该规则会为下列的文档报告一个错误：

    一些文本。

        # 缩进的代码

    更多的文本。

    ```ruby
    # 围着的代码
    ```

    更多的代码。

使用一致的样式（或者缩进或者代码栅栏）以修正违反该规则的问题。

样式可以被指定为（`fenced`，`indented`）或者简单地要求文档内使用一致的样式（`consistent`）。

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
