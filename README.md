# app-migrator
util help to convert app project into other types

### 小程序助手使用方式

* 下载 [Visual Studio Code](https://code.visualstudio.com/)，注意： Visual Studio Code 不等于 Visual Studio
* 搜索插件 “小程序助手" 并安装
* 在 Visual Studio Code 中打开微信源代码工程，遵循自动打开的文档"小程序助手使用说明"操作

汇总开发者的反馈，归纳了如下差异点，建议您在开始转换之前先看一下，方便遇到问题后回来对照分析和解决。

### API 差异点

|   功能分类    |                   支付宝                    |                  微信                   |                    入参                    |                    出参                    |                   备注                    |
| :-------: | :--------------------------------------: | :-----------------------------------: | :--------------------------------------: | :--------------------------------------: | :-------------------------------------: |
|   发起请求    |           my.httpRequest            |              wx.request               |              多timeout参数（选填）              |           微信有一个Task概念，目前支付宝不支持           |                                         |
|   上传文件    |            my.uploadFile            |             wx.uploadFile             |             多fileType参数（必填）              |           微信有一个Task概念，目前支付宝不支持           |                                         |
|   下载文件    |           my.downloadFile           |            wx.downloadFile            |                                          |           微信有一个Task概念，目前支付宝不支持           |                                         |
| WebSocket |          my.connectSocket           |           wx.connectSocket            |     支付宝少Protocols（选填/子协议数组）：支付宝目前不支持     |                                          |                                         |
| WebSocket |           my.closeSocket            |            wx.closeSocket             | 支付宝少 code（选填/一个数字值表示关闭连接的状态号，表示连接被关闭的原因） |                                          |                                         |
|    图片     |           my.chooseImage            |            wx.chooseImage             |    支付宝少 sizeType（选填/可选图片的类型，缩略图，正常图片）    |              支付宝少tempFiles               |                                         |
|    图片     |                    缺少                    |            wx.getImageInfo            |                                          |                                          |                                         |
|    图片     |            my.saveImage             |       wx.saveImageToPhotosAlbum       |         支付宝多 showActionSheet(选填)         |                                          |                                         |
|   语音录制    |           my.cancelRecord           |                  缺少                   |                                          |                                          |                                         |
|   语音播放    |           my.resumeVoice            |                  缺少                   |                                          |                                          |                                         |
|   音乐播放    |  my.getBackgroundAudioPlayerState   |   wx.getBackgroundAudioPlayerState    |    支付宝少 dataUrl（歌曲数据链接，只有在当前有音乐播放时返回）    |                                          |                                         |
| 背景音乐播放管理  |                    缺少                    |     wx.getBackgroundAudioManager      |                                          |                                          |             获取全局唯一的背景音乐管理器              |
|  视频组件控制   |        my.createVideoContext        |         wx.createVideoContext         | context方法缺少：seek  跳转到指定位置，单位s；sendDanmu 发送弹幕；playbackRate  设置倍速播放，支持的倍率有 0.5/0.8/1.0/1.25/1.5 1.4.0；requestFullScreen 进入全屏；exitFullScreen 退出全屏； |                                          |                                         |
|           |                    缺少                    |       wx.saveVideoToPhotosAlbum       |                                          |                                          |                                         |
| 已保存的文件列表  |                    缺少                    |          wx.getSavedFileList          |                                          |                                          |                                         |
| 本地保存文件信息  |                    缺少                    |          wx.getSavedFileInfo          |                                          |                                          |                                         |
|  删除保存文件   |                    缺少                    |          wx.removeSavedFile           |                                          |                                          |                                         |
| 新开页面打开文档  |                    缺少                    |            wx.openDocument            |                                          |                                          | 新页面打开文档支持doc，ppt，xls，pdf，docx，pptx，xlsx |
|    位置     |           my.getLocation            |            wx.getLocation             | 支付宝多 cacheTimeout，timeout（选填，本期改参数去除，native实现问题）少 type（选填\返回坐标类型） | 支付宝少altitude（高度）少verticalAccuracy（垂直精度）少horizontalAccuracy（水平精度） |                                         |
|    位置     |                    缺少                    |           wx.chooseLocation           |                                          |                                          |               打开地图选择地理位置                |
|   系统信息    |          my.getSystemInfo           |           wx.getSystemInfo            |                                          | 支付宝少screenWidth，少screenHeight，少system，少platform，少SDKVersion |                                         |
|   系统信息    |        my.getSystemInfoSync         |         wx.getSystemInfoSync          |                                          | 支付宝少screenWidth，少screenHeight，少system，少platform，少SDKVersion |                                         |
|   网络状态    |          my.getNetworkType          |           wx.getNetworkType           |                                          |           支付宝多networkAvailable           |                                         |
|   网络状态    |                    缺少                    |       wx.onNetworkStatusChange        |                                          |                                          |                                         |
|   屏幕亮度    |                    缺少                    |        wx.setScreenBrightness         |                                          |                                          |                                         |
|   屏幕亮度    |                    缺少                    |        wx.getScreenBrightness         |                                          |                                          |                                         |
|   屏幕亮度    |                    缺少                    |          wx.setKeepScreenOn           |                                          |                                          |                                         |
|    震动     |             my.vibrate              |            wx.vibrateLong             |                                          |                                          |             支付宝只支持一种默认的震动时间             |
|    震动     |             my.vibrate              |            wx.vibrateShort            |                                          |                                          |             支付宝只支持一种默认的震动时间             |
|   加速度计    |                  暂时不支持                   |       wx.onAccelerometerChange        |                                          |                                          |                                         |
|   加速度计    |                    缺少                    |         wx.startAccelerometer         |                                          |                                          |                                         |
|   加速度计    |                    缺少                    |         wx.stopAccelerometer          |                                          |                                          |                                         |
|    罗盘     |                  暂时不支持                   |          wx.onCompassChange           |                                          |                                          |                                         |
|    罗盘     |                    缺少                    |            wx.startCompass            |                                          |                                          |                                         |
|    罗盘     |                    缺少                    |            wx.stopCompass             |                                          |                                          |                                         |
|    扫码     |               my.scan               |              wx.scanCode              | 支付宝少onlyFromCamera（选填/是否只从相机扫码）：我们目前不支持。 |          支付宝少scanType（扫码类型）：不支持          |                                         |
|    摇一摇    |            my.watchShake            |                  缺少                   |                                          |                                          |                                         |
| 获取当前服务器时间 |          my.getServerTime           |                  缺少                   |                                          |                                          |                                         |
|   用户截屏    |                    缺少                    |        wx.onUserCaptureScreen         |                                          |                                          |                用户截屏事件监听                 |
|   添加联系人   |                    缺少                    |          wx.addPhoneContact           |                                          |                                          |                 添加系统联系人                 |
|    蓝牙     |  my.startBluetoothDevicesDiscovery  |   wx.startBluetoothDevicesDiscovery   | 支付宝少allowDuplicatesKey（选填/是否允许重复上报同一设备， 如果允许重复上报，则onDeviceFound 方法会多次上报同一设备，但是 RSSI 值会有不同）少interval（选填/上报设备的间隔，默认为0，意思是找到新设备立即上报，否则根据传入的间隔上报) |    少isDiscovering，多services（蓝牙服务对象列表）    |                                         |
|    蓝牙     |       my.getBluetoothDevices        |        wx.getBluetoothDevices         |             支付宝多services（选填）             |      支付宝 device信息多localName（广播设备名）       |                                         |
|    蓝牙     |   my.getConnectedBluetoothDevices   |    wx.getConnectedBluetoothDevices    |                                          | 支付宝多localName（广播设备名）多RSSI（信号强度）多advertiseData（广播内容） |                                         |
|    蓝牙     |   my.writeBLECharacteristicValue    |    wx.writeBLECharacteristicValue     | 支付宝多descriptorId，value参数类型不同，支付宝为HexString，微信是ArrayBuffer |                                          |                                         |
|    蓝牙     |    my.readBLECharacteristicValue    |     wx.readBLECharacteristicValue     |                                          | 支付宝characteristic少value值（蓝牙设备特征值对应的值，16进制字符串） |                                         |
|    蓝牙     | my.notifyBLECharacteristicValueChange | wx.notifyBLECharacteristicValueChange | 支付宝少state（必填/boolean：启用，停用）；多descriptorId（选填） |                                          |                                         |
|    蓝牙     |   my.getBLEDeviceCharacteristics    |    wx.getBLEDeviceCharacteristics     | 多descriptorId（选填）；多value（必填/蓝牙设备特征值对应的值，16进制字符串） |                                          |                                         |
|    蓝牙     |      my.onBluetoothDeviceFound      |       wx.onBluetoothDeviceFound       |                                          |          ios没有manufacturerData值          |                                         |
|    蓝牙     |  my.onBLECharacteristicValueChange  |   wx.onBLECharacteristicValueChange   |                                          | value参数类型不同，我们为HexString，微信是ArrayBuffer  |                                         |
|    导航     |                    缺少                    |              wx.reLaunch              |                                          |                                          |           关闭所有页面，打开到应用内的某个页面            |
|   交互反馈    |            my.showToast             |             wx.showToast              | 少icon（选填\可选图标），少image(选填\自定义图标)，少mask(选填\是否启用蒙层) |                                          |                                         |
|   交互反馈    |           my.showLoading            |            wx.showLoading             |       少mask(选填\是否启用蒙层)，多delay(选填)        |                                          |                                         |
|    联系人    |       my.chooseAlipayContact        |                  缺少                   |                                          |                                          |                                         |
|   城市选择    |            my.chooseCity            |                  缺少                   |                                          |                                          |                                         |
|   日期选择    |            my.datePicker            |                  缺少                   |                                          |                                          |                                         |
|    绘画     |       my.createCanvasContext        |        wx.createCanvasContext         | 多 context.clip方法；少context.setTextBaseline、context.setTextAlign，保存画布为文件的实现方式方式不同 |                                          |                                         |
|    地图     |         my.createMapContext         |          wx.createMapContext          |                                          | mapContext对象少context.translateMarker（平移Marker，带动画）、context.includePoints（缩放视野展示所有经纬度）、context.getRegion（获取当前地图的视野范围）、context.getScale（获取当前地图的缩放级别） |                                         |
|    键盘     |           my.hideKeyboard           |                  缺少                   |                                          |                                          |                                         |
|   自定义分析   |         my.reportAnalytics          |          wx.reportAnalytics           |                                          |                                          |                                         |
|    安全     |               my.rsa                |                  缺少                   |                                          |                                          |                                         |
|    分享     |                    缺少                    |        Page：onShareAppMessage         |                                          |                                          |                                         |
|           |                                          |                                       |                                          |                                          |                                         |

### 组件差异点

* 列表渲染 a:key 里不能有变量

即 a:key="{{variable}}" 这种形式在支付宝小程序里是不能支持的
正确的形式为：a:key="constant"

* js 模块路径需要是相对路径，资源路径需要是绝对路径

例如：

```javascript
// 意图：引用图片资源，不 ok 的做法
iconPath: './a/b/c.png'
// 意图：引用图片资源，ok 的做法
iconPath: '/a/b/c.png'

// 意图：引入当前目录下的 abc.js 模块，不 ok 的做法
var externalJs  = require('abc'); // 这个 abc 会被当成依赖库，在 node_modules 里
// 意图：引入当前目录下的 abc.js 模块 ok 的做法
var externalJs  = require('./abc'); // 这个 abc 则为当前模块目录下的 abc.js
```

* 以下组件不能通过指定 class 属性的方式引入外部样式

swiper、swiper-item，icon，progress，switch、slider、radio、radio-group、picker-view、checkbox、checkbox-group、button、picker、navigator、audio

* input 组件不支持 placeholder-style 和 placeholder-class

# 反馈
如果您在使用中遇到任何问题，请在本项目的 issues 上创建 issue，我会及时查看并回复。
