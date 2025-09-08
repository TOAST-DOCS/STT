## AI Service > Speech to Text > APIガイド

STT API v2.0は、より豊富な音声認識結果を提供します。
STT API v2.0は、旧バージョンのレスポンス構造を大幅に改善し、多様な後処理やユーザーエクスペリエンスの向上に必要な情報を、より精巧に提供します。

## API共通情報

### 事前準備

- APIを使用するには、{appKey}と{secretKey}が必要です。
- {appKey}と{secretKey}は、コンソール上部の**URL & Appkey**メニューで確認できます。

### リクエスト共通情報

- APIを使用するには{secretKey}認証処理が必要です。
- 全てのAPIリクエストヘッダの **Authorization**に{secretKey}を入れてリクエストする必要があります。

[リクエストヘッダ]

| 名前 | 値 | 説明 |
|---|---|---|
| Authorization | {secretKey} | コンソールで発行した秘密鍵 |

### レスポンス共通情報

- 全てのAPIリクエストに **200 OK**でレスポンスします。詳細なレスポンス結果はレスポンス本文のヘッダを参照してください。

[成功レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success"
    }
}
```

[失敗レスポンス本文]
```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 404,
        "resultMessage": "Please check your API Url, HTTP Method."
    }
}
```

[ヘッダ]

| 名前 | タイプ | 説明 |
|---|---|---|
| isSuccessful | Boolean | 分析API成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功時はSUCCESS、失敗時はエラー内容) |

## 音声認識API

### 音声認識
- オーディオファイルの音声データをテキスト形式で抽出します。

[URI]

| メソッド | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt |

[リクエスト本文]

- 音声ファイルのバイナリデータを入力します。
- ユーザー単語リスト(biasingList)に入力された値に基づき、「しゃだんけ」と認識された単語は「遮断機」に、「安全 運転」と認識された単語は「安全運転」に置換された結果が提供されます。

```
curl -X POST 'https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt' \
-F 'audio=@sample.mp3' \
-F 'biasingList="遮断機_しゃだんけ"' \
-F 'biasingList="安全運行_安全運行"' \ 
-H 'Authorization: ${secretKey}'
```

[フィールド]

| 名前 | タイプ | 必須かどうか |説明                                                                                                                                               |
|---|---|-------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| audio | multipart/form–data | 必須  | 音声ファイル(WAV, WebM, MP3, OGG, FLAC, AAC, AC3)                                                                                                         |
| biasingList | String[] | 任意  | 特定の単語やフレーズを優先的に認識または置換するためのパラメータ。想定される誤認識の結果を訂正したり、特定のキーワードを強化したりする場合に使用します。各項目は**「正解_モデル認識値」**の形式で構成されます。 |

#### レスポンス

[レスポンス本文]
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
            "レスポンステキストの例です",
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


[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| inputLength | Double | 認識された音声ファイルの長さ(単位：秒) |
| fileType | String | 認識された音声ファイルのタイプ |
| text | String[] | 認識された音声のテキスト変換結果 |
| timeslot | List | 同じインデックスのテキストが認識された区間情報 |
| timeslot[0].startTime | Long | 区間開始時間(millisecond) |
| timeslot[0].endTime | Long | 区間の終了時間(millisecond) |
| confidence | Double[] | 同じインデックスのテキスト認識結果の信頼度 |


## 音声認識API (非同期)

### 音声認識(非同期)
- オーディオファイルの音声データをテキスト形式で抽出します。(非同期)

[URI]

| メソッド | URI                                                                    |
|---|------------------------------------------------------------------------|
| POST | https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt/async |

[リクエスト本文]

- オーディオファイルをダウンロード可能なURLで提供し、音声認識をリクエストします。
- {appKey}と{secretKey}はコンソールで確認した値に変更してください。
- ユーザー単語リスト(biasingList)に入力された値に基づき、「しゃだんけ」と認識された単語は「遮断機」に、「安全 運転」と認識された単語は「安全運転」に置換された結果が提供されます。

```
curl -X POST 'https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt/async' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type: application/json' \
--data '{"audioUrl": "https://url/to/audioFile", "biasingList": ["遮断機_しゃだんけ", "安全運転_安全 運転"]}'
```

[フィールド]

| 名前     | タイプ     | 必須かどうか | 説明                                                                                                         |
|----------|----------|-------|--------------------------------------------------------------------------------------------------------------|
| audioUrl | String   | 必須  | 最大150MBサイズのダウンロード可能な音声ファイルURL(WAV, WebM, MP3, OGG, FLAC, AAC, AC3)                                        |
| biasingList   | String[] | 任意 | 特定の単語やフレーズを優先的に認識または置換するためのパラメータ。想定される誤認識の結果を訂正したり、特定のキーワードを強化したりする場合に使用。各項目は**「正解_モデル認識値」**の形式で構成。 |

#### レスポンス

[レスポンス本文]

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

[フィールド]

| 名前       | タイプ   | 説明                       |
|------------|--------|----------------------------|
| taskId | String | 結果照会、再試行をリクエストできるタスクUUID |


### ヘルスチェック
- リクエストしたタスクの現在の状態を照会します。

[URI]

| メソッド | URI                                                                             |
|---|---------------------------------------------------------------------------------|
| GET | https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt/async/{taskId}/status |

[フィールド]

| 名前     | タイプ     | 必須かどうか | 説明             |
|----------|----------|-------|------------------|
| taskId | String   | 必須  | 非同期音声認識APIの呼び出し後に受け取ったタスクUUID |

#### レスポンス

[レスポンス本文]

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
        "inputLength": 220.0,
        "fileType": "mp3float",
        "text": [
            "レスポンステキストの例です",
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

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| taskId | String   | 状態照会をリクエストしたタスクUUID |
| taskStatus | String   | 作業の現在状態(PENDING, IN_PROGRESS, COMPLETED, FAILED) |
| result | Result   | 作業の状態がCOMPLETEDの場合の結果値 |

[Result]

| 名前 | タイプ | 説明 |
|---|---|---|
| inputLength | Double | 認識された音声ファイルの長さ(単位：秒) |
| fileType | String | 認識された音声ファイルタイプ |
| text | String[] | 認識された音声のテキスト変換結果 |
| timeslot | List | 同じインデックスのテキストが認識された区間情報 |
| timeslot[0].startTime | Long | 区間開始時間(ミリ秒) |
| timeslot[0].endTime | Long | 区間の終了時間(ミリ秒) |
| confidence | Double[] | 同じインデックスのテキスト認識結果信頼度 |

### 再試行
- 失敗した作業の再試行をリクエストします。

[URI]

| メソッド | URI                                                                             |
|---|---------------------------------------------------------------------------------|
| GET | https://speech.api.nhncloudservice.com/v2.0/appkeys/{appKey}/stt/async/{taskId}/retry |

[フィールド]

| 名前     | タイプ     | 必須かどうか | 説明             |
|----------|----------|-------|------------------|
| taskId | String   | 必須  | 非同期音声認識API呼び出し後に受け取ったタスクUUID |

#### レスポンス

[レスポンス本文]

```
{
	"header": {
		// 省略
	},
	"result": {
		"taskId": "c337256d-b17e-42ce-9f63-a792a05ae0ef"
	}
}
```
