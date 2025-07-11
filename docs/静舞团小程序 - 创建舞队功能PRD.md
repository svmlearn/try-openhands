
# 静舞团小程序 - 创建舞队功能PRD

## 1. 产品背景

### 1.1 功能定位
**功能名称：** 创建舞队  
**核心价值：** 让用户快速创建舞队，收集完整信息以支持排行榜筛选和社区功能  
**使用场景：** 领队级用户创建新舞队时使用  

### 1.2 设计原则
- **简洁专注**：只收集必要信息，流程简单顺畅
- **信息完整**：确保收集的信息能支持排行榜筛选功能
- **体验友好**：适合中老年用户的操作习惯

## 2. 用户需求分析

### 2.1 目标用户
**主要用户：** 广场舞领队（40-70岁中老年女性为主）

### 2.2 用户场景
- **场景1**：新组建舞队，需要在小程序中创建舞队档案
- **场景2**：已有线下舞队，首次使用小程序建档
- **场景3**：舞队搬迁到新地点，需要更新活动地点信息

### 2.3 用户目标
- 快速创建舞队档案
- 完善舞队基本信息
- 设置活动地点以便其他用户发现
- 为后续舞队管理功能做准备

## 3. 功能需求

### 3.1 页面结构
```
创建舞队页面
├── 页面标题：创建舞队
├── 基本信息区域
│   ├── 舞队名称（必填）
│   ├── 舞队头像（可选）
│   ├── 舞队简介（可选）
│   └── 舞种选择（必填）
├── 活动地点区域
│   ├── 地区选择（必填）
│   ├── 详细地址（可选）
│   └── 一键定位按钮
└── 底部操作区
    ├── 取消按钮
    └── 创建舞队按钮
```

### 3.2 字段详细说明

#### 3.2.1 基本信息区域

**舞队名称**
- 字段类型：文本输入框
- 必填项：是
- 字符限制：2-20个字符
- 唯一性：全局唯一
- 校验规则：不能包含特殊字符，不能与已有舞队重名
- 错误提示：名称已存在时显示"该舞队名称已被使用，请换一个"

**舞队头像**
- 字段类型：图片上传
- 必填项：否
- 默认头像：系统提供默认头像
- 图片要求：支持JPG/PNG，大小不超过2MB
- 上传方式：相机拍摄或相册选择

**舞队简介**
- 字段类型：多行文本框
- 必填项：否
- 字符限制：0-200个字符
- 占位符文本：介绍一下你们的舞队吧~

**舞种选择**
- 字段类型：单选或多选下拉框
- 必填项：是
- 选项来源：系统预设舞种列表
- 舞种选项：广场舞、民族舞、现代舞、拉丁舞、交谊舞、健身操、太极等
- 支持多选：是（最多选择3个）

#### 3.2.2 活动地点区域

**地区选择**
- 字段类型：三级联动选择器
- 必填项：是
- 选择层级：省份 → 城市 → 区域
- 数据来源：国家标准行政区划
- 默认状态：未选择

**详细地址**
- 字段类型：文本输入框
- 必填项：否
- 字符限制：0-50个字符
- 占位符文本：如桃园小区、中山公园等（方便队员找到你们）
- 用途说明：帮助其他用户更精确地找到活动地点

**一键定位**
- 功能类型：定位按钮
- 授权要求：需要获取位置权限
- 功能描述：自动获取当前位置并填写到省市区字段
- 失败处理：定位失败时提示用户手动选择地区

## 4. 交互设计

### 4.1 页面流程
```
进入页面 → 填写基本信息 → 选择活动地点 → 点击创建 → 创建成功 → 跳转舞队管理页
```

### 4.2 关键交互

#### 4.2.1 舞队名称校验
- **实时校验**：用户输入时实时检查名称是否已存在
- **校验反馈**：已存在时在输入框下方显示红色提示文字
- **可用性提示**：名称可用时显示绿色对勾

#### 4.2.2 头像上传
- **上传触发**：点击头像区域触发上传
- **选择方式**：弹出选择框（拍摄照片/从相册选择）
- **上传反馈**：显示上传进度，成功后显示预览图
- **重新上传**：可以重新选择替换头像

#### 4.2.3 舞种选择
- **展示方式**：点击展开下拉选择框
- **多选支持**：支持选择多个舞种（最多3个）
- **选择反馈**：已选择的舞种以标签形式显示
- **取消选择**：可以点击标签取消选择

#### 4.2.4 地区选择
- **选择方式**：三级联动选择器，逐级选择
- **联动逻辑**：选择省份后显示对应城市，选择城市后显示对应区域
- **选择确认**：选择完成后自动收起选择器并显示结果

#### 4.2.5 一键定位
- **权限请求**：首次使用时请求位置权限
- **定位过程**：显示"定位中..."加载状态
- **定位成功**：自动填写省市区字段
- **定位失败**：提示"定位失败，请手动选择地区"

### 4.3 表单提交
- **提交条件**：必填字段均已填写
- **提交按钮状态**：未满足条件时按钮置灰不可点击
- **提交过程**：显示"创建中..."加载状态
- **提交成功**：显示成功提示，跳转到舞队管理页面
- **提交失败**：显示错误信息，允许重新提交

## 5. 数据结构

### 5.1 提交数据格式
```json
{
  "name": "舞队名称",
  "avatar": "头像图片URL（可选）",
  "description": "舞队简介（可选）",
  "danceTypes": ["广场舞", "民族舞"],
  "location": {
    "province": "广东省",
    "city": "深圳市",
    "district": "南山区",
    "detail": "桃园小区广场（可选）"
  }
}
```

### 5.2 返回数据格式
```json
{
  "code": 200,
  "message": "创建成功",
  "data": {
    "teamId": "团队ID",
    "name": "舞队名称",
    "status": "active"
  }
}
```

## 6. 接口需求

### 6.1 舞队名称校验
```
GET /api/teams/check-name?name=舞队名称
返回：{ "available": true/false }
```

### 6.2 获取舞种列表
```
GET /api/dance-types
返回：[{ "id": 1, "name": "广场舞" }]
```

### 6.3 获取地区数据
```
GET /api/regions?parentId=0  // 获取省份
GET /api/regions?parentId=1  // 获取某省的城市
```

### 6.4 位置解析
```
POST /api/location/parse
参数：{ "latitude": 纬度, "longitude": 经度 }
返回：{ "province": "省", "city": "市", "district": "区" }
```

### 6.5 创建舞队
```
POST /api/teams
参数：舞队信息对象
返回：创建结果
```

## 7. 异常处理

### 7.1 网络异常
- **提交失败**：显示"网络异常，请重试"，保留用户填写的信息
- **图片上传失败**：显示"图片上传失败，请重试"
- **定位失败**：显示"定位失败，请手动选择地区"

### 7.2 数据校验异常
- **名称重复**：显示"该舞队名称已被使用，请换一个"
- **必填项缺失**：高亮缺失字段，显示相应提示

### 7.3 权限异常
- **位置权限拒绝**：提示用户可手动选择地区，不影响流程继续

## 8. 成功标准

### 8.1 用户体验指标
- 页面加载时间 < 2秒
- 表单填写完成率 > 90%
- 创建成功率 > 95%

### 8.2 功能完整性
- 创建的舞队信息完整，能支持排行榜筛选
- 地区信息准确，定位功能可用
- 舞种信息完整，便于分类展示

## 9. 后续功能入口

### 9.1 创建成功后的引导
创建成功后跳转到舞队管理页面，并提供以下功能入口：
- 邀请队员加入
- 完善更多舞队信息
- 绑定设备（扫码绑定）
- 创建/管理歌单

## 10. 优先级

### 10.1 P0（必须实现）
- 基本信息填写（舞队名称、舞种）
- 活动地点设置（省市区选择）
- 创建提交流程

### 10.2 P1（重要）
- 头像上传功能
- 一键定位功能
- 舞队名称实时校验

### 10.3 P2（优化项）
- 舞队简介、详细地址等选填项
- 表单数据本地缓存
- 创建失败后的数据恢复
