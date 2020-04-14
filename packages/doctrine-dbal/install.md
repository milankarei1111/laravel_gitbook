# doctrine/dbal安裝配置

[官方文件](https://docs.spatie.be/laravel-query-builder/v2/installation-setup/)

## 安裝 composer 套件包

```bash
composer require doctrine/dbal
```

## 針對已存在資料表異動，需要有doctrine/dbal依賴

> Before changing a column, be sure to add the doctrine/dbal dependency to your composer.json file.

* 修改欄位
  * 更新欄位屬性
  * 重新命名欄位
* 移除欄位

