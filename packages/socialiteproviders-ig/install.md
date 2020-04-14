# socialiteproviders/instagram安裝配置

[Socialite Providers](https://socialiteproviders.netlify.com/providers/instagram.html)

[Integrating Instagram API in Laravel 5.6](https://quantizd.com/integrating-instagram-api-in-laravel-5-6/)

## 安裝 composer 套件包

```bash
composer require socialiteproviders/instagram
```

## 配置

### **註冊服務提供者: 在config\app.php 添加**

```php
// 在 $providers，添加以下服務提供程序

'providers' => [
    SocialiteProviders\Manager\ServiceProvider::class,
]
```

### EventServiceProvider 註冊事件監聽器

> event 功能提供一個簡單的觀察者實作，允許你在應用程式裡訂閱與監聽事件

```php
// 
protected $listen = [
    SocialiteWasCalled::class => [
        'SocialiteProviders\\Instagram\\InstagramExtendSocialite@handle',
    ],
];
```

### 申請API憑證 \(developers.facebook.com\)

[API申請指南](https://developers.facebook.com/docs/instagram-basic-display-api/getting-started)

> 從2019年10月15日開始，Instagram API平台上的新客戶端註冊和權限審查將停止，改由developers.facebook建立

### 配置Client ID

