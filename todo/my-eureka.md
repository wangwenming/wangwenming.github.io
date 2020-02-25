<renewalIntervalInSecs>30</renewalIntervalInSecs>
<durationInSecs>90</durationInSecs>
<registrationTimestamp>1572946568895</registrationTimestamp>
<lastRenewalTimestamp>1572946568895</lastRenewalTimestamp>
<evictionTimestamp>0</evictionTimestamp>
<serviceUpTimestamp>1572946568113</serviceUpTimestamp>



初次注册：
"leaseInfo":{"renewalIntervalInSecs":30,"durationInSecs":90,"registrationTimestamp":0,"lastRenewalTimestamp":0,"evictionTimestamp":0,"serviceUpTimestamp":0}

去查询：
<renewalIntervalInSecs>30</renewalIntervalInSecs>
<durationInSecs>90</durationInSecs>
<registrationTimestamp>1572947078240</registrationTimestamp>
<lastRenewalTimestamp>1572947078240</lastRenewalTimestamp>
<evictionTimestamp>0</evictionTimestamp>
<serviceUpTimestamp>1572947077282</serviceUpTimestamp>

过一段时间查询：
<renewalIntervalInSecs>30</renewalIntervalInSecs>
<durationInSecs>90</durationInSecs>
<registrationTimestamp>1572947078240</registrationTimestamp>
<lastRenewalTimestamp>1572947078240</lastRenewalTimestamp>
<evictionTimestamp>0</evictionTimestamp>
<serviceUpTimestamp>1572947077282</serviceUpTimestamp>

暂停刷新 status = overriddenstatus = OUT_OF_SERVICE
1572947347610

首次暂停才记录 lastRenewalTimestamp 时间

恢复成 status = UP overriddenstatus= UNKNOWN， 1572947606660

暂停
1572947680050
恢复
1572947742053

<registrationTimestamp>1572949512354</registrationTimestamp>
<lastRenewalTimestamp>1572949512354</lastRenewalTimestamp>
<evictionTimestamp>0</evictionTimestamp>
<serviceUpTimestamp>1572949511828</serviceUpTimestamp>



<serviceUpTimestamp>1572949964458</serviceUpTimestamp>
<serviceUpTimestamp>1572949964458</serviceUpTimestamp>
<serviceUpTimestamp>1572949964458</serviceUpTimestamp>


recentlyChangedQueue

RecentlyChangedItem
	lastUpdateTime
	leaseInfo


这些场景改变队列：
deleteStatusOverride
clearRegistry() 重置
getApplicationDeltas() 读取
internalCancel() 添加一个 
register() 添加一个
getDeltaRetentionTask 定时器删除
statusUpdate 添加


http://localhost:8088/eureka/apps/delta

才返回 /delta

真的只返回 TEST-EUREKA-SERVICE-CONSUMER 这一个

<versions__delta>9</versions__delta>
<apps__hashcode>OUT_OF_SERVICE_1_UP_1_</apps__hashcode>

getRetentionTimeInMSInDeltaQueue 
3 * 60 * 1000 

默认3分钟回收 delta



node_modules//@vue/cli-plugin-babel

  "dependencies": {
    "@babel/core": "^7.6.4",
    "@vue/babel-preset-app": "^4.0.5",
    "babel-loader": "^8.0.6",
  },


95490字节
95464 false
95455


@babel/polyfill 7.7.0 Provides polyfills necessary for a full ES2015+ environment
babel-polyfill 6.26.0 Provides polyfills necessary for a full ES2015+ environment 2年前更新






