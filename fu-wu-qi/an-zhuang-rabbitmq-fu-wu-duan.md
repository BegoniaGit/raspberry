# 安装 RabbitMQ 服务端

需要先安装 Erlang 我安装GitHub最新版： 

1.  erlang-22.3.2-1.el6.x86\_64.rpm 
2.  rabbitmq-server-3.8.3-1.el7.noarch.rpm 
3. 开启 WEB UI 管理`rabbitmq-plugins enable rabbitmq_management` 
4. 添加新用户`rabbitmqctl add_user <username> <password>` 
5. 添加管理员权限`rabbitmqctl set_user_tags <username> administrator` 

![&#x9875;&#x9762;](https://shaosim-image.oss-cn-chengdu.aliyuncs.com/20200421152349.png)

