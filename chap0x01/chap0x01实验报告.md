
# Linux系统与网络管理——H1实验报告  
---   
### 实验问题  
+ 调查并记录实验环境的如下信息：  
    - 当前 Linux 发行版基本信息  
    - 当前 Linux 内核版本信息  
+ Virtualbox 安装完 Ubuntu 之后新添加的网卡如何实现系统开机自动启用和自动获取 IP？  
+ 如何使用 scp 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件？  
+ 如何配置 SSH 免密登录？  


---   

### 实验步骤  
+ **首先为虚拟机安装扩展包**   
  ![下载](.\img\vb_extension_install_1.png)  
  ![安装成功](.\img\vb_extension_install_2.png)  
  ![检查](.\img\install_succeed.png)  

+ **登陆虚拟机**   
  输入账号密码  
  ![登录](.\img\log_in.png)  

+ **查看当前 Linux 发行版基本信息**   
  ```  
  lsb_release -a  
  cat /etc/os-release  
  cat /etc/issue  
  ```
  ![发行版本](.\img\released_version.png)  
+ **查看当前 Linux 内核版基本信息**   
  ```  
  cat /proc/version  
  uname -a  
  ```  
  ![内核版本](.\img\kernel_version.png)  
+ **不同发行版本的对比：**    
  ![对比](.\img\对比.png)  
  
+ **Virtualbox 安装完 Ubuntu 之后新添加的网卡如何实现系统开机自动启用和自动获取IP**  
  - 使用```ip a```查看所有网卡的接口信息；使用```ip link ls up```查看正在运行的接口的相关信息  
  ![ip查看网卡信息](.\img\ip查看网卡信息.png)  
  虚拟机本身就有两个网卡，这里看到两个网卡都被分配到了```ip```，说明两块网卡都已经实现了开机自启用并获取```ip```  
  **代码及输出结果如下：**   
    ```  
    cuc@cuc-lab:~$ ip a  
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host  
       valid_lft forever preferred_lft forever  
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000  
    link/ether 08:00:27:bd:4a:29 brd ff:ff:ff:ff:ff:ff  
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3  
       valid_lft 84602sec preferred_lft 84602sec  
    inet6 fe80::a00:27ff:febd:4a29/64 scope link  
       valid_lft forever preferred_lft forever  
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000  
    link/ether 08:00:27:e3:cb:9f brd ff:ff:ff:ff:ff:ff  
    inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic enp0s8  
       valid_lft 320sec preferred_lft 320sec  
    inet6 fe80::a00:27ff:fee3:cb9f/64 scope link  
       valid_lft forever preferred_lft forever  

    cuc@cuc-lab:~$ ip link ls up  
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000  
    link/ether 08:00:27:bd:4a:29 brd ff:ff:ff:ff:ff:ff  
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000  
    link/ether 08:00:27:e3:cb:9f brd ff:ff:ff:ff:ff:ff  
      ```  
      如何添加网卡并且配置在后面有提到。   
+ **如何使用 scp 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件？**  
  - 主机向虚拟机传输文件  
    - 在主机桌面新建一个```test.txt```测试文件，在虚拟机中新建一个```test_vb.txt```测试文件  
    - 使用```scp test.txt cuc@192.168.56.101:/home/cuc```将主机文件传给虚拟机  
    - 进入虚拟机查看家目录下有```hk```  ```test.txt```  ```test_vb.txt```三个文件，```test.txt```传输成功  
     ![主机向虚拟机传输文件](.\img\主机向虚拟机传输文件.png)  
     **代码及输出结果如下：**   
      ```
      C:\Users\Lenovo>cd desktop  
      C:\Users\Lenovo\Desktop>scp test.txt cuc@192.168.56.101:/home/cuc  
      test.txt                                                                              100%    8     2.2KB/s   00:00  

      C:\Users\Lenovo\Desktop>ssh cuc@192.168.56.101  
      Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-65-generic x86_64)  

      * Documentation:  https://help.ubuntu.com  
      * Management:     https://landscape.canonical.com  
      * Support:        https://ubuntu.com/advantage  

      System information as of Fri 18 Mar 2022 03:20:10 AM UTC  

      System load:  0.0                Processes:               107  
      Usage of /:   11.1% of 38.63GB   Users logged in:         0  
      Memory usage: 19%                IPv4 address for enp0s3: 10.0.2.15  
      Swap usage:   0%                 IPv4 address for enp0s8: 192.168.56.101  


      15 updates can be installed immediately.  
      0 of these updates are security updates.  
      To see these additional updates run: apt list --upgradable  


      The list of available updates is more than a week old.  
      To check for new updates run: sudo apt update  

      Last login: Fri Mar 18 03:06:03 2022 from 192.168.56.1  
      cuc@cuc-lab:~$ ls  
      hk  test.txt  test_vb.txt  
      ```  

  - 将虚拟机中的文件复制到本地  
    - 使用```scp cuc@192.168.56.101:/home/cuc/test_vb.txt /Users/Lenovo/Desktop```     
    ![将虚拟机中的文件复制到宿主机](.\img\将虚拟机中的文件复制到宿主机.png)  
    ![test_vb传输成功](.\img\test_vb传输成功.png)  
      ```   
      C:\Users\Lenovo>scp cuc@192.168.56.101:/home/cuc/test_vb.txt /Users/Lenovo/Desktop  
      test_vb.txt                                                                           100%    0     0.0KB/s   00:00  
      ```  
  - 本机向远程机传输文件```C:\Users\Lenovo\Desktop>scp test.txt root@101.133.228.186:/home```  
    ![向远程机传test文件](.\img\向远程机传test文件.png)  
    ![本机向远程虚拟机传输文件](.\img\本机向远程虚拟机传输文件.png)  
  - 从远程系统复制文件```test_txt```到本地桌面  
    - 在阿里云的体验服务器的```root```目录下创建一个```test_far.txt```文件用来做传输实验   
    ![在远程机中创建test_far](.\img\在远程机中创建test_far.png)  
    - 使用```C:\Users\Lenovo\Desktop>scp root@47.116.1.244:/root/test_far.txt /Users/Lenovo/Desktop```将远程服务器的```test_far.txt```文件拷贝到本机桌面上  
    ![从远程复制成功](.\img\从远程复制成功.png)  

    
+ **如何配置 SSH 免密登录？**  
  
  - ```windows```本地创建密钥，在```cmd```命令行中输入```ssh-keygen -b 4096 -t rsa```命令  
    ![创建密钥](.\img\创建密钥.png)  
    ```
    C:\Users\Lenovo>ssh-keygen -b 4096 -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (C:\Users\Lenovo/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in C:\Users\Lenovo/.ssh/id_rsa.
    Your public key has been saved in C:\Users\Lenovo/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:Mjrlc1FwjwXAiHpC+T8cUTmkqO0+yIG/eCW6YGUzD5I lenovo@LAPTOP-7HQLQ7UN
    The key's randomart image is:
    +---[RSA 4096]----+
    |   . . ==oo..    |
    |  o ..o.+o +     |
    | . o. .. .o .    |
    |  +oo .  .       |
    | E.O.o+.S        |
    |. *.*++o .       |
    |.= =+.o..        |
    |+.=... o         |
    |oo....           |
    +----[SHA256]-----+
    ```
  - 查看密钥  
   ![查看密钥](.\img\查看密钥.png)  
   其中的```id_dsa.pub```,这就是我们要上传的公钥  
  - 在虚拟机中使用```ssh-keygen```生成公私钥对，创建的文件夹在```home/cuc/.ssh```中  
  - 在用户目录下，使用```chmod 700 .ssh```修改```.ssh```文件权限为```700```  
  - 再进入```.ssh```目录，使用```touch authorized_keys```命令创建```authorized_keys```文件  
  - 接着使用```chmod 600 authorized_keys```将文件权限修改成```600```  
   ![虚拟机上创建密钥](.\img\虚拟机上创建密钥.png)  
  - 使用```ip a```查看虚拟机```ip```地址  
   ![虚拟机ip](.\img\虚拟机ip.png)  
   得到地址为```192.168.56.101```  
  - 检查虚拟机上有没有安装SSH服务，使用命令```ssh```  
   ![ssh已安装](.\img\ssh已安装.png)  
   执行命令后输出```ssh```命令的使用说明，则代表已经安装好了   
  - 启动```ssh```服务，使用命令```sudo service sshd start```  
  - 检查有没有成功启动```SSH```服务，使用命令```service --status-all```  
   ![成功启动ssh](.\img\成功启动ssh.png)  
    这里看到```[+] ssh```，说明已经成功启动```ssh```服务  
  - 使用```scp```将公钥传入```authorized_keys```中  
    ```
    C:\Users\Lenovo\.ssh>scp id_rsa.pub cuc@192.168.56.101:/home/cuc/.ssh
    cuc@192.168.56.101's password:
    id_rsa.pub                                                                            100%  749   375.7KB/s   00:00
    ```
  - 文件传过来后进入```Linux```中的```.ssh```目录。使用```cat id_rsa.pub >> authorized_keys```命令将公钥信息追加在```authorized_keys```中。  
   ![把主机公钥添加到虚拟机上](.\img\把主机公钥添加到虚拟机上.png)  
  - 在```cmd```中使用```ssh cuc@192.168.56.101```免密登录成功  
   ![ssh免密登录成功](.\img\ssh免密登录成功.png)  
 

---
### 实验遇到的问题及解决办法  
+ 之前没看讨论区，用了```ifconfig```，还添加了第三块网卡。后来看到讨论区老师说```ifconfig```被时代淘汰了，要用```ip```操作，而且两块网卡足够，不需要第三块，所以就要用```ip```重新做一下实验。但是之前的操作不舍得删就在这里放上来吧。  
  - 原有两个网卡，现在添加第三个网卡  
   ![添加新网卡](.\img\新添加网卡.png)  
  - 使用```ifconfig -a ```命令查看所有网卡  
   ![所有网卡](.\img\下载后可以执行ifconfig-a.png)  
   可以看到网卡一共有三个```enp0s3``` ```enp0s8``` ```enp0s9```，说明新网卡添加成功  
   - 使用```ifconfig```命令查看工作网卡  
   ![工作网卡](.\img\工作网卡.png)  
   这里看到工作网卡只有```enp0s3``` ```enp0s8```，没有```enp0s9```所以接下来要配置新网卡的开机自动启用  
   - 使用```vim```打开文件```sudo vim /etc/netplan/00-installer-config.yaml```  
    ![查看00-installer-config.yaml](.\img\查看00-installer-config.yaml.png)  
   ![原有网卡](.\img\原有网卡信息.png)  
   - 将```enp0s9:``` ```dhcp4: true```信息填入并保存退出```vim```  
   ![添加网卡3到yaml](.\img\添加网卡3到yaml.png)  
   - 执行```sudo netplan apply```生效  
   ![执行新文件](.\img\执行新文件.png)   
   - 可以看到现在的工作网卡中有了我们新添加的网卡3  
   ![网卡3在工作中](.\img\网卡3在工作中.png)  
   - 关闭```Ubuntu```后重新启动，再次使用```ifconfig```查询工作网卡，发现新添加的网卡可以完成自动启用和获取```ip```  
   ![重新开机后自启动成功](.\img\重新开机后自启动成功.png)   

+ ```ifconfig```命令无法使用  
  ![ifconfig命令无法使用](.\img\ifconfig-a命令无法使用.png)  
  **解决办法：** 按照提示进行```sudo apt install net-tools```，下载后发现```ifconfig```命令可以正常使用了  
  ![下载后可以执行ifconfig-a](.\img\下载后可以执行ifconfig-a.png)  
+ ```vim```编辑器不会退出  
  **解决办法：** 搜索[Ubuntu下怎么退出vim编辑器](https://www.cnblogs.com/personblog/p/13728641.html)  
+ 输入```ssh-copy-id```报错  
  ![ssh-copy-id报错](.\img\ssh-copy-id报错.png)  
  **解决办法：**   
  - 安装openssh server  
  ![安装openssh](.\img\安装openssh.png)  
  ![安装成功](.\img\安装成功.png)  
  - 检查环境变量是否配置  
  ![环境变量已经配好](.\img\环境变量已经配好.png)  
  看到环境变量已经配置好了  

+ 配置好免密登录之后，第二天在```cmd```命令行中使用```ssh cuc@192.168.56.101```会出现报错```ssh: connect to host 192.168.56.101 port 22: Connection timed out```  
   ![虚拟机关闭之后无法ssh登录](.\img\虚拟机关闭之后无法ssh登录.png)  
   **解决办法：**  
   因为关闭了虚拟机，把虚拟机打开就可以免密登录了。  
+ ```scp test_vb.txt KH@175.30.81.115:/Users/Lenovo/Desktop```虚拟机向宿主机传输文件报错：  
  ```
  ssh: connect to host 175.30.81.115 port 22: Connection timed out
  lost connection
  ```
  尝试在虚拟机上```ssh```登录宿主机也出现了报错：  
  ```
  ssh: connect to host 175.30.81.115 port 22: Connection timed out
  ```  
  ![虚拟机向宿主机传输文件报错](.\img\虚拟机向宿主机传输文件报错.png)  
+ 本机连接远程服务器时需要输入密码，我复制密码之后粘贴显示```Permission denied, please try again.```  
  **解决办法：** 手动输入密码就可以正常连接了。  



---
### 参考文献
+ [VirtualBox 6.1.32 Oracle VM VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
+ [如何查看Linux发行版本、内核版本信息](https://blog.csdn.net/weixin_33521678/article/details/116675154)
+ [Ubuntu20添加新网卡后如何设置自动启用并获取ip](https://blog.csdn.net/xiongyangg/article/details/110206220)
+ [Ubuntu官方文档](https://ubuntu.com/server/docs/network-configuration)
+ [Ubuntu下怎么退出vim编辑器](https://www.cnblogs.com/personblog/p/13728641.html)
+ [windows(本地)SSH免密登录linux(虚拟机)](https://blog.csdn.net/Weary_PJ/article/details/104561720)
+ [如何使用Windows自带的OpenSSH服务端](https://www.cnblogs.com/lfri/p/13423261.html)
+ [Windows通过ssh公私钥免密登录Linux](https://blog.csdn.net/nuoxiaoli/article/details/109277510)
+ [linux基本网络配置（二）详解IP命令](https://blog.csdn.net/chinaltx/article/details/86497076)
+ [Linux下的scp拷贝命令详解](https://blog.csdn.net/yhblog/article/details/83927141)
+ [scp命令用法举例](https://blog.csdn.net/qq646748739/article/details/81974432)
