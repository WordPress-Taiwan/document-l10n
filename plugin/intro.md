``` Block:InfoCallout
本文譯自〈[Introduction to Plugin Development](https://developer.wordpress.org/plugins/intro/)〉
``` 


外掛開發簡介
======

歡迎來到外掛開發者手冊。無論你正在開發第一個外掛還是第五十個外掛，我們希望此資源可以幫助你寫出最好的外掛。

外掛開發人員手冊涵蓋了各種主題，從外掛程式標頭中應包含的內容到安全最佳實踐，再到可用於建立外掛程式的工具。我們一直持續更新這份文件，如果你發現某些內容缺失或不完整，請通知 Slack 文件團隊，我們將共同改進。

為什麼要建立外掛
--------

WordPress 開發中有一條基本規則，那就是：不要碰 WordPress 核心 (Don’t touch WordPress core)。這意味著你不需要編輯核心 WordPress 檔案來為您的網站增加功能。這是因為 WordPress 每次更新都會覆蓋核心檔案。你想要新增或修改的任何功能，都應該用外掛程式。

WordPress 外掛可以根據您的需求簡單或複雜，具體取決於您想要做什麼。最簡單的外掛是單一 PHP 檔案。 [Hello Dolly](https://wordpress.org/plugins/hello-dolly/) 外掛程式就是這類外掛程式的範例。外掛程式 PHP 檔案只需要一個[外掛程式標頭](https://developer.wordpress.org/plugins/the-basics/header-requirements/)、幾個 PHP 函式以及一些用於附加函式的[勾點](https://developer.wordpress.org/plugins/hooks/)。

外掛程式可讓你大幅增加 WordPress 的功能，而無需修改 WordPress 核心本身。