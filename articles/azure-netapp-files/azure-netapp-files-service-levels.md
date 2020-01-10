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
ms.openlocfilehash: 0398cc6a5336141f51dde26ed7cf4cce8c2c0bb4
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832577"
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

- Farklı hizmet düzeylerinin fiyatı için [Azure NetApp Files fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/netapp/) bakın
- Kapasite havuzundaki kapasite tüketiminin hesaplanması için bkz. [maliyet modeli Azure NetApp Files](azure-netapp-files-cost-model.md) 
- [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [Azure NetApp Files için bkz. hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/netapp/)