## CSS的权重
### 一、CSS的引入方式
1. 在节点元素上，使用`style`属性
2. 通过`link`引入外部文件
3. 通过`style`标签在页面内引入


##### 三种引入方式的区别
`index.html`文件
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <link rel="stylesheet" type="text/css" href="body.css">
        <style type="text/css">
            body {
                background: red;
            }
        </style>
    </head>
    <body style="background: yellow;">
    </body>
    </html>
```
`body.css`文件
```css
	body {
    	background: green;
    }
```
1. 第一种方式相对后面两种优先级高，与引入顺序无关
    * 无论`link`和`style`标签放在`head`内，还是放在`body`内，或者放在`html`标签结尾，页面都会呈现`yellow`
2. 第二种和第三种为平级引入，后引入的样式覆盖之前的引入样式
    * 去掉`body`上的`style`标签
    * 调整`link`和`style`标签的先后顺序。`link`在前，`style`标签在后，页面呈现`red`，相反，页面会呈现`green`

### 二、获取节点的方式

1. `id`
2. `class`
3. 标签
4. 属性

##### `id`

在一个页面中`id`值应该是唯一，但是，当出现多个相同`id`时，样式对所有`id`节点是有效的，使用方式：`#`后面跟节点`id`值

```html
<body>
  <p id="id_p">第一个段落</p>
  <p id="id_p">第二个段落</p>
</body>
```

```css
#id_p {
  color: red;
}
```

结果显示，两个段落中的文字都呈现`red`

1. `id`相对`class`和标签具有更高的权重，当`id`和`class`、标签同时对一个节点设置样式时，`id`的权重为最高
2. 通过`link`和`style`标签对同一个`id`设置样式时，后引入的样式覆盖之前的样式

##### `class`
使用`class`可以对多个节点同时设置样式，并且可以叠加`class`使用。使用方式`.`后面跟节点单个`class`名

```html
<body>
  <p class="class-p">第一个段落</p>
  <p class="class-p class-p-2">第二个段落</p>
</body>
```

```css
.class-p {
  color: red;
}
.class-p-2 {
  color: green;
}
```

此时，第一个段落呈现`red`，第二个段落呈现`green`

调整`html`

```html
<body>
  <p class="class-p">第一个段落</p>
  <p class="class-p-2 class-p">第二个段落</p><!-- 调换class-p 和 class-p-2 的顺序  -->
</body>
```

调整`class-p`和`class-p-2`的位置后，页面呈现效果不变。说明：`class`样式的渲染和`class`的使用顺序无关，与`class`样式设置的先后顺序有关，同权重的`class`样式，在样式设置中，靠后的样式设置覆盖之前的样式设置

##### 属性

通过节点上的属性也可以得到要进行样式设置的节点

```html
<body>
  <p>第一个段落</p>
  <p title="第二个段落的title">第二个段落</p>
</body>
```

```css
[title] {
  color: red;
}
```

第二个段落有`title`属性，所以第二个段落呈现`red`

##### 标签

通过标签名获取节点进行样式设置

```html
<body>
  <p>第一个段落</p>
  <p>第二个段落</p>
</body>
```

```css
p {
  color: red;
}
```

页面中所有`p`标签节点呈现`red`

##### 混合

以上四种方式可以混合使用，对相应的节点进行样式设置。结合方式包括层级嵌套、样式叠加、节点关联等。最终以权重高者为呈现效果。

### 三、样式权重

以上四种方式，针对单个而言，`id`最高，`class`和属性同级（后面的样式覆盖之前的样式），标签最低。

当四种方式混合使用时，则以权重的结果为准。对同一结点存在的`id`、`class`、属性和标签样式进行排序，排位第一者为最终呈现效果。例如：对于节点`p`存在多种类型的样式设置，首先挑选所有带`id`的样式，包括嵌套样式。相同`id`下，对另一类型样式进行排序

```html
<body class="body">
  <p id="id_p">第一个段落</p>
</body>
```

```css
.body #id_p {
  color: red;
}

#id_p {
  color: green
}
```

虽然两种样式设置都有`id`，并且`green`效果在`red`之后被设置，但是通过排序可以得到相同`#id_p`下，前一个存在`.body`，所以最终呈现效果为`red`

存在`class`、属性和标签的样式时，依次排序，同类型或同权重（`class`和属性同权重）的样式，靠后的样式覆盖之前的样式（以类型为准，不以名称为准），最终排位第一者为最终呈现效果。

 注意：

1. 嵌套、叠加、`>`、 `+`等方式，不会影响最终效果。
2. `:nth-child`、`:first-child`、`:last-child`等伪类高于`class`和属性

### 四、`!important`
`!important`为样式中的特例，它的权重为最高，高于`id`、`class`、属性、标签以及`style`属性

```html
<body class="body" style="background: red"></body>
```

```css
.body {
  background: green !important;
}
```

页面呈现效果为`green`。但是当对样式设置进行排序时，仍然是同类型样式下，以另一类型权重高者为最终效果。例如

```css
body.body {
  background: blue !important;
}
.body {
  background: green !important;
}
```

相同`class`及`!important`下，前一种样式设置存在`body`标签，后一种则没有，所以最终效果呈现`blue`

##### 说明

1. 尽量避免使用`!important`。因为`!important`权重最高，会对节点的该属性做强制性设置，在使用时要慎重

2. 使用场景
   * 引入插件时，对插件中的样式进行强覆盖。当引入插件时，在不想修改插件中的样式代码情况下，可通过`!important`对插件内的样式属性进行强制复写
   * 对行内样式进行强覆盖。对于自动生成或者动态引入的的带有行内样式`html`结构时，可以通过`!important`对行内样式进行强制复写

3. 变通

   !important`在很多时候是不建议使用的，在`stylelint`中有一项规则即禁止使用`!important`。有一种变通的方式，可以在多数情况下实现类似`!important`的效果

   ```html
   <body class="body">
     <p class="p">
       <span class="span">一段文本</span>
     </p>
   </body>
   ```

   ```css
   .body .p .span {
     color: red;
   }
   .span.span.span.span.span {/** 自身样式叠加 **/
     color: green;
   }
   ```

   在不考虑行内样式和`id`的情况下，对自身样式进行重复叠加多次使用，可以增加`class`权重，对样式进行复写。

   使用前提：

   1. 没有行内样式`style`属性
   2. 没有`id`样式
   3. 自身样式叠加次数多余嵌套个数

   好处：不用考虑`DOM`层级关系，减少层级嵌套

### 五、总结

综合以上说明，权重的计算基本遵从以下规则：

1. 按类型比对，类型权重高者显示；
2. 同类型，按数量比对，多者显示；
3. 同数量，按先后顺序比对，后者显示

## 嵌套的使用建议

样式嵌套使用，除了增加权重外，也体现了`DOM`的某种结构关系。但嵌套并不是在任何情况下都需要的。

* 嵌套多用于块内独有的样式设置。某种样式仅在某个块内有效，可使用嵌套。
* 多个页面同时开发时，为避免合并后样式被覆盖，可使用嵌套。

嵌套的使用并不是越多越好。嵌套越多，权重越大，但同时对页面的性能消耗也越大。建议使用继承和样式叠加。