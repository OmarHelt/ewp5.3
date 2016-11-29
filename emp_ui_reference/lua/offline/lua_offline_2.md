##离线2版本Lua接口


###接口变更
#### offline:update_resource(params)

* **Description:**  
  根据offline:update_desc获取的下载资源描述，下载资源。此接口仅下载必选资源，可选资源的下载通过opt系列接口处理。本接口为[offline:update_resource()](./lua_offline_1.md#offlineupdateresource)接口的升级版。
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



###新增接口

#### offline:checkOfflineFileWithServer(filename)

* **Description:**  
  检查本地离线资源文件是否有效，对可选必选可统一判断。<br>
  有效的定义：客户端本地存在对应文件，并且和本地存储的最新服务器必选/可选资源描述一致。
* **Parameters:**
  1. filename_(required)_
     - 说明：当前资源文件名。如果是单个资源或者插件包资源，则只需要文件名；如果是插件包内单个资源，则需要针对插件包目录的相对路径+文件名。
* **Return:**
  - 类型：_Boolean_
  - 说明：true表示当前文件有效，false表示当前文件无效。