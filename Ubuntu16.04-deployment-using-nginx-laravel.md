### Ubuntu16.04 换源

`  cd /etc/apt`


```shell
  #deb包
  deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
  deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse 
  ##测试版源 
  deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse 
  # 源码 
  deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse 
  ##测试版源 
  deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse 
  # Canonical 合作伙伴和附加 
  deb http://archive.canonical.com/ubuntu/ xenial partner 
  deb http://extras.ubuntu.com/ubuntu/ xenial main
```

使用`sudo vim sources.list`打开文件，输入 `ggdG`  删除所有内容（这句指令可以理解为从第1行到最后1行

之间的内容都删了）使用 `sudo apt-get update` 即可更新获取 阿里云软件源 提供的软件列表；

### 安装软件###

    sudo apt-get update

    sudo apt-get install php nginx mysql-server 

    sudo apt-get install redis-server

    apt-get install php7.0-curl php7.0-xml php7.0-mcrypt php7.0-json php7.0-gd php7.0-mbstring php7.0-mysql 
### 配置

1.  配置 `PHP`

   输入 `/fix_pathinfo` 搜索，将 `cgi.fix_pathinfo=1` 改为 `cgi.fix_pathinfo=0 `

       sudo vim /etc/php/7.0/fpm/php.ini

2. 配置 `FPM`

  ```
   sudo vim /etc/php/7.0/fpm/pool.d/www.conf
  ```
   找到 `listen = /run/php/php7.0-fpm.sock` 修改为 `listen = /var/run/php/php7.0-fpm.sock` 。当然，你

  也可以不修改，但必须前后一致，后面会用到这个配置。

3.  配置 `Nginx`

      `sudo vim /etc/nginx/sites-available/default`

         主要是配置 `server` 这一部分，最终配置如下:

      ```
      server {
              listen 80;
          
              root /var/www/your-project-name/public;
              index index.php index.html index.htm;

              # Make site accessible from http://localhost/
              server_name www.xxx.com;

              location / {
                      # First attempt to serve request as file, then
                      # as directory, then fall back to displaying a 404.
                      try_files $uri $uri/ /index.php?$query_string;
                      # Uncomment to enable naxsi on this location
                      # include /etc/nginx/naxsi.rules
              }

              location ~ \.php$ {
                      try_files $uri /index.php =404;
                      fastcgi_split_path_info ^(.+\.php)(/.+)$;
                      fastcgi_pass unix:/run/php/php7.0-fpm.sock;
                      fastcgi_index index.php;
                      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                      include fastcgi_params;
              }
      }
      ```


解释:

   root: 是你的项目的`public`目录，也就是网站的入口

   index: 添加了，`index.php`，告诉Nginx先解析`index.php`文件

   server_name: 你的域名，没有的话填写`localhost`

   location / try_files:  修改为了`try_files $uri $uri/ /index.php?$query_string;`

   location ~ .php$:  部分告诉`Nginx`怎么解析`Php`，原封不动复制即可，但注意：`fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;`的目录要和`fpm`的配置文件中的`listen`一致。


