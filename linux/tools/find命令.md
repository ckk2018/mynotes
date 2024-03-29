# find命令
### 1. 用法
`find [options] [path...] [expression]`
- **options**<br/>
  TODO

- **path**<br/>
需要执行`find`命令的目标目录。
  ```sh
  # 从当前目录开始查找所有的`.txt`文件
  find . -name '*.txt'
  ```

- **expression**<br/>
查找时需要执行的表达式，上例中的`-name '*.txt'`即为表达式，规定了名称筛选的规则。<br/>
具体可见[表达式](#3-表达式)章节

### 2. 基本options
TODO

### 3. 表达式
- **-name**<br/>
名称筛选，支持通配符`*`。
  ```sh
  # 从当前目录开始查找所有的.so文件
  find . -name '*.so'
  ```

- **-atime**<br/>
文件访问时间筛选。
  ```sh
  # 从当前目录开始查找至少30天没有被访问过的文件
  find . -atime +30
  ```

- **-mtime**<br/>
文件修改时间筛选。
  ```sh
  # 从当前目录开始查找至少30天没有被修改过的文件
  find . -mtime +30
  ```

- **-exec**<br/>
对筛选出的文件执行操作。<br/>
用`{}`指代被筛选出的文件，表达式必须以`\;`结尾（`\`用于转义）
  ```sh
  # 从当前目录开始，删除所有30天未修改的.log文件
  find . -mtime +30 -name '*.log' -exec rm -rf {} \;
  ```