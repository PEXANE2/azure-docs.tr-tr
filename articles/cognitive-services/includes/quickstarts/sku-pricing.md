---
title: Bilişsel hizmet SKU 'Ları ve fiyatlandırma
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 8cc4bc6907f83ce062fed82dde17815fc4debd67
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104719755"
---
Aşağıdaki SKU 'ların ve fiyatlandırma bilgilerinin listesine bakın. 

#### <a name="multi-service"></a>Çoklu hizmet

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Birden çok hizmet. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/) sayfasına bakın.            | `CognitiveServices`     |


#### <a name="vision"></a>Görsel

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel Görüntü İşleme tahmin | `CustomVision.Prediction` |
| Özel Görüntü İşleme-eğitim   | `CustomVision.Training`   |
| Yüz                       | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |

#### <a name="search"></a>Arayın

| Hizmet            | Tip                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Konuşma

| Hizmet            | Tip                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

#### <a name="language"></a>Dil

| Hizmet            | Tip                |
|--------------------|---------------------|
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

#### <a name="decision"></a>Karar

| Hizmet           | Tip               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan miktar), kimlik doğrulama bilgilerinizi kullanarak göndereceğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı şunları belirtir:
* saniye başına izin verilen en fazla işlem sayısı (TPS).
* fiyatlandırma katmanında etkinleştirilen hizmet özellikleri.
* önceden tanımlı işlem sayısı için maliyet. Bu sayının üzerine gitmek, hizmetinize ilişkin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtilen ek ücret oluşmasına neden olur.

> [!NOTE]
> Bilişsel hizmetlerin birçoğu hizmeti denemek için kullanabileceğiniz ücretsiz bir katmana sahiptir. Ücretsiz katmanı kullanmak için, `F0` kaynağınız IÇIN SKU olarak kullanın.