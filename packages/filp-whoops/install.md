# filp/whoops安裝配置

[範例](https://www.golaravel.com/post/bringing-whoops-back-to-laravel-5/)

## 安裝 composer 套件包

```bash
composer require filp/whoops
```

## 設定

> 設定 app/Exceptions/Handler.php，在render\(\)方法 添加一個Whoops樣式的處理情況

```php
    /**
     * Render an exception into an HTTP response.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Exception  $exception
     * @return \Illuminate\Http\Response
     */
    public function render($request, Exception $exception)
    {
        if ($this->isHttpException($exception)) {
            return $this->renderHttpException($exception);
        }

        if (config('app.debug')) {
            return $this->renderExceptionWithWhoops($exception);
        }
        return parent::render($request, $exception);
    }

        /**
     * Render an exception using Whoops.
     *
     * @param  \Exception $e
     * @return \Illuminate\Http\Response
     */
    protected function renderExceptionWithWhoops(Exception $e)
    {
        $whoops = new \Whoops\Run;
        $whoops->pushHandler(new \Whoops\Handler\PrettyPageHandler());

        return new \Illuminate\Http\Response(
            $whoops->handleException($e)
            // $e->getStatusCode(),
            // $e->getHeaders()
        );
    }
```

