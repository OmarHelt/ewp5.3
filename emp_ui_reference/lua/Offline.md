# Offline

<!-- toc -->

离线资源的相关接口，包括手动触发升级检查接口，单个文件的下载接口。

## 协议0版本支持的Lua接口

本节的Lua接口只支持离线协议版本为0的情况，高离线协议版本不兼容支持。


### offline:download(callback)

* **Description:**  
  获取服务器端离线资源的描述。只获取描述并分析，不执行真正的资源下载。根据回调方法进行后续操作。**新版本请使用[offline:update_desc(callback)](#offlineupdatehashcallback)。**
* **Parameters:**
  1. callback_(required)_
     - 说明：监听方法，此监听方法在定义时必须有2个参数:
         * _String（json格式）_，String为请求离线资源接口返回给客户端的内容（其中download字段表示所有需要更新的必选资源描述，option_download字段表示所有可选资源的描述。其中资源如果是zip包，那么还会在当前zip包中多出一个“desc”字段，该“desc”字段中的值表示zip包中所有文件的描述。）
         * _String_，其值为1（代表强制更新）、0（可选更新）、－1（不需更新）。
* **Return:**  
  无
* **Examples:**

		function callback(content，mustupdate)
			-- 
		end
		offline:download(callback);


### offline:mustDownload()

* **Description:**  
  调用下载离线资源，不下载可选部分。由于离线流程设计，该接口必须在[offline:download(callback)](#offlinedownloadcallback)接口的回调中调用。**新版本请使用[offline:update_resource()](#offlineupdateresource)。**
* **Parameters:**  
  无
* **Return:**  
  无


### offline:downfile(filename, relatedPath, callback)

* **Description:**  
  发起单一可选离线资源下载。可选资源文件名可通过[offline:optDownloadJson()](#offlineoptdownloadjson)接口获取。**新版本请使用[offline:downOptionalFile(filename, callback)](#offlinedownoptionalfilefilename-callback)。**
* **Parameters:**  
  ```
	“option_download”:{"login.zip":"common/zip"}
  ```
  1. Filename_(required)_
    - 说明：表示“option_download”子元素的key（login.zip）。
  2. relatedPath_(required)_
    - 说明：表示“option_download”子元素的value（common/zip）。
  3. callback_(optional)_
    - 说明：监听方法。此监听方法在定义时必须有一个参数，类型为Boolean，表示当前离线资源是否下载成功。
* **Return:**  
  无
* **Examples:**

		function callback(flag)
			-- 
		end
		offline:downfile(login.zip, common/zip, callback);


### offline:optDownloadJson()

* **Description:**  
  获得当前本地保存的服务器描述文件（server.desc）中所有可选文件描述信息的json字符串（“option_download”字段中所有可选文件的描述信息）。**新版本请使用[offline:getOptInfoInServer()](#offlinegetoptinfoinserver)。**
* **Parameters:**  
  无
* **Return:**
  - 类型：_String_
  - 说明：所有可选下载的离线资源的列表（json）。

			{
				"1.zip": "common/zip",
				"msg_subscribe.zip": "common/zip",
				"fast_transfer_optional.zip": "common/zip",
				"login.zip": "android/common/zip"
			}


### offline:optDownloadJsonInLocal()

* **Description:**  
  获得本地已经下载的可选离线资源的json字符串。**新版本请使用[offline:getOptInfoInLocal()](#offlinegetoptinfoinlocal)。**
* **Parameters:**  
  无
* **Return:**
  - 类型：_String_
  - 说明：本地已经存在的可选下载的离线资源的列表（json），不包含校验hash值。（“option_download”字段中已经在本地存在并验证通过的的描述信息）。

			{
				"1.zip": "common/zip",
				"login.zip": "android/common/zip"
			}


### offline:optDownloadDescInLocal()

* **Description:**  
  获得本地已经下载的可选离线资源的资源描述文件（json字符串格式）。**新版本请使用[offline:getOptInfoInLocal()](#offlinegetoptinfoinlocal)。**
* **Parameters:**  
  无
* **Return:**
  - 类型：_String_
  - 说明：本地已经下载的可选离线资源的资源描述文件（json字符串格式），包含校验hash值。

			{
				"resources": {
					"yybk.zip": {
						"rev": "9977ad6a4d5fab2ae6ce32cd68abe39fd94f5baf",
						"path": "iphone/640-960/aa.png"
					}
					...
				}
			}


### offline:checkOfflineFile(filename, relatedPath)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。**新版本请使用[offline:checkOfflineFile(filename)](#offlinecheckofflinefilefilename)。**<br>
  有效的定义：
  1. 必选资源：客户端本地存在对应文件，并且和本地存储的已下载必选文件描述一致；
  2. 可选资源：客户端本地存在对应文件，并且和本地存储的最新服务器可选资源描述一致；
* **Parameters:**
  1. filename_(required)_
     - 说明：资源文件名
  2. relatedPath_(required)_
     - 说明：表示当前资源的相对地址
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示当前文件有效，false表示当前文件无效。


### offline:getServerDesc()

* **Description:**  
  获取本地存储的服务器离线描述信息的内容。**新版本取消此接口。**
* **Parameters:**  
  无
* **Return:**
  - 类型：_String_
  - 说明：返回本地存储的服务器离线描述信息的内容。

			{
				"mustupdate": 0,
				"http":"http://192.168.xx.xx:XXXX/resources/",
				"server":{
					"a.zip":{
						"path":"android/common/zip",
						"rev":"bf97d8c616da91391b442ef6de7b2cacb030399a",
						"desc":{
							"a/1.png":"d988a5c75c2cf75c7412e65b82a191b99ea465e7",
							"a/2.png":"e5ce2668e1d1b1aff470e61026180090adff4a0f",
							...
						}
						"download_type":1,
						"encrypt":0
					}
					"x1.png":{
						"path":"common/png",
						"rev":"eb6440da56ae683acc0b0b292fda6ec83e1684fa",
						"download_type":0,
						"encrypt":0
					}
					...
				}
				"download":{
					"x1.png":"common/png",
					"11.zip":"common/zip",
					...
				}
				"download_optional":{
					"a.zip":"android/common/zip",
					"b.zip":"common/zip",
					...
				}
				"deleted":[test1.png,test2.zip,...]
			}


### offline:remove(name)

* **Description:**  
  删除指定的文件，用于删除离线下载和插件资源（不区分必选可选)。**新版本请使用[offline:removeOptinalFile(filename)](#offlineremoveoptionalfilefilaname)。**
* **Parameters:**
  1. name_(required)_
     - 说明：文件名称（带后缀），或者文件夹（无后缀）。指定路径（带“/”）的文件按插件内单个文件处理，在插件目录下查找文件删除；文件夹或者zip后缀名文件删除对应插件目录；其余文件在离线目录下查找文件删除。
* **Return:**  
  无
* **Examples:**

		offline:remove(“vv/home.xml”); -- 将名为vv的插件下的“home.xml”文件删除。
		offline:remove(“vv”); -- 将名为vv的插件删除。
		offline:remove(“uu.lua”); -- 将离线资源“uu.lua”文件删除。


## 协议1版本支持的Lua接口

本节的Lua接口只支持离线协议版本为1及以上的情况，离线协议版本0不需要兼容支持。

- 由于离线流程设计，[offline:update_desc(callback)](#offlineupdatedesccallback)接口必须在[offline:update_hash(callback)](#offlineupdatehashcallback)或者[tls:connect(callback, parameters)](#tlsconnectcallback-parameters)接口的回调中调用；
- [offline:update_resource()](#offlineupdateresource)接口必须在[offline:update_desc(callback)](#offlineupdatedesccallback)接口的回调中调用；


### offline:update_hash(callback)

* **Description:**  
  请求resource_hash接口，获取离线资源的更新检测结果。
* **Parameters:**
  1. callback_(required)_
     - 说明：当资源更新检查接口请求结束后，调用此函数，返回资源更新的检测结果。

				function callback(update)
					...
				end
     其中update为number类型，值为检查接口返回的值。
         * 0 表示没有更新；
         * 1 表示必选资源有更新；
         * 2 表示可选资源描述有更新；
         * 3 表示必选、可选资源描述都有更新；
         * -1 表示网络请求失败。
* **Return:**
  无


### offline:update_desc(callback)

* **Description:**  
  请求resource_update接口，获取离线资源的下载描述，并缓存。
* **Parameters:**
  1. callback_(required)_
     - 当资源更新接口请求结束后，调用此函数，返回资源更新的接口信息。

				function callback(updateState)
					...
				end 
     其中updateState为number类型，值为更新接口返回的mustUpdate(m)字段内容。
         * 1 表示强制更新；
         * 0 表示用户可以选择是否更新；
         * -1 表示不需要更新；
         * -2 表示网络update接口请求失败。
* **Return:**
  无


### offline:update_resource()

* **Description:**  
  根据offline:update_desc获取的下载资源描述，下载资源。此接口仅下载必选资源，可选资源的下载通过opt系列接口处理。
* **Parameters:**  
  无
* **Return:**  
  无
* **Examples:**  
  update\_hash、update\_desc、update\_reource三个接口一般组合使用  

		offline:update_hash(updatehash_callback);
		function updatehash_callback(update)
			if update == 1 or update == 2 or update == 3 then
				-- 有必选可选资源更新时
				offline:update_desc(updateDesc_callback);
			end
		end 
		function updateDesc_callback(mustUpdate)
			if mustUpdate == 0 then
				window:alert("您有新的离线资源需要下载，是否更新？", "确定", "取消", alert_callback_zero);
			elseif mustUpdate == 1 then
				offline:update_resource();
				window:alert("有离线资源正在更新", "确定", alert_callback_one);
			end
		end
		-- window:alert 回调方法：
		-- 若mustupdate = 0，用户点击确定按钮开始下载必选资源
		function alert_callback_zero(btnIndex)
			if btnIndex == 0 then
				-- 用户点击[确定]按钮
				offline:update_resource();
			end
		end
		function alert_callback_one(btnIndex)
		end


### offline:downOptionalFile(filename, callback)

* **Description:**  
  发起单一可选离线资源下载。注：该接口只能下载可选资源，且只能下载整个插件包资源。
* **Parameters:**

		"option_download": {
			"dir1" :  //如: ebank/resources/common/zip
			{
				"zip1.zip" : //zip包中部分文件的更新 如: account.zip
			}
		}
  1. filename_(required)_
     - 说明：需要下载的可选资源文件名（account.zip）。
  2. callback_(optional)_
     - 说明：监听方法。此监听方法在定义时必须有一个参数，类型为Boolean，表示当前离线资源是否下载成功。
* **Return:**  
  无
* **Examples:**

		function callback(flag)
			-- 
		end
		offline:downOptionalFile(account.zip, callback);


### offline:getOptInfoInServer()

* **Description:**  
  获得当前本地保存的服务器描述文件（server.desc）中所有可选文件描述信息的table（“option_download”字段中所有可选文件的描述信息）。
* **Parameters:**  
  无
* **Return:**
  - 类型：_table_
  - 说明：所有可选下载的离线资源的列表。

			{
				"zip1.zip": "c4f3cffb7d68e63531c60e74611a2d2f569173e7", 
				...
			}


### offline:getOptInfoInLocal()

* **Description:**  
  获得本地已下载的可选文件描述信息的table。
* **Parameters:**  
  无
* **Return:**
  - 类型：_table_
  - 说明：本地已下载的可选资源的列表。

			{
				"zip1.zip": "c4f3cffb7d68e63531c60e74611a2d2f569173e7", 
				...
			}


### offline:checkOfflineFile(filename)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。<br>
  有效的定义：
  1. 必选资源：客户端本地存在对应文件，并且和本地存储的已下载必选文件描述一致；
  2. 可选资源：客户端本地存在对应文件，并且和本地存储的最新服务器可选资源描述一致；
* **Parameters:**
  1. filename_(required)_
     - 说明：当前资源文件名。如果是单个资源或者插件包资源，则只需要文件名；如果是插件包内单个资源，则需要针对插件包目录的相对路径+文件名。
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示当前文件有效，false表示当前文件无效。


### offline:removeOptionalFile(filaname)

* **Description:**  
  删除指定的文件，需要删除资源文件及相应的描述信息，只用于删除可选资源。需要将给定插件包内的所有资源删除。
* **Parameters:**
  1. name_(required)_
     - 说明：需要删除的可选资源文件名（account.zip）。
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示删除成功，false表示删除失败。如若本地无对应文件，则返回false。
* **Examples:**

		offline:removeOptionalFile(“account.zip”); -- 将名为account.zip的插件包内所有资源删除.


### offline:commentOfFile(filaname)

* **Description:**  
  获取资源描述，离线资源的普通资源与插件包会包含该资源的描述信息。注:插件包中的资源不包含描述信息。带有"<>"等特殊字符的描述信息，在xml解析时可能会造成解析错误，建议不要在描述中带有这些字符。
* **Parameters:**
  1. filaname_(required)_
     - 说明：需要获取描述的文件名，如果操作的文件为插件需要以.zip作为后缀表明这是一个插件，如account.zip。
* **Return:**
  - 类型：_String_
  - 说明：如果获取到描述，以字符串类型返回，如果获取失败返回空(nil)。
* **Examples:**

		offline:commentOfFile(“account.zip”); -- 获取插件account的描述信息.


## 协议2.0版本支持的Lua接口

本节的Lua接口只支持离线协议版本为2.0及以上的情况，离线协议版本1.0及以下不需要兼容支持。

### offline:update_resource(params)

* **Description:**  
  根据offline:update_desc获取的下载资源描述，下载资源。此接口仅下载必选资源，可选资源的下载通过opt系列接口处理。本接口为[offline:update_resource()](#offlineupdateresource)接口的升级版。
* **Parameters:**  
  1. params_(optional)_
     - 类型：_table_
     - 说明：该参数保存下载过程中需要回调处理的回调方法。每个回调方法都是可选的，只有参数中定义的回调方法，才会在对应的时机执行。暂提供以下两个回调接口：
         * `processCallback(downNum, totalNum)`  
         指示下载进度的回调方法，在每单个资源文件下载结束后回调。参数为当前已下载的文件数和总共需要下载的文件数，都为number类型。  
         **注：**参数中的文件数量与下载列表中的文件数量相对应，如果某个zip格式文件整个包都需要下载，则算一个文件；若只需下载包内部分资源，则按实际更新文件数算多个文件。  
         * `finishedCallback(failtable)`  
         所有资源下载结束后执行的回调，不管资源是否全部更新成功。参数为table型，key值为默认值(1，2，3...)，value记录所有更新失败的资源下载路径（路径+文件名）。
* **Return:**  
  无
* **Examples:**  
  update\_hash、update\_desc、update\_reource三个接口一般组合使用

		offline:update_hash(updatehash_callback);
		function updatehash_callback(update)
			if update == 1 or update == 2 or update == 3 then
				-- 有必选可选资源更新时
				offline:update_desc(updateDesc_callback);
			end
		end 
		function updateDesc_callback(mustUpdate)
			if mustUpdate == 0 then
				window:alert("您有新的离线资源需要下载，是否更新？", "确定", "取消", alert_callback_zero);
			elseif mustUpdate == 1 then
				update();
				window:alert("有离线资源正在更新", "确定", alert_callback_one);
			end
		end
		-- window:alert 回调方法：
		-- 若mustupdate = 0，用户点击确定按钮开始下载必选资源
		function alert_callback_zero(btnIndex)
			if btnIndex == 0 then
				-- 用户点击[确定]按钮
				update();
			end
		end
		function alert_callback_one(btnIndex)
		end
		-- 下载过程相关
		function update()
			local params = {"processCallback":processCallback, "finishedCallback":finishedCallback}
			offline:update_resource(params);
		end
		function processCallback(downNum, totalNum)
			...
		end
		function finishedCallback(failTable)
			if failTable and #failTable > 0 then
				window:alert("离线资源更新结束，但有部分资源未更新成功。");
			else
				window:alert("离线资源更新成功。");
			end
		end


### offline:checkOfflineFile(filename)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。<br>
  有效的定义：客户端本地存在对应文件，并且和本地存储的已下载必选/可选文件描述一致。
* **Parameters:**
  1. filename_(required)_
     - 说明：当前资源文件名。如果是单个资源或者插件包资源，则只需要文件名；如果是插件包内单个资源，则需要针对插件包目录的相对路径+文件名。
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示当前文件有效，false表示当前文件无效。


### offline:checkOfflineFileWithServer(filename)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。<br>
  有效的定义：客户端本地存在对应文件，并且和本地存储的最新服务器必选/可选资源描述一致。
* **Parameters:**
  1. filename_(required)_
     - 说明：当前资源文件名。如果是单个资源或者插件包资源，则只需要文件名；如果是插件包内单个资源，则需要针对插件包目录的相对路径+文件名。
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示当前文件有效，false表示当前文件无效。
 
## 协议2.1版本支持的Lua接口

本节的Lua接口只支持离线协议版本为2.1及以上的情况，离线协议版本2.0及以下不需要兼容支持。

### offline:checkOfflineFileWithLocalH5(filename)

* **Description:**  
  检测本地H5离线资源文件是否有效。
 - 有效定义： 客户端本地存在对应文件，并且和本地存储的已下载必选文件描述一致。
* **Parameters:**
  1. filename_(required)_  
     - H5资源的文件路径 + 文件名。
* **Return:**
     - 类型：Boolean
     - 说明：true表示当前文件有效，false表示当前文件无效

### offline:checkOfflineFileWithServerH5(filename)

* **Description:**  
  检测本地H5离线资源是否有效。
 - 有效定义：客户端本地存在对应文件，并且和本地存储的最新服务器资源描述一致。
* **Parameters:**
  1. filename_(required)_
     - 类型：_String_
     - 说明：H5资源的文件路径 + 文件名。
* **Return:**
     - 类型：Boolean
     - 说明：true表示当前文件有效，false表示当前文件无效

## 协议3.0版本支持的Lua接口

本节的Lua接口只支持离线协议版本为3.0及以上的情况，离线协议版本2.0及以下不需要兼容支持。

### offline:update_hash(callback, parameter)

* **Description:**  
  本接口为[offline:update_hash(callback)](#offlineupdatehashcallback)的升级版，功能和用法与其相同。
* **Parameters:**
  1. callback_(required)_
  2. parameter_(optional)_
     - 类型：Table
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数

### offline:update_desc(callback, parameter)

* **Description:**  
  本接口为[offline:update_desc(callback)](#offlineupdatedesccallback)的升级版，功能和用法与其相同。
* **Parameters:**
  1. callback_(required)_
  2. parameter_(optional)_
     - 类型：_Table_
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数。


### offline:update_resource(parameter)

* **Description:**  
  本接口为[offline:update_resource(params)](#offlineupdateresourceparams)的升级版，功能和用法与其相同。
* **Parameters:**  
  1. parameter_(optional)_
     - 类型：_Table_
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：新增如下两个key：appname：请求更新App名称；info：请求参数。

            -- 下载进度回掉：
            function processCallback(downNum, totalNum)
               
            end
            
            -- 下载结束回掉：
            function finishedCallback(failtable)
             
            end
            
            local updateresTable = {processCallback=processCallback, finishedCallback=finishedCallback, appname = "emas", info = "message"};

			offline:update_resource(updateresTable);


### offline:downOptionalFile(filename, callback, parameter)

* **Description:**  
  本接口为[offline:downOptionalFile(filename, callback)](#offlinedownoptionalfilefilename-callback)的升级版，功能和用法与其相同。
* **Parameters:**  
  1. filename_(required)_
  2. callback_(optional)_
  3. parameter_(optional)_
     - 类型：_Table_
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数。


### offline:getOptInfoInServer(appName)

* **Description:**  
  本接口为[offline:getOptInfoInServer()](#offlinegetoptinfoinserver)的升级版，功能和用法与其相同。
* **Parameters:**  
  1. appName_(optional)_
     - 类型：_String_
     - 说明：需要获取描述的App名称，若不传此值，表示获取默认App描述。


### offline:getOptInfoInLocal(appName)

* **Description:**  
  本接口为[offline:getOptInfoInLocal()](#offlinegetOptInfoInLocal)的升级版，功能和用法与其相同。
* **Parameters:**  
  1. appName_(optional)_
     - 类型：_String_
     - 说明：需要获取描述的App名称，若不传此值，表示获取默认App描述。


### offline:checkOfflineFileWithLocal(filename, appName)

* **Description:**  
  本接口为2.0版本`offline:checkOfflineFile(filename)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。
 
### offline:checkOfflineFileWithLocalH5(filename, appName)

* **Description:**  
  本接口为2.1版本`offline:checkOfflineFileWithLocalH5(filename)`的升级版，功能和用法与其相同。
  1. filename_(required)_ 
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。

### offline:checkOfflineFileWithServer(filename, appName)

* **Description:**  
  本接口为[offline:checkOfflineFileWithServer(filaname)](#offlinecheckofflinefilewithserverfilename)的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。


### offline:checkOfflineFileWithServerH5(filename, appName)

* **Description:**  
 本接口为2.1版本`offline:checkOfflineFileWithServerH5(filename)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_  
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。

### offline:removeOptionalFile(filaname, appName)

* **Description:**  
  本接口为[offline:removeOptionalFile(filaname)](#offlineremoveoptionalfilefilaname)的升级版，功能和用法与其相同。
* **Parameters:**
  1. name_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：需要删除的资源所属的App名称，若不传此值，表示删除默认App中资源。


### offline:commentOfFile(filaname, appName)

* **Description:**  
  本接口为[offline:commentOfFile(filaname)](#offlinecommentoffilefilaname)的升级版，功能和用法与其相同。
* **Parameters:**
  1. filaname_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：获取描述的资源所属的App名称，若不传此值，表示从默认App中获取。


### offline:setResReadAppName(appName)

* **Description:**  
  设置读取离线资源时使用的离线App名称。该接口设置的是一个全局变量，调用该接口后，通过`file:read()`接口、样式外联、lua脚本外联等读取的离线资源文件，都从指定App资源中获取。在再次调用该接口或者程序结束之前，指定的App名称不变。
* **Parameters:**
  1. appName_(appName)_
     - 类型：_String_
     - 说明：指定读取离线资源时的App名称。可以为nil，若为nil，表示默认App资源。
* **Return:**
  无

## 协议4.0版本支持的Lua接口

### offline:update_hash()
废弃该接口

### offline:isEnsUpdateOptional(fileName, optAppName)
* **Description:**  
判断指定文件是否为确保更新的可选资源资源
* **Parameters:**  

	1. fileName_(required)_ 类型：String 可选资源文件名
	2. optAppName_(optional)_ 类型：String 指定appname，默认当前设定的app

* **Return:**  
    类型：Boolean  true：是确保更新 ； false：不是确保更新（没有该资源时为false）

* **Examples:**

    	local isensureupdate = offline:isEnsUpdateOptional("1.zip","emas")；
    	if isensureupdate then
    		window:alert("确保更新资源");
    	end

### offline:cancelUpdateOptional(fileName, optAppName)

* **Description:**  
取消更新该资源
* **Parameters:**  

	1. fileName_(required)_ 类型：String 可选资源插件包名（依.zip结尾）
	2. optAppName_(optional)_ 类型：String 指定appname，默认当前设定的app

* **Return:**  
    无

* **Examples:**

    	local fileName = "1.zip";
    
    	function alertCallback (btnIndex)
    
    		if btnIndex == 0 then -- 点击[是]
    			-- 更新可选插件包
    			
    		elseif btnIndex == 1 then -- 点击[否]
    			offline:cancelUpdateOptional(fileName); -- 设置取消更新该插件包
    		end
    	end
    
    	local localEffective = offline:checkOfflineFileWithLocal("1.zip");
    	local serverEffective = offline:checkOfflineFileWithServer("1.zip");
    	if localEffective then
    		if serverEffective then
    			-- 可选插件包有更新
    			window:alert("是否更新资源？","是","否"，alertCallback);
    		end
    	end





--------------------------------------------

  Date      | Note | Modifier
------------|------|----------
2015-04-07  | 增加离线协议1.1版本相关API | zhou.changjin 
2015-05-15  | 离线协议1.1版本升级为2.0版，原1.1版本取消 | zhou.changjin 
2015-09-08  | 增加离线协议3.0版本相关API | zhou.changjin
2016-03-25  | 增加离线协议4.0版本相关API | liu.dongmei