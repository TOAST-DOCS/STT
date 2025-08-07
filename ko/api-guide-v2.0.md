## AI Service > Speech to Text > API 가이드

STT API v2.0은 더욱 풍부한 음성 인식 결과를 제공합니다.
STT API v2.0은 이전 버전의 응답 구조를 대폭 개선하여, 다양한 후처리와 사용자 경험 개선에 필요한 정보를 더 정교하게 제공합니다.

## API 공통 정보

### 사전 준비

- API 사용을 위해서는 {appKey}와 {secretKey}가 필요합니다.
- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

### 요청 공통 정보

- API를 사용하기 위해서는 {secretKey} 인증 처리가 필요합니다.
- 모든 API 요청 헤더의 **Authorization**에 {secretKey}를 넣어서 요청해야 합니다.

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급한 비밀 키 |

### 응답 공통 정보

- 모든 API 요청에 **200 OK**로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고합니다.

[성공 응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```

[실패 응답 본문]
```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 404,
        "resultMessage": "Please check your API Url, HTTP Method."
    }
}
```

[헤더]

| 이름 | 타입 | 설명 |
|---|---|---|
| isSuccessful | Boolean | 분석 API 성공 여부 |
| resultCode | Integer | 결과 코드 |
| resultMessage | String | 결과 메시지(성공 시 SUCCESS, 실패 시 오류 내용) |

## 음성 인식 API

### 음성 인식
- 오디오 파일의 음성 데이터를 텍스트 형태로 추출합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt |

[요청 본문]

- 음성 파일의 바이너리 데이터를 넣습니다.
- 사용자 단어 리스트(biasingList)로 입력된 값에 따라서 "차단계"로 인식된 단어는 "차단기"로, "안전 운행"으로 인식된 단어는 "안전운행"으로 치환된 결과가 제공됩니다.

```
curl -X POST 'https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-F 'biasingList="차단기_차단계"' \
-F 'biasingList="안전운행_안전 운행"' \ 
-H 'Authorization: ${secretKey}'
```

[필드]

| 이름 | 타입 | 설명                                                                                                                                                 |
|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------|
| audio | multipart/form–data | 음성 파일(WAV, WebM, MP3, OGG, FLAC, AAC, AC3)                                                                                                         |
| biasingList | multipart/form–data | 특정 단어나 구절을 우선적으로 인식하거나 치환하도록 돕는 파라미터. 예상되는 오인식 결과를 정정하거나, 특정 키워드를 강화하고자 할 때 사용합니다. 각 항목은 **"정답_모델인식값"** 형태로 구성됩니다. |

#### 응답

[응답 본문]
```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    },
    "result": {
        "inputLength": 220.0,
        "fileType": "mp3float",
        "text": [
            "예시 응답 텍스트입니다",
        ],
        "timeslot": [
            {
                "startTime": "390",
                "endTime": "12090"
            },
        ]
    }
}
```


[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| inputLength | Double | 인식된 음성 파일 길이(단위: 초) |
| fileType | String | 인식된 음성 파일 타입 |
| text | String[] | 인식된 음성의 텍스트 변환 결과 |
| timeslot | Timeslot[] | 동일 인덱스의 텍스트가 인식된 구간 정보 |

[Timeslot]

| 이름 | 타입 | 설명 |
|---|---|---|
| startTime | Long | 구간 시작 시간(millisecond) |
| endTime | Long | 구간의 종료 시간(millisecond) |
