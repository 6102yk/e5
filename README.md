# E5續訂程序
此項目為該網址的源代碼(後端) https://e5.qyi.io/
(前端) https://github.com/luoye663/e5-html
### 2020-12-20
前端框架更改為Angular,同時支持多應用，每個賬戶最多支持5個應用。
## 說明
此項目為我的新手練手作，代碼辣雞，目前已經從3月份運行到至今。  
如果要自己搭建的話得自己研究下了，不提供技術支持(懶)，記得修改配置文件 
src/main/resources/application-online.properties 
```
user.admin.githubId  - 自己的github id  
數據庫配置  
redis配置  
Rabbit配置  
github.client_id  
github.client_secret  
(這兩個在https://github.com/settings/developers 申請一個apps就行了。)
```
## 注意事項
由於懶癌发作，在程序啟動或者重啟，是不會主動把數據庫里面的用戶加入隊列，所以得手動處理。
1. 在每次啟動程序前，先清空延遲隊列  
```
rabbitmq-plugins disable rabbitmq_delayed_message_exchange
rabbitmq-plugins enable rabbitmq_delayed_message_exchange  
```
由於這個插件只能先禁用在啟用，才能進行清空。  
2. 在每次啟動程序前，清空未完成的隊列。  
在rabbitmq web管理界面 - Queues - delay_queue1 - Purge - Purge Messages  
3. ~~啟動後清空redis~~  
4. 登錄後使用http訪問工具訪問  https://domain.com/admin/sendAll 這個鏈接，設置一個token頭，為網站登錄後的token，f12 看請求(需要設置的管理員github id訪問才有能訪問)。  
ps: 使用  https://domain.com/admin/getDebugAdminToken?passwd=xxxxxx 也可以獲取管理員token，前提是在配置文件中設置密碼。  
主要目的是方便調試，所以沒有啟動就將所有用戶加入隊列(因為rabbitmq插件問題，清空延時隊列得先禁用、用插件) so......需要手動處理......  
##### 如果不按照以上的來，會出現莫名其妙的問題~

## 用到技術或框架
### spring boot  

### rabbitMq  
需要安裝rabbitmq_delayed_message_exchange插件  
同時新建一個用戶來對接此程序  
清空延時隊列方法:
```
rabbitmq-plugins disable rabbitmq_delayed_message_exchange
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
```

### Redis
默認用1庫，可自行在配置文件修改  

### Mysql
自行導入sql  
沒有寫清空日志功能，後面加上。  
按道理說日志因該存到MongoDB里，所以？
### Mybatis Plus

### Spring Security
權限配置由於就那麽幾個，所以就沒寫到mysql里面。
### log4j2
日志框架

## 鳴謝

> [IntelliJ IDEA](https://zh.wikipedia.org/zh-hans/IntelliJ_IDEA) 是一個在各個方面都最大程度地提高開发人員的生產力的 IDE，適用於 JVM 平台語言。

特別感謝 [JetBrains](https://www.jetbrains.com/?from=) 為開源項目提供免費的 [IntelliJ IDEA](https://www.jetbrains.com/idea/?from=) 等 IDE 的授權  
[<img src=".github/jetbrains-variant-3.png" width="200"/>](https://www.jetbrains.com/)
