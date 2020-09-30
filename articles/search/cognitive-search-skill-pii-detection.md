---
title: PII algılama Bilişsel Beceri (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de bir zenginleştirme ardışık düzeninde bulunan metinden kişisel bilgileri ayıklayın ve maskesini kaldırın. Bu yetenek Şu anda genel önizlemededir.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: acacf617d3f1d9ab891d08b32fc2dfb14deb64a4
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91540532"
---
# <a name="pii-detection-cognitive-skill"></a>PII algılama Bilişsel Beceri

> [!IMPORTANT] 
> Bu yetenek Şu anda genel önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Şu anda portal veya .NET SDK desteği yok.

**PII algılama** becerisi, kişisel bilgileri bir giriş metinden ayıklar ve bunu maskeleme seçeneği sunar. Bu beceri bilişsel hizmetler 'de [metin analizi](../cognitive-services/text-analytics/overview.md) tarafından sunulan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Bilişsel Arama belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk eder. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması, [Azure bilişsel arama fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)açıklanmaktadır.

## <a name="odatatype"></a>@odata.type

Microsoft. yetenekler. Text. PIIDetectionSkill

## <a name="data-limits"></a>Veri sınırları

Bir kaydın en büyük boyutu, tarafından ölçülen 50.000 karakter olmalıdır [`String.Length`](/dotnet/api/system.string.length) . Verilerinizi beceriye göndermeden önce öbeketmeniz gerekiyorsa, [metin bölme becerinizi](cognitive-search-skill-textsplit.md)kullanmayı göz önünde bulundurun.

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır ve tümü isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| `defaultLanguageCode` |    Giriş metninin dil kodu. Şimdilik yalnızca `en` desteklenir. |
| `minimumPrecision` | 0,0 ile 1,0 arasında bir değer. Güven puanı ( `piiEntities` çıktıda), ayarlanan `minimumPrecision` değerden düşükse, varlık döndürülmez veya maskelenmez. Varsayılan değer 0,0 ' dir. |
| `maskingMode` | Giriş metninde algılanan kişisel bilgileri maskelemek için çeşitli yollar sağlayan bir parametre. Aşağıdaki seçenekler desteklenir: <ul><li>`none` (varsayılan): hiçbir maskeleme gerçekleşmez ve `maskedText` Çıkış döndürülmeyecektir. </li><li> `redact`: Algılanan varlıkları giriş metinden kaldırır ve silinen değerlerin yerini almaz. Bu durumda, `piiEntities` çıktıdaki fark, maskelenmiş metin değil, özgün metinle ilgili olacaktır. </li><li> `replace`: Algılanan varlıkların parametresinde verilen karakterle yerini alır `maskingCharacter` . Karakter, algılanan varlığın uzunluğuna yinelenir, böylece uzaklıklar hem giriş metnine hem de çıktının sonuna doğru şekilde karşılık gelir `maskedText` .</li></ul> |
| `maskingCharacter` | Parametresi olarak ayarlandıysa metni maskelemek için kullanılan karakter `maskingMode` `replace` . Aşağıdaki seçenekler desteklenir: `*` (varsayılan), `#` , `X` . Bu parametre yalnızca `null` `maskingMode` olarak ayarlanmamışsa olabilir `replace` . |

## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| `languageCode`    | İsteğe bağlı. `en` varsayılan değerdir.  |
| `text`          | Çözümlenecek metin.          |

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı      | Açıklama                   |
|---------------|-------------------------------|
| `piiEntities` | Aşağıdaki alanları içeren karmaşık türlerin dizisi: <ul><li>metin (ayıklanan olarak gerçek PII)</li> <li>tür</li><li>subType</li><li>Score (daha yüksek değer, gerçek bir varlık olma olasılığı daha yüksektir)</li><li>fark (giriş metnine)</li><li>length</li></ul> </br> [Olası türler ve alt türler burada bulunabilir.](../cognitive-services/text-analytics/named-entity-types.md?tabs=personal) |
| `maskedText` | , Dışında `maskingMode` bir değere ayarlanırsa `none` , bu çıktı, seçili tarafından açıklandığı şekilde giriş metninde gerçekleştirilen maskelemenin dize sonucu olacaktır `maskingMode` .  `maskingMode`Olarak ayarlanırsa `none` , bu çıkış mevcut olmayacaktır. |

## <a name="sample-definition"></a>Örnek tanım

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

## <a name="sample-input"></a>Örnek girdi

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

## <a name="sample-output"></a>Örnek çıktı

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

Bu yeteneğin çıktılarındaki varlıklar için döndürülen uzaklıklar, [Metin Analizi API'si](../cognitive-services/text-analytics/overview.md)doğrudan geri döndürülür. Bu, özgün dizeyi dizinlemek için kullanıyorsanız, doğru içeriği ayıklamak için .net Içindeki [StringInfo](/dotnet/api/system.globalization.stringinfo) sınıfını kullanmanız gerekir.  [Daha fazla ayrıntı burada bulunabilir.](../cognitive-services/text-analytics/concepts/text-offsets.md)

## <a name="errors-and-warnings"></a>Hatalar ve uyarılar

Belge dil kodu desteklenmiyorsa bir uyarı döndürülür ve hiçbir varlık ayıklanmaz.
Metniniz boşsa bir uyarı döndürülür.
Metniniz 50.000 karakterden fazlaysa, yalnızca ilk 50.000 karakter analiz edilir ve bir uyarı verilir.

Beceri bir uyarı döndürürse, çıkış `maskedText` boş olabilir, bu da çıktıyı bekleyen tüm aşağı akış becerilerini etkileyebilir. Bu nedenle, Beceri tanımınızı yazarken eksik çıktıyla ilgili tüm uyarıları araştırdığınızdan emin olun.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)