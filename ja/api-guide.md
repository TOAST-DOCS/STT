## AI Service > Speech to Text > API 가이드

### 음성 인식 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt |

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |

[요청 본문]

- 음성 파일의 바이너리 데이터를 넣습니다.

```
curl -X POST 'https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-H 'Authorization: ${secretKey}'
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| audio | multipart/form–data | 음성 파일(WAV, WebM, MP3, OGG, FLAC, AAC, AC3) |

#### 응답

[응답 본문]
```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "inputLength": 1.85,
        "fileType": "mp3",
        "text": "안녕하세요.",
        "confidence": 0.94
    }
}
```

[헤더]

| 이름 | 타입 | 설명 |
|---|---|---|
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 SUCCESS, 실패 시 오류 내용) |

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| inputLength | Double | 인식된 음성 파일 길이(단위: 초) |
| fileType | String | 인식된 음성 파일 타입 |
| text | String | 인식된 음성의 텍스트 변환 결과 |
| confidence | Double | 인식 결과 신뢰도 |