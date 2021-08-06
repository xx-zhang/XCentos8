# 自定义 Centos8 ISO 
> 基于 centos8.2 mini 自定义自己的操作系统


## 修改内容
- 1. 增加自定义的一些 `rpm` 安装包
- 2. 修改防火墙\rpm源
- 3. 修改默认时区  
- 3. 增加自定义的脚本

# 自定义ISO步骤

## 介绍核心修改的文件
- [ks.cfg](./pkgs/ks.cfg) (推荐从根目录的), 设置启动需要安装的依赖 [关键字参考](https://fedoraproject.org/wiki/Anaconda/Kickstart/zh-cn#firstboot)
- [comps.xml](./pkgs/comps.xml) 将`pkgs`绑定到`rpms`中。
- [isolinux.cfg](./isolinux.cfg) 菜单 
- [grub.cfg](./grub.cfg) 启动和菜单

# 修改日志
## 2021/7/24 
- 本人的 `ks.cfg` 脚本不能正常安装需要的程序。当前还在研究。

