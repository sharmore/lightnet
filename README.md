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
