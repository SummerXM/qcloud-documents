## 直播流程

iLiveSDK中的音视频通讯能力被抽象为房间这个概念。在同一个房间内的成员可以看到房间内其他成员的音视频，每个房间同时最多可以有四路视频。iLiveSDK(Windows)提供了房间管理，音视频设备管理等功能，方便用户建立互动视频。具体的直播业务流程如下：


![基本直播流程](http://mc.qcloudimg.com/static/img/06d2fb5027be53492249d4b81bd2f5a5/image.png)

## 设置回调函数
iLiveSDK通过回调将一些状态和通知告知上层，用户可以按照业务需要设置回调函数。

```c++
void onForceOffline()
{
//账号已经在其他地方登陆
}
void OnMessage( const Message& msg )
{
}
void OnLocalVideo( LiveVideoFrame* video_frame, void* custom_data )
{
	//video_frame是本地画面每一帧的数据,用户需要显示本地画面时，在此回调函数中做渲染，渲染代码可参考随心播;
}
void OnRemoteVideo( LiveVideoFrame* video_frame, void* custom_data )
{
	//video_frame是远程画面每一帧的数据,用户需要显示远程画面时，在此回调函数中做渲染，渲染代码可参考随心播;
}
GetILive()->setForceOfflineCallback(onForceOffline); //设置被挤下线的通知函数;
GetILive()->setMessageCallBack(OnMessage); //设置消息的处理函数;
GetILive()->setLocalVideoCallBack(OnLocalVideo, NULL); //设置本地视频的回调函数;
GetILive()->setRemoteVideoCallBack(OnRemoteVideo, NULL); //设置远程视频的回调函数;
```

## 初始化
使用其他各项功能前必须将iLiveSDK初始化。

|接口名|接口描述|
|---|---|
|init|iLiveSDK内部初始化，告知appId.|

|参数类型|参数名|说明|
|---|---|---|
|int|appId|传入业务方appid|
|int|accountType|传入业务方 accountType|
|bool|IMSupport|是否需要im功能|

示例：
```c++
int nRet = GetILive()->init(appid, AccountType);
if (nRet != NO_ERR)
{
	//初始化失败
}
```

## 登录
用户在登录后才能使用消息通讯，互动视频等功能。登录需要填写用户id和签名。其中签名是由[腾讯登录服务](https://cloud.tencent.com/document/product/269/1507)提供的。

|接口名|接口描述|
|---|---|
|login|独立模式登录到腾讯云后台|

|参数类型|参数名|说明|
|---|---|---|
|const char * |userId|用户在独立模式下注册的帐号|
|const char * |userSig|用户在业务方后台获取到的签名|
|iLiveSuccCallback|suc|登录成功回调|
|iLiveErrCallback |err|登录失败回调|
|  void * |data |用户自定义的数据的指针，在成功和失败的回调函数中原封不动地返回 |

* 示例：
```c++
void OniLiveLoginSuccess( void* data )
{
	//登录成功
}
void OniLiveLoginError( int code, const char *desc, void* data )
{
	//登录失败
}
GetILive()->login(userId, userSig, OniLiveLoginSuccess, OniLiveLoginError, NULL);
```

## 音视频权限管理

业务层需重点关注房间成员的音视频权限。只有主播或者需要上麦的观众才能拥有音视频上行的权限。在进入或者创建房间时需要填写正确的权限（详见下文进入房间）。另外成员允许在房间内改变自己的权限。
您可以在[腾讯云控制台配置](https://github.com/zhaoyang21cn/suixinbo_doc/blob/master/SPEARConfig.md)自身业务需要的角色及其权限，腾讯云服务器会根据房间成员不同的权限分配不同的[接入机](https://cloud.tencent.com/document/product/268/7651)。错误配置权限可能导致不必要的带宽支出和观众异常上行数据等问题。

## 创建房间

|接口名|接口描述|
|---|---|
|createRoom |主播创建直播间|

|参数类型|参数名|说明|
|---|---|---|
| const iLiveRoomOption &|roomOption|主播创建房间时的配置项|
| iLiveSuccCallback|suc|创建房间成功回调|
| iLiveErrCallback |err|创建房间失败回调|
| void * |data |用户自定义的数据的指针，在成功和失败的回调函数中原封不动地返回|

* 示例：

```c++
void OniLiveCreateRoomSuc( void* data )
{
	//创建房间成功
}
void OniLiveCreateRoomErr( int code, const char *desc, void* data )
{
	//创建房间失败
}

iLiveRoomOption roomOption;
roomOption.audioCategory = AUDIO_CATEGORY_MEDIA_PLAY_AND_RECORD;//互动直播场景
roomOption.roomId = 123456;
roomOption.controlRole = "LiveMaster";
roomOption.authBits = AUTH_BITS_DEFAULT;//注意权限管理
roomOption.roomDisconnectListener = OnRoomDisconnect;//自定义的房间断连的回调
roomOption.memberStatusListener = OnMemStatusChange;//自定义的成员状态变化回调
roomOption.data = this;
GetILive()->createRoom( roomOption, OniLiveCreateRoomSuc, OniLiveCreateRoomErr, this );
```


# 5 加入房间(观众)

|接口名|接口描述|
|---|---|
|joinRoom |观众进入直播间|

|参数类型|参数名|说明|
|---|---|---|
|iLiveRoomOption|roomOption|观众进入房间时的配置项|
| SuccessCalllback|suc|加入房间成功回调|
| ErrorCallback |err|加入房间失败回调|
| void * |data |用户自定义的数据的指针，在成功和失败的回调函数中原封不动地返回|

* 示例：

```c++
void OniLiveJoinRoomSuc( void* data )
{
	//加入房间成功
}
void OniLiveJoinRoomErr( int code, const std::string& desc, void* data )
{
	//加入房间失败
}

iLiveRoomOption roomOption;
roomOption.audioCategory = AUDIO_CATEGORY_MEDIA_PLAY_AND_RECORD;//互动直播场景
roomOption.roomId = 123456;
roomOption.controlRole = "Guest";
roomOption.authBits = AUTH_BITS_JOIN_ROOM|AUTH_BITS_RECV_AUDIO|AUTH_BITS_RECV_CAMERA_VIDEO|AUTH_BITS_RECV_SCREEN_VIDEO;//注意权限管理
roomOption.memberStatusListener = Live::OnMemStatusChange;//自定义的成员状态变化回调
roomOption.roomDisconnectListener = Live::OnRoomDisconnect;//自定义的房间断连的回调
roomOption.data = g_pMainWindow->getLiveView();
GetILive()->joinRoom( roomOption, OniLiveJoinRoomSuc, OniLiveJoinRoomErr, this );
```

