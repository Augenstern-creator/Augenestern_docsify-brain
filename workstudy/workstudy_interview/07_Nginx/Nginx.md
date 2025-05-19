



# 1、目录规范

- 安装目录： /approot/应用简称/nginx
- 日志目录： /approot/应用简称/nginx



# 1、全局配置



```bash
# 全局配置

# 设置worker进程的用户,默认为 nobody
user  nobody;

# worker进程工作数设置,一般来说CPU有几个,就设置几个,或者设置为N-1也行,默认为1,建议为auto
worker_processes  auto;
# 每个worker进程可以打开的最大文件句柄数
worker_rlimit_nofile 262144


# nginx 日志级别: debug | info | notice | warn | error | crit | alert | emerg
error_log  logs/error.log;
error_log  logs/error.log  notice;
error_log  logs/error.log  info;

# 设置 nginx 进程pid
pid        logs/nginx.pid;


# 事件:包括最大连接数、线程数等等
events {
    # 默认使用epoll,建议使用异步IO模式
    use epoll;
    # 每个worker允许连接的客户端最大连接数
    worker_connections  65535;
}


# http配置
http {
	# http的全局配置
	# include 引入外部配置，提高可读性，避免单个配置文件过大
    include       /approot/应用简称/nginx/conf/mime.types;
    default_type  application/octet-stream;
    charset utf-8;

	# 设定日志格式，main为定义的格式名称，如此 access_log 就可以直接使用这个变量了
	# $remote_addr:客户端ip   $remote_user:远程客户端用户名
	# $time_local:时间和时区  $request:请求的url以及method
    # $status:响应状态码     $body_bytes_sent:响应客户端内容字节数
    # $http_referer:记录用户从哪个链接跳转过来的
    # $http_user_agent:用户所使用的代理，一般来时都是浏览器
    # $http_x_forwarded_for:通过代理服务器来记录客户端的ip
    
    # 指定访问日志格式(各变量之间用#_#分隔，该格式可以与日志中心兼容)
    log_format  main  '$time_local#_#'
    '$http_x_forwarded_for#_#remote_user#_#$remote_addr#_#'
    '$request_time#_#requesr_length#_#'
    '$upstream_addr#_#$upstream_response_time#_#upstream_response_length#_#'
    '$status#_#body_bytes_sent#_#$http_host#_#'
    '$request#_#http_referer#_#http_user_agent#_#server_port'

    # 指定访问日志格式、日志格式 
    access_log  access_log/applog/应用简称/nginx/access.log  main;
    error_log  error_log/applog/应用简称/nginx/error_log  main;


    # sendfile使用高效文件传输，提升传输性能。
    # 启用后才能使用tcp_nopush，是指当数据表累积一定大小后才发送，提高了效率
    # 对于普通应用，必须设为on.如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络IO处理速度。 
    sendfile        on;    # 高效文件传输
    tcp_nopush     on; # tcp批量发送(与sendfile联用)
    tcp_nodelay on;   # tcp包尽快发送(与keepalive联用)
    # keepalive_timeout设置客户端与服务端请求的超时时间，保证客户端多次请求的时候不会重复建立新的连接，节约资源损耗。
    keepalive_timeout  1000;

    # 打开文件元信息的缓存
    open_file_cache max=100000 inactive=20s;
    open_file_cache_valid 30s;       # 多长时间检查一次文件云信息缓存是否失效
    open_file_cache_min_uses 2;   # 打开文件元信息进入缓存的最小使用次数   


    # gzip启用压缩，html/js/css压缩后传输会更快
    gzip  on;
    gzip_min_length 1k;    # 启动压缩的最小文件大小
    gzip_buffers 4 16k;
    gzip_http_version 1.0; # 压缩支持的最小http版本
    gzip_comp_level 2; # 压缩级别
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript;
    gzip_vary on;
    gzip_disable "MSIE[1-6]\."

    # 服务配置
    server {
        # listen 监听端口
        listen       80;
        # 服务器名称,默认为localhost
        server_name  localhost;


	#访问地址 / 表示根目录
        location / {
            # root 表示访问的文件夹,其中默认是html文件夹,可以修改成自己的文件夹
            root   html;
            # index 访问页面,默认访问页面
            index  index.html index.htm;
        }

        
        # 错误页面的配置
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }


}
```





















































































