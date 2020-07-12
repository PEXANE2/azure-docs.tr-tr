---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA kurulumu
description: Azure Service Fabric 'de çalışan bir kapsayıcı için Grup tarafından yönetilen hizmet hesapları (gMSA) kurulumunu hemen öğrenin.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: d34b4c6e11628b6a4843f8a9077ebf69c9e023fe
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260886"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA 'Yı ayarlama

GMSA 'yı (grup yönetilen hizmet hesapları) ayarlamak için, kümedeki tüm düğümlere bir kimlik bilgisi belirtim dosyası ( `credspec` ) yerleştirilir. Dosya, sanal makine uzantısı kullanılarak tüm düğümlere kopyalanabilir.  `credspec`Dosya gMSA hesap bilgilerini içermelidir. Dosya hakkında daha fazla bilgi için `credspec` bkz. [kimlik bilgisi belirtimi oluşturma](/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Kimlik bilgisi belirtimi ve `Hostname` etiketi uygulama bildiriminde belirtilir. `Hostname`Etiket, kapsayıcının çalıştığı gMSA hesap adıyla eşleşmelidir.  `Hostname`Etiketi, kapsayıcının Kerberos kimlik doğrulaması kullanarak etki alanındaki diğer hizmetlere kimliğini doğrulamasına izin verir.  `Hostname`Aşağıdaki kod parçacığında, ve uygulama bildiriminde öğesini belirtmek için bir örnek `credspec` gösterilmektedir:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Sonraki adım olarak, aşağıdaki makaleleri okuyun:

* [Windows Server 2016 ' de Windows kapsayıcısını Service Fabric dağıtma](service-fabric-get-started-containers.md)
* [Linux üzerinde Service Fabric bir Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
