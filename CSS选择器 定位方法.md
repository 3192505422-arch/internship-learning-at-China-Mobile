# 🎯 CSS 选择器 · 定位方法

> Web 界面自动化，首先要 **定位到元素**，然后才能 **操作元素**。

---

## 一、常见操作

常见的操作不多，调用方式也比较固定：

| 操作 | 方法 | 说明 |
|------|------|------|
| 🖱️ 点击元素 | `.click()` | 模拟鼠标点击 |
| ⌨️ 输入文本 | `.fill('文本')` | 在元素内填入指定文本 |
| 📖 获取文本 | `.inner_text()` | 获取元素内部的可见文本 |

---

## 二、查找元素

> 按 <kbd>F12</kbd> 打开开发者工具 → 点击 **Elements（元素）** 标签 → 点击页面中对应的代码就可以定位元素。

### Locator 对象

`Page` 对象的 `locator` 定位到唯一的 HTML 元素后，就可以调用 Locator 对象的方法（如 `fill`、`click`、`inner_text`）进行操作。

### 根据标签名 / ID / Class 选择元素

#### ① 标签名（Tag）

CSS Selector 语法只需要直接写上 tag 名即可：

```python
locators = page.locator('div').all()   # 选择所有 tag 名为 div 的元素
```

#### ② ID 属性

在 ID 值前面加上 `#`：

```python
lct = page.locator('#searchtext')      # 选择 id="searchtext" 的元素
lct.fill('你好')
```

#### ③ Class 属性

在 class 值前面加上 `.`：

```python
page.locator('.animal')                # 选择 class="animal" 的元素
```

---

## 三、验证 CSS Selector

### 方法一：直接运行

```python
page.locator('#searchtext').fill('输入的文本')
```

### 方法二：浏览器开发者工具

1. <kbd>F12</kbd> 打开开发者工具
2. 点击 **Elements** 标签
3. 按 <kbd>Ctrl</kbd> + <kbd>F</kbd> 打开搜索框
4. 输入 CSS Selector 表达式

> 如果能匹配到元素，右侧会显示类似 `2 of 3` 的内容：
> - 表示当前表达式匹配到 **3 个元素**
> - 高亮显示的是其中 **第 2 个**

---

## 四、匹配多个元素

| 需求 | 方法 | 示例 |
|------|------|------|
| 获取所有匹配元素 | `.all()` | `page.locator('.plant').all()` |
| 获取匹配数量 | `.count()` | `page.locator('.plant').count()` |
| 第一个元素 | `.first` | `page.locator('.plant').first` |
| 最后一个元素 | `.last` | `page.locator('.plant').last` |
| 指定第 N 个元素 | `.nth(N)` | `page.locator('.plant').nth(1)` |

```python
# 获取所有
locators = page.locator('.plant').all()

# 统计数量
count = page.locator('.plant').count()

# 第一个和最后一个
lct = page.locator('.plant')
print(lct.first.inner_text(), lct.last.inner_text())

# 指定次序（从 0 开始）
lct = page.locator('.plant')
print(lct.nth(1).inner_text())
```

---

## 五、元素内部定位

可以通过 Locator 对象再调用 `locator`，实现在**某个元素内部**继续查找：

```python
lct = page.locator('#bottom')
# 在 #bottom 对应元素的范围内，寻找标签名为 span 的元素
eles = lct.locator('span').all()
for e in eles:
    print(e.inner_text())
```

---

## 六、选择子元素和后代元素

HTML 中，元素内部可以包含其他元素。

### 直接子元素 — `>`

```css
元素1 > 元素2
```

多层级：

```css
元素1 > 元素2 > 元素3 > 元素4    /* 最终选择元素4 */
```

### 后代元素 — 空格

```css
元素1 元素2
```

多层级：

```css
元素1 元素2 元素3 元素4          /* 最终选择元素4 */
```

---

## 七、根据属性选择

### ID 和 Class（快捷语法）

| 属性 | 语法 | 示例 |
|------|------|------|
| `id` | `#id值` | `#searchtext` |
| `class` | `.class值` | `.animal` |

### 其他属性 — 方括号 `[]`

| 需求 | 语法 | 示例 |
|------|------|------|
| 属性精确匹配 | `[属性="值"]` | `a[href="http://www.miitbeian.gov.cn"]` |
| 属性**包含**某字符串 | `[属性*="值"]` | `a[href*="miitbeian"]` |
| 属性**以某字符串开头** | `[属性^="值"]` | `a[href^="http"]` |
| 属性**以某字符串结尾** | `[属性$="值"]` | `a[href$=".gov.cn"]` |
| 同时满足多属性 | 连写 | `div[class=misc][ctype=gun]` |

> 前面可以加上标签名限制：`a[href="http://www.miitbeian.gov.cn"]` 表示选择所有标签为 `a` 且 href 属性值完全匹配的元素。

---

## 八、选择语法联合使用

### 且（并列）

将选择语法直接连写：

```css
div[class=misc][ctype=gun]    /* 同时满足这两个属性的 div 元素 */
```

### 或（逗号隔开）

用 `,` 分隔，表示"或"：

```css
span, div                      /* 选择所有 span 或 div 元素 */
```

> ⚠️ 注意优先级：`,` 优先级较高，且**不支持括号 `()`**。

---

## 九、按次序选择子节点

### 正序选择

| 语法 | 含义 | 示例 |
|------|------|------|
| `:nth-child(n)` | 父元素的第 n 个子节点 | `span:nth-child(2)` |
| `:nth-of-type(n)` | 父元素的第 n 个某类型子节点 | `span:nth-of-type(1)` |
| `:nth-child(odd)` | 父元素的奇数位子节点 | `p:nth-child(odd)` |
| `:nth-of-type(odd)` | 父元素的奇数位某类型子节点 | `p:nth-of-type(odd)` |
| `:nth-child(even)` | 父元素的偶数位子节点 | `p:nth-child(even)` |

### 倒序选择

| 语法 | 含义 | 示例 |
|------|------|------|
| `:nth-last-child(n)` | 父元素的倒数第 n 个子节点 | `p:nth-last-child(1)` |
| `:nth-last-of-type(n)` | 父元素的倒数第 n 个某类型子节点 | `p:nth-last-of-type(2)` |

```python
# 第 2 个子元素，且是 span 类型
span:nth-child(2)

# 直接写 :nth-child(2) —— 选择所有位置为第 2 的元素，不管类型
:nth-child(2)

# 第 1 个 span 类型的子元素
span:nth-of-type(1)

# 倒数第 2 个 p 元素
p:nth-last-of-type(2)
```

---

## 十、兄弟节点的选择

### 相邻兄弟 — `+`

选择**紧跟**在某个元素后面的兄弟元素：

```css
h2 + p      /* 选择紧跟在 h2 后面的第一个 p 元素 */
```

### 后续所有兄弟 — `~`

选择某个元素**后面所有**符合条件的兄弟元素：

```css
h2 ~ p      /* 选择 h2 后面所有的 p 兄弟元素 */
```

---
