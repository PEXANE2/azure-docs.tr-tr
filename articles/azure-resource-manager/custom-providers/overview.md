---
title: Özel sağlayıcılar genel bakış
description: Azure Özel Kaynak Sağlayıcıları ve Azure API düzlemini iş akışlarınıza uyacak şekilde nasıl genişleteceğimiz hakkında bilgi edinin.
author: jjbfour
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: fd21117219ea3db6946e7a1b889d92702af65b58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650493"
---
# <a name="azure-custom-resource-providers-overview"></a>Azure Özel Kaynak Sağlayıcılarına Genel Bakış

Azure Özel Kaynak Sağlayıcıları, Azure için genişletilebilirlik platformudur. Varsayılan Azure deneyimini zenginleştirmek için kullanılabilecek özel API'leri tanımlamanıza olanak tanır. Bu dokümantasyon açıklar:

- Azure Özel Kaynak Sağlayıcısı oluşturma ve dağıtma.
- Varolan iş akışlarını genişletmek için Azure Özel Kaynak Sağlayıcıları'ndan nasıl yararlanılır?
- Başlamak için kılavuzları ve kod örnekleri bulmak için nerede.

![Özel sağlayıcıya genel bakış](./media/overview/overview.png)

> [!IMPORTANT]
> Özel Sağlayıcılar şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="what-can-custom-resource-providers-do"></a>Özel kaynak sağlayıcıları ne yapabilir?

Azure Özel Kaynak Sağlayıcıları ile neler başarabileceğinize dair bazı örnekler aşağıda verilmiştir:

- Azure Kaynak Yöneticisi REST API'sini dahili ve harici hizmetleri içerecek şekilde genişletin.
- Varolan Azure iş akışlarının üstüne özel senaryolar etkinleştirin.
- Azure Kaynak Yöneticisi Şablonlarını denetimi ve efekti özelleştirin.

## <a name="what-is-a-custom-resource-provider"></a>Özel kaynak sağlayıcısı nedir

Azure Özel Kaynak Sağlayıcıları, Azure ile bitiş noktası arasında bir sözleşme oluşturarak oluşturulur. Bu sözleşme, yeni bir kaynak, **Microsoft.CustomProviders/resourceProviders**aracılığıyla yeni kaynakların ve eylemlerin bir listesini tanımlar. Özel kaynak sağlayıcısı daha sonra bu yeni API'leri Azure'da ortaya çıkarır. Azure Özel Kaynak Sağlayıcıları üç bölümden oluşur: özel kaynak sağlayıcısı, **uç noktalar**ve özel kaynaklar.

## <a name="how-to-build-custom-resource-providers"></a>Özel kaynak sağlayıcıları oluşturma

Özel kaynak sağlayıcıları, Azure ve uç noktalar arasındaki sözleşmelerin listesidir. Bu sözleşme, Azure'un bir bitiş noktasıyla nasıl etkileşimde olması gerektiğini açıklar. Kaynak sağlayıcı bir proxy gibi davranır ve istekleri ve yanıtları belirtilen **bitiş noktasına**iletir. Kaynak sağlayıcısı iki tür sözleşme belirtebilir: [**kaynak Türleri**](./custom-providers-resources-endpoint-how-to.md) ve [**eylemler.**](./custom-providers-action-endpoint-how-to.md) Bunlar uç nokta tanımları aracılığıyla etkinleştirilir. Bitiş noktası tanımı üç alandan oluşur: **ad**, **yönlendirme Türü**ve **bitiş noktası.**

Örnek Bitiş Noktası:

```JSON
{
  "name": "{endpointDefinitionName}",
  "routingType": "Proxy",
  "endpoint": "https://{endpointURL}/"
}
```

Özellik | Gerekli | Açıklama
---|---|---
ad | *Evet* | Bitiş noktası tanımının adı. Azure bu adı API aracılığıyla '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/ altında ortaya çıkaracaktır.<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}'
yönlendirmeTürü | *hayır* | **Bitiş noktası**ile sözleşme türünü belirler. Belirtilmemişse, varsayılan olarak "Proxy" olur.
endpoint | *Evet* | İstekleri yönlendirmek için bitiş noktası. Bu yanıt yanı sıra isteğin herhangi bir yan etkileri ele alınacaktır.

### <a name="building-custom-resources"></a>Özel kaynaklar oluşturma

**Kaynak Türleri,** Azure'a eklenen yeni özel kaynakları açıklar. Bunlar temel RESTful CRUD yöntemlerini ortaya çıkarır. [Özel kaynaklar oluşturma hakkında daha fazla](./custom-providers-resources-endpoint-how-to.md) şey görün

**Kaynak Türleri**ile Örnek Özel Kaynak Sağlayıcısı:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Yukarıdaki örnek için Azure'a API'ler eklendi:

Http Yöntemi | Örnek URI | Açıklama
---|---|---
PUT | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-önizleme | Azure REST API'si yeni bir kaynak oluşturmak için çağrıda bulunuyor.
DELETE | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-önizleme | Azure REST API varolan bir kaynağı silmek için çağrıda bulunuyor.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-önizleme | Azure REST API'si varolan bir kaynağı almak için çağrıda bulunuyor.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources?api-version=2018-09-01-önizleme | Azure REST API'si, varolan kaynakların listesini almak için çağrıda bulunuyor.

### <a name="building-custom-actions"></a>Özel eylemler oluşturma

**Eylemler,** Azure'a eklenen yeni eylemleri açıklar. Bunlar kaynak sağlayıcısının üstünde açıklanabilir veya **bir kaynak Türü**altında iç içe. [Özel eylemler oluşturma hakkında daha fazla](./custom-providers-action-endpoint-how-to.md) şey görün

**Eylemleri**ile Örnek Özel Kaynak Sağlayıcı:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Yukarıdaki örnek için Azure'a API'ler eklendi:

Http Yöntemi | Örnek URI | Açıklama
---|---|---
POST | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>sağlayıcılar/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomAction?api-version=2018-09-01-önizleme | Azure REST API'si eylemi etkinleştirme çağrısında bulunuyor.

## <a name="looking-for-help"></a>Yardım arıyorum

Azure Özel Kaynak Sağlayıcısı geliştirme sorsanız, [Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-custom-providers)soru sormayı deneyin. Benzer bir soru zaten sorulmuş ve yanıtlanmış olabilir, bu nedenle göndermeden önce ilk kontrol edin. Hızlı yanıt ```azure-custom-providers``` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında öğrendim. Özel bir sağlayıcı oluşturmak için sonraki makaleye gidin.

- [Hızlı başlangıç: Azure Özel Kaynak Sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Nasıl Olunması: Azure REST API'sine Özel Eylemler Ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl Yapilir: Azure REST API'sine Özel Kaynak Ekleme](./custom-providers-resources-endpoint-how-to.md)
