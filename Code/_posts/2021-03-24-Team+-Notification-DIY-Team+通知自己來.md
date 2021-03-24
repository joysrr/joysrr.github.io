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
> 開了一堆chromium去run，超佔資源的QQ

逼得我必須使用網頁版，但網頁版的通知又弱弱的，並沒有導入Notification API，讓我常常忽略訊息被人念!
所以下面介紹一下如何自己加通知，既輕量又能達到目的~~


## 準備工具
1. Tampermonkey
2. Browser
> 以下例子使用Chrome版本86.0.4240.75 64bit
> 實際操作可能會因為不同瀏覽器版本需要做調整

## 開始設定
●步驟一：設定Chrome
因為公司的Team+是架設在內網，所以被Chrome認定為不安全網域，沒辦法使用Notification API，可參考[官方說明](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins)
![設定](https://i.imgur.com/GCAVSkG.png)
	
#### 直接畫重點
	
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
	
	#### Tampermonkey步驟：
	
	1. [安裝套件](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=zh-TW)
	
	2. 新增腳本
	
		![新增腳本](https://i.imgur.com/STbLQiI.png)
	
	3.  貼上程式碼
	
		#### 註解有大概說明程式碼內容，記得要把http://tp.xxx.com.tw改成自己的連結，不然點擊會失效~
		
			// ==UserScript==
			// @name         Team+ Notification
			// @namespace    http://tampermonkey.net/
			// @version      0.1
			// @description  Show Team+ Notification by browser
			// @author       joysrr
			// @match        http://tp.xxx.com.tw/*
			// @grant        none
			// ==/UserScript==
			(function() {
			    'use strict';
			    let unreadcount = 0,
			        title = '';
			    setInterval(() => {
			        // 當頁面不在前台時才跳出通知
			        if (checkNotifyOn() && document.visibilityState === 'hidden') {
			            const chatView = document.getElementById('divPersonalLogSideBar')
			            if (chatView) {
			                //chat page
			                const unreadList = chatView.querySelectorAll('.unreadCount');
			                unreadList.forEach(unread => {
			                    unreadcount = unread.innerText;
			                    title = unread.parentElement.querySelector('.chatName').title;
			                    showNotify(unreadcount, title);
			                })
			            } else {
			                // other page
			                const notice = document.querySelectorAll('.noticeBadge')
			                for (let i = 0; i < notice.length; i++) {
			                    unreadcount = parseInt(notice[i].innerText);
			                    if (unreadcount > 0) {
			                        showNotify(unreadcount);
			                        break;
			                    }
			                }
			            }
			        }
			    }, 8000)

			    function checkNotifyOn() {
			        if (window.Notification && Notification.permission === "granted") {
			            return true;
			        } else if (window.Notification && Notification.permission !== "denied") {
			            Notification.requestPermission().then(status => {
			                if (status === "granted") {
			                    var n = new Notification("Team+ Notification", {
			                        body: 'Congratulations! Now you can use Team+ Notification',
			                        icon: '../Images/chatsbg.png',
			                        tag: 'Team+Notification'
			                    });
			                    return true;
			                } else {
			                    alert("Oops! Plz grant Notification authority!");
			                }
			            }).catch(error => {
			                console.log(error);
			            })
			        } else {
			            alert("So sad! seems like your browser didn't support Notification QQ");
			        }
			    }

			    // url要替換成自己Team+訊息的網址喔~
			    function showNotify(unread = '*', tag = 'Team+Notification', url = 'http://tp.xxx.com.tw/EIM/Chat/ChatMain.aspx') {
			        var notification = new Notification(`Team+[${tag=='Team+Notification'?unread:tag}]`, {
			            body: `您有${unread}則新的訊息，請回覆!`, // 設定內容
			            icon: '../Images/chatsbg.png', // 設定 icon
			            tag: tag, // 標籤
			            renotify: true, // 重新通知
			        }).onclick = function(e) { // 點擊
			            e.preventDefault();
			            window.open(url); // 打開Team+訊息視窗
			        };

			        return notification;
			    }
			})();
			
3. 步驟三：效果確認

	現在你可以享受流暢的電腦並且也可以即時收發訊息，
	訊息提供發訊息者、幾則未讀，點擊後可連結至Team+訊息頁面

	![效果](https://i.imgur.com/Wo25uBh.png)

### 完成!!!

## 結語
這只是一個簡單的小工具，避免你不回訊息被人盯上......
程式碼有放上[GitHub](https://github.com/joysrr/Teamplus-NotificationWeb)歡迎大家提出想法意見一起改進喔~

-----
> 參考資料
> [Notification（通知）：利用 JavaScript 實作瀏覽器推播通知](https://cythilya.github.io/2017/07/09/notification/#comment-3670533584)
> [Deprecating Powerful Features on Insecure Origins](https://sites.google.com/a/chromium.org/dev/Home/chromium-security/deprecating-powerful-features-on-insecure-origins)
>[Notifications API](https://notifications.spec.whatwg.org/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwNzA4MDM0NSwtMTQ1Nzc2MTAxOSwxMD
gxMjAyNjEsLTE2MTIzODUwNDEsMTU0Nzk2NzUxMSwtOTMwNjUy
MzA0LDQ4NTQ4NDY3OCwtMTA4MzA2MjMxMCwtMTY2NDQxNTEyOS
wtMTY2ODE2NTc3NiwtMTcxNjA3ODk0MF19
-->