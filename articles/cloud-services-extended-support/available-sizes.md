---
title: Azure Cloud Services için kullanılabilir Boyutlar (genişletilmiş destek)
description: Azure Cloud Services (genişletilmiş destek) dağıtımları için kullanılabilir Boyutlar
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: c3515b559ef647b9a043a04282b4edfc6e7fa9be
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744657"
---
# <a name="available-sizes-for-azure-cloud-services-extended-support"></a>Azure Cloud Services için kullanılabilir Boyutlar (genişletilmiş destek)

Bu makalede Cloud Services (genişletilmiş destek) örnekleri için kullanılabilir sanal makine boyutları açıklanmaktadır.   

| SKU Ailesi |  ACU/çekirdek | 
|---|---|
| [A5-7](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series)| 100 |
|[A8-A11](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#a-series---compute-intensive-instances) | 225* |
|[AV2](https://docs.microsoft.com/azure/virtual-machines/av2-series) | 100 | 
|[TID](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#d-series) | 160 | 
|[Dv2](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series) | 160-190 * |
|[Dv3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series) | 160-190 * |
|[Ev3](https://docs.microsoft.com/azure/virtual-machines/ev3-esv3-series) | 160-190 *
|[Acil](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#g-series) | 180-240 * |
|[Olsun](https://docs.microsoft.com/azure/virtual-machines/h-series) | 290-300* | 

>[!NOTE]
> * işaretli ACU'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo Boost teknolojisinden faydalanır. Performans artışının oranı VM boyutuna, iş yüküne ve aynı ana bilgisayarda çalışan iş yüklerine göre değişiklik gösterebilir.

## <a name="configure-sizes-for-cloud-services-extended-support"></a>Cloud Services için boyutları yapılandırın (genişletilmiş destek)

Hizmet tanımı dosyasındaki hizmet modelinin bir parçası olarak bir rol örneğinin sanal makine boyutunu belirtebilirsiniz. Rolün boyutu CPU çekirdeği sayısını, bellek kapasitesini ve yerel dosya sistemi boyutunu belirler.

Örneğin, Web rolü örnek boyutunu şu şekilde ayarlama `Standard_D2` : 

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2"> 
</WorkerRole> 
```

## <a name="change-the-size-of-an-existing-role"></a>Mevcut bir rolün boyutunu değiştirme

Mevcut bir rolün boyutunu değiştirmek için, hizmet tanımlama dosyasında (csdef) sanal makine boyutunu değiştirin, bulut hizmetinizi yeniden paketleyin ve yeniden dağıtın. 

## <a name="get-a-list-of-available-sizes"></a>Kullanılabilir boyutların bir listesini alın 

Kullanılabilir boyutların bir listesini almak için bkz. [kaynak SKU 'ları-Listele](https://docs.microsoft.com/rest/api/compute/resourceskus/list) ve aşağıdaki filtreleri Uygula:


`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.
