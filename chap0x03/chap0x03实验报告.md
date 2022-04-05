# Linux系统与网络管理——H3实验报告  
---   
### 实验问题
+ 如何添加一个用户并使其具备sudo执行程序的权限？
+ 如何将一个用户添加到一个用户组？
+ 如何查看当前系统的分区表和文件系统详细信息？
+ 如何实现开机自动挂载Virtualbox的共享目录分区？
+ 基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？
+ 如何通过systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？
+ 如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死？

### 实验步骤
+ **如何添加一个用户并使其具备sudo执行程序的权限？**
    + 进入超级用户`su root`
    + 添加一个用户`adduser hk`
    ![创建一个新用户](img/创建一个新用户.png)
    + 检查`hk`的`sudo`权限
    ![检查hk的sudo权限](img/检查hk的sudo权限.png) 
    缺少权限
    + 为`hk`添加`sudo`权限：
        ```
        cuc@cuc-lab:~$ su root
        Password:
        root@cuc-lab:/home/cuc# chmod u+w /etc/sudoers
        root@cuc-lab:/home/cuc# root@cuc-lab:/home/cuc# vim /etc/sudoers
        root@cuc-lab:/home/cuc# vim /etc/sudoers
        root@cuc-lab:/home/cuc# chmod u-w /etc/sudoers
        root@cuc-lab:/home/cuc#
        ``` 
        ![给hk用户添加sudo权限](img/给hk用户添加sudo权限.png)
        ![vim给hk添加sudo权限](img/vim%E7%BB%99hk%E6%B7%BB%E5%8A%A0sudo%E6%9D%83%E9%99%90.png)
    + 再次检查`hk`的`sudo`权限
    ![权限添加成功](img/hk%E7%9A%84sudo%E6%9D%83%E9%99%90%E6%B7%BB%E5%8A%A0%E6%88%90%E5%8A%9F.png)
    权限添加成功

+ **如何将一个用户添加到一个用户组？**
    + 查看所有用户`cat /etc/passwd`
    + 查看用户组`cat /etc/group`
    ![查看用户组](img/%E6%9F%A5%E7%9C%8B%E7%94%A8%E6%88%B7%E7%BB%84.png) 
    + 添加`hk`用户到`adm`用户组
    ![添加hk到adm用户组](img/%E6%B7%BB%E5%8A%A0hk%E5%88%B0adm%E7%BB%84.png) 

+ **如何查看当前系统的分区表和文件系统详细信息？**








### 实验遇到的问题及解决办法
+ 普通用户切换`root`用户输入密码后提示：`su: Authentication failure`
![切换root用户报错](img/切换root用户报错.png) 
    + **解决办法：**
        ```
        cuc@cuc-lab:~$ sudo passwd root
        New password:
        Retype new password:
        passwd: password updated successfully
        cuc@cuc-lab:~$ su root
        Password:
        root@cuc-lab:/home/cuc#
        ``` 
+ 将用户添加到用户组时出现报错：
    ```
    cuc@cuc-lab:~$ usermod -a -G adm hk
    usermod: Permission denied.
    usermod: cannot lock /etc/passwd; try again later.
    ``` 
    + **解决办法：** 直接`sudo`





### 参考文献
+ [Authentication failure解决办法](https://blog.csdn.net/qq_40511918/article/details/83650536)
+ [Linux中给普通用户添加sudo权限](https://www.cnblogs.com/zhangwuji/p/9152551.html)
+ [Linux 将一个用户添加到某一用户组中](https://blog.csdn.net/yabingshi_tech/article/details/46874179)
+ [usermod: cannot lock /etc/passwd; try again later.](https://serverfault.com/questions/1053446/usermod-cannot-lock-etc-passwd-when-usermod-is-in-etc-sudoers-as-nopasswd)