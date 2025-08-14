## AI Service > Speech to Text > Overview

Speech to Text(STT) service uses NHN Cloud's speech recognition and text synthesis technology to recognize input voice and convert the recognized voice into text. It can be applied to various fields that require conversion of voice into text such as voice dictation, device control through voice, and voice chatbot service.

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

## Voice Input Guide

For more accurate speech recognition, please refer to the following guide.

### Common recommendations

* Supported file type: WAV, MP3, WebM, OGG, FLAC, AAC, AC3
* Recommendations
    * File format: WAV
    * Bit: 16bit
    * Sample rate: 16 kHz
    * Number of channels: Mono
    * Voice file duration: 10 seconds
* Please record in an environment that is as quiet as possible.

### Synchronous API

* Maximum capacity: 10MB
* Recognition time for the voice file: minimum 0.36 seconds, maximum 150 seconds
* It is suitable for the case where a quick response is required for a short speech.

### Asynchronous API

* Maximum capacity: 150MB
* Recognition time for the voice file: minimum 0.36 seconds, maximum 3,600 seconds (1 hour)
* It is suitable for processing long speeches or large files. The results are returned asynchronously.

## Service Targets
* When it is necessary to build a feature to automatically dictate voice (customer center consultation, subtitle creation, etc.)
* When device control through voice is required (IoT device, etc.)
* When you are building a voice chatbot service

## Information on Processing of Personal Information
* While using the Speech to Text service, the customer may collect/use the user's personal information. In this case, the customer is obliged to comply with relevant laws such as the Personal Information Protection Act. In addition, by using this service, the customer consigns and provides the work regarding personal information processing to NHN Cloud. A customer in the status of a consignor may conclude a separate written personal information processing consignment contract with NHN Cloud, the consignee, and may notify the following in the privacy policy operated by the customer, and must obtain consent for provision of personal information to a third party from users.
    - Consignee: NHN Cloud Corp.
    - Consignment Description: Providing the Speech to Text service
