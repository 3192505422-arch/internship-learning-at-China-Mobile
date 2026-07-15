# 🔄 Frame、Tab 切换与实用技巧

> 多窗口、iframe、截屏、拖拽等进阶操作。

---

## 一、Frame 切换

有些元素位于 `<iframe>` 内部。在 HTML 语法中，`<frame>` 或 `<iframe>` 元素内部包含的是**被嵌入的另一份 HTML 文档**。

Playwright 默认操作范围是**当前的 HTML 文档**，不包含被嵌入的文档。如果要操作 iframe 内部的元素，就需要**切换操作范围**到被嵌入的文档中。

```python
# 产生一个 FrameLocator 对象
frame = page.frame_locator("iframe[src='sample1.html']")
# frame_locator 的参数是 CSS 或 XPath 选择器

# 在 iframe 内部进行定位
lcs = frame.locator('.plant').all()
for lc in lcs:
    print(lc.inner_text(timeout=1000))
```

> 💡 使用 Page 或 Locator 对象的 `frame_locator` 方法定位到目标 frame，这个方法会返回一个 **FrameLocator** 对象，后续定位都通过这个对象在其内部进行。

---

## 二、窗口切换

用 Playwright 打开新窗口后，即使新窗口打开了，原来的 `page` 变量对应的**还是老窗口**，自动化操作也还是在老窗口进行。

这时需要使用 **BrowserContext** 对象来管理多个窗口：

```python
from playwright.sync_api import sync_playwright

pw = sync_playwright().start()
browser = pw.chromium.launch(headless=False)

# 创建 BrowserContext 对象
context = browser.new_context()

# 通过 context 创建 page
page = context.new_page()
page.goto("https://www.byhy.net/cdn2/files/selenium/sample3.html")

# 点击链接，打开新窗口
page.locator("a").click()

# 等待 2 秒（不能用 time.sleep）
page.wait_for_timeout(2000)

# pages 属性是所有窗口对应 Page 对象的列表
newPage = context.pages[1]

# 打印新网页窗口标题
print(newPage.title())

# 打印老网页窗口标题
print(page.title())
```

### 根据标题定位窗口

如果打开了多个窗口，不知道目标窗口的次序，可以根据标题来查找：

```python
for pg in context.pages:
    # 根据标题栏字符串判断是否是要操作的窗口
    if '必应' in pg.title():
        break

print(pg.title())
```

### 窗口管理操作

| 操作 | 方法 | 说明 |
|------|------|------|
| 🎯 设为当前活动窗口 | `page.bring_to_front()` | 把指定窗口显示到最前面 |
| ❌ 关闭窗口 | `page.close()` | 关闭该 Page 对应的窗口 |

---

## 三、冻结界面

在开发者工具的 **Console（控制台）** 中执行以下 JS 代码，可以冻结界面（5 秒后界面被冻住，点击不会触发事件）：

```javascript
setTimeout(function(){ debugger; }, 5000)
```

> 常用于调试：在 5000 毫秒后执行 `debugger` 命令，界面暂停，方便检查元素状态。

---

## 四、截屏

### 整页截屏

```python
# 截屏当前页面可见内容，保存到 ss1.png
page.screenshot(path='ss1.png')

# 截屏完整页面（超过窗口高度的部分也包括）
page.screenshot(path='ss1.png', full_page=True)
```

### 元素截屏

```python
# 只对某个元素的显示内容进行截屏
page.locator('input[type=file]').screenshot(path='ss2.png')
```

---

## 五、拖拽

### 使用 Page 对象的 drag_and_drop 方法

```python
# 选中 span#t1 文本内容
page.locator('#t1').select_text()

# 拖拽到输入框 [placeholder="captcha"] 里面
page.drag_and_drop('#t1', '[placeholder="captcha"]')
```

### 使用 Locator 对象的 drag_to 方法

```python
# 选中 span#t1 文本内容
lc = page.locator('#t1')
lc.select_text()

# 拖拽到指定目标
lc.drag_to(page.locator('[placeholder="captcha"]'))
```

---

## 六、弹出对话框

Playwright 可以通过监听 `dialog` 事件来处理各种弹出框。

### Alert 弹出框

显示通知信息，只需点击 **确定**。

| 方法/属性 | 说明 |
|-----------|------|
| `dialog.accept()` | 等同于点击**确定**按钮 |
| `dialog.dismiss()` | 等同于点击**取消**按钮 |
| `dialog.message` | 对话框的提示信息 |

### Confirm 弹出框

让用户确认是否执行操作，有两个选项：**确定** / **取消**。

| 方法 | 说明 |
|------|------|
| `dialog.accept()` | 点击**确定** |
| `dialog.dismiss()` | 点击**取消** |

### Prompt 弹出框

需要用户输入信息并提交。

```python
dialog.accept('要输入的信息')  # 接受并输入文本
```

---
