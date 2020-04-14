# phpunit安裝配置

[官方文檔](https://phpunit.readthedocs.io/en/latest/index.html)

[PHPUnit 入门教程](https://learnku.com/laravel/t/22814)

[斷言分類整理](https://www.cnblogs.com/Berryxiong/p/7285240.html)

[學院君測試系列](https://xueyuanjun.com/post/19980)

## 新建一個測試

```text
php artisan make:test BasicTest
```

## 執行測試

```php
// 方法1
phpunit

// 方法2
./vendor/bin/phpunit
```

## 基本斷言\(assertion\)

* 布林值：assertTrue\(\)、assertFalse\(\)
* 相等或是空值 assertEquals\(\)、assertNull\(\)
* 作用於數組：assertContains \(\) 、assertCount \(\)、assertEmpty \(\)
* http 狀態 : assertStatus\(\)

## 測試成功

```php
PHPUnit 8.4.3 by Sebastian Bergmann and contributors.

......                                                              6 / 6 (100%)

Time: 888 ms, Memory: 20.00 MB

// 黃色:未執行或跳過存在風險
OK, but incomplete, skipped, or risky tests!
Tests: 6, Assertions: 11, Risky: 1.

// 綠色:失敗
OK (6 tests, 12 assertions)

// 紅色:失敗
Expected status code 200 but received 404.
Failed asserting that false is true.

FAILURES!
Tests: 7, Assertions: 13, Failures: 1.
```

![phpunit&#x57F7;&#x884C;&#x72C0;&#x614B;](https://github.com/milankarei1111/laravel_gitbook/tree/0cf5f998e77fce088b1b18fcb926ba37dae512f4/.gitbook/assets/phpunit_status.jpg)

## 實作 1:基本斷言測試

### 建立測試 Box 類

```php
<?php
namespace App\TestClass;

class Box
{
    /**
     * @var array
     */
    protected $items = [];

    /**
     * 使用给定项构造框
     *
     * @param array $items
     */
    public function __construct($items = [])
    {
        $this->items = $items;
    }

    /**
     * 检查指定的项目是否在框中。
     *
     * @param string $item
     * @return bool
     */
    public function has($item)
    {
        return in_array($item, $this->items);
    }

    /**
     * 从框中移除项，如果框为空，则为 null 。
     *
     * @return string
     */
    public function takeOne()
    {
        return array_shift($this->items);
    }

    /**
     * 从包含指定字母开头的框中检索所有项目。
     *
     * @param string $letter
     * @return array
     */
    public function startsWith($letter)
    {
        return array_filter($this->items, function ($item) use ($letter) {
            return stripos($item, $letter) === 0;
        });
    }
}
```

### 建立 phpunit

```text
php artisan make:test BasicTest
```

```php
<?php
namespace Tests\Feature;

use Tests\TestCase;
use App\TestClass\Box;
use Illuminate\Foundation\Testing\WithFaker;
use Illuminate\Foundation\Testing\RefreshDatabase;

class BasicTest extends TestCase
{
    public function testHasItemInBox()
    {
        $box = new Box(['cat', 'toy', 'torch']);

        $this->assertTrue($box->has('toy'));
        $this->assertFalse($box->has('ball'));
    }

    public function testTakeOneFromTheBox()
    {
        $box = new Box(['torch']);

        $this->assertEquals('torch', $box->takeOne());

        // 当前 Box 为空，应当为 Null
        $this->assertNull($box->takeOne());
    }
    public function testStartsWithALetter()
    {
        $box = new Box(['toy', 'torch', 'ball', 'cat', 'tissue']);

        $results = $box->startsWith('t');

        $this->assertCount(3, $results);
        $this->assertContains('toy', $results);
        $this->assertContains('torch', $results);
        $this->assertContains('tissue', $results);

        // 如果传递复数断言数组为空
        $this->assertEmpty($box->startsWith('s'));
    }
}
```

## 實作 2 Http 測試

```php
<?php
class ExampleTest extends TestCase
{
    public function testBasicTest()
    {
        // 測試http get請求是否成功
        $response = $this->get('/');

        $response->assertStatus(200);
    }

    public function testBasicExample()
    {
        // 測試呼叫json api
        $response = $this->withHeaders([
            'X-Header' => 'Value',
        ])->json('get','/api/users');
        $response ->assertStatus(200);

        // dump 和 dumpHeaders 方法可用於檢查和測試響應內容
        $response = $this->get('/');
        $response->dumpHeaders();
        $response->dump();
    }
    public function testDatabase()
    {
        // 數據庫測試
        $this->assertDatabaseHas('users', [
            'email' => 'admin@abc.com'
        ]);
    }
}
```

