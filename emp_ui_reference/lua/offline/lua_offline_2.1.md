###离线2.1新增接口
#### offline:checkOfflineFileWithServerH5(filename)

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
 

     
     
#### offline:checkOfflineFileWithLocalH5(filename)

* **Description:**  
  检测本地H5离线资源文件是否有效。
 - 有效定义： 客户端本地存在对应文件，并且和本地存储的已下载必选文件描述一致。
* **Parameters:**
  1. filename_(required)_  
     - H5资源的文件路径 + 文件名。
* **Return:**
     - 类型：Boolean
     - 说明：true表示当前文件有效，false表示当前文件无效