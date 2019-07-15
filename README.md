# orange_install
# orange 0.7 安装

## 安装lor
* lor项目地址:https://github.com/sumory/lor

        git clone https://github.com/sumory/lor
        cd lor
        make install
        make install LOR_HOME=/path/to/lor LORD_BIN=/path/to/lord

## 安装orange
### 前置条件：
* openresty已安装：安装目录/home/openresty
* luarocks已安装：默认安装目录，/usr/local/luarocks。注意：luarocks2.2.2+以上版本

        wget https://luarocks.github.io/luarocks/releases/luarocks-3.1.3.tar.gz
        tar -xzf luarocks-3.1.3.tar.gz
        cd luarocks-3.1.3
        find / -name "lua.h"
        ./configure --prefix=/usr/local/luarocks --with-lua-include=/home/openresty/luajit/include/luajit-2.1
        make
        make install

### 安装orange
    git clone https://github.com/orlabs/orange.git
    cd orange
    /home/openresty/bin/opm --install-dir=./ get zhangbao0325/orangelib    //opm download the 3rd packages
    /usr/local/luarocks/bin/luarocks install luafilesystem
    /usr/local/luarocks/bin/luarocks install luasocket
    cd conf
    cp orange.conf.example orange.conf
    cp nginx.conf.example nginx.conf
    make install
    /home/openresty/nginx/sbin/nginx -p /usr/local/orange/ -c /usr/local/orange/conf/nginx.conf

## 注意事项
* 1、sh start.sh之前，将/orange/install/下的.sql导入数据库，修改orange.conf，修改对应的数据库信息
* 2、Can't locate Digest/MD5.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at /home/openresty/bin/opm line 16. BEGIN failed--compilation aborted at /home/openresty/bin/opm line 16. 

        yum -y install perl perl-devel
        yum -y install perl-Digest-MD5
        
* 3、nginx: [error] init_by_lua error: ./lualib/resty/pl/path.lua:28: pl.path requires LuaFileSystem
在nginx.conf中，引入lua_package路径：
       · lua_package_cpath '/usr/local/luarocks/lib/lua/5.1/?.so;/usr/local/luarocks/lib/lua/5.1/socket/?.so;/usr/local/luarocks/lib/lua/5.1/mime/?.so;;'; ·
       · lua_package_path    './lualib/?.lua;./lualib/resty/?.lua;/usr/local/orange/?.lua;/usr/local/lor/?.lua;/usr/local/luarocks/share/lua/5.1/?.lua;;'; ·

* 4、make install之后，可以直接sh start.sh运行，不过指定的是当前目录下的配置。个人喜欢指定安装后的目录
