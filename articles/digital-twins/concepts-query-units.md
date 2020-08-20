---
title: Azure dijital TWINS 'de sorgu birimleri
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS 'te sorgu birimlerinin faturalandırma kavramını anlayın
author: baanders
ms.author: baanders
ms.date: 8/14/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 5334a1196ac8044c56e615cf8894b44646b48fb4
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88615119"
---
# <a name="query-units-in-azure-digital-twins"></a>Azure dijital TWINS 'de sorgu birimleri 

Azure dijital TWINS **sorgu birimi (QU)** , [sorgu API](https://docs.microsoft.com/rest/api/digital-twins/dataplane/query)'Sini kullanarak [Azure dijital TWINS sorgularınızı](how-to-query-graph.md) yürütmek için kullanılan isteğe bağlı bir hesaplama birimidir. 

Azure dijital TWINS tarafından desteklenen sorgu işlemlerini gerçekleştirmek için gereken CPU, ıOPS ve bellek gibi sistem kaynaklarını soyutlayıp bunun yerine sorgu birimlerindeki kullanımı izlemenize olanak sağlar.

Sorgunun karmaşıklığı, bu sorguyu yürütmek için kaç QUs tüketildiğini etkiler. 

Bu makalede sorgu birimlerinin nasıl anlaşılması ve sorgu birimi tüketiminin izlenmesi açıklanmaktadır.

## <a name="find-the-query-unit-consumption-in-azure-digital-twins"></a>Azure dijital TWINS 'te sorgu birimi tüketimini bulma 

Azure Digital TWINS [sorgu API](https://docs.microsoft.com/rest/api/digital-twins/dataplane/query)'sini kullanarak bir sorgu çalıştırdığınızda, sorgunun tükettiği Qus sayısını izlemek için yanıt üst bilgisini inceleyebilirsiniz. Azure dijital TWINS 'den geri gönderilen yanıtta "sorgu ücreti" ni arayın. 

Azure dijital TWINS [SDK 'ları](how-to-use-apis-sdks.md) , disk belleğine alınabilen yanıttan sorgu ücreti üst bilgisini ayıklamanızı sağlar. Bu bölümde, dijital TWINS sorgusunun nasıl sorgulanacağını ve sorgu ücreti üst bilgisini ayıklamak için disk belleğine alınabilen yanıtın nasıl yinelendirilecek gösterilmektedir. 

Aşağıdaki kod parçacığı, sorgu API 'SI çağrılırken oluşan sorgu ücretlerini nasıl ayıklayabileceğinizi gösterir. Sorgu ücreti başlığına erişmek için öncelikle yanıt sayfaları üzerinde dolaşır ve sonra her sayfa içindeki dijital ikizi sonuçlarının üzerinde dolaşır. 
 
```csharp
AsyncPageable<string> asyncPageableResponseWithCharge = client.QueryAsync("SELECT * FROM digitaltwins"); 
int pageNum = 0; 

// The "await" keyword here is required, as a call is made when fetching a new page. 

await foreach (Page<string> page in asyncPageableResponseWithCharge.AsPages()) 
{ 
    Console.WriteLine($"Page {++pageNum} results:"); 

    // Extract the query-charge header from the page 

    if (QueryChargeHelper.TryGetQueryCharge(page, out float queryCharge)) 
    { 
        Console.WriteLine($"Query charge was: {queryCharge}"); 
    } 

    // Iterate over the twin instances. 

    // The "await" keyword is not required here, as the paged response is local. 

    foreach (string response in page.Values) 
    { 
        BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(response); 
        Console.WriteLine($"Found digital twin '{twin.Id}'"); 
    } 
} 
```

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS sorgulama hakkında daha fazla bilgi edinmek için şu adresi ziyaret edin:
* [*Kavramlar: sorgu dili*](concepts-query-language.md)
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)
* [Sorgu API 'SI başvuru belgeleri](https://docs.microsoft.com/rest/api/digital-twins/dataplane/query/querytwins)

{1 & gt; Azure & lt; 1} sorgu ile ilgili sınırları [*genel önizlemede*](reference-service-limits.md)bulabilirsiniz.