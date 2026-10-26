# Sprites 资源库

供 2D 游戏与动画展示使用的角色 Sprite 资源。目前收录 AI 辅助制作的「大肥鱼（dafeiyu）」：蓝发、女仆装、鲸鱼尾巴的 Q 版角色，提供多个动作、透明 PNG 单帧、配套图集、动画元数据和 WebP 预览。

所有可用资源均位于 [resources/](resources/)，导入资源本身无需安装依赖。

| 待机 | 奔跑 | 特殊攻击 |
| --- | --- | --- |
| ![大肥鱼待机动画](resources/dafeiyu/webp/dafeiyu_idle.webp) | ![大肥鱼奔跑动画](resources/dafeiyu/webp/dafeiyu_run.webp) | ![大肥鱼特殊攻击动画](resources/dafeiyu/webp/dafeiyu_attack_special.webp) |

| 唱歌 | 跳舞 |
| --- | --- |
| ![大肥鱼唱歌动画](resources/dafeiyu/webp/dafeiyu_sing.webp) | ![大肥鱼跳舞动画](resources/dafeiyu/webp/dafeiyu_dance_thunder.webp) |

## 快速使用

1. 将 `resources/dafeiyu/` 复制到项目的资源目录；保留内部目录结构即可继续使用元数据中的相对路径。
2. 根据项目的加载方式选择单帧 PNG 或图集。只做网页展示时，可以直接引用 WebP。
3. 按动作元数据中的 `frames` 顺序和 `duration_ms` 播放，并使用统一的画布、缩放和锚点。

| 使用方式 | 需要的文件 | 读取方式 |
| --- | --- | --- |
| 单帧动画 | `sprites/<action>/*.png`、`metadata/dafeiyu_<action>.json` | 按 `frames[].filename` 加载，按 `frames[].duration_ms` 切帧 |
| 图集动画 | `spritesheets/dafeiyu_<action>.png`、同名 `.json`、对应 metadata | 用 TexturePacker JSON Hash 读取帧矩形，用 metadata 读取播放顺序、时长与锚点 |
| 动画展示 | `webp/dafeiyu_<action>.webp` | 在浏览器、文档或支持动画 WebP 的查看器中打开 |
| 自动枚举动作 | `dafeiyu_manifest.json` | 读取 `actions`，获取动作名、帧数、FPS、循环标记和资源路径 |

例如，在网页项目中将整个角色目录复制到 `public/assets/dafeiyu/` 后，单帧路径为 `/assets/dafeiyu/sprites/idle/idle_00.png`，图集路径为 `/assets/dafeiyu/spritesheets/dafeiyu_idle.png`。

## 目录结构

```text
resources/
└── dafeiyu/
    ├── dafeiyu_manifest.json             # 角色与全部动作索引
    ├── sprites/
    │   ├── idle/idle_00.png ...          # 每个动作独立编号，从 00 开始
    │   ├── run/run_00.png ...
    │   └── <action>/<action>_00.png ...
    ├── spritesheets/
    │   ├── dafeiyu_<action>.png          # 整张图集
    │   └── dafeiyu_<action>.json         # TexturePacker JSON Hash
    ├── metadata/
    │   └── dafeiyu_<action>.json         # 帧顺序、时长、锚点、图集矩形
    └── webp/
        ├── dafeiyu_<action>.webp        # 动画预览
        └── dafeiyu_<action>.animation.json  # 预览播放步骤与循环次数
```

`<action>` 替换为下表中的动作 ID。单帧文件名使用两位补零编号，例如 `eat_rice_00.png` 至 `eat_rice_45.png`。

## 动作清单

以下参数来自当前 manifest。只有 `idle` 和 `run` 默认循环，其余动作播放一次。

| 动作 ID          | 内容 | 预览 |
| ---              | ---      | ---                                                      |
| `idle`           | 站立     | [查看](resources/dafeiyu/webp/dafeiyu_idle.webp)          |
| `run`            | 奔跑     | [查看](resources/dafeiyu/webp/dafeiyu_run.webp)           |
| `jump`           | 跳跃     | [查看](resources/dafeiyu/webp/dafeiyu_jump.webp)          |
| `land`           | 落地     | [查看](resources/dafeiyu/webp/dafeiyu_land.webp)          |
| `dash`           | 冲刺     | [查看](resources/dafeiyu/webp/dafeiyu_dash.webp)          |
| `attack`         | 普通攻击 | [查看](resources/dafeiyu/webp/dafeiyu_attack.webp)        |
| `hurt`           | 受伤     | [查看](resources/dafeiyu/webp/dafeiyu_hurt.webp)          |
| `die`            | 死亡     | [查看](resources/dafeiyu/webp/dafeiyu_die.webp)           |
| `die_reveal`     | 现形     | [查看](resources/dafeiyu/webp/dafeiyu_die_reveal.webp)    |
| `attack_special` | 特殊攻击 | [查看](resources/dafeiyu/webp/dafeiyu_attack_special.webp) |
| `sing`           | 唱歌     | [查看](resources/dafeiyu/webp/dafeiyu_sing.webp)          |
| `dance_thunder`  | 跳舞     | [查看](resources/dafeiyu/webp/dafeiyu_dance_thunder.webp) |

可按游戏状态衔接 `run_start → run（循环）→ run_stop → idle`、`jump → land → idle` 或 `die → die_reveal`。这些是独立动作，衔接时机、位移、碰撞与技能效果由使用方控制。

## 资源规格与加载约定

### 画布、方向和锚点

| 项目 | 当前值 |
| --- | --- |
| 单帧画布 | `640 × 640` 像素，所有动作一致 |
| 图片格式 | 透明 RGBA PNG |
| 默认朝向 | 向右；向左时可水平翻转 |
| 归一化锚点 | `x = 0.5`，`y = 0.89`，从画布左上角计算 |
| 像素锚点 | `[320, 570]` |
| 图集帧 | 不旋转、不裁边，保留完整单帧画布 |

将角色世界坐标对齐到上述锚点，并在各动作间保持相同缩放，便于稳定定位。使用像素坐标时采用 `[320, 570]`；使用归一化坐标时采用 `(0.5, 0.89)`，两者有小数舍入差异。若目标工具以左下角为原点，需要相应转换纵坐标。

不要逐帧按可见轮廓重新居中或独立裁去透明边缘，否则会改变动作的定位关系。攻击图中的水流等特效已经包含在图片内；角色碰撞框、伤害判定与音效需由项目另行设置。

## 资源来源与许可

资源元数据将制作来源标注为 AI 生成，并记录了透明轮廓提取等处理信息。当前仓库未附带 `LICENSE` 或明确的资源授权说明，使用与分发的许可范围需由维护者补充确认。
