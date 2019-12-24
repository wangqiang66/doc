[TOC]

## compiler对象介绍

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
