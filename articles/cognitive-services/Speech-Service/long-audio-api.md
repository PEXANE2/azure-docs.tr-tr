---
title: Uzun ses API 'SI (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Uzun ses API 'sinin zaman uyumsuz senkron metin okuma için nasıl tasarlandığına öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: trbye
ms.openlocfilehash: 550579b40470d7a1ad02031b8140e7d0a7164f46
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310606"
---
# <a name="long-audio-api-preview"></a>Uzun ses API 'SI (Önizleme)

Uzun ses API 'SI, zaman uyumsuz birleştirme için tasarlanan uzun biçimli metin (örneğin, ses kitapları, haber makaleleri ve belgeler) için tasarlanmıştır. Bu API gerçek zamanlı olarak birleştirilmiş ses döndürmez, bunun yerine beklentiler, yanıtları yoklayacaksınız ve çıkışta hizmetten kullanılabilir hale getirildiklerinden çıktıyı tüketebilir. Konuşma SDK 'Sı tarafından kullanılan metinden konuşmaya dönüştürme API 'sinin aksine, uzun ses API 'SI 10 dakikadan daha uzun bir şekilde birleştirilmiş ses oluşturabilir ve bu da Yayımcılar ve ses içeriği platformları için idealdir.

Uzun ses API 'sinin ek avantajları:

* Hizmet tarafından döndürülen sentezlenmiş konuşma en iyi sinir sesleri kullanır.
* Sesleri hiçbiri gerçek zamanlı toplu iş modunda birleştirdiğinden ses uç noktası dağıtmanız gerekmez.

> [!NOTE]
> Uzun ses API 'SI artık [genel sinir seslerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) ve [özel sinir seslerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices)desteklemektedir.

## <a name="workflow"></a>İş akışı

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
> Çince (Mainland), Çince (Hong Kong çın ÖIB), Çince (Tayvan), Japonca ve Korece için bir sözcük iki karakter olarak sayılır. 

## <a name="submit-synthesis-requests"></a>Sensıs isteklerini gönder

Giriş içeriği hazırlandıktan sonra, isteği göndermek için [uzun biçimli ses senup hızlı](https://aka.ms/long-audio-python) başlangıcı ' nı izleyin. Birden fazla giriş dosyanız varsa, birden çok istek göndermeniz gerekir. 

**Http durum kodları** yaygın hataları gösterir.

| API | HTTP durum kodu | Description | Teklif |
|-----|------------------|-------------|----------|
| Oluştur | 400 | Ses birleştirme bu bölgede etkin değil. | Konuşma abonelik anahtarını desteklenen bir bölge ile değiştirin. |
|        | 400 | Bu bölge için yalnızca **Standart** konuşma aboneliği geçerlidir. | Konuşma abonelik anahtarını "standart" fiyatlandırma katmanıyla değiştirin. |
|        | 400 | Azure hesabı için 20.000 istek sınırını aşıyor. Lütfen yenilerini göndermeden önce bazı istekleri kaldırın. | Sunucu, her bir Azure hesabı için en fazla 20.000 istek tutar. Yeni bir istek göndermeden önce bazı istekleri silin. |
|        | 400 | Bu model şu ses senlimte kullanılamaz: {ModelId}. | {ModelId} durumunun doğru olduğundan emin olun. |
|        | 400 | İsteğin bölgesi şu modelin bölgesiyle eşleşmiyor: {ModelId}. | {ModelId} bölgesinin, isteğin bölgesiyle eşleştiğinden emin olun. |
|        | 400 | Ses birleştirme yalnızca, bayt sırası işaretleyicisi ile UTF-8 Kodlamadaki metin dosyasını destekler. | Giriş dosyalarının, bayt sırası işaretleyicisi ile UTF-8 kodlaması içinde olduğundan emin olun. |
|        | 400 | Ses senssıs isteğinde yalnızca geçerli SSML girdilerine izin verilir. | Giriş SSML ifadelerinin doğru olduğundan emin olun. |
|        | 400 | Giriş dosyasında {voiceName} ses adı bulunamadı. | SSML ses adı girdisi model KIMLIĞIYLE hizalı değil. |
|        | 400 | Giriş dosyasındaki paragraf miktarı 10.000 ' den az olmalıdır. | Dosyadaki paragrafın 10.000 ' dan küçük olduğundan emin olun. |
|        | 400 | Giriş dosyası 400 karakterden uzun olmalıdır. | Giriş dosyanızın 400 karakteri aştığından emin olun. |
|        | 404 | Voice sensııd tanımında belirtilen model bulunamıyor: {ModelId}. | {ModelId} ' ın doğru olduğundan emin olun. |
|        | 429 | Etkin ses sensimi sınırını aşıyor. Lütfen bazı isteklerin bitmesini bekleyin. | Sunucunun her bir Azure hesabı için en fazla 120 istek çalıştırmasına ve sıraya alma yapmasına izin verilir. Lütfen bekleyin ve bazı istekler tamamlanana kadar yeni istek gönderilmesini önleyin. |
| Tümü       | 429 | Çok fazla istek var. | İstemcinin her bir Azure hesabı için saniyede en fazla 5 istek göndermesine izin verilir. Lütfen saniye başına istek miktarını azaltın. |
| Sil    | 400 | Ses birleştirme görevi hala kullanımda. | Yalnızca **Tamamlanan** veya **başarısız**olan istekleri silebilirsiniz. |
| GetById   | 404 | Belirtilen varlık bulunamıyor. | Sen, ID 'nin doğru olduğundan emin olun. |

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

Uzun ses API 'SI, benzersiz uç noktaları olan birden çok bölgede kullanılabilir.

| Bölge | Uç Nokta |
|--------|----------|
| Doğu Avustralya | `https://australiaeast.customvoice.api.speech.microsoft.com` |
| Orta Kanada | `https://canadacentral.customvoice.api.speech.microsoft.com` |
| Doğu ABD | `https://eastus.customvoice.api.speech.microsoft.com` |
| Hindistan Orta | `https://centralindia.customvoice.api.speech.microsoft.com` |
| Orta Güney ABD | `https://southcentralus.customvoice.api.speech.microsoft.com` |
| Güneydoğu Asya | `https://southeastasia.customvoice.api.speech.microsoft.com` |
| Güney Birleşik Krallık | `https://uksouth.customvoice.api.speech.microsoft.com` |
| Batı Avrupa | `https://westeurope.customvoice.api.speech.microsoft.com` |
| Batı ABD 2 | `https://westus2.customvoice.api.speech.microsoft.com` |

## <a name="audio-output-formats"></a>Ses çıkış biçimleri

Esnek ses çıkışı biçimlerini destekliyoruz. ' ConcatenateResult ' parametresini ayarlayarak, paragraf başına ses çıkışları oluşturabilir veya ses çıkışlarını tek bir çıkışa birleştirebilirsiniz. Aşağıdaki ses çıkış biçimleri, uzun ses API 'SI tarafından desteklenir:

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
* [Örnek kod: C #](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Örnek kod: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
