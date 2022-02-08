## AI Service > Speech to Text > APIガイド

### 音声認識API

#### リクエスト

- {appKey}と{secretKey}はコンソール上部の**URL & Appkey**メニューで確認できます。

[URI]

| メソッド | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
|---|---|---|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[リクエスト本文]

- 音声ファイルのバイナリデータを入れます。

```
 
-F 'audio=@sample.mp3' \
-H 'Authorization: ${secretKey}'
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
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

| 名前 | タイプ | 説明 |
|---|---|---|
| isSuccessful | Boolean | 分析API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はSUCCESS、失敗時はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| inputLength | Double | 認識した音声ファイルの長さ(単位：秒) |
| fileType | String | 認識した音声ファイルのタイプ |
| text | String | 認識した音声のテキスト変換結果 |
| confidence | Double | 認識結果の信頼度 |
