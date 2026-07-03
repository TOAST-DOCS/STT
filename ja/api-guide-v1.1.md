## AI Service > Speech to Text > APIガイド

### 音声認識API

#### リクエスト

STT APIは、API呼び出し時の認証/認可のためにUser Access Keyトークンを使用します。User Access Keyトークンは、User Access Keyに基づいて発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用に関する詳細は、[User Access Keyトークン](/nhncloud/ja/public-api/user-access-key-token)を参照してください。

[URI]

| メソッド | URI                                                              |
|------|------------------------------------------------------------------|
| POST | https://api-speech.nhncloudservice.com/v1.1/appkeys/{appKey}/stt |

[リクエストヘッダ]

| 名前                  | 値                              | 説明                    |
|---------------------|--------------------------------|-----------------------|
| X-NHN-Authorization | Bearer {User Access Key Token} | User Access Key トークン |

[リクエスト本文]

- 音声ファイルのバイナリデータを入れます。

```
 
-F 'audio=@sample.mp3' \
-H 'X-NHN-Authorization: Bearer ${User Access Key Token}'
```

[フィールド]

| 名前    | タイプ                 | 説明                                    |
|-------|---------------------|---------------------------------------|
| audio | multipart/form–data | 音声ファイル(WAV、WebM、MP3、OGG、FLAC、AAC、AC3) |

#### レスポンス

[レスポンス本文]
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
        "text": "こんにちは。",
        "confidence": 0.94
    }
}
```

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 分析API成否                        |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はSUCCESS、失敗時はエラー内容) |

[フィールド]

| 名前          | タイプ    | 説明                  |
|-------------|--------|---------------------|
| inputLength | Double | 認識した音声ファイルの長さ(単位：秒) |
| fileType    | String | 認識した音声ファイルのタイプ      |
| text        | String | 認識した音声のテキスト変換結果     |
| confidence  | Double | 認識結果の信頼度            |
