# v3.31.0
## 功能更新
1. 新增：字幕背景增加圆角能力
2. 新增：编辑模式增加镜像能力
3. 优化: License 校验逻辑
4. 优化: 开启日志落地能力，接入时排查问题更精准
5. 修复：已知问题

# v3.30.0
## 功能更新
1. 优化: License 加载逻辑及权限管理
2. 修复: 其他已知问题

# v3.29.0
## 功能更新
1. 优化:裁剪性能优化
2. 优化:License 升级，初始化支持回调 License 授权信息

# v3.28.0
## 功能更新
1. 优化：提高编辑导入效率
2. 修复：已知问题

# v3.27.0
## 功能更新
1. 新增：剪同款功能
2. 新增：多源录制支持回声消除、降噪、背景音乐与录音混音
3. 新增：lut滤镜功能
4. 优化：接口调整及优化，提升接入体验,调整统一SDK的单位，时间：毫秒，角度：弧度
5. 修复：HDR视频无法合成的问题
6. 修复：其他已知bug

## 接口调整

### 录制
1. 新增`AliyunIRecorder##getPasterManager();`接口，统一进行贴图相关管理（动图、水印等）
2. 废弃AliyunIRecorder设置视频质量接口（setVideoQuality/setGop/setVideoBitrate），统一到MediaInfo设置
3. 废弃动图（EffectPaster）相关接口，`AliyunIRecorder#addPaster`, `AliyunIRecorder#removePaster`，挪到`AliyunIRecorder#getPasterManager();`统一管理
4. 废弃水印（EffectImage）相关接口，`AliyunIRecorder#addImage`, 挪到`AliyunIRecorder#getPasterManager();`统一管理
5. 废弃背景音乐接口，`AliyunIRecorder#setMusic`, 使用`AliyunIRecorder#applyBackgroundMusic;`代替
6. 废弃美颜开关接口，`AliyunIRecorder#setBeautyStatus`, 使用原有接口`AliyunIRecorder#setBeautyLevel;`代替
7. 废弃拍照及截图接口，`AliyunIRecorder#takePicture`及`AliyunIRecorder#takePhoto`， 使用`AliyunIRecorder#takePicture(boolean needBitmap, OnPictureCallback pictureCallback);`及`AliyunIRecorder#takeSnapshot(boolean needBitmap, OnPictureCallback pictureCallback);`代替
8. 移除AliyunIRecorder的MV接口(applyMv/paurseMv/resumeMv)
9. 移除AliyunIRecorder的postToGl/removeFromGl/setProperty接口
10. 方法调整：`AliyunIRecorder#setRecordCallback(RecordCallback callback)` => `AliyunIRecorder#setOnRecordCallback(OnRecordCallback callback)`
11. 方法调整：`AliyunIRecorder#setEncoderInfoCallback(EncoderInfoCallback` callback) => `AliyunIRecorder#setOnEncoderInfoCallback(OnEncoderInfoCallback callback)`
12. 方法调整:`AliyunIAudioRecorder.setRecordCallback(AudioRecordCallback callback);` => `AliyunIAudioRecorder.setOnAudioRecordCallback(OnAudioRecordCallback callback);`
13. 命名调整: `OnFrameCallBack` => `OnFrameCallback`
14. 命名调整: `OnChoosePictureSizeCallBack` => `OnChoosePictureSizeCallback`
15. 命名调整: `OnTextureIdCallBack` => `OnTextureIdCallback`


### 合拍
1. `AliyunMixStream`新增通用时间单位接口
2. 废弃AliyunIMixRecorder设置视频质量接口（setVideoQuality/setGop/setVideoBitrate），统一到MediaInfo设置
3. 废弃动图（EffectPaster）相关接口，`AliyunIMixRecorder#addPaster`, `AliyunIRecorder#removePaster`，挪到`AliyunIMixRecorder#getPasterManager();`统一管理
4. 废弃水印（EffectImage）相关接口，`AliyunIMixRecorder#addImage`, 挪到`AliyunIMixRecorder#getPasterManager();`统一管理
5. 废弃美颜开关接口，`AliyunIMixRecorder#setBeautyStatus`, 使用原有接口`AliyunIMixRecorder#setBeautyLevel;`代替
6. 移除AliyunIMixRecorder的postToGl/removeFromGl/setProperty接口
7. 方法调整：`AliyunIMixRecorder#setRecordCallback(RecordCallback callback)` => `AliyunIMixRecorder#setOnRecordCallback(OnRecordCallback callback)`
8. 方法调整：`AliyunIMixRecorder#setEncoderInfoCallback(EncoderInfoCallback` callback) => `AliyunIMixRecorder#setOnEncoderInfoCallback(OnEncoderInfoCallback callback)`


### 裁剪
1.`AliyunICrop` - 新增startCropAudio通用时间单位接口
2.`CropParam` - 新增通用时间单位接口

### 编辑
1. 新增`AliyunIClipManager`，替代原有的`AliyunIClipConstructor`
2. `AliyunIEditor`使用通用的时间单位(TimeUnit)接口，并新增以毫秒为单位的接口
3. 新增`AliyunIEditor#getPlayerController()`统一的播放控制类
4. 新增seek和draw通用时间单位接口`AliyunIEditor#seek(long time, TimeUnit timeUnit)`、`AliyunIEditor#draw(long time, TimeUnit timeUnit)`
5. 新增水印接口`AliyunIEditor#applyWaterMark(TrackEffectWaterMark trackEffectWaterMark)`
6. 新增片尾水印接口`AliyunIEditor#addTailWaterMark(TrackEffectWaterMark trackEffectWaterMark, long duration, TimeUnit timeUnit)`
7. 废弃`AliyunIEditor.createPasterManager()`,使用`AliyunIEditor.getPasterManager()`代替
8. 废弃EffectBean的滤镜接口`AliyunIEditor.applyFilter(EffectBean effect)`,使用`AliyunIEditor.applyFilter(TrackEffectFilter effectFilter)`代替
9. 废弃EffectBean的MV接口`AliyunIEditor.applyMV(EffectBean effect)`,使用`AliyunIEditor.applyMV(TrackEffectMV trackEffectMv)`代替
10. 废弃EffectPicture及静态图接口`AliyunIEditor.addImage`,使用`AliyunIEditor#getPasterManager()`的addImage代替
11. 废弃EffectFilter的动效滤镜接口`AliyunIEditor.addAnimationFilter(EffectFilter filter)`,使用`AliyunIEditor#addAnimationFilter(TrackEffectFilter filter)`的代替
12. 废弃动效滤镜监听接口`AliyunIEditor.setAnimationRestoredListener(OnAnimationFilterRestored li)`,使用`AliyunIEditor.setAnimationRestoredListener(OnAnimationFilterRestored li);`代替
13. 废弃EffectBean的音乐接口`AliyunIEditor.applyMusic(EffectBean effect)`,使用`AliyunIEditor.applyMusic(TrackAudioStream effectStream)`代替
14. 废弃EffectBean的配音接口`AliyunIEditor.applyDub(EffectBean effect)`,使用`AliyunIEditor.applyDub(TrackAudioStream effectStream)`代替
15. 废弃音效接口`AliyunIEditor.audioEffect(int id, AudioEffectType type, int weight)`,使用`AliyunIEditor.applyAudioEffect(int id, AudioEffectType type, int weight)`代替
16. 废弃`AliyunIEditor#getPasterRender`接口
17. `AliyunIPasterController`新增毫秒时间接口及通用时间设置接口
18. `AliyunPasterManager`新增通用时间设置接口

# v3.26.0
## 功能更新
1. 优化：短视频的稳定性问题
2. 修复：部分音频格式不支持问题

# v3.25.0
## 功能更新
1. 新增：画中画功能，可以在编辑界面添加画中画
2. 新增：快速获取视频缩略图模式
3. 新增：字幕动画，可以对字幕（花字）等做动画
4. 优化：包Size优化，集成后包体减少3+M
5. 优化：草稿箱新增自定义封面图
6. 修复：其它一些已知bug

# v3.24.0
## 功能更新
1. 优化：视频编码方式不再支持FFmpeg软编码
2. 升级：FFmpeg版本升级4.3.0
3. 修复：字幕在32位系统不生效的问题
4. 修复：修复音频编码为HE-AACV2的视频裁剪后在chrome播放器上无法播放的问题

# v3.23.0
## 功能更新
1. 新增：草稿箱能力，支持导出草稿
2. 新增：字幕新增背景色，对齐等能力
3. 新增：合拍新增回声消除能力
4. 新增：DEMO美颜模块替换为Queen Sdk
5. 新增：6个分屏滤镜特效
6. 优化：多源录制支持SurfaceView录屏
7. 优化：优化合拍性能，提升合成速度
8. 优化：录制支持自动删除临时视频文件
9. 优化：SDK API带注释，接入更高效
10. 修复：部分设备使用长视频合成至99%会失败的问题
11. 修复：华为设备的拍摄黑屏等问题

# v3.22.0
## 功能更新
1. 新增：编辑增加花字功能
2. 新增：新增局部屏幕采集
3. 新增：新增边录屏边摄像头采集功能
4. 新增：自定义特效Shader新增时间内建变量BUILTIN_PROGRESS
5. 修复：合拍时合拍视频高度可能会少两个像素的问题
6. 修复：修复部分场景下的稳定性问题
## 接口变动
废弃：com.aliyun.svideosdk.editor.AliyunPasterManager#addSubtitle
废弃：com.aliyun.svideosdk.editor.AliyunPasterManager#addSubtitleWithStartTime
新增：com.aliyun.svideosdk.editor.AliyunPasterManager#addCaptionWithStartTime

# v3.21.0
## 功能更新
1. 新增：合拍摄像头视频展示支持圆角边框
2. 新增：支持HEIF图片的导入工具
3. 修复部分机型软编过程中内存堆积并导致崩溃
4. 修复自定义渲染回调相机矩阵没有及时更新的问题
5. 修复SDK稳定性问题

# v3.20.0
## 功能更新
1. 新增：编辑模块音频增加淡入淡出效果
2. 新增：编辑模块增加组合字幕功能
3. 新增：编辑模块增加基础编辑能力
4. 修复部分机型多段视频素材编辑时，素材预览切换时出现花屏的情况
5. 修复编辑场景视频导出帧率设置不生效的问题
6. 修复android平台自定义渲染时相机变化矩阵可能为空的问题
7. 修复SDK稳定性问题

# v3.19.0
## 功能更新
1. 新增：编辑模块音频降噪功能。
2. 新增：合拍和视频拼接功能，支持设置背景图片和背景颜色。
3. 新增：合拍流程支持混音
4. 新增：支持录制预览阶段，回调音频数据。
5. 修复：编辑-字幕功能，放大字体到某个字号，emoji 图案不显示的问题。
6. 修复：设置水印/图片，添加某些透明光晕图片，光晕变色的问题。
7. 修复：添加静态图片旋转角度不对问题。
## 接口变动
1. 废弃接口 com.aliyun.svideosdk.editor.AudioEffectType#EFFECT_TYPE_DENOISE
2. 废弃接口 com.aliyun.svideosdk.editor.AliyunIEditor#denoise(int, boolean)

# v3.18.1
## 问题修复
1. 修复iOS部分机型硬编码内存问题
2. 修复Android合拍非填充模式下花屏问题

# v3.18.0
## 功能更新
1. 增加合拍视频指定使用的音轨功能(视频原音/录制声音/静音)。
2. 修复 android Q(10)切换画幅会闪烁黑边的问题。

## 接口变动
1. AliyunIMixRecorder#setMixAudioSource 增加合拍视频指定使用的音轨功能

# v3.17.1
## 功能更新
1. 修复某些机型合成后opengl导致的闪退问题；
2. 修复自定义字体不生效问题；
3. 修复AlivcSdkCore.setLogPath日志多线程问题。
ø
# v3.17.0
## 功能更新
1. Android SDK 包名重构优化，降低接入理解成本，更加友好，新包名统一以com.aliyun.svideosdk.*命名，详情参考最新接口文档及辅助转换工具(https://alivc-demo-cms.alicdn.com/versionProduct/sourceCode/shortVideo/tool/interface_upgrade.py)
2. 删除确定不被引用的废弃接口，列表如下：
  com.error.NativeErrorCode
  com.qu.preview.callback.OnNativeReady
  com.aliyun.qupai.editor.AliyunIExporter
  com.aliyun.qupai.editor.AliyunIPlayer
  com.aliyun.qupai.editor.OnPlayCallback
  com.aliyun.qupai.editor.OnPreparedListener
  com.aliyun.querrorcode.AliyunVideoCoreError
3. 优化萝莉音效、新增方言音效；
4. 修复拍照在极端场景下的闪退问题。

# v3.16.2
## 功能更新
1. 修复高斯模糊背景问题

# v3.16.1
## 功能更新
1. 修复若干已知问题

# v3.16.0
## 功能更新
1. 恢复主流动画功能
2. 修复线上反馈偶现崩溃问题
3. 修复长视频可能出现的播放卡顿问题
4. 修复部分机型兼容性导致的录制崩溃问题
5. 修复一些已知bug

# v3.15.0
## 功能更新
1. 修复合成视频播放卡顿问题
2. 修复视频多段变速失效问题
3. 修复一些机型前置摄像头曝光区域无效问题
4. 新增基于自定义特效制作规范的两组转场、滤镜效果转场与滤镜效果

## 接口变动
1. 新增自定义特效参数调节接口，支持实时调节特效参数
2. 支持自定义滤镜、转场特效，自定义特效制作规范请参考官方文档

# v3.14.0
## 功能更新
1. 适配Android Q系统，提升Android Q系统录制编辑输出视频性能。
2. 优化录制时实现，解决偶现的卡死问题。
3. 修复已知几处内存泄漏并优化部分性能

# v3.13.0
## 功能更新
1. 录制模块稳定性，性能全面优化
2. 录制模块支持基于RACE的美颜美型功能

## 接口变动
1. 录制模块废弃mv接口，去除添加mv功能

# v3.12.0
## 功能更新
1. 增加日志分析功能
AlivcSdkCore#setDebugLoggerLevel(AlivcDebugLoggerLevel level) 提供三个等级供用户设置
AlivcDLClose        关闭日志分析功能
AlivcDLNormal    能分析warning，error级别的日志，建议使用这个等级来做日志分析
AlivcDLAll             全量log分析，只建议在定位疑难问题时开启，不建议在正式发版中使用。
以上功能只会做sdk的日志分析
2. 编辑模块性能提升

## 接口变动
1. 编辑模块废除addRunningDisplayMode:接口，去除动态切换内容模式功能
2. 编辑模块废除removeRunningDisplayMode:接口，去除删除动态切换内容模式功能

# v3.11.0
##功能更新
1. 提升片段录制起停的速度和录制合成的速度，分段录制更加流畅
2. 优化录制进度回调粒度和精准度
3. 精准控制gop，提升部分场景下的转码速度
4. 修复已知bug，提升稳定性
5. 对外错误码统一

# v3.10.5
## 功能更新
1. 新增合拍功能接口(AliyunIMixRecorder)
2. 新增多轨道视频拼接（可以实现画中画，左右分屏等效果）（AliyunIMixComposer)

# v3.10.0

## 功能更新
1. 编辑新增大魔王，小黄人音效
2. 编辑新增mjpeg视频格式支持
3. 编辑播放提升对部分损坏视频文件的兼容性
4. 编辑/转码新增对hevc视频硬解支持
5. 转码速度提升
6. 录制新增重新设置预览窗口大小接口 AliyunIRecorder.resizePreviewSize
7. 录制修复小段录制视频时长不准确问题
8. 优化一些句柄未释放导致的泄漏隐患问题
9. 新增合成及上传单独接口,可支持单独合成及单独上传

## 接口变动
1. 编辑初始化默认不再绘制首帧，新增draw方法支持强制绘制一帧

## 其他
Maven集成方式
仓库地址maven { url "http://maven.aliyun.com/nexus/content/repositories/releases" }
专业版：com.aliyun.video.android:svideopro:3.10.0 -对应专业版的AliyunSdk-RCE.aar和armeabi-v7a&arm64-v8a的so库
标准版：com.aliyun.video.android:svideostand:3.10.0 -对应标准版的AliyunSdk-RCE.aar和armeabi-v7a&arm64-v8a的so库
基础版：com.aliyun.video.android:svideosnap:3.10.0 -对应基础版版的AliyunSdk-RC.aar和armeabi-v7a&arm64-v8a的so库
核心库：
com.aliyun.video.android:core:1.1.2 （对应AlivcCore.jar）
com.alivc.conan:AlivcConan:0.9.4
com.aliyun.video.android:AlivcSvideoFFmpeg:1.0.0


# v3.9.0

## 功能更新
1. 新增音效接口，提供萝莉，大叔，混响，回声四种音效
2. 提升编辑模块seek性能
3. 修复已知bug，提升稳定性
4. libAliFaceAREngine.so 与 libFaceAREngine.so合并为一个.so，只保留了libAliFaceAREngine.so

## 接口变动
1. OnFrameCallBack接口回调触发线程变更为非主线程


# v3.8.0

## 功能更新
1. 优化了编辑播放能力，流畅播放不卡顿；
2. 优化了编辑合成的速度；
3. 优化了视频录制预览清晰度
4. 提升低端机器上的录制帧率
5. 修复了已知bug，提升稳定性
6. 短视频SDK全面支持maven依赖
* 仓库地址：maven { url "http://maven.aliyun.com/nexus/content/repositories/releases" }
7. 用户可自行替换对应的sdk
* 专业版：
        com.aliyun.video.android:svideopro:3.8.0 -对应专业版的AliyunSdk-RCE.aar
        com.aliyun.video.android:svideopro-armv7a:3.8.0 -对应armeabi-v7a架构的短视频专业版所有so库
        com.aliyun.video.android:svideopro-arm64:3.8.0 -对应arm64-v8a架构的短视频专业版所有so库
* 标准版：
        com.aliyun.video.android:svideostand:3.8.0 -对应标准版的AliyunSdk-RCE.aar
        com.aliyun.video.android:svideostand-armv7a:3.8.0 -对应armeabi-v7a架构的短视频标准版所有so库
        com.aliyun.video.android:svideostand-arm64:3.8.0 -对应arm64-v8a架构的短视频标准版所有so库
* 基础版：
        com.aliyun.video.android:svideosnap:3.8.0 -对应基础版版的AliyunSdk-RC.aar
        com.aliyun.video.android:svideosnap-armv7a:3.8.0 -对应armeabi-v7a架构的短视频基础版所有so库
        com.aliyun.video.android:svideosnap-arm64:3.8.0 -对应arm64-v8a架构的短视频基础版所有so库
* 视频云核心库：
        compile 'com.aliyun.video.android:core:1.1.0' -对应AlivcCore.jar

8.短视频SDK内部不再包含上传SDK，开发者需要另外通过gradle添加外部依赖:
    compile 'com.aliyun.video.android:upload:1.5.2'
9.考虑到SDK稳定性监控和未来数据相关需求，短视频目前必须要依赖库:
    compile 'com.alivc.conan:AlivcConan:0.9.0'
    以及添加混淆，参考demo

## 接口变动
1. RecordCallback部分回调所在的线程有变动:
    RecordCallback#onComplete 由主线程回调变为子线程回调，如果有UI操作，需要开发者将相关操作post主线程；
    RecordCallback#onProgress 由主线程回调变为子线程回调，如果有UI操作，需要开发者将相关操作post主线程；
    RecordCallback#onMaxDuration 由主线程回调变为子线程回调，如果有UI操作，需要开发者将相关操作post主线程；
    RecordCallback#onError 由主线程回调变为子线程回调，如果有UI操作，需要开发者将相关操作post主线程；
该改动主要是为了保证回调数据与SDK内部状态的一致性，减少异常问题。

2. EditorCallback 回调变动
EditorCallback由原先的Interface改为abstract class。
添加了 mNeedRenderCallback 属性，该属性可以控制是否需要onCustomRender 和onTextureRender两个回调，
在关掉这两个回调的情况下，编辑模块的性能会有一定提升。
目前默认为关闭状态。如果需要开启，请设置这个参数为:
mNeedRenderCallback = EditorCallBack.RENDER_CALLBACK_CUSTOM(开启onCustomRender);
mNeedRenderCallback = EditorCallBack.RENDER_CALLBACK_TEXTURE（开启onTextureRender);
mNeedRenderCallback = EditorCallBack.RENDER_CALLBACK_TEXTURE|EditorCallBack.RENDER_CALLBACK_CUSTOM（同时开启两个回调）


# v3.7.8.1

## 接口变动
1. AliyunIRecorder新增postToGl和removeFromGl两个接口，用于向gl线程post和remove操作，一些需要依赖gl资源或者释放gl资源的操作可以通过这两个接口进行。



# v3.7.8

## 功能更新
1. 优化预览和录制的帧率，帧率有大幅提升。

## 接口变动
1. AliyunIRecorder.setDisplayView(GLSurfaceView surfaceView) =====>> AliyunIRecorder.setDisplayView(SurfaceView surfaceView)
GLSurfaceView 改成了 SurfaceView
2. 自定义渲染（第三方渲染）销毁gl资源，以前GLSurfaceView时可以通过GLSurfaceView.queueEvent来做，现在增加了一个gl资源销毁的回调OnTextureIdCallBack.onTextureDestroyed()，需要统一在这里面做。
3. 支持随意切换surface窗口大小，无需重启preview（如果要考虑重新选择采集分辨率则依然需要重启）
4. RecordCallback.onInitReady只会在AliyunIRecorder创建时回调一次（setRecordCallback），实际上只是为了保留老版本的兼容性，现在的版本AliyunIRecorder创建完即可进行相关操作，不需要等待onInitReady也可以。


# v3.7.7

## 功能更新
1. 增加AlivcSdkCore类，主要用于debug调试，AlivcSdkCore#register函数用于在debug模式下替换动态库，AlivcSdkCore#setLogLevel用于定制log等级
## 其他
2. 可以通过访问链接 https://h5.m.taobao.com/alicare/meebot.html?appKey=zjrE3jzzba&type=dingding_channel 进入机器人答疑，可以输入关键字信息获取答案，请尽量输入准确的信息 如 接口文档，如何添加普通动图等
3. 提高合成，裁剪的清晰度
4. 整体稳定性提升


# v3.7.5

## 功能更新
1. 修复编辑使用第三方渲染接口可能导致crash的bug
2. 时间特效播放流畅度提升
3. gif适配性扩展
4. 奇数分辨率导入视频支持
5. 优化多段录制音视频同步问题
6. 提升稳定性


# v3.7.0

## 功能更新
1. 编辑预览播放增加replay接口，如果要重播，则需要在收到onEnd回调后调用replay，具体参考Demo代码
2. 修改静音接口(AliyunIEditor#setAudioSilence)的实现，现在静音接口只能够在预览播放时静音，如果要实现合成的视频静音，则需要使用AliyunIEditor#setVolume(0)，将输出音量设置为0
3. 编辑的AliyunPasterBaseView接口新增部分属性接口，主要为以下属性：
*   getTextMaxLines——获取最大行数
    getTextAlign()——获取文字对齐方式
    getTextPaddingX()——获取文字X轴距离左边的边距，以左上角为原点
    getTextPaddingY()——获取文字Y轴距离上边的边距，以左上角为原点
    getTextFixSize()——获取文字字号
    getBackgroundBitmap()——获取文字背景图
    isTextHasLabel()——是否有背景色
    getTextBgLabelColor()——获取文字背景色
以上接口需要开发者实现。
4. AliyunIEditor#applySourceChange更新视频源后，不会自动播放，需要开发者控制播放，也就是说如果要继续播放则需要调用AliyunIEditor#play接口。
5. 缩略图/取帧（AliyunIThumbnailFetcher）相关接口包名更换，可先预编译一遍，对于编译报错的，删除原有的import，然后重新import。
6. 缩略图/取帧的回调，AliyunIThumbnailFetcher$OnThumbnailCompletion.onThumbnailReady()参数有所变动，原先的SharableBitmap改为Bitmap，可以直接使用，无需做回收
7. 缩略图/取帧的接口(addVideoSource,addImageSource)增加转场时间的参数，如果导入的视频需要考虑转场效果的时间，则需要设置转场时间，如果不需要则填0就可以了
8. 移除ScaleMode类，由VideoDisplayMode类代替
9. AliyunIReocder、AliyunICrop现在支持多实例，原有生成类AliyunRecorderCreator、AliyunCropCreator中的destroy方法被移除
10. libQuCore-ThirdParty.so 由 libsvideo_alivcffmpeg.so替代
11. 部分结构类包置为发生改变，如在原包名下找不到该类，请删除该类的import地址，重新import
12. 修复了部分Crash Bug
13. 修复了倒播卡顿的bug
14. 修复了部分机型动效滤镜效果不对的问题
15. 添加转场效果（TransitionBase)，具体查看接口文档，同时AliyunIimport接口addVideo，addImage函数优化，去掉了原来的关于转场的inDuration，outDuration，overlapDuration三个参数，统一由TransitionBase的子类来提供更为丰富的转场效果。
16. 添加特技效果接口（AliyunIEditor#addFrameAnimation)，支持自定义动画，具体查看接口文档
17. 导入多段视频支持添加多个变速时间特效（反复和倒放还是只支持单段视频的）
18. 新增删除变速效果的接口(AliyunIEditor#deleteTimeEffect)
19. 指定流、指定时间添加高斯模糊效果(AliyunIEditor#applyBlurBackground)
20. 指定流、指定时间设置显示模式填充/裁剪(AliyunIEditor#addRunningDisplayMode)
21. 增加配音接口，配音接口的音效跟随时间特效变动(AliyunIEditor#applyDub)

## 其他
1. 废弃录制添加mv的相关接口，包括
int applyMv(EffectBean effectMv) 
void pauseMv()
void resumeMv()
void restartMv()
废弃之后，相关接口可以继续使用，我们会在未来的某个版本彻底移除这些接口。


# v3.6.5

## 功能更新
1. 合成不支持ffmpeg软编
2. 添加时间特效会先走onEnd回调问题
3. 编辑设置音量，合成时设置的值无效，并且会放大音量，更改SDK默认音量值
4. 部分视频裁剪在99%卡住
5. 部分手机裁剪后的视频，编辑预览播放卡顿
6. 部分华为手机上特效滤镜有虚线
7. 部分手机上remove music Crash问题
8. 修复倒播卡顿的问题
9. 解决了yuv转rgb使用bt709公式造成的色域问题，其他色域问题正在解决中
10. 支持aac sbr格式音频
11. 音频采样率不对的问题
12. 修复了一些特效滤镜的适配性问题
13. 更新上传库，新增的字段需要短视频那边也要重新集成新增接口。

## 接口变动
1. 增加了Alivc.jar，开发者的工程中需要加入对这个jar包的依赖。


# v3.6.0

## 接口变动

1. 多视频导入(AliyunIImport)添加视频/图片
addVideo/addImage参数发生变化，原先的fadeDuration现在拆分了，拆分成上一个视频的出场时间（outDuration），下一个视频的入场时间（inDuration），以及两段视频出场入场的重合时间（overlapDuration）

2. 创建AliyunIEditor时参数变化AliyunEditorFactory.creatAliyunEditor(Uri uri, EditorCallBack callback)，原参数只有uri,现在增加了EditorCallback，替代了之前的OnPlayCallback
其中
|旧接口|对应|新接口|
|:-|:-|:-|
|OnPlayCallback.onPlayCompleted|		对应 |EditorCallback.onEnd 		|
|OnPlayCallback.onError	|		对应 |	EditorCallback.onError 	|
|OnPlayCallback.onTextureIDCallback|	对应	|EditorCallback.onCustomRender|
|OnPlayCallback.onPlayStarted|		对应  |   去掉了|
|OnPlayCallback.onSeekDone|		对应	|去掉了|


3. 创建播放器实例接口createAliyunPlayer()已不存在，播放器接口AliyunIPlayer也去掉了，其中播放控制对应的方法直接使用AliyunIEditor中的方法
|旧接口|对应|新接口|
|:-|:-|:-|
|AliyunIPlayer.getCurrentPosition|对应|AliyunIEditor.getCurrentPlayPosition|
|AliyunIPlayer.getDuration|对应|AliyunIEditor.getDuration|
|AliyunIPlayer.getRotation|对应|AliyunIEditor.getRotation|
|AliyunIPlayer.getVideoHeight|对应|AliyunIEditor.getVideoHeight|
|AliyunIPlayer.getVideoWidth|对应|AliyunIEditor.getVideoWidth|
|AliyunIPlayer.isAudioSilent|对应|AliyunIEditor.isAudioSilense|
|AliyunIPlayer.isPlaying|对应|AliyunIEditor.isPlaying|
|AliyunIPlayer.pause|对应|AliyunIEditor.pause|
|AliyunIPlayer.resume|对应|AliyunIEditor.resume|
|AliyunIPlayer.seek|对应|AliyunIEditor.seek|
|AliyunIPlayer.setAudioSilense|对应|AliyunIEditor.setAudioSilense|
|AliyunIPlayer.setDisplayMode|对应|AliyunIEditor.setDisplayMode|
|AliyunIPlayer.setFillBackgroundColor|对应|AliyunIEditor.setFillBackgroundColor|
|AliyunIPlayer.setOnPlayCallbackListener|对应|去掉了|
|AliyunIPlayer.setOnPreparedListener|对应|去掉了|
|AliyunIPlayer.setVolume|对应|AliyunIEditor.setVolume|
|AliyunIPlayer.start|对应|AliyunIEditor.start|
|AliyunIPlayer.stop|对应|AliyunIEditor.stop|

**注意**
> 该版本去掉了OnPreparedListener这个接口，意味着编辑不需要再等待OnPrepared回掉了，只要AliyunIEditor.init成功以后就可以添加特效了。

4. 设置混音权重applyMusicMixWeight接口增加了参数id，主要是因为这个版本支持多股配音流，所以需要id来区分，关于接口的详细描述可以参考接口文档

5. getExporter接口已不存在，相关的合成接口直接使用AliyunIEditor中的即可
|旧接口|对应|新接口|
|:-|:-|:-|
|AliyunIExporter.startCompose|对应|compose|
|AliyunIExporter.cancel|对应|cancelCompose|
|AliyunIExporter.setTailWatermark|对应|去掉了|
|AliyunIExporter.clearTailWatermark|对应|去掉了|


6. AliyunICompose.startCompose参数有所变化，OnComposeCallback变为AliyunIComposeCallBack
7. 创建合成实例前需要调用AliyunIEditor#saveEffectToLocal();

*PS:其他未在文档中标述的接口参数变化，在编译阶段会报错，可以参考接口文档中对于新参数的描述进行修改*
