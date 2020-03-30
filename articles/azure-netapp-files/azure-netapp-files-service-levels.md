---
title: Azure NetApp Dosyaları için hizmet düzeyleri | Microsoft Dokümanlar
description: Azure NetApp Dosyaları'nın hizmet düzeyleri için iş ortası performansını açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75832577"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp Files için hizmet düzeyleri
Hizmet düzeyleri, kapasite havuzunun bir özelliğidir. Hizmet düzeyleri, birim havuzunda, hacme atanan kotaya göre kapasite havuzundaki bir birim için izin verilen maksimum iş hacmi yle tanımlanır ve ayırt edilir.

## <a name="supported-service-levels"></a>Desteklenen hizmet düzeyleri

Azure NetApp Files üç hizmet seviyesini destekler: *Ultra,* *Premium*ve *Standart.* 

* <a name="Ultra"></a>Ultra depolama

    Ultra depolama katmanı, atanan hacim kotasının 1 TiB'i başına 128 MiB/s'ye kadar iş hacmi sağlar. 

* <a name="Premium"></a>Birinci sınıf depolama

    Premium depolama katmanı, atanan hacim kotasının 1 TiB'si başına 64 MiB/s'ye kadar iş hacmi sağlar. 

* <a name="Standard"></a>Standart depolama

    Standart depolama katmanı, atanan hacim kotasının 1 TiB'si başına 16 MiB/s'ye kadar üretim sağlar.

## <a name="throughput-limits"></a>İşleme sınırları

Bir birim için iş hacmi sınırı aşağıdaki faktörlerin birleşimi ile belirlenir:
* Hacmin ait olduğu kapasite havuzunun hizmet düzeyi
* Hacme atanan kota  

Bu kavram aşağıdaki diyagramda gösterilmiştir:

![Hizmet düzeyi illüstrasyon](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

Yukarıdaki Örnek 1'de, 2 TiB kota atanan Premium depolama katmanına sahip bir kapasite havuzundan 128 MiB/s (2 TiB * 64 MiB/s) bir iş hacmi sınırı atanır. Bu senaryo, kapasite havuzu boyutuveya fiili birim tüketimine bakılmaksızın geçerlidir.

Yukarıdaki Örnek 2'de, 100 GiB kotaatanan Premium depolama katmanına sahip bir kapasite havuzundan 6,25 MiB/s (0,09765625 TiB * 64 MiB/s) bir iş hacmi atanır. Bu senaryo, kapasite havuzu boyutuveya fiili birim tüketimine bakılmaksızın geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

- Farklı hizmet düzeylerinin fiyatı için [Azure NetApp Dosyaları fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/netapp/) bakın
- Kapasite havuzundaki kapasite tüketiminin hesaplanması için [Azure NetApp Dosyaları için Maliyet modeline](azure-netapp-files-cost-model.md) bakın 
- [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [Azure NetApp Dosyaları için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/netapp/) bakın