---
title: Yüksek kullanılabilirlik – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma kavramları
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 10679ab02826fb606af65c72621f2afb609bc81b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975542"
---
# <a name="high-availability-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure Veritabanı'nda yüksek kullanılabilirlik – Hyperscale (Citus)

Yüksek kullanılabilirlik (HA), bir sunucu grubundaki her düğümün bekleme yinelemelerini koruyarak veritabanı kapalı kalma süresini önler. Bir düğüm aşağı inerse, Hyperscale gelen bağlantıları başarısız düğümden bekleme konumuna geçer. Failover birkaç dakika içinde olur ve tanıtılan düğümleri her zaman PostgreSQL senkron akış çoğaltma yoluyla taze veri var.

Koordinatör düğümdeki HA'dan yararlanmak için veritabanı uygulamalarının bırakılan bağlantıları ve başarısız hareketleri algılaması ve yeniden denemesi gerekir. Yeni tanıtılan koordinatöre aynı bağlantı dizesiyle erişilebilir.

Kurtarma üç aşamaya ayrılabilir: algılama, başarısızlık ve tam kurtarma.  Hiper ölçek her düğümde periyodik sistem durumu denetimleri çalıştırılır ve dört başarısız denetimden sonra düğümün aşağıda olduğunu belirler. Hyperscale daha sonra birincil düğüm durumuna (failover) bir bekleme teşvik eder ve yeni bir bekleme-to-be hükümleri.
Akış çoğaltma başlar, yeni düğüm güncel getirerek.  Tüm veriler çoğaltıldığında, düğüm tam kurtarma ulaştı.

### <a name="next-steps"></a>Sonraki adımlar

- Bir Hiper ölçekli sunucu grubunda [yüksek kullanılabilirliği](howto-hyperscale-high-availability.md) nasıl etkinleştirin öğrenin.
