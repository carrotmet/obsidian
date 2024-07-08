![[Pasted image 20240617155923.png]]
![[Pasted image 20240617160011.png]]

## 通用配置
```shell
location / {
  proxy_pass http://127.0.0.1:8080;
  include proxy_params;
}

```


## 日志记录·
```shell
server {
  listen 80;
  server_name proxy.test.com;

  location / {
     proxy_pass http://192.168.175.20:8080;
     proxy_set_header Host $http_host;
     proxy_http_version 1.1;
     proxy_set_header X-Forwarded-For $proxy_add_x_forward_for;
     }
}
```
## 重复配置
vim /etc/nginx/proxy_params
```shell
proxy_set_header Host $http_host;
proxy_http_version 1.1;


proxy_connect_timeout 30;
proxy_send_timeout 60;
proxy_readtime_out 60;

proxy_buffering on;

proxy_buffer_size 32k;
proxy_buffers  4  128k;
```

```shell
location / {
   proxy_pass http://127.0.0.1:8080;
   include proxy_params;
}
```