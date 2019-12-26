[TOC]

## compiler对象介绍

Compiler的大部分内部属性的复制都是在WebpackOptionsApply。配置文件赋值的时候用到的

### 属性详解

#### 输入输出流对象

1. inputFileSystem

    在NodeEnvironmentPlugin中生成。CachedInputFileSystem对象，有level的概念

2. outputFileSystem、intermediateFileSystem

    在NodeEnvironmentPlugin中生成。graceful-fs 模块对象。

    - 底层Node的fs对象API
    - 当某个fs行为出错，该fs操作类型与参数会被记录下来
    - 当某个fs行为成功执行，会尝试将最早出错的行为取出并再次执行，出错会再次被记录

3. watchFileSystem

    在NodeEnvironmentPlugin中生成。NodeWatchFileSystem对象。使用inputFileSystem对象记录。

#### 记录日志相关

1. recordsInputPath

    读取配置文件recordsInputPath || recordsPath

    - 可以使用此文件来跟踪在每次构建之间的模块变化。生成的是一个JSON。

2. recordsOutputPath

    读取配置文件recordsOutputPath || recordsPath

    - 可以使用此文件来跟踪在每次构建之间的模块变化。生成的是一个JSON。

3. records

    内部变量，日志记录。

    - 读取recordsInputPath的内容，输出到recordsOutputPath。
  
4. infrastructureLogger

    有默认值{ level: info, debug: false, console： nodeConsole }，读取配置文件的infrastructureLogger，是一个对象。日志处理，包括level，debug，console对象。
    level: 1 - 6。verbose [true log] info warn error [false none]

#### 其他

1. name

    多配置的时候需要。读取配置文件的name

2. outputPath

    读取配置文件output.path。绝对路径，文件输出的位置

3. running

    内部变量，记录是否正编译。不管是run还是watch最终都会走到compiler方法进行编译

    - 防止没有编译完成的情况下进行编译

4. watchMode

    内部变量，是否是watch模式