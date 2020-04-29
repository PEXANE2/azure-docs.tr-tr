---
title: Azure Cloud Services Tanım Şeması (. csdef dosyası) | Microsoft Docs
description: Hizmet tanımı (. csdef) dosyası, hizmet için kullanılabilir rolleri, uç noktaları ve yapılandırma değerlerini içeren bir uygulama için bir hizmet modeli tanımlar.
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 42
author: tgore03
ms.author: tagore
ms.openlocfilehash: dadb50bd0663f47e6a1bf3d58b5187c8b466964d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79528379"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services Tanım Şeması (. csdef dosyası)
Hizmet tanımı dosyası bir uygulama için hizmet modelini tanımlar. Dosya, bir bulut hizmeti için kullanılabilir olan roller için tanımları içerir, hizmet uç noktalarını belirtir ve hizmet için yapılandırma ayarlarını oluşturur. Yapılandırma ayarı değerleri, [bulut hizmeti (klasik) yapılandırma şemasında](/previous-versions/azure/reference/ee758710(v=azure.100))açıklandığı gibi hizmet yapılandırma dosyasında ayarlanır.

Varsayılan olarak, Azure Tanılama yapılandırma şema dosyası `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizine yüklenir. `<version>` [Azure SDK](https://www.windowsazure.com/develop/downloads/)'nın yüklü sürümüyle değiştirin.

Hizmet tanım dosyası için varsayılan uzantı. csdef ' dir.

## <a name="basic-service-definition-schema"></a>Temel hizmet tanımı şeması
Hizmet tanımı dosyası bir `ServiceDefinition` öğe içermelidir. Hizmet tanımında en az bir rol (`WebRole` veya `WorkerRole`) öğesi bulunmalıdır. Tek bir tanımda tanımlanmış en fazla 25 rol içerebilir ve rol türlerini karıştırabilirsiniz. Hizmet tanımı Ayrıca, belirtilen iç uç `NetworkTrafficRules` noktalarla hangi rollerin iletişim kurabildiğini kısıtlayan isteğe bağlı öğeyi de içerir. Hizmet tanımı, uç noktaların müşteri tanımlı `LoadBalancerProbes` sistem durumu araştırmalarını içeren isteğe bağlı öğeyi de içerir.

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

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a>ServiceDefinition öğesi
`ServiceDefinition` Öğesi, hizmet tanım dosyasının en üst düzey öğesidir.

Aşağıdaki tablo, `ServiceDefinition` öğesinin özniteliklerini açıklar.

| Öznitelik               | Açıklama |
| ----------------------- | ----------- |
| ad                    |Gereklidir. Hizmetin adı. Ad, hizmet hesabı içinde benzersiz olmalıdır.|
| Topologyıchangediscovery | İsteğe bağlı. Topoloji değişiklik bildiriminin türünü belirtir. Olası değerler şunlardır:<br /><br /> -   `Blast`-Güncelleştirmeyi tüm rol örneklerine en kısa sürede gönderir. Seçeneğini belirlerseniz, rolün yeniden başlatılmadan önce rolün topoloji güncelleştirmesini işleyebilmesi gerekir.<br />-   `UpgradeDomainWalk`– Güncelleştirmeyi, önceki örnek güncelleştirmeyi başarıyla kabul ettikten sonra her bir rol örneğine sıralı bir şekilde gönderir.|
| schemaVersion           | İsteğe bağlı. Hizmet tanımı şemasının sürümünü belirtir. Şema sürümü, Visual Studio 'nun birden fazla SDK sürümü yan yana yüklenirse şema doğrulaması için kullanılacak doğru SDK araçlarını seçmesine olanak sağlar.|
| upgradeDomainCount      | İsteğe bağlı. Bu hizmetteki rollerin ayrıldığı yükseltme etki alanı sayısını belirtir. Rol örnekleri, hizmet dağıtıldığında bir yükseltme etki alanına ayrılır. Daha fazla bilgi için bkz. [bulut hizmeti rolü veya dağıtımı güncelleştirme](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment), [sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [bulut hizmeti modeli nedir](https://docs.microsoft.com/azure/cloud-services/cloud-services-model-and-package).<br /><br /> En fazla 20 yükseltme etki alanı belirtebilirsiniz. Belirtilmemişse, varsayılan yükseltme etki alanı sayısı 5 ' tir.|
