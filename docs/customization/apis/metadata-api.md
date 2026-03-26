---
title: "Metadata Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Apis
    - Customization
    - Repos
---


对于简单的网站，或者当 SEO 不是首要考虑因素时，插件和主题通常直接将元标签输出或回显到文档的 `<head>` 部分。

然而，更高级的 SEO 通常要求元标签具备*上下文*，能够感知页面上的*其他*元标签，或涉及复杂的内部逻辑。例如，[规范 URL 标签](/features/seo-tags/canonical-urls)的值可能会受到[元机器人标签](/features/seo-tags/meta-robots/overview)的存在或值的影响。对于运行复杂或多个插件和主题的网站，标准化和安全性也同样重要。在没有框架的情况下，管理这类挑战会变得越来越繁琐。

这就是为什么我们提供了一种正式的方式来管理每个页面 `<head>` 中元标签的构建和输出。我们为每个标签使用一个 `presenter`（`Abstract_Indexable_Presenter` 类的扩展），您可以轻松修改或扩展它。

## 可用呈现器
默认情况下，Yoast SEO 附带以下用于输出元标签的呈现器。

### 通用呈现器
| 呈现器 | 标签格式 | 过滤器 |
|---|-----|----|
| `Title_Presenter` | `<title>%s</title>` | `wpseo_title` |
| `Meta_Description_Presenter` | `<meta name="description" content="%s" />` | `wpseo_metadesc` |
| `Canonical_Presenter` | `<link rel="canonical" href="%s" />` | `wpseo_canonical` |
| `Robots_Presenter` | `<meta name="robots" content="%s" />` | `wpseo_robots` |
| `Meta_Author_Presenter` | `<meta name="author" content="%s" />` | `wpseo_meta_author` |

### Webmaster presenters
| Presenter | Tag format | Filter |
|---|-----|----|
| `Baidu_Presenter` | `<meta name="baidu-site-verification" content="%s" />` | n/a |
| `Bing_Presenter` | `<meta name="msvalidate.01" content="%s" />` | n/a |
| `Google_Presenter` | `<meta name="google-site-verification" content="%s" />` | n/a |
| `Pinterest_Presenter` | `<meta name="p:domain_verify" content="%s" />` | n/a |
| `Yandex_Presenter` | `<meta name="yandex-verification" content="%s" />` | n/a |

### X presenters
| Presenter | Tag format | Filter |
|---|-----|----|
| `Creator_Presenter` | `<meta name="twitter:creator" content="%s" />` | n/a |
| `Description_Presenter` | `<meta name="twitter:description" content="%s" />` | `wpseo_twitter_description` |
| `Image_Presenter` | `<meta name="twitter:image" content="%s" />` | `wpseo_twitter_image` |
| `Site_Presenter` | `<meta name="twitter:site" content="%s" />` | `wpseo_twitter_site` |
| `Title_Presenter` | `<meta name="twitter:title" content="%s" />` | `wpseo_twitter_title` |

### OpenGraph presenters
| Presenter | Tag format | Filter |
|---|-----|----|
| `Article_Author_Presenter` | `<meta property="article:author" content="%s" />` | `wpseo_opengraph_author_facebook` |
| `Article_Modified_Time_Presenter` | `<meta property="article:modified_time" content="%s" />` | n/a |
| `Article_Published_Time_Presenter` | `<meta property="article:published_time" content="%s" />` | n/a |
| `Article_Publisher_Presenter` | `<meta property="article:publisher" content="%s" />` | `wpseo_og_article_publisher` |
| `Description_Presenter` | `<meta property="og:description" content="%s" />` | `wpseo_opengraph_desc` |
| `Locale_Presenter` | `<meta property="og:locale" content="%s" />` | `wpseo_og_locale` |
| `Site_Name_Presenter` | `<meta property="og:site_name" content="%s" />` | `wpseo_opengraph_site_name` |
| `Title_Presenter` | `<meta property="og:title" content="%s" />` | `wpseo_opengraph_title` |
| `Type_Presenter` | `<meta property="og:type" content="%s" />` | `wpseo_opengraph_type` |
| `Url_Presenter` | `<meta property="og:url" content="%s" />` | `wpseo_opengraph_url` |
| `Image_Presenter` | `<meta property="og:image" content="%s" />` | `wpseo_opengraph_image` |

### Deprecated Presenters
| Presenter | Tag format | Filter | Deprecated from |
|---|-----|---|--|
| `Googlebot_Presenter` | `<meta name="googlebot" content="%s" />` | `wpseo_googlebot` | |
| `Bingbot_Presenter` | `<meta name="bingbot" content="%s" />` | `wpseo_bingbot` | |
| `FB_App_ID_Presenter` | `<meta property="fb:app_id" content="%s" />` | Yoast SEO 15.5 (Dec 2020) |

## 编辑现有元标签
有时您可能会遇到需要编辑 Yoast SEO 输出的某个元标签的情况。
为此，您应使用相应元数据展示器的相关过滤器来实现。

### 关于使用呈现器和过滤器的注意事项
- 所有过滤器都期望返回一个 _字符串_
- 某些呈现器没有关联的过滤器；通常在这些情况下，以编程方式修改相关文章/页面的值更有意义（例如使用 `Article_Published_Time_Presenter`）

#### 遗留过滤器

**请注意** 这些过滤器未来可能会变更，因为它们尚未通过 Presenter 类进行封装。

| 过滤器 | 描述 |
|----|----|
|`wpseo_opengraph_image_size` | 用于操作 Open Graph 分享所用图像尺寸的过滤器。<br />- 仅当您需要手动确定 `og:image` 标签的自定义图像尺寸时使用此过滤器。<br /> 尺寸可以是字符串值（例如 `thumbnail`）或宽度/高度值数组<br />（例如 `[ 200, 100 ]`）。<br />- 如果图像小于过滤器指定的尺寸，仍将被拒绝使用。<br />- 使用后，保存新特色图像或更新带有特色图像的文章时将始终强制执行此尺寸。<br />- 若要将此尺寸应用于使用过滤器前创建的文章，请重新索引站点。<br />- 可通过命令执行：`wp yoast index --reindex`。<a href="/features/wp-cli/reindex-indexables/">参见 Yoast SEO WP CLI：重新索引</a>。或安装<a href="https://yoast.com/help/how-to-reset-yoast-indexables/">Yoast Test Helper</a>来重置索引数据。<br />- 若站点从未建立索引，只需前往 `Yoast SEO`->`工具` 并点击 `开始 SEO 数据优化`。 |

### 示例

以下将展示我们提供的一些过滤器使用示例。

#### 操作元机器人标签输出

```php
/**
 * 将 max-video-preview 修改为 100。
 *
 * @param string $output       原始输出字符串的字符串表示。
 * @param object $presentation 包含必要数据的呈现对象。
 *
 * @return string Googlebot 元标签。
 */
function change_robots_video_preview_settings( $output, $presentation ) {
    $robots = $presentation->robots;

    $values = \array_map( function( $item ) {
        if ( strpos( $item, 'max-video-preview' ) !== false ) {
            $item = 'max-video-preview:100';
        }

        return $item;
    }, $robots );

    return \implode( ', ', $values );
}

add_filter( 'wpseo_robots', 'change_robots_video_preview_settings', 10, 2 );
```

#### Appending a string to a category's title

```php
/**
 * Changes title for a specific category.
 *
 * @param string $title        The current title.
 * @param object $presentation The presentation object containing the necessary data.
 *
 * @return string The altered title tag.
 */
function change_category_title( $title, $presentation ) {
	$categories = \get_the_category( $presentation->model->object_id );

	foreach ( $categories as $category ) {
		if ( $category->slug === 'books' ) {
			return sprintf( '%s - %s', $title, $category->name );
		}

		return $title;
	}
}

add_filter( 'wpseo_title', 'change_category_title', 10, 2 );
```

## Adding meta tags
Adding your own meta tag(s) is as simple as creating your own class which extends `Abstract_Indexable_Presenter`. To get started, you'll need to understand the following:

* Each implementation of `Abstract_Indexable_Presenter` must implement the two functions: `get` and `present`.
  * The `get` function should return the raw value of your meta (usually whatever is in the `content` property of the meta tag).
  * The `present` function should return the full meta tag.

* Every `Abstract_Indexable_Presenter` class has 3 public properties, `helpers`, `replace_vars` and the `presentation`. You can use these to gather all data you need:
  * The `helpers` property currently includes all our helper classes which you can use to, for example, determine whether or not something is an article post type in regards to schema, using `$this->helpers->schema->article->is_article_post_type( $post_type )`.
  * The `replace_vars` property ensures you can use the `$this->replace_vars( $string )` function to make use of our replacement variables in your own string if so desired.
  * The `presentation` contains all data that Yoast SEO puts out. Your IDE should make it easy for you to find the values you need.

Combined, this should lead to code that looks something like this:

```php
use Yoast\WP\SEO\Presenters\Abstract_Indexable_Presenter;

/**
 * Adds a custom my_meta_tag tag.
 */
class My_Custom_Presenter extends Abstract_Indexable_Presenter {
    /**
     * This output the full meta tag you want to add.
     */
    public function present() {
        return '<meta name="my_meta_tag" content="' . esc_attr( $this->get() ) . '" />';
    }

    /**
     * Returns the value of our new tag.
     *
     * @return string The value of our meta tag.
     */
    public function get() {
        return ( $this->presentation->open_graph_locale === 'nl_NL' ) ? 'Dutch' : 'Not dutch';
    }
}
```

For simple tags, you can use `Abstract_Indexable_Tag_Presenter`, which simply defines the string format and variable placeholder for the tag. A simple example would look like this:

```php
use Yoast\WP\SEO\Presenters\Abstract_Indexable_Tag_Presenter;

/**
 * Adds a custom my_meta_tag tag.
 */
class My_Custom_Presenter extends Abstract_Indexable_Tag_Presenter {

	/**
	 * The tag format including placeholders.
	 *
	 * @var string
	 */
	protected $tag_format = '<meta name="my_meta_tag" content="%s" />';

	/**
	 * Returns the value of our new tag.
	 *
	 * @return string The value of our meta tag.
	 */
	public function get() {
		return ( $this->presentation->open_graph_locale === 'nl_NL' ) ? 'Dutch' : 'Not dutch';
	}
}
```

这种更简单的格式要求您指定标签格式，并将值应出现的位置替换为 `%s`，因为它将被 *sprintf* 到该位置。这假设您的 `get` 函数返回一个需要属性转义的值。如果它实际上是一个 URL，您可以通过将展示器的 `$escaping` 类属性设置为 'url' 来更改转义逻辑。

由于 `Abstract_Indexable_Tag_Presenter` 本身继承自 `Abstract_Indexable_Presenter`，您在那里拥有的所有工具在这里也同样可用。

## 注册你的呈现器
无论你选择上述哪种方法，都可以通过过滤器注册你的呈现器：

```php
/**
 * 将我们的自定义呈现器添加到呈现器数组中。
 *
 * @param array $presenters 当前的呈现器数组。
 *
 * @return array 添加了自定义呈现器后的呈现器数组。
 */
function add_my_custom_presenter( $presenters ) {
	$presenters[] = new My_Custom_Presenter();

	return $presenters;
}

add_filter( 'wpseo_frontend_presenters', 'add_my_custom_presenter' );
```

## 移除元标签
可以通过使用 `wpseo_frontend_presenters` 过滤器来移除元标签。在下面的示例中，我们将从列表中移除规范 URL 展示器。

```php
/**
 * 从展示器中移除规范 URL。
 *
 * @param array $presenters 已注册的展示器。
 *
 * @return array 剩余的展示器。
 */
function remove_canonical_presenter( $presenters ) {
    return array_map( function( $presenter ) {
        if ( ! $presenter instanceof Canonical_Presenter ) {
            return $presenter;
        }
    }, $presenters );
}

add_action( 'wpseo_frontend_presenters', 'remove_canonical_presenter' );
```