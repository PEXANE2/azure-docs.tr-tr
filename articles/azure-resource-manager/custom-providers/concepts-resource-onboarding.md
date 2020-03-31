---
title: Kaynak ekleme
description: Diğer Azure kaynak türlerine yönetim veya yapılandırma uygulamak için Azure Özel Sağlayıcıları'nı kullanarak kaynak onboarding gerçekleştirme hakkında bilgi edinin.
author: jjbfour
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: jobreen
ms.openlocfilehash: 1846b036f12fe7e691021ec0248782cad946d9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650415"
---
# <a name="azure-custom-providers-resource-onboarding-overview"></a>Azure Özel Sağlayıcılar kaynağı onboarding genel bakış

Azure Özel Sağlayıcıları kaynak onboarding, Azure kaynak türleri için genişletilebilirlik modelidir. Mevcut Azure kaynaklarında işlemleri veya yönetimi ölçekte uygulamanızı sağlar. Daha fazla bilgi için Azure [Özel Sağlayıcılarının Azure'u nasıl genişletebileceğini](overview.md)öğrenin. Bu makalede açıklanır:

- Onboarding'in kaynağı neler yapabilir?
- Kaynak onboarding temelleri ve nasıl kullanılacağı.
- Başlamak için kılavuzları ve kod örnekleri bulmak için nerede.

> [!IMPORTANT]
> Özel Sağlayıcılar şu anda genel önizlemededir.
> Bu önizleme sürümü hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önermiyoruz. Bazı özellikler desteklenmeyen veya kısıtlı özelliklere sahip olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="what-can-resource-onboarding-do"></a>Kaynak onboarding ne yapabilirim?

Azure [Özel Sağlayıcıları özel kaynaklarına](./custom-providers-resources-endpoint-how-to.md)benzer şekilde, kaynak onboarding,"onboarding" isteklerini bitiş noktasına taşıyacak bir sözleşme tanımlar. Özel kaynakların aksine, kaynak onboarding yeni bir kaynak türü oluşturmaz. Bunun yerine, varolan kaynak türlerinin uzantısısağlar. Ayrıca kaynak onboarding, Azure İlkesi ile çalışır, böylece kaynakların yönetimi ve yapılandırması ölçekte yapılabilir. Kaynak onboarding iş akışları bazı örnekler:

- Sanal makine uzantılarını yükleyin ve yönetin.
- Azure depolama hesaplarında varsayılanları yükleyin ve yapılandırır.
- Ölçekte temel tanılama ayarlarını etkinleştirin.

## <a name="resource-onboarding-basics"></a>Kaynak onboarding temelleri

Microsoft.CustomProviders/resourceProviders ve Microsoft.CustomProviders/associations kaynak türlerini kullanarak, kaynağı Azure Özel Sağlayıcıları aracılığıyla yapılandırabilirsiniz. Özel bir sağlayıcı için kaynak onboarding etkinleştirmek için, yapılandırma işlemi sırasında, "Uzantı" içeren bir **yönlendirme Türü** ile "ilişkilendirmeler" adlı bir kaynak **Türü** oluşturun. Microsoft.CustomProviders/associations ve Microsoft.CustomProviders/resourceProviders'ların aynı kaynak grubuna ait olması gerekmez.

Aşağıda örnek bir Azure özel sağlayıcısı verem:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://microsoft.com/"
      }
    ]
  },
  "location": "eastus"
}
```

Özellik | Gerekli mi? | Açıklama
---|---|---
ad | Evet | Bitiş noktası tanımının adı. Kaynak onboarding için, adı "dernekler" olmalıdır.
yönlendirmeTürü | Evet | Bitiş noktası yla sözleşme türünü belirler. Kaynak onboarding için geçerli **yönlendirme Türleri** "Proxy, Önbellek,Uzantı" ve "Webhook, Önbellek, Uzantı" dır.
endpoint | Evet | İstekleri yönlendirmek için bitiş noktası. Bu yanıt ve isteğin herhangi bir yan etkileri ele alınacaktır.

Dernekler kaynak türüne sahip özel sağlayıcı oluşturduktan sonra, Microsoft.CustomProviders/associations kullanarak hedefleyebilirsiniz. Microsoft.CustomProviders/associations, diğer Azure kaynağını genişletebilen bir uzantı kaynağıdır. Microsoft.CustomProviders/associations bir örnek oluşturulduğunda, geçerli bir Microsoft.CustomProviders/resourceProviders veya Microsoft.Solutions/applications kaynak kimliği olmalıdır bir özellik **hedefResourceId**alır. Bu gibi durumlarda, istek oluşturduğunuz Microsoft.CustomProviders/resourceProviders örneğindeki ilişkilendirmekaynak türüne iletilir.

> [!NOTE]
> Bir Microsoft.Solutions/applications kaynak kimliği **targetResourceId**olarak sağlanıyorsa, yönetilen kaynak grubunda "ortak" adı olan bir Microsoft.CustomProviders/resourceProviders dağıtılmış olmalıdır.

Örnek Azure Özel Sağlayıcıları derneği:

```JSON
{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    ...
  }
}
```

Özellik | Gerekli mi? | Açıklama
---|---|---
targetResourceId | Evet | Microsoft.CustomProviders/resourceProviders veya Microsoft.Solutions/applications kaynak kimliği.

## <a name="how-to-use-resource-onboarding"></a>Kaynak onboarding nasıl kullanılır?

Kaynak onboarding Microsoft.CustomProviders/associations uzantısı kaynağı ile diğer kaynakları genişleterek çalışır. Aşağıdaki örnekte, sanal bir makine için istek yapılır, ancak herhangi bir kaynak genişletilebilir.

İlk olarak, ilişkilendirme kaynak türüne sahip özel bir sağlayıcı kaynağı oluşturmanız gerekir. Bu, özel sağlayıcıyı hedefleyen ilgili Microsoft.CustomProviders/associations kaynağı oluşturulduğunda kullanılacak geri arama URL'sini bildirir.

Örnek Microsoft.CustomProviders/resourceProviders istek oluşturmak:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://{myCustomEndpoint}/"
      }
    ]
  },
  "location": "{location}"
}
```

Özel sağlayıcı oluşturduktan sonra, diğer kaynakları hedefleyebilir ve özel sağlayıcının yan etkilerini onlara uygulayabilirsiniz.

Örnek Microsoft.CustomProviders/dernekler istek oluşturmak:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Bu istek daha sonra oluşturduğunuz özel sağlayıcıda belirtilen ve **targetResourceId** tarafından bu formda başvurulan bitiş noktasına iletilir:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/associations/{associationName}
X-MS-CustomProviders-ExtensionPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}
X-MS-CustomProviders-ExtendedResource: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}

{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Bitiş noktası bir uygulama/json `Content-Type` ve geçerli bir JSON yanıt gövdesi ile yanıt vermelidir. JSON **özellikleri** nesnesi altında döndürülen alanlar ilişkilendirme iade yanıtına eklenir.

## <a name="getting-help"></a>Yardım alma

Azure Özel Kaynak Sağlayıcıları geliştirmesi hakkında sorularınız varsa, [bunları Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-custom-providers)sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce önce kontrol edin. Hızlı yanıt ```azure-custom-providers``` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında öğrendim. Daha fazla bilgi edinmek için bu makalelere bakın:

- [Öğretici: Özel sağlayıcılarla onboarding kaynak](./tutorial-resource-onboarding.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Hızlı başlatma: Özel bir kaynak sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Nasıl yapilir: Azure REST API'sine özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapilir: Azure REST API'sine özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
