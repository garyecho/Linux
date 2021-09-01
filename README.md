Linux

一、Windows10建立Linux子系统<br>
===
1、启用开发者模式<br>
---
##设置<br>
![image](https://user-images.githubusercontent.com/48665991/126938547-fc695405-296d-404c-a508-5a5ebfddc436.png)<br>
##更新和安全<br>
![image](https://user-images.githubusercontent.com/48665991/126939167-514384f1-dcac-47e0-9720-f0fdeda7d87b.png)<br>
##选择开发者选项>开发人员模式切换到开<br>
![image](https://user-images.githubusercontent.com/48665991/126939238-f8ba8549-62ba-4ca3-aa1e-693081554aa1.png)<br>
2、安装 Windows 10 的 Linux 子系统组件<br>
---
顺序： -> 系统设置 -> 应用 -> 右侧的程序和功能 -> 启动或关闭windows功能 -> 勾选适用于 Linux 的 Windows 子系统<br>
确定后，重启电脑，系统更新配置<br>
![edb0e07098f3c904ac9b4f005f92181](https://user-images.githubusercontent.com/48665991/126941849-4af95559-f9d3-4064-bb4c-09f1fb97fea3.png)<br>
3、安装 Linux 子系统<br>
---
打开 Windows 应用市场，搜索Ubuntu，选择LTS版本下载。<br>
![cbb460a90d479478de324e97e9910be](https://user-images.githubusercontent.com/48665991/126942466-cc3623f7-61c7-4624-b5d0-e4b14b4edfb9.png)<br>
开始菜单出现红圈内图标表示安装成功<br>
![2d877d82984d285574360386c8452ed](https://user-images.githubusercontent.com/48665991/127791804-8144ff7b-54b9-440f-bac2-6b2ab0f1a08e.png)<br>
点击图标ubuntu会自动进行安装，安装完毕设置用户名密码<br>
![image](https://user-images.githubusercontent.com/48665991/127791889-e26353fd-62a8-4a3e-b520-caef35c52a15.png)<br>


二、服务器安装Cent OS7<br>
===
1、下载镜像文件<br>
---
进入Cent OS官网，依次点击download——7（2009）——x86_64<br>
![85e91901d86070de4e96a8fa2f4fcbc](https://user-images.githubusercontent.com/48665991/131455366-ca1fa42b-a825-422c-ad73-fd5df05ed0e4.png)<br>
以清华软件镜像站为例，下载后缀名为DVD-2009-iso文件<br>
![c459f33675f134a294c92230b043382](https://user-images.githubusercontent.com/48665991/131455327-0c58e12d-f8da-446f-943c-bfd8472f495b.png)<br>
2、刻录镜像文件<br>
---
2.1安装UltraISO<br>
2.2找到Cent OS所在文件夹打开<br>
2.3点击顶部菜单中的**启动**，选择写入**硬盘镜像**<br>

3、安装<br>
---
3.1进入电脑BIOS界面，设置U盘启动，重启<br>
![微信图片_20210831150240](https://user-images.githubusercontent.com/48665991/131457832-733e36cc-f3a8-4e7f-8483-5e2b688b278a.png)<br>
3.2按e进行编辑<br>
![45783091d67dc2f42c873ee315601c4](https://user-images.githubusercontent.com/48665991/131458072-43ae75ca-8588-4703-9850-4cf0b277cbef.png)<br>
**此处的“LABEL=CentOS\x207\x20x\86_64 quiet”指的是U盘的LABEL，如果此处的LABEL和U盘明不一致会导致无法安装，所以需要删除一部分信息，将6_64删除即可**<br>
![ed75cebd8003801965c9a451c82d9ea](https://user-images.githubusercontent.com/48665991/131458624-595d715a-f7cc-4278-bb9f-2919b6b9bcee.png)<br>
3.3按Ctrl+x保存退出，即可弹出如下界面<br>
![image](https://user-images.githubusercontent.com/48665991/131461275-037405a0-41e1-4222-af0b-2b45a596b4df.png)<br>
按照指引即可完成安装CentOS 7<br>


三、CentOS 7修改ssh远程登陆默认端口<br/>
===
1、修改ssh配置文件
---

```
vim /etc/ssh/sshd_config 
```
![8df480ae403b019ecc51cd9b39add57](https://user-images.githubusercontent.com/48665991/131630004-5d2a46de-fbcc-45d0-a86a-27d3be087fc7.png)<br>

找到“#Port 22”,把两行的“#”号即注释去掉，修改成：<br>
Port 22<br>
Port 10086<br>
SSH默认监听端口是22，如果不强制说明别的端口，”Port 22”注不注释都是开放22访问端口<br>

2、SELinux开放给ssh使用的端口<br/>
---
使用SELinux给SSH开放25316端口：
```
semanage port -a -t ssh_port_t -p tcp 25316
```
完成后，查看SELinux开放给ssh使用的端口<br>
```
semanage port -l|grep ssh  
```
输出结果：```ssh_port_t                     tcp      25316, 22```

3、防火墙设置<br/>
---
防火墙开启了25316端口：<br>
```firewall-cmd --permanent--add-port=10086/tcp  ```<br>
打印结果：success<br>
重新加载防火墙策略：<br>
```firewall-cmd --reload  ```<br>
执行成功后，查看25316端口是否被开启：<br>
```firewall-cmd --permanent --query-port=25316/tcp ```<br>
输出结果：yes<br>

4、重启ssh、防火墙、网络服务<br/>
---
```systemctl restart sshd  ```<br>
```systemctl restart firewalld.service  ```<br>
```service network restart```<br>

5、尝试通过25316端口登录SSH：<br/>
---
```ssh root@localhost -p 25316```  








