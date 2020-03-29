---
title: Konuk OS aile 1 emeklilik bildirimi | Microsoft Dokümanlar
description: Azure Guest OS Ailesi 1'in ne zaman gerçekleştiği ve etkilenip etkilenmediğinizi belirleme hakkında bilgi sağlar
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
ms.service: cloud-services
ms.topic: article
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: ae2df6f47d99fc5d452a6d3ea70f2dd2e4e7416b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68359592"
---
# <a name="guest-os-family-1-retirement-notice"></a>Konuk İşletim Sistemi Ailesi 1 kullanımdan kaldırma bildirimi
OS Family 1'in emekliliği ilk olarak 1 Haziran 2013'te duyuruldu.

**2 Eylül 2014** Windows Server 2008 işletim sistemini temel alan Azure Guest işletim sistemi (Guest OS) Family 1.x resmi olarak kullanımdan kaldırıldı. Aile 1'i kullanarak yeni hizmetleri dağıtma veya varolan hizmetleri yükseltme girişimleri, Konuk İşletim Sistemi Ailesi 1'in kullanımdan çekildiğini bildiren bir hata iletisiyle başarısız olur.

**Kasım 3, 2014** Guest OS Family 1 için genişletilmiş destek sona erdi ve tamamen emekli oldu. Aile 1'deki tüm hizmetler etkilenecektir. Bu hizmetleri her an durdurabiliriz. El ile yükseltmediğiniz sürece hizmetlerinizin çalışmaya devam edeceğinin garantisi yoktur.

Başka sorularınız varsa, [Bulut Hizmetleri Forumları'nı](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) ziyaret edin veya [Azure desteğine başvurun.](https://azure.microsoft.com/support/options/)

## <a name="are-you-affected"></a>Etkilendin mi?
Aşağıdakilerden herhangi biri geçerliyse Bulut Hizmetleriniz etkilenir:

1. Bulut Hizmetiniz için ServiceConfiguration.cscfg dosyasında açıkça belirtilen "osFamily = "1" değeriniz vardır.
2. Bulut Hizmetiniz için ServiceConfiguration.cscfg dosyasında açıkça belirtilen osFamily için bir değere sahip değilsiniz. Şu anda, sistem bu durumda "1" varsayılan değerini kullanır.
3. Azure portalı Konuk İşletim Sistemi aile değerinizi "Windows Server 2008" olarak listeler.

Bulut hizmetlerinizden hangisinin hangi işletim sistemi ailesi tarafından çalıştığını bulmak için, önce [Azure PowerShell'i ayarlamanız](/powershell/azureps-cmdlets-docs) gerekse de aşağıdaki komut dosyasını Azure PowerShell'de çalıştırabilirsiniz. Komut dosyası hakkında daha fazla bilgi için Azure [Guest OS Family 1 End of Life: Haziran 2014'e](https://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx)bakın.

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Komut dosyası çıkışındaki OSFamily sütunu boşsa veya "1" içeriyorsa, bulut hizmetleriniz OS Family 1'in emekliliğinden etkilenir.

## <a name="recommendations-if-you-are-affected"></a>Etkilenirseniz öneriler
Bulut Hizmeti rollerinizi desteklenen Konuk İşletim Sistemi Ailelerinden birine geçirmenizi öneririz:

**Konuk İşletim Sistemi ailesi 4.x** - Windows Server 2012 R2 *(önerilir)*

1. Uygulamanızın SDK 2.1 veya daha sonra .NET framework 4.0, 4.5 veya 4.5.1 ile kullandığından emin olun.
2. osFamily özniteliğini ServiceConfiguration.cscfg dosyasındaki "4" olarak ayarlayın ve bulut hizmetinizi yeniden dağıtın.

**Misafir İşletim Sistemi ailesi 3.x** - Windows Server 2012

1. Uygulamanızın SDK 1.8 veya sonraki leri .NET framework 4.0 veya 4.5 ile kullandığından emin olun.
2. osFamily özniteliğini ServiceConfiguration.cscfg dosyasındaki "3" olarak ayarlayın ve bulut hizmetinizi yeniden dağıtın.

**Konuk İşletim Sistemi ailesi 2.x** - Windows Server 2008 R2

1. Uygulamanızın .NET framework 3.5 veya 4.0 ile SDK 1.3 ve üzeri kullandığından emin olun.
2. osFamily özniteliğini ServiceConfiguration.cscfg dosyasındaki "2" olarak ayarlayın ve bulut hizmetinizi yeniden dağıtın.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Misafir OS Ailesi 1 için Genişletilmiş Destek Sona Erdi 3 Kasım 2014
Guest OS ailesi 1'deki bulut hizmetleri artık desteklenmeyecek. Hizmet kesintisini önlemek için en kısa sürede aile 1'den geçiş edin.  

## <a name="next-steps"></a>Sonraki adımlar
En son [Konuk İşletim Sistemi sürümlerini](cloud-services-guestos-update-matrix.md)gözden geçirin.
