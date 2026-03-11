---
name: short-drama-adapter
description: 动漫短剧分集改编工具。用于将翻译后的小说（英文）拆分为适合动漫短剧的分集内容。工作流程包括：(1) 分析原文结构和情节节奏，(2) 识别短剧节奏点（hook、冲突、反转、悬念），(3) 按1-3分钟时长规划分集，(4) 生成分集大纲和脚本。当用户需要将小说改编为动漫短剧、拆分为短剧分集、或按短剧节奏重新组织内容时使用此 skill。触发场景包括："拆分成短剧"、"动漫短剧分集"、"短剧改编"、"1-3分钟分集"、"短剧节奏"等。
---

# 动漫短剧分集改编工具

将翻译后的小说（英文）拆分为适合动漫短剧的分集内容，每集1-3分钟，按短剧节奏重新组织情节。

## 短剧特点

动漫短剧（1-3分钟/集）与传统小说的区别：

| 维度 | 传统小说 | 动漫短剧 |
|------|----------|----------|
| 节奏 | 缓慢铺垫 | 快节奏、强冲突 |
| 开头 | 渐进式 | 黄金3秒Hook |
| 结尾 | 章节完结 | Cliffhanger悬念 |
| 冲突 | 分散分布 | 每集必须有冲突 |
| 反转 | 长篇积累 | 每集1-2个反转 |
| 情感 | 细腻描写 | 外化、夸张表现 |

## 工作流程

### 第一步：原文结构分析

读取翻译后的小说全文，分析：

1. **章节结构** - 原文章节划分、每章字数
2. **情节脉络** - 主线剧情、 subplot支线
3. **冲突点** - 人物冲突、事件冲突、情感冲突
4. **转折点** - 重大反转、身份揭露、关系变化
5. **情感高潮** - 打脸、复仇、和解、表白

**输出格式**：
```
## 原文结构分析

### 章节统计
| 章节 | 字数 | 主要事件 | 冲突强度 |
|------|------|----------|----------|
| Ch1 | 2000 | 订婚晚宴冲突 | ★★★★★ |
| Ch2 | 1800 | 回忆过往 | ★★★ |

### 关键情节点
| 位置 | 事件 | 类型 | 改编优先级 |
|------|------|------|------------|
| Ch1 | 女主被羞辱 | 冲突爆发 | P0 - 必须保留 |
| Ch3 | 身份揭露 | 重大反转 | P0 - 必须保留 |
```

### 第二步：短剧节奏设计

根据短剧1-3分钟时长（约300-800字/集），设计节奏：

**短剧结构模板**（参考 [references/short-drama-structure.md](references/short-drama-structure.md)）：

```
【第X集】总时长：约2分钟

【Hook - 0-5秒】
- 最抓人眼球的画面/冲突
- 例如：女主被泼酒、男主跪地求饶

【铺垫 - 5-20秒】
- 快速交代背景
- 人物关系简要说明

【冲突升级 - 20-60秒】
- 主要冲突展开
- 对话交锋

【高潮/反转 - 60-90秒】
- 打脸时刻
- 身份揭露
- 重大转折

【Cliffhanger - 90-120秒】
- 悬念结尾
- 让观众想看下一集
```

**分集原则**：
- 每集必须有**至少一个冲突**
- 每3集必须有一个**重大反转**
- 每集结尾必须有**悬念**
- 打脸/爽点场景**单独成集**或作为集末高潮

### 第三步：分集规划

基于分析结果，规划分集：

**分集策略**：
1. **单章拆多集** - 情节丰富的章节拆成2-4集
2. **多章合并** - 过渡性章节合并为1集
3. **跨章重组** - 按冲突线重新组织（回忆、插叙调整）

**输出格式**：
```
## 分集规划总表

| 集数 | 标题 | 来源章节 | 主要事件 | 时长 | Hook | 高潮 | Cliffhanger |
|------|------|----------|----------|------|------|------|-------------|
| EP1 | 订婚宴羞辱 | Ch1前半 | 女主被泼酒 | 2min | 冲突开场 | 女主反击 | 哥哥出现 |
| EP2 | 神秘哥哥 | Ch1后半 | 哥哥身份暗示 | 1.5min | 哥哥霸气护妹 | 震慑全场 | 身份成谜 |

总集数：30集
预估总时长：60分钟
```

### 第四步：生成分集详情

为每集生成详细内容：

**输出格式**：
```
## Episode 1: The Engagement Party Humiliation
**Duration**: 2 minutes
**Source**: Chapter 1 (lines 1-50)

### Scene Breakdown

【SCENE 1 - 0:00-0:10】Hook
- **Visual**: Close-up of wine glass being knocked over
- **Action**: Chloe slaps the tea out of Stella's hand
- **Dialogue**: 
  CHLOE: "How dare you toast with tea?"
- **Impact**: Immediate conflict, establish antagonist

【SCENE 2 - 0:10-0:40】Background
- **Visual**: Wide shot of elegant engagement party
- **Action**: Stella explains alcohol allergy
- **Dialogue**:
  STELLA: "I explained my allergy..."
  ETHAN: "Drink up!"
- **Impact**: Show protagonist dilemma

【SCENE 3 - 0:40-1:20】Conflict Escalation
- **Visual**: Ethan's angry face, Chloe's fake tears
- **Action**: Ethan sides with Chloe, demands apology
- **Dialogue**:
  ETHAN: "Apologize to Chloe!"
  STELLA: "Mind your own business."
- **Impact**: Show protagonist strength

【SCENE 4 - 1:20-2:00】Climax + Cliffhanger
- **Visual**: Alexander stands up, protective stance
- **Action**: Alexander grabs Ethan's wrist
- **Dialogue**:
  ALEXANDER: "How dare you hit her?"
- **Impact**: Hook for next episode
```

### 第五步：输出分集文件

生成最终分集文档：

**文件命名**：`<小说名>_ShortDrama_Episodes.md`

**包含内容**：
1. 分集总表（Overview）
2. 每集详细脚本（Scene by scene）
3. 视觉/音效建议（Visual notes）
4. 时长统计

## 分集技巧

### 冲突密度控制
- **单集单冲突** - 适合过渡集
- **单集多冲突** - 适合高潮集
- **冲突升级** - 每集冲突比上集更激烈

### Cliffhanger类型
1. **悬念型** - "她竟然是他的..."
2. **危机型** - 主角面临危险
3. **反转型** - 刚要打脸，被打断
4. **情感型** - 告白被打断、误会加深

### 删减与合并原则
**删减**：
- 过于细腻的心理描写
- 重复的场景铺垫
- 与主线无关的支线

**保留**：
- 所有打脸场景
- 身份揭露时刻
- 感情转折点
- 复仇高潮

**合并**：
- 连续对话压缩
- 多个小冲突合并为大冲突
- 过渡场景一笔带过

## 参考资源

- **短剧结构指南**：[references/short-drama-structure.md](references/short-drama-structure.md)
- **分集示例**：[references/episode-examples.md](references/episode-examples.md)
- **节奏控制指南**：[references/pacing-guide.md](references/pacing-guide.md)

## 执行建议

- **先整体后局部**：先规划总集数，再细化每集
- **保持主线清晰**：删减支线，突出主线冲突
- **爽点密集**：打脸场景尽量单独成集或在集末
- **Hook密度**：每集开头必须吸引人
- **Cliffhanger必须**：每集结尾必须有悬念
