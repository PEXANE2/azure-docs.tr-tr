---
title: Azure ağ Izleyicisi için veri yerleşimi | Microsoft Docs
description: Bu makale, Azure ağ Izleyicisi hizmeti için veri fazlalığını anlamanıza yardımcı olur.
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
ms.openlocfilehash: 9451b6636f2f87806e3d1e39fec4e9e4d4390485
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706825"
---
# <a name="data-residency-for-azure-network-watcher"></a>Azure ağ Izleyicisi için veri yerleşimi
Bağlantı Izleyicisi (Önizleme) hizmeti dışında, Azure ağ Izleyicisi müşteri verilerini depolamaz.


## <a name="connection-monitor-preview-data-residency"></a>Bağlantı Izleyicisi (Önizleme) veri yerleşimi
Bağlantı Izleyicisi (Önizleme) hizmeti müşteri verilerini depolar. Bu veriler ağ Izleyicisi tarafından tek bir bölgede otomatik olarak depolanır. Bu nedenle bağlantı Izleyicisi (Önizleme), [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/)'nde belirtilen gereksinimler dahil olmak üzere bölge içi veri yerleşimi gereksinimlerini otomatik olarak karşılar.

## <a name="singapore-data-residency"></a>Singapur veri yerleşimi

Azure 'da, müşteri verilerinin tek bir bölgede depolanmasını sağlayan özelliği şu anda yalnızca Asya Pasifik coğrafi bölgenin Güneydoğu Asya bölgesinde (Singapur) kullanılabilir. Diğer tüm bölgeler için müşteri verileri coğrafi olarak depolanır. Daha fazla bilgi için bkz. [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)'ne genel bakış konusunu okuyun.
