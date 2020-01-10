---
title: Hizmet uç noktalarına erişim ilkeleri atama
description: Service Fabric hizmetinizde HTTP veya HTTPS uç noktalarına güvenlik erişimi ilkeleri atamayı öğrenin.
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: c7d30e85848f045b5724bb8bdc6e5c810102c044
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614664"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama
Bir farklı çalıştır ilkesi uygularsanız ve hizmet bildirimi HTTP uç noktası kaynaklarını bildiriyorsa, bir **Securityaccesspolicy**belirtmeniz gerekir.  **Securityaccesspolicy** , bu uç noktalara ayrılan bağlantı noktalarının hizmetin çalıştığı kullanıcı hesabıyla doğru bir şekilde kısıtlanmasını sağlar. Aksi halde, **http. sys** ' nin hizmete erişimi yoktur ve istemciden gelen çağrılarla ilgili hatalarla ilgilenirsiniz. Aşağıdaki örnek, tam erişim hakları sağlayan **EndpointName**adlı bir uç noktaya Customer1 hesabını uygular.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Bir HTTPS uç noktası için, istemciye döndürülecek sertifikanın adını da belirtin. Bu sertifikaya **Endpointbindingpolicy**kullanarak başvurulamıyor.  Sertifika, uygulama bildiriminin **Sertifikalar** bölümünde tanımlanmıştır.

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
> HTTPS kullanırken, aynı düğüme dağıtılan farklı hizmet örnekleri (uygulamadan bağımsız) için aynı bağlantı noktasını ve sertifikayı kullanmayın. Farklı uygulama örneklerinde aynı bağlantı noktasını kullanarak iki farklı hizmetin yükseltilmesi, yükseltme hatasına neden olur. Daha fazla bilgi için bkz. [https uç noktaları ile birden çok uygulamayı yükseltme ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Sonraki adımlar için aşağıdaki makaleleri okuyun:
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
