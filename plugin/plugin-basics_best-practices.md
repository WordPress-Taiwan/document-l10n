``` Block:InfoCallout
本文譯自〈[Best Practices](https://developer.wordpress.org/plugins/plugin-basics/best-practices/)〉
```

以下是一些幫助整理程式碼的最佳實踐，使其能夠與 WordPress 核心和其他 WordPress 外掛程式一起良好的執行。

避免命名衝突
----

當你的外掛程式對變數、函式或類別使用與另一個外掛程式相同的名稱時，就會發生命名衝突。

幸運的，你可以使用以下方法來避免命名衝突。

### Procedural Coding Method

預設情況下，所有變數、函式和類別都定義在全域命名空間中，也就是說你的外掛可以覆蓋另一個外掛設定的變數、函式和類別，反之亦然。在函式或類別內部定義的變數不受此影響。

#### 所有東西加上前綴

所有全域可存取的程式碼都應該以唯一識別碼作為前綴。前綴可以防止與其他外掛發生衝突，並防止它們覆蓋你的變數或者意外呼叫你的函式和類別。

為了防止與其他外掛衝突，你的前綴長度應至少為 3 個字母，但我們建議使用 5 個字母。因為在 WordPress.org 上我們就託管了數以萬計的外掛程式。我們的伺服器之外還有數十萬個。你會很容易遇到衝突。

一個使用前綴的好方法是，假如你的外掛程式名為「Easy Custom Post Types」，那麼你可以使用以下名稱：

* function ecpt_save_post()
* define( ‘ECPT_LICENSE’, true );
* class ECPT_Admin{}
* namespace EasyCustomPostTypes;
* update_option( 'ecpt_settings', $settings );

``` Block:InfoCallout
由於你正在將程式碼作為 WordPress 專案的一部分進行編寫，因此必須避免使用可能會和核心 WordPress 發生衝突的前綴。這包括但不限於：__（雙底線）、wp_、WordPress 或 _（單底線）

如果你正在為「子」外掛程式（例如 WooCommece 擴充功能）編寫程式碼，你同樣需要避免使用它們的任何的常見前綴（例如： Woo、WooCommerce）。

你可以在類別或命名空間中使用它們，但不能作為獨立的函式/命名空間/類別。
```

如果你使用 _n() 或 __() 進行翻譯，那是沒問題的。我們**只**討論你為外掛程式創建的功能，而不是 WordPress 的核心功能。事實上，這些核心功能就是你不需要在自己的外掛程式中使用這些前綴的原因！你不想為你的使用者破壞 WordPress。

請記住：好的前綴名稱對於你的外掛來說，必須是唯一且獨特的。以下將幫助和下一個人進行測試，並防止衝突。

**必須**加前綴的程式碼包括：

* 函式（除非命名空間）
* 類別、介面和特徵（除非命名空間）
* 命名空間
* 全域變數
* 選項和暫存

#### 檢查存在的設計

PHP 提供了許多函式來驗證變數、函式、類別和常數是否存在。如果存在，所有這些都將傳回 true。

* 變數：[isset()](http://php.net/manual/en/function.isset.php)（包括陣列、物件等）
* 函式：[function_exists()](http://php.net/manual/en/function.function-exists.php)
* 類別：[class_exists()](http://php.net/manual/en/function.class-exists.php)
* 常數：[defined()](http://php.net/manual/en/function.defined.php)

請記住，在所有函式和類別使用 `using(!function_exists('NAME ')) {` ，讓你即時的找到致命的缺陷。如果其他東西有一個同名的函式並且它們的程式碼首先加載，你的外掛就會崩潰。使用 if-exists 取代/覆蓋函式或類別應僅保留用於共享庫。

```PHP
// 如果函示 "wporg_init" 不存在，就建立
if ( ! function_exists( 'wporg_init' ) ) {
    function wporg_init() {
        register_setting( 'wporg_settings', 'wporg_option_foo' );
    }
}

// 如果函示 "wporg_get_foo"" 不存在，就建立
if ( ! function_exists( 'wporg_get_foo' ) ) {
    function wporg_get_foo() {
        return get_option( 'wporg_option_foo' );
    }
}
```

### 物件導向程式設計

解決命名衝突問題的一種更簡單的方法是使用外掛程式碼的類別。

你仍然需要檢查你想要的類別的名稱是否已被使用，但其餘的名稱將由 PHP 處理。

``` PHP
if ( ! class_exists( 'WPOrg_Plugin' ) ) {
    class WPOrg_Plugin {
        public static function init() {
            register_setting( 'wporg_settings', 'wporg_option_foo' );
        }

        public static function get_foo() {
            return get_option( 'wporg_option_foo' );
        }
    }

    WPOrg_Plugin::init();
    WPOrg_Plugin::get_foo();
}
```
#### 文件管理

外掛目錄的根級別，應該要包含你的 `plugin-name.php` 檔案和（可選）你的 [uninstall.php](https://developer.wordpress.org/plugin/the-basics/uninstall-methods/) 檔案。所有其他文件應盡可能組織到子資料夾中。

##### 資料夾結構

清晰的資料夾結構，可以幫助你和其他開發外掛程式的人將相似的文件保存在一起。

以下是供參考的範例資料夾結構：

```
/plugin-name
     plugin-name.php
     uninstall.php
     /languages
     /includes
     /admin
          /js
          /css
          /images
     /public
          /js
          /css
          /images
```

外掛架構
你為外掛選擇的架構或程式碼組織可能取決於外掛的大小。

對於與 WordPress 核心、主題或其他外掛互動有限的小型單一用途外掛，設計複雜的類別幾乎沒有好處；除非你知道這個外掛以後會大大擴充。

對於包含大量程式碼的大型外掛，請從類別開始。分開 style  和 scripts ，建置相關的文件。這將有助於程式碼整理和外掛的長期維護。

將和管理有關的程式碼邏輯與公開的程式碼邏輯分開，很有幫助。使用條件 [is_admin()](https://codex.wordpress.org/Function_Reference/is_admin)。你仍然必須執行功能檢查，因為這並未表示使用者已通過身份驗證或具有管理員等級存取權限。請參閱[檢查用戶能力](https://developer.wordpress.org/plugins/security/checking-user-capabilities/)。

例如：

```PHP
if ( is_admin() ) {
    // 我們在管理模式
    require_once __DIR__ . '/admin/plugin-name-admin.php';
}
```

### 避免直接存取文件

作為安全預防措施，如果未定義 `ABSPATH` 全局，最好禁止存取。這僅適用於包含類別或函式定義之外的程式碼的文件，例如主外掛檔案。

你可以透過在文件最上方，加入以下程式碼來實現此目的：

``` PHP 
if ( ! defined( 'ABSPATH' ) ) {
	exit; // 如果直接存取則退出
}
```

### 架構模式

雖然有許多可能的架構模式，但它們大致可以分為三種類型：

* [單一外掛文件，包含函示](https://github.com/GaryJones/move-floating-social-bar-in-genesis/blob/master/move-floating-social-bar-in-genesis.php)
* [單一外掛文件，包含類別、實例化物件和可選函示](https://github.com/norcross/wp-comment-notes/blob/master/wp-comment-notes.php)
* [主外掛文件，然後是一個或多個類別文件](https://github.com/tommcfarlin/WordPress-Plugin-Boilerplate)

#### 架構模式解釋

上述更複雜的程式碼組織的具體實作已經寫成教學和幻燈片：

* [Slash – 單例、載入器、操作、畫面、處理程式](https://jjj.blog/2012/12/slash-architecture-my-approach-to-building-wordpress-plugins/)
* [在 WordPress 外掛程式中實作 MVC 模式](http://iandunn.name/wp-mvc)

#### 範本

你可能希望從 **boilerplate** 檔案開始，而不是從頭開始寫你的外掛。使用範本的優點之一是，在你自己的外掛之間保持一致性。如果你使用其他人已經熟悉的範本文件，範本文件還可以讓其他人更輕鬆地為你的程式碼做出貢獻。

這些也可以作為不同，但可比較的架構的進一步範例。

* [WordPress 外掛範本](https://github.com/tommcfarlin/WordPress-Plugin-Boilerplate)：WordPress 外掛程式開發的基礎，旨在為建立外掛程式提供清晰一致的指南。
* [WordPress 外掛程式引導程式](https://github.com/claudiosmweb/wordpress-plugin-boilerplate)：使用 Grunt、Compass、GIT 和 SVN 開發 WordPress 外掛程式的基本引導程式。
* [WP Skeleton Plugin](https://github.com/ptahdunbar/wp-skeleton-plugin)：專注於單元測試和使用 Composer 進行開發的外掛骨架。
* [WP CLI Scaffold](https://developer.wordpress.org/cli/commands/scaffold/plugin/)：WP CLI 的 Scaffold 指令會建立一個外掛骨架，其中包含 CI 設定檔等選項

當然，你可以利用這些，或是其他不同的方法來創建自己的自訂範本。