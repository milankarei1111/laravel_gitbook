# vinkla-hashids 使用
[哈希数据 ID —— vinkla/hashids](https://learnku.com/courses/laravel-package/2019/hash-data-id-vinklahashids/1945)

我們通常都會通過route方法生成鏈接，例如route('users.show', $user->id)，生成用戶詳情頁的鏈接，結果為http://larabbs.test/users/4。

>但其實我們可以將**Eloquent模型**作為參數值傳給route方法，它會自動提取模型的主鍵來生成URL

## 覆寫方法

### getRouteKey()
> 通過route方法生成鏈接的時候，不使用用戶ID，而是hashids轉換後的字符串
* 底層生成url原始碼: vendor/laravel/framework/src/Illuminate/Routing/UrlGenerator.php
```php
public function formatParameters($parameters)
    {
        $parameters = Arr::wrap($parameters);

        foreach ($parameters as $key => $parameter) {
            if ($parameter instanceof UrlRoutable) {
                $parameters[$key] = $parameter->getRouteKey();
            }
        }

        return $parameters;
    }
```
* 所以只需要在模型中重寫getRouteKey方法，返回hashids轉換後的字符串，生成URL的時候就能獲得想要的結果。

## resolveRouteBinding()
> 路由模型綁定的查詢用戶的時候，先將字符串轉換為用戶ID，再進行查詢。
* 底層生成url原始碼: vendor/laravel/framework/src/Illuminate/Database/Eloquent/Model.php


```php
public function resolveRouteBinding($value)
{
    return $this->where($this->getRouteKeyName(), $value)->first();
}
```

---
>在模型中重寫resolveRouteBinding()、getRouteKey()，並為模型實現一個Trait，來實現URL生成以及路由模型綁定自動使用hashids的功能，即可實現。

原使用方式
route('users.show', $user->id)

模型中重寫getRouteKey方法，返回hashids轉換後的字符串，生成URL的時候就能獲得想要的結果。

模型中重寫resolveRouteBinding 方法，先進行ID 轉換

可以為模型實現一個Trait，來實現URL生成以及路由模型綁定自動使用hashids的功能。
是為模型實現的Trait，當然需要放在Models/Traits目錄中：

方法: 建立trai重寫了resolveRouteBinding和getRouteKey兩個方法

模型 使用use Traits\HashIdHelper;

修改 view原本寫的方式