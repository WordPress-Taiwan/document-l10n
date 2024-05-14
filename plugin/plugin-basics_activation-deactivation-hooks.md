``` Block:InfoCallout
本文譯自〈[Activation / Deactivation Hooks](https://developer.wordpress.org/plugins/plugin-basics/activation-deactivation-hooks/)〉
```

啟用 (activation) 和停用 (deactivation) 勾點，提供了在啟用或停用外掛程式時，執行操作的方法。

* 在啟用勾點，外掛程式可以執行新增重寫規則、新增自訂資料表或設定預設選項。
* 在停用勾點，外掛可以執行刪除臨時資料，例如快取和臨時檔案和目錄。

```Block:InfoCallout
停用勾點有時會與[移除勾點](https://developer.wordpress.org/plugins/plugin-basics/uninstall-methods/) (uninstall hook) 混淆。移除掛鉤適合永久刪除所有資料，例如刪除外掛選項或是自訂資料表......等。
```

啟用勾點
----

若要設定啟用勾點，請使用 [register_activation_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/) 函式：

``` PHP
register_activation_hook(
	__FILE__,
	'pluginprefix_function_to_run'
);
```

停用勾點
----

若要設定停用勾點，請使用 [register_deactivation_hook()](https://developer.wordpress.org/reference/functions/register_deactivation_hook/) 函式：

```PHP
register_deactivation_hook(
	__FILE__,
	'pluginprefix_function_to_run'
);
```

函式中的第一個參數都是指你的外掛主要檔案，即你在其中放置[外掛標頭註解](https://developer.wordpress.org/plugins/the-basics/header-requirements/)的文件。通常這兩個函式將從外掛主要檔案中觸發。但是，如果函式放置在任何其他檔案中，則必須更新第一個參數以正確指向外掛的主要檔案。

範例
--

啟用勾點最常見的用途之一，是在外掛程式註冊自訂貼文類型時，更新 WordPress 的永久連結。這處理了令人討厭的 404 錯誤。

讓我們看如何執行這個操作的範例：

``` PHP
/**
 \* 註冊自定義文章類型 "book" 
 */
function pluginprefix_setup_post_type() {
	register_post_type( 'book', ['public' => true ] ); 
} 
add_action( 'init', 'pluginprefix_setup_post_type' );
```

``` PHP
/**
 \* 啟用勾點
 */
function pluginprefix_activate() { 
	// 觸發我們註冊自訂貼文類型外掛程式的函式。
	pluginprefix_setup_post_type(); 
	// 註冊貼文類型後清除永久連結。
	flush_rewrite_rules(); 
}
register_activation_hook( __FILE__, 'pluginprefix_activate' );
```

如果您不熟悉註冊自訂貼文類型，請不要擔心，這將在稍後介紹。使用這個例子，只是因為它很常見。

使用上面的範例，以下是如何反轉此過程，並停用外掛：

``` PHP
/**
 \* 停用勾點
 */
function pluginprefix_deactivate() {
	// 取消註冊文章類型，因此規則不再存在於記憶體中。
	unregister_post_type( 'book' );
	// 從資料庫中刪除我們的貼文類型的規則。
	flush_rewrite_rules();
}
register_deactivation_hook( __FILE__, 'pluginprefix_deactivate' );
```

有關啟用和停用勾點的更多信息，以下是一些優秀的資源：

* WordPress 函式參考中的 [register_activation_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/)
* WordPress 函式參考中的 [register_deactivation_hook()](https://developer.wordpress.org/reference/functions/register_deactivation_hook/)