## AI Service > Speech to Text > APIガイド

### 音声認識ファイルアップロードAPI

#### リクエスト

- {appKey}と{secretKey}はコンソール上部の**URL & Appkey**メニューで確認できます。

[URI]

| メソッド | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt |
| POST | https://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/files |

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

```
curl -X POST 'https://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/files' \
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

### 音声認識ストリーミングAPI

#### リクエスト

- {appKey}と{secretKey}はコンソール上部の**URL & Appkey** メニューで確認できます。

[URI]

| URI |
|---|
| wss://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/streams |

[リクエストヘッダ]

| 名前 | 値 | 説明 |
|---|---|---|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[サンプルコード]

```java
import com.nhncloud.stt.NhnCloudWebSocketClientEndPoint;

import javax.sound.sampled.*;
import javax.websocket.ClientEndpointConfig;
import javax.websocket.ContainerProvider;
import java.net.URI;
import java.nio.ByteBuffer;
import java.util.List;
import java.util.Map;

public class NhnCloudMicWebSocketClient {

    public static void main(String[] args) {
        String serverUri = "wss://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/streams";

        var webSocketContainer = ContainerProvider.getWebSocketContainer();
        var audioFormat = new AudioFormat(16000.0F, 16, 1, true, false);
        var info = new DataLine.Info(TargetDataLine.class, audioFormat);

        if (!AudioSystem.isLineSupported(info)) {
            System.out.println("Audio line is not supported");
            System.exit(1);
        }

        try {
            var clientEndpointConfig = ClientEndpointConfig.Builder.create()
                                                                   .configurator(new ClientEndpointConfig.Configurator() {
                                                                       @Override
                                                                       public void beforeRequest(Map<String, List<String>> headers) {
                                                                           headers.put("Authorization", List.of("{secretKey}"));
                                                                       }
                                                                   }).build();
            var session = webSocketContainer.connectToServer(SttWebSocketClientEndPoint.class, clientEndpointConfig, URI.create(serverUri));

            var targetDataLine = (TargetDataLine)AudioSystem.getLine(info);
            targetDataLine.open(audioFormat, targetDataLine.getBufferSize());
            targetDataLine.start();

            System.out.println("Mic recording...");

            var chunkSize = (int)(audioFormat.getSampleRate() * 0.02);
            var buffer = new byte[chunkSize];
            var ais = new AudioInputStream(targetDataLine);

            while (targetDataLine.isOpen()) {
                int bytesRead = ais.read(buffer);
                if ((bytesRead <= 0) && (targetDataLine.isOpen())) {
                    continue;
                }
                session.getBasicRemote().sendBinary(ByteBuffer.wrap(buffer));
            }

            ais.close();
            targetDataLine.stop();
            targetDataLine.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```java
import javax.websocket.*;

@ClientEndpoint
public class NhnCloudMicWebSocketClientEndPoint extends Endpoint {
    @Override
    public void onOpen(Session session, EndpointConfig endpointConfig) {
        session.addMessageHandler(new MessageHandler.Whole<String>() {
            @Override
            public void onMessage(String s) {
                System.out.println("message: " + s);
            }
        });
    }


    @Override
    public void onClose(Session session, CloseReason closeReason) {
        System.out.println("Connection closed. reason: " + closeReason.getReasonPhrase());
    }
}
```

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
        "text": "こんにちは。",
        "confidence": 0.94
        "startTime": 11.62
        "endTime": 12.68,
        "isFinal": false
    }
}
```

[ヘッダ]

| 名前 | タイプ | 説明 |
|---|---|---|
| isSuccessful | Boolean | 分析APIの成否 |
| resultCode | Integer | 結果コード |
| resultMessage | String | 結果メッセージ(成功の場合はSUCCESS、失敗の場合はエラー内容) |

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| text | String | 認識された音声のテキスト変換結果 |
| confidence | Double | 認識結果の信頼度 |
| startTime | Double | 認識文字の開始時間 |
| endTime | Double | 認識文字の終了時間 |
| isFinal | Boolean | ストリーミング終了の有無 |
