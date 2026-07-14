web界面自动化，要对界面进行操作，首先需要定位界面元素。
必须先定位到元素再操作元素。

一、常见的操作
因为常见的操作并不是很多，而且就是一些固定的调用，比较容易理解。
1、点击元素：click（）
2、元素内输入文本：fill（）
3、获取元素内部文本：inner_text（）

二、查看特征
f12打开开发者工具。
点击“元素”中代码可以查看选中元素从而定位。
1、Locator对象：Page对象的locator定位到的如果是唯一的html元素，就可以调用Locator对象的方法，比如fill,click,inner_text等等对元素进行操作了。
2、根据tag名、id、class选择元素：
（1）根据 tag名选择元素的CSS Selector语法非常简单，直接写上tag名即可：locators = page.locator('div').all()//选择所有的tag名为div的元素。
（2）根据id属性选择元素的语法是在id号前面加上一个井号：#id值：
<input  type="text" id='searchtext' />
lct = page.locator('#searchtext')//可以使用#searchtext这样的CSS Selector来选择它
lct.fill('你好')
（3）
