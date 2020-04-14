# Intervention/image安裝配置

## 安裝 composer 套件包

```text
$ composer require cartalyst/sentinel
```

## 配置

### config\app.php配置

```php
// 在 $providers，添加以下服務提供程序
'providers' => [
     Cartalyst\Sentinel\Laravel\SentinelServiceProvider::class,
        ],


// 在$ aliases，為此包裝添加以下外觀
'aliases' => [
    'Activation' => Cartalyst\Sentinel\Laravel\Facades\Activation::class,
    'Reminder'   => Cartalyst\Sentinel\Laravel\Facades\Reminder::class,
    'Sentinel'   => Cartalyst\Sentinel\Laravel\Facades\Sentinel::class,
]
```

### 發布指令: config資料夾產生cros.php

```text
$ php artisan vendor:publish
    --provider="Cartalyst\Sentinel\Laravel\SentinelServiceProvider"
```

### 執行遷移文件

```text
$ php artisan migrate 2014_07_02_230147_migration_cartalyst_sentinel
```

### 資料庫產生以下表格

* activations
* persistences
* reminders
* roles
* role\_users
* throttle
* users

