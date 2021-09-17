# AutoApiP ———— E5自动续期


## 说明 ##
* E5自动续期程序，但是**不保证续期**
* 设置了**周六日(UTC时间)不启动**自动调用，周1-5每6小时自动启动一次 （修改看教程）

### 相关 ###
* AutoApiP：https://github.com/wangziyingwen/AutoApiP   已失效 
* AutoApiSecret：https://github.com/jing5460/AutoApiSecret  详细教程
* **错误及解决办法/续期相关知识/更新日志**：
   * P版错误说明已更新进程序，详细请运行后看action日志报告
* 视频教程：
   * B站：https://www.bilibili.com/video/BV185411n7Mq/

## 步骤 ##
* 准备工具：
   * E5开发者账号（管理员、子号均可，非个人/私人账号）
   * rclone软件，[下载地址 rclone.org ](https://downloads.rclone.org/v1.53.3/rclone-v1.53.3-windows-amd64.zip)，(windows 64）
   * 教程图片看不到请科学上网
   
* 步骤大纲：
   * 微软方面的准备工作 （获取应用id、密码、密钥）
   * GIHTHUB方面的准备工作  （获取Github密钥、设置secret）
   * 试运行
   
#### 微软方面的准备工作 ####

* **第一步，注册应用，获取应用id、secret**

    * 1）点击打开[仪表板](https://aad.portal.azure.com/)，左边点击**所有服务**，找到**应用注册**，点击+**新注册**
    
    
    * 2）填入名字，受支持账户类型前三任选，重定向填入 http://localhost:53682/ ，点击**注册**
    
    
    * 3）复制应用程序（客户端）ID到记事本备用(**获得了应用程序ID**！)，点击左边管理的**证书和密码**，点击+**新客户端密码**，点击添加，复制新客户端密码的**值**保存（**获得了应用程序密码**！）
    
    
    * 4）点击左边管理的**API权限**，点击+**添加权限**，点击常用Microsoft API里的**Microsoft Graph**(就是那个蓝色水晶)，
    点击**委托的权限**，然后在下面的条例选中下列需要的权限，最后点击底部**添加权限**
    
    **赋予api权限的时候，选择以下几个**
  
                Calendars.ReadWrite、Contacts.ReadWrite、Directory.ReadWrite.All、
                
                Files.ReadWrite.All、MailboxSettings.ReadWrite、Mail.ReadWrite、
                
                Notes.ReadWrite.All、People.Read.All、Sites.ReadWrite.All、
                
                Tasks.ReadWrite、User.ReadWrite.All
    
    
    * 5）添加完自动跳回到权限首页，点击**代表授予管理员同意**
         
         如若是**子号**运行，请用管理员账号登录[仪表板](https://aad.portal.azure.com/)找到**子号注册的应用**，点击“代表管理员授权”。 
    
    
* **第二步，获取refresh_token(微软密钥)**

    * 1）rclone.exe所在文件夹，shift+右键，在此处打开powershell，输入下面**修改后**的内容，回车后跳出浏览器，登入e5账号，点击接受，回到powershell窗口，看到一串东西。
           
                ./rclone authorize "onedrive" "应用程序(客户端)ID" "应用程序密码"
               
    * 2）在那一串东西里找到 "refresh_token"：" ，从双引号开始选中到 ","expiry":2021 为止（就是refresh_token后面双引号里那一串，不要双引号），如下图，右键复制保存（**获得了微软密钥**）
    
    
 ____________________________________________________
 
 #### GITHUB方面的准备工作 ####

 * **第一步，fork本项目**
 
     登陆/新建github账号，回到本项目页面，点击右上角fork本项目的代码到你自己的账号，然后你账号下会出现一个一模一样的项目，接下来的操作均在你的这个项目下进行。
     
     
 * **第二步，新建github密钥**
 
    * 1）进入你的个人设置页面 (右上角头像 Settings，不是仓库里的 Settings)，选择 Developer settings -> Personal access tokens -> Generate new token

    
    * 2）设置名字为 **GH_TOKEN** , 然后勾选repo，点击 Generate token ，最后**复制保存**生成的github密钥（**获得了github密钥**，一旦离开页面下次就看不到了！）
   
  
 * **第三步，新建secret**
 
    * 1）依次点击页面上栏右边的 Setting -> 左栏 Secrets -> 右上 New repository secret，新建4个secret： **GH_TOKEN、MS_TOKEN、CLIENT_ID、CLIENT_SECRET**  
   
    
     **(以下填入内容注意前后不要有空格空行)**
 
     GH_TOKEN
     ```shell
     github密钥 (第三步获得)，例如获得的密钥是abc...xyz，则在secret页面直接粘贴进去，不用做任何修改，只需保证前后没有空格空行
     ```
     MS_TOKEN
     ```shell
     微软密钥（第二步获得的refresh_token）
     ```
     CLIENT_ID
     ```shell
     应用程序ID (第一步获得)
     ```
     CLIENT_SECRET
     ```shell
     应用程序密码 (第一步获得)
     ```
________________________________________________

#### 试运行 ####
   
   * 1）点击两次右上角的星星（star，就是fork按钮的隔壁）启动action，再点击上面的Action，选择Auto Api Pro 就能看到每次的运行日志，看看运行状况

   （必需点进去build里面的run api看下，api有没有调用到位，有没有出错。外面的Auto Api打勾只能说明运行是正常的，我们还需要确认api调用成功了，就像图里的一样）
   
   
     
   * 2）再点两次星星，查看是否能再次成功运行
 
        同时，依次点击页面上栏右边的 Setting -> 左栏 Secrets（也就是Github方面准备的第三步的secret页面），应该能看到MS_TOKEN显示刚刚update了
        
        （这一步是为了保证重新上传到secret的token是正确的）
        
#### 教程最后 ####

   程序会自行按计划启动，不必操心。
   
   但是github更新了防止薅羊毛的规则，如果仓库60天无任何变动，将会暂停Action，但是会发邮件通知，所以请留意邮箱，收到邮件请上来手动启动一下action。
   （我还没有收到过此邮件，但是据说邮件里会有启动链接，或者上来按两次星星按钮就行）
   
### 教程完 ###

__________________________________________________________________________

## 额外设置 （看不懂请忽略）##
   * **定时启动修改**

   * **多账号/应用支持**
    
   * **超级参数设置**

#### 定时启动修改 ####
   
   我设定的每6小时自动运行一次（周六日不启动），每次调用3轮（点击右上角星星/star也可以立马调用一次），你们自行斟酌修改（我也不知道保持活跃要调用多少次、多久）：

  * 定时自动启动修改地方：在.github/workflow/autoapi.yml(只修改这一个)文件里，自行百度cron定时任务格式，最短每5分钟一次
   
    
#### 多账号/应用支持 ####

   如果想输入第二账号或者应用，请按上述步骤获取**第二个应用的id、密码、微软密钥：**
 
   再按以下步骤：
 
   1)增加secret
 
   依次点击页面上栏右边的 Setting -> 左栏 Secrets -> 右上 New repository secret，新增加secret：APP_NUM、MS_TOKEN_2、CLIENT_ID_2、CLIENT_SECRET_2
 
   APP_NUM
   ```shell
   账号/应用数量(现在例如是两个账号/应用，就是2 ；3个账号就填3，日后如果想要增加请修改APP_NUM)
   ```
   MS_TOKEN_2
   ```shell
   第二个账号的微软密钥（第二步refresh_token），（第三个账号/应用就是MS_TOKEN_3，如此类推）
   ```
   CLIENT_ID_2
   ```shell
   第二个账号的应用程序ID (第一步获取),（第三个账号/应用就是CLIENT_ID_3，如此类推）
   ```
   CLIENT_SECRET_2
   ```shell
   第二个账号的应用程序密码 (第一步获取),（第三个账号/应用就是CLIENT_SECRET_3，如此类推）
   ```
   
   2)修改.github/workflows/里的两个yml文件（**超过5个账号需要更改，5个及以下暂时不用修改文件，忽略这一步**）
    
   yml文件我已经注明了，看着改就行，我已经写入5个账号模板了，跟着复制粘贴很简单的（没有找到比较好的自动方案）
  
#### 超级参数设置 ####
 
   runapi.py 文件第11行有个config_list，里面是以下参数配置
     
   · 轮数：
              
             就是一次运行要跑多少轮api，也就是启动一次会重复跑几圈
    
   · 是否启动随机时间（默认关闭）：
            
            这个是每一轮结束，要不要等一个随机时间再开始调用下一轮。后面两个参数就是生成随机时间的，例如设置600，1200，就会延时600-1200s之间。
    
   · 是否开启随机api顺序（默认开启）：
            
            不开启就是初版10个api，固定顺序。开启就是28个api抽12个随机排序。
    
   · 是否开启各api延时（默认关闭）：
            
            这个是每个api之间要不要开启延时。后面两参数参考“随机时间”
    
   · 是否开启各账号延时（默认关闭）：
   
            这个是每个账号/应用之间要不要开启延时。后面两参数参考“随机时间”
    
   （延时的设置是会延长运行时间的，全关闭大概每次运行1min，开启就会适当延长）
   ![image](https://user-images.githubusercontent.com/38358681/133702634-de33fef2-8324-418e-a939-eb3e9b6d6adf.png)
   ![image](https://user-images.githubusercontent.com/38358681/133702570-3162f730-6461-4571-8a3a-1d6557a9d49d.png))
   ![image](https://user-images.githubusercontent.com/38358681/133703066-fe22bf8a-0c13-4af2-a0e8-3f92080a3eee.png)
   ![image](https://user-images.githubusercontent.com/38358681/133703073-139b2f69-1a15-4d10-9352-8cc64469bced.png)
   ![image](https://user-images.githubusercontent.com/38358681/133703085-d8fdfd44-9fb5-4639-a090-ec413fd16177.png)
   ![image](https://user-images.githubusercontent.com/38358681/133703188-992b5c21-66bf-4373-adfb-8acdcf5faa8f.png)
   ![image](https://user-images.githubusercontent.com/38358681/133703133-99d84f26-a292-4c82-ac41-3f48375dad03.png)
   ![image](https://user-images.githubusercontent.com/38358681/133706592-0d9ee9aa-0ce2-404b-b678-f289ac362cbb.png)
   ![image](https://user-images.githubusercontent.com/38358681/133706615-b1008dde-698e-4646-b896-5f0bbfed0871.png)
   ![image](https://user-images.githubusercontent.com/38358681/133706713-44a93b36-8322-46d3-aa16-1518cff4dbd9.png)
   ![image](https://user-images.githubusercontent.com/38358681/133706781-dd99ed88-9c32-47fa-913c-1028a77c6623.png)
   ![image](https://user-images.githubusercontent.com/38358681/133706863-b8af17d2-cb9e-4d06-9ea3-d7c818162cf6.png)
  
  ![image](https://user-images.githubusercontent.com/38358681/133708137-bf28d529-2bf3-4b01-b903-80957b57a0f5.png)

![image](https://user-images.githubusercontent.com/38358681/133706934-8397a403-d2fd-4068-96ce-966421380716.png)
![image](https://user-images.githubusercontent.com/38358681/133706983-35467abc-9f1e-4a94-81c4-707d40ed34da.png)
![image](https://user-images.githubusercontent.com/38358681/133707061-0a3ba554-0981-4251-9be0-4d41cbb46f47.png)
![image](https://user-images.githubusercontent.com/38358681/133708056-ad2b3063-39db-4e45-b23b-874fc913ebcd.png)

![image](https://user-images.githubusercontent.com/38358681/133708189-c354e281-3e59-4ded-87c6-4646fbcb9317.png)

### 结尾 ###

有事发issue

Q群：[56865213](https://jq.qq.com/?_wv=1027&k=jiLznNS2)  （项目相关讨论）

                             
    




