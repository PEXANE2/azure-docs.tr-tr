---
title: Hizmet bitiş noktalarına erişim ilkeleri atama
description: Hizmet Kumaşı hizmetinizdeki HTTP veya HTTPS uç noktalarına güvenlik erişim polislerini nasıl atayacaklarınız öğrenin.
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: c7d30e85848f045b5724bb8bdc6e5c810102c044
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614664"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama
Bir run-as ilkesi uygularsanız ve hizmet bildirimi HTTP uç noktası kaynaklarını bildirirse, bir **SecurityAccessPolicy**belirtmeniz gerekir.  **SecurityAccessPolicy,** bu uç noktalara tahsis edilen bağlantı noktalarının, hizmetin çalıştığı kullanıcı hesabıyla doğru bir şekilde sınırlandırılmasını sağlar. Aksi takdirde, **http.sys** hizmeterişimi yok ve istemci aramaları ile hataları olsun. Aşağıdaki örnek, Customer1 hesabını **EndpointName**adlı bir bitiş noktasına uygular ve bu da ona tam erişim hakkı verir.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

HTTPS bitiş noktası için, istemciye dönmek için sertifikanın adını da belirtin. **Sertifikaya EndpointBindingPolicy'yi**kullanarak başvurursunuz.  Sertifika, başvuru bildiriminin **Sertifikalar** bölümünde tanımlanır.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies>
```

> [!WARNING] 
> HTTPS kullanırken, aynı düğüme dağıtılan farklı hizmet örnekleri (uygulamadan bağımsız) için aynı bağlantı noktasını ve sertifikayı kullanmayın. Farklı uygulama örneklerinde aynı bağlantı noktasını kullanarak iki farklı hizmetin yükseltilmesi bir yükseltme hatasına neden olur. Daha fazla bilgi için bkz: [HTTPS uç noktalarıyla birden çok uygulamayı yükseltme. ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints)
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adımlar için aşağıdaki makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
