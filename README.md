# Android에서 Firebase 클라우드 메시징 클라이언트 앱 설정

## Firebase 및 FCM SDK 설정
클라우드 메시징 Android 라이브러리의 종속 항목을 모듈 build.gradle에 추가한다.

```build.gradle
implementation 'com.google.firebase:firebase-messaging:18.0.0'
```

## 앱 매니페스트 수정
백그라운드에서 앱의 알림을 수신하는 것 외에 다른 방식으로 메시지를 처리하려는 경우에 필요하다. 
포그라운드 앱의 알림 수신, 데이터 페이로드 수신, 업스트림 메시지 전송 등을 수행하려면 이 서비스를 확장해야 한다.

```manifest
<service
    android:name=".java.MyFirebaseMessagingService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

## 기기 등록 토큰 액세스
앱을 처음 시작할 때 FCM SDK에서 클라이언트 앱 인스턴스용 등록 토큰을 생성한다. 
단일 기기를 타겟팅하거나 기기 그룹을 만들려면 FirebaseMessagingService를 확장하고 onNewToken을 재정의하여 이 토큰에 액세스해야 한다.

# 등록 토큰이 변경되는 경우
- 앱에서 인스턴스 ID 삭제
- 새 기기에서 앱 복원
- 사용자가 앱 삭제/재설치
- 사용자가 앱 데이터 제거

현재 등록 토큰 검색시에 아래와 같이 호출하면 된다.
```kotlin
FirebaseInstanceId.getInstance().getInstanceId()
```
