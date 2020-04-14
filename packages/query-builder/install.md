# spatie/laravel-query-builder安裝配置

[官方文件](https://docs.spatie.be/laravel-query-builder/v2/installation-setup/)

## 安裝 composer 套件包

```bash
composer spatie/laravel-query-builder
```

## 發布指令: config資料夾產生 query-builder.php

```bash
php artisan vendor:publish --provider="Spatie\QueryBuilder\QueryBuilderServiceProvider" --tag="config
```

