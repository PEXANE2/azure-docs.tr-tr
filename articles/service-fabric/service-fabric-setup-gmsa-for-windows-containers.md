---
title: Azure Hizmet Kumaş ı konteyner hizmetleri için gMSA kurulumu
description: Azure Hizmet Kumaşı'nda çalışan bir kapsayıcı için Yönetilen Hizmet Hesapları (gMSA) kurulum grubunu şimdi öğrenin.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9873e2d7672412b0e1e22c6c2a774cf629fd728a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639216"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA'yı ayarlama

gMSA'yı (grup Yönetilen Hizmet Hesapları) ayarlamak için,`credspec`kümedeki tüm düğümlere bir kimlik bilgisi belirtimi dosyası yerleştirilir. Dosya bir VM uzantısı kullanılarak tüm düğümlerde kopyalanabilir.  Dosya `credspec` gMSA hesap bilgilerini içermelidir. `credspec` Dosya hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec) Kimlik bilgisi belirtimi ve `Hostname` etiket uygulama bildiriminde belirtilir. Etiket, `Hostname` kapsayıcının altında çalıştığı gMSA hesap adıile eşleşmelidir.  Etiket, `Hostname` kerberos kimlik doğrulamasını kullanarak kapsayıcının kendisini etki alanında bulunan diğer hizmetlere doğrulamasını sağlar.  Uygulama bildiriminde `Hostname` ve `credspec` içinde belirtilen bir örnek aşağıdaki parçacıkta gösterilir:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Bir sonraki adım olarak, aşağıdaki makaleleri okuyun:

* [Windows Server 2016'da Service Fabric'e Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux'ta Service Fabric'e Docker konteyneri dağıtma](service-fabric-get-started-containers-linux.md)
