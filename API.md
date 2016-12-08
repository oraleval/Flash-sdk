
# 云知声口语评测开发指南 Flash 版




| 修改人 | 修改时间 | 修改内容 |
| --- | --- | --- |
| [support@unisound.com](file:///h) | 2013-10-11 | Flash SDK 版本初始化，发布版本：v1.0 |
| [lutao@unisound.com](file:///h); [liukaijin@unisound.com](file:///h) | 2015-4-10 | 增加录音音量大小接口、定制化调整参数等,发布版本：v2.0 |
| [yankaituo@unisound.com](file:///h) | 2015-4-22 | 增加各接口、事件定义说明,发布版本：v2.1 |
| [lutao@unisound.com](file:///h) | 2015-5-14 | 删除init()方法中的APPKey参数，APPKey默认编译到sdk中 |
| [lutao@unisound.com](file:///h) | 2015-5-20 | Init()方法中修改port默认值为8085，增加backupPort默认值为80 |
| [lutao@unisound.com](file:///h) | 2015-5-28 | Init()方法中增加一个mode参数，默认值为NetworkMode.SOCKET，其他可选值为NetworkMode.HTTP |
| [lutao@unisound.com](file:///h) | 2015-6-17 | 增加setUserID方法 |
| [lutao@unisound.com](file:///h) | 2015-7-2 | 增加setEvalTimeOut方法 |
| [lutao@unisound.com](file:///h) | 2015-9-25 | 新增-11006错误 |
| [lutao@unisound.com](file:///h) | 2015-10-15 | 修改3个错误码与移动端统一 |
| [lutao@unisound.com](file:///h) | 2016-1-20 | 增加实时音量检测事件volume和麦克风静音检测事件microphoneMuted |
| [lutao@unisound.com](file:///h) | 2016-1-25 | 服务端错误码修改为未修饰的原始错误码 |
| [lutao@unisound.com](file:///h) | 2016-4-11 | 新增-50013错误及描述 |
| [lutao@unisound.com](file:///h) | 2016-6-27 | 新增-61002错误及描述 |
| [lutao@unisound.com](file:///h) | 2016-7-18 | 新增retry()方法，在评测失败的情况下主动重试 |
| [lutao@unisound.com](file:///h) | 2016-10-12 | 新增异步评测相关属性和识别相关属性 |
| [lutao@unisound.com](file:///h) | 2016-11-01 | 打分系数边界值变更 |
| [huxiaofei@unisound.com](file:///h) | 2016-11-23 | 新增第三次http评测地址修改和获取opus流 |



## 1.概述

>口语评测Flash SDK文档，旨在为第三方应用，便利集成云知声的英语口语评测服务；

### 1.1.目的

>本文档对英语口语评测接口定义进行说明；

>文档试用读者范围为：教育产品设计师、Flash软件工程师；

### 1.2.范围

>本文档定义英语口语评测的集成使用说明、体系结构、接口定义、返回结果定义等；

>不包含核心引擎的性能定义，也不包含其它配套或附赠产品的使用说明；


## 2.开发准备

### 2.1.环境说明

>1、申请APPKEY、开发Flash SDK；

>2、浏览器支持Flash Player 11.1 及以上版本、开发环境为Adobe Flash Builder 4.6；

>3、网络环境、外置麦克风；

- Tips :

 1、开发环境Adobe Flash Builder 4.7兼容性不好，不建议使用；

 2、个人开发者，请在云知声的开发者官网申请，地址为： [http://dev.hivoice.cn/index.jsp](file:///h)；


### 2.2.开发环境搭建

> 开发环境的搭建，以Adobe Flash Builder 4.6开发环境为例，演示加载Flash SDK的库文件进行开发的过程，其他开发IDE平台同理；

>1、添加库路径

>打开 Flash Builder 开发工具，单击【项目】-&gt;【属性】，点击左侧【ActionScript构建路径】，出现如下图所示。

点击【库路径】选项卡，添加口语评测的Flash SDK库文件.
>2、添加库引用

>>**import** cn.yunzhisheng.oraleval.sdk.OralEvalSDK;


## 3.功能模块描述

>在Flash SDK中，集成丰富的参数、功能模块，如音频增益模块、音量返回模块等，如下表：

| 参数、功能 | 功能说明 |
| --- | --- |
| 自动增益功能 | 自动增大评测过程中录音过小的问题； |
| 麦克风索引资源参数 | 提供麦克风给用户候选； |
| 回放录音大小参数 | 调整回放录音的音量大小； |
| 异步录音数据处理 | 解决TCP建链前，用户等待或用户录音截取的问题； |
| 数据音频回溯服务 | SDK返回评测音频的URL地址，支持历史音频回放； |
| 录音声音大小参数 | 实时返回评测过程中的录音音量大小； |
| 评测定制化调整参数 | 针对不同用户群体，提供灵活的定制化的打分评测参数； |
| …. | …. |
|   |   |


## 4.属性

### 4.1.VERSION属性 [静态]

>VERSION:String

>>说明：引擎的版本号。可直接通过OralEvalSDK.VERSION调用。


### 4.2.appKey属性

>appKey:String

>>说明：引擎的APPKey。

>注意：init()方法中不再需要传入APPKey，现在的APPKey默认编入sdk代码中了。


### 4.3.isRecording 属性

>isRecording:Boolean [只读]

>>说明：返回引擎是否处于录音状态。


### 4.4.result属性

>result:String [只读]

>>说明：获取打分结果的JSON字符串。

>>在Event.COMPLETE调度后可用。


### 4.5.pcmData属性

>pcmData:ByteArray  [只读]

>>说明：获取录音的PCM数据。

>>在Event.COMPLETE调度后可用。


### 4.6.opusData属性

>opusData:ByteArray  [只读]

>>说明：获取录音的OPUS数据。

>>在Event.COMPLETE调度后可用。


### 4.7.wavData属性

>wavData:ByteArray  [只读]

>>说明：获取录音的wave数据。

>>在Event.COMPLETE调度后可用。

### 4.8.sessionID属性

>sessionID:String  [只读]

>>说明：获取录音的sessionID数据。可以通过这个ID组合成MP3回放的URL。

>>在Event.COMPLETE调度后可用。


### 4.9.replayURL属性

>replayURL:String  [只读]

>>说明：获取录音的音频URL。数据是MP3数据，可以直接用Sound类播放。

>>在Event.COMPLETE调度后可用。

### 4.10.averageVolume属性

>averageVolume:Number [只读]

>>说明：获取录音的平均音量数值。

>>在Event.COMPLETE调度后可用。

### 4.11. isAsync属性

>isAsync:Boolean

>>说明：默认值为false。值为true时，打分机制改为异步打分，不直接给出打分结果，而是给出打分结果的URL。需使用此功能，请在init方法后设置此属性。

>>见resultURL属性。

### 4.12. resultURL属性

>resultURL:String [只读]

>>说明：异步打分时，获取打分结果的URL。

>>注：打分结果并非立即可查询。

### 4.13. asrMode属性

>asrMode:Boolean

>>说明：默认为false，值设为true时改变sdk的功能为语音识别。如需使用此功能，init方法中请将评测地址改写为识别相关地址及端口。


## 5.方法

### 5.1.获取识别对象实例getInstance方法

>**public static**** function** getInstance():OralEvalSDK

>> 参数：无

>> 说明：[静态]获取口语评测的识别器对象；


### 5.2.初始化 init 方法

>>**public**** function**init(server:String =**&quot;eval.hivoice.cn&quot;**, port:int = 8085, backupPort:int = 80, mode:String = NetworkMode.SOCKET):**void**

>>参数：

>>>a、server:String(default = **&quot;eval.hivoice.cn&quot;** )服务器地址。

>>>b、port:int(default = 8085)端口号。

>>>c、backupPort:int(default = 80)端口号。

>>>d、mode:String(default = **&quot;socket&quot;** )服务器地址。可输入值为NetworkMode.SOCKET和NetworkMode.HTTP。

>>说明：初始化公有云评测引擎；普通用户仅在初始化时调用init()即可，无需设置任何参数。


### 5.3.设置用户setUserID方法

>**public**** function**setUserID(id:String):**void**

>>参数：

>>>id:String 用户ID；

>>>说明：设置用户ID可以在数据服务中根据用户ID查询独立用户的使用数据。非必须项。

### 5.4.设置音量setVolumeLimited方法

>**public**** function**setVolumeLimited(low:int = 0, high:int = 100):**void**

>>参数：

>>> a、low:int(default = 0) 平均音量检测警告的下边界。0为不警告；
>>> b、high:int(default = 100) 平均音量检测警告的上边界。100为不警告；

>>>说明：high建议设置为：90，设置过大，用户音量过大，会导致截幅，影响评测打分；必须在每次录音前调用以重置。

### 5.5.设置打分超时setEvalTimeOut方法

>**public**** function**setEvalTimeOut(ms:int):**void**

>>参数：

>>>ms:int(default = 0) 打分超时时间，单位为毫秒。0为不超时；

>>>说明：这超时的时间指从用户或vad停止录音到获得打分结果之间的时间；在每次录音前调用以重置。超时时调度-20007打分超时错误。

### 5.6.设置打分系数setScoreCoefficient方法

>**public**** function**setScoreCoefficient(coefficient:Number = 1.0):**void**

>>参数：

>>> a、coefficient:Number(default = 1.0)用于调整评测引擎打分定制化参数，取值范围是：0.6至1.9，值越小打分越严格。超出边界的取值会自动被边界数值替代。

>>>说明：针对不同的评测用户群体，提供定制化的打分调整；必须在每次录音前调用以重置。

### 5.7.设置VAD超时setVadTimeOut方法

>**public**** function**setVadTimeOut(frontSilence:int, backSilence:int):**void**

>>参数：

>>> a、frontSilence:int 设置VAD的前端点时间，单位为：10ms，建议值为：200，即2S；

>>> b、backSilence:int设置VAD的后端点时间，建议值为：200，即2S；

>>>说明：支持评测结束后，VAD超时自动停止；必须在每次录音前调用以重置。

### 5.8.设置评测文本setContent方法

>**public**** function**setContent(text:String, serviceType:String = ServiceType.A):**void**

>>参数：
>>> a、text:String设置评测文本的内容；

>>> b、serviceType:String(default = ServiceType.A)评测类型。

>>> 说明：设置评测文本方法。必须在每次录音前调用以重置。

### 5.9.开始评测start方法

>**public**** function**start(gain:Number = 50, microphone:Microphone =**null**):**void**

>>参数：

>>> gain:Number(default = 50)麦克风增益；

>>> microphone:Microphone(default = **null** )麦克风对象。

>>> 说明：

>>>  加载用户评测文本，开始录音评测。如需手动控制麦克风对象的属性，请作为参数附加。

### 5.10.停止评测stop方法

>**public**** function**stop():**void**

>>参数：无

>>> 说明：用户一次评测完毕后，调用Stop结束；
### 5.11.重试评测retry方法

>**public**** function**retry():**void**

>>参数：无

>>> 说明：用户一次评测完毕，获得-7等失败的错误码，可以直接调用retry方法重新对上一次的录音进行评测；

### 5.12.设置第三次http评测地址

>**public**** function**setHttpServer(ip:String, port:int):**void**

>>参数：
>>> ip:String 要设置第三次http评测的IP地址

>>> port:int 端口号(default =80)

>>> 说明：用户前两次评测超时后第三次评测是将优先使用,此时设置的ip地址, 如果不设置将随机获取一个评测地址；


## 6.事件
### 6.1.complete事件

>事件对象类型: flash.events.Event

>>属性 Event.type = flash.events.Event.COMPLETE

>>在打分完成后调度。仅在该事件调度以后，result，pcmData，wavData，sessionId，averageVolume属性的值才有意义。

### 6.2.error事件

>事件对象类型: flash.events.ErrorEvent

>>属性 Event.type = flash.events.ErrorEvent.ERROR

>>在打分发声错误时调度。参见错误代码说明。

### 6.3.beginOfSpeech事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.BEGIN\_OF\_SPEECH

>>在麦克风激活（开始录音）时调度。

### 6.4.endOfSpeech事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.END\_OF\_SPEECH

>>在停止录音时调度。

### 6.5.vadTimeOut事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.VAD\_TIME\_OUT

在发生VAD超时自动停止时调度。

### 6.6.outOfMaxLimited事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.OUT\_OF\_MAX\_LIMITED

>>在打分后，发现最大音量超出警告上边界时调度。

### 6.7.outOfMinLimited事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.OUT\_OF\_MIN\_LIMITED

>>在打分后，发现最大音量低于警告下边界时调度。

### 6.8.microphoneMuted事件

>事件对象类型: cn.yunzhisheng.oraleval.sdk.events.USCEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCEvent.MICROPHONE\_MUTED

>>仅在开始录音时检测。如发现麦克风静音，则抛出该事件。

### 6.9.Volume事件

>事件对象类型：cn.yunzhisheng.oraleval.sdk.events.USCVolumeEvent

>>属性 Event.type = cn.yunzhisheng.oraleval.sdk.events.USCVolumeEvent.VOLUME

>>实时音量返回。如需侦听，建议在beginOfSpeech以后侦听，在endOfSpeech以后停止。


## 7.评测示例（Sample）

**package**

{

        **import** cn.yunzhisheng.oraleval.sdk.OralEvalSDK;

        **import** cn.yunzhisheng.oraleval.sdk.events.USCEvent;

        **import** flash.display.Sprite;

        **import** flash.events.ErrorEvent;

        **import** flash.events.Event;

        **public**** class **Sample** extends** Sprite

        {

                **private**** var** \_sdk:OralEvalSDK;

                **private**** var **\_content:String =**&quot;nice to meet you&quot;**;

                **public**** function** Sample()

                {

                        \_sdk = OralEvalSDK.getInstance();

                        \_sdk.init();

                }

                /\*\*

                \* setVolumeLimited方法可以设置音量警告的上下限，默认值位0和100

                \* setScoreCoefficient设置打分系数，有效值为0.8-1.5，默认值为1.0，

                \*/

                **private**** function**settings():**void**

                {

                        \_sdk.setVolumeLimited(minVolume, maxVolume);

                        \_sdk.setScoreCoefficient(1.0);

                }

                /\*\*

                \* 开始录音，\_content是需要打分的内容文本

                \* setVadTimeOut设置vad作用时间，单位是10ms，默认值为200和200

                \* 如需侦听音量警告，请在调用start方法前使用setVadTimeOut方法

                \* start方法可以有以下参数

                \* \_sdk.start(gain, microphone);

                \* gain 默认值为50

                \* microphone 默认值为 Microphone.getMicrophone()

                \*/

                **private**** function**startRecord():**void**

                {

                        \_sdk.setContent(\_content);

                        \_sdk.setVadTimeOut(200, 200);

                        \_sdk.start();

                        \_sdk.addEventListener(Event.COMPLETE, completeHandler);

                        \_sdk.addEventListener(ErrorEvent.ERROR, errorHandler);

                        \_sdk.addEventListener(USCEvent.BEGIN\_OF\_SPEECH, startHandler);

                        \_sdk.addEventListener(USCEvent.END\_OF\_SPEECH, endHandler);

                        \_sdk.addEventListener(USCEvent.OUT\_OF\_MAX\_LIMITED, outOfMaxHandler);

                        \_sdk.addEventListener(USCEvent.OUT\_OF\_MIN\_LIMITED, outOfMinHandler);

                }

                /\*\*

                \* 停止录音（此处不要删除侦听）

                \*/

                **private**** function**stopRecord():**void**

                {

                        \_sdk.stop();

                }

                /\*\*

                \* 打分成功回调函数

                \*/

                **private**** function**completeHandler(e:Event):**void**

                {

                        \_sdk.removeEventListener(Event.COMPLETE, completeHandler);

                        \_sdk.removeEventListener(ErrorEvent.ERROR, errorHandler);

                        \_sdk.removeEventListener(USCEvent.BEGIN\_OF\_SPEECH, startHandler);

                        \_sdk.removeEventListener(USCEvent.END\_OF\_SPEECH, endHandler);

                        \_sdk.removeEventListener(USCEvent.OUT\_OF\_MAX\_LIMITED, outOfMaxHandler);

                        \_sdk.removeEventListener(USCEvent.OUT\_OF\_MIN\_LIMITED, outOfMinHandler);

                        **trace** (\_sdk.result);

                        \_sdk.pcmData; _//录音的本地pcm数据_

                        \_sdk.wavData; _//录音的本地wav数据_

                }

                /\*\*

                \* 打分失败回调函数

                \*/

                **private**** function**errorHandler(e:ErrorEvent):**void**

                {

                        \_sdk.removeEventListener(Event.COMPLETE, completeHandler);

                        \_sdk.removeEventListener(ErrorEvent.ERROR, errorHandler);

                        \_sdk.removeEventListener(USCEvent.BEGIN\_OF\_SPEECH, startHandler);

                        \_sdk.removeEventListener(USCEvent.END\_OF\_SPEECH, endHandler);

                        \_sdk.removeEventListener(USCEvent.OUT\_OF\_MAX\_LIMITED, outOfMaxHandler);

                        \_sdk.removeEventListener(USCEvent.OUT\_OF\_MIN\_LIMITED, outOfMinHandler);

                        **trace** (e.text);

                }

                /\*\*

                \* microphone开始录音时调度

                \*/

                **private**** function**startHandler(e:USCEvent):**void**

                {

                        _// TODO Auto-generated method stub_

                }

                /\*\*

                \* microphone停止录音时调度

                \* 如果设置了vad，请在此方法下添加停止录音后的重置界面

                \*/

                **private**** function**endHandler(e:USCEvent):**void**

                {

                        _// TODO Auto-generated method stub_

                }

                /\*\*

                \* 平均音量超出设置的上边界时调度

                \*/

                **private**** function**outOfMaxHandler(e:USCEvent):**void**

                {

                        _// TODO Auto-generated method stub_

                }

                /\*\*

                \* 平均音量超出设置的下边界时调度

                \*/

                **private**** function**outOfMinHandler(e:USCEvent):**void**

                {

                        _// TODO Auto-generated method stub_

                }

        }

}


## 8.结果格式说明

### 8.1.返回结果格式（Json）

{

    &quot;version&quot;: &quot;full 1.0&quot;,

    &quot;lines&quot;: [

        {

            &quot;sample&quot;: &quot;good morning &quot;,

            &quot;usertext&quot;: &quot;good morning&quot;,

            &quot;begin&quot;: 0,

            &quot;end&quot;: 2.5,

            &quot;score&quot;: 67,

            &quot;fluency&quot;: 85,

            &quot;pronunciation&quot;: 66,

            &quot;integrity&quot;: 100,

            &quot;words&quot;: [

                {

                    &quot;text&quot;: &quot;sil&quot;,

                    &quot;type&quot;: 4,

                    &quot;begin&quot;: 0.05,

                    &quot;end&quot;: 1.5,

                    &quot;volume&quot;: 6.9,

                    &quot;score&quot;: 5.6

                },

                {

                    &quot;text&quot;: &quot;good&quot;,

                    &quot;type&quot;: 2,

                    &quot;begin&quot;: 1.5,

                    &quot;end&quot;: 1.7,

                    &quot;volume&quot;: 6,

                    &quot;score&quot;: 9.2

                },

                {

                    &quot;text&quot;: &quot;morning&quot;,

                    &quot;type&quot;: 2,

                    &quot;begin&quot;: 1.7,

                    &quot;end&quot;: 2.5,

                    &quot;volume&quot;: 7.9,

                    &quot;score&quot;: 4.2

                },

                {

                    &quot;text&quot;: &quot;sil&quot;,

                    &quot;type&quot;: 4,

                    &quot;begin&quot;: 2.5,

                    &quot;end&quot;: 2.5,

                    &quot;volume&quot;: 1.3,

                    &quot;score&quot;: 6.8

                }

            ]

        }

    ]

}

### 8.2.数据格式说明

| 名称 | 类型 | 说明 |
| --- | --- | --- |
| version | string | 结果格式版本及版本号 |
| lines | array | 每行输入文本的评测结果 |
| sample | string | 输入的标准文本 |
| usertext | string | 用户实际朗读的文本（语音识别结果） |
| begin | double | 开始时间，单位为秒 |
| end | double | 结束时间，单位为秒 |
| volume | double | 音量 |
| score | double | 总分分值 |
| fluency | double | 流利度分值 |
| pronunciation | double | 标准度分值 |
| integrity | double | 完整度分值 |
| words | array | 每个词的评测结果 |
| text | string | 词的字符串，使用&quot;sil&quot;表示语音中的静音段。 |
| type | int | 类型，共有6种类型，分别是：0 多词，1 漏词，2 正常词， 3错误词，4 静音，5 重复词 |

## 附1：        客户端错误代码说明

| 错误代码 | 备注说明 |
| --- | --- |
| -10001 | 服务器通讯错误 |
| -7（原-10002） | 服务器连接失败 |
| -12010 | 安全沙箱冲突2010 |
| -12048 | 安全沙箱冲突2048 |
| -20001 | 服务器验证错误 |
| -20007 | 打分超时错误 |
| -30002 | 说话时间超出限制 |
| -30003 | 数据压缩错误 |
| -50013 | 本地socket错误 |
| -52001 | Opus编码错误 |
| -61001 | 麦克风检测失败 |
| -61002 | 没有可用的麦克风，可能被占用 |
| -1001（原-61002） | 录音异常 |
| -62001 | 识别异常 |

## 附2：服务端错误代码说明

| **错误码** | **错误码含义** | **错误码说明** |
| --- | --- | --- |
| 0x0000FFFD | 用户传过来的appkey 错误 |   |
| 0x0000FFFC | 引擎服务stop返回错误 |   |
| 0x0000FFF7 | 用户传过来的文本是空 | 用户传过来的文本是空，或者用户文本全部由空字符组成 |
| 0x0000E001 | 连接引擎服务失败 |   |
| -2001 | 传给引擎服务的用户文本全是生词 |   |
| 0x0000E007 | 传给引擎服务的用户文本过长 |   |
| 0x0000E008 | 引擎非用户引起的出错，此错误非用户传入的文本和音频引起 | 当出现此错误码，日志等级设置为error |
| 0x00002001 | 连接n2t服务出错 |   |
| 0x00002003 | n2t解析错误 | 当出现此错误码，日志等级设置为error |
| 0x00002006 | n2t组装结果出错,只有enstar和E模式会组装结果 | 当出现此错误码，日志等级设置为error |
| 0x0000C001 | 连接转码服务出错 |   |
| 0x0000C003 | 转码服务转码过程中出错 |   |
| 0x0000F000 | 后端服务内部未知错误 |   |
| 0x0000F001 | 用户上传的TASK格式错误 |   |
| 0x0000FFFD | 用户传过来的appkey 错误 |   |
| 0x0000FFFC | 引擎服务stop返回错误 |   |
| 0x0000E001 | 连接引擎服务失败 |   |
| 0x0000FFF4 | 最开始从网络上读取私有协议8个字节的frame出错 | 服务端第一次解析。客户端发过来的包。会有超时，超时时间是8秒。如果网络不好，会出现这种错误 |
| 0x0000FFF6 | 不是私有协议的客户端 | 数据格式错误 |
| 0x0000FFF5 | 当私有协议单个包 大于32MB(2 &lt;&lt; 25)时报错 |   |
| 0x0000D001 | http client数据格式错误 | http client数据格式错误 |
| 0x0000FF0F | 用户传过来的appkey为空 |   |
