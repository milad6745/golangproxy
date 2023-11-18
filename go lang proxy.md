برای ارسال ترافیک از یک برنامه به سمت یک پراکسی سرور در Golang، می‌توانید از کتابخانه‌های مختلفی مانند `net/http` و `golang.org/x/net/proxy` استفاده کنید. در ادامه، یک مثال از نحوه ارسال درخواست HTTP از طریق یک پراکسی SOCKS5 نمایش داده شده است.

ابتدا، مطمئن شوید که کتابخانه‌های مورد نیاز نصب شده باشند:

```bash
go get golang.org/x/net/proxy
```

حالا می‌توانید از کد زیر برای ارسال درخواست HTTP از طریق پراکسی استفاده کنید:

```go
package main

import (
	"golang.org/x/net/proxy"
	"net/http"
	"net/url"
)

func main() {
	// تنظیمات پروکسی
	proxyURL, err := url.Parse("socks5://proxy-server-address:proxy-server-port")
	if err != nil {
		panic(err)
	}

	// ایجاد کلاینت با استفاده از پروکسی
	transport := &http.Transport{
		Proxy: http.ProxyURL(proxyURL),
	}

	client := &http.Client{
		Transport: transport,
	}

	// ارسال درخواست HTTP از طریق پروکسی
	response, err := client.Get("https://example.com")
	if err != nil {
		panic(err)
	}
	defer response.Body.Close()

	// پردازش پاسخ
	// ...

	// مثال: چاپ محتوای پاسخ
	// به عنوان مثال، محتوای پاسخ را چاپ می‌کنیم
	fmt.Println("Response:", response)
}
```

در کد فوق، `proxy-server-address` و `proxy-server-port` را با اطلاعات واقعی پراکسی خود جایگزین کنید. همچنین، می‌توانید مراحل پردازش پاسخ را بر اساس نیازهای خود تغییر دهید.

توجه داشته باشید که نوع پروکسی (مثل SOCKS5 یا HTTP) و تنظیمات مربوط به آن را باید با توجه به پروتکل پروکسی مورد استفاده خود تنظیم کنید.
