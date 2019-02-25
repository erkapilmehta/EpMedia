[![](https://img.shields.io/badge/minSdkVersion-16-green.svg)](https://developer.android.google.cn) [![](https://img. Shields.io/badge/FFmpeg-3.3.4-orange.svg)](https://ffmpeg.org/download.html#release_3.3) [![](https://img.shields.io/badge /release-v0.9.5-blue.svg)](https://github.com/yangjie10930/EpMedia)

# EpMedia
The video processing framework developed based on FFmpeg is easy to use and small in size, helping users to quickly implement video processing functions. It includes the following functions: editing, cropping, rotating, mirroring, merging, separating, adding LOGO, adding filters, adding background music, accelerating deceleration video, and rewinding audio and video. </br>

At present, we are still improving and fixing some bugs. If you encounter problems in use, please leave a message in Issues or contact me: yangjie10930@sina.cn.

If you get an error, please post the logcat information together.

If you use it, please give it a star. Thank you for your support and encouragement O(∩_∩)O

<a href="https://github.com/yangjie10930/EpMediaDemo" target="_blank">Demo click here</a>

PS: I plan to fix some bugs in the near future, and compile the compiled scripts open source, replace the new version of FFmpeg, and then publish a full-format supported so library, but because of the recent busy work, innocent avatars, can only be busy with this array to deal with, hope Can have a certain understanding of audio and video friends to help me improve this library, friends who are willing to contact me, I will provide the corresponding resources, thank you!

## Instructions:
* Added in build.gradle:
```Java
Allprojects {
Repositories {
...
Maven { url 'https://jitpack.io' }
}
}
```
* Add gradle dependencies:
```Java
Compile 'com.github.yangjie10930:EpMedia:v0.9.5'
```
## Single video processing:
* Create a pending video:
```Java
EpVideo epVideo = new EpVideo(url);
```
* Clip
```Java
// One parameter is the start time of the clip, and the second parameter is the duration, in seconds.
epVideo.clip(1,2);//Starting from the first second, editing for two seconds
```
* Cropping
```Java
//The parameters are the width of the crop, the height, the starting position X, and the starting position Y.
epVideo.crop(480,360,0,0);
```
* Rotate and mirror
```Java
/ / The first parameter is the rotation angle, the second parameter is whether to mirror, only supports 90, 180, 270 degrees rotation
epVideo.rotation(90,true);
```
* Add text
```Java
//The parameters are the X, Y coordinates of the added position, the font size of the text (unit px), the text color, the path of the font file, the content, and the Time class is the start time and duration of the display.
epVideo.addText(10,10,35,"red",ttfPath,text);
epVideo.addText(new EpText(10,10,35,"red",ttfPath,text,new EpText.Time(3,5)));
```
* Add logo
```Java
/ / Add a picture class
/ / parameter is the image path, X, Y, the width, height of the picture, whether it is a motion picture (only supports png, jpg, gif picture, if it is a gif picture, the last parameter is true)
EpDraw epDraw = new EpDraw(filePath,10,10,50,50,false);
epVideo.addDraw(epDraw);
or
epVideo.addDraw(new EpDraw(filePath,10,10,50,50,false,3,5));//The last two parameters are the start time and duration of the display.
```
* Add custom filters
```Java
/ / Custom filter, ffmpeg command support filters are supported
/ / Detailed results can be found at: http://blog.csdn.net/u012027644/article/details/77833484
/ / See the ffmpeg filter official website for details: http://www.ffmpeg.org/ffmpeg-filters.html
//Example String filter = "lutyuv=y=maxval+minval-val:u=maxval+minval-val:v=maxval+minval-val";//film effect
epVideo.addFilter(filter);
```
* Process a single video
```Java
EpVideo epVideo = new EpVideo(url);
/ / Output options, the parameters are the output file path (currently only supports mp4 format output)
EpEditor.OutputOption outputOption = new EpEditor.OutputOption(outFile);
outputOption.width = 480; / / output video width, if not set, the original video width and height
outputOption.height = 360; / / output video height
outputOption.frameRate = 30; / / output video frame rate, the default 30
outputOption.bitRate = 10; / / output video bit rate, the default 10
EpEditor.exec(epVideo, outputOption, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {
/ / Get the progress here
}
});
```
* Add background music
```Java
/ / Parameters are video path, audio path, output path, original video volume (1 is 100%, 0.7 is 70%, and so on), add audio volume
EpEditor.music(videoPath, audioPath, outfilePath, 1, 0.7, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {
/ / Get the progress here
}
});
```
* Separate audio and video
```Java
/ / parameters are video path, output path, output type
EpEditor.demuxer(videoPath, outfilePath,EpEditor.Format.MP3, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {
/ / Get the progress here
}
});
```
* Video shifting
```Java
//Parameters are video path, output path, variable speed (only 0.25-4 times), variable speed type (VIDEO-video (when VIDEO is selected, audio will be shielded), AUDIO-audio, ALL-video audio simultaneous shifting)
EpEditor.changePTS(videoPath, outfilePath, 2.0f, EpEditor.PTS.ALL, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {

}
});
```
* Audio and video upside down
```Java
////The parameters are video path, output path, whether the video is inverted, and whether the audio is inverted. (If both are true, the audio and video are reversed. If the video ture audio is false, the output is inverted without audio video. Video false audio ture, input inverted audio, audio reversal also use this configuration)
EpEditor.reverse(videoPath, outfilePath, true, true, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {

}
});
```
* Video to picture
```Java
The //// parameters are the video path and the output path (the path is in the form of a collection, such as pic%03d.jpg, which supports jpg and png image formats), the width of the output image, the height of the output image, and the output image per second. Quantity (2 words per second, 0.5f is one every two seconds)
EpEditor.video2pic(videoPath, outfilePath, 720, 1080, 2, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {

}
});
```
* Picture to video
```Java
The //// parameters are the image collection path, the output path, the width of the output video, the height of the output video, and the frame rate of the output video.
EpEditor.pic2video(picPath, outfilePath, 480, 320, 30, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {

}
});
```
## Multiple Video Processing & Merging
* Merge video (supports additional processing of videos to be merged)
```Java
ArrayList<EpVideo> epVideos = new ArrayList<>();
epVideos.add(new EpVideo(url));//Video 1
epVideos.add(new EpVideo(url2));//Video 2
epVideos.add(new EpVideo(url3));//Video 3
/ / Output options, the parameters are the output file path (currently only supports mp4 format output)
EpEditor.OutputOption outputOption = new EpEditor.OutputOption(outFile);
outputOption.width = 480; / / output video width, the default 480
outputOption.height = 360; / / output video height, default 360
outputOption.frameRate = 30; / / output video frame rate, the default 30
outputOption.bitRate = 10; / / output video bit rate, the default 10
EpEditor.merge(epVideos, outputOption, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {
/ / Get the progress here
}
});
```
* Lossless combined video (strict video format, resolution, frame rate, and code rate are the same. It does not support other processing operations on the video to be merged. This method combines quickly. Another: two segments of the same format audio stitching You can also use this method)
```Java
ArrayList<EpVideo> epVideos = new ArrayList<>();
epVideos.add(new EpVideo(url));//Video 1
epVideos.add(new EpVideo(url2));//Video 2
epVideos.add(new EpVideo(url3));//Video 3
EpEditor.mergeByLc(epVideos, new EpEditor.OutputOption(outFile), new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {
/ / Get the progress here
}
});
```
## Custom Command
* Enter the ffmpeg command (you don't need to lose ffmpeg at the beginning, the example "-i input.mp4 -ss 0 -t 5 output.mp4", the second parameter is the video length, in microseconds, you can fill 0)
```Java
EpEditor.execCmd("", 0, new OnEditorListener() {
@Override
Public void onSuccess() {

}

@Override
Public void onFailure() {

}

@Override
Public void onProgress(float progress) {

}
});
```
