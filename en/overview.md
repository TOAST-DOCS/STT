## AI Service > Speech to Text > Overview

Speech to Text service uses NHN Cloud's speech recognition and text synthesis technology to recognize input voice and convert the recognized voice into text. It can be applied to various fields that require conversion of voice into text such as voice dictation, device control through voice, and voice chatbot service.

## Main Features

* **Speech recognition**
    * The services uses NHN Cloud's Speech to Text engine to recognize speech from the input voice and provide the converted text.
    * The speech recognition provides synthesis results for Korean only.

* **Supports various types of voice input**
    * You can upload the voice to be recognized as a voice file.
    * You can record your voice with the microphone for voice input.
    
* **Support for downloading recognition results**
    * You can download JSON or TXT files.
    * You can download the speech recognition result file and modify it as necessary.

## 음성 입력 가이드

보다 정확한 음성 인식을 위해 아래의 가이드를 참고하시기 바랍니다.

* 업로드 파일 지원 형식 : .wav, .webm, .mp3, .ogg, .flac, .aac, .ac3
* 음성 파일 최대 시간 : 60초
* 음성 입력 파일 권장 사항
* 파일 형식 : wav
* 비트 : 16bit
* 샘플레이트 : 1.6kHz
* 채널 수 : mono
* 음성 파일 시간 : 10초
* 최대한 조용한 환경에서 녹음해 주시기 바랍니다.

## Service Targets
* When it is necessary to build a feature to automatically dictate voice (customer center consultation, subtitle creation, etc.)
* When device control through voice is required (IoT device, etc.)
* When you are building a voice chatbot service

## Information on Processing of Personal Information
* While using the Speech to Text service, the customer may collect/use the user's personal information. In this case, the customer is obliged to comply with relevant laws such as the Personal Information Protection Act. In addition, by using this service, the customer consigns and provides the work regarding personal information processing to NHN. A customer in the status of a consignor may conclude a separate written personal information processing consignment contract with NHN, the consignee, and may notify the following in the privacy policy operated by the customer, and must obtain consent for provision of personal information to a third party from users.
    - Consignee: NHN
    - Consignment Description: Providing the Speech to Text service
