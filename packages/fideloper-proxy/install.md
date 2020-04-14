# fideloper/proxy安裝配置

## 安裝

[Github文件](https://github.com/fideloper/TrustedProxy)

## 安裝配置

### 安裝 composer 套件包

```bash
composer require fideloper/proxy
```

### 配置

#### **註冊服務提供者: 在config\app.php 添加**

```php
// 在 $providers，添加以下服務提供程序

'providers' => [
    Fideloper\Proxy\TrustedProxyServiceProvider::class,]
```

#### 發布指令 config資料夾產生 trustedproxy.php

```bash
php artisan vendor:publish --provider="Spatie\Cors\CorsServiceProvider" --tag="config"
```

