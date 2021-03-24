---
title: "Team+通知自己來"
tagline: "Team+"
header:
  overlay_image: /assets/images/headerIMG01.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
classes: wide
excerpt: "Team+網頁版使用網頁推播"
tags:
  - code
  - Team+
---

## 前言
目前公司正在使用的IM(Instant Message)軟體是Team+，他提供手機(Android、IOS)、桌面版、網頁版
> 這邊不是推銷沒有業配啦

桌面版使用下來莫名地佔用CPU，初估可能是他的架構設計的關係
> 開了一堆chromium去run，超佔資源的啦

逼得我必須使用網頁版，但網頁版的通知又弱弱的，並沒有導入Notification API，讓我常常忽略訊息被人念!
所以下面介紹一下如何自己加通知，既輕量又能達到目的~~


## 準備工具
1. Tampermonkey
2. Browser
> 以下例子使用Chrome版本86.0.4240.75 64bit
> 實際操作可能會因為不同瀏覽器版本需要做調整

## 開始設定
1. 步驟一：設定Chrome
因為公司的Team+是架設在內網，所以被Chrome認定為不安全網域，沒辦法使用Notification API，可參考[官方說明](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins)
![設定](https://i.imgur.com/GCAVSkG.png)
	### 直接畫重點
   1. 前往Chrome進階設定-chrome://flags/#unsafely-treat-insecure-origin-as-secure)
   2. 加入Team+網址
		![Chrome設定](https://i.imgur.com/kLUhFLg.png)
	3. 重啟Chrome
	4. 該網站就可以使用Notification API啦
![啟用通知](https://i.imgur.com/zzjJ5PG.png)

2. 步驟二：編寫腳本
	簡單說明一下原理
	1. 讀取頁面通知Tag，抓取未讀通知數
	2. 將未讀數已通知方式傳送給使用者知道
	
	### Tampermonkey步驟：
	3. [安裝套件](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=zh-TW)
	
	4. 新增腳本
	![新增腳本](https://i.imgur.com/STbLQiI.png)
	5.  貼上程式碼(每10秒檢查一次)
	 
			(function() {
		    'use strict';
		    if (('Notification' in window)) {
		        let currcount = 0;
		        setInterval(()=>{
		            const notice = document.querySelectorAll('.noticeBadge')
		            for(let i = 0; i < notice.length;i++){
		                currcount = parseInt(notice[i].innerText);
		                if(currcount>0){
		                    showNotify(currcount);
		                    break;
		                }
		            }
		        }, 10000)
		    }

		    const notifyConfig = {
		        body: '您有新的訊息，請回復!', // 設定內容
		        icon: '../Images/chatsbg.png', // 設定 icon
		        tag:'Team+Notification',// 標籤
		        renotify: true, // 重新通知
		    };

		    function showNotify(num = '*'){
		        Notification.requestPermission(function(permission) {
		            if (permission === 'granted') {
		                // 使用者同意授權
		                var notification = new Notification(`Team+ Message[${num.toString()}]`, notifyConfig); // 建立通知
		            }
		        });
		    }
			})();
			
3. 步驟三：效果確認
![效果](https://i.imgur.com/jYIkHhP.png)

### 完成!!!


> 參考資料
> [Notification（通知）：利用 JavaScript 實作瀏覽器推播通知](https://cythilya.github.io/2017/07/09/notification/#comment-3670533584)
> [Deprecating Powerful Features on Insecure Origins](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins)
>[Notifications API](https://notifications.spec.whatwg.org/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyNDY3NjMzMl19
-->