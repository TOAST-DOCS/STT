## AI Service > Speech to Text > API 가이드

### 음성 인식 파일 업로드 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드 | URI |
|---|---|
| POST | https://speech.api.nhncloudservice.com/v1.0/appkeys/{appKey}/stt |
| POST | https://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/files |

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
```
curl -X POST 'https://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/files' \
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


### 음성 인식 스트리밍 API

#### 요청

- {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]
 
| URI |
|---|
| wss://api-stt.nhncloudservice.com/v1.0/appkeys/{appKey}/streams |

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |

[샘플 코드]

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
        "text": "안녕하세요.",
        "confidence": 0.94
        "startTime": 11.62
        "endTime": 12.68,
        "isFinal": false
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
| text | String | 인식된 음성의 텍스트 변환 결과 |
| confidence | Double | 인식 결과 신뢰도 |
| startTime | Double | 인식 문자의 시작 시간 |
| endTime | Double | 인식 문자의 종료 시간 |
| isFinal | Boolean | 스트리밍 종료 여부 |
