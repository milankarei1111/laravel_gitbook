# 驗證

[Laravel 6中文文檔 -表單驗證](https://learnku.com/docs/laravel/6.x/validation/5144)

## 控制器驗證

###  Validator Facades 調用make
```text
 返回一個Validator實例 ($validator)
```

```php
use Illuminate\Support\Facades\Validator;

class TestController extends Controller
{
    public function testValidate(request $request)
    {
        /**
         * 語法:
         * $validator = Validator::make('傳入值', '驗證規則', '自定義訊息'),
         * 返回 Validator實例 ($validator) 
         */

        // 寫法1
        $validator = Validator::make($request->all(), [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
        ], []);

        // 寫法2
        $input = [
            'title'=> '',
        ];

        $rules = [
            'user_id' => 'required|integer',
            'title' => 'nullable|string',
        ];

        $messages = [
            'required' => '這個 :attribute 必填',
        ];

        $validator = Validator::make($input, $rules, $messages);

        /**
         * 處理錯誤消息
         * 通過Validator實例 $validato調用errors()
         * 返回Illuminate\Support\MessageBag 實例,
         * 自動提供視圖的$errors變量，也是 MessageBag類的一個實例
         *
         * first('字段')  查看特定字段的第一個錯誤信息  返回 bool       $errors->first'title')
         * get('字段')    查看特定字段的所有错误消息                    $errors->get('title')
         * all()          查看所有字段的所有错误消息    返回 array      $errors->all()
         * has()          是否有錯誤
         */

        $errors = $validator->errors();
        foreach ($errors->all() as $message) {

        }
    }

}
```

## FormRequest

指令
```bash
php artisan make:request <XxxxRequest 名稱>
```


> 生成後檔案放至在 app\Http\Requests\TestRequest.php
> 
> 預設2個方法 authorize() 、 rules()
> 
> 可以自定義字段名稱 attributes() 、 錯誤訊息 messages()


```php
// 預設
namespace App\Http\Requests;
use Illuminate\Foundation\Http\FormRequest;

class TestRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     * 
     * @return bool
     */
    public function authorize()
    {
        return false;
    }
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            //
        ];
    }
}
```

```php
// 自己可以依需求客製檔案

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class TestRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     * 
     * @return bool
     */
    public function authorize()
    {
        // 預設false 未驗證會出現 403 This action is unauthorized.,改為true 已驗證
        return true; 
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            //
        ];
    }

    public function attributes()
    {
         return [
            'title' => 'required',
        ];
    }

    // 自定義錯誤消息#
     public function messages()
    {
        return [
            'title' => '標題',
        ];
    }
    
}
```

## 文件指定驗證訊息

路徑:resources/lang/xx/validation.php

```php

// 控制器
$request->validate([
    'credit_card_number' => 'required_if:payment_type,cc'
]);

// 指定自定義值:將驗證信息的:value替換為自定義的表示形式
// 輸出: The credit card number field is required when payment type is credit card.
'values' => [
    'payment_type' => [
        'cc' => 'credit card'
    ],
],

// custom 自定義

'custom' => [
    'email' => [
        'required' => 'We need to know your e-mail address!',
    ],
],

// :attribute部分替換為自定義屬性名稱
'attributes' => [
        'address'  => '地址',
        'title'  => '標題',
        'email' => '信箱',

```
