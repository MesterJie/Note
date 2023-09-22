





# git相关功能实现

## 分支合并

1.切换分支、拉取分支、切换分支、合并分支。

![](C:%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230801123231251.png)

## 第一次提交

 提交第一步要添加文件

## 如何添加远程仓库

```txt
git init
git add .
git branch -M main //修改分支名
git remote add origin 远程仓库链接
git push -u origin main
```

## Github官网进入慢，偶尔进不去

```txt
首先要获取github官网的ip地址，方法就是打开cmd，然后ping github.com
```

![image-20230922114644676](../../AppData/Roaming/Typora/typora-user-images/image-20230922114644676.png)

```txt
我们只需要打开电脑C:\Windows\System32\drivers\etc下的hosts文件编辑（需要管理员权限，右键，管理员权限打开），新增如下一行配置：
```

![image-20230922114729317](../../AppData/Roaming/Typora/typora-user-images/image-20230922114729317.png)

```txt
https://ipaddress.com/分别查询github.com和github.global.ssl.fastly.net的ip值，我查询出来ip分别为配置到了hosts文件中的：
```

![image-20230922114828586](../../AppData/Roaming/Typora/typora-user-images/image-20230922114828586.png)

```txt
刷新DNS缓存
```

![image-20230922114907300](../../AppData/Roaming/Typora/typora-user-images/image-20230922114907300.png)