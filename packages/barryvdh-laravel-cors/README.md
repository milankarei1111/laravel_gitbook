---
description: 解決前後端跨域問題
---

# barryvdh/laravel-cors

**瀏覽器的同源策略是最核心的安全功能 \(同源:網域名稱，協議，端口都相同\)**

如果兩個地址不同源，資源無法互相讀取

```text
Cookie、LocalStorage 和IndexDB 無法相互讀取；
DOM 無法相互獲得；
AJAX 請求不能相互發送。
```

解決: 透過跨域資源共享（CORS:Cross-origin resource sharing）標準，來解決瀏覽器跨域問題。

通過HTTP的Header來進行交互；主要通過後端來設置CORS配置項

## 功能

允許使用Laravel中間件配置發送跨域資源\(CROS\)共享標頭

## 針對簡單請求的CORS，服務器需要返回的字段有

```text
Access-Control-Allow-Origin ——允許的域名，這個字段是必須的，*代表允許所有域名；
Access-Control-Allow-Credentials——CORS 請求是否發送Cookie；
Access-Control-Expose-Headers —— 表示伺服器允許瀏覽器存取回應標頭的白名單
```

