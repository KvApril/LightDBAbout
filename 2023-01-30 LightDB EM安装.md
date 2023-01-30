### LightDB EM安装

#### 简介
LightDB Enterprise Manager（即 LightDB数据库监控管理平台，下文均简称为LightDB EM）是一个综合性的数据库监控和管理系统，
旨在满足数据库用户的需求，提供强大的图形界面，简化了对LightDB数据库的维护和使用。

#### 下载
```sql
下载地址：http://www.light-pg.com/downloadCate.html

系统：centos-7
服务器地址：192.168.121.111
LightDB环境: 单机版本
EM版本：lightdb-em-13.8-22.4-932edbddb-el7.x86_64.zip
```

#### 安装步骤
1.解压缩
```sql
[lightdb@localhost ~]$ unzip lightdb-em-13.8-22.4-932edbddb-el7.x86_64.zip
#查看目录
[lightdb@localhost lightdb-em-13.8-22.4-932edbddb-el7.x86_64]$ ll
总用量 0
drwxr-x---  9 lightdb lightdb 106 1月  12 20:17 agent
drwxr-xr-x 12 lightdb lightdb 182 1月  12 20:17 em
drwxrwxr-x  9 lightdb lightdb 140 1月  12 20:17 jdk
drwxr-x---  3 lightdb lightdb  23 12月 30 18:23 lightdb-x-for-em
drwxrwxr-x  4 lightdb lightdb 273 1月  12 20:17 tools
```

2. 移动em,lightdb-x-for-em到LightDB的安装目录下
```sql
[lightdb@localhost lightdb]$ ll
总用量 0
drwxr-xr-x 13 lightdb lightdb 194 1月  19 03:02 em
drwxrwxr-x  9 lightdb lightdb 140 1月   9 18:00 jdk
drwxrwxr-x  3 lightdb lightdb  23 1月   9 17:59 lightdb-x
drwxr-x---  3 lightdb lightdb  23 1月  19 03:09 lightdb-x-for-em
drwxrwxr-x  3 lightdb lightdb 107 1月   9 18:00 uninstall
[lightdb@localhost lightdb]$ 

```
3. 修改配置
```sql
有三个主要配置，可根据情况进行修改
配置1: gotty【必选】
配置2: redis【可选】
配置3: nginx【可选】
配置4: jrescloud.properties【必选】
---------------------------------
#配置并启动gotty
1.修改gotty的端口号【默认:18333】: em/scripts/gotty_start.sh
2.启动gotty: sh em/scripts/gotty_start.sh

#配置并启动redis【可选】
如果要使用EM自带的redis,按照以下步骤操作：  
1. 修改em/redis/redis.conf,按照需要修改文件末尾的端口号，和认证密码: 
    port 18331
    requirepass lightdb123
2. 启动redis: sh em/scripts/redis_start.sh

#配置并启动nginx
1.修改 em/nginx/conf/nginx.conf
    server {
        listen  17331; 
		index		 index.html;
		rewrite ^(.*)\;jsessionid=(.*)$ $1 break;
        location /em/ {
                    proxy_pass http://localhost:17333/em/;
                    proxy_buffer_size 128k;
                    proxy_buffers   32 128k;
                    proxy_busy_buffers_size 128k;
        }
    }
PS: 17331: nginx监听端口, 17333: EM服务运行端口
2.启动nginx: sh em/scripts/nginx_start.sh

#修改EM参数
1.配置文件路径: em/config/jrescloud.properties
2.修改内容：
    ${em_host}:      本地服务IP，不能是 localhost 或 127.0.0.1
    ${install_path}: 解压目录内 em 文件夹全路径，$EM_HOME/em，注意要写到em目录   

    server.port:             EM服务端口，默认为 17333
    spring.redis.host:       redis服务IP,默认使用app.web.domain
    spring.redis.port:       redis服务端口，默认为 18331
    spring.redis.password:   redis服务密码，建议修改
    gotty.port:              gotty服务端口，默认为 18333
    
    #EM监控内置使用的DB,可根据需要调整
    dyn.spring.datasources[0].name=default
    dyn.spring.datasources[0].driverClassName=org.postgresql.Driver
    dyn.spring.datasources[0].url=jdbc:postgresql://${app.web.domain}:5434/em?ApplicationName=lightdbem&socketTimeout=5000&options=-c%20lock_timeout=30min
    dyn.spring.datasources[0].username=lightdb
    dyn.spring.datasources[0].password=PWDxxxxxxxxxxxxxxxxxxxxx

3.启动EM服务: sh em/scripts/em_start.sh
4.访问EM服务:  http://${em_host}:${em_port}/em/login.html
    不经过nginx: http://192.168.121.111:17333/em/login.html
     经过nginx: http://192.168.121.111:17331/em/login.html
    
5.默认用户名/密码: system/hs123456
```

#### 使用
可参考在线手册: http://www.light-pg.com/docs/LightDB_Enterprise_Manager_Guide_Manual/current/Enterprise-Manager.html#
