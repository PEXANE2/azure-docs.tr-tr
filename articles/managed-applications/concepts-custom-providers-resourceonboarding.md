---
title: Azure özel sağlayıcılar kaynak ekleme
description: Diğer Azure kaynak türlerine yönetim veya yapılandırma uygulamak için Azure özel sağlayıcılarını kullanarak kaynak ekleme hakkında bilgi edinin.
author: jjbfour
ms.service: managed-applications
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: jobreen
ms.openlocfilehash: 6f9dcf4118dd2167f4cef35408d5ead79bf33877
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73609157"
---
# <a name="azure-custom-providers-resource-onboarding-overview"></a>Azure özel sağlayıcılar kaynak eklemeye genel bakış

Azure özel sağlayıcılar kaynak ekleme, Azure Kaynak türleri için bir genişletilebilirlik modelidir. Mevcut Azure kaynakları genelinde işlem veya yönetim işlemlerini ölçeklendirerek uygulamanıza olanak tanır. Daha fazla bilgi için bkz. [Azure özel sağlayıcılarının Azure 'U nasıl genişletebilen](./custom-providers-overview.md). Bu belgelerde şunları açıklar:

- Kaynak ekleme ne yapabilir?
- Kaynak ekleme temelleri ve kullanımı.
- Başlamak için kılavuzlar ve kod örnekleri nerede bulunur.

> [!IMPORTANT]
> Özel sağlayıcılar Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-resource-onboarding-do"></a>Kaynak ekleme ne yapabilir?

Kaynak ekleme, [Azure özel sağlayıcı özel kaynaklarına](./custom-providers-resources-endpoint-how-to.md)benzer şekilde, bir uç noktaya "ekleme" istekleri sunan bir sözleşme tanımlar. Özel kaynakların aksine, kaynak ekleme yeni bir kaynak türü oluşturmaz, bunun yerine mevcut kaynak türlerinin uzantısına izin verir. Ayrıca, kaynak ekleme Azure Ilkesiyle birlikte çalışarak kaynakların yönetimi ve yapılandırması da ölçeklenebilir şekilde yapılabilir. Kaynak ekleme iş akışlarına ilişkin bazı örnekler:

- Sanal makine uzantıları üzerine yükler ve yönetin.
- Azure depolama hesaplarında Varsayılanları karşıya yükleyin ve yapılandırın.
- Ölçek sırasında taban çizgisi tanılama ayarlarını etkinleştirin.

## <a name="resource-onboarding-basics"></a>Kaynak ekleme temelleri

Kaynak ekleme, "Microsoft. CustomProviders/resourceProviders" ve "Microsoft. CustomProviders/Association" kaynak türleri kullanılarak Azure özel sağlayıcılar aracılığıyla yapılandırılır. Özel bir sağlayıcı için kaynak eklemeyi etkinleştirmek üzere, yapılandırma işlemi sırasında "Extension" adlı bir bir **ResourceType** oluşturun ve "Extension" Içeren bir **routingtype** ile Ayrıca, "Microsoft. CustomProviders/Association" ve "Microsoft. CustomProviders/resourceProviders" öğesinin aynı kaynak grubuna ait olması gerekmez.

Örnek Azure özel sağlayıcısı:

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

Özellik | Gerekli | Açıklama
---|---|---
ad | *Yes* | Uç nokta tanımının adı. Kaynak ekleme için ad "ilişkilendirmeler" olmalıdır.
routingType | *Yes* | **Uç nokta**ile sözleşme türünü belirler. Kaynak ekleme için geçerli **Routingtypes** "proxy, önbellek, uzantı" ve "Web kancası, önbellek, uzantı" dır.
endpoint | *Yes* | İsteklerin yönlendirileceği uç nokta. Bu, yanıtın yanı sıra isteğin herhangi bir yan etkisini de işler.

"İlişkilendirmeler" kaynak türü ile özel sağlayıcı oluşturulduktan sonra, "Microsoft. CustomProviders/Association" kullanarak hedefleyebilirsiniz. "Microsoft. CustomProviders/Association", herhangi bir Azure kaynağını genişletebilen bir uzantı kaynağıdır. Bir "Microsoft. CustomProviders/Association" örneği oluşturulduğunda, geçerli bir "Microsoft. CustomProviders/resourceProviders" veya "Microsoft. Solutions/Applications" kaynak KIMLIĞI olması gereken bir **Targetresourceıd**özelliği alır. Bu durumlarda istek, oluşturduğumuz "Microsoft. CustomProviders/resourceProviders" örneğindeki "ilişkilendirmeler" kaynak türüne iletilir.

> [!Note]
> "Microsoft. Solutions/Applications" kaynak KIMLIĞI **Targetresourceıd**olarak sağlanmışsa, "public" adlı yönetilen kaynak grubunda dağıtılmış bir "Microsoft. Customproviders/resourceproviders" olması gerekir.

Örnek Azure özel sağlayıcı Ilişkilendirmesi:

```JSON
{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    ...
  }
}
```

Özellik | Gerekli | Açıklama
---|---|---
Targetresourceıd | *Yes* | "Microsoft. CustomProviders/resourceProviders" veya "Microsoft. Solutions/Applications" kaynak KIMLIĞI.

## <a name="how-to-use-resource-onboarding"></a>Kaynak ekleme 'yi kullanma

Kaynak ekleme, diğer kaynakları "Microsoft. CustomProviders/Association" uzantı kaynağıyla genişleterek işe yarar. Aşağıdaki örnekte, istek bir sanal makine için yapılır, ancak herhangi bir kaynak genişletilebilir.

İlk olarak, "ilişkilendirmeler" kaynak türüne sahip bir özel sağlayıcı kaynağı oluşturulmalıdır. Bu işlem, özel sağlayıcıyı hedefleyen karşılık gelen bir "Microsoft. CustomProviders/Association" kaynağı oluşturulduğunda kullanılacak geri çağırma URL 'sini bildirir.

Örnek "Microsoft. CustomProviders/resourceProviders" oluşturma isteği:

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

Özel sağlayıcı oluşturulduktan sonra, artık diğer kaynakları hedefleyebilir ve özel sağlayıcının yan etkileri bunlara uygulayabiliriz.

Örnek "Microsoft. CustomProviders/Association" oluşturma isteği:

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

Daha sonra bu istek, şu formda "Targetresourceıd" tarafından başvurulan ilk oluşturulan özel sağlayıcıda belirtilen uç noktaya iletilir:

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

Uç noktanın "Application/JSON" `Content-Type` ve geçerli JSON yanıt gövdesi ile yanıt vermesi gerekir. JSON 'ın "Özellikler" nesnesi altında döndürülen alanlar ilişkilendirme dönüş yanıtına eklenecektir.

## <a name="looking-for-help"></a>Yardım aranıyor

Azure özel kaynak sağlayıcısı geliştirme hakkında sorularınız varsa [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)yapmayı deneyin. Benzer bir soru zaten istendi ve yanıtlamış olabilir, bu nedenle göndermeden önce kontrol edin. Hızlı bir yanıt almak için etiketi ```azure-custom-providers``` ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında bilgi edindiniz. Özel bir sağlayıcı oluşturmak için sonraki makaleye gidin.

- [Öğretici: özel sağlayıcılarla kaynak ekleme](./tutorial-custom-providers-resource-onboarding.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-custom-providers-101.md)
- [Hızlı başlangıç: Azure özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
