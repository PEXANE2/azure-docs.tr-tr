---
title: Performans seçenekleri – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Düğüm bilgi işlem, depolama ve bölgeler de dahil olmak üzere bir Hyperscale (Citus) sunucu grubu için seçenekler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 2/18/2020
ms.openlocfilehash: 1c9b4b1099bda69764aa7a1a5a984a6316e1047d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462420"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-performance-options"></a>PostgreSQL için Azure Veritabanı – Hyperscale (Citus) performans seçenekleri

## <a name="compute-and-storage"></a>İşlem ve depolama
 
Bir Hyperscale (Citus) sunucu grubunda, alt düğümler ve koordinatör düğüm için bağımsız olarak bilgi işlem ve depolama ayarlarını seçebilirsiniz.  Bilgi işlem kaynakları, temel donanımın mantıksal CPU'sunu temsil eden vCores olarak sağlanır. Sağlama için depolama boyutu, Hyperscale (Citus) sunucu grubunuzun koordinatör ve işçi düğümleri için kullanılabilir kapasiteyi ifade eder. Depolama veritabanı dosyaları, geçici dosyalar, işlem günlükleri ve Postgres sunucu günlükleri içerir. Toplam depolama miktarı, her işçi ve koordinatör düğümü için kullanılabilir G/Ç kapasitesini de tanımlar.
 
|                       | İşçi düğümü           | Koordinatör düğüm      |
|-----------------------|-----------------------|-----------------------|
| İşlem, vCores       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| vCore başına bellek, GiB | 8                     | 4                     |
| Depolama boyutu, TiB     | 0.5, 1, 2             | 0.5, 1, 2             |
| Depolama türü          | Genel amaç (SSD) | Genel amaç (SSD) |
| IOPS                  | 3 IOPS/GiB'a kadar      | 3 IOPS/GiB'a kadar      |


## <a name="regions"></a>Bölgeler
Hiperölçek (Citus) sunucu grupları aşağıdaki Azure bölgelerinde kullanılabilir:

* Kuzey ve Güney Amerika: 
    * Orta Kanada
    * Orta ABD
    * Doğu ABD
    * Doğu ABD 2
    * Orta Kuzey ABD
    * Batı ABD 2
* Asya Pasifik:
    * Doğu Avustralya
    * Doğu Japonya
    * Güney Kore - Orta
    * Güneydoğu Asya
* Avrupa:
    * Kuzey Avrupa
    * Güney Birleşik Krallık
    * Batı Avrupa

Bu bölgelerden bazıları başlangıçta tüm Azure aboneliklerinde etkinleştirilmeyebilir. Yukarıdaki listeden bir bölge kullanmak ve aboneliğinizde görmek istemiyorsanız veya bu listede olmayan bir bölge kullanmak istiyorsanız, bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açın.

## <a name="pricing"></a>Fiyatlandırma
En güncel fiyatlandırma bilgileri için hizmet fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/postgresql/)bakın.
Azure [portalı,](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) istediğiniz yapılandırmanın maliyetini görmek için seçtiğiniz seçeneklere bağlı olarak **Yapılandırma** sekmesinde aylık maliyeti gösterir. Azure aboneliğiniz yoksa, tahmini bir fiyat elde etmek için Azure fiyatlandırma hesaplayıcısını kullanabilirsiniz. Azure [fiyatlandırma hesap makinesi](https://azure.microsoft.com/pricing/calculator/) web sitesinde **Öğeleri Ekle'yi**seçin, **Veritabanları** kategorisini genişletin ve seçenekleri özelleştirmek **için PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'nı** seçin.
 
## <a name="next-steps"></a>Sonraki adımlar
Portalda nasıl [bir Hyperscale (Citus) sunucu grubu oluşturabilirsiniz](quickstart-create-hyperscale-portal.md)öğrenin.
