本文譯自〈[Plugin Basics](https://developer.wordpress.org/plugins/plugin-basics/)〉

入門
--

簡單來說，WordPress 外掛是一個帶有 WordPress 外掛標頭註解的 PHP 檔案。強烈建議你建立一個目錄來保存外掛，以便所有外掛的檔案都整齊地放在同一個地方。

若要開始建立新外掛，請按照下列步驟操作：

1.  至 WordPress 的 **wp-content** 目錄。
2.  開啟 **plugins** 目錄。
3.  建立一個新目錄並以外掛命名（例如：外掛名稱）。
4.  開啟新外掛的目錄。
5.  建立一個新的 PHP 檔案（最好以你的外掛命名該文件，例如：plugin-name.php）。

該過程在 Unix 命令列上，如下所示：

    wordpress $ cd wp-content
    wp-content $ cd plugins
    plugins $ mkdir plugin-name
    plugins $ cd plugin-name
    plugin-name $ vi plugin-name.php

在上面的範例中，vi 是文字編輯器的名稱。使用你覺得適合的編輯器。

現在你正在編輯新外掛的 PHP 文件，你需要添加外掛標題註解。這是一個特殊格式的 PHP 區塊註解，其中包含有關外掛的後設資料，例如名稱、作者、版本、許可證等。外掛標頭註解必須符合[標頭要求](https://tw.wordpress.org/team/?post_type=handbook&p=539)，並且至少包含外掛的名稱。

外掛程式資料夾中只有一個文件應具有標題註解，如果外掛程式有多個 PHP 文件，則最多只能有其中一個文件，具有標題註解。

儲存檔案後，你應該能夠在 WordPress 網站中看到列出的外掛程式。登入你的 WordPress 網站，然後點擊 WordPress 管理員左側導覽窗格中的**外掛程式 (Plugins)**。此頁面顯示你的 WordPress 網站擁有的所有外掛程式。你的新外掛現在應該在這個列表中！

勾點：動作 (actions) 和篩選器 (filters)
------------------------------

WordPress 勾點可讓你在特定點存取 WordPress 來更改 WordPress 的行為方式，而無需編輯任何核心檔案。

WordPress 中有兩種類型的勾點：動作勾點 (actions) 和篩選器勾點 (filters)。動作勾點可讓你新增或變更 WordPress 功能，而篩選器勾點可以讓你在載入並向網站使用者顯示內容時變更內容。

勾點不僅適用於外掛程式開發者。 WordPress 核心本身也廣泛使用勾點來提供預設功能。其他勾點是未使用的預留位置，當你需要更改 WordPress 的工作方式時，可以輕鬆使用它們。這就是 WordPress 如此靈活的原因。

### 基本勾點

建立外掛時會有 3 個基本勾點，分別是 [register\_activation\_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/)、[register\_deactivation\_hook()](https://developer.wordpress.org/reference/functions/register_deactivation_hook/) 和 [register\_uninstall\_hook()](https://developer.wordpress.org/reference/functions/register_uninstall_hook/)。

當你啟動外掛程式時，會執行[啟動勾點](https://developer.wordpress.org/plugins/the-basics/activation-deactivation-hooks/) (activation hook)。 它可以提供設定外掛的功能，例如：在資料表 options 建立一些預設設定。

當你停用外掛時，[停用勾點](https://developer.wordpress.org/plugins/the-basics/activation-deactivation-hooks/) (deactivation hook) 就會運行。 你可以撰寫一個函式，來清除外掛儲存的暫時資料。

這些[移除方法](https://developer.wordpress.org/plugins/the-basics/uninstall-methods/)可以用於使用 WordPress 管理員刪除外掛程式後進行清理。 你可以使用它來刪除外掛建立的所有數據，例如添加到資料表 options 中的任何選項。

### 增加勾點

你可以使用 [do_action()](https://developer.wordpress.org/reference/functions/do_action/) 增加自己的自訂勾點，使其他開發者可以使用勾點傳遞函式，來擴展你的外掛程式。

### 移除勾點

你也可以使用呼叫 [remove_action()](https://developer.wordpress.org/reference/functions/remove_action/) 來刪除先前定義的函式。 例如，如果你的外掛是另一個外掛的附加元件，你可以使用 [remove_action()](https://developer.wordpress.org/reference/functions/remove_action/) 函式，並將和另一個外掛定義過的函式，通過 [add_action()](https://developer.wordpress.org/reference/functions/add_action/) 添加後回調。 在這些情況下，操作的優先權會相當重要，因為 [remove_action()](https://developer.wordpress.org/reference/functions/remove_action/) 需要在初始 [add_action()](https://developer.wordpress.org/reference/functions/add_action/) 之後執行。

你必須謹慎的從勾點中移除動作，以及更改優先順序，因為很難看出這些更改，會如何影響與相同勾點的其他功能。 我們強烈建議經常進行測試。

你可以在本手冊的[勾點](https://developer.wordpress.org/plugin/hooks/)章節中，了解有關創建勾點以及與其互動的更多資訊。

WordPress APIs
--------------

你是否知道 WordPress 提供了許多[應用程式介面 API](https://make.wordpress.org/core/handbook/core-apis/) ？這些 API 可以減少你需要在外掛中撰寫的程式碼。 你不會想重新發明輪子，尤其是當這麼多人為你完成了大量工作和測試。

最常見的是 [Options API](https://codex.wordpress.org/Options_API)，它可以將外掛的資料輕鬆地存在資料庫中。 如果你考慮在外掛程式中使用 [cURL](https://en.wikipedia.org/wiki/CURL)，你可能會對 [HTTP API](https://codex.wordpress.org/HTTP_API) 感興趣。

由於我們談論的是外掛，因此你需要研究 [Plugin API](https://codex.wordpress.org/Plugin_API)。 它具有多種功能可以幫助你開發外掛。

WordPress 如何讀取外掛
----------------

當 WordPress 在 WordPress 管理員的外掛頁面上載入已安裝外掛程式的清單時，它會搜尋 plugins 資料夾（及其子資料夾）以尋找帶有 WordPress 外掛程式標頭註解的 PHP 檔案。 如果你的整個外掛程式僅包含一個 PHP 檔案（例如 [Hello Dolly](https://wordpress.org/plugins/hello-dolly/)），則該檔案可以直接位於 plugins 資料夾的根目錄內。 但更常見的是，外掛檔案放在自己的資料夾中，以外掛命名。

分享你的外掛
------

有時你建立的外掛程式僅適用於你的網站。 但許多人喜歡與 WordPress 社群的其他成員分享他們的外掛。 在共享你的外掛程式之前，你需要做的一件事是選擇[授權](https://opensource.org/licenses/category) (lincense)。 這可以讓你的外掛的使用者知道他們如何被允許使用你的程式碼。 為了保持與 WordPress 核心的相容性，建議你選擇適用於 GNU 通用公共授權 (GPLv2+) 的授權。