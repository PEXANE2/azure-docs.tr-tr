---
title: Windows Virtual Desktop kullanıcı bağlantısı gecikmesi - Azure
description: Windows Sanal Masaüstü kullanıcıları için bağlantı gecikmesi.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: a4210947d771768943775a3e62c2558fa2883bd5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128185"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'nde kullanıcı bağlantısı gecikmesi belirleme

Windows Sanal Masaüstü genel olarak kullanılabilir. Yöneticiler istedikleri herhangi bir Azure bölgesinde sanal makineler (VM' ler) oluşturabilirler. Bağlantı gecikmesi, kullanıcıların ve sanal makinelerin konumuna bağlı olarak değişir. Windows Sanal Masaüstü hizmetleri, gecikme süresini artırmak için sürekli olarak yeni coğrafyalara sunulacaktır. 
 
[Windows Sanal Masaüstü Deneyimi Tahmincisi aracı,](https://azure.microsoft.com/services/virtual-desktop/assessment/) SANAL M'lerinizin gecikmesini en iyi duruma getirmek için en iyi konumu belirlemenize yardımcı olabilir. Windows Sanal Masaüstü yeni alanlara aktarılırken en uygun konumun değişmediğinden emin olmak için aracı her iki ila üç ayda bir kullanmanızı öneririz. 

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Windows Sanal Masaüstü, en yakın Windows Sanal Masaüstü hizmeti örneğini bulmak için kullanıcının DNS sunucusunun konumunu kontrol eden Azure Trafik Yöneticisi'ni kullanır. Yöneticilerin, VM'lerin konumunu seçmeden önce kullanıcının DNS sunucusunun konumunu gözden geçirmelerini öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- En iyi gecikme için en iyi konumu kontrol etmek için [Windows Sanal Masaüstü Deneyimi Tahminaracı'na](https://azure.microsoft.com/services/virtual-desktop/assessment/)bakın.
- Fiyatlandırma planları için [Windows Sanal Masaüstü fiyatlandırması'na](https://azure.microsoft.com/pricing/details/virtual-desktop/)bakın.
- Windows Sanal Masaüstü dağıtımınıza başlamak için [öğreticimize](tenant-setup-azure-active-directory.md)göz atın.