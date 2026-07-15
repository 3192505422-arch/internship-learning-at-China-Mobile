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

五、元素内部定位
可以通过Locator对象调用locator方法：
lct = page.locator('#bottom')
#在#bottom 对应元素的范围内 寻找标签名为 span 的元素。
eles = lct.locator('span').all()
for e in eles:
    print(e.inner_text())

六、选择子元素和后代元素
HTML中，元素内部可以包含其他元素。
如果元素1是元素2的直接子元素：元素1>元素2
也支持多层级选择：元素1>元素2>元素3>元素4（最终选择元素4）
如果元素2是元素1的后代元素：元素1  元素2
也支持多层级选择：元素1  元素2  元素3  元素4（最终选择元素4）

七、根据属性选择
1、id、class 都是web元素的属性，因为它们是很常用的属性，所以css选择器专门提供了根据id、class选择的语法。
2、其他属性：css选择器支持通过任何属性来选择元素，语法是用一个方括号[]。
比如<a href="http://www.miitbeian.gov.cn">苏ICP备88885574号</a>里面根据href选择上面的a元素：[href="http://www.miitbeian.gov.cn"]（选择属性href值为http://www.miitbeian.gov.cn的元素
前面可以加上标签名的限制：a[href="http://www.miitbeian.gov.cn"] 表示选择所有标签名为a，且属性href值为http://www.miitbeian.gov.cn的元素。
选择属性值包含某个字符串的元素：a[href*="miitbeian"]（选择a节点，里面的href属性包含了 miitbeian 字符串）
选择属性值以某个字符串开头的元素：a[href^="http"]（选择a节点，里面的href属性以gov.cn结尾）
指定选择的元素要同时具有多个属性的限制：div[class=misc][ctype=gun]

八、选择语法联合使用
选择语法可以联合使用：表示“且”
用“，”隔开：表示“或”（由文档中由上到下选中）
注意符号优先级，“，”优先级较高，不可以使用（）

九、按次序选择子节点
1、指定选择的元素是父元素的第几个子节点：nth-child
比如span:nth-child(2)（选择的是第2个子元素，并且是span类型）
直接这样写:nth-child(2)（选择所有位置为第2个的所有元素，不管是什么类型）
2、选择的是父元素的倒数第几个子节点：nth-last-child
比如p:nth-last-child(1)（选择第倒数第1个子元素，并且是p元素）
3、指定选择的元素是父元素的第几个某类型的子节点：nth-of-type
比如span:nth-of-type(1)（选择第1个span类型的子元素，相当于span:nth-child(2)）
4、选择父元素的倒数第几个某类型的子节点：p:nth-last-of-type(2)
比如p:nth-last-of-type(2)（选择第倒数第2个子元素，并且是p元素）
5、奇数节点：p:nth-child(odd)
父元素的某类型奇数节点：nth-of-type(odd)
6、偶数节点：p:nth-child(even)
父元素的某类型偶数节点：nth-of-type(even)

十、兄弟节点的选择
相邻兄弟关系：+（表示元素紧跟）
选择后续所有兄弟节点：~
