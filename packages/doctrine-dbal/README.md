---
description: PHP數據庫抽象層（DBAL），具有用於數據庫模式自省和管理的功能。DBAL(Database Abstraction Layer) 數據庫抽象層
---

# doctrine/dbal

[參考文件來源](https://gonzalo123.com/2011/07/11/database-abstraction-layers-in-php-pdo-versus-dbal/)

PHP有一個名為Doctrine2的出色項目。 Doctrine2是一個ORM，它使用自己DBAL的數據庫抽象層。 DBAL並不是純粹的數據庫抽象層，是基於PDO構建。 這是我們可以使用的一組PHP類，它為我們提供了“純” PDO所不具備的功能

在laravel中使用migration作為版本控制工具， 當對已存在的資料表做欄位異動的時候， 需要額外的 doctrine / dbal擴展。

