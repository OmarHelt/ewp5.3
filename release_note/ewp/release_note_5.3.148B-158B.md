# Release Note 5.3.148B~158B

<!-- toc -->

## 变更内容

### 新增功能

\#14503. 预置资源加密算法升级：密钥生成方式改为C层实现。[使用方法](http://dev.rytong.me/emp5.3/get_started/senior_topical/preset_resource.html)  

### 修复了一些bug

1、\#14306. 离线存储-是否强制更新，后台修改为强制更新，运行客户端时强制更新不生效。（管理后台）  
2、\#14362. 使用-Uvh命令升级RPM后，启动ebank失败问题。（rpm命令升级）  
3、修正ewp_offline_server、ewp_resource错误格式。（错误日志信息格式）  
4、修改缓存文件处理逻辑。（离线缓存）  
5、修复 高并发时，sessionid重复的问题。（提高sessionid不重复几率）  
6、修复 离线资源批量删除脚本不删H5资源的问题。（删除脚本 delete_offline_res）  
7、修复纯H5资源备份功能错误。（备份脚本 restore_res）   
8、修复skip_boot_gen_desc配置成true时，启动不加载离线资源缓存的问题。（离线启动）  
