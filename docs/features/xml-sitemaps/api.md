---
title: "Api"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Xml Sitemaps
    - Features
    - Repos
---


虽然 Yoast SEO 为 XML 站点地图提供了合理的默认行为（以及包含/排除的 UI 控件），但自定义主题或插件有时需要修改我们的标记或逻辑。  
在这些情况下，您可以使用以下示例来修改站点地图的生成和输出方式。

## 排除内容类型

### 排除特定文章
```php
/**
 * 从 XML 站点地图中排除文章。
 *
 * @return array 要排除的文章 ID。
 */
function exclude_posts_from_xml_sitemaps() {
    return [ 1, 2, 3 ];
}

add_filter( 'wpseo_exclude_from_sitemap_by_post_ids', 'exclude_posts_from_xml_sitemaps' );
```

### Exclude a post type
```php
/**
 * Exclude a post type from XML sitemaps.
 *
 * @param boolean $excluded  Whether the post type is excluded by default.
 * @param string  $post_type The post type to exclude.
 *
 * @return bool Whether a given post type should be excluded.
 */
function sitemap_exclude_post_type( $excluded, $post_type ) {
    return $post_type === 'recipes';
}

add_filter( 'wpseo_sitemap_exclude_post_type', 'sitemap_exclude_post_type', 10, 2 );
```

### Exclude a taxonomy
```php
/**
 * Exclude a taxonomy from XML sitemaps.
 *
 * @param boolean $excluded Whether the taxonomy is excluded by default.
 * @param string  $taxonomy The taxonomy to exclude.
 *
 * @return bool Whether a given taxonomy should be excluded.
 */
function sitemap_exclude_taxonomy( $excluded, $taxonomy ) {
    return $taxonomy === 'ingredients';
}

add_filter( 'wpseo_sitemap_exclude_taxonomy', 'sitemap_exclude_taxonomy', 10, 2 );
```

### Exclude an author
```php
/**
 * Excludes author with ID of 5 from author sitemaps.
 *
 * @param array $users Array of User objects to filter through.
 *
 * @return array The remaining authors.
 */
function sitemap_exclude_authors( $users ) {
   return array_filter( $users, function( $user ) {
        if ( $user->ID === 5 ) {
            return false;
        }

        return true;
    } );
}

add_filter( 'wpseo_sitemap_exclude_author', 'sitemap_exclude_authors' );
```

### Exclude a taxonomy term
```php
/**
 * Excludes terms with ID of 3 and 11 from terms sitemaps.
 *
 * @param array $terms Array of term IDs already excluded.
 *
 * @return array The terms to exclude.
 */
function sitemap_exclude_terms( $terms ) {
    return [ 3, 11 ];
}

add_filter( 'wpseo_exclude_from_sitemap_by_term_ids', 'sitemap_exclude_terms' );
```

## 添加内容

### 添加自定义文章类型
```php
/**
 * 为自定义文章类型添加 XML 站点地图。
 * 
 * @return void
 */
function enable_custom_sitemap() {
    global $wpseo_sitemaps;
    if ( isset( $wpseo_sitemaps ) && ! empty ( $wpseo_sitemaps ) ) {
        $wpseo_sitemaps->register_sitemap( 'recipe', 'create_recipe_sitemap' );
    }
}

add_action( 'init', 'enable_custom_sitemap' );
```

### 向 XML 站点地图索引添加额外/外部 XML 站点地图
```php
/**
 * 将额外的/自定义的 XML 站点地图字符串写入 XML 站点地图索引。
 *
 * @param string $sitemap_custom_items 描述一个或多个自定义站点地图的 XML。
 *
 * @return string 包含额外 XML 的站点地图索引。
 */
function add_sitemap_custom_items( $sitemap_custom_items ) {
    $sitemap_custom_items .= '
<sitemap>
<loc>https://www.example.com/external-sitemap-1.xml</loc>
<lastmod>2017-05-22T23:12:27+00:00</lastmod>
</sitemap>';
    return $sitemap_custom_items;
}

add_filter( 'wpseo_sitemap_index', 'add_sitemap_custom_items' );

```

### 为文章添加图片

某些主题或页面构建器模块可能不会在站点地图中显示图片。您可能需要通过过滤器 `wpseo_sitemap_urlimages` 来添加它们。此过滤器将注册图片以使其出现在站点地图上。

```php
/**
 * 一个为文章添加图片的示例。
 *
 * @param array $images 与文章相关的图片 URL 数组。
 * @param int   $post_id  文章 ID。
 * 
 * @return array 要添加的图片 URL 数组。
 */
function filter_wpseo_sitemap_urlimages( $images, $post_id ) {
  array_push( $images, [ 'src' => 'https://www.example.com/wp-content/uploads/extra-image.jpg' ]);
  return $images;
};
add_filter( 'wpseo_sitemap_urlimages', 'filter_wpseo_sitemap_urlimages' );
```

### 为分类项添加图片

某些主题、页面构建器模块或插件可能不会在站点地图中显示图片。您可能需要通过过滤器 `wpseo_sitemap_urlimages_term` 来添加它们。此过滤器将注册图片以显示在站点地图上。

```php
/**
 * 为分类项添加图片的示例。
 *
 * @param array $images 与分类项相关的图片 URL 数组。
 * @param int   $term_id  分类项 ID。
 * 
 * @return array 要添加的图片 URL 数组。
 */
function filter_wpseo_sitemap_urlimages_term( $images, $term_id ) {
  array_push( $images, [ 'src' => 'https://www.example.com/wp-content/uploads/extra-image.jpg' ]);
  return $images;
};
add_filter( 'wpseo_sitemap_urlimages_term', 'filter_wpseo_sitemap_urlimages_term' );
```

### 为首页添加图片

当首页不是静态页面，而是最新文章列表时，你可以通过过滤器 `wpseo_sitemap_urlimages_front_page` 向站点地图添加图片。该过滤器将注册图片，使其出现在站点地图中。

```php
/**
 * 一个向首页添加图片的示例。
 *
 * @param array $images 与首页相关的图片 URL 数组。
 * @return array 要添加的图片 URL 数组。
 */
function filter_wpseo_sitemap_urlimages_front_page( $images ) {
  array_push( $images, [ 'src' => 'https://www.example.com/wp-content/uploads/extra-image.jpg' ]);
  return $images;
};
add_filter( 'wpseo_sitemap_urlimages_front_page', 'filter_wpseo_sitemap_urlimages_front_page' );
```

## Miscellaneous

### Alter the URL of an entry
```php
/**
 * Alters the URL structure for an example custom post type.
 *
 * @param string  $url  The URL to modify.
 * @param WP_Post $post The post object.
 *
 * @return string The modified URL.
 */
function sitemap_post_url( $url, $post ) {
	if ( $post->post_type === 'guest_authors' ) {
		return \str_replace( 'guest-authors', 'guests', $url );
	}

	return $url;
}

add_filter( 'wpseo_xml_sitemap_post_url', 'sitemap_post_url', 10, 2 );
```

### 调整站点地图条目数量
```php
/**
 * 修改每个 XML 站点地图中的条目数量。
 *
 * @return integer 每个站点地图的最大条目数。
 */
 function max_entries_per_sitemap() {
    return 100;
}

add_filter( 'wpseo_sitemap_entries_per_page', 'max_entries_per_sitemap' );
```

### Add extra properties to the `<video:video>` container
```php
/**
 * Adds an example <video:live> property to a <video:video> container for a particular post
 *
 * @param string $property A placeholder for a custom property.
 * @param int    $post_id  The post ID.
 * 
 * @return string The property to add.
 */
 function add_video_live_property( $property = '', $post_id ) {

    // Bail if this isn't the example post we want to modify.
    if ( $post_id !== 12345 ) {
        return $property;
    }

    // Set our custom property.
    $property = '<video:live>yes</video:live>';

    return $property;
}

add_filter( 'wpseo_video_item', 'add_video_live_property', 10, 2 );
```

### 过滤 `urlset` 元素
例如，如果你想在 XML 站点地图中使用 xhtml:link 元素来添加 hreflang 标记，可以这样做：
```php
/**
 * 过滤所有站点地图的 `urlset`。
 *
 * @param string $urlset 站点地图 `urlset` 的输出。
 */
add_filter( 'wpseo_sitemap_urlset', function( $urlset ) { 
  return str_replace( '>', ' xmlns:xhtml="http://www.w3.org/1999/xhtml">', $urlset );
}, 1, 10 );
```

### Filter the sitemap URL
```php
/**
 * Allows filtering of the XSL URL used in the current environment.
 *
 * @param string $current_url The current XSL URL.
 */
add_filter( 'wpseo_sitemap_public_url', function ( $current_url ) {
  return 'https://another-url.com/main-sitemap.xml';
} );
```