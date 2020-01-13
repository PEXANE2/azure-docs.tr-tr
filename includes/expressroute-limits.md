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
ms.openlocfilehash: 1fc63dc53d61a8b2e26f97cc09a359b3f2c7665c
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901949"
---
| Kaynak | Varsayılan/en yüksek sınır |
| --- | --- |
| Abonelik başına ExpressRoute devreleri |10 |
| Abonelik başına her bölge için ExpressRoute devreleri, Azure Resource Manager ile |10 |
| ExpressRoute standardı ile Azure özel eşlemesine tanıtılan en fazla yol sayısı |4,000 |
| ExpressRoute Premium eklentisi ile Azure özel eşleme 'ye tanıtılan en fazla yol sayısı |10,000 |
| ExpressRoute bağlantısı için VNet adres alanından Azure özel eşlemeden tanıtılan en fazla yol sayısı |200 |
| ExpressRoute standardı ile Microsoft eşlemesine tanıtılan en fazla yol sayısı |200 |
| ExpressRoute Premium eklentisi ile Microsoft eşlemesi 'ne tanıtılan en fazla yol sayısı |200 |
| Aynı eşleme konumunda aynı sanal ağa bağlı olan maksimum ExpressRoute bağlantı hattı sayısı |4 |
| Farklı eşleme konumlarında aynı sanal ağa bağlı olan maksimum ExpressRoute bağlantı hattı sayısı |4 |
| ExpressRoute bağlantı hattı başına izin verilen sanal ağ bağlantısı sayısı |[ExpressRoute bağlantı hattı başına sanal ağ sayısını](#vnetpercircuit) görüntüleyin.  |

#### <a name="vnetpercircuit"></a>ExpressRoute bağlantı hattı başına sanal ağ sayısı
| **Devre boyutu** | **Standart için sanal ağ bağlantısı sayısı** | **Premium eklentisi olan sanal ağ bağlantısı sayısı** |
| --- | --- | --- |
| 50 Mb/sn |10 |20 |
| 100 Mb/sn |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mb/sn |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gb/sn |10 |100 |
| 40 Gbps* |10 |100 |
| 100 Gbps* |10 |100 |

*yalnızca *100 Gbps ExpressRoute doğrudan*

> [!NOTE]
> Global Reach bağlantı sayısı ExpressRoute bağlantı hattı başına sanal ağ bağlantısı sınırına göre sayılır. Örneğin, 10 Gbps Premium bir bağlantı, ExpressRoute ağ geçitlerine veya 95 Global Reach bağlantılara ve 95 bağlantıya ve ExpressRoute ağ geçitlerine yönelik 5 bağlantıya ve 100 bağlantı sınırına kadar olan diğer herhangi bir bileşime Global Reach izin verir devre için.
