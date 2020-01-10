---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA kurulumu
description: Azure Service Fabric 'de çalışan bir kapsayıcı için Grup tarafından yönetilen hizmet hesapları (gMSA) kurulumunu hemen öğrenin.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9873e2d7672412b0e1e22c6c2a774cf629fd728a
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639216"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA 'Yı ayarlama

GMSA 'yı (grup yönetilen hizmet hesapları) ayarlamak için, kümedeki tüm düğümlere bir kimlik bilgisi belirtim dosyası (`credspec`) yerleştirilir. Dosya, sanal makine uzantısı kullanılarak tüm düğümlere kopyalanabilir.  `credspec` dosyası gMSA hesap bilgilerini içermelidir. `credspec` dosyası hakkında daha fazla bilgi için bkz. [kimlik bilgisi belirtimi oluşturma](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Kimlik bilgisi belirtimi ve `Hostname` etiketi uygulama bildiriminde belirtilir. `Hostname` etiketinin, kapsayıcının çalıştığı gMSA hesap adıyla eşleşmesi gerekir.  `Hostname` etiketi, kapsayıcının Kerberos kimlik doğrulaması kullanarak etki alanındaki diğer hizmetlere kimliğini doğrulamasına izin verir.  `Hostname` belirtmeye yönelik bir örnek ve uygulama bildiriminde `credspec` aşağıdaki kod parçacığında gösterilmektedir:

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
