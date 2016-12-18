#Linux下php环境安装,这里仅供自己参考

##安装Apache 使用yum安装

    * yum install httpd -y
    
##安装iptables 防火墙 使用yum安装
    
    * yum install iptables -y 客户端
    * yum install iptables-service -y 服务器端

##安装php5.6 因为考虑到yum安装版本太低 用源码安装

    下载源码编译文件
        * wget http://mirrors.sohu.com/php/php-5.6.9.tar.xz (这里用的是搜狐的镜像文件)
    解压
        * tar xf php-5.6.9.tar.xz
    安装c编译库
        * yum install gcc gcc-c++ -y
    安装php依赖的包​​
        * cd php-5.6.9
        * ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --enable-pcntl --enable-mysqlnd --enable-opcache --enable-sockets --enable-sysvmsg --enable-sysvsem  --enable-sysvshm --enable-shmop --enable-zip --enable-ftp --enable-soap --enable-xml --enable-mbstring --disable-rpath --disable-debug --disable-fileinfo --with-mysql --with-mysqli --with-pdo-mysql --with-pcre-regex --with-iconv --with-zlib --with-mcrypt --with-gd --with-openssl --with-mhash --with-xmlrpc --with-curl --with-imap-ssl
        
        如果报错 configure: error: xml2-config not found. Please check your libxml2 installation.
        需要安装 libxml2
        * yum install libxml2 -y
        * yum install libxml2-devel -y
        重新执行 php依赖包
        
        再次报错 configure: error: Cannot find OpenSSL's <evp.h>
        需要安装 openssl
        * yum install openssl openssl-devel -y
        重新执行 php依赖包
        
        再次报错 configure: error: Please reinstall the libcurl distribution -easy.h should be in <curl-dir>/include/curl/
        curl的dev包没有安装
        * yum install libcurl-devel -y
        重新执行 php依赖包
        
        再次报错 configure: error: png.h not found.
        * yum install libpng-devel -y
        重新执行 php依赖包
        
        再次报错configure: error: mcrypt.h not found. Please reinstall libmcrypt.
        * yum install libmcrypt-devel -y
        重新执行 php依赖包
        
        基本不报错了 但是还可以在安装一些
        * yum install libjpeg-devel freetype-devel -y
        重新执行 php依赖包
        
        也可以一次性安装全部需要的扩展 (注意用这个将意味着上面的报错会不存在)
        * yum install libxml2 libxml2-devel openssl openssl-devel libcurl-devel libpng-devel libmcrypt-devel libjpeg-devel freetype-devel -y
        重新执行 php依赖包
        
        
        直到出现
        Thank you for using PHP.
        
        config.status: creating php5.spec
        config.status: creating main/build-defs.h
        config.status: creating scripts/phpize
        config.status: creating scripts/man1/phpize.1
        config.status: creating scripts/php-config
        config.status: creating scripts/man1/php-config.1
        config.status: creating sapi/cli/php.1
        config.status: creating sapi/fpm/php-fpm.conf
        config.status: creating sapi/fpm/init.d.php-fpm
        config.status: creating sapi/fpm/php-fpm.service
        config.status: creating sapi/fpm/php-fpm.8
        config.status: creating sapi/fpm/status.html
        config.status: creating sapi/cgi/php-cgi.1
        config.status: creating ext/phar/phar.1
        config.status: creating ext/phar/phar.phar.1
        config.status: creating main/php_config.h
        config.status: main/php_config.h is unchanged
        config.status: executing default commands
        总算成功
        
        开始安装
            * make
        出现这个 Don't forget to run 'make test'. 没什么问题
            * make install
        完成安装
        
        下一步 把php的配置文件修改到 安装目录下面 并且修改名称为php.ini
            * cp php.ini-production /usr/local/php/etc/php.ini​
        
        当我们使用nginx还要把php-fpm.conf复制到/usr/local/php/etc/里头
            * cp /root/php-5.6.9/sapi/fpm/php-fpm.conf /usr/local/php/etc/
        
        将php-fpm作为server服务 (/root/php-5.6.9/是php的源码编译后的文件夹)
            * cp /root/php-5.6.9/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
        
        设置权限，并添加服务
            * chmod +x /etc/init.d/php-fpm
            * chkconfig --add php-fpm
        
        以后可以使用如下命令管理php-fpm了
        
            * service php-fpm start
            
            * service php-fpm stop
            
            * service php-fpm restart
            
            * service php-fpm reload
        
        为了方便把php设置成为全局变量
            方法:
                全局的:
                    cd / 到这个目录编辑/etc/profile文件
                    * cd /
                    * cd /etc
                    * vim profile
                    加上
                    export PATH=$PAHT:/usr/server/php/bin
                    保存退出
                    执行
                    * source /etc/profile 
                局部的:
                    * cd /
                    * cd /home/
                    * cd /lmx/ (这是例子 lmx 是你的用户名)
                    * ls -a 查看所有文件，包含隐藏的文件
                    * vim bash_profile (这个是局部的,)
                    加上
                    export PATH=$PATH:/usr/local/php/bin
                    保存退出
                    执行 
                    * source .bash_profile
                    
                就OK了
                
                
                
    
        
        
            