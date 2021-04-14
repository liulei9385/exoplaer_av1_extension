# exoplayer_av1_extension
exoplayer supprt video av1 format 

如何在exoplayer中使用



具体参考 

https://github.com/google/ExoPlayer/tree/release-v2/extensions/av1



在app.build 引入

```java
dependencies{
     implementation fileTree(include: ['*.aar'], dir: 'aars')
}
```



kotlin代码

```kotlin
val preferExtensionDecoders = true
val useExtensionRenderers = true //是否开启扩展

val extensionRendererMode = if (useExtensionRenderers)
    if (preferExtensionDecoders)
        DefaultRenderersFactory.EXTENSION_RENDERER_MODE_PREFER
    else DefaultRenderersFactory.EXTENSION_RENDERER_MODE_ON
else DefaultRenderersFactory.EXTENSION_RENDERER_MODE_OFF

// EXTENSION_RENDERER_MODE_ON or EXTENSION_RENDERER_MODE_PREFER 开启后会自动引入的

val context = myAppContext()

val mRendererFactory = DefaultRenderersFactory(context)
mRendererFactory.setExtensionRendererMode(extensionRendererMode)

val builder = DefaultLoadControl.Builder()
val mLoadControl = builder
    .setPrioritizeTimeOverSizeThresholds(true)
    .setBufferDurationsMs(
        15_000,
        15_000,
        300,
        4_000
    )
    .build()

val defaultTrackSelector = DefaultTrackSelector(myAppContext())
val player = SimpleExoPlayer.Builder(myAppContext(), mRendererFactory)
    .setLooper(Looper.getMainLooper())
    .setHandleAudioBecomingNoisy(true)
    .setUseLazyPreparation(true)
    .setTrackSelector(defaultTrackSelector)
    .setPauseAtEndOfMediaItems(false)
    .setHandleAudioBecomingNoisy(true)
    .setLoadControl(mLoadControl)
    .setSeekParameters(SeekParameters.CLOSEST_SYNC)
    .build()
```



😊

测试了一下配置比较低的手机播放会卡顿或者无法播放的情况，稍微好一点的手机可以流畅播放的。