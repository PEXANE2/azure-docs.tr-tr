---
title: PII Algılama bilişsel beceri (önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki zenginleştirme boru hattındaki metinden kişisel olarak tanımlanabilir bilgileri ayıklayın ve maskeleyin. Bu beceri şu anda genel önizlemede.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: f21200bc6f5b25f3330f5bb87c0843caa5a84e56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298889"
---
#    <a name="pii-detection-cognitive-skill"></a>PII Algılama bilişsel beceri

> [!IMPORTANT] 
> Bu beceri şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. Şu anda portal veya .NET SDK desteği yok.

**KIŞISEL Algılama** becerisi, giriş metninden kişisel olarak tanımlayıcı bilgileri ayıklar ve bu metinden çeşitli şekillerde gizleme seçeneği sunar. Bu beceri, Bilişsel Hizmetlerde [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sağlanan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.PIIDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi beceriye göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harf duyarlıdır ve hepsi isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| defaultLanguageCode |    Giriş metninin dil kodu. Şimdilik, sadece `en` desteklenir. |
| minimum Hassasiyet | 0,0 ile 1,0 arasındaki bir değer. Güven puanı `piiEntities` (çıktıdaki) ayarlanan `minimumPrecision` değerden daha düşükse, varlık döndürülmez veya maskelenir. Varsayılan değer 0.0'dır. |
| maskelemeMode | Giriş metninde algılanan KIŞISEL'i maskelemek için çeşitli yollar sağlayan bir parametre. Aşağıdaki seçenekler desteklenir: <ul><li>`none`(varsayılan): Bu, maskeleme yapılmayan ve çıkıÅ `maskedText` ın iade edilmeye </li><li> `redact`: Bu seçenek, algılanan varlıkları giriş metninden kaldırır ve bunları hiçbir şeyle değiştirmez. Bu durumda, `piiEntities` çıktıdaki ofsetin maskeli metinle değil, özgün metinle ilişkili olacağını unutmayın. </li><li> `replace`: Bu seçenek, algılanan varlıkları `maskingCharacter` parametrede verilen karakterle değiştirir.  Uzaklıkların hem giriş metnine hem de çıktıya `maskedText`doğru bir şekilde karşılık verilebilsin diye karakter algılanan varlığın uzunluğuna tekrarlanır.</li></ul> |
| maskelemeKarakter | `maskingMode` Parametre ' ye ayarlanmışsa metni maskelemek için `replace`kullanılacak karakter. Aşağıdaki seçenekler desteklenir: `*` `#`(varsayılan), `X`. Bu parametre yalnızca `null` `maskingMode` ' olarak `replace`ayarlanmazsa olabilir. |


## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode    | İsteğe bağlı. `en` varsayılan değerdir.  |
| metin          | Çözümlemek için metin.          |

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı adı      | Açıklama                   |
|---------------|-------------------------------|
| piiEntities | Aşağıdaki alanları içeren karmaşık türleri dizisi: <ul><li>metin (Çıkarılan gerçek KIŞISEL Bilgiler)</li> <li>type</li><li>Alt</li><li>skor (Daha yüksek değer, gerçek bir varlık olma olasılığının daha yüksek olduğu anlamına gelir)</li><li>ofset (giriş metnine)</li><li>length</li></ul> </br> [Olası türleri ve alt Türleri burada bulabilirsiniz.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/named-entity-types?tabs=personal) |
| maskeliMetin | Başka `maskingMode` bir değere `none`ayarlanmışsa, bu çıktı, seçili `maskingMode`metinde açıklandığı gibi yapılan maskelemenin dize sonucu olacaktır.  `maskingMode` Ayarlanmışsa, bu çıktı mevcut olmayacaktır. `none` |

##    <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.PIIDetectionSkill",
    "defaultLanguageCode": "en",
    "minimumPrecision": 0.5,
    "maskingMode": "replace",
    "maskingCharacter": "*",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "piiEntities"
      },
      {
        "name": "maskedText"
      }
    ]
  }
```
##    <a name="sample-input"></a>Örnek giriş

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft employee with ssn 859-98-0987 is using our awesome API's."
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "piiEntities":[ 
           { 
              "text":"859-98-0987",
              "type":"U.S. Social Security Number (SSN)",
              "subtype":"",
              "offset":28,
              "length":11,
              "score":0.65
           }
        ],
        "maskedText": "Microsoft employee with ssn *********** is using our awesome API's."
      }
    }
  ]
}
```

Bu becerinin çıktısındaki varlıklar için döndürülen uzaklıkların doğrudan [Text Analytics API'sinden](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)döndürüldİğİni unutmayın , yani bunları özgün dizeiçin kullanıyorsanız, doğru içeriği ayıklamak için [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) sınıfını .NET'te kullanmanız gerekir.  [Daha fazla bilgi burada bulabilirsiniz.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/concepts/text-offsets)

## <a name="error-and-warning-cases"></a>Hata ve uyarı durumları
Belgenin dil kodu desteklenmezse, bir uyarı döndürülür ve hiçbir varlık ayıklanır.
Metniniz boşsa, bir uyarı üretilir.
Metniniz 50.000 karakterden büyükse, yalnızca ilk 50.000 karakter analiz edilir ve bir uyarı verilir.

Beceri bir uyarı döndürürse, çıktı `maskedText` boş olabilir.  Bu, bu çıktının daha sonraki becerilere girdi için var olmasını beklerseniz, beklendiği gibi çalışmayacağı anlamına gelir. Skillset tanımınızı yazarken bunu aklınızda bulundurun.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
