# Intervention/image使用方法

[sentinel 3.x 官方文檔](https://cartalyst.com/manual/sentinel/3.x)

[官方演示登入功能](https://demo.cartalyst.com/sentinel-kickstart/login)

[hotexamples.com 程式範例 ](https://hotexamples.com/examples/cartalyst.sentinel.laravel.facades/Sentinel/getUser/php-sentinel-getuser-method-examples.html)

[範例:Sentinel 2.0 用戶認證應用](http://laravel51.blogspot.com/2015/08/3-sentinel-20.html)

## 判斷是否登入

```php
// 驗證用戶
Sentinel::authenticate([
    'email' => $request->input('email'),
    'password' => $request->input('password'),
]);
// 檢查用戶是否登入(可用於中間件)
Sentinel::check()
```

## 取得認證資料

```php
// 檢查當前登陸用戶
Sentinel::getUser(bool $check)

// 通過憑證查找用戶
Sentinel::findByCredentials([
    'login' => 'admin@gmail.com'
])

// 查詢 admin 角色 	
Sentinel::findRoleBySlug('admin')
```

## 登出
```php
// 刪除當前登入用戶session
Sentinel::logout()
```

## 增刪改查

### 新增

```php
/*
 * @ $credentials 註冊資料
 * @ $callback 是否激活
*/

// 通過憑證查找用戶
Sentinel::findByCredentials([
    'login' => 'abc@gmail.com'
])

// 註冊
Sentinel::register(array $credentials, bool $callback）

// 註冊並激活用戶 
Sentinel::registerAndActivate (array $credentials)

Sentinel::create($credentials)

```

## 管理角色

**登陸後才可呼叫 =&gt; inRole\(\)**

```text
$role = Sentinel::findRoleBySlug('admin');
```

role

| id | slug | name |
| :--- | :---: | ---: |
| 1 | Admin | 系統管理員 |
| 2 | Boss | 公司主管 |

role users

| user\_id | role\_id |
| :--- | :---: |
| 12 | 1 |
| 13 | 1 |

user

| id | email |
| :--- | :---: |
| 12 | admin@abc.com |
| 13 | boss@abc.com |

