---
title: "Wpseo Video"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Changelogs
    - Repos
    - Data
---

## 15.1

发布日期：2025-11-18

#### 功能增强

* 检测由区块输出的视频。

#### 错误修复

* 修复了作者文章 URL 包含 & 符号会导致 XML 无效的错误。
* 修复了 Wistia 嵌入代码因 `embedType` 未知而无法正确识别的错误。
* 修复了重新索引后保存会导致设置丢失的错误。
* 修复了重新索引视频时未使用已保存视频标签的错误。
* 修复了使用 Wistia 时视频站点地图包含无效 `player_loc` 的错误。

#### 其他

* 将最低要求的 Yoast SEO 版本提升至 26.4。
* 取消对 PHP 7.2 和 7.3 的兼容性支持。
* 将最低支持的 WordPress 版本设置为 6.7。
* 将“WordPress 测试至”版本设置为 6.8。

## 15.0

发布日期：2024-12-03

#### 错误修复

* 修复了“打开搜索外观”按钮无法使用的错误。

#### 其他

* 将 _Google 预览编辑器_ 更名为 _搜索外观_，以匹配 Yoast SEO 中的命名方式。
* 将最低要求的 Yoast SEO 版本设为 24.0。
* 将最低支持的 WordPress 版本设为 6.5。
* 将 _WordPress 测试至_ 版本设为 6.7。
* 修复了 WordPress 6.7 中关于 `_load_textdomain_just_in_time` 加载不正确的 PHP 提示。

## 14.9

发布日期：2024-05-28

#### 功能增强

* 增强了 `VideoObject` 模式对象的 `uploadDate` 属性，将其格式从仅日期改为日期时间，以满足富媒体搜索结果的最新推荐要求。
* 为 Yoast Video SEO 引入了一种新的翻译获取方式，通过利用 TranslationPress 服务。现在我们可以根据每个安装的用户设置来加载翻译，而无需在每个版本中都附带所有翻译。这意味着插件发布包更小，用户服务器上的冗余更少。

#### 其他

* 修复了对嵌入式 TED 视频的支持。
* 提升了安全策略的可发现性。
* 明确了必需的 PHP 扩展。
* 将所有 Twitter 引用重命名为 X。
* 将 Yoast SEO 最低要求版本设置为 22.8。
* 将最低支持的 WordPress 版本设置为 6.4。
* 将测试通过的 WordPress 最高版本设置为 6.5。
* 通过 [WP]Packagist 使用此包的用户现在可以使用 `composer/installers` v2。

## 14.8

发布日期：2023-08-08

#### 功能增强

* 新增用于索引视频的 CLI 命令：`wp yoast video index`。

#### 错误修复

* 修复了在嵌入超过30天前访问过的视频页面时会抛出警告的问题。
* 修复了Wistia视频无法通过FitVids设置正确响应式显示，并会抛出关于jQuery控制台错误的问题。

#### 其他

* 不再兼容 PHP 5.6、7.0 和 7.1。
* 将 Yoast SEO 最低要求版本设为 20.13。
* 将最低支持的 WordPress 版本设为 6.1。
* 将已测试的 WordPress 最高版本设为 6.3。

## 14.7

发布日期：2023-03-14

#### 功能增强

* 改进了多个过滤器的内联文档。
* 通过添加 `$post_id` 参数，提升了 `wpseo_video_{$type}_details` 过滤器的可用性。
* 通过允许在视频不适合家庭观看时返回 `false`，提升了 `wpseo_video_family_friendly` 过滤器的可用性。

#### 错误修复

* 修复了因 `FILTER_SANITIZE_STRING` 而抛出 PHP8 弃用通知的错误。
* 修复了关于 `VideoObject` 片段中 `isFamilyFriendly` 属性被设置为字符串而非布尔值而抛出模式验证警告的错误。
* 修复了当 _XML 站点地图_ 被禁用时，指向 _XML 站点地图_ 设置的链接不正确的问题。
* 修复了当基石内容开关开启时，_视频标题_ 评估和 _视频正文_ 评估也会出现在可读性分析标签下的问题。

#### 其他

* 提升与 PHP 8.1 的兼容性。
* 减少 PHP 8.1 弃用通知的干扰。
* 将 Yoast SEO 最低要求版本设为 20.3。
* 将最低支持的 WordPress 版本设为 6.0。
* 停止支持 [Flowplayer5](https://wordpress.org/plugins/flowplayer5/) 插件，因其已不再受作者维护。
* 停止支持 [JW Player for WordPress](https://wordpress.org/plugins/jw-player-plugin-for-wordpress/) 插件，因其已不再受作者维护。
* 停止支持 [Smart YouTube](https://wordpress.org/plugins/smart-youtube/) 插件，因其已不再受作者维护。
* 停止支持 [TubePress](https://wordpress.org/plugins/tubepress/) 插件，因其已不再受作者维护。

## 14.6

发布日期：2022-07-12

#### 功能增强

* 更新了 Schema 操作，以遵循 Yoast SEO 19.3 版本为 `WebPage` 片段 `@id` 引入的新最佳实践。

#### 其他

* 将测试通过的 WordPress 版本上限设为 6.0。
* 将最低要求的 Yoast SEO 版本设为 19.3。

## 14.5

发布日期：2022-05-06

#### 功能改进

* 遵循 Google 的弃用政策，移除了 `video:category` 属性以及 `player_loc` 属性上的 `allow_embed` 属性。
* 移除了视频分类的输入字段。
* 移除了站点地图 `Tag` 列中的尾部逗号。

#### 其他

* 将 Yoast SEO 的最低要求版本设置为 18.8。

## 14.4

发布日期：2022-04-19

#### 错误修复

* 修复了访问 RSS2 订阅源时会显示 `Only variables should be assigned by reference` PHP 提示的缺陷。
* 修复了包含视频但无可用 `WP_Post` 对象的页面会抛出 PHP 提示的缺陷。

#### 其他

* 将 Yoast SEO 最低要求版本设置为 18.6。
* 将 WordPress 最低要求版本设置为 5.8。

## 14.3

发布日期：2022-01-25

#### 功能增强

* 针对日语视频正文评估进行调整，改为按字符而非单词计数文本长度，并应用不同的长度边界。

#### 其他

* 将所需最低免费版本提升至 18.0。
* 将 WordPress 测试版本上限设置为 5.9。

## 14.2
 
发布日期：2021-12-14

#### 其他

* 确保与 Yoast SEO Free 17.8 版本兼容。
* 将最低必需的 Yoast SEO 版本设置为 17.8。

## 14.1

发布日期：2021-08-10

#### 错误修复

* 修复了当 Yoast SEO Premium 与 WP 5.8 同时启用时，文章编辑器中 Yoast 侧边栏会显示重复的 Video 部分的错误。

## 14.0
 
发布日期：2021-07-13

#### 功能增强

* 将所有视频 SEO 元标签的键/值对添加到我们的 REST API 中。
* 将 Yoast SEO 的最低必需版本设置为 16.7。

#### 其他

* 将最低要求的 WordPress 版本设置为 5.6。
* 将最低要求的 Yoast SEO 版本设置为 16.7。

## 13.9

发布日期：2021-06-03

#### 错误修复

* 修复了启用 `YouTube 嵌入：仅在用户点击播放时加载 YouTube 播放器以加快页面加载速度` 功能时，嵌入式 YouTube 视频上不显示播放图标的问题。

## 13.8
 
发布日期：2021-04-06

#### 功能增强

* 在 `VideoObject` Schema 输出中添加 `isPartOf` 属性，用于引用 `Article` 或 `WebPage` schema。
* 将 `VideoObject` Schema 输出中的 `uploadDate` 格式更改为 `yyyy-mm-dd`，移除了时间部分。

#### 其他

* 将 WordPress 测试版本上限设为 5.7，最低支持的 WordPress 版本设为 5.6。

## 13.7

发布日期：2021-02-23

#### 功能增强

* 在 Elementor 的 Yoast 侧边栏中新增 Yoast Video SEO 功能。

## 13.6

发布日期：2021-02-10

#### 功能增强

* 通过在所有 Yoast 元框中使用统一组件，优化了视频元框的外观。
* 在区块编辑器的 Yoast 侧边栏中新增视频设置选项。
* 优化了视频标签输入功能，使其更加直观易用。
* 新增“YouTube 嵌入”选项，用户点击播放图标时才会加载视频播放器，从而提升含 YouTube 视频页面的加载速度。

#### 其他

* 新增通知功能，告知用户设置变更。

## 13.5

发布日期：2020-01-12

#### 改进

* 新增钩子 (`wpseo_video_supported_plugins`)，允许开发者添加对 Yoast 官方未支持插件的兼容。感谢 [Ian Jenkins](https://github.com/jenkoian)。
* 当视频未设置描述时，输出“无描述”以避免生成无效的 videoObject 架构。

## 13.4
 
发布日期：2020-10-27

#### 功能增强

* 在 XML 站点地图中将文本字段包裹在 CDATA 块中，以防止奇怪的 XML 和 HTML 实体破坏站点地图。
* 改进了视频 SEO 元框中“禁用视频”复选框的文案。
* 当文章已拥有 Facebook 图片时，不再输出视频图片/静态图像。

## 13.3
 
发布日期：2020-08-18

#### 其他

* 激活插件时启用跟踪功能。可在 Yoast SEO 配置向导中禁用此功能。
* 将最低支持的 WordPress 版本设置为 5.4。

## 13.2

发布日期：2020-07-21

#### 错误修复

* 修复了当视频从页面移除但未从元数据框中移除手动分配的视频缩略图时，会出现 PHP 警告的错误。

## 13.1

发布日期：2020-05-13

#### 错误修复

* 修复了视频站点地图中视频会输出 `last_fetched` 标签的错误。

## 13.0
 
发布日期：2020-04-28

#### 功能增强

* 不再输出 `og:video:secure_url` 元标签，因为它总是会与类似的 `og:video` 标签一同输出。
* 对于包含视频的文章和页面，将 `og:type` 元标签的值从 `video.other` 更改为默认的 `article` 类型。

#### 其他

* YouTube 视频嵌入的元数据现在每 30 天刷新一次，以解决与 YouTube 服务条款的合规性问题，其中规定公共数据最多只能缓存 30 天。
* 使 `VideoObject` 模式功能与 Yoast SEO 14.0 兼容。
* 将最低支持的 Yoast SEO 版本设置为 14.0。
* 将最低 WordPress 版本设置为 5.3。

## 12.5

发布日期：2020-03-31

#### 功能增强

* 在 VideoObject 模式片段中新增了 inLanguage 属性。

## 12.4.1

发布日期：2020-03-04

#### 错误修复

* 修复了 YouTube 视频无法被识别的问题，该问题曾导致视频无法被包含在 XML 视频站点地图中，且元数据框中的视频设置不可用。您可以通过视频 SEO 选项中的“重新索引视频”按钮来重新索引您的 YouTube 视频。

## 12.4

发布日期：2020-01-07

#### 错误修复

* 修复了 `ya:ovs:upload_date` 元标签会输出错误时间的错误。
* 修复了在多站点环境中结合 MultilingualPress 使用时，会保存错误视频元数据的错误。

## 12.3.1

发布日期：2019-11-27

#### 错误修复

* 修复了一个当 Yoast SEO 与另一个包含名为 `Date_Helper` 类的插件或主题同时使用时，会抛出致命错误的缺陷。

#### 其他

* 将 Yoast SEO 最低版本要求设置为 12.6.1。

## 12.3

发布日期：2019-11-26

#### 其他

* 将最低要求的 WordPress 版本设置为 5.2，最低 YoastSEO 版本设置为 12.6，最低 PHP 版本设置为 5.6.20。
* 将 Video SEO 管理页面上的 HelpScout Beacon 1.0 替换为 Beacon 2.0。
* 修复了查看 RSS 源时可能出现的“未定义索引”提示。

## 12.2

发布日期：2019-10-15

#### 功能增强

* 当浏览器窗口中看不到常规按钮时，在 Yoast SEO Video 管理页面上显示一个浮动的 `保存设置` 按钮。

#### 其他

* 兼容 Yoast SEO 12.2

## 12.1

发布日期：2019-09-17

#### 其他

* 兼容 Yoast SEO 12.1

## 12.0

发布日期：2019-09-03

#### 其他

* 兼容 Yoast SEO 12.0

## 11.9

发布日期：2019-08-20

#### 功能增强

* 即使安装了多个 Yoast SEO 附加组件，该附加组件现在也会在 Yoast SEO 元数据框中拥有自己的独立标签页。

#### 其他

* 将 Yoast SEO 的最低必需版本设置为 11.9。

## 11.8

发布日期：2019-08-06

#### 错误修复

* 修复了视频标题评估和视频正文评估未加载的错误。

#### 其他

* 兼容 Yoast SEO 11.8

## 11.7

发布日期：2019-07-23

#### 其他

* 兼容 Yoast SEO 11.7

## 11.6

发布日期：2019-07-09

#### 其他

* 兼容 Yoast SEO 11.6

## 11.5

发布日期：2019-06-25

#### 其他

* 兼容 Yoast SEO 11.5

## 11.4

发布日期：2019-06-12

#### 其他

* 兼容 Yoast SEO 11.4

## 11.3

发布日期：2019-05-28

#### 其他

* 兼容 Yoast SEO 11.3

## 11.2

发布日期：2019-05-15

#### 其他

* 兼容 Yoast SEO 11.2

## 11.1

发布日期：2019-04-30

#### 功能增强

* 视频 SEO 现已采用 Yoast SEO 的新结构化数据方案。[在我们的文档中了解更多关于结构化数据输出的信息](https://yoa.st/video-schema)。

#### 错误修复

* 修复了当 FV Flowplayer Video Player 插件启用时，在编辑器中出现的致命错误。

## 11.0

发布日期：2019-04-16

#### 其他

* 兼容 Yoast SEO 11.0

## 10.1

发布日期：2019-04-02

#### 其他

* 将 Yoast SEO 的最低必需版本设置为 10.1。
* 移除了已弃用的方法（版本 &lt; 6.1）。

## 10.0

发布日期：2019-03-12

#### 其他

* 兼容 Yoast SEO 10.0

## 9.7

发布日期：2019-02-26

#### 其他

* 兼容 Yoast SEO 9.7

## 9.6.1

发布日期：2019-02-12

#### 其他

* 兼容 Yoast SEO 9.6.1

## 9.6

发布日期：2019-02-12

#### 其他

* 兼容 Yoast SEO 9.6

## 9.5

发布日期：2019-01-22

#### 其他

* 兼容 Yoast SEO 9.5

## 9.4

发布日期：2019-01-08

#### 其他

* 兼容 Yoast SEO 9.4

## 9.3

发布日期：2018-12-18

#### 其他

* 兼容 Yoast SEO 9.3

## 9.2

发布日期：2018-11-20

#### 其他

* 兼容 Yoast SEO 9.2

## 9.1

发布日期：2018-11-06

#### 其他

* 兼容 Yoast SEO 9.1

## 9.0

发布日期：2018-10-23

#### 其他

* 兼容 Yoast SEO 9.0

## 8.4

发布日期：2018-10-09

#### 错误修复

* 修复了插件更新时变更日志无法显示的缺陷。

#### 其他

* 兼容 Yoast SEO 8.4

## 8.3

发布日期：2018-09-25

* 兼容 Yoast SEO 8.3

## 8.2

发布日期：2018-09-11

* 修复了一个错误：视频缩略图被用作 OpenGraph 图像，而非社交预览部分设置的图像，导致 Facebook 显示错误图片。
* 兼容 Yoast SEO 8.2

## 8.1

发布日期：2018-08-28

* 兼容 Yoast SEO 8.1

## 8.0.1

发布日期：2018-08-21

* 修复了一个因文件被错误名称引用而导致其始终无法正确加载的错误。

## 8.0

发布日期：2018-08-14

* 兼容 Yoast SEO 8.0

## 7.9.1

发布日期：2018-08-07

* 兼容 Yoast SEO 7.9.1

## 7.9

发布日期：2018-07-24

* 兼容 Yoast SEO 7.9

## 7.8

发布日期：2018-07-10

* 兼容 Yoast SEO 7.8

## 7.7

发布日期：2018-06-26

* 兼容 Yoast SEO 7.7

## 7.6

发布日期：2018-06-05

* 兼容 Yoast SEO 7.6

## 7.5

发布日期：2018-05-15

* 兼容 Yoast SEO 7.5

## 7.4

发布日期：2018-05-03

* 兼容 Yoast SEO 7.4

## 7.3

发布日期：2018-04-17

* 兼容 Yoast SEO 7.3

## 7.2

发布日期：2018-04-03

* 安全加固。
* 兼容 Yoast SEO 7.2

## 7.1

发布日期：2018-03-20

* 为所有已软弃用的方法、操作、钩子或过滤器添加了消息。为四个尚未有弃用消息的函数添加了弃用消息。
* 兼容 Yoast SEO 7.1

## 7.0

发布日期：2018-03-06

更新内容：

* 修改了激活警告信息，不再暗示激活失败，而是说明只要 Yoast SEO 未激活，相关功能将无法正常使用。

#### 其他

* 需要安装 Yoast SEO 7.0 或更高版本。
* 移除了对 [Vzaar 视频平台](https://vzaar.com/) 的支持。
* 移除了对通过以下插件和主题添加视频的支持，这些插件和主题已不再可用、不再（积极）维护或已被插件作者弃用：
    * [Embed Plus 高级 YouTube 嵌入插件](https://wordpress.org/plugins/embedplus-for-wordpress/)
    * [YouTube 内嵌框架嵌入](https://wordpress.org/plugins/iframe-embed-for-youtube/)
    * [Instabuilder](https://instabuilder.com/v2.0/launch/)
    * [KISS YouTube 插件](https://wordpress.org/plugins/kiss-youtube/)
    * PluginBuddy VidEmbed
    * [简易视频嵌入器](https://wordpress.org/plugins/simple-video-embedder/)
    * [Sublime Video](https://wordpress.org/plugins/sublimevideo-official/)
    * [VideoJS - WordPress HTML5 视频播放器](https://wordpress.org/plugins/videojs-html5-video-player-for-wordpress/)
    * [VideoPress](https://wordpress.org/plugins/video/)
    * [Viper 视频快速标签](https://wordpress.org/plugins/vipers-video-quicktags/)
    * [Vippy](https://wordpress.org/plugins/vippy/)
    * [Vzaar 媒体管理](https://wordpress.org/plugins/vzaar-media-management/)
    * [Vzaar 官方媒体管理器](https://wordpress.org/plugins/vzaar-official-plugin/)
    * Weaver 主题
    * [WordPress 视频插件](https://wordpress.org/plugins/wordpress-video-plugin/)
    * WP OS FLV
    * [WP YouTube 播放器](https://wordpress.org/plugins/wp-youtube-player/)
    * [YouTube 插入我](https://wordpress.org/plugins/youtube-insert-me/)
    * [YouTube 短代码](https://wordpress.org/plugins/youtube-shortcode/)
    * [YouTube 白标短代码](https://wordpress.org/plugins/youtube-white-label-shortcode/)
    * [带样式的 YouTube](https://wordpress.org/plugins/youtube-with-style/)
    * [YouTuber](https://wordpress.org/plugins/youtuber/)
    * Premise
    * [WordPress 自动 YouTube 视频发布](https://wordpress.org/plugins/automatic-youtube-video-posts/)

## 6.3

发布日期：2018-02-13

* 当主页和站点 URL 相同时，从静态文件加载 XSL 样式表。
* 兼容 Yoast SEO 6.3

## 6.2

发布日期：2018-01-23

* 兼容 Yoast SEO 6.2

## 6.1

发布日期：2018-01-09

* 兼容 Yoast SEO 6.1

## 6.0

发布日期：2017-12-20

* 兼容 Yoast SEO 6.0

## 5.9

发布日期：2017-12-05

#### 变更内容

* 移除了当 Yoast SEO Premium 未激活时停用此插件的功能。
* 兼容 Yoast SEO 5.9。

## 5.8

发布日期：2017-11-15

* 兼容 Yoast SEO 5.8

## 5.7

发布日期：2017-10-24

* 兼容 Yoast SEO 5.7

## 5.6

发布日期：2017-10-10

#### 变更内容

* 将子菜单注册所依赖的权限能力更改为 `wpseo_manage_options`
* 将子菜单的注册方式更改为使用 `wpseo_submenu_pages` 过滤器

#### 错误修复

* 修复了许可证检查端点使用错误 URL 的问题

## 5.5

发布日期：2017-09-26

* 将国际化模块更新至 3.0 版本。
* 兼容 Yoast SEO 5.5。

## 5.4

发布日期：2017-09-06

* 兼容 Yoast SEO 5.4。

## 5.3

发布日期：2017-08-22

* 修复了生成视频站点地图时调用已弃用方法的问题。
* 移除了 `wp_installing` 的 polyfill。

## 5.2

发布日期：2017-08-08

* 兼容 Yoast SEO 5.2。

## 5.1

发布日期：2017-07-25

* 修复了 `isFamilyFriendly` 元属性未正确设置的错误。

## 5.0

发布日期：2017-07-06

* 兼容 Yoast SEO 5.0。

## 4.9

发布日期：2017-06-07

* 兼容 Yoast SEO 4.9。

## 4.8

发布日期：2017-05-23

* 兼容 Yoast SEO 4.8。

## 4.7

发布日期：2017-05-02

* 兼容 Yoast SEO 4.7。

## 4.6

发布日期：2017-04-11

* 兼容 Yoast SEO 4.6。

## 4.5

发布日期：2017-03-21

* 仅使已配置文章类型的站点地图失效。
* 修复了一个错误：当插件在未安装 Yoast SEO 或 Yoast SEO Premium 的情况下激活时，会引发致命错误。

## 4.4

发布日期：2017-02-28

* 为视频评分字段添加了最小值和最大值。
* 添加了 `og:video:secure_url` 元标签。

## 4.3

发布日期：2017-02-14

* 兼容 Yoast SEO 4.3。

## 4.2.1

发布日期：2017-02-03

#### 错误修复

* 修复了“致命错误：未找到类 'yoast_i18n'”的问题。

## 4.2

发布日期：2017-01-31

#### 错误修复

* 修复了缺失或未遵循指南的翻译者评论。

## 4.1

发布日期：2017-01-17

#### 错误修复

* 修复了指向谷歌视频站点地图文章的链接。
* 修复了视频SEO菜单会覆盖“升级高级版”菜单项的错误。
* 修复：如果文章使用了带有变量的自定义标题/描述模板，这些变量在视频站点地图中未能被正确替换的问题。
* 微小的拼写和语法修正。
* 如果之前检测到视频，但文章类型后来被排除在VideoSEO之外，视频的OpenGraph和Schema标签仍会被添加到前端页面。此问题现已修复。
* 修复视频对象元标签的大小写敏感性问题。
* 微小的XHTML语法修正。

#### 功能增强

* 优化通知信息的样式设计。
* 针对 Yoast SEO 的兼容性进行小幅改进。
* 对按钮和翻译的用户界面进行细微优化。
* 在视频页面头部添加 video:duration 标签。
* 明确说明“允许其他网站嵌入视频”选项的具体作用。
* 在元数据编辑框中更清晰地描述“家庭友好”选项。
* 通过添加一些 Yandex 专属标签来增强对 Yandex 搜索引擎的支持。可使用新的 wpseovideoyandex_support 过滤器关闭此功能（返回 false 即可关闭）。
* 允许通过新的 wpseovideoobjectmetacontent 过滤器添加额外的 Schema 元标签，例如为视频对象添加文字稿。
* 更清晰地描述“家庭友好”复选框。

## 4.0

发布日期：2016-12-13

* 修复了 YouTube 视频播放器 URL，使其始终使用协议。这解决了 Google 使站点地图失效以及 Facebook 无法识别播放器的问题。（现有帖子需要强制重新索引）

## 3.9

发布日期：2016-11-29

#### 功能增强

*   新增对更多 Wistia 视频 URL 和嵌入代码的支持。如果您使用 Wistia 视频服务，强烈建议重新索引您的视频。
*   为私有 Vimeo 视频的详细信息检索添加了后备方案。这将允许识别这些视频。（现有文章需要强制重新索引）。
*   新增对 //player.vimeo.com/... 类型 URL 的识别。（现有文章需要强制重新索引）。
*   将 'og:type' 元值更改为更准确的 'video.other'。
*   将 'og:video:type' 元值更改为 HTML5，在大多数情况下现在比 Flash 更准确。
*   在 WP 多站点上安装时，行为略有改进。

#### 错误修复

* 修复了 YouTube 视频播放器 URL。这应能解决在 Facebook 等网站嵌入视频时出现的黑屏和/或“无法解析 DNS”错误。（现有帖子需强制重新索引）
* 将 Vimeo 视频播放器 URL 更新为新的 HTML5 播放器格式（支持 Flash 回退）。这应能解决在 Facebook 等网站嵌入这些视频时出现的黑屏和/或“无法解析 DNS”错误。（现有帖子需强制重新索引）

## 3.8
 
发布日期：2016-11-08

#### 功能增强

* VideoSEO 插件现在将遵循 wpseo_sitemaps_base_url 过滤器。
* 使 oEmbed 识别功能与即将发布的 WP 4.7 兼容。

#### 错误修复

* 修复了添加 YouTube 视频时出现的致命错误。
* 修复了在自定义文章类型中添加视频时显示未定义索引的错误。
* 修复了对 Advanced Responsive Video Embedder 插件的支持。
* 修复了当存在自定义文章类型“video”时，站点地图样式错误的问题。
* 确保视频站点地图在插件激活后立即可用，并在停用后不可用。
* 修复了每篇文章设置中的“禁用此文章的视频”选项未在 og: 元标签中生效的问题，该问题导致即使文章视频被禁用，Facebook 仍会显示视频。
* 如果视频文章的发布日期遇到无效日期，将重新评估发布日期。
* 如果视频文章标题或内容/摘要已更新或曾经更新过，现在将在站点地图和视频元数据中反映出来。（现有文章需要强制重新索引）
* 如果视频文章的 SEO 标题或 SEO 描述已添加或调整，现在将在站点地图中反映出来。（现有文章需要强制重新索引）
* 如果已为包含视频的文章类型设置了 SEO 描述模板，现在将生效。（现有文章需要强制重新索引）
* 如果视频文章最初保存为草稿，之后才发布，站点地图中的发布日期会卡在草稿日期，此问题已修复。（现有文章需要强制重新索引）
* “强制重新索引”功能在实现进度条时出现故障，现已修复。勾选“强制重新索引”复选框现在将再次按预期工作，包括缩略图的重新生成。
* “重新索引”功能未正确遵循 VideoSEO 设置页面上为视频站点地图设置的待索引文章类型，无意中导致重新索引的项目少于应有数量，此问题已修复。
* 重新索引功能已变得更高效，对于相同数量的文章，现在应该更快。
* 重新索引后，站点地图缓存未自动清除，此问题已修复。

## 3.7

发布日期：2016-10-11

#### 功能增强

* 为 uStudio 视频添加了基于 iframe 的支持。
* 添加了缺失的 index.php 文件。

## 3.6

发布日期：2016-09-27

#### 功能增强

* 通用无障碍访问功能改进。

## 3.5

发布日期：2016-09-07

#### 变更内容

* 新增对 Featured Video 插件的支持，感谢贡献者 [ahoereth](https://github.com/ahoereth)

## 3.4

发布日期：2016-07-19

#### 变更

* 更新了翻译。

## 3.3

发布日期：2016-06-14

#### 功能增强

* 在 Yoast SEO Video 设置页面添加了 Yoast i18n 模块，用于告知用户该插件尚未提供其语言版本，并说明他们可以采取的措施。
* 错误修复
* 修复了 Yoast SEO Video 的支持信标被添加到所有 Yoast SEO 设置页面的问题。
* 修复了当多个付费 Yoast 插件同时激活时，更新功能无法可靠运行的问题。

## 3.2

发布日期：2016-04-20

* 修复了激活时视频站点地图缓存未被清除的错误。
* 修复了添加到内容分析中的视频特定检查在 Yoast SEO 3.2 及更高版本中不再起作用的错误。
* 修复了在插件页面点击“立即更新”按钮无法正确更新的错误。

## 3.1

发布日期：2015-03-01

#### 错误修复

* 修复了文章编辑页面的一个 JS 错误，该错误在与 Yoast SEO 3.0.7 以上版本同时使用时会导致内容分析功能中断。
* 修复了一个错误，该错误有时会因 ssl 问题导致我们的许可证管理器无法连接到我们的许可系统。

#### 功能增强

* 确保用户在更新或禁用/启用插件后无需重新激活许可证
* 在视频 SEO 设置页面添加支持信标，使用户能够通过 WordPress 后台请求技术支持

## 3.0

发布日期：2015年11月18日

* 与所有其他 Yoast SEO WordPress 插件版本同步。

#### 错误修复

* 修复了在重新索引视频站点地图时可能出现的致命错误。
* 修复了与 Yoast SEO 3.0 结合使用时损坏的视频元框。
* 修复了 Yoast SEO 3.0 中已移除过滤器的弃用警告。

#### 功能增强

* 确保视频特定内容分析检查与 Yoast SEO 3.0 中的实时内容分析工具良好兼容。

## 2.0.4

发布日期：2015-06-23

#### 错误修复

* 修复了无法识别 https YouTube URL 的问题。
* 修复了保存选项时站点地图缓存不会被清除的问题。
* 更改为 YouTube v3 API，使 YouTube 集成功能恢复正常。
* 修复了滚动条颜色显示不正确的问题。

## 2.0.3
 
发布日期：2015-03-25

#### 错误修复

* 修复了视频站点地图可能包含格式错误的日期时间的问题。
* 修复了在 WooCommerce 中创建新产品时触发的全局变量 $post 的未定义索引通知。
* 修复了视频站点地图中标题变量解析不佳的问题。
* 修复了视频缩略图保存时没有扩展名的问题。

#### 增强功能

* 新增 5 种语言：en_GB、es_MX、fr_FR、nl_NL 和 tr_TR。

## 2.0.2

发布日期：2014-12-17

#### 错误修复：

* 修复代码片段预览中的通知问题

#### 功能增强

* 在重新索引视频站点地图时显示进度条

## 2.0.1

发布日期：2014-11-11

#### 错误修复

* 已修复：Vixxy 短代码/URL 组合未被识别的问题
* 已修复：缺少样式表的错误
* 已修复：站点地图上的限制问题

#### 功能增强

* 新增波斯语和巴西葡萄牙语翻译
* 移除法语、荷兰语和瑞典语翻译。如需协助翻译这些语言，请前往 translate.yoast.com 注册！
* 优化丹麦语、德语、匈牙利语和意大利语翻译

## 2.0
 
发布日期：2014-10-07

#### 错误修复

* 修复：短代码列表经常无法正确重置。
* 修复：转义的短代码仍会被搜索视频。
* 修复：无名称的短代码属性并非总能被识别。
* 修复：Flickr 视频详情获取失败，现在需要 SSL。
* 修复：[JW Player](https://wordpress.org/extend/plugins/jw-player-plugin-for-wordpress/) 插件支持与 fitvids.js 之间的兼容性问题。
* 修复：之前只检查找到的第一个短代码是否为视频短代码，然后回退到其他方法；现在会检查所有短代码直到找到视频短代码。如果未找到，仍会回退到其他查找视频的方法。
* 修复：wordpress.tv 视频详情并非总能获取到的小问题。
* 修复：`[videopress]` 短代码 - 尽管有插件支持 - 仍未被识别。
* 修复：数字视频 ID 有时会阻止视频详情获取的问题。
* 修复：vidyard 详情获取大部分失败的问题。
* 修复：最后一个识别的元字段内容会覆盖先前找到信息的错误。
* 修复：包含 html/文本和末尾 URL 混合内容的元字段可能被接受为 content_loc 的错误。
* 修复：VideoSEO 插件在升级 WPSEO 时会自动停用。此问题不应再发生。

#### 功能增强

* 新增支持识别视频附件，无需额外插件。
* 新增支持将 `.ogv` 文件识别为视频文件。
* 新增支持自定义 Wistia 域名。
* 更多视频 URL 将被正确识别。
* 全面增强对无协议 URL 的支持。
* 在管理栏中添加 Video SEO 菜单项。
* 由于 [Viddler 不再支持免费个人账户](https://gigaom.com/2014/02/07/viddler-gets-ready-to-delete-personal-videos/)，WP 4.0 移除了对 Viddler 视频的 oembed 支持。对于仍在使用 Viddler 的用户，Video SEO 将继续支持 OEmbed 功能以及 SEO 优化。

#### 支持的服务

* 新增对 23Video 视频的支持（获取视频详情）。
* 新增对 Archive.org 视频的支持（获取视频详情）。
* 新增对 CollegeHumor.com 视频的支持（获取视频详情）。
* 新增对 Funnyordie.com 视频的支持（获取视频详情 - 遗憾的是目前尚不支持短链接）。
* 新增对 Hulu.com 视频的支持（获取视频详情）。
* 新增对 Revision3 视频的支持（获取视频详情）。
* 新增对 TED 视频的支持（获取视频详情）。
* 新增对 VideoJug 视频的支持（获取视频详情）。
* 通过 Embedly 新增对 Snotr 视频的支持（有限的视频详情）。
* 通过 Embedly 新增对 Spike.com/IFilm 视频的支持（获取视频详情）。
* 通过 Embedly 新增对 Vine 视频的支持（获取视频详情）。
* 若无视频详情获取功能，仍将尝试通过 Embedly 获取详情。
* 大幅改进对上传/本地托管视频的支持（获取视频详情）。
* 改进对 YouTube（国家）子域名和替代协议（httpvhd、httpvhp、youtube::）的支持。移除了仅音频嵌入的支持，因为音频并非视频。
* 改进对 Animoto 视频的支持（识别网址）。
* 改进对 Blip.tv 视频的支持（增强识别能力，从而更好地获取视频详情）。
* 改进对 Dailymotion.com 视频的支持（识别短链接）。
* 改进对 Flickr 视频的支持（识别短链接并更好地获取视频详情）。
* 改进对 Viddler 视频的支持（获取视频详情）。
* 改进对 VideoPress 和 WordPress.tv 的支持（获取视频详情）。
* 改进对 Vimeo 视频的支持（网址识别和获取视频详情）。
* 改进对 Vzaar 视频的支持（网址识别和获取视频详情）。
* 改进对 Wistia 视频的支持（网址识别和获取视频详情）。
* 略微改进对 YouTube 视频的支持（获取视频详情）。

#### 支持的插件

* 新增对 [Flowplayer HTML5](https://wordpress.org/plugins/flowplayer5/) 插件的支持。
* 新增对 [JetPack](https://wordpress.org/plugins/jetpack/) 插件短代码模块的支持。
* 新增对 [VideoPress](https://wordpress.org/plugins/video/) 插件的支持。
* 新增对 [YouTube Embed Plus](https://wordpress.org/plugins/youtube-embed-plus/) 插件的支持。
* 改进对 [Advanced Responsive Video Embedder](https://wordpress.org/plugins/advanced-responsive-video-embedder/) 插件的支持 - 大量短代码此前未被识别。
* 改进对 [IFrame Embed for YouTube](https://wordpress.org/extend/plugins/iframe-embed-for-youtube/) 插件的支持 - 短代码此前未被识别。
* 改进对 [Simple Video Embedder](https://wordpress.org/plugins/simple-video-embedder/) 插件的支持 - 短代码已可识别，优化了对自定义字段的处理。
* 改进对 [Sublime Video](https://wordpress.org/extend/plugins/sublimevideo-official/) 插件的支持 - 此前未能识别所有可能的视频源。
* 改进对 [TubePress](https://wordpress.org/extend/plugins/tubepress/) 插件的支持 - 新增 Vimeo 支持。
* 改进对 [Viper Video Quicktags](https://wordpress.org/extend/plugins/vipers-video-quicktags/) 插件的支持 - 大量短代码此前未被识别。
* 改进对 [WP Video Lightbox](https://wordpress.org/extend/plugins/wp-video-lightbox/) 插件的支持 - 此前不支持缩略图。
* 改进对 [WP YouTube Player](https://wordpress.org/extend/plugins/wp-youtube-player/) 插件的支持 - 新增对 id（替代 url）以及 width、height 属性的支持。
* 改进对 [YouTuber](https://wordpress.org/extend/plugins/youtuber/) 插件的支持 - 此前不支持短代码。
* 改进对 [YouTube Embed](https://wordpress.org/extend/plugins/youtube-embed/) 插件的支持 - 增强了对替代协议的识别。
* 改进对 [YouTube with Style](https://wordpress.org/extend/plugins/youtube-with-style/) 插件的支持 - 播放列表语法此前会破坏支持。
* 移除对 [Better Youtube Embeds](https://wordpress.org/extend/plugins/dirtysuds-embed-youtube-iframe/) 插件的支持，因其功能现已包含在 WP 核心中且该插件已不再活跃。
* 移除对 [Instabuilder](https://instabuilder.com/) 插件的支持。
* 移除对 Premise 插件的显式支持。
* 移除对 [Youtube Brackets](https://wordpress.org/extend/plugins/youtube-brackets/) 插件的显式支持，因其已八年未更新。

#### 其他

* WordPress 最低要求现为 3.6。
* 添加了许可证信息
* 应用了一些最佳实践

## 1.7.2

发布日期：2014-07-17

* 修复了 1.7 更新后内容后添加空格的问题。

## 1.7.1

发布日期：2014-07-15

*   修复了因 1.7 版本中缺少版本号更新而导致的更新错误。

## 1.7

发布日期：2014-07-14

#### 错误修复

* 修复：`mrss_item` 中 `$content` 可能为空的问题。
* 修复：升级程序中的一个小错误。
* 修复：添加 Animoto 和 Screenr oembed 提供商时的错误。
* 修复：当提供冲突的 HTTP 协议时，站点地图出现错误的问题。
* 修复：即使未找到包含视频的文章，视频站点地图仍可能显示在站点地图索引中。
* 修复：从内容生成的视频描述可能在单词或 HTML 实体中间截断。
* 修复：插件激活时的错误。
* 修复：当存在名为 'video' 的自定义文章类型时，站点地图冲突的问题。
* 修复：元数据框中持续时间显示不正确的问题。

#### 功能增强

* 为 wistia.net 域名和 wistia 协议相对链接添加 oembed 支持
* 将语言文件加载移至 init 钩子，以支持翻译覆盖
* 改进上传文件的清理机制
* 更新摘要预览功能，采用与 WP SEO 先前更新一致的谷歌最新设计变更，修复部分用户遇到的 JavaScript 错误
* 在插件无法运行的情况下自动停用
* 提升获取的 YouTube 缩略图尺寸

## 1.6.3

发布日期：2014-03-31

#### 错误修复

* 修复了 sanitize_rating 中缺失变量导致的警告。

## 1.6.2

发布日期：2014-03-17

#### 错误修复

* 修复了缺失变量导致的警告。
* 更新 Fitvids.js 以解决其相关问题。

#### 增强功能

* 当 `SCRIPT_DEBUG` 开启时，Fitvids 将以未压缩版本包含。

## 1.6.1

发布日期：2014-03-11

#### 错误修复

* 修复错误的布尔值检查。

## 1.6

发布日期：2014-03-11

兼容 WPSEO 1.5 并实现了相同的选项与元数据理念。

#### 错误修复

* 已修复：不应以静态方式调用非静态方法
* 已修复：noindex 设置未得到正确遵循
* 已修复：部分管理表单文本不一致
* 已修复：加载新文章时的警告
* 已修复：更改后始终重新验证许可证密钥

#### i18n

* 更新了 .pot 文件
* 更新了 it_IT 语言文件

## 1.5.5.1

发布日期：2013-11-23

#### 错误修复

* 确保缩略图可用。
* 将插件的初始化移至更早的钩子，以确保生成 XML 站点地图时插件已就绪。

## 1.5.5

发布日期：2013-11-22

#### 错误修复

* 移除对 `WPSEO_URL` 常量的依赖。
* 修复手动选择缩略图时，OpenGraph 和 Schema.org 输出中使用了错误图片的问题。
* 在 `index_content()` 后恢复 `$shortcode_tags` 至原始状态。

#### 功能增强

* 使用媒体上传器更改视频缩略图。
* 新增设置，允许视频直接在 Facebook 上播放（默认开启）。

## 1.5.4.6

发布日期：2013-09-20

#### 错误修复

* 防止第 4169 行因未设置视频分类法而出现警告。
* 防止自定义字段键名中包含空格时出现问题。
* 增加了对更多 Dailymotion URL 的支持。

#### 功能增强

* 移除 CDATA，改用正确的实体编码。
* 在视频站点地图 XML 和 XSL 上强制使用 200 状态码和适当的缓存。
* 增加对 WP YouTube Lyte 短代码的支持。

#### i18n

* 将 wpseo-video.pot 重命名为 yoast-video-seo.pot
* 更新了 fr_FR
* 新增了 hu_HU

## 1.5.4.5

发布日期：2013-09-06

* 为充分利用本次更新的新功能，请重新索引您的视频。

#### 错误修复

* 修复了若干 i18n 命名空间问题。
* 使分类描述中的视频能够再次正常加载。
* 修复了 Wistia 弹窗嵌入和 Wistia https URL 的问题。
* 防止在 XML 视频站点地图中输出视频的 hd 属性。
* 确保开放图谱图像始终设置为“完整”尺寸。
* 为 YouTube 视频添加宽度和高度属性。
* 防止在显示分类术语中的视频时站点地图出现通知。
* 防止 XML 视频站点地图中出现错误或空日期。

#### 功能增强

* 新增为每个视频手动添加标签的选项。
* 新增覆盖视频分类的选项（通常默认使用首篇文章分类）。
* 在 XML 视频站点地图中按修改日期升序排列视频。
* 添加“完整”的 Facebook 视频集成功能。
* 新增对 [Advanced Responsive Video Embedder](https://wordpress.org/plugins/advanced-responsive-video-embedder/) 插件的支持。
* 新增对 muzu.tv 的支持。
* 允许检测包含数组的自定义字段。
* 新增对自定义 Vimeo URL 的支持（例如 [https://vimeo.com/yoast/video-seo](https://vimeo.com/yoast/video-seo)）。
* 确保视频缩略图始终也作为 og:image 输出。
* 新增对 Instabuilder 视频短代码的支持。
* 新增对 Vidyard 的支持。
* 通过常量设置许可证密钥。
* 新增对 Cincopa 的支持。
* 新增对 Brightcove 的支持。
* 在视频站点地图中新增对“归档介绍文本”（Genesis）内视频的支持。
* 新增对 WP OS FLV 插件的支持。
* 新增对 [Wordpress Automatic Youtube Video Post](https://wordpress.org/plugins/automatic-youtube-video-posts/) 插件的支持。

## 1.5.4.4

发布日期：2013-09-05

#### 错误修复

* 自定义字段设置中的空格现在会被正确修剪。
* 修复了 Vzaar URL 的问题。
* 现在能正确检测带有额外类的 Wistia 嵌入代码。

#### 功能增强

* 视频站点地图现在遵循与文章站点地图相同的分页规则。
* 新增初步的 Screenr 支持。
* 视频 XML 站点地图的日期现在能正确从最近修改的含视频文章中获取。

## 1.5.4.3

发布日期：2013-09-04

#### 功能增强

* 新增对 `fvplayer` 短代码的支持。
* 新增手动修改或输入时长的选项。

## 1.5.4.2

发布日期：2013-09-03

#### 错误修复

* 当文章禁用视频功能时，现已正确允许正常的元描述长度。

#### 功能增强

* 新增了禁用 RSS 增强功能的选项，以防止与播客插件发生冲突。

## 1.5.4.1

发布日期：2013-09-02

*   将插件的加载优先级调整为 20，以与核心 WordPress SEO 插件的升级保持一致。

## 1.5.4

发布日期：2013-09-01

#### 功能增强

* 新增对 [fitvids.js](https://fitvidsjs.com/) 的支持，在视频 SEO 设置中启用后，可使您的 Youtube / Vimeo / Blip.tv / Viddler / Wistia 视频具备响应式特性，即实现自适应布局。请注意此功能可能不兼容所有嵌入代码，若遇到特定代码无法使用的情况请告知我们。
* 移除了 ping 功能，该功能已在核心插件中修复。
* 新增强制更新检测：使用本插件需将 WordPress 升级至 3.4 或更高版本，并将 WordPress SEO 插件升级至 1.4 或更高版本。

#### 错误修复

* 修复了视频站点地图最后修改时间无法更新的问题。

## 1.5.3

发布日期：2013-08-01

#### 功能增强

* 改进默认设置：安装时默认启用所有公开文章类型
* 通过设置 `YOAST_VIDEO_SITEMAP_BASENAME` 常量，可更改视频站点地图的基础名称（从 video-sitemap.xml 改为任意名称）
* 若文章元数据值经过编码，插件现可自动解码

#### 错误修复

* 当已设置 OpenGraph 图片时，不再覆盖它。
* 在视频架构前添加额外换行，以便 oEmbed 正常工作。
* 不再依赖 Vzaar 服务器的响应来创建站点地图，正确使用引荐来源验证请求，并在设置中添加选项以配置您的 Vzaar CNAME。
* 当存在 slug 为 `video` 的文章类型时，插件现在会自动将基础名称更改为 `yoast-video`。
* 不再为元框中的空描述打印空的 `&lt;p&gt;` 标签。
* 改进逻辑，确保“此图片”链接仅在视频缩略图未被覆盖时正确显示。

## 1.5.2

发布日期：2013-06-01

#### 功能增强

* 新增对 Vzaar 视频的支持，可通过两种插件之一，使用 iframe、object embed 或短代码嵌入。
* 新增 TubePress 支持。

#### 错误修复

* 新增对 Wistia.net 的支持（不仅限于 .com）。
* 修复了解析 youtube_sc 短代码时的错误。

## 1.5.1

发布日期：2013-05-01

#### 错误修复

* 改进了激活功能。

#### 功能增强

* 新增对泰坦灯箱的支持。
* 防止了一些通知。

## 1.5

发布日期：2013-04-01

#### 错误修复

* 将 `mrss_gallery_lookup` 设为公开以防止出现通知。
* 修复了针对 YouTube 等平台的部分对象检测形式。
* 修复了 `[video]` 短代码的检测。

#### 功能增强

* 允许停用许可证密钥，以便在其他域名上使用。
* 在视频选项卡上添加指向检测到的缩略图的链接。
* 将文本域从 `wordpress-seo` 更改为 `yoast-video-seo`。
* 确保所有字符串均可翻译。
* 优化管理区域样式。

#### i18n

* 若需翻译，您现在可以将插件翻译为您的母语，详情请查看 [translate.yoast.com](https://translate.yoast.com/projects/yoast-video-seo)。
* 将文本域从 `wordpress-seo` 更改为 `yoast-video-seo`。
* 在仓库中添加了 .pot 文件。
* 添加了荷兰语翻译。

## 1.4.4

发布日期：2013-01-02

#### 错误修复

* 防止与 content_width 全局变量相关的问题。
* 防止尝试激活已激活的许可证。
* 防止自定义字段出现通知。
* 修复 wistia 弹出式嵌入功能。

#### 新功能

* 新增 PluginBuddy VidEmbed 支持。

## 1.4.3

发布日期：2012-12-02

#### 错误修复

* 现在可匹配页面中的多个 iframe / object 元素
* 修复了若干因 URL 未加引号导致嵌入内容无法识别的错误

#### 新功能

* 当主题未设置内容宽度时，新增了手动设置选项
* 新增对 Sublime video 及其官方 WordPress 插件的支持
* 新增 Animoto 的 SEO 和 oEmbed 支持
* 视频站点地图新增向 Bing 发送 ping 的功能
* 为 YouTube 嵌入新增大量支持的插件和短代码

## 1.4.2

发布日期：2012-11-02

#### 错误修复 / 功能增强

* 尝试防止许可证验证超时。
* 清理插件中的大量正则表达式。
* 防止使用相对图像 URL 路径以及仅将图像设置为 'h'。
* 防止文章重复输出。
* 修复了一个小错误，该错误曾导致 URL 中视频 ID 位置异常的 YouTube 链接无法正常工作。
* 改进 Wistia 嵌入支持。
* 延长视频信息请求的超时时间。

#### 新功能

* 新增对 html5 视频元素的支持（早该如此！）
* 新增对 [vimeo id= 和 [youtube id= 嵌入代码的支持
* 新增对自定义字段中仅包含文件 URL 的自托管视频的支持，此类视频将使用特色图像作为缩略图
* 若无视频缩略图，现可通用回退至文章缩略图

## 1.4

发布日期：2012-11-01

#### 错误修复 / 功能增强

* 修复 Vimeo 嵌入检测。
* 将 Vimeo 切换至 oEmbed API。
* 可用时，为 jwplayer 嵌入使用 html5_file。

#### 新功能

* 在 WordPress SEO 的页面分析标签页中新增了视频内容优化提示。
* 新增对 WP Video Lightbox 插件的支持。
* 新增对 Flowplayer 插件的初步支持。
* 新增对 Wistia 视频托管平台的支持。
* 新增对 Vippy 视频托管平台的支持（感谢 Ronald Huereca）。
* 新增对 Weaver 主题短代码的支持。

## 1.3.4

发布日期：2012-10-05

#### 错误修复

* 修复了 Viddler 检查。
* 修复了 videoObject 输出的标签剥离问题。
* 在 feed 中不过滤内容。
* 改进了 VideoPress 嵌入 ID 的解析。

#### 功能增强

* 新增支持检查视频的自定义字段。
* 新增支持 Press75 的 Simple Video Embedder（从而支持其所有主题）。

## 1.3.3

发布日期：2012-10-04

#### 错误修复

* 当路径为相对路径而非绝对路径时，正确捕获缩略图。
* 同时清理未正确注册短代码的插件。
* 防止出现空标题。
* 将 XML 站点地图和 MediaRSS 的文本内容包裹在 CDATA 标签中，这解决了约 90 万个编码问题。
* 修复了 [Veoh](https://www.veoh.com/) 支持。

#### 功能增强

* 当文章属于多个分类时，超出部分现将被用作标签。
* 对于没有缩略图且无内容位置或播放器位置的视频，不再输出站点地图行。
* 若描述和摘要为空，则使用标题作为描述，因为空描述是无效的。
* 更改了家庭友好变量的名称，以避免与旧数据产生冲突。
* 新增对 `video:uploader` 标签的支持，该标签会自动链接到文章作者的发布页面。
* 在 XML 站点地图中，使术语使用其自身名称作为分类。
* 新增对使用文件和图像属性（而非媒体 ID）的 jwplayer 短代码嵌入的支持。
* 新增对 [WordPress 视频插件](https://wordpress.org/extend/plugins/wordpress-video-plugin/) 的支持。
* 新增对 [MediaElements.js](https://wordpress.org/extend/plugins/media-element-html5-video-and-audio-player/) 插件的支持。
* 新增对 [WP YouTube Player](https://wordpress.org/extend/plugins/wp-youtube-player/) 插件的支持。
* 新增对 [Embed Plus 高级 YouTube 嵌入插件](https://wordpress.org/extend/plugins/embedplus-for-wordpress/) 的支持。
* 新增对 [VideoJS - WordPress HTML5 视频播放器](https://wordpress.org/extend/plugins/videojs-html5-video-player-for-wordpress/) 插件的支持。
* 新增对 [YouTube 短代码](https://wordpress.org/extend/plugins/youtube-shortcode/) 插件的支持。

## 1.3.2

发布日期：2012-10-03

#### 错误修复

* 修复 XSLT URL 问题，这次是真的。有时你不得不忽略 WordPress 的内部机制，因为它们就是<br />错的。这次就是这种情况。现在 XSL 文件的路径应该总是正确的。不过请注意“应该”这个词。
* 改进 YouTube ID 的匹配，显然它们也可以包含下划线。
* 通过连续循环处理 100 篇文章来改进重新索引过程，以避免内存问题。
* 修复了一个非常烦人的错误，即视频默认会被标记为非家庭友好内容。
* 强制将观看次数设为整数。

#### 功能改进

* 调整了家庭友好性逻辑。现在默认所有视频都是家庭友好的，<br />您需要勾选复选框才能将其设为非家庭友好。

## 1.3.1

发布日期：2012-10-02

#### 错误修复

* 防止使用图片的相对路径
* 防止 post_id 出现在 XML 视频站点地图中
* 修复指向 XSLT 的错误 URL

#### 功能增强

* 新增对 [JW Player 插件](https://wordpress.org/extend/plugins/jw-player-plugin-for-wordpress/) 嵌入的支持（目前仅支持 `mediaid=&lt;数字&gt;` 格式的嵌入）。

## 1.3

发布日期：2012-10-01

#### 错误修复

* 进行了更多 YouTube 嵌入修复，同时解决了空 YouTube ID 问题。
* 正确从 YouTube 获取缩略图，而非“假定”一个 URL。
* 改进了从 YouTube API 获取时长的代码。

#### Enhancements

* Add support for searching through category / tag / term descriptions for video content.
* Get viewcount from YouTube API.
* Add option to hide sitemap from everyone except admins and Googlebot.
* Add option to disable the video integration on a single post and page by adding a checkbox on the Video tab.
* Changed the way reindex gets called, so the admin keeps working immediately after a reindex without a refresh.
* Added option to force reindexing of old posts that have already been indexed as having video (normally<br />they're just refreshed but no external calls are being done).

## 1.2.2

发布日期：2012-09-03

#### 错误修复

* 正确支持 [youtube]视频ID[/youtube] 类型的嵌入短代码。

#### 功能增强

* 新增选项，仅向管理员和谷歌爬虫展示 XML 视频站点地图，其他访客将无法查看。这能有效防止<br />其他访客下载您的视频文件。

## 1.2.1

发布日期：2012-09-02

#### 错误修复

* 正确处理 index.php 的 URL。
* 始终为 Google ping 发送正确的视频站点地图 URL。
* 正确清理视频描述和标签，以便在 XML 站点地图中显示。

#### 功能增强

* 新增对 Smart Youtube Pro 的支持。
* 新增对 Viddler iframe 嵌入代码的支持。
* 新增对 youtu.be oEmbed 的支持。
* 初步支持 Brightcove。

## 1.2

发布日期：2012-09-01

* 元数据框中的视频标签页现已可用，因此您可以更改预览图像。
* 插件现已完全支持 videoObject 架构。
* 修复了若干视频识别问题，特别是针对 YouTube iframe 嵌入代码，如果您使用了这些代码，请务必在视频 SEO 页面点击重新索引。

## 1.1

发布日期：2012-09-01

* 此版本在激活方面应表现更佳。
* 插件设置现已移至其专属的 SEO -> Video SEO 管理页面，不再位于 XML 站点地图页面内。
* 插件现在能识别使用 object 标签或 iframe 嵌入的 YouTube 和 Vimeo 视频，如需使用此功能，只需点击“重新索引视频”。
* 改进了摘要预览的日期显示。
* 修复了一些提示信息。

## 1.0

发布日期：2012-04-16

* 初始版本