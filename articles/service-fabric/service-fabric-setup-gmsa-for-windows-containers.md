---
title: Azure Service Fabric kapsayıcı Hizmetleri için gMSA kurulumu | Microsoft Docs
description: Azure Service Fabric 'de çalışan bir kapsayıcı için gMSA kurulumunu hemen öğrenin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: subramar
ms.openlocfilehash: 09994c7676de8470efff1707598ddf32a48e41a0
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599192"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Service Fabric üzerinde çalışan Windows kapsayıcıları için gMSA 'Yı ayarlama

GMSA 'yı (grup yönetilen hizmet hesapları) ayarlamak için, kümedeki tüm düğümlere bir kimlik`credspec`bilgisi belirtim dosyası () yerleştirilir. Dosya, sanal makine uzantısı kullanılarak tüm düğümlere kopyalanabilir.  `credspec` Dosya gMSA hesap bilgilerini içermelidir. `credspec` Dosya hakkında daha fazla bilgi için bkz. [kimlik bilgisi belirtimi oluşturma](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). Kimlik bilgisi belirtimi ve `Hostname` etiketi uygulama bildiriminde belirtilir. `Hostname` Etiket, kapsayıcının çalıştığı gMSA hesap adıyla eşleşmelidir.  `Hostname` Etiketi, kapsayıcının Kerberos kimlik doğrulaması kullanarak etki alanındaki diğer hizmetlere kimliğini doğrulamasına izin verir.  Aşağıdaki kod parçacığında, `Hostname` `credspec` ve uygulama bildiriminde öğesini belirtmek için bir örnek gösterilmektedir:

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
