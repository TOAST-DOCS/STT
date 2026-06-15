## AI Service > Speech to Text > API Guide

### Speech Recognition API

#### Request

### Authentication and Authorization

STT API uses User Access Key tokens for authentication and authorization when making API calls. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, see the [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

[URI]

| Method  | URI                                                              |
|---------|------------------------------------------------------------------|
| POST    | https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/stt |

[Request Header]

| Name                | Value                          | Description            |
|---------------------|--------------------------------|------------------------|
| X-NHN-Authorization | Bearer {User Access Key Token} | User Access Key token |

[Request Body]

- Input the binary data of the voice file.

```
curl -X POST 'https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-H 'X-NHN-Authorization: Bearer ${User Access Key Token}'
```

[Field]

| Name  | Type                | Description                                      |
|-------|---------------------|--------------------------------------------------|
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

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API successful or not                                |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (SUCCESS on success, error details on failure) |

[Field]

| Name        | Type   | Description                                    |
|-------------|--------|------------------------------------------------|
| inputLength | Double | Recognized voice file duration (unit: seconds) |
| fileType    | String | Recognized voice file type                     |
| text        | String | Text conversion result of recognized speech    |
| confidence  | Double | Recognition result confidence                  |