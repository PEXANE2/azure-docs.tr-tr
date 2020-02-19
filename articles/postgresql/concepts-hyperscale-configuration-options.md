---
title: Performans seçenekleri – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: Düğüm hesaplama, depolama ve bölgeler dahil olmak üzere hiper ölçek (Citus) sunucu grubu seçenekleri.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 2/18/2020
ms.openlocfilehash: 1c9b4b1099bda69764aa7a1a5a984a6316e1047d
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462420"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-performance-options"></a>PostgreSQL için Azure veritabanı – Hyperscale (Citus) performans seçenekleri

## <a name="compute-and-storage"></a>İşlem ve depolama
 
İşlem ve depolama ayarlarını, çalışan düğümleri ve bir hiper ölçek (Citus) sunucu grubundaki düzenleyici düğümü için bağımsız olarak seçebilirsiniz.  İşlem kaynakları, temel alınan donanımın mantıksal CPU 'sunu temsil eden sanal çekirdekler olarak sağlanır. Sağlama için depolama boyutu, Hiperscale (Citus) sunucu grubunuzdaki düzenleyici ve çalışan düğümlerinin kullanabildiği kapasiteyi ifade eder. Depolama veritabanı dosyalarını, geçici dosyaları, işlem günlüklerini ve Postgres sunucu günlüklerini içerir. Sağladığınız toplam depolama miktarı, her çalışan ve düzenleyici düğümü için kullanılabilen g/ç kapasitesini de tanımlar.
 
|                       | Çalışan düğümü           | Düzenleyici düğümü      |
|-----------------------|-----------------------|-----------------------|
| İşlem, sanal çekirdekler       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| VCore başına bellek, GiB | 8                     | 4                     |
| Depolama boyutu, TiB     | 0,5, 1, 2             | 0,5, 1, 2             |
| Depolama türü          | Genel amaçlı (SSD) | Genel amaçlı (SSD) |
| IOPS                  | En fazla 3 ıOPS/GiB      | En fazla 3 ıOPS/GiB      |


## <a name="regions"></a>Bölgeler
Hiper ölçek (Citus) sunucu grupları aşağıdaki Azure bölgelerinde kullanılabilir:

* Kuzey
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
* 'Ya
    * Kuzey Avrupa
    * Güney Birleşik Krallık
    * Batı Avrupa

Bu bölgelerden bazıları başlangıçta tüm Azure aboneliklerinde etkinleştirilmemiş olabilir. Yukarıdaki listeden bir bölge kullanmak ve bunu aboneliğinizde görmezseniz veya bu listede olmayan bir bölge kullanmak istiyorsanız, bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açın.

## <a name="pricing"></a>Fiyatlandırma
En güncel fiyatlandırma bilgileri için bkz. hizmet [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/).
İstediğiniz yapılandırmanın maliyetini görmek için [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) , seçtiğiniz seçeneklere göre **yapılandırma** sekmesindeki aylık maliyeti gösterir. Azure aboneliğiniz yoksa, tahmini bir fiyat almak için Azure Fiyatlandırma hesaplayıcısı ' nı kullanabilirsiniz. [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/) Web sitesinde, **öğe Ekle**' yi seçin, **veritabanları** kategorisini genişletin ve ardından seçenekleri özelleştirmek Için **PostgreSQL Için Azure veritabanı – hiper ölçek (Citus)** öğesini seçin.
 
## <a name="next-steps"></a>Sonraki adımlar
[Portalda bir Hyperscale (Citus) sunucu grubu oluşturmayı](quickstart-create-hyperscale-portal.md)öğrenin.
