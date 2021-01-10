---
layout: post
title: '[TIL] macOS에 Flutter 개발환경구축'
subtitle: 
categories: til
tags: til flutter
comments: true
date: 2021-01-10 23:12:17 +0900
lastmod: 2021-01-11 13:00:17 +0900
sitemap:
  changefreq: daily
  priority: 1
published: true
---

macOS에 Flutter 개발환경구축<br />



# macOS에 Flutter 설치

강의 번호: flutter
비고: https://youtu.be/3JMjR_ahT44
완료여부: Yes
유형: 스터디
작성일시: 2021년 1월 3일 오후 11:47
최종 편집일시: 2021년 1월 11일 오전 1:03

## 1. Flutter 설치

Flutter 다운로드

```basic
https://flutter.dev/docs/get-started/install/macos
```

다운로드 파일 압축 해제 후 해당 위치에 옮기기

```basic
/Users/kang/Developer
```

환경변수 설정(macOS 카탈리나의 경우입니다.)
터미널에서 환경 변수 추가 후 시스템 적용

```basic
$ vim .zshrc
```

```basic
i

export PATH="$PATH:/Users/kang/Developer/flutter/bin"

:wq!
```

```basic
$ source .zshrc
```

flutter 버전 확인

```basic
$ flutter -version
Downloading Dart SDK from Flutter engine ae90085a8437c0ae94d6b5ad2741739ebc742cb4...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  172M  100  172M    0     0  1170k      0  0:02:30  0:02:30 --:--:-- 1675k
Building flutter tool...
Could not find an option with short name "-e".

Run 'flutter -h' (or 'flutter <command> -h') for available flutter commands and options.
$ flutter --version
Flutter 1.22.5 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 7891006299 (4 weeks ago) • 2020-12-10 11:54:40 -0800
Engine • revision ae90085a84
Tools • Dart 2.10.4

  ╔════════════════════════════════════════════════════════════════════════════╗
  ║                 Welcome to Flutter! - https://flutter.dev                  ║
  ║                                                                            ║
  ║ The Flutter tool uses Google Analytics to anonymously report feature usage ║
  ║ statistics and basic crash reports. This data is used to help improve      ║
  ║ Flutter tools over time.                                                   ║
  ║                                                                            ║
  ║ Flutter tool analytics are not sent on the very first run. To disable      ║
  ║ reporting, type 'flutter config --no-analytics'. To display the current    ║
  ║ setting, type 'flutter config'. If you opt out of analytics, an opt-out    ║
  ║ event will be sent, and then no further information will be sent by the    ║
  ║ Flutter tool.                                                              ║
  ║                                                                            ║
  ║ By downloading the Flutter SDK, you agree to the Google Terms of Service.  ║
  ║ Note: The Google Privacy Policy describes how data is handled in this      ║
  ║ service.                                                                   ║
  ║                                                                            ║
  ║ Moreover, Flutter includes the Dart SDK, which may send usage metrics and  ║
  ║ crash reports to Google.                                                   ║
  ║                                                                            ║
  ║ Read about data we send with crash reports:                                ║
  ║ https://flutter.dev/docs/reference/crash-reporting                         ║
  ║                                                                            ║
  ║ See Google's privacy policy:                                               ║
  ║ https://policies.google.com/privacy                                        ║
  ╚════════════════════════════════════════════════════════════════════════════╝
```

flutter 설치 체크

```basic
$ flutter doctor

Downloading Material fonts...                                       1.0s
Downloading Gradle Wrapper...                                       0.1s
Downloading package sky_engine...                                   0.4s
Downloading flutter_patched_sdk tools...                            4.0s
Downloading flutter_patched_sdk_product tools...                    4.0s
Downloading darwin-x64 tools...                                    11.2s
Downloading libimobiledevice...                                     0.2s
Downloading usbmuxd...                                              0.2s
Downloading libplist...                                             0.2s
Downloading openssl...                                              0.5s
Downloading ios-deploy...                                           0.2s
Downloading darwin-x64/font-subset tools...                         0.8s
Downloading android-arm-profile/darwin-x64 tools...                 1.0s
Downloading android-arm-release/darwin-x64 tools...                 0.9s
Downloading android-arm64-profile/darwin-x64 tools...               1.1s
Downloading android-arm64-release/darwin-x64 tools...               0.9s
Downloading android-x64-profile/darwin-x64 tools...                 1.2s
Downloading android-x64-release/darwin-x64 tools...                 0.9s
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
[✗] Android toolchain - develop for Android devices
    ✗ Unable to locate Android SDK.
      Install Android Studio from: https://developer.android.com/studio/index.html
      On first launch it will assist you in installing the Android SDK components.
      (or visit https://flutter.dev/docs/get-started/install/macos#android-setup for
      detailed instructions).
      If the Android SDK has been installed to a custom location, set ANDROID_SDK_ROOT to
      that location.
      You may also want to add it to your PATH environment variable.

[✗] Xcode - develop for iOS and macOS
    ✗ Xcode installation is incomplete; a full installation is necessary for iOS
      development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
        sudo xcodebuild -runFirstLaunch
    ✗ CocoaPods installed but not working.
        You appear to have CocoaPods installed but it is not working.
        This can happen if the version of Ruby that CocoaPods was installed with is
        different from the one being used to invoke it.
        This can usually be fixed by re-installing CocoaPods. For more info, see
        https://github.com/flutter/flutter/issues/14293.
      To re-install CocoaPods, run:
        sudo gem install cocoapods
[!] Android Studio (not installed)
[!] IntelliJ IDEA Ultimate Edition (version 2019.3.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.52.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 6 categories.
➜  ~
```

## 2. Android 개발환경 구축

안드로이드 다운로드 후 설치

```basic
https://flutter.dev/docs/get-started/install/macos#android-setup
```

안드로이드 스튜디오 실행 

```basic
- Do not import settings > OK
- Next
- Standard > Next
- Darcula > Next
- 자동으로 컴포넌트 다운로드 및 설치
- Finish

추가 관련 파일 다운로드 됨
```

```basic
Preparing "Install SDK Patch Applier v4 (revision: 1)".
Downloading https://dl.google.com/android/repository/3534162-studio.sdk-patcher.zip
"Install SDK Patch Applier v4 (revision: 1)" ready.
Installing SDK Patch Applier v4 in /Users/kang/Library/Android/sdk/patcher/v4
"Install SDK Patch Applier v4 (revision: 1)" complete.
"Install SDK Patch Applier v4 (revision: 1)" finished.
Preparing "Install Android SDK Platform-Tools (revision: 30.0.5)".
Downloading https://dl.google.com/android/repository/eabcd8b4b7ab518c6af9c941af8494072f17ec4b.platform-tools_r30.0.5-darwin.zip
"Install Android SDK Platform-Tools (revision: 30.0.5)" ready.
Installing Android SDK Platform-Tools in /Users/kang/Library/Android/sdk/platform-tools
"Install Android SDK Platform-Tools (revision: 30.0.5)" complete.
"Install Android SDK Platform-Tools (revision: 30.0.5)" finished.
Preparing "Install Android Emulator (revision: 30.3.5)".
Downloading https://dl.google.com/android/repository/emulator-darwin-7033400.zip
"Install Android Emulator (revision: 30.3.5)" ready.
Installing Android Emulator in /Users/kang/Library/Android/sdk/emulator
"Install Android Emulator (revision: 30.3.5)" complete.
"Install Android Emulator (revision: 30.3.5)" finished.
Preparing "Install Android SDK Tools (revision: 26.1.1)".
Downloading https://dl.google.com/android/repository/sdk-tools-darwin-4333796.zip
"Install Android SDK Tools (revision: 26.1.1)" ready.
Installing Android SDK Tools in /Users/kang/Library/Android/sdk/tools
"Install Android SDK Tools (revision: 26.1.1)" complete.
"Install Android SDK Tools (revision: 26.1.1)" finished.
Preparing "Install Sources for Android 30 (revision: 1)".
Downloading https://dl.google.com/android/repository/sources-30_r01.zip
"Install Sources for Android 30 (revision: 1)" ready.
Installing Sources for Android 30 in /Users/kang/Library/Android/sdk/sources/android-30
"Install Sources for Android 30 (revision: 1)" complete.
"Install Sources for Android 30 (revision: 1)" finished.
Preparing "Install Android SDK Build-Tools 30.0.3 (revision: 30.0.3)".
Downloading https://dl.google.com/android/repository/f6d24b187cc6bd534c6c37604205171784ac5621.build-tools_r30.0.3-macosx.zip
"Install Android SDK Build-Tools 30.0.3 (revision: 30.0.3)" ready.
Installing Android SDK Build-Tools 30.0.3 in /Users/kang/Library/Android/sdk/build-tools/30.0.3
"Install Android SDK Build-Tools 30.0.3 (revision: 30.0.3)" complete.
"Install Android SDK Build-Tools 30.0.3 (revision: 30.0.3)" finished.
Preparing "Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)".
Downloading https://dl.google.com/android/repository/extras/intel/haxm-macosx_v7_5_1.zip
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" ready.
Installing Intel x86 Emulator Accelerator (HAXM installer) in /Users/kang/Library/Android/sdk/extras/intel/Hardware_Accelerated_Execution_Manager
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" complete.
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" finished.
Preparing "Install Google APIs Intel x86 Atom System Image (revision: 9)".
Downloading https://dl.google.com/android/repository/sys-img/google_apis/x86-30_r09.zip
"Install Google APIs Intel x86 Atom System Image (revision: 9)" ready.
Installing Google APIs Intel x86 Atom System Image in /Users/kang/Library/Android/sdk/system-images/android-30/google_apis/x86
"Install Google APIs Intel x86 Atom System Image (revision: 9)" complete.
"Install Google APIs Intel x86 Atom System Image (revision: 9)" finished.
Preparing "Install Android SDK Platform 30 (revision: 3)".
Downloading https://dl.google.com/android/repository/platform-30_r03.zip
"Install Android SDK Platform 30 (revision: 3)" ready.
Installing Android SDK Platform 30 in /Users/kang/Library/Android/sdk/platforms/android-30
"Install Android SDK Platform 30 (revision: 3)" complete.
"Install Android SDK Platform 30 (revision: 3)" finished.
Parsing /Users/kang/Library/Android/sdk/build-tools/30.0.3/package.xml
Parsing /Users/kang/Library/Android/sdk/emulator/package.xml
Parsing /Users/kang/Library/Android/sdk/extras/intel/Hardware_Accelerated_Execution_Manager/package.xml
Parsing /Users/kang/Library/Android/sdk/patcher/v4/package.xml
Parsing /Users/kang/Library/Android/sdk/platform-tools/package.xml
Parsing /Users/kang/Library/Android/sdk/platforms/android-30/package.xml
Parsing /Users/kang/Library/Android/sdk/sources/android-30/package.xml
Parsing /Users/kang/Library/Android/sdk/system-images/android-30/google_apis/x86/package.xml
Parsing /Users/kang/Library/Android/sdk/tools/package.xml
Android SDK is up to date.
Running Intel® HAXM installer
Silent installation Pass! 
Creating Android virtual device
Android virtual device Pixel_3a_API_30_x86 was successfully created
```

안드로이드 스튜디오 앱 웰컴 첫 화면에서

```basic
Configure > Preferences > Plugins
Flutter 검색 후 Install > Accept > Install > Restart IDE > Restart
```

(추가) 버전 문제 관련 버그 픽스

```basic
$ ln -s ~/Library/Application\ Support/Google/AndroidStudio4.1/plugins ~/Library/Application\ Support/AndroidStudio4.1

(참고)
https://stackoverflow.com/questions/51860845/flutter-plugin-not-installed-error-when-running-flutter-doctor
```

안드로이드 툴체인 해결 관련

```basic
$  ~ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
[!] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
    ✗ Android licenses not accepted.  To resolve this, run: flutter doctor
      --android-licenses
```

```basic
$ flutter doctor --android-licenses

Warning: File /Users/kang/.android/repositories.cfg could not be loaded.        
7 of 7 SDK package licenses not accepted. 100% Computing updates...             
Review licenses that have not been accepted (y/N)?  y

1/7: License android-googletv-license:
---------------------------------------

Accept? (y/N): y

2/7: License android-sdk-arm-dbt-license:
---------------------------------------
Accept? (y/N): y

3/7: License android-sdk-license:
---------------------------------------
Accept? (y/N): y

4/7: License android-sdk-preview-license:
---------------------------------------
Accept? (y/N): y

5/7: License google-gdk-license:
---------------------------------------
Accept? (y/N): y

6/7: License intel-android-extra-license:
---------------------------------------
Accept? (y/N): y

7/7: License mips-android-sysimage-license:
---------------------------------------
Accept? (y/N): y
All SDK package licenses accepted
```

## 3. iOS 개발환경 구축

### Xcode 설치

xcode가 설치 안되어 있는거 확인

```basic
$ flutter doctor

Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
 
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[✗] Xcode - develop for iOS and macOS
    ✗ Xcode installation is incomplete; a full installation is necessary for iOS
      development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
        sudo xcodebuild -runFirstLaunch
    ✗ CocoaPods installed but not working.
        You appear to have CocoaPods installed but it is not working.
        This can happen if the version of Ruby that CocoaPods was installed with is
        different from the one being used to invoke it.
        This can usually be fixed by re-installing CocoaPods. For more info, see
        https://github.com/flutter/flutter/issues/14293.
      To re-install CocoaPods, run:
        sudo gem install cocoapods
[✓] Android Studio (version 4.1)
[!] IntelliJ IDEA Ultimate Edition (version 2019.3.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.52.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 4 categories.
➜  ~
```

xcode iTunse에서 다운로드

```basic
https://itunes.apple.com/us/app/xcode/id497799835
```

xcode 앱 한번 실행 후 터미널에서 아래 코드 실행

```basic
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
Password: {시스템 비밀번호}
$ sudo xcodebuild -runFirstLaunch
```

flutter doctor에서 다시 확인

```basic
$ flutter doctor                                                       

Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[!] Xcode - develop for iOS and macOS (Xcode 12.3)
    ✗ CocoaPods installed but not working.
        You appear to have CocoaPods installed but it is not working.
        This can happen if the version of Ruby that CocoaPods was installed with is
        different from the one being used to invoke it.
        This can usually be fixed by re-installing CocoaPods. For more info, see
        https://github.com/flutter/flutter/issues/14293.
      To re-install CocoaPods, run:
        sudo gem install cocoapods
[✓] Android Studio (version 4.1)
[!] IntelliJ IDEA Ultimate Edition (version 2019.3.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.52.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 4 categories.
➜  ~
```

CocoaPods installed but not working. 관련

```basic
$ sudo gem install cocoapods

Password:{시스템 비밀번호}
Fetching thread_safe-0.3.6.gem
Fetching tzinfo-1.2.9.gem
Fetching activesupport-5.2.4.4.gem
Fetching nap-1.1.0.gem
Fetching concurrent-ruby-1.1.7.gem
Fetching i18n-1.8.7.gem
Fetching fuzzy_match-2.0.4.gem
Fetching httpclient-2.8.3.gem
Fetching addressable-2.7.0.gem
Fetching algoliasearch-1.27.5.gem
Fetching ffi-1.14.2.gem
Fetching ethon-0.12.0.gem
Fetching typhoeus-1.4.0.gem
Fetching netrc-0.11.0.gem
Fetching public_suffix-4.0.6.gem
Fetching cocoapods-core-1.10.1.gem
Fetching claide-1.0.3.gem
Fetching cocoapods-deintegrate-1.0.4.gem
Fetching cocoapods-downloader-1.4.0.gem
Fetching cocoapods-plugins-1.0.0.gem
Fetching cocoapods-search-1.0.0.gem
Fetching cocoapods-trunk-1.5.0.gem
Fetching cocoapods-try-1.2.0.gem
Fetching molinillo-0.6.6.gem
Fetching atomos-0.1.3.gem
Fetching CFPropertyList-3.0.3.gem
Fetching colored2-3.1.2.gem
Fetching nanaimo-0.3.0.gem
Fetching xcodeproj-1.19.0.gem
Fetching escape-0.0.4.gem
Fetching fourflusher-2.3.1.gem
Fetching gh_inspector-1.1.3.gem
Fetching ruby-macho-1.4.0.gem
Fetching cocoapods-1.10.1.gem
Successfully installed concurrent-ruby-1.1.7

HEADS UP! i18n 1.1 changed fallbacks to exclude default locale.
But that may break your application.

If you are upgrading your Rails application from an older version of Rails:

Please check your Rails app for 'config.i18n.fallbacks = true'.
If you're using I18n (>= 1.1.0) and Rails (< 5.2.2), this should be
'config.i18n.fallbacks = [I18n.default_locale]'.
If not, fallbacks will be broken in your app by I18n 1.1.x.

If you are starting a NEW Rails application, you can ignore this notice.

For more info see:
https://github.com/svenfuchs/i18n/releases/tag/v1.1.0

Successfully installed i18n-1.8.7
Successfully installed thread_safe-0.3.6
Successfully installed tzinfo-1.2.9
Successfully installed activesupport-5.2.4.4
Successfully installed nap-1.1.0
Successfully installed fuzzy_match-2.0.4
Successfully installed httpclient-2.8.3
A new major version is available for Algolia! Please now use the https://rubygems.org/gems/algolia gem to get the latest features.
Successfully installed algoliasearch-1.27.5
Building native extensions. This could take a while...
Successfully installed ffi-1.14.2
Successfully installed ethon-0.12.0
Successfully installed typhoeus-1.4.0
Successfully installed netrc-0.11.0
Successfully installed public_suffix-4.0.6
Successfully installed addressable-2.7.0
Successfully installed cocoapods-core-1.10.1
Successfully installed claide-1.0.3
Successfully installed cocoapods-deintegrate-1.0.4
Successfully installed cocoapods-downloader-1.4.0
Successfully installed cocoapods-plugins-1.0.0
Successfully installed cocoapods-search-1.0.0
Successfully installed cocoapods-trunk-1.5.0
Successfully installed cocoapods-try-1.2.0
Successfully installed molinillo-0.6.6
Successfully installed atomos-0.1.3
Successfully installed CFPropertyList-3.0.3
Successfully installed colored2-3.1.2
Successfully installed nanaimo-0.3.0
Successfully installed xcodeproj-1.19.0
Successfully installed escape-0.0.4
Successfully installed fourflusher-2.3.1
Successfully installed gh_inspector-1.1.3
Successfully installed ruby-macho-1.4.0
Successfully installed cocoapods-1.10.1
Parsing documentation for concurrent-ruby-1.1.7
Installing ri documentation for concurrent-ruby-1.1.7
Parsing documentation for i18n-1.8.7
Installing ri documentation for i18n-1.8.7
Parsing documentation for thread_safe-0.3.6
Installing ri documentation for thread_safe-0.3.6
Parsing documentation for tzinfo-1.2.9
Installing ri documentation for tzinfo-1.2.9
Parsing documentation for activesupport-5.2.4.4
Installing ri documentation for activesupport-5.2.4.4
Parsing documentation for nap-1.1.0
Installing ri documentation for nap-1.1.0
Parsing documentation for fuzzy_match-2.0.4
Installing ri documentation for fuzzy_match-2.0.4
Parsing documentation for httpclient-2.8.3
Installing ri documentation for httpclient-2.8.3
Parsing documentation for algoliasearch-1.27.5
Installing ri documentation for algoliasearch-1.27.5
Parsing documentation for ffi-1.14.2
Installing ri documentation for ffi-1.14.2
Parsing documentation for ethon-0.12.0
Installing ri documentation for ethon-0.12.0
Parsing documentation for typhoeus-1.4.0
Installing ri documentation for typhoeus-1.4.0
Parsing documentation for netrc-0.11.0
Installing ri documentation for netrc-0.11.0
Parsing documentation for public_suffix-4.0.6
Installing ri documentation for public_suffix-4.0.6
Parsing documentation for addressable-2.7.0
Installing ri documentation for addressable-2.7.0
Parsing documentation for cocoapods-core-1.10.1
Installing ri documentation for cocoapods-core-1.10.1
Parsing documentation for claide-1.0.3
Installing ri documentation for claide-1.0.3
Parsing documentation for cocoapods-deintegrate-1.0.4
Installing ri documentation for cocoapods-deintegrate-1.0.4
Parsing documentation for cocoapods-downloader-1.4.0
Installing ri documentation for cocoapods-downloader-1.4.0
Parsing documentation for cocoapods-plugins-1.0.0
Installing ri documentation for cocoapods-plugins-1.0.0
Parsing documentation for cocoapods-search-1.0.0
Installing ri documentation for cocoapods-search-1.0.0
Parsing documentation for cocoapods-trunk-1.5.0
Installing ri documentation for cocoapods-trunk-1.5.0
Parsing documentation for cocoapods-try-1.2.0
Installing ri documentation for cocoapods-try-1.2.0
Parsing documentation for molinillo-0.6.6
Installing ri documentation for molinillo-0.6.6
Parsing documentation for atomos-0.1.3
Installing ri documentation for atomos-0.1.3
Parsing documentation for CFPropertyList-3.0.3
Installing ri documentation for CFPropertyList-3.0.3
Parsing documentation for colored2-3.1.2
Installing ri documentation for colored2-3.1.2
Parsing documentation for nanaimo-0.3.0
Installing ri documentation for nanaimo-0.3.0
Parsing documentation for xcodeproj-1.19.0
Installing ri documentation for xcodeproj-1.19.0
Parsing documentation for escape-0.0.4
Installing ri documentation for escape-0.0.4
Parsing documentation for fourflusher-2.3.1
Installing ri documentation for fourflusher-2.3.1
Parsing documentation for gh_inspector-1.1.3
Installing ri documentation for gh_inspector-1.1.3
Parsing documentation for ruby-macho-1.4.0
Installing ri documentation for ruby-macho-1.4.0
Parsing documentation for cocoapods-1.10.1
Installing ri documentation for cocoapods-1.10.1
Done installing documentation for concurrent-ruby, i18n, thread_safe, tzinfo, activesupport, nap, fuzzy_match, httpclient, algoliasearch, ffi, ethon, typhoeus, netrc, public_suffix, addressable, cocoapods-core, claide, cocoapods-deintegrate, cocoapods-downloader, cocoapods-plugins, cocoapods-search, cocoapods-trunk, cocoapods-try, molinillo, atomos, CFPropertyList, colored2, nanaimo, xcodeproj, escape, fourflusher, gh_inspector, ruby-macho, cocoapods after 35 seconds
34 gems installed
➜  ~
```

flutter doctor 다시 확인

```basic
$ flutter doctor  
               
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[✓] Xcode - develop for iOS and macOS (Xcode 12.3)
[✓] Android Studio (version 4.1)
[!] IntelliJ IDEA Ultimate Edition (version 2019.3.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.52.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
 
[!] Connected device                          
    ! No devices available

! Doctor found issues in 3 categories.
➜  ~
```

Xcode 라이센스 관련

```basic
$ sudo xcodebuild -license

~~~~
스페이스 쭈욱 누르다가 agree 누르면 됨
By typing 'agree' you are agreeing to the terms of the software license agreements. Type 'print' to print them or anything else to cancel, [agree, print, cancel]   {agree}

You can view the license agreements in Xcode's About Box, or at /Applications/Xcode.app/Contents/Resources/English.lproj/License.rtf
```

시뮬레이터 실행 테스트

```basic
$ open -a Simulator
```

### iOS 기기 배포 관련 추가사항

CocoaPods 설치

```basic
$ sudo gem install cocoapods

Password: {시스템 비밀번호}
Successfully installed cocoapods-1.10.1
Parsing documentation for cocoapods-1.10.1
Done installing documentation for cocoapods after 1 seconds
1 gem installed
➜  ~
```

안드로이드 스튜디오에서 만들었던 프로젝트 내 폴더에 pod 설치

```basic
$ cd Documents/AndroidStudioProjects/testing_flutter/ios
$ pod init
$ ios pod install

Analyzing dependencies
Downloading dependencies
Generating Pods project
Integrating client project
Pod installation complete! There are 0 dependencies from the Podfile and 0 total pods installed.

[!] The Podfile does not contain any dependencies.

[!] Automatically assigning platform `iOS` with version `9.0` on target `Runner` because no platform was specified. Please specify a platform for this target in your Podfile. See `https://guides.cocoapods.org/syntax/podfile.html#platform`.

[!] CocoaPods did not set the base configuration of your project because your project already has a custom config set. In order for CocoaPods integration to work at all, please either set the base configurations of the target `Runner` to `Target Support Files/Pods-Runner/Pods-Runner.debug.xcconfig` or include the `Target Support Files/Pods-Runner/Pods-Runner.debug.xcconfig` in your build configuration (`Flutter/Debug.xcconfig`).

[!] CocoaPods did not set the base configuration of your project because your project already has a custom config set. In order for CocoaPods integration to work at all, please either set the base configurations of the target `Runner` to `Target Support Files/Pods-Runner/Pods-Runner.release.xcconfig` or include the `Target Support Files/Pods-Runner/Pods-Runner.release.xcconfig` in your build configuration (`Flutter/Release.xcconfig`).

[!] CocoaPods did not set the base configuration of your project because your project already has a custom config set. In order for CocoaPods integration to work at all, please either set the base configurations of the target `Runner` to `Target Support Files/Pods-Runner/Pods-Runner.profile.xcconfig` or include the `Target Support Files/Pods-Runner/Pods-Runner.profile.xcconfig` in your build configuration (`Flutter/Release.xcconfig`).
➜  ios
```

## InterlliJ IDE에 Flutter 설치

인텔리제이 앱 웰컴 첫 화면에서

```basic
Configure > Preferences > Plugins
Flutter 검색 후 Install > Restart IDE > Restart
```

flutter doctor에서 확인

```basic
$ flutter doctor

Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 1.22.5, on Mac OS X 10.15.6 19G73 darwin-x64, locale en-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[✓] Xcode - develop for iOS and macOS (Xcode 12.3)
[✓] Android Studio (version 4.1)
[✓] IntelliJ IDEA Ultimate Edition (version 2019.3.5)
[!] VS Code (version 1.52.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
 
[!] Connected device                          
    ! No devices available

! Doctor found issues in 2 categories.
➜  ~
```

## 개발환경 구축 완료 :)

## 참고

- 유튜브 강의 : [https://youtu.be/3JMjR_ahT44]
<br/>
개발자님들 덕분에 많이 배울 수 있었습니다. 감사의 말씀 드립니다.<br/>