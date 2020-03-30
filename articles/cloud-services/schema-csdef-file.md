---
title: Azure Bulut Hizmetleri Tanımı Şeması (.csdef Dosyası) | Microsoft Dokümanlar
description: Hizmet tanımı (.csdef) dosyası, hizmet için kullanılabilir rolleri, uç noktalarını ve yapılandırma değerlerini içeren bir uygulama için hizmet modelini tanımlar.
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 42
author: tgore03
ms.author: tagore
ms.openlocfilehash: dadb50bd0663f47e6a1bf3d58b5187c8b466964d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528379"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Bulut Hizmetleri Tanımı Şeması (.csdef Dosyası)
Hizmet tanımı dosyası, bir uygulama için hizmet modelini tanımlar. Dosya, bulut hizmetinde kullanılabilen rollerin tanımlarını içerir, hizmet bitiş noktalarını belirtir ve hizmet için yapılandırma ayarlarını oluşturur. Yapılandırma ayar değerleri, [Bulut Hizmeti (klasik) Yapılandırma Şeması](/previous-versions/azure/reference/ee758710(v=azure.100))tarafından açıklandığı gibi hizmet yapılandırma dosyasında ayarlanır.

Varsayılan olarak, Azure Tanılama yapılandırma şeması dosyası `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizine yüklenir. Azure `<version>` [SDK'nın](https://www.windowsazure.com/develop/downloads/)yüklü sürümüyle değiştirin.

Hizmet tanımı dosyasının varsayılan uzantısı .csdef'tir.

## <a name="basic-service-definition-schema"></a>Temel hizmet tanımı şeması
Hizmet tanımı dosyası nda `ServiceDefinition` bir öğe bulunmalıdır. Hizmet tanımı en az bir`WebRole` rol `WorkerRole`(veya ) öğesi içermelidir. Tek bir tanımda tanımlanan en fazla 25 rol içerebilir ve rol türlerini karıştırabilirsiniz. Hizmet tanımı, hangi `NetworkTrafficRules` rollerin belirtilen dahili uç noktalarla iletişim kurabileceğini kısıtlayan isteğe bağlı öğeyi de içerir. Hizmet tanımı, uç `LoadBalancerProbes` noktaların müşteri tanımlı sistem sondalarını içeren isteğe bağlı öğeyi de içerir.

Hizmet tanımı dosyasının temel biçimi aşağıdaki gibidir.

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
Aşağıdaki konular şema açıklar:

- [LoadBalancerProbe Şeması](schema-csdef-loadbalancerprobe.md)
- [WebRole Şeması](schema-csdef-webrole.md)
- [WorkerRole Şeması](schema-csdef-workerrole.md)
- [NetworkTrafficRules Şeması](schema-csdef-networktrafficrules.md)

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a>ServiceDefinition Öğesi
Öğe, `ServiceDefinition` hizmet tanımı dosyasının üst düzey öğesidir.

Aşağıdaki tabloda `ServiceDefinition` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik               | Açıklama |
| ----------------------- | ----------- |
| ad                    |Gereklidir. Hizmetin adı. Ad, hizmet hesabında benzersiz olmalıdır.|
| topolojiChangeDiscovery | İsteğe bağlı. Topoloji değişikliği bildiriminin türünü belirtir. Olası değerler şunlardır:<br /><br /> -   `Blast`- Güncelleştirmeyi en kısa sürede tüm rol örneklerine gönderir. Seçeneği seçerseniz, rolün yeniden başlatılmadan topoloji güncelleştirmesini işleyebilir.<br />-   `UpgradeDomainWalk`– Güncelleştirmeyi, önceki örnek güncelleştirmeyi başarıyla kabul ettikten sonra, güncelleştirmeyi sıralı bir şekilde her rol örneğine gönderir.|
| şemaVersiyon           | İsteğe bağlı. Hizmet tanımı şemasının sürümünü belirtir. Şema sürümü Visual Studio SDK'nın birden fazla sürümü yan yana yüklenirse şema doğrulama için kullanılacak doğru SDK araçlarını seçmenize olanak tanır.|
| yükseltmeDomainCount      | İsteğe bağlı. Bu hizmetteki rollerin tahsis edildiği yükseltme etki alanlarının sayısını belirtir. Rol örnekleri, hizmet dağıtıldığında yükseltme etki alanına ayrılır. Daha fazla bilgi için bkz: [Bulut hizmeti rolünü veya dağıtımı güncelleştirin,](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment)sanal makinelerin [kullanılabilirliğini yönetin](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [Bulut Hizmet Modeli Nedir.](https://docs.microsoft.com/azure/cloud-services/cloud-services-model-and-package)<br /><br /> En fazla 20 yükseltme etki alanı belirtebilirsiniz. Belirtilmemişse, varsayılan yükseltme etki alanları sayısı 5'tir.|
