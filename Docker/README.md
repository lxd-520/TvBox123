#    Docker搭建本地直播源项目备忘




#    //  什么是 youshandefeiyang/allinone ?

allinone 是斗鱼，虎牙，抖音等直播平台的直播源代理程序，可以直接观看多个流畅完美满血 4K 源。除了 Docker 外，还提供了主流平台的二进制文件，包括 exe 格式。

#   //   Docker/DockerHub 国内镜像源/加速列表[整理分享](https://www.xuxlc.cn/article/details-40.html)


Docker镜像加速，在原镜像地址前加[ docker.1ms.run/ ]


#  //    Docker安装allinone采集直播源


docker run -d \
   --restart unless-stopped \
   --name allinone \
   --net=host \
   --privileged=true \
   -p 35455:35455 \
   youshandefeiyang/allinone


##   内置接口如下：

|  直播源   | 接口  |
|  ----  | ----  |
| TPTV  | http://192.168.0.236:35455/tptv.m3u |
| 聚合TV  | http://192.168.0.236:35455/tv.m3u |
| YY 轮播  | http://192.168.0.236:35455/yylunbo.m3u | 
| 虎牙一起看  | http://192.168.0.236:35455/huyayqk.m3u |   
| 斗鱼一起看  | http://192.168.0.236:35455/douyuyqk.m3u | 
| BiliBili 生活  | http://192.168.0.236:35455/bililive.m3u |




#  //	进阶1：配置 watchtower 每天早上五点自动监听allinone直播源更新

  
docker run -d \
   --name watchtower \
   --restart unless-stopped \
   -v /var/run/docker.sock:/var/run/docker.sock \
   containrrr/watchtower \
   allinone -c --schedule "0 0 5 * * *"



#  //	进阶2：直播源重新分组并转换TXT格式


docker run -d \
   --restart=always \
   --name allinone_format \
   -p 35456:35456 \
   yuexuangu/allinone_format:latest




##	格式：http://【群晖IP】:35456/tv.php?h=【allinoneIP】&p=【allinonePort】&m=1&t=0

##	例：{M3U格式}(http://192.168.0.236:35456/tv.php?h=192.168.0.236&p=35455&m=1&t=0)


##	例：{TXT格式}(http://192.168.0.236:35456/tv.php?h=192.168.0.236&p=35455&m=1&t=1)











