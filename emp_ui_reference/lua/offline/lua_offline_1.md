##离线1版本Lua接口

###变更接口
	
#### offline:checkOfflineFile(filename)

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
  
###废弃接口  
  
#### offline:remove(name)
**请使用[offline:removeOptionalFile(filename)](#offlineremoveoptionalfilefilaname)。**

#### offline:optDownloadJsonInLocal()
**请使用[offline:getOptInfoInLocal()](#offlinegetoptinfoinlocal)。**

#### offline:optDownloadDescInLocal()
**请使用[offline:getOptInfoInLocal()](#offlinegetoptinfoinlocal)。**

#### offline:downfile(filename, relatedPath, callback)
**请使用[offline:downOptionalFile(filename, callback)](#offlinedownoptionalfilefilename-callback)。**
#### offline:mustDownload()
**请使用[offline:update_resource()](#offlineupdateresource)。**
#### offline:download(callback)
**请使用[offline:update_desc(callback)](#offlineupdatehashcallback)。**

###新增接口：

#### offline:update_hash(callback)

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
  
#### offline:update_desc(callback)

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
  
#### offline:update_resource()

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
		
#### offline:downOptionalFile(filename, callback)

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

#### offline:getOptInfoInServer()

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

#### offline:getOptInfoInLocal()

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
			
#### offline:removeOptionalFile(filaname)

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
		
#### offline:commentOfFile(filaname)

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
		
