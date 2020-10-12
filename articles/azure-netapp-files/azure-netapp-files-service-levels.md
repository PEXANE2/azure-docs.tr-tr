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
ms.date: 10/06/2020
ms.author: b-juche
ms.openlocfilehash: 1c64bd10b34b61797cb3bf3de0cd7d2aa819e795
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777148"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp Files için hizmet düzeyleri
Hizmet düzeyleri, bir kapasite havuzunun özniteliğidir. Hizmet düzeyleri, birime atanan kota temelinde kapasite havuzundaki bir birim için izin verilen en yüksek aktarım hızına göre tanımlanır ve farklılaştırılır.

## <a name="supported-service-levels"></a>Desteklenen hizmet düzeyleri

Azure NetApp Files üç hizmet düzeyini destekler: *Ultra*, *Premium*ve *Standart*. 

* <a name="Ultra"></a>Ultra depolama

    Ultra Storage katmanı, sağlanan 1 TiB kapasiteye göre 128 ' e kadar MIB/sn işleme sağlar. 

* <a name="Premium"></a>Premium Depolama

    Premium depolama katmanı, sağlanan 1 TiB kapasiteye göre 64 ' e kadar MIB/sn işleme sağlar. 

* <a name="Standard"></a>Standart depolama

    Standart depolama katmanı, sağlanan 1 TiB kapasiteye göre 16 adede kadar MIB/sn performansı sağlar.

## <a name="throughput-limits"></a>İşleme sınırları

Bir birim için üretilen iş sınırı, aşağıdaki faktörlerin birleşimiyle belirlenir:
* Birimin ait olduğu kapasite havuzunun hizmet düzeyi
* Birime atanan kota  
* Kapasite havuzunun QoS türü (*Otomatik* veya *el ile*)  

### <a name="throughput-limit-examples-of-volumes-in-an-auto-qos-capacity-pool"></a>Bir otomatik QoS kapasite havuzundaki birimlerin aktarım hızı sınırı örnekleri

Aşağıdaki diyagramda, bir otomatik QoS kapasite havuzundaki birimlerin verimlilik limiti örnekleri gösterilmektedir:

![Hizmet düzeyi çizimi](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

* Yukarıdaki örnek 1 ' de, 2 GB kota atanmış Premium Depolama katmanına sahip bir otomatik QoS KAPASİTE havuzundan bir birime, 128 MIB/s (2 TiB * 64 MIB/s) aktarım hızı sınırı atanır. Bu senaryo, kapasite havuzu boyutu veya gerçek birim tüketimi ne olursa olsun geçerlidir.

* Yukarıdaki 2. örnekte, 100 GiB kotasının atandığı Premium Depolama katmanına sahip bir otomatik QoS KAPASİTE havuzundan bir birime, 6,25 MiB/sn (0,09765625 TiB * 64 MiB/s) aktarım hızı sınırı atanır. Bu senaryo, kapasite havuzu boyutu veya gerçek birim tüketimi ne olursa olsun geçerlidir.

### <a name="throughput-limit-examples-of-volumes-in-a-manual-qos-capacity-pool"></a>Bir el ile QoS kapasite havuzundaki birimlerin aktarım hızı sınırı örnekleri 

El ile QoS kapasite havuzu kullanıyorsanız, bir birimin kapasitesini ve iş üretimini bağımsız olarak atayabilirsiniz. El ile QoS kapasite havuzunda bir birim oluşturduğunuzda, üretilen iş (MIB/S) değerini belirtebilirsiniz. Bir el ile QoS kapasite havuzundaki birimlere atanan toplam verimlilik, havuzun boyutuna ve hizmet düzeyine bağlıdır. Bu, (TiB x hizmet düzeyi aktarım hızı/TiB içindeki kapasite havuzu boyutu) tarafından belirlenir. Örneğin, Ultra hizmet düzeyine sahip 10-TiB kapasite havuzunda birimler için kullanılabilir toplam 1280 MIB/sn (10 TiB x 128 MIB/s/TiB) üretilen iş kapasitesi vardır.

SAP HANA sistem için, bu kapasite havuzu aşağıdaki birimleri oluşturmak için kullanılabilir. Her birim, uygulama gereksinimlerinizi karşılamak için bireysel boyut ve aktarım hızı sağlar:

* SAP HANA veri hacmi: Boyut 4 TiB, en fazla 704 MIB/sn
* SAP HANA günlük birimi: Boyut 0,5 TiB, en fazla 256 MIB/sn
* SAP HANA paylaşılan birim: Boyut 1 TiB, en fazla 64 MIB/sn
* Yedekleme birimi SAP HANA: Boyut 4,5 TiB, en fazla 256 MIB/sn

Aşağıdaki diyagramda SAP HANA birimlerine yönelik senaryolar gösterilmektedir:

![QoS SAP HANA birim senaryoları](../media/azure-netapp-files/qos-sap-hana-volume-scenarios.png) 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md) 
- [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
- [Azure NetApp Files için Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/netapp/)
- [Birimin hizmet düzeyini dinamik olarak değiştirme](dynamic-change-volume-service-level.md) 
- [Bölgeler arası çoğaltma için hizmet düzeyi hedefleri](cross-region-replication-introduction.md#service-level-objectives)
