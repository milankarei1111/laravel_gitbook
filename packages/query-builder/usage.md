# spatie/laravel-query-builder使用方法

## 基本用法 \(可合併運用 使用 「&」 隔開\)

```text
Selecting fields for a query 篩選欄位            /users?fields[users]=id,email
Filtering a query 過濾                          /users?filter[email]=abc
Sorting a query 排序                            /users?sort=email  預設asc 加上「-email』desc
Appending attributes to a query 將屬性附加到查詢  /users?append=fullname
Including relations 關聯                         /users?include=oauthProviders
```

```php
 public function index()
    {
        $users = QueryBuilder::for(User::class)
        ->allowedFields('id') // 顯示欄位 select
        ->allowedFilters(['id', 'name', 'email']) // 包含該欄位的字段，like
        ->allowedSorts(['id', 'email']) // 依照指定欄位排序，sort
        ->allowedAppends(['fullname'])  // 回傳model自定義方法的附加欄位,名稱對應自定義方法的名稱  as
        ->allowedIncludes('oauthProviders') // 參數為關聯定義的方法
        ->get();
        // ->toSql(); // 轉成SQL語法
        return $users;
    }
```

## 高階用法

### Extending query builder 擴展原本的查詢建    /users?include=oauthProviders

```php
    $users = QueryBuilder::for(User::where('id', 23)) // base query instead of model
            ->allowedIncludes(['oauthProviders'])
            // ->where('activated', true) // chain on any of Laravel's query methods
            ->first(); // we only need one specific user
```

### paginate 分頁

```php
  $users = QueryBuilder::for(User::class)
        ->allowedFilters(['name', 'email'])
        ->paginate(3)   // 分頁頁數
        ->appends(request()->query()); // 分頁url會顯示查詢的參數
```

