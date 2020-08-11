---
title: Azure ağ Izleyicisi için veri yerleşimi | Microsoft Docs
description: Ağ Izleyicisi hizmeti için veri fazlalığını anlama
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2020
ms.author: damendo
ms.openlocfilehash: 14b4d3568c129c77260b00d554db520809dfd670
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88068375"
---
# <a name="data-residency-for-the-azure-network-watcher"></a>Azure ağ Izleyicisi için veri yerleşimi
Azure ağ Izleyicisi, bağlantı Izleyicisi (Önizleme) hizmeti haricinde müşteri verilerini depolamaz.


## <a name="connection-monitor-preview-data-residency"></a>Bağlantı Izleyicisi (Önizleme) veri yerleşimi
*Bağlantı İzleyicisi (Önizleme)* hizmeti müşteri verilerini depolar. Bu veriler ağ Izleyicisi tarafından tek bir bölgede otomatik olarak depolanır. Bu nedenle *Bağlantı İzleyicisi (Önizleme)* , [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/)'nde belirtilen dahil olmak üzere bölge içi veri yerleşimi gereksinimlerini otomatik olarak karşılar.

## <a name="singapore-data-residency"></a>Singapur veri yerleşimi

Azure 'da, müşteri verilerinin tek bir bölgede depolanmasını etkinleştirme özelliği şu anda yalnızca Asya Pasifik coğrafi bölgenin Güneydoğu Asya bölgesinde (Singapur) kullanılabilir. Diğer tüm bölgeler için müşteri verileri coğrafi olarak depolanır. Daha fazla bilgi için bkz. [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/).

> [!Note]
> **Önceki çoğaltma** Müşterilerin Son Kullanıcı IP adresleriyle ilgili erişilebilirlik, gecikme süresi ve ağ topolojisi değişikliklerini izleyebilmesi için, Son Kullanıcı IP adreslerini ağ Izleyicisi örneğiyle depolama seçeneği vardır. Bu son kullanıcı IP adresleri, müşteri verileri olarak sınıflandırılabilir. 15 Temmuz 2020 itibariyle ağ Izleyicisi bu verileri tek bir bölgede deposakıyor. (Müşteri verileri artık HK 'ye çoğaltılmıyor.) Bu veriler artık HK 'ye çoğaltılmıyor. Bu müşteri verileri, bekleyen olarak şifrelenir.
> 
> Bu müşteri verilerine üçüncü bir şahıs erişebiliyorsa, üçüncü tarafın IP adresini bilmesini sağlar, ancak üçüncü tarafa IP adresiyle ilişkili makineye veya cihaza erişim izni vermez. Ağ Izleyicisi, HK 'de bu müşteri verilerine hiçbir üçüncü taraf erişmeyeceğini düşündüğü.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)'ne genel bakış konusunu okuyun.
