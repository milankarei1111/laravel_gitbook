# tymon/jwt-auth安裝配置

## 安裝 composer 套件包

```bash
$ composer require tymon/jwt-auth:dev-develop
```

## 配置

### **註冊服務提供者: 在config\app.php 添加**

{% code title="" %}
```

```
{% endcode %}

```php
// 在 $providers，添加以下服務提供程序

'providers' => [
        Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
]
```

### 發布指令 config資料夾產生jwt.php

```bash
$ php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider
```

### **在「.env」產生JWT憑證**

{% hint style="danger" %}
**JWT\_SECRET=secret，若憑證為空值，被視為不合法身分**
{% endhint %}

```bash
$ php artisan jwt:secret
```

