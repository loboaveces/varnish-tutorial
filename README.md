# varnish-tutorial
Mentoring de Varnish y Fastly

## 1. Fundamentals of Varnish
### What is Varnish?
Varnish is a reverse HTTP proxy (an HTTP accelerator). A reverse proxy is a proxy server that appears to clients as an ordinary server.
Varnish stores (caches) files or fragments of files in memory that are used to reduce the response time and network bandwidth consumption on future, equivalent requests.

### Varnish Configuration Language (VCL)
Varnish is flexible because you can configure it and write your own caching policies in its Varnish Configuration Language (VCL). VCL is a domain specific language based on C. VCL is then translated to C code and compiled, therefore Varnish executes lightning fast. Varnish has shown itself to work well both on large (and expensive) servers and tiny appliances.

```
vcl 4.0;
backend default {
 .host = "127.0.0.1";
 .port = "8080";
}
sub vcl_recv {
 # Do request header transformations here.
 if (req.url ~ "^/admin") {
 return(pass);
 }
}
```
When you need functionalities that VCL does not provide, e.g., look for an IP address in a database, you can write raw C code in your VCL. That is in-line C in VCL. However, in-line C is strongly discouraged because in-line C is more difficult to debug, maintain and develop with other developers. Instead in adding in-line C, you should modularized your C code in Varnish modules, also known as VMODs

### How objects are stored?
Objects are local stores of response messages as defined in https://tools.ietf.org/html/rfc7234. They are mapped with a hash key and they are stored in memory. References to objects in memory are kept in a hash tree.
A rather unique feature of Varnish is that it allows you to control the input of the hashing algorithm. The key is by default made out of the HTTP Host header and the URL, which is sufficient and recommended for typical cases. However, you are able to create the key from something else.
![objects](https://res.cloudinary.com/dkoli3kgi/image/upload/v1641134742/Screen_Shot_2022-01-02_at_10.31.31_cobri1.png)
