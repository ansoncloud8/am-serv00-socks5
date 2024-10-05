# [am-serv00-socks5](https://github.com/amclubs/am-serv00-socks5)
这个项目的脚本安装管理并运行一个socks5代理,替换CF的PROXYIP,解决访问ChatGPT、TikTok等问题,不再烦恼优选PROXYIP

#
▶️ **新人[YouTube](https://youtube.com/@AM_CLUB)** 需要您的支持，请务必帮我**点赞**、**关注**、**打开小铃铛**，***十分感谢！！！*** ✅
</br>🎁 不要只是下载或Fork。请 **follow** 我的GitHub、给我所有项目一个 **Star** 星星（拜托了）！你的支持是我不断前进的动力！ 💖
</br>✅**解锁更多技术请点击进入 YouTube频道[【@AM_CLUB】](https://youtube.com/@AM_CLUB) 、[【个人博客】](https://am.809098.xyz)** 、TG群[【AM科技 | 分享交流群】](https://t.me/AM_CLUBS) 、获取免费节点[【进群发送关键字: 订阅】](https://t.me/AM_CLUBS)



# 免费serv00服务器一键脚本部署socks5
- 部署视频教程：[小白教程](https://youtu.be/Bw82BH_ecC4)

# 部署教程：

## 一、需要准备的前提资料
### 1、首先注册一个Serv00账号，建议使用gmail邮箱注册，注册好会有一封邮箱上面写着你注册时的用户名和密码
- 注册帐号地址：https://serv00.com
<center>注册帐号请查看下面视频</center>
<center><a href="https://youtu.be/NET1FTlfDTs">[点击观看视频教程]</a></center>

![image](https://github.com/user-attachments/assets/b3b3733b-3553-45dd-9346-c4664251755f)
  
### 2、加下群发送关键字 ssh 获取连接工具
Telegram频道：https://t.me/AM_CLUBS

## 二、安装前需准备的初始设置
- 1、登入邮件里面发你的 DevilWEB webpanel 后面的网址，进入网站后点击 Change languag 把面板改成英文
- 2、然后在左边栏点击 Additonal services ,接着点击 Run your own applications 看到一个 Enable 点击
- 3、找到 Port reservation 点击后面的 Add Port 新开一个端口，随便写，也可以点击 Port后面的 Random随机选择Port tybe 选择 TCP
- 4、然后点击 Port list 你会看到一个端口
  <img width="1264" alt="socks5-1" src="https://github.com/user-attachments/assets/3ceba2fa-a03f-416a-ba4d-5b86288f9561">

- 5、 启用管理权限：
<img width="719" alt="serv00" src="https://github.com/user-attachments/assets/48466f3a-1b75-4cf3-8dd9-7c2e440b73fe">

***完成此步骤后，退出 SSH 并再次登录。***

## 三、开始安装部署

- 1、用我们前面下载的工具登入SSH(有些工具 第一次连接还是会弹出输出密码记得点X 然后再添加密码 )
使用 serv00 通过电子邮件发送给您的信息。
```
ssh <username>@<panel>.serv00.com
```

- 2、进入到面板后复制下面代码到面板安装
### nohup模式(一键安装 **新手小白用这个！**)
```
bash <(curl -s https://raw.githubusercontent.com/amclubs/am-serv00-socks5/main/install-socks5.sh)
```

- 3、根据脚本的提示信息进行操作
  
设置socks5变量需要你输入3个变量
```
socks5的端口(在面板里开放的端口)
socks5的用户名(自己定义)
socks5的密码(自己定义)
```
安装nezha-agent(如果已安装过就不用安装,填N)
```
是否安装 nezha-agent (Y/N 回车N)
```
crontab定时任务(建议添加,填Y)
```
是否添加 crontab 守护进程的计划任务(Y/N 回车N)
```

成功运行并启动socks5代理后，脚本会提示“代理工作正常，脚本结束“

- 4、查看保活crontab任务
```
crontab -l
```

上面命令完会显示下面信息就是有保活设置成功
```
@reboot pkill -kill -u <username> && nohup /home/<username>/.s5/s5 -c /home/<username>/.s5/config.json >/dev/null 2>&1 & && nohup /home/<username>/.nezha-agent/start.sh >/dev/null 2>&1 &
*/12 * * * * pgrep -x "nezha-agent" > /dev/null || nohup /home/<username>/.nezha-agent/start.sh >/dev/null 2>&1 &
*/12 * * * * pgrep -x "s5" > /dev/null || nohup /home/<username>/.s5/s5 -c /home/<username>/.s5/config.json >/dev/null 2>&1 &
```
如果没有保活设置成功可以执行下面命令
- 重启机保活
```
(crontab -l; echo "@reboot pkill -kill -u <username> && nohup /home/${USER}/.s5/s5 -c /home/${USER}/.s5/config.json >/dev/null 2>&1 & && nohup /home/<username>/.nezha-agent/start.sh >/dev/null 2>&1 &") | crontab -
```
- nezha-agent保活
```
(crontab -l; echo "*/12 * * * * pgrep -x "nezha-agent" > /dev/null || nohup /home/${USER}/.nezha-agent/start.sh >/dev/null 2>&1 &") | crontab -
```
s5保活
```
(crontab -l; echo "*/12 * * * * pgrep -x "s5" > /dev/null || nohup /home/${USER}/.s5/s5 -c /home/${USER}/.s5/config.json >/dev/null 2>&1 &") | crontab -
```


- 5、socks5卸载命令(卸载完就执行第2步的安装命令重新安装)
```
pgrep -f 's5' | xargs -r kill
rm -rf ~/.s5
```

## 其它说明：
----
### ~pm2模式(不推荐)~
- ~一键安装~

~`bash <(curl -s https://raw.githubusercontent.com/amclubs/am-serv00-socks5/install-socks5-pm2.sh)`~


- 一键卸载pm2
```bash
pm2 unstartup && pm2 delete all && npm uninstall -g pm2
```
----

  # 
 <center><details><summary><strong> [点击展开] 赞赏支持 ~🧧</strong></summary>
 *我非常感谢您的赞赏和支持，它们将极大地激励我继续创新，持续产生有价值的工作。*
  
 - **USDT-TRC20:** `TWTxUyay6QJN3K4fs4kvJTT8Zfa2mWTwDD`
  
 </details></center>

 #
 免责声明:
 - 1、该项目设计和开发仅供学习、研究和安全测试目的。使用者在下载和使用该项目时，必须遵守当地法律和规定。使用者有责任确保他们的行为符合其所在地区的法律、规章以及其他适用的规定。
 - 2、作者对任何人或团体使用该项目进行的任何非法活动不承担责任。使用者使用该项目时产生的任何后果由使用者本人承担。
 - 3、作者不对使用该项目可能引起的任何直接或间接损害负责。作者保留随时更新本免责声明的权利，且不另行通知。