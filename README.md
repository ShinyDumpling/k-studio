# K-Studio

一套专注于从参考图反推中文 AI 写真提示词的 Agent 技能集，兼容所有支持 SKILL.md 格式的 Agent（Hermes、Codex、Claude Code 等）。每个模块均可独立使用，也可通过 `kiss` 主入口串联成完整流程。

## 快速开始

对任意 Agent 说：

> 帮我安装这套 skills：`https://github.com/ShinyDumpling/K-studio`

## 技能总览

### 主流程（kiss 调度链）

| 技能 | 职责 | 独立使用 |
|------|------|---------|
| `kiss` | 主入口，固定顺序调度 10 模块，支持预览/满血/精简三种模式 | — |
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
| `k-studio-compact` | 精简模式，10 模块输出压缩为一段约 800 字的精简提示词 | — |
| `k-studio-preview` | 预览模式，合并整组共性输出系列写真综合色提示词 | — |
| `k-studio-max` | 满血模式，按图片编号逐张输出保留所有差异 | — |
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
      └─ 输出 → 预览模式（默认，k-studio-preview） / 满血模式（k-studio-max） / 精简模式（k-studio-compact）
```

调用顺序固定：先整体后局部，先主体后细节。预览模式会先完成主体、服装、环境等全局特征分析，再将光影、构图、动作等差异维度整理为组内变化范围。只有用户明确要求满血模式时，才按图片编号拆分输出。

## 输出模式

| 模式 | 触发关键词 | 输出结构 | 适用场景 |
|------|-----------|---------|---------|
| **预览模式**（默认） | 不指定 | 系列写真综合色提示词，保留维度标题，合并共性，差异写成变化范围 | 同一组同主题照片，想生成同系列新图 |
| **满血模式** | "满血模式""逐图输出""每张图一份" | 按图片编号逐张输出，每张图完整结果或差异模块逐图编号 | 需要精确复刻每张图的所有差异 |
| **精简模式** | "精简模式""摘要模式""汇总输出" | 约 800 字两段式文本，去维度标题，多图用画面描述列表 | 一次性拿整组提示词，直接复制用 |

## 使用示例

**完整反推：**
> "使用kiss反推这组照片的提示词"
> "/kiss"

**满血输出：**
> "/kiss 满血模式，每张图一份完整提示词"

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
