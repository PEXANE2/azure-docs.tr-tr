---
title: PII algılama Bilişsel Beceri (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de bir zenginleştirme ardışık düzeninde bulunan metinden kişisel olarak tanımlanabilir bilgileri ayıklayın ve maskesini kaldırın. Bu yetenek Şu anda genel önizlemededir.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 7011d971ddb1888b312173a42ecd4a50f0954395
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849051"
---
#   <a name="pii-detection-cognitive-skill"></a>PII algılama Bilişsel Beceri

> [!IMPORTANT] 
> Bu yetenek Şu anda genel önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Şu anda portal veya .NET SDK desteği yok.

**PII algılama** yeteneği, kişisel olarak tanımlanabilir bilgileri bir giriş metinden ayıklar ve bu metni çeşitli yollarla maskelemeniz için size bir seçenek sunar. Bu beceri bilişsel hizmetler 'de [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sunulan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Bilişsel Arama belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk eder. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması, [Azure bilişsel arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.


## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. PIIDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu, [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülen 50.000 karakter olmalıdır. Verileri beceriye göndermeden önce bölmeniz gerekirse, [metin bölme becerinizi](cognitive-search-skill-textsplit.md)kullanmayı göz önünde bulundurun.

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır ve tümü isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| defaultLanguageCode | Giriş metninin dil kodu. Şimdilik yalnızca `en` desteklenir. |
| minimumPrecision | 0,0 ile 1,0 arasında bir değer. Güven puanı (`piiEntities` çıktısında), küme `minimumPrecision` değerinden düşükse, varlık döndürülmez veya maskelenmez. Varsayılan değer 0,0 ' dir. |
| maskingMode | Giriş metninde algılanan PII 'yi maskelemek için çeşitli yollar sağlayan bir parametre. Aşağıdaki seçenekler desteklenir: <ul><li>`none` (varsayılan): Bu, hiçbir maskeleme gerçekleştirilmeyeceği ve `maskedText` çıkışının döndürülmeyeceği anlamına gelir. </li><li> `redact`: Bu seçenek, algılanan varlıkları giriş metinden kaldırır ve bunları hiçbir şeyle değiştirmez. Bu durumda, `piiEntities` çıkışındaki kaydırın, maskelenmiş metin değil, özgün metinle ilişkili olacağını unutmayın. </li><li> `replace`: Bu seçenek, algılanan varlıkların `maskingCharacter` parametresinde verilen karakterle yerini alır.  Karakter, algılanan varlığın uzunluğuna yinelenir, böylece uzaklıklar hem giriş metnine hem de çıkış `maskedText`doğru şekilde karşılık gelir.</li></ul> |
| maskingCharacter | `maskingMode` parametresi `replace`olarak ayarlandıysa metni maskelenecek olan karakter. Aşağıdaki seçenekler desteklenir: `*` (varsayılan), `#`, `X`. Bu parametre yalnızca `maskingMode` `replace`olarak ayarlanmamışsa `null` olabilir. |


## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `en` varsayılan değerdir.  |
| metin          | Çözümlenecek metin.          |

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| piiEntities | Aşağıdaki alanları içeren karmaşık türlerin dizisi: <ul><li>metin (ayıklanan olarak gerçek PII)</li> <li>type</li><li>subType</li><li>Score (daha yüksek değer, gerçek bir varlık olma olasılığı daha yüksektir)</li><li>fark (giriş metnine)</li><li>length</li></ul> </br> [Olası türler ve alt türler burada bulunabilir.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/named-entity-types?tabs=personal) |
| maskedText | `maskingMode`, `none`dışında bir değere ayarlanırsa, bu çıktı, seçili `maskingMode`açıklandığı şekilde giriş metninde gerçekleştirilen maskelemenin dize sonucu olur.  `maskingMode` `none`olarak ayarlanırsa, bu çıkış mevcut olmayacaktır. |

##  <a name="sample-definition"></a>Örnek tanım

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
##  <a name="sample-input"></a>Örnek giriş

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

##  <a name="sample-output"></a>Örnek çıktı

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


## <a name="error-and-warning-cases"></a>Hata ve uyarı durumları
Belge dil kodu desteklenmiyorsa bir uyarı döndürülür ve hiçbir varlık ayıklanmaz.
Metniniz boşsa bir uyarı üretilir.
Metniniz 50.000 karakterden fazlaysa, yalnızca ilk 50.000 karakter analiz edilir ve bir uyarı verilir.

Beceri bir uyarı döndürürse, çıkış `maskedText` boş olabilir.  Bu, çıkışın daha sonraki yeteneklere giriş için var olmasını beklediğinizi, istenen şekilde çalışmayacağı anlamına gelir. Beceri tanımınızı yazarken bunu aklınızda bulundurun.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
