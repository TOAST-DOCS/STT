## AI Service > Speech to Text > API Guide

### Speech Recognition API

#### Request

* An AppKey or a Project Integrated Appkey is required to use the STT API.<br/>
  An AppKey is a unique authentication key issued for each individual NHN Cloud service, while a Project Integrated Appkey is a common authentication key that can be shared across multiple services within a single NHN Cloud project.<br/>
  For more information on checking and using Appkeys, please refer to the [Appkey](/nhncloud/en/public-api/appkey). For more information on creating and using Project Integrated Appkeys, please refer to the [Project Integrated Appkey](/nhncloud/en/public-api/project-integrated-appkey).

[URI]

| Method | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt |

[Request Header]

| Name | Value | Description |
|---|---|---|
| Authorization | {secretKey} | Security key issued from the console |

[Request Body]

- Input the binary data of the voice file.

```
curl -X POST 'https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-H 'Authorization: ${secretKey}'
```

[Field]

| Name | Type | Description |
|---|---|---|
| audio | multipart/form–data | Voice file (WAV, WebM, MP3, OGG, FLAC, AAC, AC3) |

#### Response

[Response Body]
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
        "text": "Hello.",
        "confidence": 0.94
    }
}
```

[Header]

| Name | Type | Description |
|---|---|---|
| isSuccessful | Boolean | Analysis API successful or not |
| resultCode | Integer | Result code |
| resultMessage | String | Result message (SUCCESS on success, error details on failure) |

[Field]

| Name | Type | Description |
|---|---|---|
| inputLength | Double | Recognized voice file duration (unit: seconds) |
| fileType | String | Recognized voice file type |
| text | String | Text conversion result of recognized speech |
| confidence | Double | Recognition result confidence |