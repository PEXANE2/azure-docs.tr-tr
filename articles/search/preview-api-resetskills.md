---
title: Becerileri sıfırlama (api-Version = 2019-050 -06-Preview)
titleSuffix: Azure Cognitive Search
description: Önizleme REST API, bir beceri tamamen veya kısmi olarak yeniden işlenmesine gerek duyduğunuzda artımlı zenginleştirme için kullanılır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 4557e966c431eca31aa7577bebc75caae9365dc9
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832149"
---
# <a name="reset-skills-api-version2019-05-06-preview"></a>Becerileri sıfırlama (api-Version = 2019-05 -06-Preview)

> [!IMPORTANT] 
> Artımlı zenginleştirme Şu anda genel önizlemededir. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

**Yetenek sıfırlama** isteği, bir sonraki Dizin Oluşturucu çalıştırmasında hangi becerilerin işleyeceğini belirtir. Önbelleğe alma özelliği etkin olan Dizin oluşturucular için, dizin oluşturucunun algılayamayacağı yetenek güncelleştirmelerine yönelik işleme açıkça istek yapabilirsiniz. Örneğin, özel bir beceriye yönelik düzeltmeler gibi dış değişiklikler yaparsanız, yeteneği yeniden çalıştırmak için bu API 'yi kullanabilirsiniz. Bilgi deposu veya arama dizini gibi çıkışlar, önbellekteki yeniden kullanılabilir veriler ve güncelleştirilmiş yetenek başına yeni içerik kullanılarak yenilenir.

Bir HTTP PUT kullanarak var olan bir [beceri](https://docs.microsoft.com/rest/api/searchservice/create-skillset) sıfırlayabilirsiniz ve bu, istek URI 'sindeki güncelleştirilecek beceri adını belirtebilirsiniz. İstekte **api-Version = 2019-05 -06-Preview** kullanmanız gerekir.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]/resetskills?api-version=2019-05-06-Preview
api-key: [admin key]
Content-Type: application/json
```  

Alternatif olarak, Gönder ' i kullanın ve isteğin gövdesinde Dizin Oluşturucu adını yerleştirin:

```http
POST https://[service name].search.windows.net/indexers/resetskills?api-version=[api-version]  
api-key: [admin key]  
```  

## <a name="request-headers"></a>İstek üst bilgileri  

 Aşağıdaki tabloda gerekli ve isteğe bağlı istek üstbilgileri açıklanmaktadır.  

|İstek Başlığı|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Bunu `application/json` ayarla|  
|*api anahtarı:*|Gereklidir. `api-key`, arama hizmetinize yönelik isteğin kimliğini doğrulamak için kullanılır. Bu, hizmetinize özgü bir dize değeridir. **Reset beceri** isteği, yönetici anahtarınıza (bir sorgu anahtarından farklı olarak) ayarlanmış bir `api-key` üst bilgisi içermelidir.|  

İstek URL 'sini oluşturmak için hizmet adına de ihtiyacınız vardır. Hizmetin genel bakış sayfasından hizmet adı ve `api-key` her ikisini de alabilirsiniz Azure portal. Bkz. sayfa gezintisi yardımı için [Azure bilişsel arama hizmeti oluşturma](https://docs.microsoft.com/azure/search/search-create-service-portal) .  

## <a name="request-body-syntax"></a>İstek gövdesi sözdizimi  

İsteğin gövdesi, yetenek adlarından oluşan bir dizidir.

```json
{   
    "skillNames" : ["<SKILL-1>", "SKILL-n"]
}  
```

## <a name="response"></a>Yanıt  

Durum kodu: 204 başarılı bir yanıt için Içerik yok. 

## <a name="see-also"></a>Ayrıca bkz.

+ [REST API 'Leri ara](https://docs.microsoft.com/rest/api/searchservice)
+ [HTTP durum kodu](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)  
+ [AI zenginleştirme genel bakış](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)
+ [Önbelleğe alma ve artımlı zenginleştirme yapılandırma](search-howto-incremental-index.md)
+ [Artımlı zenginleştirme](cognitive-search-incremental-indexing-conceptual.md)