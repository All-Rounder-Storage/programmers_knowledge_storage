# 안드로이드의 apk 와 aab 를 알아보자

## 공부하게 된 이유
- 안드로이드 앱 파일로 앱 배포시 필수적으로 제작하지만 내부의 내용에 대해선 잘 알지 못해 알아보기 위함이다.

### APK 란 ? 
- Android Application Package 의 약자로 확장자가 .apk 로 끝나는 설치 파일 이며 zip 형식으로 압축 되어있음
- Android 플랫폼에서 어플리케이션 설치를 위해 배포할 수 있는 패키지 파일
- 구글 플레이스토어 업로드를 위해 설치 파일을 올려 심사를 받을 수 있었음
    - 구글 플레이 스토어에서 apk 로의 업로드 허용을 중단하고 **aab 파일**로의 업로드만 허용하여
      내부 배포 혹은 사내 서버 업로드가 아닌 이상 구글 플레이 스토어 업로드에는 이제 사용할 수 없음
      
### APK 파일이 생성되는 과정
1. 앱 프로젝트를 이루는 파일이 컴파일 된다. (.java / .kt / .xml 등)
2. 컴파일 완료 시 **.class 파일**이 생성된다.
3. .class 파일을 가상머신에서 인식이 가능한  **.dex 파일** 로 변환되고, **레이아웃 및 리소스(이미지, 아이콘 등)**를 포함한 파일이 생성된다.
3. zip 형태로 압축 되어 .apk 파일이 생성된다.

![APK](https://user-images.githubusercontent.com/49216939/182615846-25f30c8b-bb95-4559-8b38-185c059d2cf2.png)

#### .dex 파일 이란 ?
- 앱이 가상머신 위에서 구동되기 위한 바이너리 실행 파일, 이 파일을 토대로 앱이 구동되어진다.

- .dex 파일에서 .class 파일을 추출하면 어플리케이션의 소스코드를 볼 수 있다.(reverse engineering)
    - 난독화가 적용되어 있을 경우 볼 수 없는 코드도 존재한다.
### APK 파일 구조
`META-INF`
- Signing 과 관련된 정보가 담겨 있는 폴더
`assets`
- 앱 리소스가 저장되는 폴더
- 주로 동영상, 음악, HTML 파일 등 용량이 큰 파일 위주로 저장됨
`res`
- 앱 리소스가 저장되는 폴더
- assets 에 저장되는 파일에 비해 용량이 작은 파일이 저장됨
- 이미지, 아이콘, lottie 파일, layout 파일 등
- 내부 폴더로는 Drawable / Layout / Value 등이 존재함
    - Drawable : 이미지, 아이콘, 레이아웃 shape 파일 등
    - Layout : 개발자가 작성한 화면 파일(xml)
    - Values : 화면과 관련된 설정 파일 존재
        - dimens.xml : margin/padding 설정 값 파일
        - string.xml : 앱 내 활용한 텍스트 파일
        - styles.xml : 화면 디자인 관련 설정 파일(색상, 배경, 테마 등)
`lib`
- 앱에서 사용한 라이브러리 파일이 저장되는 폴더
`com`
- 컴파일이 왼료된 클래스 파일(*.class)이 존재
`AndroidManifest.xml`
- 앱의 정보가 기록된 파일
- 앱의 패키지명, 버전, 설정된 권한 등 이 기록되어있음
`*.dex`
- 가상 머신이 인식하여 앱을 구동 시킬 수 있는 파일
`resource.arsc`
- res 폴더에 있는 정보가 기록되어있음

#### 앱이 구동되는 과정
`자바/코틀린/xml(layout) 코드 -> 컴파일 -> .class 파일 생성 -> .dex 파일 생성 -> dex 참조하여 가상 머신에서 구동`

![apk build flow](https://user-images.githubusercontent.com/49216939/182616880-30fc1216-514c-4c58-93a7-a62c75fd1184.png)

##### 이미지 추가 설명
- Signing ? 
    - apk 제작 시 서명하는 절차로 개발자가 개발을 완료한 apk 라는 인증을 남기는 절차
    - 스토어 업로드 시에도 필요로 한다.
        - 최초 **업로드** 시에는 개발자가 직접 만든 키 이용
        - 이후 **업데이트** 시에는 스토어에서 제공해주는 앱 서명키를 이용하여 업데이트하고 심사를 요청할 수 있다.
- ADB ? 
    - Android Debug Bridge 의 약자로 앱 디버깅 / 설치 등의 과정에서 활용할 수 있음
    - 사진에서 나타난 의미로는 이용해서 기기에 설치하거나 에뮬레이터에 설치를 말한다.
    - 등록과 설정을 해두면 무선으로도 사용할 수 있다.

- emulator ?
    - 가상의 기기, 컴퓨터 상에서 안드로이드 핸드폰 인 것 처럼 작동
        - 안드로이드 스튜디오에서 설치 가능

### AAB 란 ?
- Android App Bundle 의 약자로 .aab 형식의 파일
- 2018년 구글에서 발표한 새로운 형식의 배포 파일

### DVM / ART ? 

### JIT / AOT

[reference](https://www.charlezz.com/?p=42686)
[reference](https://medium.com/@logishudson0218/android-apk%EC%9D%98-%EA%B5%AC%EC%84%B1%EA%B3%BC-%EC%83%88%EB%A1%9C%EC%9A%B4-app-bundle-%EB%B0%A9%EC%8B%9D-274b1a4cfb62)