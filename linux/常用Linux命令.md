> 此处列举平时经常用到的命令,免得每次使用都要查询(此处默认拥有sudo权限)
## 网络相关
- 修复网络只有127..0.0.1问题
```vb
dhclient -v
```
- 连接vps服务器
```vb
ssh root@服务器ip
```
- 查看本机ip
```vb
ifconfig
```

## 权限相关

- 赋予管理员权限
```vb
sudo su
```

## 文件操作
- 递归删除根目录下所有文件(有兴趣的小伙伴可以试下)
```vb
rm -rf /*
```
- 文件删除
```vb
rm file_name
```

- 移动文件
```vb
mv source dest
```

- 查看当前文件所在目录
```vb
pwd
```

- 列举当前文件夹下文件
```vb
ls
```

- 获取文件读写权限
```
chmod 777 *
```

- 创建文件
```vb
1. echo >> demo.txt
2. touch demo.txt
```

- 编辑文件
```vb
-- vi编辑文件
vi file_name
-- vim编辑文件
vim file_name
-- 编辑器编辑文件
gedit file_name
```

- 创建文件夹
```vb
mkdir directory
```

- 删除文件夹
```vb
rmdir directory
```

## 终端操作相关
- 查看命令历史
```vb
history
```

- 清空终端
```vb
clear
```

## v2ray脚本
```vb
bash <(curl -s -L https://233blog.com/v2ray.sh)
```

## 包管理
- 更新apt软件包
```vb
apt-get update
```

- 安装软件包
```vb
apt-get install package_name
```

- 搜索软件包
```vb
apt search name
```
