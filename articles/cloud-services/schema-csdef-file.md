---
title: Azure Cloud Services Tanım Şeması (. csdef dosyası) | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 42
author: tgore03
ms.author: tagore
ms.openlocfilehash: 11660fe4f76cad9577b66e72893670dd1b9b559d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75449069"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services Tanım Şeması (. csdef dosyası)
Hizmet tanımı dosyası bir uygulama için hizmet modelini tanımlar. Dosya, bir bulut hizmeti için kullanılabilir olan roller için tanımları içerir, hizmet uç noktalarını belirtir ve hizmet için yapılandırma ayarlarını oluşturur. Yapılandırma ayarı değerleri, [bulut hizmeti (klasik) yapılandırma şemasında](/previous-versions/azure/reference/ee758710(v=azure.100))açıklandığı gibi hizmet yapılandırma dosyasında ayarlanır.

Varsayılan olarak, Azure Tanılama yapılandırma şema dosyası `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizinine yüklenir. `<version>`, [Azure SDK](https://www.windowsazure.com/develop/downloads/)'nın yüklü sürümüyle değiştirin.

Hizmet tanım dosyası için varsayılan uzantı. csdef ' dir.

## <a name="basic-service-definition-schema"></a>Temel hizmet tanımı şeması
Hizmet tanımı dosyası bir `ServiceDefinition` öğesi içermelidir. Hizmet tanımı en az bir rol (`WebRole` veya `WorkerRole`) öğesi içermelidir. Tek bir tanımda tanımlanmış en fazla 25 rol içerebilir ve rol türlerini karıştırabilirsiniz. Hizmet tanımı Ayrıca, belirtilen iç uç noktalarla hangi rollerin iletişim kurabildiğini kısıtlayan isteğe bağlı `NetworkTrafficRules` öğesini de içerir. Hizmet tanımı, uç noktaların müşteri tanımlı sistem durumu araştırmalarını içeren isteğe bağlı `LoadBalancerProbes` öğesini de içerir.

Hizmet tanım dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Şema tanımları
Aşağıdaki konular şemayı anlatmaktadır:

- [LoadBalancerProbe Şeması](schema-csdef-loadbalancerprobe.md)
- [WebRole Şeması](schema-csdef-webrole.md)
- [WorkerRole Şeması](schema-csdef-workerrole.md)
- [NetworkTrafficRules Şeması](schema-csdef-networktrafficrules.md)

##  <a name="ServiceDefinition"></a>ServiceDefinition öğesi
`ServiceDefinition` öğesi, hizmet tanım dosyasının en üst düzey öğesidir.

Aşağıdaki tabloda `ServiceDefinition` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik               | Açıklama |
| ----------------------- | ----------- |
| ad                    |Gereklidir. Hizmetin adı. Ad, hizmet hesabı içinde benzersiz olmalıdır.|
| Topologyıchangediscovery | İsteğe bağlı. Topoloji değişiklik bildiriminin türünü belirtir. Olası değerler şunlardır:<br /><br /> -   `Blast`-güncelleştirmeyi tüm rol örneklerine en kısa sürede gönderir. Seçeneğini belirlerseniz, rolün yeniden başlatılmadan önce rolün topoloji güncelleştirmesini işleyebilmesi gerekir.<br />-   `UpgradeDomainWalk` – güncelleştirmeyi, bir önceki örnek güncelleştirmeyi başarıyla kabul ettikten sonra her bir rol örneğine sıralı bir şekilde gönderir.|
| schemaVersion           | İsteğe bağlı. Hizmet tanımı şemasının sürümünü belirtir. Şema sürümü, Visual Studio 'nun birden fazla SDK sürümü yan yana yüklenirse şema doğrulaması için kullanılacak doğru SDK araçlarını seçmesine olanak sağlar.|
| upgradeDomainCount      | İsteğe bağlı. Bu hizmetteki rollerin ayrıldığı yükseltme etki alanı sayısını belirtir. Rol örnekleri, hizmet dağıtıldığında bir yükseltme etki alanına ayrılır. Daha fazla bilgi için bkz. [bulut hizmeti rolünü veya dağıtımını güncelleştirme](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment), [kullanılabilirlik sanal makinelerini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [bulut hizmeti modeli nedir?](https://docs.microsoft.com/azure/cloud-services/cloud-services-model-and-package).<br /><br /> En fazla 20 yükseltme etki alanı belirtebilirsiniz. Belirtilmemişse, varsayılan yükseltme etki alanı sayısı 5 ' tir.|
