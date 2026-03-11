---
name: novel-translation-helper
description: 中文小说英文翻译工具。用于将中文小说翻译成适合海外读者阅读的英文版本。工作流程包括：(1) 提取术语表（人名、地名、专有名词），(2) 将术语翻译为完整英文姓名（名字+姓氏均为英文），(3) 选择地名本地化策略（可选择完全本地化为美国/英国地名，不保留任何中国痕迹），(4) 生成术语表文件供用户确认，(5) 全文翻译并应用术语表，确保家族/公司/地名命名一致性。当用户需要将中文小说翻译成英文、提取术语表、或进行小说本地化翻译时使用此 skill。触发场景包括："翻译小说"、"翻译成英文"、"提取术语表"、"小说翻译"、"英文翻译"、"地名本地化"、"美国地名"、"完全本地化"等。
---

# 中文小说英文翻译助手

将中文小说翻译成适合海外读者阅读的英文版本，确保人名、地名使用完整英文名称（名字+姓氏均为英文，非拼音），并支持**完全本地化**——将所有中国地名（包括真实城市）替换为目标国家（如美国）的地名，不留任何中国痕迹。

## 工作流程

### 第一步：地名本地化策略选择

在开始翻译前，询问用户地名本地化的偏好：

```
请选择地名本地化策略：

1. 【完全本地化 - 美国】将所有地名替换为美国地名（推荐）
   - 包括北京、上海等真实中国城市
   - 最终效果：读者完全看不出中国背景

2. 【完全本地化 - 英国】将所有地名替换为英国地名
   - 包括北京、上海等真实中国城市
   - 最终效果：读者完全看不出中国背景

3. 【保留真实城市】保留中国真实城市名（Beijing, Shanghai）
   - 仅替换虚构城市
   - 适合需要保留部分中国背景的作品

4. 【通用本地化】使用通用英文虚构地名
   - 不对应任何真实国家

请输入数字 1-4 选择策略：
```

**默认选择**：如果用户未指定，默认使用选项 1（完全本地化为美国地名）。

### 第二步：术语表提取

读取中文小说全文，提取以下类别术语：

1. **人物名称** - 所有角色姓名（主角、配角、提及人物）
2. **地点名称** - 城市、国家、公司、建筑、虚构地点
3. **专有名词** - 组织名、特殊物品、头衔称号

**地名分类**：
- **真实中国城市** - 北京、上海、广州、深圳等（根据策略决定是否替换）
- **虚构中国城市** - 京海、上京、云城等（必须替换）
- **通用地点** - 公司、住所、商场等

### 第三步：地名本地化处理

#### 策略 1：完全本地化 - 美国（推荐）

**将所有中国地名替换为美国地名，不留中国痕迹**：

| 中文地名 | 类型 | 建议美国地名 | 说明 |
|----------|------|--------------|------|
| 北京 | 首都 | **Washington D.C.** | 美国首都对应中国首都 |
| 上海 | 经济中心 | **New York City** | 经济中心对应 |
| 广州/深圳 | 南方经济 | **Los Angeles** / **Miami** | 南方城市对应 |
| 成都/重庆 | 西部中心 | **Chicago** / **Denver** | 中部/西部对应 |
| 京海/上京 | 虚构一线 | **Manhattan, NY** | 金融商业 |
| 南城 | 虚构南方 | **Los Angeles, CA** | 娱乐产业 |
| 云城 | 虚构 | **Seattle, WA** | 科技城市 |
| 江城 | 虚构 | **Chicago, IL** | 工业中心 |
| 港城 | 虚构港口 | **San Francisco, CA** | 港口科技 |
| 中国 | 国家 | **the United States** / **America** | 完全替换 |
| 国内/国外 | 相对概念 | **domestic** / **international** / **overseas** | 根据语境 |

**首都对应原则**：
- 北京（中国首都）→ Washington D.C.（美国首都）
- 上海（经济中心）→ New York City（美国经济中心）
- 香港（金融中心）→ Manhattan / Wall Street

#### 策略 2：完全本地化 - 英国

| 中文地名 | 建议英国地名 |
|----------|--------------|
| 北京 | **London** |
| 上海 | **London** / **Manchester** |
| 广州/深圳 | **Birmingham** / **Liverpool** |
| 京海/上京 | **London** |
| 中国 | **the United Kingdom** / **Britain** |

#### 策略 3：保留真实城市
- 北京 → Beijing
- 上海 → Shanghai
- 虚构城市 → 美国/英国/通用地名

#### 策略 4：通用本地化
使用虚构的英文地名，不对应任何真实国家。

### 第四步：姓氏选择与家族命名体系（仅人名去重）

**核心原则**：每个中文姓氏对应一个英文姓氏，家族成员共享同一姓氏。**仅对人名/姓氏进行去重检查，地名和专有名词可以重复使用**。

#### 4.1 读取历史术语表

**必须首先检查**：`references/used-names.md`

该文件记录了所有历史小说使用过的：
- 家族姓氏（Gray, Foster, Shaw等）- **必须去重**
- 人物全名（Stella Gray, Ethan Foster等）- **必须去重**

**注意**：地名（Washington D.C., Manhattan等）和专有名词不需要检查重复，不同小说可以重复使用相同地名。

#### 4.2 去重规则

**严禁重复使用（仅针对人名）**：
1. **家族姓氏** - 如果"Gray"已被使用，新小说不能再使用
2. **人物全名** - 如果"Stella Gray"已被使用，不能再使用

**可以自由使用（无需去重）**：
- **地名** - Washington D.C., New York, Manhattan 等可在不同小说重复使用
- **公司类型后缀** - Corporation, Group, Industries 等可以重复使用
- **通用名字** - Stella, Ethan 等名字 + 不同姓氏 = 不同人物

#### 4.3 冲突处理流程

**姓氏冲突处理**：
```
新小说姓氏：顾
历史记录：顾 → Gray（已被使用）
处理：选择替代姓氏
  - 首选：Grey（变体）
  - 次选：Grayson（扩展）
  - 备选：从可用姓氏列表选择（Shaw, Lewis等）
```

**地名无需冲突处理**：
```
新小说背景：北京→Washington D.C.
处理方式：直接使用，无需检查历史记录
说明：不同小说可以使用相同的地名设置
```

#### 4.4 姓氏选择流程

```
提取新小说姓氏列表（顾、傅、沈...）
    ↓
对每个姓氏：
  1. 检查 used-names.md
  2. 如果未使用 → 直接分配
  3. 如果已使用 → 选择替代姓氏
    ↓
生成本小说专属姓氏映射
```

**示例**：

小说1已使用：
- 顾 → Gray
- 傅 → Foster

小说2新提取：
- 顾 → 检测到Gray已使用 → 选择 **Grey**
- 沈 → 未使用 → 分配 **Shaw**
- 陆 → 未使用 → 分配 **Lewis**

#### 4.5 向用户报告去重情况

```
【历史术语表检查】

已检查历史记录：共[X]本小说

本次姓氏映射：
├─ 顾 → Gray ⚠️ 检测到历史使用
│   └─ 替代方案：Grey / Grayson / Graham
├─ 沈 → Shaw ✓ 未使用
└─ 陆 → Lewis ✓ 未使用

【建议映射】
- 顾 → Grey（推荐，与Gray区分但相似）
- 沈 → Shaw
- 陆 → Lewis

是否确认此映射？
1. "确认" - 使用推荐映射
2. "修改：顾 → [新姓氏]" - 手动指定
```

#### 4.6 家族命名示例

小说1（已归档）：
```
顾 → Gray → Stella Gray, Alexander Gray → Gray Corporation
傅 → Foster → Ethan Foster → Foster Corporation
```

小说2（新翻译）：
```
顾 → Grey → Sophia Grey, William Grey → Grey Corporation
沈 → Shaw → Victoria Shaw, Daniel Shaw → Shaw Enterprises
```

**注意**：Gray和Grey是两个不同的家族，读者不会混淆。

### 第五步：公司/组织命名（基于家族姓氏）

| 类型 | 命名格式 | 示例 |
|------|----------|------|
| 大型集团 | [姓氏] Corporation / Group | Foster Corporation, Gray Group |
| 中型公司 | [姓氏] Company / Industries | Shaw Company, Foster Industries |
| 家族住所 | [姓氏] Residence / Estate | Gray Residence, Foster Estate |

### 第六步：生成完整术语表

展示术语表给用户确认：

```
【地名本地化策略】：完全本地化 - 美国

【地名映射】（所有中国地名已替换）
- 北京 → Washington D.C.
- 上海 → New York City
- 京海 → Manhattan, New York
- 中国 → the United States
- 国内 → domestic

【家族姓氏体系】
- 顾 → Gray (家族成员：Stella, Alexander, Olivia)
- 傅 → Foster (家族成员：Ethan)

【人物名称】
- 顾晓念 → Stella Gray
- 傅景行 → Ethan Foster
- 顾裴琛 → Alexander Gray
...

【公司/组织】
- 傅氏集团 → Foster Corporation
- 顾氏 → Gray Corporation
- 顾家 → Gray Residence

是否有需要修改的地方？
1. "确认" - 使用当前术语表继续翻译
2. "修改：XX → YY" - 指定修改某一项
```

### 第七步：生成术语表文件

**文件命名**：`术语表_<小说名缩写>_<日期>.csv`

**格式**：
```csv
Category,Chinese,English,FamilySurname/LocationType,Notes
Character,顾晓念,Stella Gray,Gray,Protagonist
Location,北京,"Washington, D.C.",USA,Capital city mapping
Location,上海,"New York City, NY",USA,Economic center mapping
Location,京海,"Manhattan, NY",USA,Fictional city localized
Organization,傅氏集团,Foster Corporation,Foster,Company
```

**同时更新**：`references/used-names.md` - 仅记录本次使用的家族姓氏和人名（地名不记录，可重复使用）

### 第八步：全文翻译

基于确认的术语表翻译全文：

**翻译要求**：
1. 用流畅自然的英文表达
2. 所有术语表中的词汇必须使用对应的英文
3. **地名完全本地化**：所有中国地名替换为美国/英国地名
4. 公司/组织名基于家族姓氏
5. 对话符合人物性格和身份

**完全本地化翻译示例**（美国策略）：
```
原文：顾氏集团是北京的商业龙头
译文：Gray Group was the dominant force in Washington D.C.

原文：整个中国谁不知道
译文：Everyone in the United States knows

原文：傅氏在上海商圈站了起来
译文：Foster Corporation established itself in the New York business circle

原文：我在北京的海外公司工作
译文：I worked at the company's international branch in Washington D.C.
或：I worked at the headquarters in New York
```

**注意事项**：
- "国内" → domestic / within the country / stateside
- "国外/海外" → international / overseas / abroad
- "回国" → return to the States / come back home
- "出国" → go abroad / leave the country

### 第九步：保存与报告

保存翻译完成的文件：

**文件命名**：`<原文件名>_EN.txt`

**向用户报告**：
- 翻译完成字数统计
- 术语表使用数量
- 家族姓氏映射关系
- **地名本地化策略**（完全本地化 - 美国/英国）
- 主要地名映射列表
- 文件保存路径

## 命名资源

- **英文人名库**：[references/english-names.md](references/english-names.md) - 英文名字及姓氏选择
- **英文地名库**：[references/english-locations.md](references/english-locations.md) - 美国/英国/通用地名选项，包含真实城市替换指南
- **历史术语表**：[references/used-names.md](references/used-names.md) - 已使用的家族姓氏和人名记录（仅用于人名去重，地名无需记录）

## 完全本地化参考

### 中国城市 → 美国城市对应表

详见 [references/english-locations.md](references/english-locations.md) 的"完全本地化映射"部分

**主要对应关系**：
| 中国城市 | 美国对应城市 | 理由 |
|----------|--------------|------|
| 北京（首都） | Washington D.C. | 首都对应首都 |
| 上海（经济中心） | New York City | 经济中心对应 |
| 广州/深圳（南方经济） | Los Angeles / Miami | 南方港口经济 |
| 香港（金融） | Manhattan / Wall Street | 金融中心对应 |
| 杭州（科技） | Seattle / Silicon Valley | 科技产业对应 |
| 成都（休闲） | Denver / Austin | 宜居休闲城市 |

### 文化概念替换

| 中文概念 | 本地化替换 | 说明 |
|----------|------------|------|
| 中国 | the United States / America | 完全替换 |
| 国内 | domestic / stateside | 美国本土 |
| 国外 | international / overseas | 国际/海外 |
| 回国 | return to the States | 回美国 |
| 出国 | go abroad / leave the country | 出国 |
| 中式/中国 | American | 美式 |

## 执行建议

- **完全本地化优先**：默认使用完全本地化为美国地名，不留中国痕迹
- **地理一致性**：同一小说中的地点应在地理上合理（如同在东海岸）
- **家族姓氏一致性**：确保同一家族所有成员使用相同英文姓氏
- **公司命名**：公司名称必须与家族姓氏对应
- **用户确认**：术语表确认时特别展示地名映射，确保用户满意
