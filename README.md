参考：
http://www.rabbitmq.com/which-erlang.html
https://www.jianshu.com/p/d7ba6df94a26
https://blog.csdn.net/Dream_ya/article/details/84570199

清除之前的记录
rm -rf /usr/lib64/erlang
rm -rf /var/lib/rabbitmq
rpm -e rabbitmq-server

CentOS 7 的下载安装：
http://www.unixodbc.org/unixODBC-2.3.7.tar.gz   ./configure --prefix=/usr/local
http://ftp.gnu.org/gnu/ncurses/ncurses-6.0.tar.gz  ./configure --prefix=/usr/local
http://distfiles.macports.org/openssl/openssl-1.0.2.tar.gz  ./config --prefix=/usr/local/ssl    Makefile中CFLAG选项加上-fPIC
http://erlang.org/download/otp_src_R16B03.tar.gz  ./configure --prefix=/usr/local/erlang --with-ssl=/usr/local/ssl --enable-threads --enable-smp-support --enable-kernel-poll --enable-hipe --without-javac
https://www.rabbitmq.com/releases/rabbitmq-server/v3.6.10/rabbitmq-server-3.6.10-1.el6.noarch.rpm  rpm -i --nodeps rabbitmq-server-3.6.10-1.el6.noarch.rpm

准备：
mkdir -p /var/lib/rabbitmq
chown -R rabbitmq:rabbitmq /var/lib/rabbitmq

在/etc/profile里面添加
export PATH=$PATH:/usr/local/erlang/bin
source /etc/profile

启动：rabbitmq-server -detached

开启后台插件：
rabbitmq-plugins enable rabbitmq_management 

添加用户：rabbitmqctl add_user {username} {password} 
删除用户：rabbitmqctl delete_user {username} 
修改密码：rabbitmqctl change_password {username} {newpassword} 
设置用户角色：rabbitmqctl set_user_tags {username} {tag} 
查看用户：rabbitmqctl list_users
tag可以为administrator, monitoring, management 
举例： 
rabbitmqctl add_user hgame 123456 
rabbitmqctl set_user_tags hgame administrator 
rabbitmqctl list_users

vhost管理
添加vhost: rabbitmqctl add_vhost {name} 
删除vhost: rabbitmqctl delete_vhost {name}
举例： 
rabbitmqctl add_vhost hgame

设置权限
rabbitmqctl set_permissions -p {vhost_name} {username} '.*' '.*' '.*' 
举例： 
rabbitmqctl set_permissions -p hgame hgame '.*' '.*' '.*' 

查看权限
rabbitmqctl list_user_permissions {username}
rabbitmqctl list_permissions -p {vhost_name}

// 清除权限
rabbitmqctl clear_permissions [-p VHostPath] User


示例：
 rabbitmqctl list_users
 rabbitmqctl delete_user guest
 rabbitmqctl list_vhosts   
 rabbitmqctl delete_vhost /
 
 rabbitmqctl add_user hgame 123456
 rabbitmqctl add_vhost hgame
 rabbitmqctl set_user_tags hgame administrator 
 rabbitmqctl set_permissions -p hgame hgame '.*' '.*' '.*' 


BUG:
RabbitMQ（3.6.15）使用之前出现负载高了偶现挂掉，导致服务不可用的情况。经日志分析发现大量短连接，查阅官方文档发现是erlang（18.1）的BUG，官方已在19、20已经修复。
官方bug https://groups.google.com/forum/#!searchin/rabbitmq-users/connection_closed%7Csort:date/rabbitmq-users/UGQbdvM1rTA/t4-q6Z27BAAJ
https://bugs.erlang.org/browse/ERL-430
