---
title: Metin Moderasyon API'sini kullanarak metni orta düzeye göre - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Çevrimiçi konsoldaki Metin Moderasyonu API'sini kullanarak metin moderasyonunu test edin.
services: cognitive-services
author: PatrickFarley
ms.author: pafarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: ad365c2d4c171105d8dec89d818ef481361d1ff8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272602"
---
# <a name="moderate-text-from-the-api-console"></a>API konsolundan orta metin

Metin içeriğinizi küfür için tarayıp özel ve paylaşılan listelerle karşılaştırmak için Azure İçerik Moderatörü'ndeki [Metin Moderasyon API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) kullanın.

## <a name="get-your-api-key"></a>API anahtarınızı alın

API'yi çevrimiçi konsolda test etmeden önce abonelik anahtarınız gerekir. Bu, **Ayarlar** sekmesinde, **Ocp-Apim-Abonelik-Anahtar** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API başvurusuna gidin

[Metin Moderasyon API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f)gidin. 

  **Metin - Ekran** sayfası açılır.

## <a name="open-the-api-console"></a>API konsolu açma

**API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

  ![Metin - Ekran sayfası bölge seçimi](images/test-drive-region.png)

  **Metin - Ekran** API konsolu açılır.

## <a name="select-the-inputs"></a>Girişleri seçin

### <a name="parameters"></a>Parametreler

Metin ekranınızda kullanmak istediğiniz sorgu parametrelerini seçin. Bu örnekte, **dil**için varsayılan değeri kullanın. İşlem yürütmenin bir parçası olarak olası dili otomatik olarak algılayacaktır, çünkü boş da bırakabilirsiniz.

> [!NOTE]
> **Dil** parametresi için `eng` makine destekli **sınıflandırma** yanıtını (önizleme özelliği) görmek için atayın veya boş bırakın. **Bu özellik yalnızca İngilizce'yi destekler.**
>
> **Küfür terimleri** algılamaiçin, bu makalede listelenen desteklenen dillerin [ISO 639-3 kodunu](http://www-01.sil.org/iso639-3/codes.asp) kullanın veya boş bırakın.

**Otomatik düzeltme**, **KIŞISEL ,** ve **sınıflandırmak (önizleme)** için, **doğru**seçin. **ListId** alanını boş bırakın.

  ![Metin - Ekran konsolu sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü

**İçerik Türü**için, ekrana gelmek istediğiniz içerik türünü seçin. Bu örnekte, varsayılan **metin/düz** içerik türünü kullanın. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Tatmak için örnek metin

İstek **gövde** kutusuna, bazı metin girin. Aşağıdaki örnekte metinde kasıtlı bir yazım hatası gösterilmektedir.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 4255550111, IP: 
255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.
```

## <a name="analyze-the-response"></a>Yanıtı analiz edin

Aşağıdaki yanıt, API'den gelen çeşitli görüşleri gösterir. Potansiyel küfür, kişisel veriler, sınıflandırma (önizleme) ve otomatik olarak düzeltilmiş sürümü içerir.

> [!NOTE]
> Makine destekli 'Sınıflandırma' özelliği önizlemededir ve yalnızca İngilizce'yi destekler.

```json
{
   "original_text":"Is this a grabage or crap email abcdef@abcd.com, phone: 
   6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.",
   "normalized_text":"   grabage  crap email abcdef@abcd.com, phone: 
   6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.",
   "auto_corrected_text":"Is this a garbage or crap email abcdef@abcd.com, phone: 
   6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.",
   "status":{
      "code":3000,
      "description":"OK"
   },
   "pii":{
      "email":[
         {
            "detected":"abcdef@abcd.com",
            "sub_type":"Regular",
            "text":"abcdef@abcd.com",
            "index":32
         }
      ],
      "ssn":[

      ],
      "ipa":[
         {
            "sub_type":"IPV4",
            "text":"255.255.255.255",
            "index":72
         }
      ],
      "phone":[
         {
            "country_code":"US",
            "text":"6657789887",
            "index":56
         }
      ],
      "address":[
         {
            "text":"1 Microsoft Way, Redmond, WA 98052",
            "index":89
         }
      ]
   },
   "language":"eng",
   "terms":[
      {
         "index":12,
         "original_index":21,
         "list_id":0,
         "term":"crap"
      }
   ],
   "tracking_id":"WU_ibiza_65a1016d-0f67-45d2-b838-b8f373d6d52e_ContentModerator.
   F0_fe000d38-8ecd-47b5-a8b0-4764df00e3b5"
}
```

JSON yanıtındaki tüm bölümlerin ayrıntılı bir açıklaması için [Metin Moderasyonu](text-moderation-api.md) kavramsal kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API'sini kullanın veya uygulamanızla tümleştirmek için [.NET SDK'yı hızlı bir şekilde izleyin.](dotnet-sdk-quickstart.md)
