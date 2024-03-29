# DenyHosts

dpkg --configure -a

sudo apt-get install denyhosts


service denyhosts start
 Usage: /etc/init.d/denyhosts {start|stop|restart|force-reload|status}       
 
 
 
 你可以前往官网http://sourceforge.net/projects/denyhost/ 选择一个你喜欢的版本下载，或者直接运行下面的命令下载。
cd /tmp/ && wget http://jaist.dl.sourceforge.net/project/denyhost/denyhost-2.10/denyhosts-2.10.tar.gz
#解压
tar xzvf denyhosts-2.10.tar.gz
#切换到目录
cd denyhosts
#安装
sudo python setup.py install
配置
经过上面这几步，基本已经完成了denyhosts 的安装，下面我们还需要进行一些配置。
#复制daemon文件到init.d 目录
sudo cp /usr/local/bin/daemon-control-dist /etc/init.d/denyhosts
sudo vim /etc/init.d/denyhosts
#A:将打开的文件里面的DENYHOSTS_BIN 修改为你自己denyhosts的目录，一般应该是/usr/local/bin/denyhosts.py
#B:或者是建立一个软连接
ln -s /usr/local/bin/denyhosts.py /usr/sbin/denyhosts

#注意：A或者B只需要执行一个就行了！</span></span>
现在denyhosts 就已经安装配置好了，我们可以先把我们自己的IP加到系统的白名单里面
#编辑白名单

sudo vim /etc/hosts.allow 

#按下面的样例添加你的IP

# /etc/hosts.allow: list of hosts that are allowed to access the system.
# See the manual pages hosts_access(5) and hosts_options(5).
 #
 # Example: ALL: LOCAL @wangheng.org
 # ALL: .wangheng.org EXCEPT terminalserver.wangheng.org
 #
 # If you’re going to protect the portmapper use the name “rpcbind” for the
 # daemon name. See rpcbind(8) and rpc.mountd(8) for further information.
 #
 sshd: 123.123.123.123
编辑保存后，我们需要重启一下服务：

sudo /etc/init.d/denyhosts restart





安装

# aptitude install denyhosts
配置

# vim /etc/denyhosts.conf
# 用户登录的日志文件
SECURE_LOG = /var/log/secure
# 禁止登陆的主机文件
HOSTS_DENY = /etc/hosts.deny
# 清除已禁止主机的时间，y年，w星期，d天
PURGE_DENY = 2w
# 禁止的服务名
BLOCK_SERVICE = sshd
# 允许无效用户登录失败的次数
DENY_THRESHOLD_INVALID = 1
# 允许普通用户登陆失败的次数(root用户除外)，用户数据文件/etc/passwd
DENY_THRESHOLD_VALID = 2
# 允许 root 用户登陆失败的次数
DENY_THRESHOLD_ROOT = 1
# 限制restricted-usernames文件列表中用户登录失败的次数
DENY_THRESHOLD_RESTRICTED = 5
# DenyHosts 数据保存目录
WORK_DIR = /usr/share/denyhosts/data
SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS=YES
# 是否做域名反解
HOSTNAME_LOOKUP=YES
#程序的进程ID 当DenyHOts启动的时候写入 pid，已确保服务正确启动，防止同時启动多个服务
LOCK_FILE = /var/run/denyhosts.pid
# 管理员邮件地址
ADMIN_EMAIL = admin@domain.com
# 如果设置了 ADMIN_EMAIL 下面就要设置 smtp 的 host
SMTP_HOST = localhost
SMTP_PORT = 25
# 发信的 header
SMTP_FROM = DenyHosts <nobody@localhost>
# 发信标题 如果你有N台主机，需要修改发信标题，来识别来自那台机器的信
SMTP_SUBJECT = DenyHosts Report
# 用户的失败登录计数重置为0的时间(/etc/passwd)
AGE_RESET_VALID=5d
# root用户失败登录计数重置为0的时间
AGE_RESET_ROOT=25d
#用户的失败登录计数重置为0的时间(/usr/share/denyhosts/data/restricted-usernames)
AGE_RESET_RESTRICTED=25d
# 无效用户的失败登录计数重置为0的时间(/etc/passwd)
AGE_RESET_INVALID=10d
# DenyHosts 的日志文件
DAEMON_LOG = /var/log/denyhosts
以上根据自己的需要修改即可，一般主要修改三种用户登录失败的次数，还有管理员的邮件地址即可，其他的使用系统默认就行。
配置完重启使服务生效
/etc/init.d/denyhosts restart
