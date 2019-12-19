## dom 渲染

对于dom渲染是顺序渲染的。 里面存在这style和link的区别。加载的时候渲染dom也存在不同。
按照我的推算，应该渲染cssom存在一个延迟，在延迟内容存在其他css的文件或者样式的引入，会进行合并生成一个cssom

```html
/*test1.css*/
div {
  background:orange;
}

/*test.css*/
div {
  background:blue;
}

<style>
    div {
        width: 100px;
        height: 100px;
        transition: background 1s;
        background: yellow;
    }
  </style>
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: black;
    }
  </style>
<span></span>
<link rel="stylesheet" href="./test.css">
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: blueviolet;
    }
  </style>
<span></span>
<link rel="stylesheet" href="./test1.css">
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: red;
    }
  </style>
<span></span>
<div></div>
<!-- <link rel="stylesheet" href="./test.css?defer=true"> -->
<div></div>
```

前面黑色直接到红色渐变 说明link阻碍cssom的构建


```html
<style>
    div {
        width: 100px;
        height: 100px;
        transition: background 1s;
        background: yellow;
    }
  </style>
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: black;
    }
  </style>
<span></span>
<link rel="stylesheet" href="./test.css">
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: blueviolet;
    }
  </style>
<span></span>
<link rel="stylesheet" href="./test1.css？defer?true">
<div></div>
<style>
    div {
        width: 100px;
        height: 100px;
        background: red;
    }
  </style>
<!-- <link rel="stylesheet" href="./test.css?defer=true"> -->
<div></div>
```

前面black直接到blueviolet再到red渐变 说明link阻碍cssom的构建 并且cssom是延迟的
