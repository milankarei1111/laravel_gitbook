# tymon/jwt-auth使用方法

## model 引用JWTSubject \(以User.php 為例\) 並加入下列2個方法

* 宣告User model為JWTSubject介面

```php
use Tymon\JWTAuth\Contracts\JWTSubject;
class User extends Authenticatable implements JWTSubject
{
    //
}
```

* 加入getJWTIdentifier  、getJWTCustomClaims 方法

```php
// 識別碼 
public function getJWTIdentifier()
 {
   return $this->getKey();
 }

// 自定義聲明
public function getJWTCustomClaims()
 {
     return [];
  }
```

* 修改 config\Auth.php

```php
// 修改 config\Auth.php
'guards' => [
        'api' => [
            'driver' => 'jwt',
            'provider' => 'users',
        ],
    ],
```

* 配置 API路由 route\]\api.php
* 新增 API Controller =&gt; Controllers\AuthController
* **Methods**
  * Multiple Guards: 若預設'defaults' =&gt; \['guard' =&gt; web\]未改成api，則需指定auth\('api'\)
  * attempt\(\) 認證 回傳jwt值
  * $token = auth\('api'\)-&gt;attempt\($credentials\);  

