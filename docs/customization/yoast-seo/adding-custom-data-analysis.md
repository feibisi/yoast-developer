---
title: "Adding Custom Data Analysis"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo
    - Customization
    - Repos
---


Yoast SEO 配备了一套强大的工具，不仅能帮助您提升 SEO，还能通过分析您正在处理的文章或页面内容来帮助您写出更好的文本。

在某些情况下，插件开发者可能希望向分析中添加一些额外数据，他们认为这些数据对其整体可读性和 SEO 评分有额外的重要性。Yoast SEO 允许这种分析的扩展。

这对于那些以我们的插件可能无法原生检测的方式（例如通过自定义数据结构、函数和短代码）添加页面内容的插件也很有用，他们希望我们将这些内容纳入分析。

**注意：** 在本示例中，我们将使用 ES6 语法，因此如果您尚未使用 ES6 或不使用像 [Babel](https://babeljs.io/) 或 [Webpack](https://webpack.js.org/) 这样的编译器，请确保调整代码以适应您自己的语法。

## 基础概念

在将自定义数据添加到分析之前，需要确保 Yoast SEO 能够识别这些数据的存在。实现这一目标的方法是编写自定义 JavaScript 插件。以下步骤将引导您了解创建此类插件的基本原理。

### Creating the plugin

First, create a file in your own plugin's `js/` directory (i.e. `js/MyCustomDataPlugin.js`).

In this file, you'll have to ensure that, at a minimum, the following is present:

```js
/* global YoastSEO */

class MyCustomDataPlugin {
    constructor() {
       // Ensure YoastSEO.js is present and can access the necessary features.
       if ( typeof YoastSEO === "undefined" || typeof YoastSEO.analysis === "undefined" || typeof YoastSEO.analysis.worker === "undefined" ) {
            return;
        }

        YoastSEO.app.registerPlugin( "MyCustomDataPlugin", { status: "ready" } );
        
        this.registerModifications();
    }

    /**
	 * Registers the addContent modification.
	 *
	 * @returns {void}
	 */
	registerModifications() {
		const callback = this.addContent.bind( this );

        // Ensure that the additional data is being seen as a modification to the content.
		YoastSEO.app.registerModification( "content", callback, "MyCustomDataPlugin", 10 );
	}

	/**
	 * Adds to the content to be analyzed by the analyzer.
	 *
	 * @param {string} data The current data string.
	 *
	 * @returns {string} The data string parameter with the added content.
	 */
	addContent( data ) {
        data += "Hello, I'm some additional data!";

		return data ;
	}
}

/**
 * Adds eventlistener to load the plugin.
 */
if ( typeof YoastSEO !== "undefined" && typeof YoastSEO.app !== "undefined" ) {
  new MyCustomDataPlugin();
} else {
  jQuery( window ).on(
    "YoastSEO:ready",
    function() {
      new MyCustomDataPlugin();
    }
  );
}
```

The above code adds "Hello, I'm some additional data!" to the text analysis, but you're not limited to just sending hard-coded strings to your custom plugin. For example, you could add a custom input field and read its contents.

**Note:** The last part in the above JavaScript is used to properly load the JavaScript class _after_ YoastSEO.js is done initializing. Please make sure you don't forget to add this.

### Registering the plugin with WordPress

The next step to make things work, is to make sure your JavaScript code is properly loaded. To achieve this, we'll have to ensure our plugin is properly registered and loaded by WordPress.
The main importance is that the above JavaScript file is loaded at the right time. Therefor, it's necessary to implement a chain of hooks that'll run once WordPress loads your plugin.

Assuming you have a plugin file present (i.e. `MyCustomPlugin.php`), your file might look a little something like this:

```php
<?php
// ...
class MyCustomPlugin {

    /**
     * MyCustomPlugin constructor.
     */
    public function __construct() {
        // ...
        add_action( 'admin_enqueue_scripts', [ $this, 'enqueue_scripts' ] );
    }
    
    /** 
     * Enqueues the plugin file.
     */
    public function enqueue_scripts() {
        wp_enqueue_script( 'my-custom-plugin', plugins_url( 'js/MyCustomDataPlugin.js', __FILE__ ), [], '1.', true );
    }

}

/** 
 * Loads the plugin.
 */
function loadMyCustomPlugin() {
    new MyCustomPlugin();
}

if ( ! wp_installing() ) {
	add_action( 'plugins_loaded', 'loadMyCustomPlugin', 20 );
}
```

### 测试代码

最后一步是测试代码。请确保您的插件已正确加载并创建一篇新文章。
作为测试，您可以将关键词设置为“additional”，并查看 SEO 分析是否在“引言中的关键短语”结果中正确检测到它。