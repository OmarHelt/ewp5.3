##离线4版本Lua接口

###废弃接口
#### offline:update_hash()
**废弃该接口**

###新增接口
#### offline:isEnsUpdateOptional(fileName, optAppName)
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
    	
    	
#### offline:cancelUpdateOptional(fileName, optAppName)

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