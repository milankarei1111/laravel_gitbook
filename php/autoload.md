# Autoload

> 開發者自己定義怎樣載入類別的檔案

[COMPOSER設計原理與基本用法](http://blog.turn.tw/?p=1039)

[PHP系列 - Autoload 自動載入](http://justericgg.logdown.com/posts/196891-php-series-autoload)

## 早期讀取套件的方法

> 透過 require\_once\(\) 與 include\_once\(\)
>
> * require 和 require\_once
>   * 都是用來引入檔案，通常放在 PHP 程式的最前面，PHP 程式在執行前，就會先讀入 require 所指定引入的檔案。

* require\_once 可避免重複引入，故建議用後者。引不到檔案會出現錯誤息，而且程式會停止執行。
  * include 和 include\_once
  * 都是用來引入檔案，一般是放在流程控制的處理區段中
  * include\_once 可避免重複引入，故建議用後者。引不到檔案會出現錯誤息，但程式不會停止。
  * require 和 include
* require\(\) 適合用來引入靜態的內容（如版權宣告）。
* include\(\) 則適合用來引入動態的程式碼（程式內容會依其他程式碼而變動）。

```php
//index.php
include_once "Member.php";
include_once "Mailsender.php";
include_once "Validate.php";
include_once "Other.php";
```

* 缺點
  * 許多檔案用到同樣class，在不同地方都需要再載入一次。
  * 當類別多了起來，會顯得很亂、忘記載入時還會出現錯誤。
* 解決方式:寫一個php，負責載入所有類別，然後在其他檔案都載入這支檔案

## 出現 PHP Autoload 機制

* \_\_autoload
* spl\_autoload
* autoload 與 namespace

### \_\_autoload\(\)

> PHP 5 開始提供俗稱「magic method」的函式
>
> autoload.php負責自動載入 class

```php
// autoload.php
function __autoload($classname) {
    if ($classname === 'xxx.php'){
        $filename = "./". $classname .".php";
        include_once($filename);
    } else if ($classname === 'yyy.php'){
        $filename = "./other_library/". $classname .".php";
        include_once($filename);
    } else if ($classname === 'zzz.php'){
        $filename = "./my_library/". $classname .".php";
        include_once($filename);
    }
    // blah
}
```

> 引入autoload.php，需要使用在 classes 目錄底下\(不包含子目錄\)的所有 class 都將會自動被載入
>
> ```php
> <?php
> //index.php
> include_once __DIR__ . "/autoload.php";
> ```

* 缺點
  * 就是這個\_\_autoload函式內容會變得很巨大。
  * 假設類別目錄非常多，需要不斷加入目錄判斷，在整理檔案的時候，顯得有些混亂。

### spl\_autoload\(\) 與 spl\_autoload\_register\(\)

> PHP 5.1.2開始，多提供了一個函式，多寫幾個autoload函式，然後註冊起來，效果跟直接使用\_\_autoload相同。
>
> 可以針對不同用途的類別，分批autoload了
>
> \`\`\`php spl\_autoload\_register\('my\_library\_loader'\); spl\_autoload\_register\('other\_library\_loader'\); spl\_autoload\_register\('basic\_loader'\);

function my\_library\_loader\($classname\) { $filename = "./my\_library/". $classname .".php"; include\_once\($filename\); } function other\_library\_loader\($classname\) { $filename = "./other\_library/". $classname .".php"; include\_once\($filename\); } function basic\_loader\($classname\) { $filename = "./". $classname .".php"; include\_once\($filename\);

```text
* 缺點
    * 每個專案都要寫上一堆程式碼
    * 假設類別目錄非常多，需要不斷加入目錄判斷，在整理檔案的時候，顯得有些混亂。

* 解決
    * 發明一個工具直接讀取存放class載入一切 => composer套件管理

---

### namespaces 命名空間
> php 5.3出現，目的是用來組織類別與函數，並且避免名稱的衝突。


---
### composer套件管理php
> 安裝後，vendor資料夾，內含一個autoload.php。
你只要載入這個檔案
```php
<?php
require 'vendor/autoload.php'
```

* 需要的所有類別，都會在適當的時候、以適當的方式自動載入。

