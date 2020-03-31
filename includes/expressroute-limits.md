---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 07/25/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d1402bcc0c46003429e1809e8d09e9662218cd82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334608"
---
| Kaynak | Sınır |
| --- | --- |
| Abonelik başına ExpressRoute devreleri |10 |
| Azure Kaynak Yöneticisi ile abonelik başına bölge başına ExpressRoute devreleri |10 |
| ExpressRoute Standard ile Azure özel eşlemelerine reklamı yapılan maksimum rota sayısı |4.000 |
| ExpressRoute Premium eklentisi ile Azure özel eşlemelerine reklamı yapılan maksimum rota sayısı |10,000 |
| ExpressRoute bağlantısı için VNet adres alanından Azure özel eşlemelerinden reklamı yapılan maksimum rota sayısı |200 |
| ExpressRoute Standard ile eşleyen Microsoft'a reklamı yapılan maksimum rota sayısı |200 |
| ExpressRoute Premium eklentisi ile microsoft'a verilen maksimum rota sayısı |200 |
| Aynı sanal ağa aynı eşleme konumunda ki bağlantılı maksimum ExpressRoute devresi sayısı |4 |
| Farklı eşleme konumlarında aynı sanal ağa bağlı maksimum ExpressRoute devresi sayısı |4 |
| ExpressRoute devresi başına izin verilen sanal ağ bağlantısı sayısı |ExpressRoute devre tablosu [başına sanal ağ sayısına](#vnetpercircuit) bakın.  |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a><a name="vnetpercircuit"></a>ExpressRoute devresi başına sanal ağ sayısı
| **Devre boyutu** | **Standart için sanal ağ bağlantısı sayısı** | **Premium eklentiile sanal ağ bağlantısı sayısı** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mb/sn |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |
| 40 Gbps* |10 |100 |
| 100 Gbps* |10 |100 |

**Sadece 100 Gbps ExpressRoute Direct*

> [!NOTE]
> Global Reach bağlantıları ExpressRoute Circuit başına sanal ağ bağlantıları sınırına göre sayılır. Örneğin, 10 Gbps Premium Devre, ExpressRoute Ağ Geçitleri'ne 5 Global Erişim bağlantısı ve 95 bağlantı veya 95 Global Reach bağlantısı ve ExpressRoute Ağ GeçitLeri'ne veya 100 bağlantı sınırına kadar olan diğer kombinasyonlara 5 bağlantı için izin verir. devre için.
