# format.obys.agency 拆解

> 网址：https://format.obys.agency/ ｜ 类型：未确认（被拦，没拆） ｜ 拆解：2026-11-04 ｜ 证据：有效帧 0（桌面 6 帧、手机 3 帧拍到的都是 nginx 403 页）、站点 CSS/JS 0 个（抓到的 1 个 CSS 和内联脚本都属于 403 页）

## 一句话
未拆。主页对扫描浏览器返回 403，robots.txt 又点名禁止 Claude 系代理，按规矩停止，没有绕过。

## 架构与技术栈
- 只知道服务器是 nginx（响应头 `server: nginx`、`x-ray` 头，robots.txt 带「adm.tools Managed content」，是主机商面板统一加的屏蔽段）。其余未读到。

## 视觉与配色
未读到。

## 字体与排版
未读到。

## 布局
未读到。

## 动效
未读到。

## 交互与组件
未读到。

## 质感
未读到。

## 可借鉴到巡航永恒
- 暂无。要拆得由 Joe 本人用自己的浏览器打开，录屏或截图后交给 AI 分析；或换一个参考站。

## 没读到
- 2026-11-04 主页 `https://format.obys.agency/` 对扫描用的无头 Edge（经本机代理 127.0.0.1:8800）返回 403 Forbidden（页面标题「403 Forbidden - Nginx」），首屏和滚动帧全是错误页。
- 同日 robots.txt（200）对 ClaudeBot、Claude-User、Claude-SearchBot、GPTBot、PerplexityBot 等一长串 AI / SEO 爬虫写了 `Disallow: /`，`User-agent: *` 才是 Allow。`sitemap.xml` 404。
- 没有换 UA、换出口 IP、改用 curl 去重试：robots.txt 明确拒绝 Claude 代理，重试就是绕过。
- 因此它是字体站、杂志还是作品集，排版网格、文字动效、颗粒参数、页面转场全部未知。
