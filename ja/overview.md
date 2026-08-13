<!-- pre-align:aligned sig=6a53191beba7 -->

<a id="ai-service-speech-to-text-overview"></a>
## AI Service > Speech to Text > 概要 { #ai-service-speech-to-text-overview }

Speech to Text(STT)はNHN Cloudの音声認識および文字合成技術を利用して、入力された音声を認識し、認識した音声をテキストに変換して提供します。音声の書き出し、音声によるデバイス制御、音声チャットボットサービスなど、音声を文字に変換して利用するさまざまな分野に適用できます。

<a id="main-features"></a>
## 主な機能 { #main-features }

* **音声認識**
    * 入力された音声からNHN CloudのSpeech to Textエンジンを利用して音声を認識し、変換されたテキストを提供します。
    * 音声認識は韓国語に限り合成結果を提供します。

* **さまざまな方式の音声入力をサポート**
    * 認識する音声を音声ファイルでアップロードできます。
    * マイクで音声を録音して音声入力ができます。

* **認識結果ダウンロードサポート**
    * JSON、TXTファイルをダウンロードできます。
    * 音声認識結果ファイルをダウンロードして自由に修正できます。

<a id="voice-input-guide"></a>
## 音声入力ガイド { #voice-input-guide }

より正確な音声認識のために以下のガイドを参照してください。

<a id="common-recommendations"></a>
### 共通推奨事項 { #common-recommendations }

* サポートファイル形式: WAV, MP3, WebM, OGG, FLAC, AAC, AC3
* 推奨事項
    * ファイル形式：WAV
    * Bit：16bit
    * サンプルレート：16kHz
    * チャンネル数：モノ(mono)
    * 音声ファイル時間：10秒
* なるべく静かな環境で録音してください。

<a id="synchronous-api"></a>
### 同期(Synchronous) API { #synchronous-api }

* 最大容量：150MB
* 音声ファイルの認識可能時間：最小0.36秒、最大3,600秒
* 短い音声に対して迅速なレスポンスが必要な場合に適しています。

<a id="asynchronous-api"></a>
### 非同期(Asynchronous) API { #asynchronous-api }

* 最大容量：150MB
* 音声ファイルの認識可能時間：最小0.36秒、最大3,600秒(1時間)
* 長い音声または大容量ファイルの処理に適しており、結果は非同期で返されます。

<a id="service-targets"></a>
## サービス対象 { #service-targets }
* 音声を自動的に書き出す機能の構築が必要な場合(サポート相談、字幕作成など)
* 音声によるデバイス制御が必要な場合(IoTデバイスなど)
* 音声チャットボットサービスを構築する場合

<a id="information-on-processing-of-personal-information"></a>
## 個人情報処理についての案内 { #information-on-processing-of-personal-information }
* Speech to Textサービスを利用する過程でお客様は利用者の個人情報を収集/利用することができます。この場合、お客様は個人情報保護法など関連法令を遵守する義務があります。また本サービスを利用することによりお客様はNHN Cloudに個人情報処理に関する業務を委託および提供することになります。委託者の立場にあるお客様は、受託会社であるNHN Cloudと別途書面による個人情報処理業務委託契約を締結することができ、お客様が運営する個人情報処理方針に以下の内容を参考にして告知することができ、利用者から個人情報の第三者提供に関する同意を得る必要があります。
    - 受託業者：NHN Cloud(株)
    - 委託業務の内容：Speech to Textサービス提供
