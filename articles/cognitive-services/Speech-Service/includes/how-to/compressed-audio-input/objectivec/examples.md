---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 788903d802ca47c763734e7cf6ddbbf3b0032203
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409576"
---
Sıkıştırılmış ses biçiminde Konuşma hizmetine akış yapmak `SPXPullAudioInputStream` `SPXPushAudioInputStream`için bir veya .

Aşağıdaki snippet, akışın `SPXAudioConfiguration` sıkıştırma biçimi olarak `SPXPushAudioInputStream`bir MP3'ü belirten bir örneğinden nasıl oluşturulacak larını gösterir.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Bir sonraki snippet, sıkıştırılmış ses verilerinin bir dosyadan `SPXPushAudioInputStream`nasıl okunabileceğini ve .'e nasıl pompalanabileceğini gösterir.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
