# Apache 因信号量爆满而导致的无法启动
 看error log有报错，提示
 ![1.png](https://raw.githubusercontent.com/yangzhihuimacpro/local_test/master/image001.png)
 
 查看磁盘空间，不管是存储还是I节点，都没问题
 
 ![2.png](https://raw.githubusercontent.com/yangzhihuimacpro/local_test/master/image003.gif)
 
 由于apache的其他原因，如非正常的重启或关闭，导致信道被占满;
 查看信道使用情况:
  ![3.png](https://raw.githubusercontent.com/yangzhihuimacpro/local_test/master/image004.gif)
  
  最后就是需要删除这些没有释放的信道
CentOS释放信道命令：

            for i in `ipcs -s|grep 'apache'|awk -F' ' '{print $2}'`;do (ipcrm -s $i); done
非CentOS可以使用：

                for i in `ipcs -s | awk '/httpd/ {print $2}'`; do (ipcrm -s $i); done
重启Apache成功！查看信道信息
                
                ipcs –s
