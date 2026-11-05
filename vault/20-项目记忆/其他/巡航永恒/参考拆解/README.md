# 巡航永恒 · 参考站拆解（索引与归纳）

> 2026-11-04～05 家用机。Joe 给的 9 个站拆了 8 个，另找 3 个胶片颗粒浓的站。参考页（第三版）：https://claude.ai/artifact/X4hNyb2gJGfwMddc4v1YTE
> 方法：拆站脚本（本机 Edge 截开场和滚动多帧 + 计算样式 + 抓 CSS/JS 做关键字统计）+ 读源码。脚本和模板在 `E:\Projects\eternal-cruise\tools\拆站\`。
> 抽查：对源码复核 9 处关键数值，8 处一致；Bugatti「视频秒数 × 30vh」没在压缩代码里找到原式，未复核。
> 各拆解里写的截图路径在会话临时目录，可能已被清掉；代表帧另存在 `E:\Projects\eternal-cruise\参考截图\`（只在家用机），参考页里也有。

## 文件
| 站 | 文件 | 一句话 | 技术栈 |
|---|---|---|---|
| Immersive Garden | immersive-g.md | 一个连续的石膏浮雕 3D 展厅，换页靠镜头推进 | Nuxt 3 + three r151 + GSAP 3.12.5 + Lenis 1.1.14 + Howler |
| Bugatti Tourbillon | bugatti-tourbillon.md | 把车当机械表讲，滚动逐段剖开车身 | SvelteKit + GSAP 3.14.2（ScrollSmoother）+ imgix 视频 |
| darkroom.engineering | darkroom.md | 黑底红字的暗房仪器，悬停让网点照片「显影」 | Next 16 + GSAP 3.15 + Lenis 2 dev + three r187（只作品页） |
| Santioni | santioni.md | 会动的铜版画短片，每秒 8 帧，年龄门兼开声音 | Active Theory 自研 HydraX + Sanity |
| Buck's Sauce 单品页 | bucks-sauce.md | 一叠贴纸和卡牌，卖点靠两张卡洗牌 | Next 16 + GSAP 3.14.2 + Lenis + R3F + Shopify |
| PX PUSH | pxpush.md | 开着的 CRT 电视，四层纯 CSS 屏幕质感 | Nuxt 3.19 + GSAP 3.13 + Lenis 1.3.11 + three r180 |
| serotoninn | serotoninn.md | 黑白照片撕开一道口露出原色（无头浏览器被拦，只读源码） | WordPress + Vite + GSAP 3.14.2 + Lenis 1.1.20 |
| Made in Evolve · Kissuomo | evolve-kissuomo.md | 案例页是一卷放不完的片子，结尾就是下一部的封面 | Next 16.2（Webflow 导出）+ GSAP 3.15 + Lenis 1.3.25 |
| David Lynch Tribute | davidlynch.md | 6 部片做成一卷，滚动逐帧拖着黑白颗粒画面走 | Taptop 无代码 + GSAP 3.12.5 + lottie |
| CULTISH™ 5 | cultish5.md | 400 张黑白街拍里 3D 漫游，全屏噪点压成同一种底片 | React + R3F three r182 + Motion + Lenis |
| Brother Film Co. | brotherfilm.md | 正在放映的胶片，底色就是跳动的噪点图 | WordPress + jQuery + GSAP 2 + three r111（2020） |
| Obys Format | obys-format.md | 未拆：站方 403，robots.txt 禁 Claude 系代理 | — |
| 颗粒站候选 | grain-候选.md | 20 多个候选的入选 / 淘汰理由 | — |

## 顶级站的 8 条共同做法
1. **一个隐喻管全站**：展厅、机械表、暗房仪器、CRT 电视、放映中的胶片……加载、导航、转场、页脚都用这个隐喻的词汇说话。巡航永恒的隐喻：一场黑白公路电影的放映。
2. **质感是独立的一层，能调能关**：pxpush 四层 CSS（颗粒 overlay .3 跳位 / 暗角 .7 / 全屏 blur 10px@.15 / 扫描线）；Cultish 一层噪点 opacity .8，三档只改透明度；Brother 底色本身就是噪点图。
3. **时间被量化，不跑满 60 帧**：Santioni 8fps、Cultish 12.5fps、pxpush 20fps、Brother 约 15fps 跳一次。巡航永恒按 24 或 12 帧跳，像放映机，也省 GPU。
4. **滚动就是播放头**：Bugatti 视频跟滚动、Lynch 帧号 = 滚动进度、Evolve 46 帧 / 300vh、Santioni 每幕 1.25–4 屏的配置表。首页「一部短片」是成熟做法，不是冒险。
5. **转场带信息**：darkroom 幕布写目的地名、Evolve 黑幕写目的地名且结尾是下一部封面（同一张图）、immersive-g 推镜 1.3s + 黑场 1.2s。
6. **开场是仪式，也是声音授权**：immersive-g 加载即片头字卡、serotoninn 钟面扫开 3s、Santioni 年龄门点一下顺便开声音、Cultish「Step inside / sound on」。
7. **一条签名缓动 + 一张动效数值表**：serotoninn `cubic-bezier(.75,0,.25,1)` 0.7s 用遍全站；Buck's 全站时长 .25–1.25s、错峰 .028–.05s，元素只挂 data 属性。
8. **字体三分工、大小对比极端**：Brother 巨型压缩体 216px 对等宽 12–16px（约 15:1）；Lynch 片名卡 285px 行高 .54；Bugatti 等宽小字当仪表读数；Evolve 84px 行高 .8 + 打字机索引。

反面共性（巡航永恒要反着做，也是作品集里能讲的差异点）：首访动辄 18–53 MB；11 个站里至少 6 个完全没处理「减少动态效果」；immersive-g、Cultish 把字画进 WebGL 或做成图片，读屏读不到；Buck's 动画元素默认透明等 JS，桌面首屏黑 6 秒。

## 巡航永恒的组合方案
- **A 放映厅（推荐）**：首页是一部滚动短片。片头倒计时（serotoninn）→ 片名字距漏斗（Lynch）→「有声观看 / 静音观看」（Santioni、Cultish）→ 黑边推到 2.39:1 → 冷启动按滚动逐帧（Bugatti、Lynch）+ 仪器面板式引线标注（darkroom）+ 按住点火（Santioni）→ 上路分镜按幕高配置表（Santioni）+ 旁白逐行显影（immersive-g）→ 发动机代际字母顶替（Bugatti）→ 三台车巨型片名卡（Lynch、Brother）→「下一卷」字卡黑场转场（Evolve、darkroom）→ 片尾字标残影 + 翻牌（pxpush、serotoninn）。车型页照 Evolve 一卷片的结构，结尾是下一台车的封面。
- **B 暗房印样**：首页是一张 4×3 印样；每格片边印字（Brother），默认网点版悬停显影（darkroom），当前格油性笔圈（Buck's），点格推镜放大成宽银幕（immersive-g）。作品集目录感最强，主角时刻分散。建议把它做成车型 / 故事的列表页，和 A 一起用。
- **C 一镜到底**：全站一个连续的 WebGL 黑白夜路场景（immersive-g 式），换页 = 镜头沿路推进。炫技上限最高，但首访 20 MB 以上、手机吃力、读屏要另做一套 DOM，工期最长。

## 颗粒做法对比
| 做法 | 出处 | 参数 | 代价 |
|---|---|---|---|
| 开场生成噪点 PNG + 伪元素平铺跳位 | Cultish | 128px、alpha 30/255、300% 平铺、opacity .8、8s steps(10) | 最轻，只动 transform |
| 噪点图当底色 | Brother | 200px JPG 灰度 12/255、140% 平铺、6s steps(9) | 约 120 KB，黑不再是纯黑 |
| 四层屏幕叠加 | pxpush | 颗粒 overlay .3、.5s steps(1)；暗角 .7；blur 10px@.15 | 全屏 blur 在手机上贵，它手机直接关颗粒 |
| 蓝噪声跟滚动走 | Santioni | multiply 10%，静止不闪 | 着色器里做，需要 WebGL |
| 颗粒烧在素材里 | Lynch | Lottie 位图序列 | 21 MB，最重 |
推荐：Cultish 的生成法当全站颗粒层（三档 opacity），Brother 的噪点底色代替纯黑；按 24 或 12 帧跳；手机降一档，减少动态效果时静止。

## 技术栈观察
- GSAP 出现在 10/11 个站（Cultish 用 Motion）；Lenis 7/11；three.js 7/11；框架五花八门（Next 3、Nuxt 2、SvelteKit 1、WordPress 2、无代码 1、自研 1）。效果靠「GSAP + Lenis + 需要时才上 three.js」和素材处理，不靠框架。
- GSAP 自 2025-04-30 起全部免费，含 SplitText、MorphSVG 等原付费插件，可商用（来源：webflow.com/blog/gsap-becomes-free）。
- Lenis 参考值：darkroom 滚轮 lerp .125、只在 `(pointer: fine)` 启用、减少动态时 lerp 1、转场时锁滚动；immersive-g 桌面 lerp .05；Cultish .08。

## 没读到
- Obys Format：站方拒绝，未拆；要看请 Joe 自己截图或录屏。
- Santioni 年龄门后的 18 幕、serotoninn 的实际画面、Cultish 门后的 3D 漫游、各站声音：只读了源码，没看到或没听到。
- 手感（缓动顺不顺、滚动阻尼）截图看不出来，留给 Joe 点原站看。
