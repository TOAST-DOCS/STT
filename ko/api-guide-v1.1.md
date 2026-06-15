## AI Service > Speech to Text > API 가이드

### 음성 인식 API

#### 요청

STT API를 사용하려면 인증/인가를 위해 User Access Key 토큰을 사용합니다. User Access Key 토큰은 User Access Key를 기반으로 발급되는 Bearer 타입의 일시적 액세스 토큰입니다. User Access Key 토큰 발급 및 사용에 대한 자세한 내용은 [User Access Key 토큰](/nhncloud/ko/public-api/user-access-key-token)을 참고하세요.

[URI]

| 메서드  | URI                                                              |
|------|------------------------------------------------------------------|
| POST | https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/stt |

[요청 헤더]

| 이름                  | 값                              | 설명                  |
|---------------------|--------------------------------|---------------------|
| X-NHN-Authorization | Bearer {User Access Key Token} | User Access Key 토큰 |

[요청 본문]

- 음성 파일의 바이너리 데이터를 넣습니다.

```
curl -X POST 'https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-H 'X-NHN-Authorization: Bearer ${User Access Key Token}'
```

[필드]

| 이름    | 타입                  | 설명                                         |
|-------|---------------------|--------------------------------------------|
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

| 이름            | 타입      | 설명                               |
|---------------|---------|----------------------------------|
| isSuccessful  | Boolean | 분석 API 성공 여부                     |
| resultCode    | Integer | 결과 코드                            |
| resultMessage | String  | 결과 메시지(성공 시 SUCCESS, 실패 시 오류 내용) |

[필드]

| 이름          | 타입     | 설명                  |
|-------------|--------|---------------------|
| inputLength | Double | 인식된 음성 파일 길이(단위: 초) |
| fileType    | String | 인식된 음성 파일 타입        |
| text        | String | 인식된 음성의 텍스트 변환 결과   |
| confidence  | Double | 인식 결과 신뢰도           |