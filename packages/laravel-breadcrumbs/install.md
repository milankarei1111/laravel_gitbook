# breadcrumbs安裝配置

## 安裝 composer 套件包

```bash
composer require davejamesmiller/laravel-breadcrumbs
```

## 發布指令: config 資料夾產生 breadcrumbs.php

```bash
php artisan vendor:publish --tag=breadcrumbs-config
```

## 配置 breadcrumbs.php 文件

'view' =&gt; '麵包屑的樣板路徑'

## 定義麵包屑

> 新增 routes/breadcrumbs.php

### 靜態定義

```php
//register: 靜態定義 寫法1:Home
Breadcrumbs::register('home', function($breadcrumbs){
    $breadcrumbs->push('主控台', route('home'), ['icon' => 'fa fa-fw fa-home']);
});


//register: 靜態定義 寫法2:Home url 不使用icon
Breadcrumbs::register('home', function ($trail) {
    $trail->push('主控台', url('/home'));
});
```

> view調用 Breadcrumbs::render\('home'\)
>
> ```php
> @section('breadcrumb')
>   <section class="content-header">
>     <h1>主控台</h1>
>     {{ Breadcrumbs::render('home') }}
>   </section>
> @endsection
> ```

### 動態定義

```php
Breadcrumbs::macro('resource', function ($name, $title, $icon = null, $field_name = 'title', $parent = null) {

// 文章分類
    Breadcrumbs::for("$name.index", function ($trail) use ($name, $title, $icon, $parent) {
        if ($parent) $trail->parent($parent);
        $trail->push($title, route("$name.index"), ['icon' => $icon]);
    });

    // 文章目錄 > 新增
    Breadcrumbs::for("$name.create", function ($trail) use ($name){
        $trail->parent("$name.index");
        $trail->push('新增', route("$name.create"));
    });
});
```

> 再去新增每個資源 Breadcrumbs::resource\(\)
>
> ```php
> // 用檔案減少 breadcrumbs.php 的負擔
> Breadcrumbs::resource('articles', '文章', 'fa fa-fw fa-file');
> ```
>
> view調用 Breadcrumbs::render\('home'\)
>
> ```php
> @section('breadcrumb')
>   <section class="content-header">
>     <h1>文章管理</h1>
>     {{ Breadcrumbs::render() }}
>   </section>
> @endsection
> ```

