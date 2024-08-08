---
outline: deep
---

# Feign配置

### 修改HttpClient

OpenFeign默认使用jdk自带的HttpURLConnection，我们知道HttpURLConnection没有连接池、性能和效率比较低，如果采用默认，很可能会遇到性能问题导致系统故障。

<https://blog.csdn.net/hkk666123/article/details/113964715>
<https://www.51cto.com/article/674466.html>
<https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/>
