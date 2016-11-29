# Offline

<!-- toc -->

离线资源的相关接口，包括手动触发升级检查接口，单个文件的下载接口。

### [协议0版本支持的Lua接口](./lua_offline_0.md)
本节的Lua接口只支持离线协议版本为0的情况，高离线协议版本不兼容支持。


### [协议1版本支持的Lua接口](./lua_offline_1.md)

本节的Lua接口只支持离线协议版本为1及以上的情况，离线协议版本0不需要兼容支持。

- 由于离线流程设计，offline:update_desc(callback)接口必须在offline:update_hash(callback)或者tls:connect(callback, parameters)接口的回调中调用；
- offline:update_resource()接口必须在offline:update_desc(callback)接口的回调中调用；

### [协议2.0版本支持的Lua接口](./lua_offline_2.md)

本节的Lua接口只支持离线协议版本为2.0及以上的情况，离线协议版本1.0及以下不需要兼容支持。

### [协议2.1版本支持的Lua接口](./lua_offline_2.1.md)

本节的Lua接口只支持离线协议版本为2.1及以上的情况，离线协议版本2.0及以下不需要兼容支持。


### [协议3.0版本支持的Lua接口](./lua_offline_3.md)

本节的Lua接口只支持离线协议版本为3.0及以上的情况，离线协议版本2.0及以下不需要兼容支持。

### [协议4.0版本支持的Lua接口](./lua_offline_4.md)