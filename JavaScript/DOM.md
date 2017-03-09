### DOM

----

#### 一.常用DOM种类

一共12种，常用的是三种

*1.nodeType*

| nodeType |  类型  |
| :------: | :--: |
|    1     | 元素节点 |
|    2     | 属性节点 |
|    3     | 文本节点 |

所有的节点都有ownerDocument属性，该属性指向整个文档的文档节点，可以直接访问文档节点

**注：不是所有的节点都有子节点**

*2.节点关系*

childNodes	//有兼容性问题

chidren		//有兼容性问题

parentNode	//无兼容性问题

fristChild	||	firstElementChild		//有兼容性问题

lastChild		||	lastElementChild		//有兼容性问题

previousSibling	||	previousElementSibling		//有兼容性问题

nextSibling	||	nextElementSibling		//有兼容性问题





*3.节点操作*

appendChild()

repalceChild()

insertBefore()

removeChild()

cloneNode(ture)	//深复制 		||	cloneNode(false)	//浅复制

cloneNode一般是只复制节点，不复制节点的事件，但是ie中却复制事件，因此复制前，应该先把事件清空



#### A.文档节点

##### 1.文档子节点

documentElement	指向html

document.body	指向body

document.title	//修改文档的显示title但是不会修改title元素的内容

~~document.head = docuement.head ||docuement.getElementsByTagName('head')[0] ;~~



##### 2.查找元素

getElementById()

getElementsByTagName()

getElementsByName()



##### 3.文档写入

write()，原样写入与writeIn()字符创的末尾加一个换行符（\n）



####  B.元素节点

tagName == nodeName

*1.取得特性*

getAttribute()

直接自定义属性，不会被添加到元素的特性上，因此获取不到返回null（ie下例外），使用setAttribute()可以

```javascript
//直接自定义属性
oDvi.index = 'demo5'
alert(oDiv.getAttribute('index'));//返回null(ie除外)

//使用setAttribute()
oDiv.setAttribute('index','demo5');
alert(oDiv.getAttribute('index'));//demo5
```



*2.设置特性*

setAttribute()

用setAttribute()设置的属性，可以在**ele.attributes中访问到**

```javascript
//直接访问
oDiv.setAttribute('index','demo5');
alert(oDiv.index);//返回undefined(ie除外)

//使用getAttribute()
oDiv.setAttribute('index','demo5');
alert(oDiv.getAttribute('index'));//demo5
```



*3.removeAttribute()*

貌似只能清除通过setAttribute()设置的属性

特性貌似并不是经常使用

特性的主要用途：遍历元素的所有特性

```javascript
//遍历特性函数
function listAttribute(ele) {
	var arr = new Array(),
		attrName,
		attrValue,
		i,
		len;
	for (i=0,len=ele.attributes.length; i<len; i++) {
		attrName = ele.attributes[i].nodeName;
		attrValue = ele.attributes[i].nodeValue;
		arr.push(attrName+': '+attrValue);
	}
	return arr.join('\n');
}
```

#### C.文本节点

1.创建文本节点

document.creatTextNode()

文本节点会对传入的字符进行编码，与document.write()是有区别的

```javascript
var oDiv = document.getElementById('div1');
var oText = document.createTextNode('<p>haha</p>');
oDiv.appendChild(oText);//<p>haha</p>
```

如果两个文本节点是同胞节点，那么就会连在一起显示，没有空格

2.规范化文本节点

normalize()	用来解决DOM操作导致的相邻文本节点的问题

3.分割文本节点

splitText()

常用来从文本中提取信息

#### D.注释节点

浏览器不会识别位于</html>标签后面的注释，所以要访问注释节点，一定要保证他们是html的后代

用的很少，在ie8中，注释节点被看成是标签名“！”的元素，ie9中没有把注释看成元素，但是它仍然通过一个名为HTMLCommentElement的构造函数来表示注释

#### 二.DOM操作技术

##### A.动态脚本

两种方式，外链引用外部js脚本与<script>内部包含脚本		

//ie下需要注意兼容问题，因为ie下不可访问<script>的子节点，只能通过其text属性添加

```javascript
var script = document.createElement('script');
script.type = 'text/javascript';
script.text = 'function sayHi() {alert("Hi")}; sayHi()';
document.body.appendChild(script);
```

##### B.动态样式

与动态脚本的操作方式类似

#### 三.DOM扩展

##### A.选择符API

jQuery的核心就是通过css选择符查询DOM文档取得元素的引用，从而抛开了getElementById()与getElementsByTagName()

目前已经完全支持Selectors API Level1的浏览器有 ie8+、ff3.5、chrome、opera10+

1.querySelector()

返回与模式匹配的第一个元素，找不到返回null，传入不支持的模式会报错

2.querySelectorAll()

返回所有匹配的元素

3.matchesSelector()

兼容性上存在问题，不同浏览器方法不同		//貌似没有找到相关的东西吗

##### B.HTML5

getElementsByClassName()		//浏览器兼容性问题，ie9+，ff3+

classList		//兼容性太差，

##### C.焦点管理

document.activeElement		当前获得焦点的属性

默认情况下，文档加载完成后，其中保存的是body，加载期间为null

##### D.HTMLDocument

1.document.readyState 文档加载问题

loading：正在加载文档

interactive：文档加载完毕，文档已经解析，但是一些图片之类的资源还在下载中

complete：加载完毕

2.document.compatMode

标准模式： CSS1Compat

混杂模式：BackCompat

3.insertAdjacentHTML()

基本不存在兼容性问题

4.scrollIntoView()		ie8+	

##### E.专有扩展

1.contains()

2.compareDocumentPosition()

3.innerText属性		ff45之前不支持该功能

可以采用textContent

4.outerText

------

### DOM2与DOM3

