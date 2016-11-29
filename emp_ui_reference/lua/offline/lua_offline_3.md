##离线3版本Lua接口

###变更接口
#### offline:update_hash(callback, parameter)

* **Description:**  
  本接口为`offline:update_hash(callback)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. callback_(required)_
  2. parameter_(optional)_
     - 类型：Table
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数

#### offline:update_desc(callback, parameter)

* **Description:**  
  本接口为`offline:update_desc(callback)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. callback_(required)_
  2. parameter_(optional)_
     - 类型：_Table_
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数。
	   
#### offline:update_resource(parameter)

* **Description:**  
  本接口为`offline:update_resource(params)`的升级版，功能和用法与其相同。
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
			
			
#### offline:downOptionalFile(filename, callback, parameter)

* **Description:**  
  本接口为`offline:downOptionalFile(filename, callback)`的升级版，功能和用法与其相同。
* **Parameters:**  
  1. filename_(required)_
  2. callback_(optional)_
  3. parameter_(optional)_
     - 类型：_Table_
     - 说明：若不传此值，表示更新默认App资源。
     
	   示例：{appname = "emas", info = "message"}appname：请求更新App名称；info：请求参数。
	   
	   
#### offline:getOptInfoInServer(appName)

* **Description:**  
  本接口为`offline:getOptInfoInServer()`的升级版，功能和用法与其相同。
* **Parameters:**  
  1. appName_(optional)_
     - 类型：_String_
     - 说明：需要获取描述的App名称，若不传此值，表示获取默认App描述。


#### offline:getOptInfoInLocal(appName)

* **Description:**  
  本接口为`offline:getOptInfoInLocal()`的升级版，功能和用法与其相同。
* **Parameters:**  
  1. appName_(optional)_
     - 类型：_String_
     - 说明：需要获取描述的App名称，若不传此值，表示获取默认App描述。
     
#### offline:checkOfflineFileWithLocalH5(filename, appName)

* **Description:**  
  本接口为2.1版本`offline:checkOfflineFileWithLocalH5(filename)`的升级版，功能和用法与其相同。
  1. filename_(required)_ 
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。


#### offline:checkOfflineFileWithServer(filename, appName)
0，1，2
* **Description:**  
  本接口为`offline:checkOfflineFileWithServer(filaname)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。
     
     
#### offline:checkOfflineFileWithServerH5(filename, appName)

* **Description:**  
 本接口为2.1版本`offline:checkOfflineFileWithServerH5(filename)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_  
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。


#### offline:removeOptionalFile(filaname, appName)

* **Description:**  
  本接口为`offline:removeOptionalFile(filaname)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. name_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：需要删除的资源所属的App名称，若不传此值，表示删除默认App中资源。
     
     
#### offline:commentOfFile(filaname, appName)

* **Description:**  
  本接口为`offline:commentOfFile(filaname)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filaname_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：获取描述的资源所属的App名称，若不传此值，表示从默认App中获取。
#### offline:checkOfflineFileWithLocal(filename, appName)

* **Description:**  
  本接口为1.0版本`offline:checkOfflineFile(filename)`的升级版，功能和用法与其相同。
* **Parameters:**
  1. filename_(required)_
  2. appName_(optional)_
     - 类型：_String_
     - 说明：被检查资源所属的App名称，若不传此值，表示在默认App中检查。
     

###新增接口
      
#### offline:setResReadAppName(appName)

* **Description:**  
  设置读取离线资源时使用的离线App名称。该接口设置的是一个全局变量，调用该接口后，通过`file:read()`接口、样式外联、lua脚本外联等读取的离线资源文件，都从指定App资源中获取。在再次调用该接口或者程序结束之前，指定的App名称不变。
* **Parameters:**
  1. appName_(appName)_
     - 类型：_String_
     - 说明：指定读取离线资源时的App名称。可以为nil，若为nil，表示默认App资源。
* **Return:**
  无
  