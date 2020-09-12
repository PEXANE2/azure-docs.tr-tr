---
title: Bilişsel Hizmetler kapsayıcı görüntüsü etiketleri
titleSuffix: Azure Cognitive Services
description: Tüm bilişsel hizmet kapsayıcısı resim etiketlerinin kapsamlı bir listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 08/31/2020
ms.author: aahi
ms.openlocfilehash: 4a98007917e95b1880661fe35bfd3f85f633e9a3
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321778"
---
# <a name="azure-cognitive-services-container-image-tags"></a>Azure bilişsel hizmetler kapsayıcı görüntüsü etiketleri

Azure bilişsel hizmetler birçok kapsayıcı görüntüsü sunar. Kapsayıcı kayıt defterleri ve ilgili depolar kapsayıcı görüntüleri arasında farklılık gösterir. Her kapsayıcı görüntüsü adı birden çok etiket sunar. Kapsayıcı görüntüsü etiketi, kapsayıcı görüntüsünü sürüm oluşturma mekanizmasından oluşur. Bu makale, tüm bilişsel hizmetler kapsayıcı görüntülerini ve bunların kullanılabilir etiketlerini listelemek için kapsamlı bir başvuru olarak kullanılmak üzere tasarlanmıştır.

> [!TIP]
> Kullanırken [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) , **büyük/küçük harfe duyarlı**olduğu için kapsayıcı kayıt defteri, depo, kapsayıcı görüntüsü adı ve karşılık gelen etiket büyük küçük harfe yakın bir ilgi ödeyin.

## <a name="anomaly-detector"></a>Anomali Algılayıcısı

[Anomali algılayıcı][ad-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `anomaly-detector` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |

## <a name="computer-vision"></a>Görüntü İşleme

[Görüntü işleme][cv-containers] okuma OCR kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-read` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-read` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest ( (2.0.013250001-amd64-preview)` | • Kapsayıcı için bellek kullanımını daha da azaltın. |
|                                          | • Çoklu pods kurulumu için dış önbellek gereklidir. Örneğin, önbelleği önbelleğe alma için ayarlama Reddır. |
|                                          | • Redsıs Cache ayarlandığında ve ResultExpirationPeriod = 0 olduğunda sonuçlarda sorun giderme sorunu yok.  |
|                                          | • 26MB için istek gövdesi boyut sınırlamasını kaldır. Kapsayıcı artık >26MB dosyalarını kabul edebilir.  |
|                                          | • Konsol günlüğüne zaman damgası ve derleme sürümü ekleyin.  |
| `1.1.013050001-amd64-preview`            | * Sistemin tanınma sonuçlarını temizlemesi gerektiğinde belirtmek için ReadEngineConfig: ResultExpirationPeriod kapsayıcı başlatma yapılandırması eklendi. |
|                                          | Ayar saat cinsinden ve varsayılan değer 48hr ' dir.   |
|                                          |   Bu ayar, özellikle kapsayıcı bellek içi depolama kullanılırken sonuç depolamaya yönelik bellek kullanımını azaltabilir.  |
|                                          |    * Örnek 1. ReadEngineConfig: ResultExpirationPeriod = 1, sistem, işlemden sonraki tanınma sonucunu 1hr olarak temizler.   |
|                                          |    * Örnek 2. ReadEngineConfig: ResultExpirationPeriod = 0, sistem, sonuç alımı sonrasında tanınma sonucunu temizler.  |
|                                          | Sisteme geçersiz görüntü biçimi geçirildiğinde 500 Iç sunucu hatası düzeltildi. Şimdi 400 hatası döndürür:   |
|                                          | `{`  |
|                                          | `"error": {`  |
|                                          |      `"code": "InvalidImageSize",`  |
|                                          |      `"message": "Image must be between 1024 and 209715200 bytes."`  |
|                                          |          `}`  |
|                                          | `}`  |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

## <a name="face"></a>Yüz Tanıma

[Yüz][fa-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-face` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-face` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |
| `1.1.006490002-amd64-preview` |       |
| `1.0.005940002-amd64-preview` |       |
| `1.0.005550001-amd64-preview` |       |

## <a name="form-recognizer"></a>Form Tanıma

[Form tanıyıcı][fr-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-form-recognizer` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

[Lusıs][lu-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `luis` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/luis` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.010330004-amd64-preview` |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.008010002-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.007020001-amd64-preview` |       |

## <a name="custom-speech-to-text"></a>Özel Konuşma Tanıma metin

[Özel konuşma tanıma metin][sp-cstt] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-custom-speech-to-text` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri            | Notlar |
|-----------------------|:------|
| `latest`              |       |
| `2.3.1-amd64-preview` |       | 
| `2.3.0-amd64-preview` |       |
| `2.2.0-amd64-preview` |       |
| `2.1.1-amd64-preview` |       |
| `2.1.0-amd64-preview` |       |
| `2.0.2-amd64-preview` |       |
| `2.0.0-amd64-preview` |       |

## <a name="custom-text-to-speech"></a>Özel metin okuma

[Özel metin okuma][sp-ctts] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-custom-text-to-speech` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri            | Notlar |
|-----------------------|:------|
| `latest`              |       |
| `1.6.0-amd64-preview` |       |
| `1.5.0-amd64-preview` |       |
| `1.4.0-amd64-preview` |       |
| `1.3.0-amd64-preview` |       |

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

[Konuşmadan metne][sp-stt] kapsayıcı görüntüsü, `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-speech-to-text` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` .
Konuşmadan metne görüntülerinin *ABD devlet Virginia* sürümü v 2.4.0 'te desteklenir ve üzerinde bulunabilir `containerpreview.azurecr.io/microsoft/fairfax/cognitive-services-speech-to-text` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                  | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `latest`                    | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.4.0-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.4.0-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.4.0-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.4.0-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.4.0-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.4.0-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.4.0-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.4.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.4.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.4.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.4.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.4.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.4.0-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.4.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.4.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.4.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.4.0-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.4.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.4.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.4.0-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.4.0-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.4.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.4.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.4.0-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.4.0-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.4.0-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.4.0-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.4.0-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.4.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.4.0-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.4.0-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.4.0-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.4.0-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.4.0-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.4.0-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.4.0-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.4.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.4.0-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.4.0-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.3.1-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.3.1-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.3.1-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.3.1-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.3.1-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.3.1-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.3.1-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.3.1-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.3.1-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.3.1-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.3.1-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.3.1-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.3.1-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.3.1-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.3.1-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.3.1-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.3.1-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.3.1-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.3.1-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.3.1-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.3.1-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.3.1-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.3.1-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.3.1-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.3.1-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.3.1-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.3.1-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.3.1-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.3.1-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.3.1-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.3.1-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.3.1-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.3.1-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.3.1-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.3.1-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.3.1-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.3.1-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.3.1-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.3.1-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.3.0-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.3.0-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.3.0-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.3.0-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.3.0-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.3.0-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.3.0-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.3.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.3.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.3.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.3.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.3.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.3.0-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.3.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.3.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.3.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.3.0-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.3.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.3.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.3.0-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.3.0-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.3.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.3.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.3.0-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.3.0-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.3.0-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.3.0-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.3.0-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.3.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.3.0-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.3.0-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.3.0-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.3.0-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.3.0-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.3.0-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.3.0-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.3.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.3.0-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.3.0-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.2.0-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.2.0-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.2.0-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.2.0-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.2.0-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.2.0-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.2.0-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.2.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.2.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.2.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.2.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.2.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.2.0-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.2.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.2.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.2.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.2.0-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.2.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.2.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.2.0-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.2.0-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.2.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.2.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.2.0-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.2.0-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.2.0-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.2.0-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.2.0-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.2.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.2.0-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.2.0-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.2.0-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.2.0-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.2.0-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.2.0-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.2.0-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.2.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.2.0-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.2.0-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.1.1-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.1.1-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.1.1-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.1.1-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.1.1-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.1.1-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.1.1-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.1.1-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.1.1-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.1.1-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.1.1-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.1.1-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.1.1-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.1.1-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.1.1-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.1.1-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.1.1-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.1.1-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.1.1-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.1.1-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.1.1-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.1.1-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.1.1-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.1.1-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.1.1-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.1.1-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.1.1-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.1.1-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.1.1-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.1.1-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.1.1-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.1.1-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.1.1-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.1.1-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.1.1-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.1.1-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.1.1-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.1.1-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.1.1-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.1.1-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.1.0-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.1.0-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.1.0-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.1.0-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.1.0-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.1.0-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.1.0-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.1.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.1.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.1.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.1.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.1.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.1.0-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.1.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.1.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.1.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.1.0-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.1.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.1.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.1.0-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.1.0-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.1.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.1.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.1.0-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.1.0-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.1.0-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.1.0-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.1.0-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.1.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.1.0-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.1.0-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.1.0-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.1.0-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.1.0-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.1.0-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.1.0-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.1.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.1.0-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.1.0-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.0.3-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.0.2-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.0.2-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.0.2-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.0.2-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.0.2-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.0.2-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.0.2-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.0.2-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.0.2-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.0.2-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.0.2-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.0.2-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.0.2-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.0.2-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.0.2-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.0.2-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.0.2-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.0.2-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.0.2-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.0.2-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.0.2-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.0.2-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.0.2-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.0.2-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.0.2-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.0.2-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.0.2-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.0.2-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.0.2-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.0.2-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.0.2-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.0.2-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.0.2-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.0.2-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.0.2-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.0.2-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.0.2-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.0.2-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.0.2-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.0.1-amd64-ar-ae-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `2.0.1-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.0.1-amd64-ar-kw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `2.0.1-amd64-ar-qa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `2.0.1-amd64-ar-sa-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `2.0.1-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.0.1-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.0.1-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.0.1-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.0.1-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.0.1-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.0.1-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.0.1-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.0.1-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.0.1-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.0.1-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.0.1-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.0.1-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.0.1-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.0.1-amd64-gu-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `2.0.1-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.0.1-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.0.1-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.0.1-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.0.1-amd64-mr-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `2.0.1-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.0.1-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.0.1-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.0.1-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.0.1-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.0.1-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.0.1-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.0.1-amd64-ta-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `2.0.1-amd64-te-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `2.0.1-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.0.1-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.0.1-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.0.1-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.0.1-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `2.0.0-amd64-ar-eg-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `2.0.0-amd64-ca-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `2.0.0-amd64-da-dk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `2.0.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `2.0.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `2.0.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `2.0.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `2.0.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `2.0.0-amd64-en-nz-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `2.0.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `2.0.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `2.0.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `2.0.0-amd64-fi-fi-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `2.0.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `2.0.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `2.0.0-amd64-hi-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `2.0.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `2.0.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `2.0.0-amd64-ko-kr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `2.0.0-amd64-nb-no-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `2.0.0-amd64-nl-nl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `2.0.0-amd64-pl-pl-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `2.0.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `2.0.0-amd64-pt-pt-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `2.0.0-amd64-ru-ru-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `2.0.0-amd64-sv-se-preview` | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `2.0.0-amd64-th-th-preview` | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `2.0.0-amd64-tr-tr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `2.0.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `2.0.0-amd64-zh-hk-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `2.0.0-amd64-zh-tw-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |
| `1.2.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.2.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.2.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.2.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.2.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.2.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.2.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.2.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.2.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.2.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.2.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.2.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.2.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.2.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `1.1.3-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.1.3-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.1.3-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.1.3-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.1.3-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.1.3-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.1.3-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.1.3-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.1.3-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.1.3-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.1.3-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.1.3-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.1.3-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.1.3-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `1.1.2-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.1.2-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.1.2-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.1.2-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.1.2-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.1.2-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.1.2-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.1.2-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.1.2-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.1.2-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.1.2-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.1.2-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.1.2-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.1.2-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `1.1.1-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.1.1-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.1.1-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.1.1-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.1.1-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.1.1-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.1.1-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.1.1-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.1.1-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.1.1-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.1.1-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.1.1-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.1.1-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.1.1-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `1.1.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.1.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.1.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.1.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.1.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.1.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.1.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.1.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.1.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.1.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.1.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.1.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.1.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.1.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `1.0.0-amd64-de-de-preview` | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `1.0.0-amd64-en-au-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `1.0.0-amd64-en-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `1.0.0-amd64-en-gb-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `1.0.0-amd64-en-in-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `1.0.0-amd64-en-us-preview` | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `1.0.0-amd64-es-es-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `1.0.0-amd64-es-mx-preview` | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `1.0.0-amd64-fr-ca-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `1.0.0-amd64-fr-fr-preview` | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `1.0.0-amd64-it-it-preview` | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `1.0.0-amd64-ja-jp-preview` | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `1.0.0-amd64-pt-br-preview` | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `1.0.0-amd64-zh-cn-preview` | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |

## <a name="text-to-speech"></a>Metin okuma

[Metin okuma][sp-tts] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-text-to-speech` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-ar-eg-hoda-preview`            | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `1.6.0-amd64-ar-sa-naayf-preview`           | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-bg-bg-ivan-preview`            | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `1.6.0-amd64-ca-es-herenarus-preview`       | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-cs-cz-jakub-preview`           | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-da-dk-hellerus-preview`        | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-de-at-michael-preview`         | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-de-ch-karsten-preview`         | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-el-gr-stefanos-preview`        | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-en-ca-heatherrus-preview`      | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `1.6.0-amd64-en-ca-linda-preview`           | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-en-ie-sean-preview`            | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `1.6.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-en-us-aria24krus-preview`      | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-en-us-ariarus-preview`         | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-fi-fi-heidirus-preview`        | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-fr-ch-guillaume-preview`       | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.6.0-amd64-he-il-asaf-preview`            | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `1.6.0-amd64-hi-in-hemant-preview`          | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `1.6.0-amd64-hi-in-kalpana-apollo-preview`  | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `1.6.0-amd64-hi-in-kalpana-preview`         | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-hr-hr-matej-preview`           | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-hu-hu-szabolcs-preview`        | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-id-id-andika-preview`          | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `1.6.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-ms-my-rizwan-preview`          | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `1.6.0-amd64-nb-no-huldarus-preview`        | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-nl-nl-hannarus-preview`        | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-pl-pl-paulinarus-preview`      | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `1.6.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.6.0-amd64-pt-pt-heliarus-preview`        | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-ro-ro-andrei-preview`          | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `1.6.0-amd64-ru-ru-ekaterinarus-preview`    | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-ru-ru-irina-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-ru-ru-pavel-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-sk-sk-filip-preview`           | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `1.6.0-amd64-sl-si-lado-preview`            | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `1.6.0-amd64-sv-se-hedvigrus-preview`       | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-ta-in-valluvar-preview`        | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-te-in-chitra-preview`          | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `1.6.0-amd64-th-th-pattara-preview`         | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-tr-tr-sedarus-preview`         | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `1.6.0-amd64-vi-vn-an-preview`              | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `1.6.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.6.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-zh-hk-danny-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-zh-hk-tracy-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.6.0-amd64-zh-hk-tracyrus-preview`        | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `1.6.0-amd64-zh-tw-hanhanrus-preview`       | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `1.6.0-amd64-zh-tw-yating-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.6.0-amd64-zh-tw-zhiwei-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-ar-eg-hoda-preview`            | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `1.5.0-amd64-ar-sa-naayf-preview`           | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-bg-bg-ivan-preview`            | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `1.5.0-amd64-ca-es-herenarus-preview`       | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-cs-cz-jakub-preview`           | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-da-dk-hellerus-preview`        | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-de-at-michael-preview`         | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-de-ch-karsten-preview`         | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-el-gr-stefanos-preview`        | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-en-ca-heatherrus-preview`      | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `1.5.0-amd64-en-ca-linda-preview`           | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-en-ie-sean-preview`            | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `1.5.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-en-us-aria24krus-preview`      | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-en-us-ariarus-preview`         | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-fi-fi-heidirus-preview`        | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-fr-ch-guillaume-preview`       | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.5.0-amd64-he-il-asaf-preview`            | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `1.5.0-amd64-hi-in-hemant-preview`          | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `1.5.0-amd64-hi-in-kalpana-apollo-preview`  | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `1.5.0-amd64-hi-in-kalpana-preview`         | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-hr-hr-matej-preview`           | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-hu-hu-szabolcs-preview`        | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-id-id-andika-preview`          | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `1.5.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-ms-my-rizwan-preview`          | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `1.5.0-amd64-nb-no-huldarus-preview`        | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-nl-nl-hannarus-preview`        | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-pl-pl-paulinarus-preview`      | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `1.5.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.5.0-amd64-pt-pt-heliarus-preview`        | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-ro-ro-andrei-preview`          | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `1.5.0-amd64-ru-ru-ekaterinarus-preview`    | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-ru-ru-irina-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-ru-ru-pavel-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-sk-sk-filip-preview`           | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `1.5.0-amd64-sl-si-lado-preview`            | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `1.5.0-amd64-sv-se-hedvigrus-preview`       | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-ta-in-valluvar-preview`        | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-te-in-chitra-preview`          | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `1.5.0-amd64-th-th-pattara-preview`         | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-tr-tr-sedarus-preview`         | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `1.5.0-amd64-vi-vn-an-preview`              | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `1.5.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.5.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-zh-hk-danny-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-zh-hk-tracy-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.5.0-amd64-zh-hk-tracyrus-preview`        | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `1.5.0-amd64-zh-tw-hanhanrus-preview`       | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `1.5.0-amd64-zh-tw-yating-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.5.0-amd64-zh-tw-zhiwei-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-ar-eg-hoda-preview`            | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `1.4.0-amd64-ar-sa-naayf-preview`           | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-bg-bg-ivan-preview`            | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `1.4.0-amd64-ca-es-herenarus-preview`       | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-cs-cz-jakub-preview`           | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-da-dk-hellerus-preview`        | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-de-at-michael-preview`         | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-de-ch-karsten-preview`         | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-el-gr-stefanos-preview`        | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-en-ca-heatherrus-preview`      | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `1.4.0-amd64-en-ca-linda-preview`           | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-en-ie-sean-preview`            | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `1.4.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-en-us-aria24krus-preview`      | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-en-us-ariarus-preview`         | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-fi-fi-heidirus-preview`        | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-fr-ch-guillaume-preview`       | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.4.0-amd64-he-il-asaf-preview`            | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `1.4.0-amd64-hi-in-hemant-preview`          | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `1.4.0-amd64-hi-in-kalpana-apollo-preview`  | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `1.4.0-amd64-hi-in-kalpana-preview`         | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-hr-hr-matej-preview`           | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-hu-hu-szabolcs-preview`        | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-id-id-andika-preview`          | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `1.4.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-ms-my-rizwan-preview`          | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `1.4.0-amd64-nb-no-huldarus-preview`        | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-nl-nl-hannarus-preview`        | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-pl-pl-paulinarus-preview`      | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `1.4.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.4.0-amd64-pt-pt-heliarus-preview`        | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-ro-ro-andrei-preview`          | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `1.4.0-amd64-ru-ru-ekaterinarus-preview`    | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-ru-ru-irina-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-ru-ru-pavel-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-sk-sk-filip-preview`           | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `1.4.0-amd64-sl-si-lado-preview`            | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `1.4.0-amd64-sv-se-hedvigrus-preview`       | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-ta-in-valluvar-preview`        | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-te-in-chitra-preview`          | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `1.4.0-amd64-th-th-pattara-preview`         | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-tr-tr-sedarus-preview`         | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `1.4.0-amd64-vi-vn-an-preview`              | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `1.4.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.4.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-zh-hk-danny-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-zh-hk-tracy-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.4.0-amd64-zh-hk-tracyrus-preview`        | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `1.4.0-amd64-zh-tw-hanhanrus-preview`       | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `1.4.0-amd64-zh-tw-yating-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.4.0-amd64-zh-tw-zhiwei-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-ar-eg-hoda-preview`            | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-ar-sa-naayf-preview`           | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-bg-bg-ivan-preview`            | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-ca-es-herenarus-preview`       | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-cs-cz-jakub-preview`           | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-da-dk-hellerus-preview`        | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-de-at-michael-preview`         | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-de-ch-karsten-preview`         | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-el-gr-stefanos-preview`        | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-ca-heatherrus-preview`      | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-en-ca-linda-preview`           | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-en-ie-sean-preview`            | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-us-jessa24krus-preview`     | `en-US`Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-jessarus-preview`        | `en-US`Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fi-fi-heidirus-preview`        | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fr-ch-guillaume-preview`       | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-he-il-asaf-preview`            | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-hi-in-hemant-preview`          | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-hi-in-kalpana-preview`         | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-hi-in-kalpana-preview`         | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-hr-hr-matej-preview`           | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-hu-hu-szabolcs-preview`        | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-id-id-andika-preview`          | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ms-my-rizwan-preview`          | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-nb-no-huldarus-preview`        | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-nl-nl-hannarus-preview`        | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-pl-pl-paulinarus-preview`      | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-pt-pt-heliarus-preview`        | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ro-ro-andrei-preview`          | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-ru-ru-ekaterinarus-preview`    | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ru-ru-irina-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ru-ru-pavel-apollo-preview`    | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-sk-sk-filip-preview`           | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-sl-si-lado-preview`            | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-sv-se-hedvigrus-preview`       | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-ta-in-valluvar-preview`        | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-te-in-chitra-preview`          | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-th-th-pattara-preview`         | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-tr-tr-sedarus-preview`         | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-vi-vn-an-preview`              | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `1.3.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.3.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-zh-hk-danny-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-zh-hk-tracy-apollo-preview`    | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-zh-hk-tracyrus-preview`        | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-zh-tw-hanhanrus-preview`       | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-zh-tw-yating-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-zh-tw-zhiwei-apollo-preview`   | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.2.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-us-jessa24krus-preview`     | `en-US`Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-jessarus-preview`        | `en-US`Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.2.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.2.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.2.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-de-de-hedda-preview`           | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | `de-DE`Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-de-de-stefan-apollo-preview`   | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-en-au-catherine-preview`       | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-au-hayleyrus-preview`       | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-gb-george-apollo-preview`   | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-en-gb-hazelrus-preview`        | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-gb-susan-apollo-preview`    | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-en-in-heera-apollo-preview`    | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-en-in-priyarus-preview`        | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-in-ravi-apollo-preview`     | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-us-jessa24krus-preview`     | `en-US`Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-jessarus-preview`        | `en-US`Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.1.0-amd64-es-es-helenarus-preview`       | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-es-es-laura-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-es-es-pablo-apollo-preview`    | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-es-mx-hildarus-preview`        | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-es-mx-raul-apollo-preview`     | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-ca-caroline-preview`        | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-it-it-luciarus-preview`        | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-ja-jp-harukarus-preview`       | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-ko-kr-heamirus-preview`        | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-pt-br-heloisarus-preview`      | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.1.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.1.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.0.0-amd64-en-us-benjaminrus-preview`     | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.0.0-amd64-en-us-guy24krus-preview`       | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.0.0-amd64-en-us-jessa24krus-preview`     | `en-US`Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.0.0-amd64-en-us-jessarus-preview`        | `en-US`Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.0.0-amd64-en-us-zirarus-preview`         | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.0.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.0.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.0.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |

## <a name="neural-text-to-speech"></a>Sinir metin okuma

[Sinir Text-to-Speech] [SP-NTTS] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. Depo içinde bulunur `microsoft` ve olarak adlandırılır `cognitive-services-neural-text-to-speech` . Tam kapsayıcı görüntü adı, olur `containerpreview.azurecr.io/microsoft/cognitive-services-neural-text-to-speech` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `1.1.0-amd64-de-de-katjaneural-preview`     | `de-DE`Yerel ayar ve `de-DE-KatjaNeural` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-au-natashaneural-preview`   | `en-AU`Yerel ayar ve `en-AU-NatashaNeural` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-en-ca-claraneural-preview`     | `en-CA`Yerel ayar ve `en-CA-ClaraNeural` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-gb-libbyneural-preview`     | `en-GB`Yerel ayar ve `en-GB-LibbyNeural` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-gb-mianeural-preview`       | `en-GB`Yerel ayar ve `en-GB-MiaNeural` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-us-arianeural-preview`      | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `1.1.0-amd64-en-us-guyneural-preview`       | `en-US`Yerel ayar ve `en-US-GuyNeural` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-es-es-elviraneural-preview`    | `es-ES`Yerel ayar ve `es-ES-ElviraNeural` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-es-mx-dalianeural-preview`     | `es-MX`Yerel ayar ve `es-MX-DaliaNeural` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-ca-sylvieneural-preview`    | `fr-CA`Yerel ayar ve `fr-CA-SylvieNeural` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-fr-fr-deniseneural-preview`    | `fr-FR`Yerel ayar ve `fr-FR-DeniseNeural` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-it-it-elsaneural-preview`      | `it-IT`Yerel ayar ve `it-IT-ElsaNeural` sesle kapsayıcı görüntüsü.      |
| `1.1.0-amd64-ja-jp-nanamineural-preview`    | `ja-JP`Yerel ayar ve `ja-JP-NanamiNeural` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-ko-kr-sunhineural-preview`     | `ko-KR`Yerel ayar ve `ko-KR-SunHiNeural` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-pt-br-franciscaneural-preview` | `pt-BR`Yerel ayar ve `pt-BR-FranciscaNeural` sesle kapsayıcı görüntüsü. |
| `1.1.0-amd64-zh-cn-xiaoxiaoneural-preview`  | `zh-CN`Yerel ayar ve `zh-CN-XiaoxiaoNeural` sesle kapsayıcı görüntüsü.  |

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama

[Anahtar ifade ayıklama][ta-kp] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `keyphrase` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/keyphrase` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="language-detection"></a>Dil Algılama

[Dil algılama][ta-la] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `language` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/language` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="sentiment-analysis"></a>Yaklaşım Analizi

[Yaklaşım Analizi][ta-se] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `sentiment` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/sentiment` .

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

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
| `1.1.009301-amd64-preview`    | Yaklaşım Analizi v2      |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
