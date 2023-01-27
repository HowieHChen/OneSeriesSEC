<div align="center">

# OneSeriesSEC  

[![License](https://img.shields.io/github/license/HowieHChen/OneSeriesSEC.svg?label=License)](https://github.com/HowieHChen/OneSeriesSEC/blob/master/LICENSE) 

</div>

推荐您先了解以下相关内容
- [OneSeriesStarter](https://github.com/HowieHChen/OneSeriesStarter)
- [自适应图标](https://developer.android.google.cn/guide/practices/ui_guidelines/icon_design_adaptive?hl=zh-cn)
- [启动画面](https://developer.android.google.cn/guide/topics/ui/splash-screen?hl=zh-cn)
- [[GUIDE] Apex Launcher Theme Tutorial](https://forum.xda-developers.com/showthread.php?t=1649891)

---

## 快速了解  

### 这是什么  
这是一个第三方的`三星主题`的图标包应用，它不是由三星的主题工具生成的，但在一定操作下能被三星的`主题商店服务`识别并使用。  

### 主要目的  
通过本项目，你可以根据自己的需求生成自己的`三星主题图标包`，你可以*相对*自由地创建自己的图标包。

### 关于主题公园
由于三星主题机制的限制，三星提供了`主题公园`以便用户使用第三方图标包，但`主题公园`为了提供如 *自定义图标大小形状与颜色*、*单个定制图标* 等功能，会对第三方图标包进行解包与二次处理并再次打包生成对应的`三星主题图标包`。  

`主题公园`提供了友善的用户交互，但也带了包括但不限于下述内容的限制
- 图标二次压缩导致的图标模糊
- 生成的图标包只支持单层的普通图标（无论第三方图标包是否是自适应图标包），由此造成了桌面的`拉面条动画`消失和部分应用 [启动画面](https://developer.android.google.cn/guide/topics/ui/splash-screen?hl=zh-cn) 的大图标有白边等。
- 生成的图标包只包含本机已安装应用的图标，这意味新装应用后需重新生成图标包。
- 图标的覆盖仅限于应用，无法为单个应用的不同 Activity 或快捷方式适配不同的图标  

而本项目绕过了`主题公园`的二次打包过程，直接生成`三星主题图标包`。

### 特性与问题 

#### 主要特性 
- 支持[自适应图标](https://developer.android.google.cn/guide/practices/ui_guidelines/icon_design_adaptive?hl=zh-cn)与传统单层图标
- 自定义图标底板与蒙板（也可都不使用，一般情况下只对未适配的应用生效）
- 全局图标缩放
- 图标将被直接使用，画质不会被再压缩
- 自动覆盖新应用（图标包内已适配的应用）

#### 已知问题
- 由于并未被三星官方明面支持，所以图标包的效果会受系统版本影响。也可能会在后续的系统更新中失效。
- 只支持静态图标，不支持动态图标（适配时钟与日历后，这两个应用的图标不会再动态更新）
- 消息框 ([Toast](https://developer.android.google.cn/guide/topics/ui/notifiers/toasts?hl=zh-cn)) 、最小化窗口的悬浮图标等处无法获取自适应图标，会有白底或黑底。 (**OneUI 4.X**, 自适应图标)
- 在应用图标包后，锁屏快捷方式无法获取自适应图标，无法实现像默认图标时的半透明样式。 (**OneUI 5.X**, 自适应图标)
- `OneUI 启动器`的图标与系统别处图标的逻辑不太一样，蒙板与底板的使用效果不一致，统一桌面图标大小后系统内别处图标大小会不一致。 (**OneUI 5.X**, 自适应图标)
- 自适应图标的形状受系统限制，无法更改。
- 由于三星在 OneUI 3.1.1 及以后版本中才支持应用图标包至第三方应用，所以在更早的系统版本中无法使用。
- 应用图标包后，未适配应用的图标将被强制为单层图标，丢失自适应图标及其特性。

---

## 使用说明

### 配置需求
- One UI 3.1.1 (Android 11) 或更高版本 (折叠屏设备)
- One UI 4.0 (Android 12) 或更高版本 (除折叠屏设备以外的设备)
- 特殊说明: One UI 3.1 及以下版本的操作系统上无法使用

### 主要规则
- 图标通过定义可绘制对象(drawable)来创建，可以使用xml来创建自适应图标（详见[自适应图标](https://developer.android.google.cn/guide/practices/ui_guidelines/icon_design_adaptive?hl=zh-cn)），也可直接以PNG等图片资源创建。
- 图标通过包名与类名（可选）来匹配，所以图标的命名即为应用的包名与类名（可选），并将 **.** 换为 **_** ，大小换为小写。当图标名只有包名时，将覆盖整个应用，当图标名为包名与类名时，将只覆盖这个类（优先级大于只有包名的图标）。你可以通过下面的例子来具体了解。  

| 应用 | 包名 | 类名 | 自适应图标 | 普通图标 |
| --- | --- | --- | --- | --- |
| QQ | com.tencent.mobileqq | 无 | com_tencent_mobileqq.xml | com_tencent_mobileqq.png |
| Google | com.google.android.googlequicksearchbox | 无 | com_google_android_googlequicksearchbox.xml | com_google_android_googlequicksearchbox.png |
| Google 语音搜索 | com.google.android.googlequicksearchbox | com.google.android.googlequicksearchbox.VoiceSearchActivity | com_google_android_googlequicksearchbox_voicesearchactivity.xml | com_google_android_googlequicksearchbox_voicesearchactivity.png |

- **drawable-xxxhdpi** 文件夹中的 *ic_icon_bg.png* 和 *ic_mask.png* 为底板和遮罩，为三星默认图标形状，你可以将它们替换为自己需要的形状，也可以直接删除。

### 快速上手
1. 安装并配置 Android Studio 或类似产品
2. 在本项目页面点击 Code -> Download ZIP 将本项目下载到本地（或者直接点这里下载[源代码](https://github.com/HowieHChen/OneSeriesSEC/archive/refs/heads/master.zip)）
3. 解压压缩包（到某个目录），打开 Android Studio ，点 File -> Open ，找到对应目录并打开项目 
4. 将自己的图标资源按规则命名好并放入对应文件夹 (项目已包含大量的图标，自适应图标定义在 **drawable-anydpi-v26** 文件夹中，其对应的图片文件在 **mipmap-nodpi** 文件夹中)
5. 修改配置项（可选，不清楚的直接跳过）
6. 打开 **res\values\strings.xml** ，修改 **app_name** 为自己的应用名；打开 **assets\themes.json** ，修改 **title** 为自己的图标包名字
7. 打开 **app** 目录下的 **build.gradle** ，修改 **versionCode** 和 **versionName** （**versionCode** 过低会导致三星主题商店提示更新而无法应用，默认为1000，若提示更新可改至2000或更高）
8. 打开 **app** 目录下的 **build.gradle** ，修改 **applicationId** 的值为你要替换的图标包的应用包名（若不清楚，请前往 [OneSeriesStarter](https://github.com/HowieHChen/OneSeriesStarter)）
9. 打包应用（若不清楚，可参考[相关教程](https://cloud.tencent.com/developer/article/1772561)）
- 由于不支持动态图标，故分成了 **standard** 和 **special** 两个版本，前者覆盖了时钟和日历应用而后者没有。 

### 使用图标包
1. 下载任一主题商店图标包 (也可使用主题公园创建个新的图标包)
2. 获取该图标包的包名 (主题商店内下载的图标包可在图标详情页点右上角的分享获取URL，其中**appId=**后面部分即为图标包的包名。主题公园创建新图标包可在创建前打开酷安的应用管理页并挂后台，再回主题公园创建图标包，成功后返回酷安即可看到新增加的图标包，可复制包名或卸载)
3. 卸载第一步安装的图标包（ADB卸载、酷安卸载、使用 [OneSeriesStarter](https://github.com/HowieHChen/OneSeriesStarter) 内的控制中心卸载）
4. 构建你自己的图标包（详见[快速上手](https://github.com/HowieHChen/OneSeriesSEC#%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B) ，其中的 **applicationId** 填入第二步获取的包名）
5. 安装你自己打包生成的图标包
6. 回到主题商店/主题公园应用

### 可配置项目
| 键 | 位置 | 类型 | 值 | 说明 | 
| --- | --- | --- | --- | --- |
| theme_designer_enable_third_party_app_icon | res\values\bool.xml | bool | true / false | 图标是否覆盖第三方应用 |
| mask_from_theme | res\values\bool.xml | bool | true / false | 是否使用图标包内的遮罩 |
| icon_scale_size | res\values\integer.xml | integer | 0-100 (也可大于100) | 图标缩放系数 |
| icon_bg_range | res\values\integer.xml | integer | 0、1、2、3 | 自适应图标置为1。 置为2时不额外添加图标底板 (具体细节暂时未知) |
| app_name | res\values\string.xml | string | 文本 | 应用名 |
| thumbnail_iconpack.jpg | assets\preview | JPG | 图片 | 在主题商店中的缩略图 |
| iconpack_summary.jpg</br>iconpack_preview1.jpg</br>iconpack_preview2.jpg | assets\preview | JPG | 图片 | 在主题商店详情页中的展示图（图片命名与顺序可在assets\themes.json中修改） |