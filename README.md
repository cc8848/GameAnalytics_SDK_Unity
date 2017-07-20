## TalkingData Game Analytics Unity SDK
Game Analytics Unity 平台 SDK 由`封装层`和 `Native SDK` 两部分构成，目前GitHub上提供了封装层代码，需要从 [TalkingData官网](https://www.talkingdata.com/spa/sdk/#/config) 下载最新版的 Android 和 iOS 平台 Native SDK，组合使用。

### 集成说明
1. 下载本项目（封装层）到本地；  
2. 访问 [TalkingData官网](https://www.talkingdata.com/spa/sdk/#/config) 下载最新版的 Android 和 iOS 平台 Game Analytics SDK（ Native SDK）
	- 方法1：选择 Unity 平台进行功能定制；
	- 方法2：分别选择 Android 和 iOS 平台进行功能定制，请确保两个平台功能项一致；  
	![](apply.png)
3. 将下载的最新版 `Native SDK` 复制到`封装层`中，构成完整的 Unity SDK。  
	- Android 平台  
	将最新的 .jar 文件复制到 `Assets/Plugins/Android` 目录下
	- iOS 平台  
	将最新的 .a 文件复制到 `Assets/Plugins/iOS` 目录下
4. 按 `Native SDK` 功能选项对`封装层`代码进行必要的删减，详见“注意事项”第2条；
5. 将 Unity SDK 集成您需要统计的工程中，并按 [集成文档](http://doc.talkingdata.com/posts/65) 进行必要配置和功能调用。

### 注意事项
1. 分别选择 Android 和 iOS 平台进行功能定制时，请确保两个平台功能项一致。
2. 如果申请 Native SDK 时只选择了部分功能，则需要在本项目中删除未选择功能对应的封装层代码。  
	a) 未选择`自定义事件`功能则删除以下3部分  
	删除 `Assets/TalkingDataScripts/TalkingDataGA.cs` 文件中如下代码：
	
	```
		[DllImport ("__Internal")]
		private static extern void tdgaOnEvent(string eventId, string []keys, string []stringValues, double []numberValues, int count);
	```
	```
		public static void OnEvent(string actionId, Dictionary<string, object> parameters) {
			...
		}
	```
	删除 `Assets/Plugins/iOS/TalkingDataGA.mm` 文件中如下代码：
	
	```
    	void tdgaOnEvent(const char *eventId, const char *keys[], const char *stringValues[], double numberValues[], int count) {
			...
		}
	```
	删除 `Assets/Plugins/iOS/TalkingDataGA.h` 文件中如下代码：
	
	```
	+ (void)onEvent:(NSString *)eventId eventData:(NSDictionary *)eventData;
	```
	b) 未选择`推送营销`功能则删除以下３部分  
	删除 `Assets/TalkingDataScripts/TalkingDataGA.cs` 文件中如下代码：
	
	```
		[DllImport ("__Internal")]
		private static extern void tdgaSetDeviceToken(string deviceToken);
		
		[DllImport ("__Internal")]
		private static extern void tdgaHandlePushMessage(string message);
	```
	```
	#if UNITY_IPHONE
	#if UNITY_5
		public static void SetDeviceToken() {
			...
		}
		
		public static void HandlePushMessage() {
			...
		}
	#else
		public static void SetDeviceToken() {
			...
		}
		
		public static void HandlePushMessage() {
			...
		}
	#endif
	#endif
	```
	删除 `Assets/Plugins/iOS/TalkingData.mm` 文件中如下代码：

	```
		void tdgaSetDeviceToken(const char *deviceToken) {
			...
		}
		
		void tdgaHandlePushMessage(const char *message) {
			...
		}
	```
	删除 `Assets/Plugins/iOS/TalkingData.h` 文件中如下代码：

	```
	+ (void)setDeviceToken:(NSData *)deviceToken;
	+ (BOOL)handleTDGAPushMessage:(NSDictionary *)message;
	```
	