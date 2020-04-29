---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: f0baac79bfbfc06b24d692caef87a20013c4654c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81421922"
---
Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için, `SPXPullAudioInputStream` veya `SPXPushAudioInputStream`oluşturun.

Aşağıdaki kod parçacığında, akışın sıkıştırma biçimi olarak `SPXAudioConfiguration` bir MP3 belirterek bir `SPXPushAudioInputStream`öğesinin örneğinden nasıl oluşturulacağı gösterilmektedir.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Sonraki kod parçacığı, sıkıştırılmış ses verilerinin bir dosyadan nasıl okunacağını ve içine balkatılmadığını gösterir `SPXPushAudioInputStream`.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
