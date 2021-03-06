
---
title: Publish and Subscribe to Streams
description: 
platform: Android
updatedAt: Tue Dec 11 2018 20:50:55 GMT+0000 (UTC)
---
# Publish and Subscribe to Streams
Before publishing or subscribing to any streams, ensure that you prepared the development environment and joined the channel. See [Integrate the SDK](../../en/Video/android_video.md).

## Implementation
###  Enable the video mode
Call the `enableVideo` method to enable the video mode. The voice function is enabled by default in the Agora SDK, so you can call the `enableVideo` method before or after joining a channel.

-   If you enable the video mode before joining a channel, you enter directly into a video call.
-   If you enable the video mode after joining a channel, the voice call switches to a video call.


```
mRtcEngine.enableVideo();
```

### Set the video profile
After the video mode is enabled, use the `setVideoEncoderConfiguration` method to set the video encoder profile.

In the `setVideoEncoderConfiguration` method, specify the video encoding resolution, frame rate, bitrate, and orientation mode. See the `setVideoEncoderConfiguration` method for more information.

> -   The parameters specified in the `setVideoEncoderConfiguration` method are the maximum values under ideal network conditions. If the video engine cannot render the video using the specified parameters due to poor network conditions, the parameters further down the list are considered until a successful configuration is found.
> -   If the device camera does not support the resolution specified by the video profile, the SDK automatically chooses a suitable camera resolution. This does not change the encoder resolution, and encoding will still use the resolution specified by the `setVideoEncoderConfiguration` method.

```
VideoEncoderConfiguration.ORIENTATION_MODE
orientationMode =
VideoEncoderConfiguration.ORIENTATION_MODE.ORIENTATION_MODE_FIXED_PORTRAIT;

    VideoEncoderConfiguration.VideoDimensions dimensions = new VideoEncoderConfiguration.VideoDimensions(360, 640);

    VideoEncoderConfiguration videoEncoderConfiguration = new VideoEncoderConfiguration(dimensions, frameRate, bitrate, orientationMode);

mRtcEngine.setVideoEncoderConfiguration(videoEncoderConfiguration);
```

### Set the local video view
The local video view is the display area of the local video streams on the user’s local device.

Call the `setupLocalVideo` method before entering into a channel to bind the application with the video window of the local stream and configure the local video display. The `setupLocalVideo` method creates a `surfaceView` object for the video stream by initializing:

-   For the Z order media overlay using `surfaceView.setZOrderMediaOverlay`, setting the parameter to true places the view over of the parent view.
-   For `surfaceView`, add `surfaceView` to the `local_video_view_container` layout `container`.

To complete the local video setup, pass a new `VideoCanvas` object to mRtcEngine. This binds the video window `surfaceView` and configures the video display settings.

```
 private void setupLocalVideo() {
    FrameLayout container = (FrameLayout) findViewById(R.id.local_video_view_container);
    SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
    surfaceView.setZOrderMediaOverlay(true);
    container.addView(surfaceView);
    mRtcEngine.setupLocalVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, 0));
}
```

### Set the remote video view
The remote video view is the display area of the remote video streams on the user’s local device.

Call the `setupRemoteVideo` method to set the video display view for the remote user with their guid.

In the `setupRemoteVideo` method:

-   Gets a reference to the remote video view in the layout.
-   Creates and adds a **View** object to the layout.
-   Creates a **VideoCanvas** and associates the view with it.
-   Tags the **View** with the channel ID.
-   Hides the quick tips.

Users need to pass the uid of the remote user in this method. If the remote uid is unknown to the app, set it when the app receives the `onUserJoined` event.


```
private void setupRemoteVideo(int uid) {
   FrameLayout container = (FrameLayout) findViewById(R.id.remote_video_view_container);

   if (container.getChildCount() >= 1) {
       return;
   }

   SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
   container.addView(surfaceView);
   mRtcEngine.setupRemoteVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, uid));

   surfaceView.setTag(uid);
   View tipMsg = findViewById(R.id.quick_tips_when_use_agora_sdk);
   tipMsg.setVisibility(View.GONE);
  }
```

## Next Steps
You are now in a video call. When the call ends, use the Agora SDK to exit the current call:

- [Leave the Channel](../../en/Video/leave_android.md)

For other functions such as manipulating the audio volume, audio effect, video resolution or video source, you can refer to the following sections:

- [Adjust the volume](../../en/Video/volume_android.md)
- [Play the Audio Effects/Audio Mixing](../../en/Video/effect_mixing_android.md)
- [Use In-ear Monitoring](../../en/Video/in-ear_android.md)
- [Adjust the Pitch and Tone](../../en/Video/voice_effect_android.md)
- [Set the Video Profile](../../en/Video/videoProfile_android.md)
- [Customize the Video/Audio Source and Renderer](../../en/Video/custom_video_android.md)
- [Share the Screen](../../en/Video/screensharing_android.md)
