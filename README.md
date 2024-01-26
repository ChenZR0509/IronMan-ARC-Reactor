# 钢铁侠反应堆

------

{% note blue 'fas fa-bullhorn' simple %}

​	之前我购买了拓竹的A1 mini 3D打印机，官方客服送给我了两个盲盒，一个是引擎，我在MakerWorld上下载了对应的模型图纸并打印了出来，如图1、2、3所示。另一个LED小夜灯，我没有进行打印，一方面是因为整个模型单次打印时长过多，另一方面是因为需要使用透光材质。但是本着不能浪费的原则，我在盲盒给的零件基础上进行了改造，便制作了下面的反应堆模型。
{% endnote %}

![图1-引擎模型](./钢铁侠反应堆/001.jpg)

![图2-引擎模型](./钢铁侠反应堆/002.jpg)

![图3-引擎模型](./钢铁侠反应堆/003.jpg)

## 第一部分	模型展示

![图4-钢铁侠反应堆模型图](./钢铁侠反应堆/001.png)

## 第二部分	视频记录

## 第三部分	项目地址

{% note blue 'fas fa-bullhorn' simple %}

https://github.com/ChenZR0509/IronMan-ARC-Reactor.git

{% endnote %}

## 第四部分	Nano配置

- 登录账号：

  ```
  账号：root
  密码：fa
  ```

- 更新软件包：

  ```shell
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.old
  //打开/etc/apt/sources.list文件
  deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe 
  deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe 
  deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe 
  deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe 
  deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe 
  deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe 
  deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe 
  deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe 
  deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe 
  deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
  //删除原内容并复制上述内容然后保存
  sudo apt-get update
  sudo apt-get upgrade
  ```

- 配置NanoPi显示时间：

  ```shell
  NanoPi上电后发现其显示时间与本地时间相差八个小时，需要设置时间：
  timedatectl set-timezone Asia/Shanghai     >>>服务器时区设置
  date -s "2020-12-20 16:34:50"    >>>服务器时间设置
  ```

- 开启热点：

  ```shell
  cd /usr/local
  #如果git速度太慢的话可以用电脑翻墙下载，并使用winscp软件传递到NanoPi上
  git clone https://github.com/oblique/create_ap
  cd create_ap
  #编译
  make install
  #安装依赖库
  apt-get install util-Linux procps hostapd iproute2 iw haveged dnsmasq
  #查看网络配置
  ifconfig
  sudo nohup create_ap 无线网络 有线网络 WIFI显示名称 WIFI密码 &
  #可以查看输出文件nohup.out来判断热点的创建情况
  ```

- 部署博客：

  1. 安装nginx：

     ```shell
     sudo apt-get install nginx
     ```

  2. 安装gcc：

     ```shell
     sudo apt install build-essential
     ```

  3. 安装PCRE：

     ```shell
     cd /usr/local/
     wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
     tar -xvf pcre-8.37.tar.gz
     cd pcre-8.37
     ./configure
     make && make install
     //查看版本信息
     pcre-config --version
     ```

  4. 安装openssl：

     ```shell
     apt-get install openssl
     apt-get install libssl-dev
     ```

  5. 安装Zlib和libtool：

     ```shell
     sudo apt-get install zlib1g
     sudo apt-get install zlib1g.dev
     sudo apt-get install libtool
     ```

  6. 安装nginx：

     ```shell
     cd /usr/local/
     wget http://nginx.org/download/nginx-1.17.9.tar.gz
     tar -xvf nginx-1.17.9.tar.gz
     cd nginx-1.17.9
     ./configure
     make && make install
     ```

  7. 安装nodejs和npm：

     ```shell
     sudo apt install nodejs npm
     //查看版本信息
     nodejs --version
     npm -v
     ```

  8. 安装git：

     ```shell
     sudo apt-get install git
     adduser git	//按照提示要求输入相关信息：密码、用户名、手机等
     chmod 740 /etc/sudoers
     //使用WinSCP连接自己的服务器进入到/root/etc/sudoers
     //找到这个root	ALL=(ALL) 	ALL
     //在下面一行添加：
     git 	ALL=(ALL) 	ALL
     //设置文件权限
     chmod 400 /etc/sudoers
     //设置git密钥
     sudo passwd git
     ```

     ```shell
     //切换git用户
     su git
     cd ~
     mkdir .ssh
     cd .ssh
     //这里使用指令也可以使用WinSCP打开此文件进行操作
     //进入到文件内复制自己电脑C:\Users\15424\.ssh\id_rsa.pub文件内容
     vi authorized_keys
     //添加文件权限
     chmod 600 ~/.ssh/authorized_keys
     chmod 700 ~/.ssh
     ```

     ```shell
     cd ~
     git init --bare blog.git
     //打开此文件
     vi ~/blog.git/hooks/post-receive
     //在文件中添加
     git --work-tree=/home/www/website --git-dir=/home/git/blog.git checkout -f
     //添加文件权限
     chmod +x ~/blog.git/hooks/post-receive
     ```

  9. 博客部署：

     ```shell
     //进入root账户，在桌面创建博客文件夹：
     su root
     cd /home
     mkdir www
     cd www
     mkdir website
     修改文件夹权限
     chmod 777 /home/www/website
     chmod 777 /home/www
     ```

     ```shell
     //打开自己电脑的cmd，查看ssh连接是否成功
     ssh -v git@服务器的公网ip
     ```

     ```shell
     //打开vscode打开hexo初始化的blog文件夹修改本地配置文件：
     deploy:
       type: git
       repo: git@NanoPi的IP地址:/home/git/blog.git
       branch: master
     ```

     ```shell
     //在之前的博客中nginx配置conf文件不知道为什么在NanoPi上一直部署不成功，显示Nginx的欢迎界面
     //所以直接修改/etc/nginx/sites-enabled/default文件
     //在其中找到root /var/www/html;并注释掉
     //添加下面的内容：
     root /home/www/website;
     ```

     ```shell
     //在/root/etc/rc.0/init.d/添加nginx的脚本文件：
     //新建nginx
     #!/bin/bash
     #Startup script for the nginx Web Server
     #chkconfig: 2345 85 15
     nginx=/usr/local/nginx/sbin/nginx
     conf=/usr/local/nginx/conf/nginx.conf
     case $1 in 
     start)
     echo -n "Starting Nginx"
     $nginx -c $conf
     echo " done."
     ;;
     stop)
     echo -n "Stopping Nginx"
     killall -9 nginx
     echo " done."
     ;;
     test)
     $nginx -t -c $conf
     echo "Success."
     ;;
     reload)
     echo -n "Reloading Nginx"
     ps auxww | grep nginx | grep master | awk '{print $2}' | xargs kill -HUP
     echo " done."
     ;;
     restart)
     $nginx -s reload
     echo "reload done."
     ;;
     *)
     echo "Usage: $0 {start|restart|reload|stop|test|show}"
     ;;
     esac
     ```

     ```
     //相关控制命令
     启动service nginx start
     停止service nginx stop
     重启service nginx reload
     ```

