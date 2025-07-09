# 静舞团小程序UI设计文档

## 1. 项目概述

静舞团小程序是一款面向老年用户的舞蹈音乐工具类产品，旨在提供简单易用的音乐播放、歌单管理、舞队社区和团队管理功能。本项目采用HTML、CSS和JavaScript构建高保真UI原型，遵循iOS设计规范（iPhone 15 Pro风格），并特别关注适老化设计。

## 2. 文件架构

### 2.1 总体架构

- **index.html**: 项目总展示页面，包含所有功能模块的概览和导航
- **各功能模块HTML文件**: 按照功能模块分类的独立页面
- **共享资源**: 字体、图标、样式等

### 2.2 功能模块文件结构

#### 播放板块
- **display.html**: 播放器与歌单管理页面 (小程序首页)
- **device-schedule-simple.html**: 设备定时播放日程页

#### 社区板块
- **community.html**: 社区舞队列表页面
- **team-leaderboard.html**: 舞队魅力榜页面
- **team-detail-guest.html**: 舞队详情页面（未加入视图）
- **team-detail-member.html**: 舞队详情页面（已加入成员视图）
- **team-detail-leader.html**: 舞队详情页面（领队视图）

#### 团队板块
- **team-page-without-team.html**: 我的舞队页面（无舞队时）
- **team-management.html**: 舞队管理页面
- **join-team-form.html**: 加入舞队表单页面
- **create-team-form.html**: 创建舞队表单页面
- **team-application-record.html**: （用户）舞队申请记录页
- **team-join-record.html**: （用户）入队记录页

#### 个人中心板块
- **profile.html**: 个人中心页面

## 3. 设计要求

### 3.1 适老化设计原则

1. **字体大小**
   - 标题: 18-24px
   - 正文: 16-18px
   - 辅助文本: 14-16px
   - 避免使用过小的字体（<14px）

2. **触摸目标尺寸**
   - 按钮最小尺寸: 44×44像素
   - 列表项最小高度: 60像素
   - 交互元素间距: 至少8像素

3. **色彩对比**
   - 文本与背景对比度: 至少4.5:1
   - 使用明确的视觉层次
   - 避免仅依靠颜色传达信息

4. **简化界面**
   - 减少每个页面的元素数量
   - 使用清晰的视觉层次
   - 避免复杂的手势操作

5. **明确反馈**
   - 点击操作有明显的视觉反馈
   - 状态变化清晰可见
   - 提供足够的操作确认

6. **导航简化**
   - 使用大而明显的导航元素
   - 提供清晰的返回路径
   - 保持导航结构一致性

### 3.2 iOS设计规范

1. **视觉风格**
   - 遵循iOS 15+设计语言
   - 使用系统字体: SF Pro
   - 标准圆角半径: 8-12px

2. **颜色系统**
   - 主色: iOS蓝 (#007AFF)
   - 强调色: iOS橙 (#FF9500)
   - 中性色: iOS灰 (#8E8E93)
   - 背景色: iOS浅灰 (#F2F2F7)

3. **交互元素**
   - 标准iOS控件样式
   - 遵循iOS手势交互模式
   - 使用系统图标风格

## 4. 页面功能与关系

### 4.1 播放板块

#### 播放器页面 (display.html)
- **功能**: 显示当前播放歌曲、播放控制、音量调节，并集成歌单列表（当前歌单、我的收藏）的管理功能。
- **关系**: 作为小程序首页，可通过标签页切换到其他主要模块。

#### 设备定时播放日程页 (device-schedule-simple.html)
- **功能**: 管理硬件设备的播放日程，实现音乐的定时播放功能。
- **关系**: 属于高级功能，可能从设置或播放页面进入。

### 4.2 社区板块

#### 社区舞队列表页面 (community.html)
- **功能**: 浏览所有舞队、按舞种筛选、搜索舞队。页面中上部嵌入"舞队魅力榜"模块作为入口。
- **关系**: 点击舞队进入舞队详情页面，点击榜单入口进入舞队魅力榜页面。

#### 舞队魅力榜页面 (team-leaderboard.html)
- **功能**: 展示基于舞队活跃度、荣誉等多维度数据的综合排名列表。支持按地区筛选。
- **关系**: 从社区页面进入，可返回社区页面。

#### 舞队详情页面 (team-detail-*.html)
- **功能**: 查看舞队信息、成员列表、活动安排
- **关系**: 根据用户身份显示不同视图（未加入/成员/领队）

### 4.3 团队板块

#### 我的舞队页面 (team-page-without-team.html)
- **功能**: 用户未加入任何舞队时显示的页面，引导用户创建或发现舞队。
- **关系**: 从底部"团队"标签页进入。

#### 舞队管理页面 (team-management.html)
- **功能**: 成员管理、申请审批、舞队设置
- **关系**: 仅舞队领队可访问

#### 加入/创建舞队表单页面
- **功能**: 提交加入申请或创建新舞队
- **关系**: 从我的舞队页面或舞队详情页进入

#### 申请记录页面
- **功能**: 查看自己提交过的所有入队申请及其当前状态。
- **关系**: 从个人中心进入

### 4.4 个人中心板块

#### 个人中心页面 (profile.html)
- **功能**: 用户信息、收藏内容、活动记录、设置
- **关系**: 可管理个人资料和应用设置，并查看各类记录。

## 5. 开发规范

### 5.1 HTML结构规范

1. **基本结构**
   ```html
   <!-- 状态栏 -->
   <div class="status-bar">...</div>
   
   <!-- 标题栏/导航栏 -->
   <div class="nav-bar">...</div>
   
   <!-- 内容区域 -->
   <div class="content">...</div>
   
   <!-- 底部导航栏 -->
   <div class="bottom-nav">...</div>
   ```

2. **命名约定**
   - 使用语义化的类名
   - 组件名使用连字符分隔（如 `playlist-item`）
   - 状态类使用描述性名称（如 `active`, `disabled`）

### 5.2 CSS样式规范

1. **通用样式**
   - 使用Tailwind CSS框架
   - 自定义样式放在`<style>`标签中
   - 保持一致的颜色变量和间距

2. **适老化样式**
   - 大字体、大按钮
   - 高对比度
   - 明确的视觉反馈

3. **响应式设计**
   - 基于375px宽度设计
   - 确保在不同iPhone尺寸上正常显示

### 5.3 JavaScript交互规范

1. **简单交互**
   - 页面跳转
   - 标签页切换
   - 列表项点击

2. **表单处理**
   - 简化表单
   - 明确的错误提示
   - 大型输入控件

### 5.4 页面归类
- **index.html** (总展示页面)
- **display.html** (播放器与歌单管理)
- **community.html** (社区页面)
- **team-leaderboard.html** (舞队魅力榜页面)
- **team-detail-guest.html** (舞队详情-访客)
- **team-detail-member.html** (舞队详情-成员)
- **team-detail-leader.html** (舞队详情-领队)
- **team-page-without-team.html** (无舞队状态页)
- **team-management.html** (舞队管理页面)
- **join-team-form.html** (加入舞队表单)
- **create-team-form.html** (创建舞队表单)
- **team-application-record.html** (入队申请记录)
- **team-join-record.html** (入队记录)
- **profile.html** (个人中心页面)
- **device-schedule-simple.html** (设备日程页)

## 6. 开发计划

### 6.1 已完成页面
- index.html (总展示页面)
- community.html (社区页面)
- display.html (播放器页面)
- profile.html (个人中心页面)
- team.html (团队页面)
- playlist.html (歌单列表页面)
- team-detail-guest.html (舞队详情-访客)
- team-detail-member.html (舞队详情-成员)
- team-detail-leader.html (舞队详情-领队)
- team-leaderboard.html (舞队魅力榜页面)

### 6.2 待开发页面
- (暂无，当前所有HTML文件均已作为现有功能归档)

### 6.3 开发优先级
1. 核心功能页面完善与交互优化
2. 各页面间跳转逻辑校验
3. 整体测试和优化

## 7. 注意事项

1. **一致性**
   - 保持视觉风格一致
   - 保持交互模式一致
   - 保持导航结构一致

2. **简洁性**
   - 避免复杂的界面元素
   - 减少每个页面的信息量
   - 使用直观的图标和标签

3. **可访问性**
   - 考虑视力和触摸精度下降的用户
   - 提供足够的操作反馈
   - 避免依赖复杂手势

4. **性能**
   - 确保页面加载迅速
   - 优化图片和资源
   - 减少不必要的动画效果

## 8. 参考资源

- [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/WAI/standards-guidelines/wcag/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Font Awesome Icons](https://fontawesome.com/icons)

---

*文档版本: 1.0*  
*最后更新: 2023年3月10日*  
*作者: 静境智能声学科技有限公司*
