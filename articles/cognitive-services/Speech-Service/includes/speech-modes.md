---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: trbye
ms.openlocfilehash: d97a1bf00d722414bedda4be79adc0c26d02c751
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422158"
---
## <a name="speech-modes"></a>Konuşma modları

**Etkileşimli**
- Komut ve kontrol senaryoları içindir.
- X segmentasyon süresi değerine sahiptir.
- Tanınan bir sözcük sonunda, hizmet o istek kimliğinden ses işlemeyi durdurur ve sırayı sona erdirir. Bağlantı kapalı değil.
- Tanıma için maksimum sınır 20'dir.
- Çağırmak için tipik `RecognizeOnceAsync`Karbon çağrısı .

**Konuşma**
- Daha uzun süre çalışan tanımalar içindir.
- Y. segmentasyon zaman çıkış değerine sahiptir (Y != X)
- Dönüş sona ermeden birden fazla tam söyleyiş işleyecek.
- Çok fazla sessizlik için dönüş sona erecek.
- Karbon yeni bir istek kimliği ile devam edecek ve gerektiğinde ses yeniden.
- Hizmet, 10 dakikalık konuşma tanıma sonrasında zorla bağlantısını kesecektir.
- Karbon, onaylanmamış sesi yeniden bağlar ve yeniden oynatır.
- Karbon ile `StartContinuousRecognition`çağrılır.

**Dikte**
- Kullanıcıların konuşarak noktalama işaretlerini belirtmelerine olanak tanır.
- Tanımayı başlatan API `EnableDictation` çağrısıne bakılmaksızın `SpeechConfig` nesne üzerinde belirterek Karbon olarak çağrılır.
- 1.<sup>st</sup> taraf kümesi `speech.fragment` ara sonuçlar için iletileri döndürür,<sup>3.</sup> `speech.hypothesis`