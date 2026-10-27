# Anatomy — 使用指南

> 一个把抽象技术概念**画成真实机器**的开源项目。
> 21k+ stars · MIT License · 零依赖 · Node ≥ 20.10

---

## 一句话理解

普通示意图是「方框 + 箭头 + 标签」。Anatomy 反其道而行：

| 概念 | Anatomy 会画成 |
|---|---|
| 缓存键命中就跳过计算 | **弹子锁** —— 每个弹子都对上，锁芯才转，才重新渲染 |
| 一个 GPU context 服务多个 canvas | **龙门绘图机** —— 一个笔头依次造访 16 个井 |
| 7 个 render target 每 texel 字节数不同 | **七层抽屉柜** —— 每层抽屉深度就是它的字节数 |
| 可分离模糊 49×49 vs 49+49 | **钉床印章** + 导轨上的两把梳子 |
| 令牌桶限流器 | **会漏沙的沙漏漏斗**，配额以恒定速率补充 |

**物体本身就是解释。** 读者因为认得这台机器，所以理解了原理。

---

## 三种产物形态

| 类型 | 说明 | 例子 |
|---|---|---|
| **SVG**（核心） | 实体等距零件 + 四阶明暗着色，前后景深排序，装进带标题/提示/实时读数的卡片 | 百分表、台式电脑、测试台 |
| **SVG + WebGL** | 画不出来的效果：火焰、尾焰、水波、焦散、灯光。着色器跑在 SVG 前后两层之间，**和图形同一个等距相机**，所以它真的在画面里 | 猛禽发动机、波动槽 |
| **3D**（beta） | 真正转起来：旋钮、手指、断裂脱落 | turning-dial |

每张图支持明/暗两套主题。**亮色是设计时的主主题**。

---

## 在 OpenClaw 里怎么用

✅ **已安装到你的 OpenClaw**（`~/.openclaw/workspace/skills/anatomy`），状态 `ready`，随时可用。

### 三个好习惯

1. **说想法，别说图画** —— 告诉它要解释什么原理，并给出真实数字（尺寸、数量、速率、阈值）。Anatomy 负责发明那个物体，这正是最难的部分。
2. **或者直接说物体** —— 「一台意式咖啡机，侧板打开」，再说清楚什么该动。
3. **说清楚最终落在哪** —— 独立 HTML 页 / 单独的 SVG 文件 / React 组件；亮色还是暗色。

### 提问模板

```
/anatomy 解释一下令牌桶限流器怎么工作，做成可交互的
```

```
/anatomy 给我们的 Next.js 文档做一张图：数据库连接池，10 个连接，
请求借一个还一个，10 个都忙时新请求进队列排队。
React 组件 + 静态 HTML 预览，暗色主题
```

```
/anatomy 用等距 SVG 解释 CDN 边缘缓存怎么工作，命中 vs 未命中，
TTL 60 秒，给暗色落地页用。只要 SVG 文件和 PNG 预览
```

```
/anatomy 一个实验台上的本生灯：打开空气旋钮，惰性黄焰变成呼呼的蓝色锥焰。
火焰用 WebGL，一个 HTML 文件，暗色主题
```

```
/anatomy 拆掉侧板的意式咖啡机：锅炉、换热管、冲煮头、水泵、蒸汽棒和压力表。
拉咖啡时显示水流。独立 HTML，亮色主题
```

### 触发词

不写 `/anatomy` 也会触发：只要提到「等距插画」「解释性图表」「文档配图」「博客配图」「落地页插画」「交互式讲解」「Linear 风格技术图」等。

---

## 本地自行构建

```bash
cd ~/.openclaw/workspace/skills/anatomy

# 构建示例（会写出独立 HTML）
node examples/dial-indicator/build.mjs
node examples/desk-computer/build.mjs
node examples/ripple-tank/build.mjs

# 亮色主题
node examples/dial-indicator/build.mjs --light

# 几何审计：证明没有零件互相穿模、没有悬空端点、深度顺序正确
node examples/raptor-engine/build.mjs --audit

# 截图（需要 Chrome/Chromium）
node scripts/capture.mjs examples/dial-indicator/dial-indicator.html --out shot.png
```

### 项目结构

```
anatomy/
├── SKILL.md          # skill 本体：工作流 + 工艺规则
├── kit/              # 绘图库
│   ├── iso-kit.mjs   #   核心：相机、实体、零件、SVG 渲染器、卡片与页面
│   ├── lathe.mjs     #   任意轴向的回转体
│   ├── tube.mjs      #   管道与线缆
│   ├── gl.mjs        #   WebGL 层（着色器嵌在图里）
│   ├── turn.mjs      #   3D 运行时
│   └── audit.mjs     #   几何审计
├── references/       # 长文指南：craft / kit / motion / react / verify / webgl
├── scripts/          # headless Chrome 检查与构建辅助
└── examples/         # 9 个完整示例
```

**零依赖**：只用 Node 内置模块，不需要 `npm install`。

---

## 已部署的演示

**线上**：https://griz112358-blip.github.io/anatomy-figures/

| 示例 | 展示了什么 |
|---|---|
| [dial-indicator](https://griz112358-blip.github.io/anatomy-figures/dial-indicator.html) | 百分表 + 可滑动滑座 + 实时读数。**最小的示例，约 300 行，建议从这个入门** |
| [desk-computer](https://griz112358-blip.github.io/anatomy-figures/desk-computer.html) | 1984 风格台式电脑，键盘可输入，字符出现在 CRT 屏幕上，关机画面塌缩成一条线。约 85 个实体 |
| [ripple-tank](https://griz112358-blip.github.io/anatomy-figures/ripple-tank.html) | 教学波动槽，44° 俯视。两层 WebGL（灯光焦散 + 水面反射）由色散与光学模型驱动 |

**源码**：https://github.com/griz112358-blip/anatomy-figures

---

## 尺寸与耗时

| 尺寸 | 规模 | 耗时 |
|---|---|---|
| **Figure** | 单个机构，40–80 个零件，最多一层 WebGL | 通常 1 小时内 |
| **Hero** | 密集物体，多层 WebGL 或一个 3D 主体，80–150 个零件 | 约 2–4 小时 |
| **Epic** | 整幅画面级 | 更久 |

> 它**故意很慢** —— 零件清单、几何构建、审计、每个接缝在两种主题下的特写检查，这些都在质量里。

---

## 限制

- **不是图表库** —— 没有坐标轴、没有数据系列。数字以实时读数形式出现，由同一个驱动零件的模型算出。
- **画机器，不画人** —— 仪器、工具、家具可以；人脸、精确地图不行。没有机械原理的概念会做得很弱。
- **需要 WebGL** —— 没有 WebGL 时 SVG 依然可读；用户开启「减弱动态效果」时所有图静止。
- **3D 模式还是 beta** —— 只在示例和一个测试图上验证过。

---

## 更多

- 官网：https://skills.wheresryan.sh/anatomy
- 上游仓库：https://github.com/wheresryan22/anatomy
- 支持打印：`node scripts/print.mjs examples/raptor-engine/sheet.mjs --format all` 输出 A4 PDF（海报 / 技术图纸 / 讲解手册）
- 分享到社区画廊：跟 agent 说「把这张图发布到 Anatomy 社区画廊」，会开一个 PR 到 `anatomy-community`
