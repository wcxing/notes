
- sh build.sh $branch $partner $classtype

运行项目目录下 build.sh 文件；

- rsync -avz --delete build/static/*  worker@172.21.138.18:/apps/webroot/repos/wenzai-fe/live-pc-student/immerse/$partner

同步文件到服务器机器上，

- ssh worker@172.21.138.18 "ln -s /apps/webroot/repos/wenzai-fe/live-pc-student/immerse/$partner /apps/webroot/production/room/live-pc-student/immerse/$partner"

创建一个软链接