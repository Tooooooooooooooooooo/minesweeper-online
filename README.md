# 联机扫雷 Minesweeper Online

Windows 经典风格的在线联机多人扫雷游戏，单文件 HTML，无需任何后端。

## 功能

- 经典扫雷玩法：左键翻开、右键插旗/双击数字展开、首击安全
- 联机对战（GoEasy pubsub 中继，杭州节点）：创建/加入房间、房间加密、实时同步
- **加入方零延迟展开**：棋盘快照自带雷区布局，加入方点击本地立即展开，无需等主机回包
- 大厅：GoEasy channel 房间列表、全网排行榜
- 当局扫雷统计：每位玩家扫出的雷数实时对比
- 分色显示：按玩家给扫出的格子染色
- 隐藏彩蛋：英文输入法下输入 `chimei` 开启辅助
- 难度：初级 / 中级 / 高级 / 满屏自适应

## 本地运行

直接用浏览器打开 `minesweeper-online.html` 即可（联机需要能访问公网）。

## GoEasy appkey（已配置）

联机传输使用 **GoEasy pubsub**（国内免费 WebSocket 中继，杭州节点）。
`index.html` 已内置真实 appkey，**开箱即用，无需任何配置**：

```js
const GOEASY_APPKEY = 'BC-bc4854af72304030828cd59a65c7869a';
```

实测数据（2026-08-13，大陆直连杭州节点）：
- 连接建立：~164ms
- 房间消息端到端转发（预热后稳态）：**34~41ms，平均 36ms**
- 对比原方案 EMQX 美东节点 ~500ms，整体提速约 14 倍

> 若你要换用自己的账号：注册 https://www.goeasy.io/cn/signup.html →
> 创建应用（区域选**杭州**，host 为 `hangzhou.goeasy.io`）→ 替换上面常量即可。
> 免费版额度：500 日活 / 10 万条消息每月；大厅与排行榜基于 channel
> 最近 30 条消息（history），个人小规模使用完全足够。

## 部署到线上

### 方式一：任意静态托管（推荐，最简单）

本目录为纯静态站点（入口 `index.html`），可直接拖到任意静态托管平台：
Netlify / Vercel / Cloudflare Pages / Render / GitHub Pages 均可，无需构建。

### 方式二：Render.com（免费）

1. 把本目录内容推送到一个 GitHub 仓库
2. 登录 https://render.com → New → **Blueprint**（选择仓库，自动读取 `render.yaml`）
   或 New → **Static Site**（手动配置：Build Command 留空，Publish Directory 填 `.`）
3. 部署完成后得到 `https://<name>.onrender.com` 公网链接

> 说明：房间/排行榜数据经由 GoEasy 公共中继转发，属免费公共基础设施；
> 房间名即房间号，房间密码由创建者设置，未加密传输（与同类休闲联机一致）。
