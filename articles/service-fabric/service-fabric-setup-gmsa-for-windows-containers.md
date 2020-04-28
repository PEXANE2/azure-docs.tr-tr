---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA kurulumu
description: Azure Service Fabric 'de çalışan bir kapsayıcı için Grup tarafından yönetilen hizmet hesapları (gMSA) kurulumunu hemen öğrenin.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9873e2d7672412b0e1e22c6c2a774cf629fd728a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75639216"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA 'Yı ayarlama

GMSA 'yı (grup yönetilen hizmet hesapları) ayarlamak için, kümedeki tüm düğümlere bir kimlik`credspec`bilgisi belirtim dosyası () yerleştirilir. Dosya, sanal makine uzantısı kullanılarak tüm düğümlere kopyalanabilir.  `credspec` Dosya gMSA hesap bilgilerini içermelidir. `credspec` Dosya hakkında daha fazla bilgi için bkz. [kimlik bilgisi belirtimi oluşturma](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Kimlik bilgisi belirtimi ve `Hostname` etiketi uygulama bildiriminde belirtilir. `Hostname` Etiket, kapsayıcının çalıştığı gMSA hesap adıyla eşleşmelidir.  `Hostname` Etiketi, kapsayıcının Kerberos kimlik doğrulaması kullanarak etki alanındaki diğer hizmetlere kimliğini doğrulamasına izin verir.  Aşağıdaki kod parçacığında, `Hostname` ve `credspec` uygulama bildiriminde öğesini belirtmek için bir örnek gösterilmektedir:

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
