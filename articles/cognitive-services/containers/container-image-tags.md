---
title: Bilişsel Hizmetler kapsayıcı görüntüsü etiketleri
titleSuffix: Azure Cognitive Services
description: Tüm bilişsel hizmet kapsayıcısı resim etiketlerinin kapsamlı bir listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/06/2020
ms.author: aahi
ms.openlocfilehash: 0efb278c217ede94f67b47250633622501a24414
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94412553"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure bilişsel hizmetler kapsayıcı görüntü etiketleri ve sürüm notları

Azure bilişsel hizmetler birçok kapsayıcı görüntüsü sunar. Kapsayıcı kayıt defterleri ve ilgili depolar kapsayıcı görüntüleri arasında farklılık gösterir. Her kapsayıcı görüntüsü adı birden çok etiket sunar. Kapsayıcı görüntüsü etiketi, kapsayıcı görüntüsünü sürüm oluşturma mekanizmasından oluşur. Bu makale, tüm bilişsel hizmetler kapsayıcı görüntülerini ve bunların kullanılabilir etiketlerini listelemek için kapsamlı bir başvuru olarak kullanılmak üzere tasarlanmıştır.

> [!TIP]
> Kullanırken [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) , **büyük/küçük harfe duyarlı** olduğu için kapsayıcı kayıt defteri, depo, kapsayıcı görüntüsü adı ve karşılık gelen etiket büyük küçük harfe yakın bir ilgi ödeyin.

## <a name="anomaly-detector"></a>Anomali Algılayıcısı

[Anomali algılayıcı][ad-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/decision` ve olarak adlandırılır `anomaly-detector` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>OCR 'yi oku (optik karakter tanıma)

[Görüntü işleme][cv-containers] okuma OCR kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `read` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/vision/read` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notları `v2.0.013250001-amd64-preview` :

* Kapsayıcı için bellek kullanımını daha da azaltın.
* Çoklu pods kurulumu için dış önbellek gereklidir. Örneğin, önbelleği önbelleğe alma için ayarlama Reddır.
* Redsıs önbelleği ayarlandığında ve `ResultExpirationPeriod` 0 olarak ayarlandığında, eksik sonuçlar düzeltildi.
* 26MB için istek gövdesi boyut sınırlamasını kaldır. Kapsayıcı artık >26MB dosyalarını kabul edebilir.
* Konsol günlüğüne zaman damgası ve derleme sürümü ekleyin.

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `2.0.013250001-amd64-preview` |  |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

İçin sürüm notları `1.1.013050001-amd64-preview`

* `ReadEngineConfig:ResultExpirationPeriod`Sistemin, tanınma sonuçlarını temizlemesi gerektiğinde belirtmek için kapsayıcı başlatma yapılandırması eklendi. 
    * Ayar saat cinsinden ve varsayılan değer 48 saattir.
    * Bu ayar, özellikle kapsayıcı bellek içi depolama kullanılırken sonuç depolamaya yönelik bellek kullanımını azaltabilir.
    * Örnek 1. ReadEngineConfig: ResultExpirationPeriod = 1, sistem, işlemden sonraki tanınma sonucunu 1hr olarak temizler.
    * Bu yapılandırma 0 olarak ayarlandıysa, sonuç alındıktan sonra sistem tanınma sonucunu temizler.

* Sisteme geçersiz bir görüntü biçimi geçirildiğinde 500 iç sunucu hatası düzeltildi. Şimdi 400 hatası döndürür:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Form Tanıma

[Form tanıyıcı][fr-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/custom-form` ve olarak adlandırılır `labeltool` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

[Lusıs][lu-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/language` ve olarak adlandırılır `luis` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/language/luis` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Özel Konuşma Tanıma metin

[Özel konuşma tanıma-metin][sp-cstt] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `custom-speech-to-text` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text` . Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `2.6.0-amd64` :

**Özellikler**
* PhraseList v2 desteği 
* Tümcecik listeleri aşağıdaki yerel ayarlarda desteklenir:
    * En-au
    * en-CA
    * en-GB
    * En-ın
    * En-NZ
    * tr-tr
    * zh-cn
* Özel temel model indirme desteği. 
    * `BaseModelLocale=<locale>` `docker run` Komutuyla kullanın
* .NET 3,1 'ye tam olarak geçirildi

**Düzeltilen**
* Güvenilirlik puanının her zaman bir ayırma modunda 1 olduğu bir sorun düzeltildi
* TextAnalytics 3,0 API 'sine geçirildi

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır.

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `2.6.0-amd64`                 |       |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `2.5.0-amd64` :

**Özellikler**
* Özel modellerdeki özel söylenişi destekleme
* Azure ve Azure ABD kamu bulutunu destekleme

**Düzeltilen**
* Farklı Çalıştır modunda kök olmayan kullanıcı sorunu giderme

| Resim etiketleri                    | Notlar               |
|-------------------------------|:--------------------|
| `2.5.0-amd64`                 |   1. GA sürümü    |

---

## <a name="custom-text-to-speech"></a>Özel metin okuma

[Özel metin okuma][sp-ctts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `custom-text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech` . Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `1.8.0-amd64` :

**Özellikler**
* .NET 3,1 'ye tam olarak geçirildi

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.8.0-amd64`                 |       |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `1.7.0-amd64` :

**Özellik**
* Kısmen .NET 3,1 'e geçirildi

| Resim etiketleri                    | Notlar               |
|-------------------------------|:--------------------|
| `1.7.0-amd64`                 |   1. GA sürümü    |

---

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

[Konuşmadan metne][sp-stt] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `speech-to-text` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` . [Mr 'de etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list)tam listesini bulabilirsiniz.

Konuşmadan metne v 2.5.0 bu yana, *ABD devlet Virginia* bölgesinde görüntüler desteklenir. Lütfen bu bölgeyi kullanırken *ABD devlet Virginia* faturalandırma uç noktasını ve API anahtarlarını kullanın.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `2.6.0-amd64-<locale>` :

**Özellikler**
* En son modellere yükseltildi ve tamamen .NET 3,1 'e geçirildi
* PhraseList v2 desteği
* Tümcecik listeleri aşağıdaki yerel ayarlarda desteklenir:
    * En-au
    * en-CA
    * en-GB
    * En-ın
    * En-NZ
    * tr-tr
    * zh-cn
* Yeni yerel ayar desteği `cs-CZ` 
    * Büyük harfler ve noktalama işaretleri Şu anda desteklenmiyor.

**Düzeltilen**
* Güven puanlarının her zaman bir artırma modunda 1 olduğu bir sorunu düzeltir
* Geçirilmiş TextAnalytics 3,0 API 'sini kullanın

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır. 

| Resim etiketleri                    | Notlar                                                                                                | 
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Yerel ayara sahip kapsayıcı görüntüsü `en-US` .                                                             |
| `2.6.0-amd64-<locale>`        | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `2.6.0-amd64-en-us`. |

Bu kapsayıcıda aşağıdaki yerel ayarlar var.

| V 2.6.0 için yerel ayar           | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `ar-eg`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `ar-kw`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `ar-qa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `ar-sa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `ca-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `cs-cz`                     | Yerel ayara sahip kapsayıcı görüntüsü `cs-CZ` . |
| `da-dk`                     | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `de-de`                     | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `en-au`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `en-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `en-gb`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `en-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `en-nz`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `en-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `es-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `es-mx`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `fi-fi`                     | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `fr-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `fr-fr`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `gu-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `hi-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `it-it`                     | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `ja-jp`                     | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `ko-kr`                     | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `mr-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `nb-no`                     | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `nl-nl`                     | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `pl-pl`                     | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `pt-br`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `pt-pt`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `ru-ru`                     | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `sv-se`                     | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `ta-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `te-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `th-th`                     | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `tr-tr`                     | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `zh-cn`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `zh-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `zh-tw`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `2.5.0-amd64-<locale>` :

**Özellikler**
* Azure US kamu bulutu desteği

**Düzeltilen**
* Kök olmayan kullanıcı olarak çalışırken bir sorunu düzeltme modunda düzeltir

| Resim etiketleri                  | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `2.5.0-amd64-<locale>`      | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `2.5.0-amd64-en-us`.  |

Bu kapsayıcıda aşağıdaki yerel ayarlar var.

| V 2.5.0 için yerel ayar           | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `ar-eg`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `ar-kw`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `ar-qa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `ar-sa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `ca-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `da-dk`                     | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `de-de`                     | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `en-au`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `en-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `en-gb`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `en-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `en-nz`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `en-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `es-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `es-mx`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `fi-fi`                     | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `fr-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `fr-fr`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `gu-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `hi-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `it-it`                     | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `ja-jp`                     | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `ko-kr`                     | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `mr-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `nb-no`                     | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `nl-nl`                     | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `pl-pl`                     | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `pt-br`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `pt-pt`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `ru-ru`                     | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `sv-se`                     | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `ta-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `te-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `th-th`                     | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `tr-tr`                     | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `zh-cn`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `zh-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `zh-tw`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |

---

## <a name="text-to-speech"></a>Metin okuma

[Metin okuma][sp-tts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `1.8.0-amd64-<locale-and-voice>` :

**Özellik**
* .NET 3,1 'ye tam olarak geçirildi

| Resim etiketleri                                  | Notlar                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.                                            | 
| `1.8.0-amd64-<locale-and-voice>`            | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.8.0-amd64-en-us-ariarus`.  |


| V 1.8.0 için yerel ayarlar                          | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `ar-sa-naayf`                               | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `bg-bg-ivan`                                | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `ca-es-herenarus`                           | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `cs-cz-jakub`                               | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `da-dk-hellerus`                            | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `de-at-michael`                             | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `de-ch-karsten`                             | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `de-de-hedda`                               | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-heddarus`                            | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-stefan-apollo`                       | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `el-gr-stefanos`                            | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `en-au-catherine`                           | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `en-au-hayleyrus`                           | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `en-ca-heatherrus`                          | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `en-ca-linda`                               | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `en-gb-george-apollo`                       | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `en-gb-hazelrus`                            | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `en-gb-susan-apollo`                        | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-ie-sean`                                | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `en-in-heera-apollo`                        | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-in-priyarus`                            | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `en-in-ravi-apollo`                         | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `en-us-benjaminrus`                         | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `en-us-guy24krus`                           | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `en-us-aria24krus`                          | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.      |
| `en-us-ariarus`                             | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         |
| `en-us-zirarus`                             | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `es-es-helenarus`                           | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `es-es-laura-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-es-pablo-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-mx-hildarus`                            | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `es-mx-raul-apollo`                         | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `fi-fi-heidirus`                            | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `fr-ca-caroline`                            | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `fr-ca-harmonierus`                         | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `fr-ch-guillaume`                           | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `fr-fr-hortenserus`                         | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `fr-fr-julie-apollo`                        | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `fr-fr-paul-apollo`                         | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `he-il-asaf`                                | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `hi-in-hemant`                              | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `hi-in-kalpana-apollo`                      | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `hi-in-kalpana`                             | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `hr-hr-matej`                               | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `hu-hu-szabolcs`                            | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `id-id-andika`                              | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `it-it-cosimo-apollo`                       | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `it-it-luciarus`                            | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `ja-jp-ayumi-apollo`                        | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `ja-jp-harukarus`                           | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `ja-jp-ichiro-apollo`                       | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `ko-kr-heamirus`                            | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `ms-my-rizwan`                              | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `nb-no-huldarus`                            | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `nl-nl-hannarus`                            | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `pl-pl-paulinarus`                          | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-br-daniel-apollo`                       | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `pt-br-heloisarus`                          | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-pt-heliarus`                            | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `ro-ro-andrei`                              | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `ru-ru-ekaterinarus`                        | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `ru-ru-irina-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `ru-ru-pavel-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `sk-sk-filip`                               | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `sl-si-lado`                                | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `sv-se-hedvigrus`                           | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `ta-in-valluvar`                            | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `te-in-chitra`                              | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `th-th-pattara`                             | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `tr-tr-sedarus`                             | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `vi-vn-an`                                  | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `zh-cn-huihuirus`                           | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `zh-cn-kangkang-apollo`                     | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `zh-cn-yaoyao-apollo`                       | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-hk-danny-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracy-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracyrus`                            | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `zh-tw-hanhanrus`                           | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `zh-tw-yating-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `1.7.0-amd64-<locale-and-voice>` :

**Özellik**
* Bileşenler .NET 3,1 'ye yükseltildi

| Resim etiketleri                                  | Notlar                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.7.0-amd64-<locale-and-voice>`            | 1. GA sürümü. `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.7.0-amd64-en-us-ariarus`.  |


| V 1.7.0 için yerel ayarlar                          | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `ar-sa-naayf`                               | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `bg-bg-ivan`                                | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `ca-es-herenarus`                           | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `cs-cz-jakub`                               | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `da-dk-hellerus`                            | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `de-at-michael`                             | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `de-ch-karsten`                             | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `de-de-hedda`                               | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-heddarus`                            | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-stefan-apollo`                       | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `el-gr-stefanos`                            | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `en-au-catherine`                           | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `en-au-hayleyrus`                           | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `en-ca-heatherrus`                          | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `en-ca-linda`                               | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `en-gb-george-apollo`                       | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `en-gb-hazelrus`                            | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `en-gb-susan-apollo`                        | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-ie-sean`                                | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `en-in-heera-apollo`                        | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-in-priyarus`                            | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `en-in-ravi-apollo`                         | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `en-us-benjaminrus`                         | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `en-us-guy24krus`                           | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `en-us-aria24krus`                          | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.      |
| `en-us-ariarus`                             | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         |
| `en-us-zirarus`                             | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `es-es-helenarus`                           | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `es-es-laura-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-es-pablo-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-mx-hildarus`                            | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `es-mx-raul-apollo`                         | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `fi-fi-heidirus`                            | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `fr-ca-caroline`                            | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `fr-ca-harmonierus`                         | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `fr-ch-guillaume`                           | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `fr-fr-hortenserus`                         | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `fr-fr-julie-apollo`                        | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `fr-fr-paul-apollo`                         | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `he-il-asaf`                                | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `hi-in-hemant`                              | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `hi-in-kalpana-apollo`                      | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `hi-in-kalpana`                             | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `hr-hr-matej`                               | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `hu-hu-szabolcs`                            | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `id-id-andika`                              | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `it-it-cosimo-apollo`                       | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `it-it-luciarus`                            | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `ja-jp-ayumi-apollo`                        | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `ja-jp-harukarus`                           | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `ja-jp-ichiro-apollo`                       | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `ko-kr-heamirus`                            | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `ms-my-rizwan`                              | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `nb-no-huldarus`                            | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `nl-nl-hannarus`                            | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `pl-pl-paulinarus`                          | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-br-daniel-apollo`                       | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `pt-br-heloisarus`                          | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-pt-heliarus`                            | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `ro-ro-andrei`                              | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `ru-ru-ekaterinarus`                        | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `ru-ru-irina-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `ru-ru-pavel-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `sk-sk-filip`                               | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `sl-si-lado`                                | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `sv-se-hedvigrus`                           | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `ta-in-valluvar`                            | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `te-in-chitra`                              | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `th-th-pattara`                             | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `tr-tr-sedarus`                             | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `vi-vn-an`                                  | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `zh-cn-huihuirus`                           | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `zh-cn-kangkang-apollo`                     | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `zh-cn-yaoyao-apollo`                       | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-hk-danny-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracy-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracyrus`                            | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `zh-tw-hanhanrus`                           | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `zh-tw-yating-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |

---

## <a name="neural-text-to-speech"></a>Sinir metin okuma

[Sinir metin okuma][sp-ntts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `neural-text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list)tam listesini de bulabilirsiniz.

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.2.0-amd64-en-us-arianeural-preview`. |


| v 1.2.0 Preview yerel ayarları ve sesler           | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `de-de-katjaneural-preview`                 | `de-DE`Yerel ayar ve `de-DE-KatjaNeural` sesle kapsayıcı görüntüsü.     |
| `en-au-natashaneural-preview`               | `en-AU`Yerel ayar ve `en-AU-NatashaNeural` sesle kapsayıcı görüntüsü.   |
| `en-ca-claraneural-preview`                 | `en-CA`Yerel ayar ve `en-CA-ClaraNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-libbyneural-preview`                 | `en-GB`Yerel ayar ve `en-GB-LibbyNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-mianeural-preview`                   | `en-GB`Yerel ayar ve `en-GB-MiaNeural` sesle kapsayıcı görüntüsü.       |
| `en-us-arianeural-preview`                  | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `en-us-guyneural-preview`                   | `en-US`Yerel ayar ve `en-US-GuyNeural` sesle kapsayıcı görüntüsü.       |
| `es-es-elviraneural-preview`                | `es-ES`Yerel ayar ve `es-ES-ElviraNeural` sesle kapsayıcı görüntüsü.    |
| `es-mx-dalianeural-preview`                 | `es-MX`Yerel ayar ve `es-MX-DaliaNeural` sesle kapsayıcı görüntüsü.     |
| `fr-ca-sylvieneural-preview`                | `fr-CA`Yerel ayar ve `fr-CA-SylvieNeural` sesle kapsayıcı görüntüsü.    |
| `fr-fr-deniseneural-preview`                | `fr-FR`Yerel ayar ve `fr-FR-DeniseNeural` sesle kapsayıcı görüntüsü.    |
| `it-it-elsaneural-preview`                  | `it-IT`Yerel ayar ve `it-IT-ElsaNeural` sesle kapsayıcı görüntüsü.      |
| `ja-jp-nanamineural-preview`                | `ja-JP`Yerel ayar ve `ja-JP-NanamiNeural` sesle kapsayıcı görüntüsü.    |
| `ko-kr-sunhineural-preview`                 | `ko-KR`Yerel ayar ve `ko-KR-SunHiNeural` sesle kapsayıcı görüntüsü.     |
| `pt-br-franciscaneural-preview`             | `pt-BR`Yerel ayar ve `pt-BR-FranciscaNeural` sesle kapsayıcı görüntüsü. |
| `zh-cn-xiaoxiaoneural-preview`              | `zh-CN`Yerel ayar ve `zh-CN-XiaoxiaoNeural` sesle kapsayıcı görüntüsü.  |

## <a name="speech-language-detection"></a>Konuşma dili algılama

[Konuşma dili algılama][sp-lid] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `language-detection` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list)tam listesini de bulabilirsiniz.

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama

kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `keyphrase` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)


| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Metin dili algılama

[Dil algılama][ta-la] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `language` . Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`


Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-versions"></a>[En son sürümler](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)


| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

[Yaklaşım Analizi][ta-se] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `sentiment` . Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list)tam listesini de bulabilirsiniz.

| Resim etiketleri | Notlar                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Yaklaşım Analizi v3 (Ingilizce)               |
| `3.0-es`   | Yaklaşım Analizi v3 (Ispanyolca)               |
| `3.0-fr`   | Yaklaşım Analizi v3 (Fransızca)                |
| `3.0-it`   | Yaklaşım Analizi v3 (Italyanca)               |
| `3.0-de`   | Yaklaşım Analizi v3 (Almanca)                |
| `3.0-zh`   | Yaklaşım Analizi v3 (Çince-Basitleştirilmiş)  |
| `3.0-zht`  | Yaklaşım Analizi v3 (Çince-Geleneksel) |
| `3.0-ja`   | Yaklaşım Analizi v3 (Japonca)              |
| `3.0-pt`   | Yaklaşım Analizi v3 (Portekizce)            |
| `3.0-nl`   | Yaklaşım Analizi v3 (Felemenkçe)                 |
| `2.1`    | Yaklaşım Analizi v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
