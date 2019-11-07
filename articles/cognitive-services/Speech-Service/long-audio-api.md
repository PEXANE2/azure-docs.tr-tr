---
title: Uzun ses API 'SI (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/26/2019
ms.author: erhopf
ms.openlocfilehash: 391cddbbd1b69fb7cb5422adbaea2f3378e273bf
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580155"
---
# <a name="long-audio-api-preview"></a>Uzun ses API 'SI (Önizleme)

Uzun ses API 'SI, zaman uyumsuz birleştirme için tasarlanan uzun biçimli metin (örneğin, ses kitapları) için tasarlanmıştır. Bu API gerçek zamanlı olarak birleştirilmiş ses döndürmez, bunun yerine beklentiler, yanıtları yoklayacaksınız ve çıkışta hizmetten kullanılabilir hale getirildiklerinden çıktıyı tüketebilir. Konuşma SDK 'Sı tarafından kullanılan metinden konuşmaya dönüştürme API 'sinin aksine, uzun ses API 'SI 10 dakikadan daha uzun bir şekilde birleştirilmiş ses oluşturabilir ve bu da Yayımcılar ve ses içeriği platformları için idealdir.

Uzun ses API 'sinin ek avantajları:

* Hizmet tarafından döndürülen sentezleştirilmiş konuşma, yüksek kaliteli ses çıkışları sağlayan sinir seslerini kullanır.
* Gerçek zamanlı yanıtlar desteklenmediğinden, bir ses uç noktası dağıtmanız gerekmez.

## <a name="workflow"></a>İş akışı

Genellikle, uzun ses API 'SI kullanılırken, birleştirme için bir metin dosyası ya da dosya gönderir, durum için yoklama yapılır, ardından durum başarılı olursa ses çıkışını indirebilirsiniz.

Bu diyagram, iş akışına üst düzey bir genel bakış sağlar.

![Uzun ses API iş akışı diyagramı](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Birleştirme için içerik hazırlama

Metin dosyanızı hazırlarken şunları yaptığınızdan emin olun:

* Düz metin (. txt) ya da SSML metni (. txt)
  * Düz metin için, her paragraf **Enter/Return** -View [düz metin girişi örneğine](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) vurarak ayrılır
  * SSML metninde her SSML parçası bir paragraf olarak değerlendirilir. SSML parçaları farklı paragraflar ile ayrılmalıdır- [SSML metin girişi örneğini](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt)görüntüleyin. Dil kodu için bkz. [konuşma Sensumu biçimlendirme dili (SSML)](speech-synthesis-markup.md)
* , [Bayt sıra Işaretiyle UTF-8 olarak kodlanır (BOM)](https://www.w3.org/International/questions/qa-utf8-bom.en#bom)
* 10.000 ' den fazla karakter veya en fazla 50 paragraf içerir
* ZIP değil tek bir dosyadır

## <a name="audio-output-formats"></a>Ses çıkış biçimleri

Aşağıdaki ses çıkış biçimleri, uzun ses API 'SI tarafından desteklenir:

> [!NOTE]
> Varsayılan ses biçimi Riff-16khz-16bit-mono-PCM ' dir.

* Riff-8khz-16bit-mono-PCM
* Riff-16khz-16bit-mono-PCM
* Riff-24khz-16bit-mono-PCM
* Riff-48kHz-16bit-mono-PCM
* Ses-16khz-32K bit hızı-mono-MP3
* Ses-16khz-64kbit hızı-mono-MP3
* Ses-16khz-128kbit hızı-mono-MP3
* Ses-24khz-48kbit hızı-mono-MP3
* Ses-24khz-96kbit hızı-mono-MP3
* Ses-24khz-160kbit hızı-mono-MP3

## <a name="quickstarts"></a>Hızlı Başlangıçlar

Uzun ses API 'sini başarılı bir şekilde çalıştırmanıza yardımcı olacak hızlı başlangıçlara sunuyoruz. Bu tablo, dile göre düzenlenen uzun ses API 'SI hızlı başlangıçlarını bir listesini içerir.

* [Hızlı başlangıç: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Örnek kod
Uzun ses API 'sinin örnek kodu GitHub ' da kullanılabilir.

* [Örnek kod: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Örnek kod:C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Örnek kod: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
