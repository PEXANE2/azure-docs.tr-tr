---
title: Yüksek kullanılabilirlik – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma kavramları
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 406b4f2ada665d65ee0452579e24744d1f7cfc63
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91314862"
---
# <a name="high-availability-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda yüksek kullanılabilirlik – hiper ölçek (Citus)

Yüksek kullanılabilirlik (HA), bir sunucu grubundaki her düğümün bekleyen çoğaltmalarını tutarak veritabanı kapalı kalma süresini önler. Düğüm kapalıysa, hiper ölçek (Citus) başarısız olan düğümden gelen bağlantıları bekleme durumuna geçirir. Yük devretme birkaç dakika içinde gerçekleşir ve Yükseltilen düğümlerin her zaman PostgreSQL zaman uyumlu akış çoğaltması aracılığıyla yeni verileri vardır.

Koordinatör düğümündeki HA avantajlarından yararlanmak için veritabanı uygulamalarının, bırakılan bağlantıları ve başarısız işlemleri algılaması ve yeniden denemesi gerekir. Yeni yükseltilen düzenleyiciye aynı bağlantı dizesiyle erişilebilecektir.

Kurtarma üç aşamaya ayrılabilir: algılama, yük devretme ve tam kurtarma.  Hiper ölçek (Citus) her düğümde düzenli aralıklarla sistem durumu denetimleri çalıştırır ve dört başarısız denetim, bir düğümün aşağı doğru olduğunu belirler. Hiper ölçek (Citus), bekleme konumuna birincil düğüm durumunu (yük devretme) yükseltir ve yeni bir bekleme moduna hazırlar.
Akış çoğaltma başlar ve yeni düğüm güncel hale getiriliyor.  Tüm veriler çoğaltıldığında, düğüm tam kurtarmaya ulaştı.

### <a name="next-steps"></a>Sonraki adımlar

- Hiper ölçek (Citus) sunucu grubunda [yüksek kullanılabilirliği nasıl etkinleştireceğinizi](howto-hyperscale-high-availability.md) öğrenin.
