web界面自动化，要对界面进行操作，首先需要定位界面元素。
必须先定位到元素再操作元素。

一、常见的操作
因为常见的操作并不是很多，而且就是一些固定的调用，比较容易理解。
1、点击元素：click（）
2、元素内输入文本：fill（）
3、获取元素内部文本：inner_text（）

二、查找元素
f12打开开发者工具。
点击“元素”中代码可以查看选中元素从而定位。
1、Locator对象：Page对象的locator定位到的如果是唯一的html元素，就可以调用Locator对象的方法，比如fill,click,inner_text等等对元素进行操作了。
2、根据tag名、id、class选择元素：
（1）根据 tag名选择元素的CSS Selector语法非常简单，直接写上tag名即可：locators = page.locator('div').all()//选择所有的tag名为div的元素。
（2）根据id属性选择元素的语法是在id号前面加上一个井号：#id值：
<input  type="text" id='searchtext' />
lct = page.locator('#searchtext')//可以使用#searchtext这样的CSS Selector来选择它
lct.fill('你好')
（3）根据class属性选择元素的语法是在class值前面加上一个点：.class值：
page.locator('.animal')//选择class属性值为animal的元素动物

三、验证CSS Selector
1、直接运行：page.locator('#searchtext').fill('输入的文本')
2、在浏览器开发者工具：f12打开开发者工具→点击Elements标签→同时按Ctrl和F键出现搜索框→在搜索框输入任何CSS Selector表达式 ，如果能选择到元素， 右边会显示出类似2of3这样的内容。（2of3：当前的选择语法，在当前网页上共选择到3个元素，目前高亮显示的是第2个。）

四、匹配多个元素
如果一个 locator表达式匹配多个元素，要获取所有的元素对应的locator 对象，使用all方法：locators = page.locator('.plant').all()
只需要得到某种表达式对应的元素数量 ，可以使用 count方法：count = page.locator('.plant').count()
只需要得到某种表达式对应的第一个，或者最后一个元素。可以使用first和last属性：lct = page.locator('.plant')
print(lct.first.inner_text(), lct.last.inner_text())
也可以通过nth方法，获取指定次序的元素：lct=page.locator('.plant')
print(lct.nth(1).inner_text())
