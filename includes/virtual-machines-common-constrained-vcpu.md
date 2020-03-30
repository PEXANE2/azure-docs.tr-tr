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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188366"
---
SQL Server veya Oracle gibi bazı veritabanı iş yükleri yüksek bellek, depolama ve G/Ç bant genişliği gerektirir, ancak yüksek çekirdek sayısı gerektirmez. Birçok veritabanı iş yükü CPU yoğun değildir. Azure, aynı bellek, depolama ve G/Ç bant genişliğini korurken yazılım lisanslama maliyetini azaltmak için VM vCPU sayısını sınırlandırabileceğiniz belirli VM boyutları sunar.

VCPU sayısı, orijinal VM boyutunun yarısı veya dörtte biri ile sınırlandırılabilir. Bu yeni VM boyutları, bunları tanımlamanızı kolaylaştırmak için etkin vCPUs sayısını belirten bir sonek vardır.

Örneğin, geçerli VM boyutu Standard_GS5 32 vCPUs, 448 GB RAM, 64 disk (256 TB'ye kadar) ve 80.000 IOP veya 2 GB/İş Bant genişliği yle birlikte gelir. Yeni VM boyutları Standard_GS5-16 ve Standard_GS5-8 sırasıyla 16 ve 8 aktif vCPUs ile birlikte gelir, bellek için Standard_GS5 özellikleri geri kalanı korurken, depolama, ve G / Ç bant genişliği.

SQL Server veya Oracle için alınan lisans ücretleri yeni vCPU sayısıyla sınırlıdır ve diğer ürünler yeni vCPU sayısına göre ücretlendirilmelidir. Bu da VM özelliklerinin aktif (faturalandırılabilir) vCPUs'lara oranının %50 ila %75 oranında artmasıyla sonuçlanır. Bu yeni VM boyutları, müşteri iş yüklerinin yazılım lisanslama maliyetini optimize ederken aynı bellek, depolama ve G/Ç bant genişliğini kullanmasına olanak tanır. Şu anda, işletim sistemi lisanslama içeren işlem maliyeti, özgün boyutu yla aynı kalır. Daha fazla bilgi için daha [uygun maliyetli veritabanı iş yükleri için Azure VM boyutlarına](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)bakın.


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
| Standard_E16-4s_v3  | 4    | E16s_v3 ile aynı |
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
| Standard_DS13 4_v2  | 4    | Same as DS13_v2 |
| Standard_DS13 2_v2  | 2    | Same as DS13_v2 |
| Standard_DS14-8_v2  | 8    | DS14_v2 ile aynı |
| Standard_DS14-4_v2  | 4    | DS14_v2 ile aynı |
