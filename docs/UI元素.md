# 静舞团小程序UI设计规范

## 全局设计规范 (基于 Figma 及 display.html)

### 核心配色

| 用途 | 颜色名称 | HEX / 值 | CSS 变量 |
| --- | --- | --- | --- |
| 主题色/高亮 | 经典亮橙 | `#FF9400` | `var(--brand-primary)` |
| 主要文字 | 深灰色 | `#454545` | `var(--text-primary)` |
| 次要/提示文字 | 中灰色 | `#8F8F94` | `var(--text-secondary)` |
<!-- | 通用页面背景 | 暖米色 | `#FFF9F5` | `var(--bg-light)` | -->
| 播放页背景 | 橙白渐变 | `linear-gradient(180deg, #FF9400 0%, #FFFFFF 93.27%)` | - |
| 卡片/组件背景 | 纯白色 | `#FFFFFF` | `var(--bg-white)` |
| 元素背景/禁用 | 浅灰色 | `#E5E5EB` | `var(--bg-gray-light)` |
| 操作-确认 | 经典蓝 | `#007AFF` | `var(--action-blue)` |

### 字体层次

| 级别 | 用途 | 字号 | 字重 (Font-Weight) | 颜色 |
| --- | --- | --- | --- | --- |
| H1 / 页面标题 | 页面主标题, 如"欢乐的舞步" | `24px` | `700` (Bold) | `var(--text-primary)` |
| H2 / 副标题 | 歌曲类型, 如"经典广场舞曲" | `16px` | `400` (Regular) | `var(--text-secondary)` |
| H3 / 按钮文字 | 主要按钮, 如"播放列表" | `20px` | `600` (Semibold) | `var(--text-primary)` / `var(--text-white)` |
| 正文 (Body) | 时间标签等普通文本 | `14px` | `400` (Regular) | `var(--text-secondary)` |
| 小字 (Caption) | 底部导航文字 | `12px` | `400` (Regular) | `var(--text-secondary)` / `var(--brand-primary)`|

### 小程序UI布局规范

基于微信小程序的标准UI结构，所有页面顶部应遵循以下布局规范：

#### 顶部留白结构

| 层级 | 高度 | 背景色 | 内容说明 | CSS 实现 |
| --- | --- | --- | --- | --- |
| 状态栏 | `44px` | `#FFFFFF` | 系统时间、电池电量等系统信息 | `.status-bar` |
| 小程序导航栏 | `44px` | `#FFFFFF` | 小程序标题 + 右侧操作按钮 | `.miniprogram-nav` |
| **总顶部留白高度** | **`88px`** | **纯白色** | **微信小程序标准顶部区域** | - |

#### 小程序导航栏元素

| 元素 | 位置 | 样式规范 | 说明 |
| --- | --- | --- | --- |
| 小程序标题 | 左侧 | `font-size: 16px; font-weight: 500; color: #1F2937` | 显示小程序名称 |
| 三个点按钮 | 右侧 | `color: #6B7280` | 小程序菜单 `⋯` |
| 关闭按钮 | 右侧 | `border: 1px solid #9CA3AF; border-radius: 50%` | 小程序关闭 `⭘` |

#### 页面内容区域高度计算

根据顶部留白结构，页面主内容区域的高度应使用以下公式：

```css
/* 标准页面内容区域 */
.main-content {
    height: calc(100% - 44px - 44px - 83px);
    /* 100% - 状态栏 - 小程序导航栏 - 底部导航栏 */
}

/* 有标题栏的页面内容区域 */
.page-content {
    height: calc(100% - 44px - 44px - 56px - 83px);
    /* 100% - 状态栏 - 小程序导航栏 - 页面标题栏 - 底部导航栏 */
}
```

#### 实现示例

```css
/* 状态栏样式 */
.status-bar { 
    height: 44px; 
    color: var(--text-dark); 
    background-color: #FFFFFF;
    padding: 0 16px;
}

/* 小程序导航栏样式 */
.miniprogram-nav {
    height: 44px;
    background-color: #FFFFFF;
    padding: 0 16px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}
```

```html
<!-- 微信小程序标准顶部结构 -->
<!-- 状态栏 -->
<div class="status-bar flex justify-between items-center">
    <div class="text-base font-semibold">10:00</div>
    <div class="flex items-center space-x-2">
        <span class="text-base font-medium">88%</span>
        <i class="fas fa-battery-three-quarters"></i>
    </div>
</div>

<!-- 小程序导航栏 -->
<div class="miniprogram-nav flex justify-between items-center">
    <div class="text-base font-medium text-gray-800">静舞团小程序</div>
    <div class="flex items-center space-x-2">
        <i class="fas fa-ellipsis-h text-gray-600"></i>
        <div class="w-6 h-6 border border-gray-400 rounded-full flex items-center justify-center">
            <i class="fas fa-times text-xs text-gray-600"></i>
        </div>
    </div>
</div>
```

## 微信小程序UI真实模拟完整方案

### 核心原理

微信小程序的UI包含两部分：
1. **页面内容区域**：可以有各种背景色、渐变等
2. **系统UI区域**：始终为纯白色不透明，包含状态栏和小程序导航栏

### 完美模拟实现方法

使用**固定定位覆盖层**技术，在页面顶部添加一个纯白色不透明的固定覆盖层来模拟小程序UI：

#### 1. CSS样式定义

```css
/* 全局重置样式 - 确保页面100%契合框架 */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
}

/* 页面背景 - 可以是任何颜色或渐变 */
body {
    background: linear-gradient(180deg, #FF9400 0%, #FFFFFF 93.27%);
    width: 375px;
    height: 812px;
    margin: 0;
    padding: 0;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    position: relative;
}

/* 小程序UI区域 - 固定在顶部的纯白色不透明覆盖层 */
.miniprogram-ui-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 88px;
    background-color: #FFFFFF;
    z-index: 1000;
    display: flex;
    flex-direction: column;
}

/* 状态栏样式 */
.status-bar {
    height: 44px;
    padding: 0 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #1F2937;
    font-size: 14px;
    font-weight: 600;
}

/* 小程序导航栏样式 */
.miniprogram-nav {
    height: 44px;
    padding: 0 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.miniprogram-title {
    font-size: 16px;
    font-weight: 500;
    color: #1F2937;
}

.miniprogram-actions {
    display: flex;
    align-items: center;
    gap: 12px;
}

.miniprogram-menu {
    color: #6B7280;
    font-size: 18px;
}

.miniprogram-close {
    width: 24px;
    height: 24px;
    border: 1px solid #9CA3AF;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #6B7280;
    font-size: 12px;
}
```

#### 2. HTML结构实现

```html
<body>
    <!-- 小程序UI覆盖层 - 固定在顶部的纯白色区域 -->
    <div class="miniprogram-ui-overlay">
        <!-- 状态栏 -->
        <div class="status-bar">
            <div>10:00</div>
            <div style="display: flex; align-items: center; gap: 8px;">
                <span>88%</span>
                <i class="fas fa-battery-three-quarters"></i>
            </div>
        </div>
        
        <!-- 小程序导航栏 -->
        <div class="miniprogram-nav">
            <div class="miniprogram-title">静舞团小程序</div>
            <div class="miniprogram-actions">
                <i class="fas fa-ellipsis-h miniprogram-menu"></i>
                <div class="miniprogram-close">
                    <i class="fas fa-times"></i>
                </div>
            </div>
        </div>
    </div>

    <!-- 页面内容区域 - 从系统UI下方开始，保留88px顶部空间 -->
    <div class="pt-[88px] flex flex-col h-screen">
        <!-- 页面具体内容... -->
    </div>
</body>
```

#### 3. 关键技术要点

| 技术点 | 说明 | 重要性 |
| --- | --- | --- |
| **全局重置样式** | `* { margin: 0; padding: 0; box-sizing: border-box; }` 确保页面100%契合框架 | ⭐⭐⭐⭐⭐ |
| `html, body` 重置 | 确保html和body无默认边距，占满整个容器 | ⭐⭐⭐⭐⭐ |
| `position: fixed` | 确保UI覆盖层始终固定在顶部 | ⭐⭐⭐⭐⭐ |
| `z-index: 1000` | 确保UI覆盖层显示在所有内容之上 | ⭐⭐⭐⭐⭐ |
| `background-color: #FFFFFF` | 纯白色不透明背景 | ⭐⭐⭐⭐⭐ |
| `pt-[88px]` | 为页面内容预留顶部空间 | ⭐⭐⭐⭐⭐ |
| `height: 88px` | 精确的小程序UI高度（44px+44px） | ⭐⭐⭐⭐ |

#### 4. 适用场景

✅ **适用于所有页面类型**：
- 有背景渐变的页面（如播放页、舞队详情页）
- 纯色背景页面
- 有复杂背景图案的页面
- 需要完美模拟小程序外观的所有情况

✅ **完美解决的问题**：
- **页面100%契合框架**：消除所有默认边距和内边距
- 页面背景保持设计效果
- 顶部UI区域始终为纯白色不透明
- 完全符合微信小程序真实外观
- 页面内容正确布局，不被UI覆盖层遮挡

#### 5. 实际效果

使用此方案后的页面效果：
- **顶部88px**：纯白色不透明的小程序UI（状态栏+导航栏）
- **下方区域**：保持原有页面背景和内容布局
- **整体效果**：完美模拟微信小程序的真实UI外观

这个方案已在 `team-detail-guest.html` 和 `team-page-without-team.html` 中成功验证和应用。

## display.html 的配色
使用经典的亮橙色 #FF9500 作为主色，并结合其他调整建议来修改 display.html 的配色。
核心调整思路：
背景: 页面背景 暖米色 #FFF9F5，卡片 纯白 #FFFFFF。
文本: 主要文本 深灰 #333333，次要文本 中灰 #757575。
主操作 (播放/暂停, 播放列表): 亮橙色 #FF9500。
次操作 (添加歌曲, 连接蓝牙): 经典蓝 #007AFF。
辅助操作 (扫码): 经典绿 #34C759。
进度条/点缀: 可以用之前的 暖橙珊瑚色 #F5846A 或稍浅的橙色。
分割线: 暖灰 #E5DCD8。

## display.html 中播放列表部分 (#playlistContent) 的配色，使其与我们更新后的经典亮橙色方案保持一致。
主要改动：
列表页背景: 将 #playlistContent 的背景从 bg-white 改为 bg-light #FFF9F5，与播放页保持一致。
头部区域:
确保头部（返回按钮、标题）的背景也是 bg-light。
返回按钮的文字和图标颜色使用 text-link #FF9500。
标题使用 text-primary #333333。
歌曲列表项:
默认状态下的歌曲图标背景从 bg-gray-100 改为 bg-gray #F7F2EE，图标颜色为 text-secondary #757575。
激活状态下的歌曲图标背景为 brand-primary #FF9500，图标颜色为 text-white #FFFFFF。
删除按钮 (.delete-button) 默认颜色为 text-secondary #757575，可以考虑在 :hover 或 :active 状态下变为 error #F5605C （但这在触摸设备上效果不明显，保持灰色也可）。

## community.html 应用新的配色方案。
主要调整：
添加 CSS 变量: 在 <style> 标签内添加了 :root 规则，定义了新的颜色变量。
页面背景: body 背景色改为 var(--bg-light)。
状态栏/标题栏: 保持白色背景，调整文字颜色为 var(--text-primary) 和 var(--text-secondary)。
搜索栏: 背景色改为更柔和的 rgba 或 var(--bg-gray)，图标和文字颜色调整为 var(--text-secondary) 和 var(--text-primary)，聚焦时的 box-shadow 颜色调整为 var(--brand-primary) 的 rgba。
舞种筛选按钮:
激活状态 (全部): 背景色 var(--brand-primary)，文字 var(--text-white)。
非激活状态: 背景色 var(--bg-white)，边框和文字颜色使用对应的功能色或主题色（如橙、绿、紫、红）。这里为紫色和红色定义了新的变量 --tag-purple-text, --tag-purple-bg, --tag-red-text, --tag-red-bg 等，你也可以根据需要映射到现有颜色。
舞队卡片: 背景 var(--bg-white)，文字 var(--text-primary)，阴影颜色调整，激活状态背景 var(--bg-gray)。
舞种标签: 背景色和文字颜色调整，使用更柔和的搭配。
分区标题下划线: 颜色改为 var(--brand-primary)。
底部导航栏: 背景色 var(--bg-blur)，图标和文字颜色调整，激活状态使用 var(--brand-primary)。


## team-detail-guest.html，应用新的配色方案。
主要调整：
添加 CSS 变量: 在 <style> 标签内添加了 :root 规则，定义了新的颜色变量。
页面背景: body 背景色改为 var(--bg-light)。
状态栏: 因为状态栏文字是在图片上，通常需要根据图片颜色调整，这里暂时保持白色文字。
返回按钮: 背景改为半透明白色，图标改为 var(--text-primary)。
封面文字/头像: 封面上的舞队名称用白色。舞队头像和成员头像的边框用白色。
成员数量/头像占位符: 文字颜色 var(--text-secondary)，头像占位符背景 var(--bg-gray)。
加入舞队按钮: 背景 var(--brand-primary)，文字 var(--text-white)。
信息卡片/介绍区域: 背景 var(--bg-white)，标签文字 var(--text-secondary)，值文字 var(--text-primary)。
舞种标签: 定义了一个蓝色的标签样式作为示例，背景 var(--bg-blue-light)，文字 var(--action-blue)。
分区标题: 文字 var(--text-primary)，下划线 var(--border-base)。
提示文字: 背景 var(--bg-gray)，文字 var(--text-secondary)。

# team-page-without-team.html，应用新的配色方案。
主要调整：
添加 CSS 变量: 在 <style> 标签内添加了 :root 规则，定义了新的颜色变量。
页面和元素背景: body 背景设为 var(--bg-light)，状态栏、标题栏和操作卡片背景设为 var(--bg-white)。
文字颜色: 标题栏文字、操作卡片文字、提示文字等，分别设为 var(--text-primary) 和 var(--text-secondary)。
操作卡片:
边框: 改为 var(--border-base) 的虚线。
"创建舞队"图标背景: var(--bg-highlight) (浅橙色)，图标颜色: var(--brand-primary) (橙色)。
"加入舞队"图标背景: var(--tag-blue-bg) (浅蓝色)，图标颜色: var(--action-blue) (蓝色)。
Active 状态: 背景改为 var(--bg-gray)。
底部导航:
背景: var(--bg-blur) (暖米色模糊)。
顶部边框: var(--divider)。
未选中项: var(--text-secondary)。
选中项 ("舞队"): var(--brand-primary)。

# create-team-form.html 页面，应用新的颜色规范。
主要调整：
添加 CSS 变量: 在 <style> 标签内添加 :root 规则，定义新的颜色变量。
页面和组件背景:
body 背景设为 var(--bg-light)。
状态栏、标题栏、表单元素、设备卡片背景设为 var(--bg-white)。
文字和图标颜色:
主要文字（标题、标签、输入内容）使用 var(--text-primary)。
次要文字（提示、占位符）使用 var(--text-secondary)。
返回按钮图标、链接使用 var(--brand-primary)。
表单元素样式:
边框使用 var(--border-base)。
聚焦时边框和阴影使用 var(--brand-primary)。
复选框 accent-color 使用 var(--brand-primary)。
下拉选择框箭头图标颜色改为 var(--text-secondary) (在 SVG 数据 URI 中修改)。
图片上传区域:
边框使用 var(--border-base) (虚线)。
图标和提示文字使用 var(--text-secondary)。
悬停时边框使用 var(--brand-primary)，背景使用 var(--bg-highlight)。
按钮:
提交按钮背景 var(--brand-primary)，文字 var(--text-white)。
时间确认按钮背景 var(--brand-primary)，文字 var(--text-white)。
设备选择:
设备卡片选中边框 var(--brand-primary)。
星期/时间段未选中标签背景 var(--bg-gray)。
星期/时间段选中标签背景 var(--bg-highlight)，边框和文字 var(--brand-primary)。
提示框背景使用 var(--tag-blue-bg)，文字 var(--action-blue)。
警告框背景使用 var(--bg-highlight)，文字 var(--brand-primary)。

# team-join-record.html，应用新的配色方案。
主要调整：
添加 CSS 变量: 在 <style> 标签内添加 :root 规则，定义新的颜色变量。
背景色:
body: var(--bg-light)
状态栏、标题栏、列表项、弹窗内容、底部栏: var(--bg-white)
弹窗遮罩: 保持 rgba(0, 0, 0, 0.5)
文本颜色:
主要文本 (标题、列表项舞队名、弹窗标题/值): var(--text-primary)
次要文本 (弹窗标签、确认提示): var(--text-secondary)
状态角标/按钮上的白色文字: var(--text-white)
弹窗取消按钮文字: var(--text-primary)
边框/分割线: 使用 var(--border-base) 或 var(--divider)。
元素颜色:
状态角标 (Status Badge):
申请中 (pending): 背景 var(--action-blue)，文字 var(--text-white)。
已拒绝 (rejected): 背景 var(--bg-gray)，文字 var(--text-secondary)。
按钮 (Action Buttons):
取消申请按钮 (cancel-button): 背景 var(--error)，文字 var(--text-white)。 Active 状态为更深的红色。
底部返回按钮 (back-button): 背景 var(--brand-primary)，文字 var(--text-white)。 Active 状态为更深的橙色，添加阴影。
弹窗:
关闭按钮: 背景 var(--bg-gray)，图标 var(--text-secondary)。
确认按钮 (confirmCancelButton): 背景 var(--brand-primary)，文字 var(--text-white)。
取消按钮 (cancelConfirmButton): 背景 var(--bg-gray)，文字 var(--text-primary)。





