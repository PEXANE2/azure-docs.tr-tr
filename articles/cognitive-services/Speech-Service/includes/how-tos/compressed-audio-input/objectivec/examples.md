---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 788903d802ca47c763734e7cf6ddbbf3b0032203
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78943809"
---
Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için bir `SPXPullAudioInputStream` veya `SPXPushAudioInputStream`oluşturun.

Aşağıdaki kod parçacığında, akışın sıkıştırma biçimi olarak bir MP3 belirterek, bir `SPXPushAudioInputStream`örneğinden bir `SPXAudioConfiguration` oluşturma gösterilmektedir.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

Sonraki kod parçacığında sıkıştırılmış ses verilerinin bir dosyadan okunmasının ve `SPXPushAudioInputStream`nasıl balde gösterildiği gösterilmektedir.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
