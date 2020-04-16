---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: f0baac79bfbfc06b24d692caef87a20013c4654c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421922"
---
Sıkıştırılmış ses biçiminde Konuşma hizmetine akış yapmak `SPXPullAudioInputStream` `SPXPushAudioInputStream`için bir veya .

Aşağıdaki snippet, akışın `SPXAudioConfiguration` sıkıştırma biçimi olarak `SPXPushAudioInputStream`bir MP3'ü belirten bir örneğinden nasıl oluşturulacak larını gösterir.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Bir sonraki snippet, sıkıştırılmış ses verilerinin bir dosyadan `SPXPushAudioInputStream`nasıl okunabileceğini ve .'e nasıl pompalanabileceğini gösterir.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
