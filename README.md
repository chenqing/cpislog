cpislog
=======

基于时间的日志分析程序

squid中的cpislog应用举例
========================

>1、统计一段时间的本机的流量，注意这里是大B，和mrtg的存在一个8倍的关系

#####[root@host ~]# cpislog  --start_time 20121212120000 --end_time 20121212121500  -i 5 
12月12日12:0:0 到 12月12日12:4:59 流量是: 3.38GB 
12月12日12:5:0 到 12月12日12:9:59 流量是: 3.37GB 
12月12日12:10:0 到 12月12日12:14:59 流量是: 3.56GB 

>2、统计一段时间的本机的流量，某个url的流量

#####[root@host ~]# cpislog  --start_time 20121212120000 --end_time 20121212121200  -i 4 -u storage3.cdn.kugou.com
12月12日12:0:0 到 12月12日12:3:59 流量是: 7.55MB 
12月12日12:4:0 到 12月12日12:7:59 流量是: 2.52MB 
12月12日12:8:0 到 12月12日12:11:59 流量是: 4.15MB 
>3、直接输出一段时间的日志，自己利用awk，grep进行过滤

#####[root@host ~]# cpislog  --start_time 20121212120000 --end_time 20121212121200|grep storage3.cdn.kugou.com|grep TCP_HIT/206
1355284934.112    161 10.244.220.32 TCP_HIT/206 33083 GET   http://storage3.cdn.kugou.com/201212121202/78bc3491f325c7e8da0aefd2c6ea3b84/M00/00/1D/OtfxzE5rogfGRnJCAF2iq7aIwZ4827.mp3  - DIRECT/115.239.249.144 audio/mpeg "-" "-" -
1355284998.698    404 117.136.20.69 TCP_HIT/206 8468 GET http://storage3.cdn.kugou.com/201212121159/f3f240196d88e71b978a07b465085549/M00/32/AD/OtfxzE6IZSerdlYlAAlvFYooE7E143.m4a  - DIRECT/122.228.72.8 application/octet-stream "-" "-" -
1355285023.078    382 117.136.20.76 TCP_HIT/206 8517 GET http://storage3.cdn.kugou.com/201212121203/0dd3c6de39c6c95754531adbf9217132/M00/0D/30/e4Olpk6SxdOb28clABHXvld_R-o934.m4a  - DIRECT/115.239.249.144 application/octet-stream "-" "-" -

#####[root@CMN-HF-1-3O1 ~]# cpislog  --start_time 20121212134000 |grep storage3.cdn.kugou.com|grep TCP_HIT/206
1355290832.565    274 221.130.131.228 TCP_HIT/206 16715 GET http://storage3.cdn.kugou.com/201212121340/15b451ea508891e892271cfdd8cb146b/M01/04/DE/OtfxzE5uBtPmvLBHABD2dNSlnB4117.m4a  - DIRECT/122.227.245.90 application/octet-stream "-" "-" -
1355291023.142  38858 117.136.20.66 TCP_HIT/206 16712 GET http://storage3.cdn.kugou.com/201212121342/4b27e91fbba63fd3c6ee36dfb0eaf452/M01/26/01/OtfxzE6B4i_OUco1ABLgV2us_b8496.m4a  - DIRECT/122.228.72.8 application/octet-stream "-" "-" -
1355291023.167  42797 117.136.20.66 TCP_HIT/206 16703 GET http://storage3.cdn.kugou.com/201212121342/4b27e91fbba63fd3c6ee36dfb0eaf452/M01/26/01/OtfxzE6B4i_OUco1ABLgV2us_b8496.m4a  - DIRECT/122.227.245.90 application/octet-stream "-" "-" -

#####[root@CMN-HF-1-3O1 ~]# cpislog  --range_time 10m |grep storage3.cdn.kugou.com|grep TCP_HIT/206

1355290713.253    242 211.141.204.113 TCP_HIT/206 33085 GET http://storage3.cdn.kugou.com/201212121334/12c019d00a685a3250cb058a10ce12cf/M00/0C/93/OtfxzE5whpnee2bQAFzf5sB-ETQ792.mp3  - DIRECT/115.239.249.144 audio/mpeg "-" "-" -
1355290832.565    274 221.130.131.228 TCP_HIT/206 16715 GET http://storage3.cdn.kugou.com/201212121340/15b451ea508891e892271cfdd8cb146b/M01/04/DE/OtfxzE5uBtPmvLBHABD2dNSlnB4117.m4a  - DIRECT/122.227.245.90 application/octet-stream "-" "-" -
/4b27e91fbba63fd3c6ee36dfb0eaf452/M01/26/01/OtfxzE6B4i_OUco1ABLgV2us_b8496.m4a  - DIRECT/122.227.245.90 application/octet-stream "-" "-" -
>4、结合gb命令进行过滤和排序

#####[root@host ~]# cpislog  --start_time 20121212134000 |gb -f4 -r
  84701 TCP_HIT/200               
  73122 TCP_MISS/200              
  72168 TCP_MEM_HIT/200           
  14202 TCP_REFRESH_HIT/200       
  11115 TCP_MISS/206              
   6040 TCP_REFRESH_MISS/200      
   4827 TCP_MISS/302              
   3426 TCP_MISS/404              
   2250 TCP_HIT/206               
   2076 TCP_IMS_HIT/304           
    829 TCP_MISS/503              
    812 TCP_MISS/304              
    790 TCP_MISS/504              
    574 TCP_MISS/301              
    560 TCP_MISS/403              
    558 TCP_MEM_HIT/301           
    515 TCP_DENIED/404            
    445 TCP_MISS/204              
    285 TCP_HIT/302               
    244 TCP_MISS/000              
    180 TCP_REFRESH_MISS/302      
    178 TCP_HIT/404               
    169 TCP_MEM_HIT/302           
    137 TCP_HIT/301               
     92 TCP_MISS/500              
     88 TCP_REFRESH_MISS/301      
     59 TCP_REFRESH_HIT/304       
     54 TCP_REFRESH_MISS/404      
     38 TCP_HIT/000               
 
