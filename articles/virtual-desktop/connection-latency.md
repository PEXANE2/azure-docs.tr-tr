---
title: Windows sanal masaüstü Kullanıcı bağlantısı gecikmesi-Azure
description: Windows sanal masaüstü kullanıcıları için bağlantı gecikme süresi.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8a60779fb045aa612a6ba0988c4635752f973f60
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82607410"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Windows sanal masaüstü 'nde Kullanıcı bağlantı gecikmesini belirleme

Windows sanal masaüstü küresel olarak kullanılabilir. Yöneticiler, istedikleri Azure bölgesinde sanal makineler (VM) oluşturabilir. Bağlantı gecikmesi, kullanıcıların ve sanal makinelerin konumuna göre değişir. Windows Sanal Masaüstü Hizmetleri, gecikme süresini artırmak için sürekli olarak yeni coğrafi bir şekilde kullanıma sunulacaktır. 
 
[Windows sanal masaüstü deneyimi Estimator Aracı](https://azure.microsoft.com/services/virtual-desktop/assessment/) , sanal makinelerinizin gecikmesini iyileştirmek için en iyi konumu belirlemenize yardımcı olabilir. Windows sanal masaüstü yeni alanlara kaydolurken en iyi konumun değişmediğinden emin olmak için aracı iki ile üç ayda bir kullanmanızı öneririz. 

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Windows sanal masaüstü, en yakın Windows sanal masaüstü hizmeti örneğini bulmak için kullanıcının DNS sunucusunun konumunu denetleyen Azure Traffic Manager kullanır. Yöneticiler, VM 'Lerin konumunu seçmeden önce kullanıcının DNS sunucusunun konumunu incelemenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- En iyi gecikme süresinin en iyi konumunu denetlemek için bkz. [Windows sanal masaüstü deneyimi Estimator Aracı](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Fiyatlandırma planları için bkz. [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Windows sanal masaüstü dağıtımınızı kullanmaya başlamak için [öğreticimize](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md)göz atın.