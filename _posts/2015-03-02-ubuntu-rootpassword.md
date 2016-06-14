---

layout: default
title: Ubuntu su root 认证失败
abstract: ubuntu su root
comments: false

---

## 启动

su root提示认证失败

ubuntu root是默认禁用了，不答应用root登陆，所以先要设置root密码。   

执行：sudo passwd root 接着输入密码和root密码，重复密码。再重新启动就可以用root登陆。

```javascript

changhu@ubuntu2015:~$ sudo passwd 
Password: <--- 输入安装时那个用户的密码 
Enter new UNIX password: <--- 新的Root用户密码 
Retype new UNIX password: <--- 重复新的Root用户密码 
passwd：已成功更新密码

```
