# Intervention/image使用方法

[官方使用文件](http://image.intervention.io/use/basics)

## 使用方法

### 讀取圖片 make\(\)

```php
Image::make('images/test.jpg');
```

### 修改圖片的大小 resize\(\)

```php
Image::make('images/test.jpg')->resize(900, 200);
```

### 上傳圖片 save\(路徑, 品質0-100,格式\)

```php
$img = Image::make('images/test.jpg')
$img->->resize(900, 200)->save('images/canvas_path.jpg');
```

### 加入另一張圖片 insert\(路徑, 位置, X, Y\)

位置

* top-left \(default\)
* top
* top-right
* left
* center
* right
* bottom-left
* bottom
* bottom-right

```php
$img = Image::make('images/test.jpg');
// 距離右邊界 15 像素,距离下邊界10像素
$img->insert($origin_add_watermark, 'bottom-right', 15, 10);
```

### 加寬 widen\(\)  模糊特效 blur\(\)

```php
$img = Image::make('images/test.jpg')
$img->widen(1500)->blur(15);
```

### 畫布 canvas\(\)

> 需安裝ImageMagick PHP擴展包 [安裝教學](https://mlocati.github.io/articles/php-windows-imagick.html)
>
> * 依系統下載對應檔案:
>   * php\_imagick-3.4.4-7.2-ts-vc15-x64
>   * ImageMagick-7.0.7-11-vc15-x64
> * php\_imagick 解壓縮後將 php\_imagick.dll 檔案移到php/php-7.2.1-nts/ext資料夾
> * ImageMagick解壓縮後將bin資料夾移動至與 php.exe同目錄下
> * php.ini文件添加：extension=php\_imagick.dll

```php
$img = Image::canvas(200, 200,'#ff0000');
```

### 文字 text\(文字, X, Y, $Callback\)

```php
$img = Image::make('images/logo.jpg');
// write text at position
$img->text('The quick brown fox jumps over the lazy dog.', 50, 100);
// use callback to define details
$img->text('Hi', 40, 50, function($font) {
    $font->file('5'); // 為True Type字體文件或GD庫內部字體之一的1到5之間的整數值。默認值：1
    $font->size(100);
    $font->color('#fdf6e3'); 
    $font->align('center'); // 水平對其
    $font->valign('top');   // 垂直對齊
    $font->angle(1);   // 旋轉角度
})->save('images/newlogo.jpg');
```

### http直接回傳畫布

```php
$img = Image::canvas(200, 300, '#ff0000');
// send HTTP header and output image data
return $img->response();
```

## 實作

```php
// PostController
class PostController extends Controller
{
    public function index()
    {
        // 路徑為: public/images/
        $origin_add_watermark = 'images/origin_add_watermark.jpg';
        $logo = 'images/logo.jpg';

        $pathArr = array();
        $pathArr['origin_path'] = 'images/test.jpg';
        $pathArr['origin_resize_path'] = 'images/newtest2.jpg';
        $pathArr['watermark_right'] = 'images/watermark_right.jpg';
        $pathArr['watermark_left'] = 'images/watermark_left.jpg';
        $pathArr['origin_widen_path'] = 'images/widen_path.jpg';
        $pathArr['canvas_path'] = 'images/canvas_path.jpg';
        $pathArr['canvas_watermark_path'] = 'images/canvas_watermark_path.jpg';
        $pathArr['examples'] = 'images/examples.jpg';


        // // 讀取資料
        $img = Image::make($pathArr['origin_path']);

        // 測試1 - 修改指定图片的大小
        $img->resize(900, 200);
        // 調整後重新儲存到其他路径
        $img->save($pathArr['origin_resize_path']);

        // 測試2 -插入水印, 水印位置在原图片的右下角,  距离右边距 15 像素,距离下边距 10 像素,
        $img->insert($origin_add_watermark, 'bottom-right', 50, 10);
        $img->save($pathArr['watermark_right']);

        // 測試3 左邊浮水印
        $img = Image::make($pathArr['origin_path'])->resize(900, 200)
                    ->insert($origin_add_watermark, 'bottom-left', 1, 2)
                    ->save($pathArr['watermark_left']);

        // 測試4 加寬1500 並且加入模糊特效
        $img = Image::make($pathArr['origin_path'])->widen(1500)->blur(15);

        $img->save($pathArr['origin_widen_path']);

         // 產生畫布
        $image = Image::canvas(200, 200,'#ff0000');
        $image->save($pathArr['canvas_path']);

        // 產生浮水印
        $image = Image::canvas(280, 280);
        $image->text('text', 150, 20, function ($font) {
            $font->file(5); // 為True Type字體文件或GD庫內部字體之一的1到5之間的整數值。默認值：1
            $font->valign('top');
            $font->align('right');
            $font->color(array(255, 255, 255, 0.3)); // RGN與介於1（不透明）和0（完全透明）之間的alpha值
        })
        ->insert($origin_add_watermark, 'bottom-right', 10, 15)
        ->resize(200, 200)
        ->save($pathArr['canvas_watermark_path']);

        // create Image from file
            $img = Image::make($logo);
            $img->text('The quick brown fox jumps over the lazy dog.', 50, 100);
            // use callback to define details
            $img->text('Hi', 40, 50, function($font) {
                $font->file('5');
                $font->size(100);
                $font->color('#fdf6e3');
                $font->align('center');
                $font->valign('top');
                $font->angle(10);  // 旋轉角度
            })->save($pathArr['examples']);
        return view('backend.post.index', compact('pathArr'));

        // http直接回傳畫布
        // $img = Image::canvas(200, 300, '#ff0000');
        // // send HTTP header and output image data
        // return $img->response();

        // $img = Image::cache(function($image) {
        //     $image->make($logo)->resize(300, 200)->greyscale();
        // });
    }
}
```

```php
// view
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div class="container">
        <div class="content">
            <div class="title">Laravel image Demo</div>
            <div>
                <p>Origin Image 1221 * 264</p>
                <img src="{{ asset($pathArr['origin_path']) }}"><br/>
            </div>

            <div class="hr"><i class="fa fa-hand-o-down"></i></div>
            <div class="item2">
                <p>Resize To 500 * 100 Image</p>
                <img src="{{  asset($pathArr['origin_resize_path']) }}"><br/>
            </div>

            <div class="hr"><i class="fa fa-hand-o-down"></i></div>
            <div>
                <p>Resize And Add Watermark Image right</p>
                <img src="{{  asset($pathArr['watermark_left']) }}"><br/>
            </div>
            <div>
                <p>Resize And Add Watermark Image left </p>
                <img src="{{  asset($pathArr['watermark_right']) }}"><br/>
            </div>
            <div>
                <p>Resize widen 1500 and blur</p>
                <img src="{{  asset($pathArr['origin_widen_path']) }}"><br/>
            </div>
            <div>
                <p>And canvas</p>
                <img src="{{  asset($pathArr['canvas_path']) }}"><br/>
            </div>
            <div>
                <p>And canvas and Watermark</p>
                <img src="{{  asset($pathArr['canvas_watermark_path']) }}"><br/>
            </div>
            <div class="hr"><i class="fa fa-hand-o-down"></i></div>
            <div>
                <p>And canvas and Watermark</p>
                <img src="{{  asset($pathArr['examples']) }}"><br/>
            </div>
        </div>
    </div>
</html>
```

