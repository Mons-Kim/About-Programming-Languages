# Setup



## Requirements

- Android 4.1 (API level 16) or higher
- Java 7 or higher
- Android Gradle plugin 3.4.0 or higher



## Sendbird 설정

- Sendbird계정의 대시보드에서 프로젝트를 위한 Sendbird Application을 생성
- 프로젝트와 Sendbird서버를 연결하기 위해 해당 application에 할당된 App ID가 필요함



## 프로젝트 설정

1. repository 설정

   - Gradle 6.7 이하

     ```build.gradle(application)
     allprojects {
         repositories {
             maven { url "https://repo.sendbird.com/public/maven" }
         }
     }
     ```

   - Gradle 6.8 이상

     ```settings.gradle
     dependencyResolutionManagement {
         repositories {
             maven { url "https://repo.sendbird.com/public/maven" }
         }
     }
     ```

2. 라이브러리 연동

   ```build.gradle(module)
   dependencies {
       implementation 'com.sendbird.sdk:sendbird-android-sdk:3.1.7'
   }
   ```

3. Permission 설정

   ```AndroidManifest.xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```

4. (옵션) proguard 설정 

5. - "build.gradle(module)"에서 minifyEnabled true로 설정되어 있으면

   ```proguard-rules.pro
   -dontwarn com.sendbird.android.shadow.**
   ```

   
