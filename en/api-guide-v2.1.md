<!-- pre-align:aligned sig=c189227c350c -->

<a id="ai-service-speech-to-text-api-guide"></a>
## AI Service > Speech to Text > API Guide { #ai-service-speech-to-text-api-guide }

Speech to Text API v2.1 provides more varied results for voice recognition.
Speech to Text API v2.1 significantly improves the response structure of previous verions, providing more sophisticated information needed for various post-processing and improving the user experience.

<a id="api-common-information"></a>
## API Common Information { #api-common-information }

<a id="preliminary-preparation"></a>
### Preliminary preparation { #preliminary-preparation }

Speech to Text API uses User Access Key tokens for authentication and authorization. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key. For more information on issuing and using User Access Key tokens, see the [User Access Key Token](/nhncloud/en/public-api/user-access-key-token).

[Request Header]

| Name                | Value                          | Description            |
|---------------------|--------------------------------|------------------------|
| X-NHN-Authorization | Bearer {User Access Key Token} | User Access Key token |

<a id="response-common-information"></a>
### Response Common Information { #response-common-information }

- Respond **200 OK** for every API request. For the detailed response result, refer to the header of the response body.

[Success Response Body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```

[Failure Response Body]
```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 404,
        "resultMessage": "Please check your API Url, HTTP Method."
    }
}
```

[Header]

| Name          | Type    | Description                                                     |
|---------------|---------|-----------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API success or failure                                 |
| resultCode    | Integer | Result Code                                                     |
| resultMessage | String  | Result Message (SUCCESS for success, error message for failure) |

<a id="voice-recognition-api"></a>
## Voice Recognition API { #voice-recognition-api }

<a id="voice-recognition"></a>
### Voice Recognition { #voice-recognition }
- Extract the voice data from the audio file into text format.

[URI]

| Method  | URI                                                              |
|---------|------------------------------------------------------------------|
| POST    | https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt |

[Request Body]

- Insert binary data of the voice file.
- As per the user word list (biasingList), the word recognized "차단계" is replaced with "차단기", and the word recognized "안전 운행" is replaced with "안전운행".

```
curl -X POST 'https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-F 'biasingList="차단기_차단계"' \
-F 'biasingList="안전운행_안전 운행"' \ 
-H 'X-NHN-Authorization: Bearer ${User Access Key Token}'
```

[Field]

| Name        | Type                | Required     | Description                                                                                                                 |
|-------------|---------------------|-------|--------------------------------------------------------------------------------------------------------------------|
| audio       | multipart/form–data | Required     | Voice file (WAV, WebM, MP3, OGG, FLAC, AAC, AC3)                                                                         |
| biasingList | String[]            | Not required | Parameters that help to prioritize recognition or replacement of specific words or phrases. It's used for when you want to correct expected misrecognition results or strengthen specific keywords. Each item is structured in the form **"answer_modelRecognitionValue**. |

<a id="voice-recognition-response"></a>
#### Response

[Response Body]
```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    },
    "result": {
        "inputLength": 220.1,
        "fileType": "mp3float",
        "text": [
            "An example response text",
        ],
        "timeslot": [
            {
                "startTime": "390",
                "endTime": "12090"
            },
        ],
		"confidence": [
			0
		]
    }
}
```


[Field]

| Name                  | Type     | Description                                                        |
|-----------------------|----------|--------------------------------------------------------------------|
| inputLength           | Double   | Recognized voice file length (unit: second)                        |
| fileType              | String   | Recognized voice file type                                         |
| text                  | String[] | Result for text conversion of recognized voice                     |
| timeslot              | List     | Section information where the text of the same index is recognized |
| timeslot[0].startTime | Long     | Section start time (millisecond)                                   |
| timeslot[0].endTime   | Long     | Section end time (millisecond)                                     |
| confidence            | Double[] | Reliability of text recognition results for the same index         |


<a id="voice-recognition-api-asynchronous"></a>
## Voice Recognition API (asynchronous) { #voice-recognition-api-asynchronous }

<a id="voice-recognition-asynchronous"></a>
### Voice Recognition (asynchronous) { #voice-recognition-asynchronous }
- Extract voice data from audio files in text format (asynchronous).

[URI]

| Method  | URI                                                                    |
|---------|------------------------------------------------------------------------|
| POST    | https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt/async |

[Request Body]

- Request speech recognition by providing an audio file as a downloadable URL.
- Change {appKey} to the value you checked in the console, and replace {User Access Key Token} with your issued User Access Key token.
- As per the user word list (biasingList), the word recognized "차단계" is replaced with "차단기", and the word recognized "안전 운행" is replaced with "안전운행".

```
curl -X POST 'https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt/async' \
-H 'X-NHN-Authorization: Bearer ${User Access Key Token}' \
-H 'Content-Type: application/json' \
--data '{"audioUrl": "https://url/to/audioFile", "biasingList": ["차단기_차단계", "안전운행_안전 운행"]}'
```

[Field]

| Name        | Type      | Required     | Description                                                                                                           |
|-------------|----------|-------|--------------------------------------------------------------------------------------------------------------|
| audioUrl    | String    | Required     | Downloadable audio files up to 150MB in size (WAV, WebM, MP3, OGG, FLAC, AAC, AC3)                                         |
| biasingList | String[]  | Not required | Parameters that help to prioritize recognition or replacement of specific words or phrases. It's used for when you want to correct expected misrecognition results or strengthen specific keywords. Each item is structured in the form **"answer_modelRecognitionValue**. |

<a id="voice-recognition-asynchronous-response"></a>
#### Response

[Response Body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "taskId": "6acb2d15-2180-4e79-b92f-45b1e887e920"
}
```

[Field]

| Name   | Type   | Description                                 |
|--------|--------|---------------------------------------------|
| taskId | String | Task UUID that can request results, retries |


<a id="check-status"></a>
### Check Status { #check-status }
- Retrieve the current status of the task requested.

[URI]

| Method  | URI                                                                                    |
|---------|----------------------------------------------------------------------------------------|
| GET     | https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt/async/{taskId}/status |

[Field]

| Name   | Type   | Required  | Description                            |
|--------|--------|-------|-------------------------------|
| taskId | String | Required  | Task UUID received after calling the asynchronous speech recognition API |

<a id="check-status-response"></a>
#### Response

[Response Body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "success"
    },
    "taskId": "d3dc604c-ebef-411a-959e-16f99770f2cf",
    "taskStatus": "COMPLETED",
    "result": {
        "inputLength": 220.1,
        "fileType": "mp3float",
        "text": [
            "An example response text",
        ],
        "timeslot": [
            {
                "startTime": "390",
                "endTime": "12090"
            }
        ],
		"confidence": [
			0
		]
    }
}
```

[Field]

| Name       | Type   | Description                                                   |
|------------|--------|---------------------------------------------------------------|
| taskId     | String | Task UUID requesting status inquiry                           |
| taskStatus | String | Current task status (PENDING, IN_PROGRESS, COMPLETED, FAILED) |
| result     | Result | Result value in case the task status is COMPLETED             |

[Result]

| Name                  | Type     | Description                     |
|-----------------------|----------|------------------------|
| inputLength           | Double   | Recognized voice file length (unit: second)                        |
| fileType              | String   | Recognized voice file type                                         |
| text                  | String[] | Result for text conversion of recognized voice                     |
| timeslot              | List     | Section information where the text of the same index is recognized |
| timeslot[0].startTime | Long     | Section start time (millisecond)                                   |
| timeslot[0].endTime   | Long     | Section end time (millisecond)                                     |
| confidence            | Double[] | Reliability of text recognition results for the same index         |

<a id="retry"></a>
### Retry { #retry }
- Request to retry failed task.

[URI]

| Method  | URI                                                                                   |
|---------|---------------------------------------------------------------------------------------|
| GET     | https://api-speech.nhncloudservice.com/v2.1/appkeys/{appKey}/stt/async/{taskId}/retry |

[Field]

| Name   | Type   | Required | Description                                                              |
|--------|--------|----------|--------------------------------------------------------------------------|
| taskId | String | Required | Task UUID received after calling the asynchronous speech recognition API |

<a id="retry-response"></a>
#### Response

[Response Body]

```
{
	"header": {
		// Omitteds
	},
	"result": {
		"taskId": "c337256d-b17e-42ce-9f63-a792a05ae0ef"
	}
}
```
