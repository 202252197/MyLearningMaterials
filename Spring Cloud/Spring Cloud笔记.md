//下载redis安装包
wget http://download.redis.io/releases/redis-3.0.6.tar.gz
//解压
tar zxvf redis-3.0.6.tar.gz
//跳转到 redis目录
cd redis-3.0.0
//编译安装
make MALLOC=libc