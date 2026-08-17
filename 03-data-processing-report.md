# 工作流 03：数据整理与报表自动生成

> 难度：★★★☆☆ | 搭建时间：2-3小时 | 首选平台：n8n / Coze

---

## 一、场景描述

### 谁需要这个工作流

**典型用户画像**：
- 电商运营，每天要从淘宝/拼多多/抖音后台导出数据，汇总成日报
- 企业行政/HR，需要汇总考勤、绩效、招聘数据做周报月报
- 销售团队管理者，需要整合 CRM、财务、库存数据做销售分析
- 财务人员，每月需要从多个系统拉数据做对账和财务报表

### 解决什么问题

**痛点**：
1. 每天花 1-2 小时从不同系统导数据、复制粘贴、做表格
2. 数据格式不统一，手工清洗容易出错
3. 报表内容千篇一律，没有真正的数据洞察
4. 领导要的数据总是很急，来不及做分析

**解决方案**：
搭建数据自动化工作流——定时从各数据源拉取数据，自动清洗整合，用 AI 生成数据洞察和文字解读，输出可视化报表，推送给相关人员。

---

## 二、触发条件

| 触发方式 | 说明 | 适用场景 |
|---------|------|---------|
| 每日定时 | 每天固定时间（如 9:00）生成日报 | 日报 |
| 每周定时 | 每周一早上生成上周周报 | 周报 |
| 每月定时 | 每月 1 日生成上月月报 | 月报 |
| 手动触发 | 点击按钮即时生成 | 临时报表 |

---

## 三、完整处理步骤

```
定时触发
    ↓
[步骤1] 数据采集 — 从多个数据源拉取数据
    ├─ 数据源1：电商后台（淘宝/拼多多/抖音）
    ├─ 数据源2：CRM 系统（客户/销售数据）
    ├─ 数据源3：财务系统（收入/成本数据）
    ├─ 数据源4：广告平台（投放数据）
    └─ 数据源5：Google Analytics / 百度统计
    ↓
[步骤2] 数据清洗 — 去重、格式化、异常值处理
    ↓
[步骤3] 数据整合 — 多源数据合并到统一表格
    ↓
[步骤4] 指标计算 — 计算环比、同比、达成率等衍生指标
    ↓
[步骤5] AI 分析 — 生成数据洞察和文字解读
    ├─ 整体趋势分析
    ├─ 异常数据识别
    ├─ 同比环比变化
    └─ 建议与行动项
    ↓
[步骤6] 报表生成 — 输出可视化报表（图表 + 文字）
    ↓
[步骤7] 报表推送 — 发送到飞书/企业微信/邮箱
```

---

## 四、输出结果

### 直接输出

1. **标准化数据表**：清洗后的数据写入飞书/Google Sheets
2. **可视化报表**：包含图表的数据看板
3. **AI 数据洞察**：文字版的数据分析和建议
4. **推送通知**：报表链接发送到相关人

### 日报示例结构

```
📊 日报 | 2026年8月15日 | XX 电商运营

一、核心指标速览
- 销售额：¥45,230（环比 +12.3%）
- 订单量：326 单（环比 +8.5%）
- 客单价：¥138.7（环比 +3.5%）
- 退货率：3.2%（环比 -0.5%）

二、趋势图表
[销售额7日趋势折线图]
[各渠道销售占比饼图]

三、AI 数据洞察
1. 销售额连续 3 天上涨，主要由抖音渠道贡献（+28%）
2. 客单价提升原因是高单价商品 B 的销量增加了 45%
3. 退货率下降，售后流程优化见效
4. 需关注：拼多多渠道连续 2 天下滑，建议检查活动是否到期

四、今日建议行动项
- [ ] 检查拼多多渠道活动状态
- [ ] 追加商品 B 库存（按当前销量 3 天将售罄）
- [ ] 复制抖音渠道成功经验到视频号
```

---

## 五、推荐工具组合

### 方案 A：n8n + 飞书方案（推荐）

| 组件 | 工具 | 用途 |
|------|------|------|
| 工作流引擎 | n8n | 编排整体流程 |
| 数据采集 | 各平台 API + HTTP Request | 拉取多源数据 |
| 数据清洗 | n8n Code 节点（JavaScript） | 数据处理 |
| 数据存储 | 飞书多维表格 / Google Sheets | 存储清洗后数据 |
| AI 分析 | OpenAI GPT-4 API | 生成数据洞察 |
| 图表生成 | ECharts / QuickChart API | 生成图表图片 |
| 报表推送 | 飞书机器人 / 企业微信 / 邮件 | 推送报表 |

### 方案 B：Coze 工作流方案（适合轻量需求）

| 组件 | 工具 | 用途 |
|------|------|------|
| 工作流引擎 | Coze 工作流 | 编排流程 |
| 数据采集 | Coze 插件（HTTP请求） | 拉取数据 |
| 数据处理 | Coze 代码节点 | 清洗数据 |
| AI 分析 | Coze 内置 LLM | 生成洞察 |
| 报表推送 | Coze 飞书插件 | 推送到飞书 |

---

## 六、详细配置步骤（以 n8n + 飞书为例）

### 准备工作

1. 注册 n8n 账号
2. 准备数据源 API：
   - 淘宝开放平台 API（或用 RPA 工具导出数据）
   - 抖音电商开放平台 API
   - Google Analytics API
   - 企业自建系统 API
3. 注册 OpenAI API
4. 创建飞书多维表格用于存储数据

### 第 1 步：创建飞书数据表

创建「运营日报数据表」，字段如下：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| 日期 | 日期 | 数据日期 |
| 销售额 | 数字 | 当日销售额 |
| 订单量 | 数字 | 当日订单数 |
| 客单价 | 数字 | 自动计算 |
| 退货率 | 百分比 | 退货订单占比 |
| 抖音渠道销售额 | 数字 | 分渠道 |
| 拼多多渠道销售额 | 数字 | 分渠道 |
| 淘宝渠道销售额 | 数字 | 分渠道 |
| 广告投放费用 | 数字 | 当日广告费 |
| ROAS | 数字 | 广告投入产出比 |
| 备注 | 文本 | 特殊事项 |

### 第 2 步：在 n8n 创建工作流

1. 登录 n8n，创建新工作流「数据整理与报表生成」

### 第 3 步：配置定时触发器

1. 添加「Schedule Trigger」节点
2. 配置：
   - 触发规则：At 09:00 AM, every day
   - 时区：Asia/Shanghai

### 第 4 步：配置数据采集节点

**采集节点 1：淘宝订单数据**

```
节点名称：采集淘宝数据
节点类型：HTTP Request
请求方式：GET
URL：https://eco.taobao.com/router/rest
参数：
  method: taobao.trades.sold.get
  app_key: {{淘宝App Key}}
  sign: {{签名}}
  timestamp: {{当前时间戳}}
  format: json
  v: 2.0
  start_created: {{昨日00:00:00}}
  end_created: {{昨日23:59:59}}
  page_size: 100
```

> 如果淘宝 API 接入复杂，替代方案：用 RPA 工具（如影刀 RPA）每天自动导出淘宝后台数据到 Excel，再用 n8n 读取该 Excel 文件。

**采集节点 2：抖音电商数据**

```
节点名称：采集抖音数据
节点类型：HTTP Request
请求方式：POST
URL：https://openapi-fxg.jinritemai.com/data/shop/data
请求头：
  Content-Type: application/json
请求体：
{
  "start_time": "{{昨日时间戳}}",
  "end_time": "{{今日时间戳}}",
  "data_type": "shop"
}
```

**采集节点 3：广告投放数据（以巨量引擎为例）**

```
节点名称：采集广告数据
节点类型：HTTP Request
请求方式：POST
URL：https://ad.oceanengine.com/open_api/v2.0/report/ad/get
请求头：
  Access-Token: {{巨量引擎Token}}
请求体：
{
  "start_date": "{{昨日日期}}",
  "end_date": "{{昨日日期}}",
  "fields": ["stat_cost","show_cnt","click_cnt","convert_cnt"]
}
```

### 第 5 步：配置数据清洗节点

1. 添加「Code」节点，编写数据清洗逻辑：

```javascript
// 节点名称：数据清洗与整合
// 将各平台数据统一格式

const taobaoData = items[0].json;  // 淘宝数据
const douyinData = items[1].json;  // 抖音数据
const adData = items[2].json;      // 广告数据

// 计算汇总指标
const taobaoSales = taobaoData.trades ? 
  taobaoData.trades.reduce((sum, t) => sum + parseFloat(t.payment), 0) : 0;
const taobaoOrders = taobaoData.trades ? taobaoData.trades.length : 0;

const douyinSales = douyinData.data ? parseFloat(douyinData.data.order_amount) : 0;
const douyinOrders = douyinData.data ? douyinData.data.order_count : 0;

const totalSales = taobaoSales + douyinSales;
const totalOrders = taobaoOrders + douyinOrders;
const avgOrderValue = totalOrders > 0 ? totalSales / totalOrders : 0;

const adCost = adData.data && adData.data.list ? 
  adData.data.list.reduce((sum, a) => sum + parseFloat(a.stat_cost), 0) : 0;
const roas = adCost > 0 ? totalSales / adCost : 0;

// 返回清洗后的数据
return {
  json: {
    date: new Date(Date.now() - 86400000).toISOString().split('T')[0], // 昨天日期
    total_sales: totalSales.toFixed(2),
    total_orders: totalOrders,
    avg_order_value: avgOrderValue.toFixed(2),
    taobao_sales: taobaoSales.toFixed(2),
    douyin_sales: douyinSales.toFixed(2),
    ad_cost: adCost.toFixed(2),
    roas: roas.toFixed(2)
  }
};
```

### 第 6 步：配置数据写入飞书节点

```
节点名称：写入飞书表格
节点类型：HTTP Request
请求方式：POST
URL：https://open.feishu.cn/open-apis/bitable/v1/apps/{APP_TOKEN}/tables/{TABLE_ID}/records
请求头：
  Authorization: Bearer {{飞书Token}}
  Content-Type: application/json
请求体：
{
  "fields": {
    "日期": {{日期}},
    "销售额": {{total_sales}},
    "订单量": {{total_orders}},
    "客单价": {{avg_order_value}},
    "淘宝渠道销售额": {{taobao_sales}},
    "抖音渠道销售额": {{douyin_sales}},
    "广告投放费用": {{ad_cost}},
    "ROAS": {{roas}}
  }
}
```

### 第 7 步：配置 AI 数据分析节点

1. 添加「OpenAI」节点

```
节点名称：AI数据洞察
节点类型：OpenAI
Resource：Chat
Model：gpt-4o
System Message：

你是一个数据分析专家。请根据以下数据生成运营日报的数据洞察。

# 数据要求
1. 识别关键变化（同比、环比）
2. 找出异常数据并解释可能原因
3. 给出 3-5 条可执行的建议
4. 语气专业但通俗，让非数据人员也能理解

# 输出格式
一、核心指标速览
[用一句话总结当日表现]

二、数据洞察
1. [洞察1]
2. [洞察2]
3. [洞察3]

三、建议行动项
- [ ] [行动1]
- [ ] [行动2]
- [ ] [行动3]

User Message：
今日数据（{{日期}}）：
- 销售额：¥{{total_sales}}
- 订单量：{{total_orders}}单
- 客单价：¥{{avg_order_value}}
- 淘宝渠道：¥{{taobao_sales}}
- 抖音渠道：¥{{douyin_sales}}
- 广告费用：¥{{ad_cost}}
- ROAS：{{roas}}

请与昨日数据对比分析，并生成洞察报告。
```

2. 为了让 AI 能对比历史数据，需要先查询昨日数据。添加一个 HTTP Request 节点查询飞书表格中昨天的记录。

### 第 8 步：配置图表生成节点

使用 QuickChart API 生成图表图片：

```
节点名称：生成趋势图
节点类型：HTTP Request
请求方式：POST
URL：https://quickchart.io/chart
请求头：
  Content-Type: application/json
请求体：
{
  "chart": {
    "type": "line",
    "data": {
      "labels": ["8/9", "8/10", "8/11", "8/12", "8/13", "8/14", "8/15"],
      "datasets": [
        {
          "label": "销售额",
          "data": [38200, 40100, 39500, 42300, 41000, 40200, 45230],
          "borderColor": "#4A90D9",
          "backgroundColor": "rgba(74, 144, 217, 0.1)"
        }
      ]
    },
    "options": {
      "title": { "display": true, "text": "近7日销售额趋势" },
      "scales": {
        "y": { "ticks": { "callback": "function(value) { return '¥' + value; }" } }
      }
    }
  }
}
```

> QuickChart 返回的是 PNG 图片，可以直接嵌入飞书消息或保存到图床。

### 第 9 步：配置报表推送节点

```
节点名称：推送日报到飞书
节点类型：HTTP Request
请求方式：POST
URL：飞书群机器人 Webhook
请求体：
{
  "msgtype": "interactive",
  "card": {
    "header": {
      "title": { "tag": "plain_text", "content": "📊 运营日报 | {{日期}}" },
      "template": "blue"
    },
    "elements": [
      {
        "tag": "div",
        "text": { "tag": "lark_md", "content": "**销售额：¥{{total_sales}}** | **订单：{{total_orders}}单** | **客单价：¥{{avg_order_value}}**" }
      },
      {
        "tag": "img",
        "img_key": "{{趋势图图片key}}",
        "alt": { "tag": "plain_text", "content": "7日趋势图" }
      },
      {
        "tag": "div",
        "text": { "tag": "lark_md", "content": "{{AI洞察内容}}" }
      },
      {
        "tag": "action",
        "actions": [
          {
            "tag": "button",
            "text": { "tag": "plain_text", "content": "查看完整报表" },
            "url": "{{飞书表格链接}}",
            "type": "primary"
          }
        ]
      }
    ]
  }
}
```

### 第 10 步：测试工作流

1. 手动触发工作流
2. 检查各节点是否正常执行
3. 检查飞书表格是否写入了数据
4. 检查飞书群是否收到了日报消息
5. 检查图表是否正确生成
6. 检查 AI 洞察内容是否合理

---

## 七、预估效果与 ROI

### 效果预估

| 指标 | 手动操作 | 自动化后 | 改善 |
|------|---------|---------|------|
| 日报制作时间 | 60-90 分钟/天 | 0 分钟（自动） | 完全解放 |
| 数据准确率 | 90%（人为出错） | 99.9% | 提升 10% |
| 报表及时性 | 10:00 前难完成 | 09:00 自动推送 | 提前 1 小时+ |
| 数据分析深度 | 基础汇总 | AI 洞察 + 建议 | 质的飞跃 |

### 成本核算

| 项目 | 月成本 |
|------|--------|
| n8n 云端版 | 免费（1000 次执行内） |
| OpenAI API（约 30 次/月） | $2-5 |
| QuickChart | 免费 |
| 飞书 | 免费 |
| **合计** | **$2-5/月（¥15-35）** |

### ROI 计算

- 每天节省 1.5 小时 × 22 天 = 33 小时/月
- 按时薪 ¥50 计算，每月节省 ¥1650
- **投入 ¥15-35/月，节省 ¥1650/月，ROI ≈ 4700%+**

---

## 八、常见问题与解决方案

### Q1：数据源 API 需要复杂签名怎么办

**解决**：
1. 对于签名复杂的 API（如淘宝、抖音），在 n8n 的 Code 节点中编写签名算法
2. 或使用各平台官方 SDK，在 n8n 自定义节点中调用
3. 最简单的替代方案：用 RPA 工具（影刀/八爪鱼）定时导出数据到 Excel/CSV，n8n 读取文件

### Q2：AI 生成的数据分析不够深入

**解决**：
1. 在提示词中增加更多上下文，比如：
   - 最近 7 天的趋势数据
   - 去年同期对比数据
   - 业务背景信息（如最近做了什么活动）
2. 在提示词中要求 AI 使用具体的分析框架：
   ```
   请使用以下框架分析：
   1. 趋势分析：是上升还是下降？幅度如何？
   2. 结构分析：各渠道占比是否合理？
   3. 异常分析：哪些数据偏离正常范围？
   4. 归因分析：变化的原因可能是什么？
   5. 行动建议：基于以上分析，建议做什么？
   ```
3. 给 AI 提供历史优秀分析报告作为参考示例

### Q3：图表样式不好看

**解决**：
1. 使用 QuickChart 的自定义主题功能
2. 或改用 ECharts 生成图表，保存为图片
3. 或直接在飞书多维表格中创建仪表盘视图，自动更新图表
4. 推荐配色方案：
   - 主色：#4A90D9（蓝）
   - 辅色：#F5A623（橙）、#7ED321（绿）
   - 警示：#D0021B（红）

### Q4：数据量太大，n8n 执行超时

**解决**：
1. 在数据采集时做分页处理，每次只拉取一部分数据
2. 在 n8n 工作流设置中将超时时间调大（默认 60 秒，可调到 300 秒）
3. 将数据采集和处理拆分成两个工作流，通过飞书表格中转
4. 如果自部署 n8n，可调整服务器配置

### Q5：周末和节假日不想发日报

**解决**：
1. 在 n8n 的 Schedule Trigger 中配置只在工作日执行：
   - 触发规则：At 09:00 AM, Monday through Friday
2. 对于节假日，在 Code 节点中增加日期判断：
   ```javascript
   const today = new Date();
   const holidays = ['2026-01-01', '2026-02-10', '2026-05-01']; // 节假日列表
   const todayStr = today.toISOString().split('T')[0];
   if (holidays.includes(todayStr)) {
     // 跳过执行
     return null;
   }
   ```

### Q6：老板想随时看实时数据怎么办

**解决**：
1. 在飞书多维表格中创建仪表盘（Dashboard），实时展示最新数据
2. 设置飞书机器人关键词触发：在群里发「今日数据」自动触发工作流，返回最新数据
3. 用 Coze 创建一个数据查询 Bot，老板可以直接对话查询：
   ```
   用户：今天卖了多少？
   Bot：今日销售额 ¥45,230，比昨天增长 12.3%，抖音渠道贡献最大...
   ```

---

## 九、进阶优化方向

1. **预测分析**：接入时间序列预测模型，预测未来 7 天的销售趋势
2. **异常告警**：数据异常时（如销售额暴跌 50%）立即推送告警
3. **多维度下钻**：支持按商品、地区、时段等维度下钻分析
4. **自动归因**：结合广告、活动、天气等外部数据，自动归因销售变化
5. **报表模板库**：预设多种报表模板（日报/周报/月报/专题分析），一键切换

---

*本指南基于 n8n 云端版和飞书开放平台 2026年8月版本编写。*
