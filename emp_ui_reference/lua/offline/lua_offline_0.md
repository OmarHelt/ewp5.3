##离线0版本Lua接口
###新增接口：

#### offline:download(callback)

* **Description:**  
  获取服务器端离线资源的描述。只获取描述并分析，不执行真正的资源下载。根据回调方法进行后续操作。**新版本请使用[offline:update_desc(callback)](./lua_offline_1.md#offlineupdatehashcallback)。**
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


#### offline:mustDownload()

* **Description:**  
  调用下载离线资源，不下载可选部分。由于离线流程设计，该接口必须在[offline:download(callback)](#offlinedownloadcallback)接口的回调中调用。**新版本请使用[offline:update_resource()](./lua_offline_1.md#offlineupdateresource)。**
* **Parameters:**  
  无
* **Return:**  
  无


#### offline:downfile(filename, relatedPath, callback)

* **Description:**  
  发起单一可选离线资源下载。可选资源文件名可通过[offline:optDownloadJson()](#offlineoptdownloadjson)接口获取。**新版本请使用[offline:downOptionalFile(filename, callback)](./lua_offline_1.md#offlinedownoptionalfilefilename-callback)。**
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


#### offline:optDownloadJson()

* **Description:**  
  获得当前本地保存的服务器描述文件（server.desc）中所有可选文件描述信息的json字符串（“option_download”字段中所有可选文件的描述信息）。**新版本请使用[offline:getOptInfoInServer()](./lua_offline_1.md#offlinegetoptinfoinserver)。**
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


#### offline:optDownloadJsonInLocal()

* **Description:**  
  获得本地已经下载的可选离线资源的json字符串。**新版本请使用[offline:getOptInfoInLocal()](./lua_offline_1.md#offlinegetoptinfoinlocal)。**
* **Parameters:**  
  无
* **Return:**
  - 类型：_String_
  - 说明：本地已经存在的可选下载的离线资源的列表（json），不包含校验hash值。（“option_download”字段中已经在本地存在并验证通过的的描述信息）。

			{
				"1.zip": "common/zip",
				"login.zip": "android/common/zip"
			}


#### offline:optDownloadDescInLocal()

* **Description:**  
  获得本地已经下载的可选离线资源的资源描述文件（json字符串格式）。**新版本请使用[offline:getOptInfoInLocal()](./lua_offline_1.md#offlinegetoptinfoinlocal)。**
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


#### offline:checkOfflineFile(filename, relatedPath)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。**新版本请使用[offline:checkOfflineFile(filename)](./lua_offline_1.md#offlinecheckofflinefilefilename)。**<br>
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


#### offline:getServerDesc()

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


#### offline:remove(name)

* **Description:**  
  删除指定的文件，用于删除离线下载和插件资源（不区分必选可选)。**新版本请使用[offline:removeOptinalFile(filename)](./lua_offline_1.md#offlineremoveoptionalfilefilaname)。**
* **Parameters:**
  1. name_(required)_
     - 说明：文件名称（带后缀），或者文件夹（无后缀）。指定路径（带“/”）的文件按插件内单个文件处理，在插件目录下查找文件删除；文件夹或者zip后缀名文件删除对应插件目录；其余文件在离线目录下查找文件删除。
* **Return:**  
  无
* **Examples:**

		offline:remove(“vv/home.xml”); -- 将名为vv的插件下的“home.xml”文件删除。
		offline:remove(“vv”); -- 将名为vv的插件删除。
		offline:remove(“uu.lua”); -- 将离线资源“uu.lua”文件删除。
