###准备工作

1.安装sass
！！！在安装sass之前必须的先安装ruby，因为sass是由ruby发明的
然后再通过npm来安装
`npm install node-sass -g`
如果npm安装失败，需要先`npm uninstall node-sass -g`卸掉缓存文件，再用`cnpm install node-sass -g`去安装一次
2.建好文件结构

![image.png](http://upload-images.jianshu.io/upload_images/8168023-2875cc2389a282cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

src是我们的需要去编辑的scss文件，css是通过node-sass转译成的css文件，我们再html里引用的还是dist目录下的css文件，因为浏览器只认识html，css，js，其他的都不认识，就比如scss，less之类。
3.使用
`node-sass src/input.scss dist/output.css -w input.scss`
也就是`node-sass scss目录地址 css目录地址 -w scss文件`
可以动态的监听scss的变化，实时的转译成css文件，然后在页面手动刷新

###现在我们来学习sass语法

#####1. 使用变量;

#####变量声明;

sass让人们受益的一个重要特性就是它为css引入了变量。你可以把反复使用的css属性值 定义成变量，然后通过变量名来引用它们，而无需重复书写这一属性值。或者，对于仅使用过一 次的属性值，你可以赋予其一个易懂的变量名，让人一眼就知道这个属性值的用途。
**sass使用 `$`符号来标识变量**
```
//编译前sass写法
$h3-color:red;
$width:300px;
.box{
  width: $width;
  h3{
    color: $h3-color;
  }
}

//编译后
.box {
  width: 300px; }
  .box h3 {
    color: red; }

```
######变量引用;
凡是css属性的标准值（比如说1px或者bold）可存在的地方，变量就可以使用。css生成时，变量会被它们的值所替代。之后，如果你需要一个不同的值，只需要改变这个变量的值，则所有引用此变量的地方生成的值都会随之改变。
这个意思就是，我们写好一个一个变量，在需要的地方去引用，假如以后我们的需要改动了，我就可以只去改写变量就行了，如果是直接写css文件，那以后改动的话相对而言就需要多发费点时间和精力。
```
$h3-color:red;
$width:300px;
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.box {
    width: $width;
    border: $highlight-border;
    h3 {
        color: $h3-color;
    }
}

//编译后
.box {
  width: 300px;
  border: 1px solid #F90; }
  .box h3 {
    color: red; }

```
######变量名用中划线还是下划线分隔;
申明变量的时候，不一定非得使用中划线或下划线中的一种，两者是互通的，$h3-color;和$h3_color;作用是一样的，但是要注意的是，但是在sass中纯css部分不互通，比如类名、ID或属性名。
也就是说，申明变量可以使用中横线或者下划线，结果一样，但是css类名、ID或属性名不行。
#####2、嵌套CSS 规则
这是我们正常写的css代码，多个重复，很烦人是不是，比如，box每次都要重复，我可不可以不写这么多次？答案是可以的，所以这就是sass很好用的一个因素之一。
```
.box {
    width: 300px;
    border: 1px solid #F90;
}

.box h3 {
    color: red;
}

.box .parent {
    border: 1px solid #F90;
    background: #fefefe;
}
```
编译前的代码
```
$h3-color:red;
$width:300px;
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.box {
    width: $width;
    border: $highlight-border;
    h3 {
        color: $h3-color;
    }
    .parent{
      border: $highlight-border;
      background: #fefefe;
    }
}
```
再来看下html是怎么写的
```
 <div class="box">
    <h3>你好 </h1>
    <div class="parent">
      <h3>world</h2>
      <div class="children">
        <h3>这里是sass</h3>
      </div>
    </div>
  </div>
```
是不是感觉sass写的感觉和html很相似啊，box1里面有h3还有个parent，里面还有其他的标签，然后写sass的时候，只要按照html的结构去写就行了，最大的好处是结构一目了然，层层对应，相同的类名或者id不要再多写几次。

######子组合选择器和同层组合选择器：>、+和~;
* **>**:子代选择器，这个和css中的子代选择器类似`article > section { border: 1px solid #ccc }`
* **+**：同层相邻组合选择器，就是选择同级中相邻的元素`header + p { font-size: 1.1em }`；
* **~**：同层全体组合选择器，这个就厉害了，不管你是多远的地方我都能找到你，其中把中间的都包括了`article ~ article { border-top: 1px dashed #ccc }`

###### 嵌套属性;
嵌套属性的规则是这样的：把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。就像css选择器嵌套一样，sass会把你的子属性一一解开，把根属性和子属性部分通过中划线-连接起来，最后生成的效果与你手动一遍遍写的css样式一样：
```
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
//编译后
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```
#####静默注释;
css中注释的作用包括帮助你组织样式、以后你看自己的代码时明白为什么这样写，以及简单的样式说明。但是，你并不希望每个浏览网站源码的人都能看到所有注释。
```
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```
