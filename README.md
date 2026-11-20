# DayLeft - APK 만들기

준비물: Node.js 18+, Android Studio (SDK 포함), JDK 17+

1. 이 폴더에서 `npm install`
2. `npx cap add android`
3. `npx cap sync android`
4. `npx cap open android` -> Android Studio에서 Build > Build APK(s)
   (결과: android/app/build/outputs/apk/debug/app-debug.apk)

## 권한 (android/app/src/main/AndroidManifest.xml 에 추가)
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<uses-permission android:name="android.permission.INTERNET" />

## 스캔을 ML Kit으로 바꾸기 (권장)
www/index.html 의 startScan() 안을 아래처럼 바꾸면 기기 성능에 상관없이 인식됩니다.

  import 없이 쓰려면 번들러가 필요하므로, 간단히는 웹 방식(BarcodeDetector)을 먼저 시험하고
  인식이 불안정하면 @capacitor-mlkit/barcode-scanning 의 BarcodeScanner.scan() 결과의
  barcodes[0].rawValue 를 onBarcode(rawValue)에 넘기세요.

## 백그라운드 알림으로 바꾸기
checkAlerts() 대신 @capacitor/local-notifications 의 schedule()로
"유통기한 - 알림일수" 날짜 오전 9시에 알림을 예약하면 앱을 닫아도 울립니다.

## 컴퓨터에 아무것도 설치하지 않고 APK 받기 (GitHub Actions)
1. github.com 에서 무료 계정을 만들고 새 저장소(Repository)를 만든다. (이름: dayleft, Public)
2. 이 zip의 압축을 풀고, 폴더 안의 모든 파일(.github 폴더 포함)을 저장소에 업로드한다.
   (저장소 화면 > Add file > Upload files. 브랜치 이름은 main)
3. 저장소의 Actions 탭 > "Build APK" > Run workflow 를 누른다.
4. 약 5~10분 뒤 완료되면 실행 결과 화면 아래 Artifacts 의 "DayLeft-apk"를 내려받는다.
   (zip 안에 app-debug.apk 가 들어 있다)
5. APK를 폰으로 옮겨 설치한다. ("출처를 알 수 없는 앱" 허용 필요)
