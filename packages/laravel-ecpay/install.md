# 安裝

## 安裝 composer 套件包

```bash
composer require tsaiyihua/laravel-ecpay 
```

版本支援

* v2.x
  * PHP >= 7.2
  * Laravel >= 6.0

* v1.x
  * PHP >= 7
  * Laravel >= 5.7 < 6.0

## 發布指令 config資料夾產生 config\ecpay.php

```bash
php artisan vendor:publish --tag=ecpay
```

## 配置. .env檔案 

* 測試參數可參考API文件: **前置準備事項**

  * ECPAY_MERCHANT_ID=廠商編號
  * ECPAY_HASH_KEY=介接 HashKey 
  * ECPAY_HASH_IV=介接 HashIV 
  * ECPAY_INVOICE_HASH_KEY=電子發票介接的 HashKey 
  * ECPAY_INVOICE_HASH_IV=電子發票介接的 HashIV