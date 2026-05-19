# K-Studio

一套专注于从参考图反推中文 AI 写真提示词的 Agent 技能集，兼容所有支持 SKILL.md 格式的 Agent（Hermes、Codex、Claude Code 等）。每个模块均可独立使用，也可通过 `kiss` 主入口串联成完整流程。

## 快速开始

对任意 Agent 说：

> 帮我安装这套 skills：`https://github.com/ShinyDumpling/K-studio`

## 技能总览

### 主流程（kiss 调度链）

| 技能 | 职责 | 独立使用 |
|------|------|---------|
| `kiss` | 主入口，固定顺序调度 9 模块，支持直出/精简两种模式 | — |
| `k-studio-style` | 整体风格、色调、视觉调性 | ✅ 使用kiss反推整体风格 |
| `k-studio-subject` | 主体特征（脸型、发型、肤色、妆容） | ✅ 使用kiss分析发型和妆容 |
| `k-studio-clothing` | 服装、穿搭、饰品 | ✅ 使用kiss反推穿搭风格 |
| `k-studio-environment` | 环境、场景、空间结构 | ✅ 使用kiss反推环境场景 |
| `k-studio-technical` | 设备、焦段、光圈 | ✅ 使用kiss分析设备参数 |
| `k-studio-texture` | 成像质感、颗粒、锐度 | ✅ 使用kiss反推成像质感 |
| `k-studio-lighting` | 光影、光源、光质 | ✅ 使用kiss分析光影 |
| `k-studio-color` | 色调、调色、滤镜、胶片风格 | ✅ 使用kiss分析色调 |
| `k-studio-framing` | 构图、景别、裁切 | ✅ 使用kiss分析构图 |
| `k-studio-action` | 动作姿态、表情神态 | ✅ 使用kiss反推动作 |

### 辅助工具

| 技能 | 职责 | 独立使用 |
|------|------|---------|
| `k-studio-compact` | 精简模式，9 模块输出压缩为一段约 800 字的精简提示词 | — |
| `k-studio-compliance` | 违规内容检查与替换建议 | ✅ 使用kiss检查提示词哪里会违规 |

## 工作流程

```
kiss（主入口，判断输出模式）
  │
  ├─ 1. k-studio-style（整体风格、色调、视觉调性）
  ├─ 2. k-studio-subject（主体脸型、发型、肤色、妆容）
  ├─ 3. k-studio-clothing（服装、穿搭、饰品）
  ├─ 4. k-studio-environment（环境、场景、空间结构）
  ├─ 5. k-studio-technical（设备、焦段、光圈）
  ├─ 6. k-studio-texture（成像质感、颗粒、锐度）
  ├─ 7. k-studio-lighting（光影、光源、光质）
  ├─ 8. k-studio-color（色调、调色、滤镜、胶片风格）
  ├─ 9. k-studio-framing（构图、景别、裁切）
  ├─ 10. k-studio-action（动作姿态、表情神态）
      │
      └─ 输出 → 直出模式（默认） / 精简模式（compact）
```

调用顺序固定：先整体后局部，先主体后细节。可逐图输出的模块（lighting / framing / action）固定放在后段，先完成主体与环境等全局特征分析。

## 输出模式

| 模式 | 触发关键词 | 输出结构 | 适用场景 |
|------|-----------|---------|---------|
| **直出模式**（默认） | 不指定，或说"直出" | 9 个模块独立输出，带维度标题，模块间空行分隔 | 需要逐项检查/修改某个维度、做二次编辑 |
| **精简模式** | "精简模式""摘要模式""汇总输出" | 两段式连续文本，约 800 字，去掉维度标题，差异化维度用补充句说明 | 一次性拿整组照片的提示词，直接复制用 |

## 使用示例

**完整反推：**
> "使用kiss反推这组照片的提示词"
> "/kiss"

**精简输出：**
> "/kiss 精简模式，反推这组毕业照"

**单独指定维度：**
> "/kiss 反推动作"
> "/kiss 只看构图和光影"

**单个模块直接使用：**
> "使用kiss分析构图" → 调用 k-studio-framing
> "使用kiss分析光影" → 调用 k-studio-lighting
> "使用kiss反推穿搭风格" → 调用 k-studio-clothing
> "使用kiss分析设备参数" → 调用 k-studio-technical
> "使用kiss检查违规" → 调用 k-studio-compliance
