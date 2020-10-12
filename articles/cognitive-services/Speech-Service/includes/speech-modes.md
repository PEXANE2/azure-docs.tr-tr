---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: trbye
ms.openlocfilehash: d97a1bf00d722414bedda4be79adc0c26d02c751
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81422158"
---
## <a name="speech-modes"></a>Konuşma modları

**LiDE**
- Komut ve denetim senaryolarına yöneliktir.
- , Bir segment zaman aşımı değeri X ' i içerir.
- Tanınan bir zaman sonunda hizmet, bu istek KIMLIĞINDEN ses işlemeyi durduruyor ve sonra da sona erecektir. Bağlantı kapalı değil.
- Maksimum tanıma sınırı 20s 'dir.
- Invoke için tipik karbon çağrısı `RecognizeOnceAsync` .

**Konuşma**
- Daha uzun süre çalışan için tasarlanmıştır.
- , Bir segmentleme zaman aşımı değeri olan Y. (Y! = X)
- , Açmayı sonlandırmadan birden çok tamamlanmış şekilde işlem görür.
- , Çok fazla sessizlik için de sona erdir.
- Karbon, yeni bir istek KIMLIĞIYLE devam eder ve gerekirse sesi yeniden dener.
- Hizmet, 10 dakikadan kısa bir süre sonra, konuşma tanımayı zorla keser.
- Karbon, geri bağlantı kurar ve kabul edilmemiş sesi yeniden oynaacaktır.
- Karbon ile çağrıldı `StartContinuousRecognition` .

**Etme**
- Kullanıcıların konuşmayı konuşarak noktalama işaretleri belirlemesine izin verir.
- `EnableDictation` `SpeechConfig` TANıMAYı Başlatan API çağrısından bağımsız olarak nesne üzerinde belirtilerek bilgi olarak çağrılır.
- 1<sup>St</sup> taraf kümesi `speech.fragment` , 3. taraf ileti döndüren ara sonuçlara yönelik<sup>rd</sup> iletileri döndürür `speech.hypothesis` .