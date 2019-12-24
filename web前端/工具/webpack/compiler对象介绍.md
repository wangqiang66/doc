[TOC]

## compiler对象介绍

Compiler的大部分内部属性的复制都是在WebpackOptionsApply。配置文件赋值的时候用到的

### 属性详解

1. recordsInputPath

    读取配置文件recordsInputPath || recordsPath

    - 可以使用此文件来跟踪在每次构建之间的模块变化。生成的是一个JSON。

2. recordsOutputPath

    读取配置文件recordsOutputPath || recordsPath

    - 可以使用此文件来跟踪在每次构建之间的模块变化。生成的是一个JSON。
  
3. inputFileSystem

    在NodeEnvironmentPlugin中生成。CachedInputFileSystem对象，有level的概念

4. outputFileSystem、intermediateFileSystem

    在NodeEnvironmentPlugin中生成。graceful-fs 模块对象。

    - 底层Node的fs对象API
    - 当某个fs行为出错，该fs操作类型与参数会被记录下来
    - 当某个fs行为成功执行，会尝试将最早出错的行为取出并再次执行，出错会再次被记录

5. watchFileSystem

    在NodeEnvironmentPlugin中生成。NodeWatchFileSystem对象。使用inputFileSystem对象记录。

6. name

    多配置的时候需要。读取配置文件的name

7. outputPath

    读取配置文件output.path。绝对路径，文件输出的位置

8. infrastructureLogger

    有默认值{ level: info, debug: false, console： nodeConsole }，读取配置文件的infrastructureLogger，是一个对象。日志处理，包括level，debug，console对象。
    level: 1 - 6。verbose [true log] info warn error [false none]
