---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 24fbab2bb637edf6c7e31215cdc7ac82bdb0ebab
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67188366"
---
SQL Server veya Oracle gibi bazı veritabanı iş yükleri yüksek bellek, depolama ve g/ç bant genişliği gerektirir, ancak yüksek çekirdekli bir sayı değildir. Birçok veritabanı iş yükü CPU yoğunluklu değildir. Azure, aynı bellek, depolama ve g/ç bant genişliğini koruyarak, VM vCPU sayısını, yazılım lisanslama maliyetini azaltmak üzere kısıtlamak için belirli VM boyutları sunar.

VCPU sayısı, özgün VM boyutunun bir yarısı veya bir çeyrekte kısıtlanabilir. Bu yeni VM boyutları, tanımanıza daha kolay hale getirmek için etkin vCPU sayısını belirten bir soneke sahiptir.

Örneğin, geçerli VM boyutu Standard_GS5, 32 vCPU, 448 GB RAM, 64 disk (256 TB 'a kadar) ve 80.000 IOPS veya 2 GB/s g/ç bant genişliği ile gelir. Standard_GS5-16 ve Standard_GS5-8 yeni VM boyutları, sırasıyla 16 ve 8 etkin vCPU ile birlikte gelir; bu arada bellek, depolama ve g/ç bant genişliği için Standard_GS5 özelliklerinin geri kalanını sürdürirken.

SQL Server veya Oracle için ücretlendirilen lisanslama ücretleri yeni vCPU sayısı ile sınırlıdır ve diğer ürünlerin yeni vCPU sayısı temelinde ücretlendirilmelidir. Bu, VM özelliklerinin etkin (Faturalanabilir) vCPU 'Lara oranını artırdığı için %50 ile %75 arasında bir sonuç verir. Bu yeni VM boyutları, müşteri iş yüklerinin, yazılım lisanslama maliyetini iyileştirirken aynı bellek, depolama ve g/ç bant genişliğini kullanmasına izin verir. Şu anda, işletim maliyeti, işletim sistemi lisansı içeren işlem maliyeti, özgün boyutuyla aynı kalır. Daha fazla bilgi için bkz. [Azure VM boyutları daha fazla maliyetli veritabanı iş yükleri için](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Adı                | Sanal işlemci | Özellikler           |
|---------------------|------|-----------------|
| Standard_M8-2ms     | 2    | M8ms ile aynı    |
| Standard_M8-4ms     | 4    | M8ms ile aynı    |
| Standard_M16-4ms    | 4    | M16ms ile aynı   |
| Standard_M16-8ms    | 8    | M16ms ile aynı   |
| Standard_M32-8ms    | 8    | M32ms ile aynı   |
| Standard_M32-16ms   | 16   | M32ms ile aynı   |
| Standard_M64-32ms   | 32   | M64ms ile aynı   |
| Standard_M64-16ms   | 16   | M64ms ile aynı   |
| Standard_M128-64ms  | 64   | M128ms ile aynı  |
| Standard_M128-32ms  | 32   | M128ms ile aynı  |
| Standard_E4 2s_v3   | 2    | E4s_v3 ile aynı  |
| Standard_E8 4s_v3   | 4    | E8s_v3 ile aynı  |
| Standard_E8 2s_v3   | 2    | E8s_v3 ile aynı  |
| Standard_E16 8s_v3  | 8    | E16s_v3 ile aynı |
| Standard_E16 4s_v3  | 4    | E16s_v3 ile aynı |
| Standard_E32 16s_v3 | 16   | E32s_v3 ile aynı |
| Standard_E32 8s_v3  | 8    | E32s_v3 ile aynı |
| Standard_E64 32s_v3 | 32   | E64s_v3 ile aynı |
| Standard_E64 16s_v3 | 16   | E64s_v3 ile aynı |
| Standard_GS4-8      | 8    | GS4 ile aynı     |
| Standard_GS4-4      | 4    | GS4 ile aynı     |
| Standard_GS5-16     | 16   | GS5 ile aynı     |
| Standard_GS5-8      | 8    | GS5 ile aynı     |
| Standard_DS11 1_v2  | 1    | DS11_v2 ile aynı |
| Standard_DS12 2_v2  | 2    | DS12_v2 ile aynı |
| Standard_DS12 1_v2  | 1    | DS12_v2 ile aynı |
| Standard_DS13 4_v2  | 4    | DS13_v2 ile aynı |
| Standard_DS13 2_v2  | 2    | DS13_v2 ile aynı |
| Standard_DS14 8_v2  | 8    | DS14_v2 ile aynı |
| Standard_DS14 4_v2  | 4    | DS14_v2 ile aynı |
