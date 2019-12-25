## 访问买家的前端界面
1. 项目的前后端是完全分离的，买家端前端的代码在另一个仓库，使用`git clone https://github.com/sqmax/vuejs-project.git`下载前端项目，其中项目根路径（vuejs-project）下的dist目录就是前端编译后的代码。
2. 修改nginx的配置文件，让nginx可以找到前端代码。在nginx根目录下的conf目录下有一个nginx.conf文件，它就是我们要修改的配置文件，其中有下面一段：

```xml
 server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   F:\vuejs-project\dist; #前端资源路径
            index  index.html index.htm;
        }
	location /sell/ {
		proxy_pass http://127.0.0.1:8080/sell/;
	}

```

上面的`F:\vuejs-project\dist;`该为你刚才git clone下的前端项目的dist目录。

4. 双击nginx.exe启动nginx服务器，如果已启动过，命令行进入nginx的根目录，输入`nginx -s reload`重启nginx服务器。
5. 浏览器访问：`http://127.0.0.1/#/order/`，这是会出现空白界面，按F2打开浏览器的开发者工具，在浏览器的控制台输入`document.cookie='openid=abc123'`
向该域名下添加cookie。再次访问：`http://127.0.0.1`，这时就可以访问到前端界面了。如下：

![](https://i.postimg.cc/MG0S8fcR/weixin.png)
6. 对于手机端微信公众号内访问，还要使用到内网穿透工具，由于微信里不能直接访问ip地址，还要购买域名，还涉及到挺复杂的微信调试。这里就不再介绍。可以使用postman这个工具模拟微信点餐下单。访问接口参见controller包下以Buyer开头的类。         
7. 如果想查看微信端的访问效果，可以在微信客户端访问这个链接：`http://sell.springboot.cn/`。（注意这是师兄上线的项目演示）
如果使用电脑访问的话，可以首先访问：[http://sell.springboot.cn/#/order/](http://sell.springboot.cn/#/order/)；
然后，按`F12`打开浏览器的开发者工具，点击控制台，在控制台输入：`document.cookie='openid=abc123'`；
然后重新访问：[http://sell.springboot.cn](http://sell.springboot.cn)，就可以看到前端效果了。
