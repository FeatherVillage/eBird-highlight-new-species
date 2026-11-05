# eBird 未见鸟种高亮（eBird Highlight New Species）

一个 Tampermonkey 用户脚本：用你自己的 eBird 生涯清单（lifelist）来标记**你还没见过的鸟种**，省去在页面上一个个对照的功夫。

> 本仓库是 [wzy0421/jsfiles](https://github.com/wzy0421/jsfiles) 的 fork 改写版。原脚本是「英文名 → 英文名 (中文名)」的方向；这个改版把高亮逻辑拆出来单独维护，并修了大量误判。

## 功能

| 场景 | 表现 |
|---|---|
| 普通页面里出现你没见过的种 | 名字变 `#A35F00`（棕橙），后面加一个 `*` |
| 单国特有种且你没见过 | `#C0262E`（深红） |
| 打开媒体查看器 / 灯箱时，非特有种 | `#FFD52E`（亮黄） |
| 已见过的种 | 不变色 |
| 逃逸种、未识别、杂交、`sp.`、未描述的类群 | 永不高亮 |

- 支持 `ebird.org`、`media.ebird.org`、`macaulaylibrary.org`。
- 生态位：物种页、checklist、地区页、批量照片列表、媒体网格与查看器等都有覆盖。
- 合并种/拆分类群（如 Redpoll、白喉林莺复合种）与官方带括号别名（如 `纹胸鹛 (纹胸巨鹛)`）都做了等价识别。
- 清单同步只认**国家/地区**（`world`、`CN`、`CN-33`…）；热点（`L1509300`）与年度清单不会被当成地区清单写入。
- 清单存在 Tampermonkey 存储里（`GM_getValue`），ebird.org 与 media.ebird.org 跨域共享。

## 安装

1. 浏览器装 [Tampermonkey](https://www.tampermonkey.net/)。
2. 点这里安装：[**eBird-highlight-new-species.user.js**](https://raw.githubusercontent.com/FeatherVillage/eBird-highlight-new-species/main/eBird-highlight-new-species.user.js)
   （Tampermonkey 会直接弹出安装页；如果只是显示了源码，说明还没装 Tampermonkey。）
3. 先打开一次你的生涯清单，例如 <https://ebird.org/lifelist?time=life&r=world>，等右下角出现「已同步 N 种」的提示。
4. 之后正常浏览 eBird 即可。

想换地区（比如广东 `CN-44`）就打开对应地区的生涯清单页 `https://ebird.org/lifelist?time=life&r=CN-44`，脚本会自动换成本地区的名单。

## 说明与边界

- 清单同步靠页面里的「已记录物种数」与物种名列表，所以**清单页需要完整加载**；半加载时会等待计数稳定再写入。
- 页面里的英文名、学名、非鸟文本不会被误标（只认已知中文鸟名）。
- 卡片被站点复用、文本在之后被替换时，脚本会重新核对当前文本，标记会跟着更新，不会残留。
- eBird 有时会给中文名带上官方括注别名（全库 55 个，例如 `斑头秋沙鸭 (白秋沙鸭)`），这类名字与裸主名互认。

## 数据来源

- 鸟名与特有种信息来自 **eBird Taxonomy API**（Cornell Lab of Ornithology）：<https://api.ebird.org/v2/ref/taxonomy/ebird>。
- 本脚本只读取你浏览器里已经加载的清单页面，不额外请求你的个人数据，也不上传任何东西。

## 致谢 / 上游

- 原始思路与最早的「eBird 中文鸟名 / 未记录种高亮」脚本：[wzy0421/jsfiles](https://github.com/wzy0421/jsfiles)（`birdMap.js`、`birdMapV2.js`）。
- 鸟名数据：eBird / Cornell Lab of Ornithology。

## 许可

上游仓库没有 LICENSE 文件，也没有在脚本头部声明许可，因此**本仓库不额外声明任何开源许可证**，也不代表作者放弃了权利：

- 版权归原作者与本改版作者所有；
- 欢迎个人安装使用、提 issue 反馈问题；
- 如需**再分发**（打包进别的项目、发布到应用商店/镜像站、改个名字再发）或商业使用，请先联系原作者取得许可。

如果你就是原作者并希望这里采用某个许可证，开个 issue 说一声即可。

## 版本历史

- **3.0.5** — 热点清单（`?r=L1509300`）不再被当成地区清单同步；只认同家/地区。
- **3.0.4** — 带括号官方别名（55 个）不再被误判成未见；旧名 ↔ 主名双向识别；拆分类群不合并。
- **3.0.3** — 修掉「已见过的鸟偶尔被标成未见」：节点复用/文本被替换后重新核对，星号按节点记录，颜色随场景即时切换。
- **3.0.2** — 保留官方括号别名；物种页顶栏的「上一个/下一个种」不加英文名（配套的名字脚本改动）。
- **3.0.0** — 从合并脚本拆成两个：加中英名 / 高亮未见种。
