---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: trbye
ms.openlocfilehash: d97a1bf00d722414bedda4be79adc0c26d02c751
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422158"
---
## <a name="speech-modes"></a>Konuşma modları

**LiDE**
- Komut ve denetim senaryolarına yöneliktir.
- , Bir segment zaman aşımı değeri X ' i içerir.
- Tanınan bir zaman sonunda hizmet, bu istek KIMLIĞINDEN ses işlemeyi durduruyor ve sonra da sona erecektir. Bağlantı kapalı değil.
- Maksimum tanıma sınırı 20s 'dir.
- Invoke için tipik karbon çağrısı `RecognizeOnceAsync`.

**Konuşma**
- Daha uzun süre çalışan için tasarlanmıştır.
- , Bir segmentleme zaman aşımı değeri olan Y. (Y! = X)
- , Açmayı sonlandırmadan birden çok tamamlanmış şekilde işlem görür.
- , Çok fazla sessizlik için de sona erdir.
- Karbon, yeni bir istek KIMLIĞIYLE devam eder ve gerekirse sesi yeniden dener.
- Hizmet, 10 dakikadan kısa bir süre sonra, konuşma tanımayı zorla keser.
- Karbon, geri bağlantı kurar ve kabul edilmemiş sesi yeniden oynaacaktır.
- Karbon ile `StartContinuousRecognition`çağrıldı.

**Etme**
- Kullanıcıların konuşmayı konuşarak noktalama işaretleri belirlemesine izin verir.
- Tanımayı Başlatan API çağrısından bağımsız `EnableDictation` olarak `SpeechConfig` nesne üzerinde belirtilerek bilgi olarak çağrılır.
- 1<sup>St</sup> <sup>taraf kümesi, 3.</sup> taraf ileti döndüren `speech.hypothesis` ara sonuçlara yönelik iletileri döndürür `speech.fragment` .