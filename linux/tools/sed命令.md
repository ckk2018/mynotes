# sed命令
### 1. 基本options
- **-e**<br/>
指定要执行的脚本语句。

- **-n**<br/>
`sed`命令默认会将操作后的文本完整输出，如果提供了`-i`则输出到文件，否则输出到控制台。`-n`可以禁用这个默认输出。

- **-i**<br/>
原地编辑文件。在将操作后的文本写入文件之前，会先清空文件，保证写入后的文件内容与文本操作结果一致。

- **-E/-r**<br/>
使能脚本中的扩展正则匹配。当使用正则时经常搭配该option。

### 2. 脚本命令
- **s**<br/>
文本替换，格式为`range s/xxx/***/g`，其中：<br/>
`range`代表[范围筛选](#3-范围筛选)，<br/>
`xxx`为需要替换的内容，可以是字符串，也可以是正则，<br/>
`***`为用来替换的内容，一般都是字符串，<br/>
`g`代表全局替换，不写则只替换每行第一个匹配到的元素。
  ```sh
  # test.log
  hello, world~
  
  # 将`hello`替换为`Hello`
  sed -e 's/hello/Hello/' test.log
  
  # 使用正则将行首的`h`替换为`H`
  sed -e 's/^h/H/' test.log
  ```

- **p**<br/>
结果输出，格式为`range p`，其中：<br/>
`range`代表[范围筛选](#3-范围筛选)。<br/>
一般配合`-n`使用，用于在禁用了默认的完整输出后，仅输出筛选出的内容。若提供`-i`则原地输出到文件，否则输出到控制台。<br/>
⚠️*注意：`-n`不是必须的，加上是为了避免重复打印。*
  ```sh
  # test.log
  first hello world~
  second hello world~
  
  # 仅打印第二行
  sed -n -e '2p' test.log
  
  # 不使用`-n`，会打印全部文本，并将第二行额外打印一次
  sed -e '2p' test.log
  ```

- **a\\**<br/>
行追加，格式为`range a\xxx`，其中：<br/>
`range`代表[范围筛选](#3-范围筛选)，<br/>
`xxx`为需要追加的文本内容。<br/>
⚠️*注意：反斜杠`\`不可省略*。
  ```sh
  # test.log
  hello, world~

  # 在最后一行后追加新行
  sed -e '$a\hello world again!' test.log
  ```

- **i\\**<br/>
行插入，格式为`range i\xxx`，其中：<br/>
`range`代表[范围筛选](#3-范围筛选)，<br/>
`xxx`为需要插入的文本内容。<br/>
⚠️*注意：反斜杠`\`不可省略*。
  ```sh
  # test.log
  hello, world~

  # 在最后一行前插入新行
  sed -e '$i\before hello world~' test.log
  ```

### 3. 范围筛选
默认的，`sed`的脚本命令会对文本的<font color="scarlet">**所有行**</font>执行，例如：<br/>
`sed -e 'a\hello' test.log`会在每一行后追加新行；<br/>
`sed -e 'i\hello' test.log`会在每一行前插入新行；<br/>
`sed -e 's/h/H/g' test.log`会将每一行中的所有'h'替换为'H'。<br/>
当我们想对文本进行局部地操作时，就需要用到范围筛选功能。

- **行号筛选**<br/>
通过直接输入目标行在文本中的行号进行筛选。
  ```sh
  # test.log
  first line
  second line
  last line
  
  # 仅打印第二行
  sed -n -e '2p' test.log  # 这里'2p'前面的'2'即为行号筛选

  # 特殊
  sed -n -e '$p' test.log  # 打印最后一行
  ```

- **正则筛选**<br/>
通过正则表达式筛选需要操作的行。
  ```sh
  # test.log
  text body
  # first comment
  # second comment
  #no space comment

  # 打印文件中的注释行
  sed -n -E -e '/^# ?/ p' test.log
  ```
