# Sprites 资源库

供 2D 游戏与动画展示使用的角色 Sprite 资源。目前收录 AI 辅助制作的「大肥鱼（dafeiyu）」：蓝发、女仆装、鲸鱼尾巴的 Q 版角色，提供 **13 个动作、113 张透明 PNG 单帧**，以及配套图集、动画元数据和 WebP 预览。

所有可用资源均位于 [resources/](resources/)，导入资源本身无需安装依赖。完整动作与播放参数见 [dafeiyu_manifest.json](resources/dafeiyu/dafeiyu_manifest.json)。

| 待机 | 奔跑 | 特殊攻击 |
| --- | --- | --- |
| ![大肥鱼待机动画](resources/dafeiyu/webp/dafeiyu_idle.webp) | ![大肥鱼奔跑动画](resources/dafeiyu/webp/dafeiyu_run.webp) | ![大肥鱼特殊攻击动画](resources/dafeiyu/webp/dafeiyu_attack_special.webp) |

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

| 动作 ID | 内容 | 帧数 | FPS | 循环 | 预览 |
| --- | --- | ---: | ---: | :---: | --- |
| `idle` | 待机 / 站立 | 8 | 8 | 是 | [查看](resources/dafeiyu/webp/dafeiyu_idle.webp) |
| `run` | 奔跑 | 6 | 12 | 是 | [查看](resources/dafeiyu/webp/dafeiyu_run.webp) |
| `jump` | 跳跃 | 6 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_jump.webp) |
| `land` | 落地 | 6 | 14 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_land.webp) |
| `dash` | 冲刺 | 6 | 16 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_dash.webp) |
| `attack` | 普通攻击 | 6 | 14 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_attack.webp) |
| `hurt` | 受伤 | 5 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_hurt.webp) |
| `die` | 死亡 | 6 | 10 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_die.webp) |
| `die_reveal` | 死亡后现原形 | 6 | 10 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_die_reveal.webp) |
| `eat_rice` | 吃大白饭 | 46 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_eat_rice.webp) |
| `attack_special` | 特殊攻击 / 水流特效 | 8 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_attack_special.webp) |
| `run_start` | 起跑过渡 | 2 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_run_start.webp) |
| `run_stop` | 停跑过渡 | 2 | 12 | 否 | [查看](resources/dafeiyu/webp/dafeiyu_run_stop.webp) |

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

### 图集布局

图集按从左到右、从上到下排列，每格 `640 × 640`，最多 4 列：

| 动作 | 图集尺寸 | 网格 |
| --- | --- | --- |
| `run_start`、`run_stop` | `1280 × 640` | 2 列 × 1 行 |
| `eat_rice` | `2560 × 7680` | 4 列 × 12 行 |
| 其余动作 | `2560 × 1280` | 4 列 × 2 行 |

部分图集末尾有空格，例如 `hurt` 只有 5 帧，`eat_rice` 只有 46 帧。**请按 manifest 的 `count` 或 JSON 中实际存在的帧播放，不要把整张网格的所有格子都加入动画。**

`eat_rice` 图集高度为 7680 像素。若目标设备或引擎的纹理尺寸上限不足，可使用单帧资源，或从单帧重新打包为多张图集。

### JSON 字段与路径

以下以角色根目录 `resources/dafeiyu/` 为基准；复制资源后，将其替换为项目中的实际位置。

| 文件 / 字段 | 含义与路径基准 |
| --- | --- |
| manifest 的 `frame_size`、`anchor`、`facing` | 全角色共享的画布、锚点和默认朝向 |
| manifest 的 `actions[].sheet`、`actions[].preview` | 相对角色根目录，例如 `spritesheets/dafeiyu_idle.png` |
| metadata 的 `frames[].filename` | 相对角色根目录，例如 `sprites/idle/idle_00.png`；按数组顺序播放 |
| metadata 的 `frames[].duration_ms` | 该帧持续时间，单位毫秒 |
| metadata 的 `sheet` | 仅为图集文件名；实际文件位于角色根目录下的 `spritesheets/` |
| 图集 JSON 的 `meta.image` | 相对该图集 JSON 所在目录的 PNG 文件名 |
| 图集 JSON 的 `frames` | 以 `idle_00.png` 这样的文件名为键；`frame` 为图集中的矩形，`duration` 为毫秒时长 |
| metadata 的 `atlas_frames` | 同样以单帧文件名为键，提供图集矩形与锚点 |
| WebP 配套 JSON 的 `steps` | 预览的帧名与 `duration_ms`；`loop = 0` 表示无限循环，`1` 表示播放一次 |

读取图集时，可取 `frames[].filename` 的最后一段作为图集帧名，例如将 `sprites/idle/idle_00.png` 映射到 `idle_00.png`。metadata 的 `source` 是制作来源描述，不作为运行时加载路径。

精确还原导出节奏时，优先使用 metadata 的逐帧 `duration_ms`。`fps` 是动作的标称帧率，例如 12 FPS 对应的导出帧时长为 83 ms，因此累计时长与直接按 `1000 / fps` 计算会略有差异。运行时循环开关使用 manifest / metadata 的布尔值 `loop`；WebP 配套文件使用的是整数循环次数。

## 浏览器播放示例

下面的示例使用 Canvas 加载单帧，按 metadata 的时长播放；循环动作持续播放，非循环动作结束后停留在最后一帧。

将以下内容保存为仓库根目录的 `preview.html`，把 `action` 改成动作清单中的任一 ID 即可切换动画。

```html
<!doctype html>
<html lang="zh-CN">
<meta charset="utf-8">
<title>大肥鱼 Sprite 预览</title>
<canvas id="sprite" width="640" height="640"
        style="width:320px;max-width:100%;background:#203044"></canvas>
<script type="module">
const base = new URL('./resources/dafeiyu/', document.baseURI);
const action = 'idle';
const response = await fetch(new URL('metadata/dafeiyu_' + action + '.json', base));
if (!response.ok) throw new Error('元数据读取失败：' + response.status);
const meta = await response.json();

const images = await Promise.all(meta.frames.map(({ filename }) =>
  new Promise((resolve, reject) => {
    const image = new Image();
    image.onload = () => resolve(image);
    image.onerror = reject;
    image.src = new URL(filename, base).href;
  })
));

const canvas = document.querySelector('#sprite');
const ctx = canvas.getContext('2d');
const total = meta.frames.reduce((sum, frame) => sum + frame.duration_ms, 0);
let start;

function render(now) {
  start ??= now;
  const elapsed = now - start;
  let remaining = meta.loop ? elapsed % total : Math.min(elapsed, total - 1);
  let index = 0;
  while (index < meta.frames.length - 1 && remaining >= meta.frames[index].duration_ms) {
    remaining -= meta.frames[index].duration_ms;
    index += 1;
  }
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.drawImage(images[index], 0, 0);
  if (meta.loop || elapsed < total) requestAnimationFrame(render);
}

requestAnimationFrame(render);
</script>
</html>
```

在仓库根目录启动静态文件服务器，例如已安装 Python 时运行：

```sh
python -m http.server 8000 --bind 127.0.0.1
```

然后打开 [http://127.0.0.1:8000/preview.html](http://127.0.0.1:8000/preview.html)。示例通过 HTTP 读取 JSON 与图片，请通过静态服务器访问；嵌入其他项目时，修改 `base` 为复制后的角色目录地址。

只需展示预览动画时，可直接引用 WebP：

```html
<img src="resources/dafeiyu/webp/dafeiyu_idle.webp" alt="大肥鱼待机动画" width="240">
```

多数动作的 WebP 设置为播放一次，查看结束后可重新打开预览。游戏内的播放逻辑以 metadata 为准。

## 资源来源与许可

资源元数据将制作来源标注为 AI 生成，并记录了透明轮廓提取等处理信息。当前仓库未附带 `LICENSE` 或明确的资源授权说明，使用与分发的许可范围需由维护者补充确认。
