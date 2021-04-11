---
title: include dosyası
description: include dosyası
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: include
ms.date: 12/05/2019
ms.author: duau
ms.custom: include file
ms.openlocfilehash: b1465b9bf855d592d195277b616c860a23c179f6
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504726"
---
| Ağ Geçidi SKU 'SU | Saniyedeki bağlantı sayısı | Akış sayısı | Saniye başına mega bit | Paket/saniye | Bağlantı hattı bant genişliği | Ağ Geçidi tarafından tanıtılan yolların sayısı (MSEE 'e) | Ağ Geçidi tarafından öğrenilen yolların sayısı (MSEE) | Sanal ağdaki sanal makinelerin sayısı |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Temel SKU (kullanım dışı)** | Yok | Yok | 500 | Yok | Yok | Yok | Yok | Yok |
| **Standart SKU/ErGw1AZ** | 7.000 | 400,000 | >1.000 | >100.000 | 1 Gbps |  500 | 4.000 | 2.000 (bakım sırasında 1.000 ' e düşürün, daha sonra geri yükler.) | 
| **Yüksek performanslı SKU/ErGw2AZ** | 14.000 | 840.000 | >2.000 | 250.000 | 1 Gbps | 500 | ~ 9.500 (Sanal ağda 6.500 ' den fazla VM varsa, 4.000 ' e düşürün.) | 4.500 |
| **Ultra performans SKU 'SU/ErGw3AZ** | 16.000 | 950.000 | ~ 10.000 | 1.000.000 | 1 Gbps | 500 | ~ 9.500 | 11.000 |
