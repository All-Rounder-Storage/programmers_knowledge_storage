# 안드로이드의 apk 와 aab 를 알아보자

## 공부하게 된 이유
- 안드로이드 앱 파일로 앱 배포시 필수적으로 제작하지만 내부의 내용에 대해선 잘 알지 못해 알아보기 위함이다.

### APK 란 ? 
- Android Application Package 의 약자로 확장자가 .apk 로 끝나는 설치 파일 이며 zip 형식으로 압축 되어있음
- Android 플랫폼에서 어플리케이션 설치를 위해 배포할 수 있는 패키지 파일
- 구글 플레이스토어 업로드를 위해 설치 파일을 올려 심사를 받을 수 있었음
    - 최근에는 구글 플레이 스토어에서 apk 로의 업로드 허용을 중단하고 **aab 파일**로의 업로드만 허용함
        - 따라서 APK 업로드는 **내부 배포 혹은 사내 서버 업로드** 가 아닌 이상 구글 플레이 스토어 업로드에는 이제 사용할 수 없음
      
### APK 파일이 생성되는 과정
1. 앱 프로젝트를 이루는 파일이 컴파일 된다. (.java / .kt / .xml 등)
2. 컴파일 완료 시 **.class 파일**이 생성된다.
3. .class 파일을 가상머신에서 인식이 가능한  **.dex 파일** 로 변환되고, **레이아웃 및 리소스(이미지, 아이콘 등)** 를 포함한 파일이 생성된다.
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
- **Android App Bundle** 의 약자로 .aab 형식의 파일
- 2018년 구글에서 발표한 새로운 형식의 배포 파일로 구글이 제공하는 Dynamic Delivery 를 지원 받을 수 있다.
- 하나의 통 apk 가 아닌 모듈 식으로 분리된 apk 가 여러개 생성, 사용자의 설정에 따라 조각조각 나뉜 apk 가 모여 하나의 앱을 구성
- AAB 파일로 기기에 직접 설치는 불가능하다.

`근데 APK 도 있는데 왜 AAB 가 등장했나요 ?`
- 안드로이드가 점차 발전하면서 초창기와는 달리 할 수 있는 것들이 많이 늘어났다.
    - EX. 다국어 지원, 고화질 이미지 혹은 영상 제공, 다양한 사이즈 단말 대응
    - 이를 처리하다보면 APK 의 사이즈가 커지고, 사이즈가 커지면 사용자는 설치하기가 부담스럽다.
    - 이러한 점을 해결하기 위해 **구글에서 새롭게 제시하는 배포 방식인 AAB** 가 나오게 되었다.

`Dynamic Delivery 는 무엇인가요 ?`
- 사용자의 디바이스 설정에 따라 최적화된 APK 를 제공해 설치/실행 해주는 기술
- 사용자는 aab 파일로 스토어에서 설치 시 앱 실행에 필요한 코드와 리소스만을 다운로드 합니다.(앱 사이즈 ↓)
- 다양한 장치 지원을 위해서 장치별로 apk 를 빌드/서명/관리 할 필요없이 알아서 사용자에게 **최적화된 다운로드**가 가능합니다.

### AAB 파일 구조
![AAB](https://user-images.githubusercontent.com/49216939/182872285-9a3da2ba-a0fc-4c66-859d-ad0362ae68cf.png)

`base/ , feature1/, feature2/`
- 각각의 모듈을 대표하는 최상위 폴더
`BUNDLE-METADATA/`
- 플레이 스토어, signing, dex files, proguard mappings 등의 정보를 담고 있는 메타 파일들의 집합

`Module Protocol Buffer(*.pb) files`
- 각각의 모듈 정보에 대한 메타데이터를 플레이 스토어에 제공하는 파일

`manifest/`
- 각 모듈의 AndroidManifest.xml 파일을 담아두는 폴더

`dex/` 
- 각 모듈의 dex files 를 담아두는 디렉터리

`res/, lib/, assets/`
- 기존 APK 방식과 동일한 구조로 되어 있는 폴더
- App Bundle 을 업로드할 때, 플레이 스토어에서 설치될 디바이스 설정에 맞는 파일/패키지/디렉터리를 검사

`root/` 
- APK 의 루트 파일이 담기는 폴더

### 안드로이드의 가상 머신 ?
- 안드로이드는 초창기에 JVM 대신 **DVM**을 사용했습니다.
- 안드로이드 발전에 따라 DVM 에서 **ART** 로 바뀌었습니다.
    - ART 는 엄연히 따지면 VM 은 아니지만 VM 에서 완전히 벗어날 수 있는 형태도 아님

`왜 JVM 을 사용하지 않았나요 ? `
- JVM 은 오픈소스인 안드로이드에 적용하기엔 **라이센스 관련 이슈**가 존재
- 메모리를 많이 사용하기에 모든게 PC 보다 열약한 모바일 기기에 적용하기에는 한계 존재

#### DVM 을 사용한 이유
```markdown
- DVM 이 적은 메모리 사용을 하면서 빠르게 동작할 수 있기 때문이다.
    - 모바일이라는 특성 상 많은 메모리 요구는 곧 성능 저하를 뜻함
      - 많은 메모리를 요구하는 JVM 은 라이센스 문제를 제외하고도 사용 시 어려움 존재
```

#### ART 가 DVM 을 대체 할 수 있었던 이유
```markdown
- DVM 사용 시 보다 CPU / 메모리 / 배터리 소모량 낮음(압도적인 성능 개선)
- 부가적인 기능 개선
    - 가비지 컬렉션 최적화
    - 개발 및 디버깅 기능 개선 (ex. 샘플링 프로파일러 지원, 예외 및 비정상 종료 보고 진단 세부정보 제공, 자세한 디버깅 기능 추가 지원 등)
```

#### DVM 이란 ?
- Dalvik virtual machine 으로 **모바일 환경에서 JVM 을 대체** 할 수 있도록 구글이 개발해서 안드로이드 가상 머신으로 사용
- JIT 컴파일러를 사용

#### ART 란 ? (부제 : VM 인듯 VM 아닌 VM 같은 너)
- Android Runtime 으로 DVM 을 대체한 실행 환경
    - 부제 처럼 VM 은 아니지만 VM 처럼 동작하기 위한 기능을 가짐
- 초창기에는 AOT 컴파일러 사용
- 지속적으로 발전하며 **JIT 와 AOT 컴파일러** 모두 사용

`부제가 붙은 이유 ?`
- ART 에서는 최초 1회 dex 파일을 OAT 파일로 **미리 컴파일** 하여 가지고 있고, OAT 파일은 dex 파일을 컴파일 해 만들어진 native machine code 를 가지고 있음
    - 이는 기존 가상 머신 위에서 필요에 따라 실시간 으로 컴파일 하는 형식이 아니며, 가상머신을 거치기 않고 실행되어 가상 머신 없이 동작한다고 할 수도 있기 때문이다.
    - 하지만 native machine code 내부에 가상머신 상태를 흉내낸 코드가 있고, 이를 통해 동작하기 때문에 부제를 붙였습니다.

#### DVM / ART 차이점
1. **컴파일 타임이 상이**
    - DVM 은 **실행 시마다 컴파일** (CPU/메모리/배터리 소모 ↑)
    - ART 는 앱 설치 시에 전체 코드를 컴파일, 실행 시에는 변환된 코드를 읽어들이는 동작만 함 (CPU/메모리/배터리 소모 ↓)
        - 안드로이드 누가 버전 이상 부터는 JIT/AOT 컴파일러 모두 이용해서 설치 및 실행
            - **최초 설치 시 JIT 이용(설치 시간/ 용량 소모 ↓)** 
            - 차후 기기 미사용 시에 컴파일을 조금씩 해서 **자주 사용하는 앱은 AOT 방식으로 전환하도록 함**  
    
2. **지원하는 비트 차이 존재**
    - DVM 은 32 비트만 지원
    - ART 는 32, 64 모두 지원

3. **설치 파일 크기 상이**
    - DVM 은 비교적 설치 파일 작음 **(설치 시간 ↓)**
    - ART 는 비교적 설치 파일 큼 **(설치 시간 ↑)**

4. **실행 과정의 차이**
    - DVM : **java --> java byte code --> dalvik code --> DVM**
    - ART : **dex --> dex2oat --> native code --> ART Runtime**

#### JIT / AOT 컴파일러
`JIT(Just In Time)`
- 실행되는 시점에 코드를 컴파일 하여 machine code 변환 후 RAM 에 올리고 동작

`AOT(Ahead of Time)`
- 앱 설치 시에 모든 코드를 컴파일 하여 machine code 로 변환 후 ROM 에 저장, 실행 시에는 컴파일 된 코드를 읽어 바로 동작 시킴

##### JIT / AOT 개념이 찰떡 같이 기억에 남을 비유
- Q. 김치찌개를 만들어야 하는데 무슨 재료가 필요하지 ?

`JIT`

![JIT](https://user-images.githubusercontent.com/49216939/182871467-81a61099-462d-4d9a-94f1-6b8020a57e58.jpg)

- 필요해보이는 것만 일단 준비

`AOT`

![AOT](https://user-images.githubusercontent.com/49216939/182871378-0174fea6-367e-478c-8806-5877dc661da6.jpg)

- 필요한 것은 모두 준비하고 손질까지 완료

- [reference](https://www.charlezz.com/?p=42686)
- [reference](https://medium.com/@logishudson0218/android-apk%EC%9D%98-%EA%B5%AC%EC%84%B1%EA%B3%BC-%EC%83%88%EB%A1%9C%EC%9A%B4-app-bundle-%EB%B0%A9%EC%8B%9D-274b1a4cfb62)
- [document](https://source.android.com/devices/tech/dalvik?hl=ko)