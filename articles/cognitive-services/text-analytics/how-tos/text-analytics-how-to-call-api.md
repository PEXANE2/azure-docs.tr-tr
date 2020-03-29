---
title: Metin Analizi API’sini çağırma
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Metin Analizi REST API ve Postacı'yı nasıl arayabilirsiniz açıklanmaktadır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: afb576c265ccdd4a014ed678331f030a0442a197
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219307"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Metin Analizi REST API nasıl çağrılır?

**Text Analytics API'sine** yapılan aramalar, herhangi bir dilde formüle edebilirsiniz HTTP POST / GET aramaları vardır. Bu makalede, temel kavramları göstermek için REST ve [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) kullanın.

Her istek erişim anahtarınızı ve bir HTTP bitiş noktasını içermelidir. Bitiş noktası, kaydolma sırasında seçtiğiniz bölgeyi, hizmet URL'sini ve istekte `sentiment`kullanılan `keyphrases` `languages`bir `entities`kaynağı belirtir: , , ve . 

Text Analytics'in devletsiz olduğunu, bu nedenle yönetilen veri varlıkları olmadığını hatırlayın. Metniniz yüklenir, alındıktan sonra analiz edilir ve sonuçlar hemen arama uygulamasına döndürülür.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON şema tanımı

Giriş ham yapılandırılmamış metinde JSON olmalıdır. XML desteklenmez. Şema, aşağıdaki listede açıklanan öğelerden oluşan basittir. 

Şu anda tüm Metin Analizi işlemleri için aynı belgeleri gönderebilirsiniz: duyarlılık, anahtar tümcecik, dil algılama ve varlık tanımlama. (Şema gelecekte her analiz için değişebilir.)

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`id` |Veri türü dizedir, ancak uygulamada belge kimlikleri gerçeksayılar olma eğilimindedir. | Gerekli | Sistem, çıktıyı yapılandırmak için sağladığınız tetkikleri kullanır. İstekteki her kimlik için dil kodları, anahtar tümcecikler ve duyarlılık puanları oluşturulur.|
|`text` | Yapılandırılmamış ham metin, 5.120 karaktere kadar. | Gerekli | Dil algılama için metin herhangi bir dilde ifade edilebilir. Duyarlılık analizi, anahtar tümcecik çıkarma ve varlık tanımlaması için metin desteklenen bir [dilde](../text-analytics-supported-languages.md)olmalıdır. |
|`language` | Desteklenen bir [dil](../text-analytics-supported-languages.md) için 2 karakterli [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) kodu | Değişir | Duygusallık analizi, anahtar cümle çıkarma ve varlık bağlama için gerekli; dil algılama için isteğe bağlıdır. Bunu dışlarsanız hata yoktur, ancak çözümleme onsuz zayıflamış olur. Dil kodu `text` sağladığınız koda karşılık gelir. |

Sınırlar hakkında daha fazla bilgi için Metin [Analizine Genel Bakış > Veri limitlerine](../overview.md#data-limits)bakın. 

## <a name="set-up-a-request-in-postman"></a>Postacı'da istek tesayar

Hizmet, 1 MB boyutuna kadar olan isteği kabul eder. Postacı 'yı (veya başka bir Web API test aracını) kullanıyorsanız, son noktayı kullanmak istediğiniz kaynağı içerecek şekilde ayarlayın ve erişim anahtarını istek üstbilgisinde sağlayın. Her işlem, bitiş noktasına uygun kaynağı eklenizi gerektirir. 

1. Postacı olarak:

   + İstek türü olarak **Gönder'i** seçin.
   + Portal sayfasından kopyaladığınız bitiş noktasına yapıştırın.
   + Bir kaynak ekle.

   Kaynak uç noktaları aşağıdaki gibidir (bölgeniz değişebilir):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/entities`

2. Üç istek üstbilgisini ayarlayın:

   + `Ocp-Apim-Subscription-Key`: Azure portalından elde edilen erişim anahtarınız.
   + `Content-Type`: uygulama/json.
   + `Accept`: uygulama/json.

   İsteğiniz bir **/keyPhrases** kaynağı varsayarak aşağıdaki ekran görüntüsüne benzer olmalıdır.

   ![Bitiş noktası ve üstbilgi içeren ekran görüntüsü isteme](../media/postman-request-keyphrase-1.png)

4. **Gövde'yi** tıklatın ve biçim için **ham'ı** seçin.

   ![Gövde ayarlarıyla ekran görüntüsü iste](../media/postman-request-body-raw.png)

5. Bazı JSON belgelerine amaçlanan çözümleme için geçerli bir biçimde yapıştırın. Belirli bir analiz hakkında daha fazla bilgi için aşağıdaki konulara bakın:

  + [Dil algılama](text-analytics-how-to-language-detection.md)  
  + [Anahtar tümcecik çıkarma](text-analytics-how-to-keyword-extraction.md)  
  + [Yaklaşım analizi](text-analytics-how-to-sentiment-analysis.md)  
  + [Varlık tanıma](text-analytics-how-to-entity-linking.md)  


6. İsteği göndermek için **Gönder'i** tıklatın. Dakika ve saniye başına gönderebileceğiniz istek sayısı hakkında bilgi için genel bakışta [veri sınırları](../overview.md#data-limits) bölümüne bakın.

   Postacı'da yanıt, istekte sağlanan her belge kimliği için bir öğeyle birlikte bir sonraki pencerede tek bir JSON belgesi olarak görüntülenir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine Genel Bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dili algılama](text-analytics-how-to-language-detection.md)
