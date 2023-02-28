


# FFmpegKit for Python

<img src="https://github.com/arthenica/ffmpeg-kit/blob/main/docs/assets/ffmpeg-kit-icon-v9.png" width="240">

`FFmpegKit` is a collection of tools to use `FFmpeg` in `Android`, `iOS`, `Linux`, `macOS`, `tvOS`, `Flutter` and `React Native` applications.

It includes scripts to build `FFmpeg` native libraries, a wrapper library to run `FFmpeg`/`FFprobe` commands in
 applications and 8 prebuilt binary packages available at [Github](https://github.com/arthenica/ffmpeg-kit/releases),
 [Maven Central](https://search.maven.org), [CocoaPods](https://cocoapods.org), [pub](https://pub.dev) and [npm](https://www.npmjs.com).

### 1. Features
- Scripts to build FFmpeg native libraries
- `FFmpegKit` wrapper library to run `FFmpeg`/`FFprobe` commands in applications
- Supports native platforms: Android, iOS, Linux, macOS and tvOS
- Supports hybrid platforms: Flutter, React Native
- Based on FFmpeg `v4.5-dev` or later with optional system and external libraries
- 8 prebuilt binary packages available at [Github](https://github.com/arthenica/ffmpeg-kit/releases), [Maven Central](https://search.maven.org), [CocoaPods](https://cocoapods.org), [pub](https://pub.dev) and [npm](https://www.npmjs.com)
- Licensed under `LGPL 3.0` by default, `GPL v3.0` if GPL licensed libraries are enabled

## 2. Android (+Python)

### 1. Features
- Supports `API Level 24+` on Main releases and `API Level 16+` on LTS releases
- Includes `arm-v7a`, `arm-v7a-neon`, `arm64-v8a`, `x86` and `x86_64` architectures
- Can handle Storage Access Framework (SAF) Uris
- Camera access on [supported devices](https://developer.android.com/ndk/guides/stable_apis#camera)
- Builds shared native libraries (.so)
- Creates Android archive with .aar extension

### 2. BUILDING (Is after `Using with Python`)                                                         
### USE PREBUILT PACKAGE FROM RELEASES SECTION      

### 3. Using With Python


##### 0)   Add SmartExpetion Common and SmartExeption java from (https://github.com/tanersener/smart-exception/releases/tag/v0.2.1) 
##### 1)   and the aar from Releases tab or the one generated by you to your projects folder.    
##### 2)   In Buildozer spec file add jnius to requirements and the aar file. (Alongside what you already have)
```
requirements = jnius
android.add_aars = ffmpeg-kit-release.aar
android.add_jars = smart-exception-common-0.2.1.jar, smart-exception-java-0.2.1.jar
``` 
##### 3)   Using Pyjnius, declare the variables in Python (https://pyjnius.readthedocs.io/en/stable/)
```
#IMPORTING jnius
from jnius import autoclass
from jnius import * 
#Declaring Variable so it can be used
FFMPEG = autoclass('com.sahib.pyff.ffpy')
```

BOTH (FFMPEG and FFPROBE) RETURN OUTPUT OF THE COMMAND         
##### 4) To Use FFMPEG 
```
#EXECUTED FFMPEG COMMAND, COMMAND IS STRING
ffmpegCommand = FFMPEG.Run("COMMAND")

#PRINTS RETURN (OUTPUT OF THE COMMAND)
print(ffmpegCommand)
```

##### 5) To use FFProbe
```
#EXECUTED FFProbe COMMAND, COMMAND IS STRING
probeCommand = FFMPEG.RunProbe("Command")

#PRINTS RETURN (OUTPUT OF THE COMMAND)
print(probeCommand)

```
NOTE - FILTER_COMPLEX can not be used. (IDK why, if you have solution tell me please)      



#### EXAMPLES      
##### 1) FFMPEG     
```
from jnius import autoclass
from jnius import * 
#Declaring Variable so it can be used
FFMPEG = autoclass('com.sahib.pyff.ffpy')


#THIS COVERTS VIDEO INTO an audio file (MP4 TO WAV)
d = FFMPEG.Run(str("-i video.mp4 -ab 160k -ac 2 -ar 44100 -vn TEMP/audio.wav"))

#THIS PRINTS THE OUTPUT (I don't think you even need output in ffmpeg unless for trouble shooting)
print(d) 

```


##### 2) FFProbe
```
from jnius import autoclass
from jnius import * 
#Declaring Variable so it can be used
FFMPEG = autoclass('com.sahib.pyff.ffpy')


#Gets Framerate of video
frameRate = FFMPEG.RunProbe("-v error -select_streams v -of default=noprint_wrappers=1:nokey=1 -show_entries stream=r_frame_rate video.mp4")
#This command outputs 30/1 at 30 frames

frameRate = frameRate.split("/")[0]
#this makes it 30 instead of 30/1

frameRate = int(frameRate)
print(frameRate)
#Converts framerate to integer and prints it

```    





### 2. Building (Skip this part and use prebuilt from RELEASES section)

Run `android.sh` at project root directory to build `ffmpeg-kit` and `ffmpeg` shared libraries. 

Please note that `FFmpegKit` project repository includes the source code of `FFmpegKit` only. `android.sh` needs 
network connectivity and internet access to `github.com` in order to download the source code of `FFmpeg` and 
external libraries enabled.

#### 2.1 Prerequisites

`android.sh` requires the following tools and packages.

##### 2.1.1 Android Tools
   - Android SDK Build Tools
   - Android NDK r22b or later with LLDB and CMake (See [#292](https://github.com/arthenica/ffmpeg-kit/issues/292) if you want to use NDK r23b)

##### 2.1.2 Packages

Use your package manager (apt, yum, dnf, brew, etc.) to install the following packages.

```
autoconf automake libtool pkg-config curl cmake gcc gperf texinfo yasm nasm bison autogen git wget autopoint meson ninja
```

##### 2.1.3 Environment Variables 

Set `ANDROID_SDK_ROOT` and `ANDROID_NDK_ROOT` environment variables before running `android.sh`.

```
export ANDROID_SDK_ROOT=<Android SDK Path>
export ANDROID_NDK_ROOT=<Android NDK Path>
```

#### 2.2 Options

Use `--enable-<library name>` flag to support additional external or system libraries and
`--disable-<architecture name>` to disable architectures you don't want to build.

```
./android.sh --enable-fontconfig --disable-arm-v7a-neon
```

Run `--help` to see all available build options.

#### 2.3 LTS Binaries

Use `--lts` option to build lts binaries for each architecture.

#### 2.4 Build Output

All libraries created by `android.sh` can be found under the `prebuilt` directory.

- `Android` archive (.aar file) for `Main` builds is located under the `bundle-android-aar` folder.
- LTS is not supported sorry. (For Python)

