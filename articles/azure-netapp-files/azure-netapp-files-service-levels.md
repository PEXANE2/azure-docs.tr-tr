---
title: Azure NetApp Files için hizmet düzeyleri | Microsoft Docs
description: Azure NetApp Files hizmet düzeyleri için üretilen iş performansını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: b-juche
ms.openlocfilehash: 639f1e09fdb5603965209e5b5ee6c224ad238b76
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533130"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp Files için hizmet düzeyleri
Hizmet düzeyleri, bir kapasite havuzunun özniteliğidir. Hizmet düzeyleri, birime atanan kota temelinde kapasite havuzundaki bir birim için izin verilen en yüksek aktarım hızına göre tanımlanır ve farklılaştırılır.

## <a name="supported-service-levels"></a>Desteklenen hizmet düzeyleri

Azure NetApp Files üç hizmet düzeyini destekler: *Ultra*, *Premium*ve *Standart*. 

* <a name="Ultra"></a>Ultra depolama

    Ultra Storage katmanı, 1 TiB başına birim kotasının atandığı 128 MIB/sn 'ye kadar işleme sağlar. 

* <a name="Premium"></a>Premium Depolama

    Premium depolama katmanı, atanan birim kotasının 1 TiB 'ye kadar en fazla 64 MIB/sn kapasitesi sağlar. 

* <a name="Standard"></a>Standart depolama

    Standart depolama katmanı, 1 TiB başına birim kotası atanan 16 adede kadar MIB/sn aktarım hızı sağlar.

## <a name="throughput-limits"></a>İşleme sınırları

Bir birim için üretilen iş sınırı, aşağıdaki faktörlerin birleşimiyle belirlenir:
* Birimin ait olduğu kapasite havuzunun hizmet düzeyi
* Birime atanan kota  

Bu kavram aşağıdaki diyagramda gösterilmiştir:

![Hizmet düzeyi çizimi](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

Yukarıdaki örnek 1 ' de, 2 GB kota atanmış olan Premium Depolama katmanına sahip bir kapasite havuzundan bir birime 128 MIB/s (2 TiB * 64 MiB/sn) aktarım hızı sınırı atanır. Bu senaryo, kapasite havuzu boyutu veya gerçek birim tüketimi ne olursa olsun geçerlidir.

Yukarıdaki 2. örnekte, 100 GiB kotasının atandığı Premium Depolama katmanına sahip bir kapasite havuzundan bir birime, 6,25 MiB/sn (0,09765625 TiB * 64 MiB/s) aktarım hızı sınırı atanır. Bu senaryo, kapasite havuzu boyutu veya gerçek birim tüketimi ne olursa olsun geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md) 
- [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [Azure NetApp Files için Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/netapp/)
- [Birimin hizmet düzeyini dinamik olarak değiştirme](dynamic-change-volume-service-level.md) 