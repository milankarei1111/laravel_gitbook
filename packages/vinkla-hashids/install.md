# vinkla-hashids

## 安裝 composer 套件包

```bash
composer require vinkla/hashids
```

## 配置

### 發布指令 config資料夾產生 hashids.php

```bash
php artisan vendor:publish --provider="Vinkla\Hashids\HashidsServiceProvider"
```

### 在配置文件中修改 salt 和 length信息
1. salt: 為了保證字符串不被輕易的破解，配置salt 給目標數字額外增加一些變量，
  保證他人在沒有相同salt的情況下，無法將符串再轉換回數字，增加了安全性。
2. length:指定轉換後字符串的最小長度，這樣能保證ID統一美觀。

### 使用Tinker 來驗證一下相關的功能
```bash
php artisan tinker
```
```bash
Psy Shell v0.9.11 (PHP 7.2.1 — cli) by Justin Hileman
>>>
    $hashId = Hashids::encode(10);
=> "yw2mNvmvW"
>>> Hashids::decode($hashId);
=> [
     10,
   ]
>>>
```
