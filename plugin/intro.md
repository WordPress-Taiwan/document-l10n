``` Block:InfoCallout
本文譯自〈[Introduction to Plugin Development](https://developer.wordpress.org/plugins/intro/)〉
``` 


外掛開發簡介
======

歡迎來到《外掛開發人員手冊》。無論你正在開發第一個還是第五十個外掛，我們都希望可以透過這份資源幫助盡可能你寫出最好的外掛。

《外掛開發人員手冊》涵蓋了各種主題，從外掛標頭中應包含的內容到安全性的最佳作法，再到可用於建立外掛的工具。這份文件會持續更新，如果你發現某些內容缺失或不完整，請透過 Slack 通知文件團隊，我們將共同改進。

## 為什麼要建立外掛

WordPress 開發中有一條基本規則，那就是:「不要變動 WordPress 核心程式 (Don’t touch WordPress core)」，這意味著開發者並不會透過編輯 WordPress 核心程式的檔案來增加網站功能，因為 WordPress 每次更新都會覆蓋這些檔案。如果想要新增或修改任何功能，都應該用外掛。

WordPress 外掛根據需求可以相當簡單或很複雜，一切取決於想要達成什麼目的。最簡單的外掛是單一的 PHP 檔案，[Hello Dolly](https://wordpress.org/plugins/hello-dolly/) 就是這類外掛的範例。外掛的 PHP 檔案只需要一個[外掛程式標頭](https://developer.wordpress.org/plugins/the-basics/header-requirements/)、幾個 PHP 函式以及供這些函式連結的[勾點 (Hooks)](https://developer.wordpress.org/plugins/hooks/)。

外掛可讓你大幅擴充 WordPress 的功能，而毋需修改 WordPress 核心程式。