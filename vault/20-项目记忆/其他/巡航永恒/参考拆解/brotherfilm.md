# Brother Film Co. 拆解

> 网址：https://brotherfilmco.com ｜ 类型：影视制作公司官网（伦敦 Peckham，作品集 + 片单；Awwwards Site of the Day 2020-03-09） ｜ 拆解：2026-11-05 ｜ 证据：桌面 16 帧、手机 5 帧、CSS/JS 112 个文件（72 CSS 多为插件按实例生成的内联样式 + 40 JS）+ curl 取 5 张噪点贴图量尺寸和灰度

## 一句话
把整个网站当成一条正在放映的胶片：底色本身就是一张会跳的噪点图，每段作品视频外面绕一圈像胶片片边印字一样滚动的项目名，背后压着 10% 透明度的巨型压缩体片名；界面只有黑、白、灰，颜色全部留给片子本身。

## 架构与技术栈
- 框架 / 渲染：WordPress 自制主题 `wp-theme-brother`，jQuery 2.2.4 + jQuery UI；Google Site Kit。没有前端框架（summary.framework 只有 wordpress）。
- 动效 / 滚动 / 3D / 声音：TweenMax（GSAP 2 代）；smooth-scrollbar（`Scrollbar.init(#scroll_wrap)`，damping 0.05，页面 overflow:hidden，帧名全是 y0）；jquery.marquee（片边印字）；three.js r111 + OBJLoader / MTLLoader（一个带立方体环境折射的 3D 物件，左下角那块白色折面）；lottie-web（加载 Logo 动画 `#logo_anim`）。无声音。
- 托管与内容来源：Cloudflare（响应头 cf-ray）；作品视频全部嵌 Vimeo 播放器（`controls=0`、白色主题）；片单走 WP 自定义文章类型 films、分类 advertising / charity / originals，分页 /films/page/2…5。
- 页面结构（按截图）：加载黑屏 → 首屏「BROTHER FILM」四行叠排（上三行被裁掉一半，像连续几格胶片）+ 一句等宽字简介 → Featured projects：每个作品一格视频，外框四边滚动印字（BEST OF BROTHER / QUADRANT X SHOPIFY SIX YEARS / BARBOUR SCOTLAND…），背后巨型描边片名 → 「VIEW ALL FILMS」+ 页脚联系方式 → 继续滚动自动转到 /films（黑屏 + 中心小点过渡）→「OUR WORK」+ 片单网格 + 底部筛选条。手机首屏先给「BROTHER MUSIC / BROTHER FILM」二选一。
- 路由与转场：多页 + AJAX 转场（标题从首页变成「Brother Film | Films」，scan 期间加载 9 个 document，多数是 Vimeo iframe）。

## 视觉与配色
| 角色 | 色值 | 用在哪 |
|---|---|---|
| 底色 | #000000 + 噪点图 bg1.jpg（平均亮度约 #0C0C0C） | `.bg_layout` 固定底，实际看到的「黑」是一张颗粒图 |
| 主文字 | #FFFFFF | 等宽小字、导航、片边印字 |
| 次文字 | #FFFFFF 50% | 「Go to Brother Music」等次要链接 |
| 首屏大字 | 看图约 #9A9A9A | 「BROTHER FILM」叠排 |
| 幽灵片名 | #D0D0D0 10%，描边 1px 白 10–20% | 视频背后的巨型片名、联系页大标题 |
| 白色模式 | #FFFFFF + 白噪点 noise1.jpg / whitenoise1.gif | 菜单展开态的白底 |
| 分隔线 | 白 21% 透明、1px | 顶栏、筛选条、Cookie 条上下各一条，间隔 2px 的双线 |

## 字体与排版
| 角色 | 字体 | 字号 1440 / 390 | 字重 | 行高 / 字距 | 大小写 |
|---|---|---|---|---|---|
| 首屏 / 章节巨字 | Druk Condensed Super | 216px（.home_section_title）/ 看图约 64px | 400（字体本身极粗） | 0.9 / 0 | 全大写 |
| 联系页大标题 | Druk Condensed Super | 277px | 400 | 0.9 / 0 | 全大写 |
| 视频背后幽灵片名 | Druk Condensed Super | 193px（片单页 315px） | 400 | 0.9 / 0 | 全大写 |
| 正文 / 导航 | Source Code Pro | 14–16px / 12px | 300 | 1.33 / 0 | 原样 |
| 片边印字 | Source Code Pro | 13px / 看图约 9px | 300 | 1.0 / 0 | 全大写 |
| 小字（邮箱、筛选） | Source Code Pro | 11–12px | 300 | 1.33 / 0 | 原样 |

授权：Druk Condensed Super 商用付费（Commercial Type），自托管 woff 46 KB；Source Code Pro 开源（OFL），Google Fonts 200/300。
- 排版手法：一个极窄极粗的展示体只做巨字（200px 以上），一个细等宽体做所有功能字（12–16px，字重 300），大小比约 15:1；中间没有第三档。巨字两种用法：叠排裁切当「连续几格」，或 10% 透明 + 细描边垫在视频后面当水印。导航竖排旋转 −90°。

## 布局
- 版式：中轴单列，视频格约 1035×585（看图，约 72% 屏宽），左边竖排导航（fixed，bottom 50px、left 70px，rotate(−90deg)），右上一个外链，右下竖排「Featured projects ↑↓」切换。
- 内容区：文本块 max-width 740–750px，容器 1200px。
- 断点：480 / 500 / 600 / 767 / 900 / 991 / 1199 / 1200 / 1640 / 1920，另有按高度的 `(min-width:1400px) and (min-height:1000px / 1200px)`。
- 手机：导航收到顶部一行，视频满宽，底部固定条「← Featured projects →」左右切换。

## 动效
| 触发 | 效果 | 时长 | 缓动 | 实现 |
|---|---|---|---|---|
| 常驻 | 底色噪点图平移跳动（见质感） | 6s 一轮，约每 67ms 跳一格 | steps(9) | CSS `@keyframes grained` |
| 常驻 | 片边印字绕视频外框滚动，四边各一条 | jquery.marquee duration 25000ms；各实例生成的 keyframes 49–109s 一圈 | linear | jquery.marquee `direction:'right', duplicated:true` |
| 滚动 | 平滑滚动 | 每帧 | damping 0.05 | smooth-scrollbar |
| 进场 | 作品格 translateY(100px)→0、opacity 0→1 | 500ms，delay 200ms | ease | `.in_view .films_list_item` |
| 进场 | 装饰形状闪现 opacity 0→1→0 | 800–1100ms，delay 200ms | linear | `@keyframes shape_show` |
| 悬停 | 光标小球放大 | 200ms | cubic-bezier(.175,.885,.32,1.275)（回弹） | `.theBall`，body `cursor:none` |
| 悬停 | 视频背后幽灵片名浮现、上移 | 400ms（颜色 500ms） | ease | `.film_bg_title.title_showing` top 100%→85% |
| 通用 | 透明度过渡 | 400ms（48 处） | ease | CSS transitions |

## 交互与组件
- 导航：左侧竖排四个词 Films / Clients / About / Contact，顶栏 `mix-blend-mode:difference`，滚到顶部区域时竖排导航淡出。
- 光标：隐藏系统光标，换成一个小球 `.theBall`，在视频上变成白色菱形「GO」（看图）。
- 片单页：底部固定筛选条（ALL / Advertising / Charity / Originals），背景是 50fps 跳动的噪点 GIF，上沿 2px 双线。
- 加载：黑底 + Lottie Logo + 计数（`.counter` 30% 透明）。
- 声音：无。视频静音无控件（`controls=0`）。
- 手机：首屏「BROTHER MUSIC / BROTHER FILM」两家公司二选一；作品左右滑。

## 质感
- 颗粒做法：**平铺噪点图片 + CSS 平移**，三种素材：
  - 底色 bg1.jpg：200×200、16.6 KB，深灰噪点（平均灰度 12/255，标准差 4.1），挂在 fixed 的 `.bg_layout:before`，宽高 140%、left/top −20%，`animation: grained 6s steps(9) infinite`，关键帧在 ±10% 之间 translate3d 跳。
  - 条带 noise1.gif：200×200 动图，18 帧、每帧 20ms（约 50fps），深灰噪点；用在顶栏、筛选条、Cookie 条和它们上下 2px 的细线上，还用作片名字的填充（`background-clip:text`）。
  - 白色模式：whitenoise1.gif（26 帧 ×20ms，平均灰度 247）、noise1.jpg（100×100）。弹窗用更暗的 bg.jpg（平均 7.5），opacity .9。
- 混合模式：噪点层都是 normal，不叠 overlay；它就是底色本身。
- 性能：贴图合计约 120 KB，动画只动 transform（`translate3d` 开合成层）；GIF 条带靠浏览器解码，不占 JS。
- 胶片隐喻：视频外框四边的滚动小字 = 胶片片边印字；首屏同一行字叠四次、裁掉上半 = 连续画格。

## 可借鉴到巡航永恒
- 学：视频 / 照片外框四边绕一圈 13px 等宽小字，慢速滚动（一圈 50–100s）→「胶片印样」首页每一格的片边：印车型、年份、发动机代号和镜头编号（如「X48 · ROLL 03 · FRAME 12」），像真底片的边码。
- 学：底色直接用一张 200px 噪点图（灰度 12/255、标准差 4 左右）平铺 + `steps(9)` 6s 跳动，代替纯 #000 → 巡航永恒全站的「黑」，和 Cultish 的叠层法二选一。
- 学：巨型压缩体只出现在两处：叠排裁切当连续画格、10% 透明垫在画面后当水印 → 三台车章节的片名（X48 / FAT BOY / BREAKOUT）垫在冷启动视频后面。
- 学：一粗一细两套字、大小比约 15:1，功能字统一 12–16px 等宽细体 → 规格表、引线标注、时间轴刻度都用等宽体。
- 学：细线做成 2px 间隔的双线 + 噪点 GIF 填充 → 底部固定的章节切换条。
- 不学：WordPress + jQuery + GSAP 2 的老栈；72 个插件生成的内联样式；Vimeo 外链播放器在部分网络下会弹人机验证（见没读到）；隐藏系统光标。

## 没读到
- 部分 Vimeo 嵌入画面显示「We couldn't verify the security of your connection」（Vimeo 侧的 Cloudflare Turnstile 人机验证，请求里有 challenges.cloudflare.com），按规矩没有绕过，那几段片子的内容没看到。
- 桌面开场 0.5–8 秒全黑，加载动画（Lottie Logo、计数）没截到；首页是否也有 Music / Film 二选一只在手机上看到。
- 3D 物件（OBJ + 折射）具体是什么形状、怎么跟鼠标动，只看到左下角一角。
- 滚动到底自动转到 /films 是看帧序推断的转场方式，没读到对应脚本。
- 站点 2020 年获奖，部分插件已过时（控制台 6 条 401），现在的手感可能和获奖时不同。
</content>
</invoke>
