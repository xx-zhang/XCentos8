# centos8 自定义iso

- [参考fedora](https://fedoraproject.org/w/index.php?title=anaconda/kickstart/zh-cn&oldid=378528)
- [知乎创建启动项](https://zhuanlan.zhihu.com/p/140972579)
- [定制启动壁纸](https://www.pianshen.com/article/19221767685/)
- [cenbtos7-5.5-kenel-集成](https://blog.csdn.net/weixin_39599317/article/details/112729710)
- [centos7 手动安装内核](https://www.cnblogs.com/xzkzzz/p/9627658.html)

## 相关辅助脚本
```bash

yum -y install createrepo mkisofs anaconda-runtime  \
     isomd5sum squashfs-tools genisoimage rsync 
yum -y install yum-utils 

# 测试后，ISO自动运行未通过
mkdir rpms && cd rpms \
    yumdownloader --resolve --destdir . python38 git curl wget gcc gcc-c++ cmake \
    ioping yum-utils device-mapper-persistent-data lvm2 docker-ce 

#  推荐使用
yumdownloader --resolve --destdir . ioping fio sysbench iperf3 \
    python38 git gcc gcc-c++ wget cmake 

mount -o loop CentOS-8.2.2004-x86_64-minimal.iso /mnt/iso/

# /usr/bin/rsync -a --exclude=Packages/ \
    --exclude=repodata/ /mnt/iso/ /iso/
# /usr/bin/rsync -a  --exclude=Packages/ /mnt/iso/ /iso/
/usr/bin/rsync -a  /mnt/iso/ /ISO/

/usr/bin/cp rpms/* /ISO/BaseOS/Packages/

## ks 检验
#yum -y install pykickstart 
#ksvalidator ks.cfg

genisoimage -joliet-long \
    -V CentOS8 \
    -o CentOS-8-Mini-amd64.iso \
    -b isolinux/isolinux.bin \
    -c isolinux/boot.cat \
    -no-emul-boot -boot-load-size 4 \
    -boot-info-table -R -J -v -cache-inodes -T -eltorito-alt-boot \
    -e images/efiboot.img -no-emul-boot /ISO
```

## 注意安装 pgsql的依赖
-   `readline-devel zlib-devel `


## 测试
```bash

virt-install  -n kk -r 2048  --vcpus 2 \
    --cdrom /root/CentOS-8-Mini-amd64.iso \
    --disk path=/root/demo.qcow2,size=5,format=qcow2,bus=virtio,target=vda  \
     --network bridge=virbr0,model=virtio --os-type linux \
    --graphics vnc,listen=0.0.0.0,port=5989 \
      --os-variant rhl8 --boot hd

```