#### 应用场景
选取一段文本，对该段文本进行操作

#### 兼容性
1. window.getSelection() 或 document.getSelection()
    * FF、Chrome、IE9（含）+、Opera、Safari
2. document.selection
    * IE8

#### 常用属性及方法
```javascript
var selectionObj = window.getSelection();
```

1. 获取选取文本的内容

	```javascript
    var selectedText = selectionObj.toString();
    ```
    该方法可以获取到选择的文本内容，不受节点限制
    ```html
    <p>本书原作者 Zakas 长期<span class="selected">供职于雅虎</span>，是著名的 JS 库 YUI 的主要作者，有着非<span class="selected">常丰富的一线</span>工作经验。他同时也是一个成功的作者，其最重要的著作《 JavaScript 高级编程》基本上是 JS 领域的必读之作，而他还出版了另一些质量很高的著作。《高级编程》一书实际上并不是完全高深的内容，而是从基本的层次开始讲述，逐步提高，全书结构比较良好，对初学者或有一定经验的开发者来说都是很有用的。</p>
    ```
	![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085154104-1321258170.png)
    此时`selectedText`为`著名的 JS 库`

	![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085205026-1629434881.png)
    此时`selectedText`为`于雅虎，是著名`，所以`toString`方法不受标签限制，返回选取的文本内容

2. 获取选取文本起止点所在节点
    ```js
    var anchorNode = selectionObj.anchorNode;
    var baseNode = selectionObj.baseNode;
    var extentNode = selectionObj.extentNode;
    var focusNode = selectionObj.focusNode;
    ```
    `anchorNode`和`baseNode`为选取文本起始点所在节点，`extentNode`和`focusNode`为选取文本结束点所在节点

    ![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085154104-1321258170.png)
    此时，不管是从前向后选取还是从后向前选取，选取的文本都在`，是著名的 JS 库 YUI 的主要作者，有着非`范围内，所以`anchorNode`、`baseNode`、`extentNode`和`focusNode`是相同的

    ![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085205026-1629434881.png)
    此时，如果是从前向后选取，`anchorNode`和`baseNode`应该是`<span class="selected">供职于雅虎</span>`，`extentNode`和`focusNode`为`，是著名的 JS 库 YUI 的主要作者，有着非`；如果是从后向前选取，则相反
3. 获取选取文本在其所在节点中的起止位置
	```javascript
    var anchorOffset = selectionObj.anchorOffset;
    var baseOffset = selectionObj.baseOffset;
    var extentOffset = selectionObj.extentOffset;
    var focusOffset = selectionObj.focusOffset;
    ```
    `anchorOffset`和`baseOffset`为选取文本起点所在节点的位置（从0开始，从左向右数），`extentOffset`和`focusOffset`为选取文本结束点所在节点的位置（从0开始，从左向右数）

    ![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085154104-1321258170.png)
    此时，选取的文本在同一节点内，如果是从前向后选取，则`anchorOffset`和`baseOffset`为`2`，`extentOffset`和`focusOffset`为`10`，因为从`，`前开始数`0`，到`著`字前，为`2`，到`库`字后，为`10`（`JS`前后有空格）；相反的，如果从后向前选取，则赋值交换

    ![](http://images2015.cnblogs.com/blog/569926/201702/569926-20170211085205026-1629434881.png)
    此时，选取的文本不在同一节点内，如果是从前向后选取，则`anchorOffset`和`baseOffset`为起点`雅`字在节点`<span class="selected">供职于雅虎</span>`中的起始位置`3`（字之前的位置），`extentOffset`和`focusOffset`为结束点`名`字在节点`，是著名的 JS 库 YUI 的主要作者，有着非`中的结束位置`4`（字之后的位置）;如果是从后向前选取，则赋值交换
4. 判断是否有选取内容
有多种方法可以判断当前是否有选取的内容
    * `anchorNode`为`null`
    * `toString`返回为空
    * `anchorOffset`和`focusOffset`相等
    * `isCollapsed`为`true`
    * `type`等于`Caret`或`None`（当有选取内容时，`type`为`Range`）
    * `rangeCount`为`0`

#### IE8下的使用
```javascript
var selectionObj = document.selection;
var rangeObj = selectionObj.createRange();
```
IE8通过`createRange`方法将选取的文本作为块处理，位置标记没有起止点之分
```json
{
	boundingHeight: 19,
    boundingLeft: 213,
    boundingTop: 16,
    boundingWidth: 96,
    htmlText: '<SPAN class="selected">雅虎</SPAN>，是著名',
    offsetLeft: 211,
    offsetTop: 14,
    text: '雅虎，是著名'
}
```
在IE8下还可以通过命令`execCommand`对选取的文本进行操作