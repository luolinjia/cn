---
layout: post
title: Java版iPhone批量推送
logo: http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/7510EA6756ED_zpsa7b5f3cd.jpg
categories:
- Study
- Work
tags:
- 手機推送
- Java
- iPhone
- 证书
---

> 据網上版本進行更改  

iPhone推送基本原理流程圖  

![iPhone Push](http://i1154.photobucket.com/albums/p531/luolinjia/blog%20images/acd75433-36fc-3d47-92fa-a68f9ff19cf7_zps928c75b3.jpg)  

從上圖我們可以看到。 

　　1、首先是應用程序註冊推送。 

　　2、 IOS跟APNS Server要deviceToken。應用程序接受deviceToken。 

　　3、應用程序將deviceToken發送給PUSH服務端程序。 

　　4、服務端程序向APNS服務發送消息。 

　　5、APNS服務將消息發送給iPhone應用程序。 

> 依賴包：  

bcprov-jdk16-145-1.jar  

commons-io-2.0.1.jar  

commons-lang-2.5.jar  

log4j-1.2.16.jar  

javapns-jdk16-163.jar  


> java 代碼：  

{% highlight java %}
	/**
	 * 向iphone群組推送消息.(單個推送也可)
	 * @author Karl.luo at 2013-06-14
	 * @param deviceTokens
	 *            iphone手機獲取的token
	 * @param content
	 *            推送內容
	 * @param dict
	 * 			  需要添加的字典
	 */
	public static String mobilePushiPhone(List<String> deviceTokens,
			String content, String dict) {
		// apple service 此host為測試地址
		String host = "gateway.sandbox.push.apple.com"; 
		int port = 2195; // 端口
		// 文件地址，可以根據自己的實際情況拿到證書文件地址
		String filePos = (TipsDao.class.getResource("/") + "developer.p12")
				.substring(6); 
		log.debug(TipsDao.class.getResource("/"));
		// 證書文件密碼
		String fileCode = "111111"; 
		try {
			PayLoad payLoad = new PayLoad();
			payLoad.addAlert(content);// push的内容
			payLoad.addBadge(1);// 應用圖標上小紅圈的數值
			payLoad.addSound("default");// 鈴聲

			// 添加字典
			payLoad.addCustomDictionary("url", dict);
			PushNotificationManager pushManager = PushNotificationManager
					.getInstance();

			// 鏈接到APNs
			pushManager.initializeConnection(host, port, filePos, fileCode,
					SSLConnectionHelper.KEYSTORE_TYPE_PKCS12);

			// 開始循環推送
			for (int i = 0; i < deviceTokens.size(); i++) {
				pushManager.addDevice("iphone" + i, deviceTokens.get(i));
				Device client = pushManager.getDevice("iphone" + i);
				pushManager.sendNotification(client, payLoad);
			}
			// 斷開鏈接
			pushManager.stopConnection();
			for (int i = 0; i < deviceTokens.size(); i++) {
				pushManager.removeDevice("iphone" + i);
			}
			log.info("iphone 推送消息成功");
			return "0000";
		} catch (Exception e) {
			log.error("(ESB-Exception)异常：iphone推送消息異常", e);
			return "9999";
		}
	}
{% endhighlight %}   

> Ps:最重要的就是一定要保證證書文件的正確性，此上方法即可適用單個推送，也可滿足群組推送

