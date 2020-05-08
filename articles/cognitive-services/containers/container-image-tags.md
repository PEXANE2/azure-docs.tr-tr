---
title: Bilişsel Hizmetler kapsayıcı görüntüsü etiketleri
titleSuffix: Azure Cognitive Services
description: Tüm bilişsel hizmet kapsayıcısı resim etiketlerinin kapsamlı bir listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 850d7b9732ad547d502b042e7eaad5d3b471030e
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82926009"
---
# <a name="azure-cognitive-services-container-image-tags"></a>Azure bilişsel hizmetler kapsayıcı görüntüsü etiketleri

Azure bilişsel hizmetler birçok kapsayıcı görüntüsü sunar. Kapsayıcı kayıt defterleri ve ilgili depolar kapsayıcı görüntüleri arasında farklılık gösterir. Her kapsayıcı görüntüsü adı birden çok etiket sunar. Kapsayıcı görüntüsü etiketi, kapsayıcı görüntüsünü sürüm oluşturma mekanizmasından oluşur. Bu makale, tüm bilişsel hizmetler kapsayıcı görüntülerini ve bunların kullanılabilir etiketlerini listelemek için kapsamlı bir başvuru olarak kullanılmak üzere tasarlanmıştır.

> [!TIP]
> Kullanırken [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), **büyük/küçük harfe duyarlı**olduğu için kapsayıcı kayıt defteri, depo, kapsayıcı görüntüsü adı ve karşılık gelen etiket büyük küçük harfe yakın bir ilgi ödeyin.

## <a name="anomaly-detector"></a>Anomali Algılayıcısı

[Anomali algılayıcı][ad-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-anomaly-detector`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008500001-amd64-preview` |       |

## <a name="computer-vision"></a>Görüntü İşleme

[Görüntü işleme][cv-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-read`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-read`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

## <a name="face"></a>Yüz

[Yüz][fa-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-face`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-face`olur.

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

[Form tanıyıcı][fr-containers] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-form-recognizer`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

[Lusıs][lu-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. `azure-cognitive-services` Depo içinde bulunur ve olarak adlandırılır `luis`. Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/luis`olur.

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

[Özel konuşma tanıma metin][sp-cstt] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-custom-speech-to-text`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri            | Notlar |
|-----------------------|:------|
| `latest`              |       |
| `2.2.0-amd64-preview` |       |
| `2.1.1-amd64-preview` |       |
| `2.1.0-amd64-preview` |       |
| `2.0.2-amd64-preview` |       |
| `2.0.0-amd64-preview` |       |

## <a name="custom-text-to-speech"></a>Özel metin okuma

[Özel metin okuma][sp-ctts] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-custom-text-to-speech`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri            | Notlar |
|-----------------------|:------|
| `latest`              |       |
| `1.3.0-amd64-preview` |       |

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

[Konuşmadan metne][sp-stt] kapsayıcı görüntüsü, `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-speech-to-text`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                  | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `latest`                    | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ar-ae-preview` | `ar-AE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ar-kw-preview` | `ar-KW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ar-qa-preview` | `ar-QA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ar-sa-preview` | `ar-SA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-gu-in-preview` | `gu-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-mr-in-preview` | `mr-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-ta-in-preview` | `ta-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-te-in-preview` | `te-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.2.0-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ar-ae-preview` | `ar-AE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ar-kw-preview` | `ar-KW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ar-qa-preview` | `ar-QA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ar-sa-preview` | `ar-SA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-gu-in-preview` | `gu-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-mr-in-preview` | `mr-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-ta-in-preview` | `ta-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-te-in-preview` | `te-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.1-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ar-ae-preview` | `ar-AE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ar-kw-preview` | `ar-KW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ar-qa-preview` | `ar-QA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ar-sa-preview` | `ar-SA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-gu-in-preview` | `gu-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-mr-in-preview` | `mr-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-ta-in-preview` | `ta-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-te-in-preview` | `te-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.1.0-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.3-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ar-ae-preview` | `ar-AE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ar-kw-preview` | `ar-KW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ar-qa-preview` | `ar-QA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ar-sa-preview` | `ar-SA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-gu-in-preview` | `gu-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-mr-in-preview` | `mr-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-ta-in-preview` | `ta-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-te-in-preview` | `te-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.2-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ar-ae-preview` | `ar-AE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ar-kw-preview` | `ar-KW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ar-qa-preview` | `ar-QA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ar-sa-preview` | `ar-SA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-gu-in-preview` | `gu-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-mr-in-preview` | `mr-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-ta-in-preview` | `ta-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-te-in-preview` | `te-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.1-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-ar-eg-preview` | `ar-EG` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-ca-es-preview` | `ca-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-da-dk-preview` | `da-DK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-nz-preview` | `en-NZ` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-fi-fi-preview` | `fi-FI` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-hi-in-preview` | `hi-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-ko-kr-preview` | `ko-KR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-nb-no-preview` | `nb-NO` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-nl-nl-preview` | `nl-NL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-pl-pl-preview` | `pl-PL` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-pt-pt-preview` | `pt-PT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-ru-ru-preview` | `ru-RU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-sv-se-preview` | `sv-SE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-th-th-preview` | `th-TH` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-tr-tr-preview` | `tr-TR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-zh-hk-preview` | `zh-HK` Yerel ayara sahip kapsayıcı görüntüsü. |
| `2.0.0-amd64-zh-tw-preview` | `zh-TW` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.2.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.3-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.2-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.1-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.1.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-de-de-preview` | `de-DE` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-en-au-preview` | `en-AU` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-en-ca-preview` | `en-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-en-gb-preview` | `en-GB` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-en-in-preview` | `en-IN` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-en-us-preview` | `en-US` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-es-es-preview` | `es-ES` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-es-mx-preview` | `es-MX` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-fr-ca-preview` | `fr-CA` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-fr-fr-preview` | `fr-FR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-it-it-preview` | `it-IT` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-ja-jp-preview` | `ja-JP` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-pt-br-preview` | `pt-BR` Yerel ayara sahip kapsayıcı görüntüsü. |
| `1.0.0-amd64-zh-cn-preview` | `zh-CN` Yerel ayara sahip kapsayıcı görüntüsü. |

## <a name="text-to-speech"></a>Metin okuma

[Metin okuma][sp-tts] kapsayıcı görüntüsü `containerpreview.azurecr.io` kapsayıcı kayıt defterinde bulunabilir. `microsoft` Depo içinde bulunur ve olarak adlandırılır `cognitive-services-text-to-speech`. Tam kapsayıcı görüntü adı, `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US` Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ar-eg-hoda-preview`            | `ar-EG` Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-ar-sa-naayf-preview`           | `ar-SA` Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-bg-bg-ivan-preview`            | `bg-BG` Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-ca-es-herenarus-preview`       | `ca-ES` Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-cs-cz-jakub-preview`           | `cs-CZ` Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-da-dk-hellerus-preview`        | `da-DK` Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-de-at-michael-preview`         | `de-AT` Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-de-ch-karsten-preview`         | `de-CH` Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-de-de-hedda-preview`           | `de-DE` Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-de-de-stefan-apollo-preview`   | `de-DE` Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-el-gr-stefanos-preview`        | `el-GR` Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-au-catherine-preview`       | `en-AU` Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-au-hayleyrus-preview`       | `en-AU` Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-ca-heatherrus-preview`      | `en-CA` Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-en-ca-linda-preview`           | `en-CA` Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-en-gb-george-apollo-preview`   | `en-GB` Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-en-gb-hazelrus-preview`        | `en-GB` Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-gb-susan-apollo-preview`    | `en-GB` Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-en-ie-sean-preview`            | `en-IE` Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-en-in-heera-apollo-preview`    | `en-IN` Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-en-in-priyarus-preview`        | `en-IN` Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-in-ravi-apollo-preview`     | `en-IN` Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-benjaminrus-preview`     | `en-US` Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-guy24krus-preview`       | `en-US` Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-en-us-jessa24krus-preview`     | `en-US` Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-en-us-jessarus-preview`        | `en-US` Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-en-us-zirarus-preview`         | `en-US` Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-es-es-helenarus-preview`       | `es-ES` Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-es-es-laura-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-es-es-pablo-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-es-mx-hildarus-preview`        | `es-MX` Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-es-mx-raul-apollo-preview`     | `es-MX` Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fi-fi-heidirus-preview`        | `fi-FI` Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-fr-ca-caroline-preview`        | `fr-CA` Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA` Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fr-ch-guillaume-preview`       | `fr-CH` Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR` Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR` Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR` Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.3.0-amd64-he-il-asaf-preview`            | `he-IL` Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-hi-in-hemant-preview`          | `hi-IN` Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | `hi-IN` Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | `hi-IN` Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-hi-in-kalpana-preview`         | `hi-IN` Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-hr-hr-matej-preview`           | `hr-HR` Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-hu-hu-szabolcs-preview`        | `hu-HU` Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-id-id-andika-preview`          | `id-ID` Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT` Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-it-it-luciarus-preview`        | `it-IT` Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP` Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ja-jp-harukarus-preview`       | `ja-JP` Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP` Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-ko-kr-heamirus-preview`        | `ko-KR` Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ms-my-rizwan-preview`          | `ms-MY` Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-nb-no-huldarus-preview`        | `nb-NO` Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-nl-nl-hannarus-preview`        | `nl-NL` Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-pl-pl-paulinarus-preview`      | `pl-PL` Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR` Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-pt-br-heloisarus-preview`      | `pt-BR` Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-pt-pt-heliarus-preview`        | `pt-PT` Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-ro-ro-andrei-preview`          | `ro-RO` Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-ru-ru-ekaterinarus-preview`    | `ru-RU` Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ru-ru-irina-apollo-preview`    | `ru-RU` Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-ru-ru-pavel-apollo-preview`    | `ru-RU` Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-sk-sk-filip-preview`           | `sk-SK` Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `1.3.0-amd64-sl-si-lado-preview`            | `sl-SI` Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `1.3.0-amd64-sv-se-hedvigrus-preview`       | `sv-SE` Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-ta-in-valluvar-preview`        | `ta-IN` Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-te-in-chitra-preview`          | `te-IN` Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `1.3.0-amd64-th-th-pattara-preview`         | `th-TH` Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-tr-tr-sedarus-preview`         | `tr-TR` Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `1.3.0-amd64-vi-vn-an-preview`              | `vi-VN` Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `1.3.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN` Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN` Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.3.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN` Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-zh-hk-danny-apollo-preview`    | `zh-HK` Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-zh-hk-tracy-apollo-preview`    | `zh-HK` Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.3.0-amd64-zh-hk-tracyrus-preview`        | `zh-HK` Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `1.3.0-amd64-zh-tw-hanhanrus-preview`       | `zh-TW` Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `1.3.0-amd64-zh-tw-yating-apollo-preview`   | `zh-TW` Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.3.0-amd64-zh-tw-zhiwei-apollo-preview`   | `zh-TW` Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.2.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-de-de-stefan-apollo-preview`   | `de-DE` Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-en-au-catherine-preview`       | `en-AU` Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-au-hayleyrus-preview`       | `en-AU` Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-gb-george-apollo-preview`   | `en-GB` Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-en-gb-hazelrus-preview`        | `en-GB` Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-gb-susan-apollo-preview`    | `en-GB` Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-en-in-heera-apollo-preview`    | `en-IN` Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-en-in-priyarus-preview`        | `en-IN` Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-in-ravi-apollo-preview`     | `en-IN` Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-benjaminrus-preview`     | `en-US` Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-guy24krus-preview`       | `en-US` Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-en-us-jessa24krus-preview`     | `en-US` Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-en-us-jessarus-preview`        | `en-US` Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-en-us-zirarus-preview`         | `en-US` Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.2.0-amd64-es-es-helenarus-preview`       | `es-ES` Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-es-es-laura-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-es-es-pablo-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-es-mx-hildarus-preview`        | `es-MX` Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-es-mx-raul-apollo-preview`     | `es-MX` Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-ca-caroline-preview`        | `fr-CA` Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA` Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR` Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR` Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR` Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.2.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT` Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-it-it-luciarus-preview`        | `it-IT` Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP` Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.2.0-amd64-ja-jp-harukarus-preview`       | `ja-JP` Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP` Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-ko-kr-heamirus-preview`        | `ko-KR` Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.2.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR` Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.2.0-amd64-pt-br-heloisarus-preview`      | `pt-BR` Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.2.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN` Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.2.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN` Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.2.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN` Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-de-de-hedda-preview`           | `de-DE` Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | `de-DE` Yerel ayar ve `de-DE-HeddaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-de-de-stefan-apollo-preview`   | `de-DE` Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-en-au-catherine-preview`       | `en-AU` Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-au-hayleyrus-preview`       | `en-AU` Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-gb-george-apollo-preview`   | `en-GB` Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-en-gb-hazelrus-preview`        | `en-GB` Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-gb-susan-apollo-preview`    | `en-GB` Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-en-in-heera-apollo-preview`    | `en-IN` Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-en-in-priyarus-preview`        | `en-IN` Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-in-ravi-apollo-preview`     | `en-IN` Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-benjaminrus-preview`     | `en-US` Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-guy24krus-preview`       | `en-US` Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-en-us-jessa24krus-preview`     | `en-US` Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-en-us-jessarus-preview`        | `en-US` Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-en-us-zirarus-preview`         | `en-US` Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.1.0-amd64-es-es-helenarus-preview`       | `es-ES` Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-es-es-laura-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-es-es-pablo-apollo-preview`    | `es-ES` Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-es-mx-hildarus-preview`        | `es-MX` Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-es-mx-raul-apollo-preview`     | `es-MX` Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-ca-caroline-preview`        | `fr-CA` Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-fr-ca-harmonierus-preview`     | `fr-CA` Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-fr-hortenserus-preview`     | `fr-FR` Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-fr-fr-julie-apollo-preview`    | `fr-FR` Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-fr-fr-paul-apollo-preview`     | `fr-FR` Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `1.1.0-amd64-it-it-cosimo-apollo-preview`   | `it-IT` Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-it-it-luciarus-preview`        | `it-IT` Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-ja-jp-ayumi-apollo-preview`    | `ja-JP` Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `1.1.0-amd64-ja-jp-harukarus-preview`       | `ja-JP` Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-ja-jp-ichiro-apollo-preview`   | `ja-JP` Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-ko-kr-heamirus-preview`        | `ko-KR` Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `1.1.0-amd64-pt-br-daniel-apollo-preview`   | `pt-BR` Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.1.0-amd64-pt-br-heloisarus-preview`      | `pt-BR` Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `1.1.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN` Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.1.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN` Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.1.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN` Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `1.0.0-amd64-en-us-benjaminrus-preview`     | `en-US` Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `1.0.0-amd64-en-us-guy24krus-preview`       | `en-US` Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `1.0.0-amd64-en-us-jessa24krus-preview`     | `en-US` Yerel ayar ve `en-US-Jessa24kRUS` sesle kapsayıcı görüntüsü.     |
| `1.0.0-amd64-en-us-jessarus-preview`        | `en-US` Yerel ayar ve `en-US-JessaRUS` sesle kapsayıcı görüntüsü.        |
| `1.0.0-amd64-en-us-zirarus-preview`         | `en-US` Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `1.0.0-amd64-zh-cn-huihuirus-preview`       | `zh-CN` Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `1.0.0-amd64-zh-cn-kangkang-apollo-preview` | `zh-CN` Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `1.0.0-amd64-zh-cn-yaoyao-apollo-preview`   | `zh-CN` Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama

[Anahtar ifade ayıklama][ta-kp] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. `azure-cognitive-services` Depo içinde bulunur ve olarak adlandırılır `keyphrase`. Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/keyphrase`olur.

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

[Dil algılama][ta-la] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. `azure-cognitive-services` Depo içinde bulunur ve olarak adlandırılır `language`. Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/language`olur.

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

[Yaklaşım Analizi][ta-se] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. `azure-cognitive-services` Depo içinde bulunur ve olarak adlandırılır `sentiment`. Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/sentiment`olur.

Bu kapsayıcı görüntüsünün aşağıdaki etiketleri mevcuttur:

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
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
