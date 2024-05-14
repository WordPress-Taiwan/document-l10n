``` Block:InfoCallout
本文譯自〈[Header Requirements](https://developer.wordpress.org/plugins/plugin-basics/header-requirements/)〉
```

如[入門](https://tw.wordpress.org/team/?post_type=handbook&p=532#getting-started)一章所述，主 PHP 檔案應包含標頭註解，告訴 WordPress 這個檔案是一個外掛程式，並提供有關該外掛程式的資訊。

最小必須欄位
------

最低要求，標頭註解需要包含一個外掛名稱：

```PHP
/*
 \* Plugin Name: YOUR PLUGIN NAME
 */
```

標頭欄位
----

可用的標頭欄位：

* **Plugin Name:** (_必須_) 你的外掛程式的名稱，將顯示在 WordPress 管理員的外掛程式清單中。
* **Plugin URI:** 外掛的主頁，應該是唯一的 URL，最好在你自己的網站上。這對於你的外掛來說必須是唯一的。不可以在這使用 WordPress.org URL。
* **Description:** 外掛程式的簡短描述，如 WordPress 管理中的外掛程式部分所示。請將此描述控制在 140 個字元以內。
* **Version:** 外掛的目前版本號，例如 1.0 或 1.0.3。
* **Requires at least:** 該外掛程式可以運行的最低 WordPress 版本。
* **Requires PHP:** 所需最低 PHP 版本。
* **Author:** 外掛作者的姓名。可以使用逗號列出多位作者。
* **Author URI:** 作者的網站或其他網站（例如 WordPress.org）上的個人資料。
* **License:** 外掛授權的簡稱（slug）（例如 GPLv2）。有關授權的更多資訊，請參閱 [WordPress.org 指南](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#1-plugins-must-be-compatible-with-the-gnu-general-public-license)。
* **License URI:** 授權全文的連結 (例如 [https://www.gnu.org/licenses/gpl-2.0.html](https://www.gnu.org/licenses/gpl-2.0.html)).
* **Text Domain:** 外掛的 [gettext](https://www.gnu.org/software/gettext/) 文字域。更多資訊可以在[如何國際化外掛頁面](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/)的[文字域](http://Text%20Domain)部分找到。
* **Domain Path:** 網域路徑讓 WordPress 知道在哪裡可以找到翻譯。更多資訊可以在[如何國際化外掛](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/)頁面的[網域路徑](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/#domain-path)部分找到。
* **Network: **外掛是否只能在網路範圍內啟動。只能設為 true，不需要時應省略。
* **Update URI:** 允許第三方外掛程式避免意外被 WordPress.org 外掛目錄中同名外掛的更新覆蓋。如需了解更多信息，請閱讀相關的[開發說明](https://make.wordpress.org/core/2021/06/29/introducing-update-uri-plugin-header-in-wordpress-5-8/)。
* **Requires Plugins**: 以逗號分隔的 WordPress.org 格式的相依項列表，例如 my-plugin（不支援 my-plugin/my-plugin.php）。它不支援外掛中的逗號。有關更多信息，請閱讀相關的[開發說明](https://make.wordpress.org/core/2024/03/05/introducing-plugin-dependencies-in-wordpress-6-5/)。

一個合法的標題註解 PHP 檔案可能如下所示：

``` PHP
/\* \* Plugin Name:       My Basics Plugin * Plugin URI:        https://example.com/plugins/the-basics/ * Description:       Handle the basics with this plugin. * Version:           1.10.3 * Requires at least: 5.2 * Requires PHP:      7.2 * Author:            John Smith * Author URI:        https://author.example.com/ * License:           GPL v2 or later * License URI:       https://www.gnu.org/licenses/gpl-2.0.html * Update URI:        https://example.com/my-plugin/ * Text Domain:       my-basics-plugin * Domain Path:       /languages * Requires Plugins:  my-plugin, yet-another-plugin */
```

這是另一個使用檔案階層 PHPDoc DocBlock 以及 WordPress 外掛程式標頭的範例：

``` PHP
/\*\* \* Plugin Name * * @package           PluginPackage * @author            Your Name * @copyright         2019 Your Name or Company Name * @license           GPL-2.0-or-later * * @wordpress-plugin * Plugin Name:       Plugin Name * Plugin URI:        https://example.com/plugin-name * Description:       Description of the plugin. * Version:           1.0.0 * Requires at least: 5.2 * Requires PHP:      7.2 * Author:            Your Name * Author URI:        https://example.com * Text Domain:       plugin-slug * License:           GPL v2 or later * License URI:       http://www.gnu.org/licenses/gpl-2.0.txt * Update URI:        https://example.com/my-plugin/ * Requires Plugins:  my-plugin, yet-another-plugin */
```

``` Block:InfoCallout
筆記
--

專案指派版本號碼的時候，請記住 WordPress 使用 PHP 的 version_compare() 函式來比較外掛程式版本號。因此，在發布新版本的外掛程式之前，你應該確保該 PHP 函式認為新版本比舊版本「更大」。例如：1.02 實際上大於 1.1 。
```