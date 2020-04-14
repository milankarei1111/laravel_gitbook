# barryvdh/laravel-cors安裝配置

## 安裝 composer 套件包

```bash
$ composer require barryvdh/laravel-cors
```

## 配置

### **註冊服務提供者: 在config\app.php 添加**

```php
// 在 $providers，添加以下服務提供程序

'providers' => [
        Barryvdh\Cors\ServiceProvider::class,
]
```

### 發布指令 config資料夾產生cros.php

```bash
$ php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"
```

### 使用 \(app\Http\Kernel 中間件註冊\)

```php
// 全局使用跨域  $middleware
protected $middleware = [        
            \Barryvdh\Cors\HandleCors::class,
        ],
    ];

// 特定路由或中間件群組 $middlewareGroups
protected $middlewareGroups = [        
       'api' => [
            \Barryvdh\Cors\HandleCors::class,
        ],
    ];
```

