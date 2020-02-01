---
title: Uzun ses API 'SI (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Uzun ses API 'sinin zaman uyumsuz senkron metin okuma için nasıl tasarlandığına öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: dapine
ms.openlocfilehash: 033103e10971be2f6c220ccdb2c3586c7dc2ef05
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900256"
---
# <a name="long-audio-api-preview"></a>Uzun ses API 'SI (Önizleme)

Uzun ses API 'SI, zaman uyumsuz birleştirme için tasarlanan uzun biçimli metin (örneğin, ses kitapları) için tasarlanmıştır. Bu API gerçek zamanlı olarak birleştirilmiş ses döndürmez, bunun yerine beklentiler, yanıtları yoklayacaksınız ve çıkışta hizmetten kullanılabilir hale getirildiklerinden çıktıyı tüketebilir. Konuşma SDK 'Sı tarafından kullanılan metinden konuşmaya dönüştürme API 'sinin aksine, uzun ses API 'SI 10 dakikadan daha uzun bir şekilde birleştirilmiş ses oluşturabilir ve bu da Yayımcılar ve ses içeriği platformları için idealdir.

Uzun ses API 'sinin ek avantajları:

* Hizmet tarafından döndürülen sentezleştirilmiş konuşma, yüksek kaliteli ses çıkışları sağlayan sinir seslerini kullanır.
* Gerçek zamanlı yanıtlar desteklenmediğinden, bir ses uç noktası dağıtmanız gerekmez.

> [!NOTE]
> Uzun ses API 'SI artık yalnızca [özel sinir sesini](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices)destekliyor.

## <a name="workflow"></a>İş Akışı

Genellikle, uzun ses API 'SI kullanılırken, birleştirme için bir metin dosyası ya da dosya gönderir, durum için yoklama yapılır, ardından durum başarılı olursa ses çıkışını indirebilirsiniz.

Bu diyagram, iş akışına üst düzey bir genel bakış sağlar.

![Uzun ses API iş akışı diyagramı](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Birleştirme için içerik hazırlama

Metin dosyanızı hazırlarken şunları yaptığınızdan emin olun:

* Düz metin (. txt) ya da SSML metni (. txt)
* , [Bayt sıra Işaretiyle UTF-8 olarak kodlanır (BOM)](https://www.w3.org/International/questions/qa-utf8-bom.en#bom)
* ZIP değil tek bir dosyadır
* SSML metni için düz metin veya 400 [faturalanabilir karakter](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech#pricing-note) için 400 karakterden daha uzun ve 10.000 ' den az paragraf içeriyor
  * Düz metin için, her paragraf **Enter/Return** -View [düz metin girişi örneğine](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) vurarak ayrılır
  * SSML metninde her SSML parçası bir paragraf olarak değerlendirilir. SSML parçaları farklı paragraflar ile ayrılmalıdır- [SSML metin girişi örneğini](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt) görüntüleyin
> [!NOTE]
> Çince (Mainland), Çince (Hong Kong), Çince (Tayvan), Japonca ve Korece için bir sözcük iki karakter olarak sayılır. 

## <a name="submit-synthesis-requests"></a>Sensıs isteklerini gönder

Giriş içeriği hazırlandıktan sonra, isteği göndermek için [uzun biçimli ses senup hızlı](https://aka.ms/long-audio-python) başlangıcı ' nı izleyin. Birden fazla giriş dosyanız varsa, birden çok istek göndermeniz gerekir. Bilmeniz için bazı sınırlamalar vardır: 
* İstemcinin her bir Azure abonelik hesabı için saniyede en fazla 5 istek göndermesine izin verilir. Sınırlama aştıysa, istemci 429 hata kodu (çok fazla istek) alır. Lütfen saniye başına istek miktarını azaltın
* Sunucunun her bir Azure abonelik hesabı için en fazla 120 istek çalıştırmasına ve sıraya alma yapmasına izin verilir. Sınırlama aşıldıysa, sunucu 429 hata kodu (çok fazla istek) döndürür. Lütfen bekleyin ve bazı istekler tamamlanana kadar yeni istek gönderilmesini önleyin
* Sunucu, her bir Azure aboneliği hesabı için en fazla 20.000 istek tutar. Bu sınırlama aşıldıysa, yeni bir istek göndermeden önce lütfen bazı istekleri silin

## <a name="audio-output-formats"></a>Ses çıkış biçimleri

Esnek ses çıkışı biçimlerini destekliyoruz. ' ConcatenateResult ' parametresini ayarlayarak, paragraf başına ses çıkışları oluşturabilir veya sesos 'yi bir çıkışa birleştirebilirsiniz. Aşağıdaki ses çıkış biçimleri, uzun ses API 'SI tarafından desteklenir:

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
