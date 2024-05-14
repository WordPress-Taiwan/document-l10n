本文譯自〈[What is a Plugin?](https://developer.wordpress.org/plugins/intro/what-is-a-plugin/)〉

外掛程式是擴展 WordPress 核心功能的程式碼包。 WordPress 外掛程式由 PHP 程式碼組成，可以包含其他資源，例如圖片、CSS 和 JavaScript。

透過製作自己的外掛，你可以擴充 WordPress，即在 WordPress 既有功能之上，建立額外的功能。例如，你可以開發一個外掛程式來顯示你網站上最近十篇文章的連結。

或者，使用 WordPress 的自訂文章類型。你可以開發一個外掛，建立一個功能齊全的客服任務系統 (support ticketing system)，其中包含電子信箱通知、自訂任務狀態和客戶的入口網站。有著無限的可能！

大多數 WordPress 外掛程式都由許多文件組成，但外掛實際上只需要一個主文件，並在標頭中包含 [DocBlock](http://en.wikipedia.org/wiki/PHPDoc#DocBlock)。

[Hello Dolly](https://wordpress.org/plugins/hello-dolly/) 是最早的外掛之一，長度只有 [100 行](https://plugins.trac.wordpress.org/browser/hello-dolly/trunk/hello.php)。 Hello Dolly 在 WordPress 管理員中顯示這首[著名歌曲](http://en.wikipedia.org/wiki/Hello,_Dolly!_(song))的歌詞。 PHP 檔案中使用了一些 CSS 來控制歌詞的樣式。

作為 WordPress.org 外掛程式作者，你有一個好的機會來建立一個將被數百萬 WordPress 使用者安裝、修改和喜愛的外掛程式。你所需要做的就是將你的好想法轉化為程式碼。外掛手冊可以幫助你解決這個問題。