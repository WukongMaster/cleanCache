
<p>登录VPS的SSH之后，执行如下代码后reboot重启服务器<br />
第一步：创建文件夹和文件名：<br />
mkdir -p /opt/script/cron &amp;&amp; vim /opt/script/cron/cleanCache.sh<br />
输入如下文字，之后":wq"保存退出</p>
<p><img class="alignnone size-full wp-image-43182" title="03e786e47f606901cb7e9ec90d244ff5" src="https://linuxword.com/wp-content/uploads/2025/03/03e786e47f606901cb7e9ec90d244ff5.png" alt="03e786e47f606901cb7e9ec90d244ff5" width="605" height="325" /><br />
#!/bin/bash<br />
#description: 清除缓存<br />
echo "开始清除缓存"<br />
sync;sync;sync #写入硬盘，防止数据丢失<br />
chmod -R 777 /opt/script/cron #修改其文件的權限<br />
chmod -R 777 /var/spool/mail #修改其郵件消息的權限<br />
#sleep 10 #延迟10秒<br />
echo 1 &gt; /proc/sys/vm/drop_caches<br />
echo 2 &gt; /proc/sys/vm/drop_caches<br />
echo 3 &gt; /proc/sys/vm/drop_caches<br />
echo "结束清除缓存"<br />
#description: 删除30天之前的r日志文件<br />
echo "删除30天之前的r日志文件"<br />
find /var/log -mtime +1 -type f -name \*.log | xargs rm -f<br />
echo "30天之前的r日志文件删除完毕"</p>
<p>第二步：修改权限：<br />
chmod -R 777 /opt/script/cron<br />
第二步：将自动执行CRON的命令放入到计划启动中：<br />
crontab -e<br />
*/9 * * * * sh /opt/script/cron/cleanCache.sh<br />
：wq 保存退出，输入reboot 重启，你会发现自动清理了缓存占用了太多的内存，从此以后你的服务器的内存高枕无忧，再怎么折腾都够使用了</p>


或者直接使用如下命令：
# Cron Cleanup Script
一键脚本用于清理缓存和日志，支持 CentOS、Ubuntu、Debian。

## 在线安装
在SSH中运行以下命令：

bash <(curl -sL https://raw.githubusercontent.com/FoxBary/smallvps/main/vmshellvps.sh)
