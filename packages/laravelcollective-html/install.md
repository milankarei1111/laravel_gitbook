# laravelcollective/html安裝配置

## 安裝 composer 套件包

```bash
composer require laravelcollective/html
```

## 配置

### **註冊服務提供者: 在config\app.php 添加**

```php
// 在 $providers，添加以下服務提供程序

'providers' => [
    Collective\Html\HtmlServiceProvider::class,
]

// 在 $aliases，添加別名
'aliases' => [
    'Form' => Collective\Html\FormFacade::class,
    'Html' => Collective\Html\HtmlFacade::class,
]
```

