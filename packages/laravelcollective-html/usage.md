# 使用方法

[laravelcollective官方文件](https://laravelcollective.com/docs/6.0/html#text)

[Laravel學習筆記(13)-Form 表單進階](http://blog.tonycube.com/2015/01/laravel-13-form.html)

{% hint style="danger" %}
在內部使用嚴格的比較（===而不是==）因此在將數字值傳遞到表單時要小心，
Html傳遞的資料型態必須與Session存的值資料型態一致!
{% endhint %}

```php
//  '1' === '1' - comparing passed value and old session value
echo Form::radio('category_id', '1'); 

//  1 === '1' - comparing passed value and old session value
echo Form::radio('category_id', 1); 
```
## 創建表單

{% hint style="info" %}
HTML表單僅支持POST和GET，PUT因此DELETE需使用_method表單添加隱藏字段來欺騙方法。
{% endhint %}

```php
{!! Form::open(['url' => 'foo/bar']) !!}
    //
{!! Form::close() !!}
```

## 表單元件
```php
{{ Form::label('title_label','測試元件') }}
{{Form::label('email', 'E-Mail Address', ['class' => 'awesome'])}}

{{ Form::text('title','哈囉! 你好',['size'=>30,'placeholder'=>'123']) }}
{{ Form::textarea('content','內容') }}
{{ Form::label('password_label','密碼會顯示.......') }}
{{ Form::password('password') }}
{{ Form::file('upload') }}
{{ Form::checkbox('habit', 'reading', true) }}看書

{{ Form::radio('city', 'taipei', true) }}Taipei
{{ Form::radio('city', 'taichung') }}Taichung
{{ Form::radio('city', 'kaohsiung') }}Kaohsiung

{{ Form::select('size', ['L'=>'大','M'=>'中','S'=>'小'], 'M') }}
{{ Form::selectRange('number', 10, 20) }}
{{ Form::submit('submit') }}
{{ Form::button('button') }}
```

## 模型綁定
>表單已經和 $user 所儲存的 User 模型綁定，在之後的欄位，就不需要再指定值，只要欄位名稱和 User 的屬性名稱相同即可。
```php
// 寫法1
{{ Form::model($user, ['action'=>['AdminController@update', $user->id], 'method'=>'PATCH']) }}
// 寫法2
{{ Form::model($user, ['route' => ['admin.user.update', $user->id], 'method'=>'PATCH'])}}

{!! Form::close() !!}
```