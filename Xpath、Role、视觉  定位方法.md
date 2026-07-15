# 🔍 XPath、Role、视觉 定位方法

> 除了 CSS 选择器之外，Playwright 还支持多种其他定位方式。

---

## 一、XPath 定位

**XPath**（XML Path Language）是由 W3C 指定的，用于在 XML 和 HTML 文档中选择节点的语言。

Playwright 的 Locator 也支持 XPath 语法：

```python
element = page.locator('//*[@href="http://www.miitbeian.gov.cn"]')
```

等价于 CSS 选择器写法：

```python
element = page.locator('[href="http://www.miitbeian.gov.cn"]')
```

---

## 二、根据文本内容定位

当需要用页面中的**文字内容**来定位元素时，CSS 选择器不太方便，可以使用 `get_by_text` 方法。

### 精确匹配

```python
from playwright.sync_api import sync_playwright

p = sync_playwright().start()

browser = p.chromium.launch(headless=False)
page = browser.new_page()
page.goto("https://www.byhy.net/cdn2/files/selenium/stock1.html")

# 根据文本内容选择所有包含 "11" 的元素
elements = page.get_by_text('11').all()

# 打印出元素文本
for ele in elements:
    print(ele.inner_text())
```

### 正则匹配

```python
import re

# 选择文本以 "11" 结尾的元素
elements = page.get_by_text(re.compile("11$")).all()
```

> 💡 `re.compile("11$")` 中的 `$` 表示以 `11` 结尾

---

## 三、根据元素 Role 定位

Playwright 支持根据元素的 **ARIA Role** 属性来定位元素。

### 什么是 ARIA Role？

ARIA 为 Web 元素增加了一些与角色（Role）相关的属性定义，方便辅助技术工具（如屏幕阅读器）识别和操作。

例如，系统注册成功时的弹窗：

```html
<!-- 没有 ARIA 属性 —— 辅助技术难以识别重要性 -->
<div class="alert-message">
  您已成功注册，很快您将收到一封确认电子邮件
</div>

<!-- 加上 ARIA role —— 辅助技术可以识别出这是重要信息 -->
<div class="alert-message" role="alert">
  您已成功注册，很快您将收到一封确认电子邮件
</div>
```

### 通过 Role 定位

```python
# 根据 role 定位 alert 元素
lc = page.get_by_role('alert')

# 打印元素文本
print(lc.inner_text())
```

> ℹ️ 有些特定的语义元素（如 `<button>`、`<nav>`）被 ARIA 规范认定为**自身就包含 ARIA role 信息**，不需要显式添加 `role` 属性。

> 🤖 **最佳实践：** ARIA 规则比较复杂，建议使用 Playwright 的 codegen 代码助手。codegen 生成代码时，能使用 role 定位的会**优先使用 role 定位**。
>
> ```bash
> playwright codegen
> ```

---

## 四、其他用户视觉定位

以下 4 种定位方式都可以通过 codegen 自动生成，实际上也可以用 CSS 选择器替代：

| 定位方式 | 说明 | CSS 选择器等价写法 |
|----------|------|-------------------|
| ① Placeholder | 根据输入框的占位文本定位 | `[placeholder="文本"]` |
| ② Label | 根据关联的 label 标签定位 | `#label-id` 或 `[aria-label="文本"]` |
| ③ All text | 根据元素内所有文本定位 | 可用 `:has-text()` |
| ④ Title | 根据元素的 title 属性定位 | `[title="文本"]` |

---

## 五、缺省等待时间

Playwright 中，当我们定位元素并执行操作（如 `click`、`fill`）时，如果当时找不到该元素，Playwright **不会立即报错**，而是默认等待 **30 秒**。如果 30 秒内元素出现，立即操作成功返回。

### 默认行为

```python
from playwright.sync_api import sync_playwright

p = sync_playwright().start()
browser = p.chromium.launch(headless=False)
page = browser.new_page()
page.goto("https://www.byhy.net/cdn2/files/selenium/stock1.html")
page.locator('#kw').fill('通讯\n')
page.locator('#go').click()

element = page.locator("[id='1']")
print(element.inner_text())
```

### 修改等待时间

```python
from playwright.sync_api import sync_playwright

p = sync_playwright().start()
browser = p.chromium.launch(headless=False, slow_mo=50)
context = browser.new_context()
context.set_default_timeout(10)  # 修改缺省等待时间为 10 毫秒

page = context.new_page()  # 通过 context 创建 Page 对象
page.goto("https://www.byhy.net/cdn2/files/selenium/stock1.html")
page.locator('#kw').fill('通讯\n')
page.locator('#go').click()

element = page.locator("[id='1']")
print(element.inner_text())
```

> ⚙️ **参数说明：**
> - `slow_mo=50` — 操作之间延迟 50 毫秒，让操作节奏慢一点，便于观察
> - `set_default_timeout(10)` — 将元素等待超时改为 10 毫秒（慎用，太短容易超时报错）

---

> 🔍 **XPath、Role、视觉 定位方法** — 由 Echo 🔥 美化整理
