[TOC]

## compilation对象介绍

compilation的大部分内部属性的赋值都是在Compiler。

### 属性详情

#### 来自compilation属性

1. compiler

    Compiler对象

2. resolverFactory

    Compiler的resolverFactory对象

3. inputFileSystem
4. cache
5. compilerPath
6. requestShortener
7. options

#### 内部属性

1. chunkGraph

    ChunkGraph对象 { _modules, _chunks, _blockChunkGroups, moduleGraph }

    - _modules: WeakMap({ module: ChunkGraphModule })
    - ChunkGraphModule对象 { chunks, entryInChunks, runtimeInChunks, hash, renderedHash, id, runtimeRequirements }

    - _chunks: WeakMap({ module: ChunkGraphChunk })
    - ChunkGraphChunk对象 { modules, entryModules, runtimeModules, runtimeRequirements, runtimeRequirementsInTree }

2. modules

    Set(module)

3. namedChunks

    Map({name, Chunk})

    - Chunk对象 { id, ids, debugId, name, preventIntegration, filenameTemplate, _groups, files, auxiliaryFiles, rendered, hash, contentHash, renderedHash, chunkReason, extraAsync }

4. chunks

    Set(Chunk)

5. entryDependencies

    Map({name, Array})