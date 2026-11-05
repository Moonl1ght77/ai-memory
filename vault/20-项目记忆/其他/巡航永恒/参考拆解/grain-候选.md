# 胶片颗粒参考站 · 候选清单

> 2026-11-05 整理。找法：Awwwards 的 Film & TV、Photography、Filters and Effects、Retro 标签列表（2019-10 至 2026-11，共约 1100 张获奖卡片）→ 缩略图拼版肉眼挑黑白 / 低饱和 → 首页 HTML + CSS 关键字打分（feTurbulence、grain / noise 类和贴图、grain 关键帧）→ 再用 scan.mjs 扫截图确认；另查 Codrops 案例。排除名单里的 18 个站没有再看。
> 标准：颗粒是视觉主角之一、复古电影 / 胶片摄影气质、黑白或低饱和、年代不太老、获奖、现在能打开。

## 入选（已写拆解）
- David Lynch Tribute ｜ https://davidlynch.uprock.pro/ ｜ Awwwards HM 2025-11-02（[来源](https://www.awwwards.com/sites/david-lynch-tribute)）｜ 入选：黑白 + 红的林奇作品专题，往下滚 = 往后放，颗粒烧在 Lottie 逐帧素材里；视频 12 个 404 是硬伤 → davidlynch.md
- CULTISH™ 5 ｜ https://5.cultish.com/ ｜ Awwwards HM 2026-03-29（[来源](https://www.awwwards.com/sites/cultish-5)）｜ 入选：全站颗粒最重（canvas 生成 128px 噪点、opacity .8、steps(10) 跳动），400 张照片全是黑白街拍；门后 3D 漫游没截到 → cultish5.md
- Brother Film Co. ｜ https://brotherfilmco.com ｜ Awwwards SOTD 2020-03-09（[来源](https://www.awwwards.com/sites/brother-film-co)）｜ 入选：底色就是一张跳动的噪点图，视频外框绕一圈片边印字，界面纯黑白灰；片子本身是彩色广告 → brotherfilm.md

## 扫过截图后淘汰
- Podium ｜ https://podium.global/ ｜ SOTD 2026-06-27（[来源](https://www.awwwards.com/sites/podium)）+ Codrops 案例（[来源](https://tympanus.net/codrops/2026/06/23/podium-building-a-website-where-running-becomes-storytelling/)）｜ 淘汰：颗粒只在首屏跟鼠标走的着色器里，主体是彩色运动片截图 + 3D 石头
- A24 概念站（Ravi Klaassens）｜ https://a24.raviklaassens.com/ ｜ SOTD 2026-10-26（[来源](https://www.awwwards.com/sites/a24)）｜ 淘汰：浅灰底彩色 DVD 盘片轮播，噪点层 opacity .2 几乎看不出
- Josh Goldsmith ｜ https://josh-goldsmith.com/ ｜ HM 2026-08-12（[来源](https://www.awwwards.com/sites/josh-goldsmith-director-ep)）｜ 淘汰：有 SVG feTurbulence 颗粒（baseFrequency .85、opacity .175、screen），但主体是彩色广告视频网格，不复古
- 21 Hours on the Moon ｜ https://www.21hrs.space/ ｜ SOTD 2026-07-10（[来源](https://www.awwwards.com/sites/21-hrs-on-the-moon)）｜ 淘汰：阿波罗 11 号月面地图式漫游，没有颗粒层，像纪录片控制台
- Let It Rip Pictures ｜ https://letitrippictures.com ｜ HM 2026-02-24（[来源](https://www.awwwards.com/sites/let-it-rip-pictures)）｜ 淘汰：彩色广告片轮播，无颗粒
- Thomas Mamfredas（Art of noticing）｜ https://thomasmamfredas.com ｜ HM 2026-04-12（[来源](https://www.awwwards.com/sites/art-of-noticing)）｜ 淘汰：黑底照片拼贴，彩色黑白混排，无颗粒层
- Mario Roudil ｜ https://marioroudil.com/ ｜ SOTD 2025-04-10（[来源](https://www.awwwards.com/sites/mario-roudil)）｜ 淘汰：白底彩色缩略图横条，无颗粒
- Unbelievably Crap（Liubomyr Ulytskyi）｜ https://www.unbelievablycrap.com/ ｜ SOTD 2026-10-29（[来源](https://www.awwwards.com/sites/unbelievably-crap)）｜ 淘汰：有 film grain 层，但整体是红黑科技界面，不是电影 / 胶片气质
- Depo Luxe ｜ https://depoluxe.xyz/ ｜ SOTD 2026-07-07（[来源](https://www.awwwards.com/sites/depo-luxe)）｜ 淘汰：全屏模糊彩色视频，是模糊不是颗粒
- Perfect Days 电影官网 ｜ https://www.perfectdays-movie.jp/ ｜ HM 2023-12-19（[来源](https://www.awwwards.com/sites/perfect-days-official-site)）｜ 淘汰：电影感很强，但彩色低饱和，噪声贴图只用来做 WebGL 扰动，不是颗粒（可作「放映日记」氛围参考）
- Diego Stevens ｜ https://diegostevens.com/ ｜ HM 2025-05-19（[来源](https://www.awwwards.com/sites/diegostevens-com)）｜ 淘汰：颗粒视频只在顶栏「Movie」模式里出现（整页去色 + sepia + 模糊 + 缩到 0.9 圆角，噪点视频 exclusion .3），默认页不颗粒；内容是创业者个人品牌。「放映模式」开关可单独借
- Hollywood Exhibit 2026（ELLE & Esquire）｜ https://www.hollywoodexhibit2026.com ｜ SOTD 2026-08-02（[来源](https://www.awwwards.com/sites/hearst-exhibit-2026)）｜ 淘汰：彩色杂志封面画廊，噪点层（noise.png、.15s step-end）很淡
- Belward Tree Films ｜ http://belwardtreefilms.com/ ｜ HM 2020-12-14（[来源](https://www.awwwards.com/sites/belward-tree-films)）｜ 淘汰：奶油底 + 暖色人像，噪点 jpg 很淡
- Siena Film Foundation ｜ https://siena.film ｜ SOTD 2025-03-18（[来源](https://www.awwwards.com/sites/siena-film-foundation)）｜ 淘汰：ENTER 门挡住，脚本只拍到门口 Logo；缩略图看是暖色剧照

## 被拦或没扫完
- Orage Studio ｜ https://orage.studio/ ｜ SOTD 2025-10-29（[来源](https://www.awwwards.com/sites/orage)）｜ 淘汰：无头浏览器 403（NinjaFirewall），按规矩不绕过；源码看是着色器颗粒 `pow(grain,15.)*0.8`（稀疏亮点，像灰尘），值得以后手动看
- Partizan ｜ https://partizan.com/ ｜ SOTD 2026-07-26（[来源](https://www.awwwards.com/sites/partizan)）｜ 淘汰：curl 200 但无头浏览器 403，没截到图；源码颗粒 uGrainAmount 0.03，很轻
- Paul Kram ｜ https://paulkram.com/ ｜ HM 2025-12-17（[来源](https://www.awwwards.com/sites/paulkram-photographer)）｜ 淘汰：无头浏览器 403
- YK Produce ｜ https://ykproduce.co.jp/ ｜ HM 2026-03-23（[来源](https://www.awwwards.com/sites/yk-produce-inc)）｜ 淘汰：扫描 2 分半还没出帧，我中途停了（WebGL 截图慢），未看到；源码快筛无颗粒关键字

## 只做了源码快筛就淘汰
- Hardik Bhansali「22.09.2000 he was born」｜ https://www.hardikbhansali.com/ ｜ SOTD 2025-07-23（[来源](https://www.awwwards.com/sites/22-09-2000-he-was-born)）｜ 淘汰：VHS / CRT 故障着色器，暖色家庭录像感，不是胶片颗粒
- Killian Herzer ｜ http://killianherzer.com ｜ HM 2026-04-11（[来源](https://www.awwwards.com/sites/killian-herzer)）｜ 淘汰：SVG 颗粒（baseFrequency .8、3 octaves、500px 平铺），但是红黑开发者作品集
- Jonathan Grado ｜ https://www.jonathangrado.com ｜ HM 2024-09-17（[来源](https://www.awwwards.com/sites/jonathan-grado)）｜ 淘汰：同款 canvas 噪点（256px、alpha 25、8s steps(10)），只铺在红色区块
- Velour Productions ｜ https://velourproductions.com/ ｜ HM 2026-09-18（[来源](https://www.awwwards.com/sites/velour-productions)）｜ 淘汰：有颗粒关键帧，但主色红橙 #FE3000
- Jealous Films ｜ https://showcase.studiothis.co.uk/jealous ｜ HM 2026-09-19（[来源](https://www.awwwards.com/sites/jealous-films)）｜ 淘汰：黑白调色板但源码无颗粒
- Milledollars ｜ https://milledollars.fr/ ｜ SOTD 2026-10-02（[来源](https://www.awwwards.com/sites/milledollars)）｜ 淘汰：无颗粒层
- La Dernière Danse（Ronin161）｜ https://ladernieredanse.ronin161.com/ ｜ HM 2025-01-20（[来源](https://www.awwwards.com/sites/ladernieredanse)）｜ 淘汰：UE5 短片站，源码无颗粒；Ronin161 主站颗粒在 3D 后期里，见 Codrops 案例（[来源](https://tympanus.net/codrops/2024/02/20/case-study-ronin161s-portfolio-2024/)）
</content>
</invoke>
