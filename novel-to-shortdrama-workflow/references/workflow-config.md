# 工作流配置指南

novel-to-shortdrama-workflow 的配置选项和参数说明。

## 默认配置

```yaml
workflow:
  name: "小说到短剧工作流"
  version: "1.0"
  
# 步骤控制
steps:
  step1_localization:
    enabled: true
    sub_steps:
      redline_check: true          # 价值观红线检查
      cultural_symbols: true       # 文化符号本地化
      plot_adaptation: true        # 剧情逻辑适配
      expression_cleanup: true     # 表达清洗
    
  step2_translation:
    enabled: true
    location_strategy: "usa_full"  # 地名策略
    # 可选: usa_full, uk_full, generic, keep_real
    name_translation: "full_english"  # 人名翻译方式
    # 可选: full_english, keep_pinyin_surname, full_pinyin
    
  step3_drama_adaptation:
    enabled: true
    episode_duration: "mixed"      # 分集时长
    # 可选: 1min, 1_5min, 2min, 2_5min, 3min, mixed
    pacing: "high_conflict"        # 节奏密度
    # 可选: high_conflict, medium, slow_burn
    episodes_target: "auto"        # 目标集数
    # 可选: auto, 20, 30, 40, 50

# 输出控制
output:
  generate_report: true            # 生成工作报告
  save_intermediate: true          # 保存中间文件
  compress_output: false           # 压缩输出
  
# 用户交互
interaction:
  confirm_each_step: true          # 每步后确认
  allow_modifications: true        # 允许用户修改
  timeout_seconds: 300             # 等待超时时间
```

## 地名策略详解

### usa_full - 完全本地化为美国
```yaml
location_strategy: "usa_full"
```

**映射规则**：
- 北京 → Washington, D.C.
- 上海 → New York City
- 广州/深圳 → Los Angeles / San Francisco
- 中国 → the United States
- 国内 → domestic / stateside
- 国外 → international / overseas

**适用场景**：目标市场为美国，希望完全消除中国背景

### uk_full - 完全本地化为英国
```yaml
location_strategy: "uk_full"
```

**映射规则**：
- 北京 → London
- 上海 → London / Manchester
- 中国 → the United Kingdom

**适用场景**：目标市场为英国/欧洲

### generic - 通用虚构地名
```yaml
location_strategy: "generic"
```

**映射规则**：
- 北京 → Riverton / Sterling
- 上海 → Metro City
- 使用虚构城市名，不对应真实国家

**适用场景**：架空背景，不特定于某个国家

### keep_real - 保留真实城市名
```yaml
location_strategy: "keep_real"
```

**映射规则**：
- 北京 → Beijing
- 上海 → Shanghai
- 虚构城市 → 使用generic策略

**适用场景**：需要保留部分中国背景

## 分集时长策略

### 1min - 快节奏短剧
```yaml
episode_duration: "1min"
```

- 每集约1分钟（300-400词）
- 适合：抖音/快手等超短视频平台
- 特点：节奏极快，冲突密集
- 原10章小说 → 约50-60集

### 1_5min - 标准短剧
```yaml
episode_duration: "1_5min"
```

- 每集约1.5分钟（450-600词）
- 适合：标准短剧平台
- 特点：平衡节奏和深度
- 原10章小说 → 约30-40集

### 2min - 完整短剧
```yaml
episode_duration: "2min"
```

- 每集约2分钟（600-800词）
- 适合：精品短剧
- 特点：情节完整，可展开细节
- 原10章小说 → 约20-30集

### mixed - 混合时长（推荐）
```yaml
episode_duration: "mixed"
```

- 根据情节复杂度自动调整
- 过渡集：1-1.5分钟
- 标准集：2分钟
- 特别篇：2.5-3分钟
- 原10章小说 → 约25-35集

## 节奏密度设置

### high_conflict - 高冲突密度
```yaml
pacing: "high_conflict"
```

- 每集至少2个冲突点
- 每3集一个重大反转
- 适合：爽文、复仇文、打脸文
- 观众留存率高，但需要强情节支撑

### medium - 中等密度
```yaml
pacing: "medium"
```

- 每集1-2个冲突点
- 每5集一个重大反转
- 适合：甜宠文、虐恋文
- 平衡情节推进和情感铺垫

### slow_burn - 慢热型
```yaml
pacing: "slow_burn"
```

- 每2集1个冲突点
- 每10集一个重大反转
- 适合：正剧、历史文、权谋文
- 注重人物塑造和世界观构建

## 预设配置模板

### 模板1：美式爽剧（推荐默认）
```yaml
preset: "american_drama"
location_strategy: "usa_full"
episode_duration: "mixed"
pacing: "high_conflict"
```

**特点**：
- 完全美国本地化
- 快节奏、高冲突
- 适合TikTok/Reels/Shorts

### 模板2：英式精品剧
```yaml
preset: "british_drama"
location_strategy: "uk_full"
episode_duration: "2min"
pacing: "medium"
```

**特点**：
- 英国背景
- 较慢节奏，注重对白
- 适合精品短剧平台

### 模板3：架空奇幻剧
```yaml
preset: "fantasy_drama"
location_strategy: "generic"
episode_duration: "2min"
pacing: "medium"
```

**特点**：
- 虚构世界
- 平衡节奏
- 适合奇幻/玄幻题材

### 模板4：保留文化特色
```yaml
preset: "cultural_keep"
location_strategy: "keep_real"
episode_duration: "1_5min"
pacing: "medium"
```

**特点**：
- 保留中国地名
- 适合需要文化背景的作品
- 目标受众为对中国文化感兴趣的观众

## 自定义配置示例

### 示例1：霸道总裁文配置
```yaml
workflow:
  steps:
    step1_localization:
      enabled: true
      # 保持默认
    
    step2_translation:
      enabled: true
      location_strategy: "usa_full"
      # 总裁文适合美国金融背景
    
    step3_drama_adaptation:
      enabled: true
      episode_duration: "mixed"
      pacing: "high_conflict"
      # 高冲突适合打脸剧情
```

### 示例2：甜宠文配置
```yaml
workflow:
  steps:
    step1_localization:
      enabled: true
      sub_steps:
        expression_cleanup: true
        # 特别注意表达清洗，使对话更自然
    
    step2_translation:
      enabled: true
      location_strategy: "usa_full"
      name_translation: "full_english"
    
    step3_drama_adaptation:
      enabled: true
      episode_duration: "1_5min"
      pacing: "medium"
      # 中等节奏，注重情感
```

### 示例3：历史/权谋文配置
```yaml
workflow:
  steps:
    step1_localization:
      enabled: true
      # 可能需要大量剧情调整
    
    step2_translation:
      enabled: true
      location_strategy: "generic"
      # 使用虚构地名，避免历史对应问题
    
    step3_drama_adaptation:
      enabled: true
      episode_duration: "2min"
      pacing: "slow_burn"
      # 慢节奏，注重权谋布局
```

## 配置文件使用

### 方式1：命令行参数
```bash
开始工作流，文件：小说.txt，配置：{location_strategy: usa_full, pacing: high_conflict}
```

### 方式2：配置文件
```bash
开始工作流，文件：小说.txt，配置文件：config.yaml
```

### 方式3：交互式选择
```bash
开始工作流，文件：小说.txt
[系统提示选择配置]
请选择预设配置：
1. 美式爽剧（推荐）
2. 英式精品剧
3. 架空奇幻剧
4. 保留文化特色
5. 自定义配置
```

## 配置优化建议

### 根据小说类型选择

| 小说类型 | 推荐配置 | 理由 |
|----------|----------|------|
| 霸道总裁 | usa_full + high_conflict | 美国商业背景，快节奏打脸 |
| 甜宠恋爱 | usa_full + medium | 美国都市背景，注重情感 |
| 复仇爽文 | usa_full + high_conflict | 快节奏，密集冲突 |
| 历史权谋 | generic + slow_burn | 虚构背景，慢节奏布局 |
| 玄幻修仙 | generic + medium | 架空世界，平衡节奏 |
| 现实主义 | keep_real + medium | 保留中国背景 |

### 根据目标平台选择

| 平台 | 推荐时长 | 推荐节奏 |
|------|----------|----------|
| TikTok/抖音 | 1min | high_conflict |
| YouTube Shorts | 1-1.5min | high_conflict |
| 爱奇艺/腾讯视频 | 2min | medium |
| Netflix/平台剧 | 2-3min | slow_burn |

## 配置调试

### 测试配置
```bash
测试配置，文件：小说.txt，配置：{preset: american_drama}
预览输出：
- 预估集数：30集
- 预估时长：60分钟
- 主要修改：价值观[X]处，文化[X]处
- 术语映射预览：[...]
确认执行？(是/否)
```

### 保存常用配置
```bash
保存当前配置为预设：我的默认配置
```

### 加载预设
```bash
开始工作流，文件：小说.txt，使用预设：我的默认配置
```
