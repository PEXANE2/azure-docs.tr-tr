---
title: Azure PowerShell Örnekleri - Service Fabric
description: PowerShell kullanarak Azure Service Fabric kümeleri, uygulamaları ve hizmetleri oluşturma ve yönetme hakkında bilgi edinin.
ms.topic: sample
ms.date: 11/29/2018
ms.custom: mvc
ms.openlocfilehash: 4b85fd604eb27f0963af882b41e823d800005dda
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86187107"
---
# <a name="azure-service-fabric-powershell-samples"></a>Azure Service Fabric PowerShell örnekleri

Aşağıdaki tablo, Service Fabric kümelerini, uygulamalarını ve hizmetlerini oluşturup yöneten PowerShell betiği örneklerinin bağlantılarını içerir.

[!INCLUDE [links to azure CLI and service fabric CLI](../../includes/service-fabric-powershell.md)]

| Komut Dosyası | Açıklama |
|-|-|
| **Küme oluşturma** ||
| [Küme oluşturma (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Bir Azure Service Fabric kümesi oluşturur. |
| **Kümeyi, düğümleri ve altyapıyı yönetme** ||
| [Uygulama sertifikası ekleme](./scripts/service-fabric-powershell-add-application-certificate.md)| Key Vault için bir x509 sertifikası oluşturur ve bunu kümenizdeki bir sanal makine ölçek kümesine dağıtır. |
| [Küme sanal makinelerinde RDP bağlantı noktası aralığını güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Dağıtılan bir kümedeki küme düğümü sanal makinelerinde RDP bağlantı noktası aralığını değiştirir.|
| [Küme düğümü sanal makineleri için yönetici kullanıcı adını ve parolasını güncelleştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Küme düğümü sanal makineleri için yönetici kullanıcı adını ve parolasını güncelleştirir. |
| [Yük dengeleyicide bağlantı noktası açma](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Belirli bir bağlantı noktasında gelen trafiğe izin vermek için Azure yük dengeleyicide bir uygulama bağlantı noktasını açın. |
| [Gelen ağ güvenlik grubu kuralı oluşturma](./scripts/service-fabric-powershell-add-nsg-rule.md) | Belirli bir bağlantı noktası üzerindeki kümeye gelen trafiğe izin vermek için bir gelen ağ güvenlik grubu kuralı oluşturun. |
| **Uygulamaları yönetme** ||
| [Uygulama dağıtma](./scripts/service-fabric-powershell-deploy-application.md)| Kümeye bir uygulama dağıtın.|
| [Uygulamayı yükseltme](./scripts/service-fabric-powershell-upgrade-application.md)| Uygulamayı yükseltin.|
| [Uygulamayı kaldırma](./scripts/service-fabric-powershell-remove-application.md)| Kümeden bir uygulama kaldırın.|
