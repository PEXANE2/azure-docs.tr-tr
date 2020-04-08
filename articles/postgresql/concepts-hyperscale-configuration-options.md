---
title: Yapılandırma seçenekleri – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Düğüm bilgi işlem, depolama ve bölgeler de dahil olmak üzere bir Hyperscale (Citus) sunucu grubu için seçenekler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 4/6/2020
ms.openlocfilehash: a2c376ec2bd1f03b626c11b0d6a6c3850c9ef8c4
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804597"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-configuration-options"></a>PostgreSQL için Azure Veritabanı – Hyperscale (Citus) yapılandırma seçenekleri

## <a name="compute-and-storage"></a>İşlem ve depolama
 
Bir Hyperscale (Citus) sunucu grubunda, alt düğümler ve koordinatör düğüm için bağımsız olarak bilgi işlem ve depolama ayarlarını seçebilirsiniz.  Bilgi işlem kaynakları, temel donanımın mantıksal CPU'sunu temsil eden vCores olarak sağlanır. Sağlama için depolama boyutu, Hyperscale (Citus) sunucu grubunuzun koordinatör ve işçi düğümleri için kullanılabilir kapasiteyi ifade eder. Depolama veritabanı dosyaları, geçici dosyalar, işlem günlükleri ve Postgres sunucu günlükleri içerir.
 
|                       | İşçi düğümü           | Koordinatör düğüm      |
|-----------------------|-----------------------|-----------------------|
| İşlem, vCores       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| vCore başına bellek, GiB | 8                     | 4                     |
| Depolama boyutu, TiB     | 0.5, 1, 2             | 0.5, 1, 2             |
| Depolama türü          | Genel amaç (SSD) | Genel amaç (SSD) |
| IOPS                  | 3 IOPS/GiB'a kadar      | 3 IOPS/GiB'a kadar      |

Tek bir Hyperscale (Citus) düğümündeki toplam RAM miktarı seçilen vCore sayısına bağlıdır.

| vCores | Bir işçi düğümü, GiB RAM | Koordinatör düğüm, GiB RAM |
|--------|--------------------------|---------------------------|
| 4      | 32                       | 16                        |
| 8      | 64                       | 32                        |
| 16     | 128                      | 64                        |
| 32     | 256                      | 128                       |
| 64     | 432                      | 256                       |

Toplam depolama miktarı, her işçi ve koordinatör düğümü için kullanılabilir G/Ç kapasitesini de tanımlar.

| Depolama boyutu, TiB | Maksimum IOPS |
|-------------------|--------------|
| 0,5               | 1536        |
| 1                 | 3072        |
| 2                 | 6,148        |

Tüm Hyperscale (Citus) kümesi için, toplanan IOPS aşağıdaki değerlere çalışır:

| İşçi düğümleri | 0.5 TiB, toplam IOPS | 1 TiB, toplam IOPS | 2 TiB, toplam IOPS |
|--------------|---------------------|-------------------|-------------------|
| 2            | 3072               | 6144             | 12,296            |
| 3            | 4,608               | 9,216             | 18,444            |
| 4            | 6144               | 12,288            | 24,592            |
| 5            | 7,680               | 15,360            | 30,740            |
| 6            | 9,216               | 18,432            | 36,888            |
| 7            | 10,752              | 21,504            | 43,036            |
| 8            | 12,288              | 24,576            | 49,184            |
| 9            | 13,824              | 27,648            | 55,332            |
| 10           | 15,360              | 30,720            | 61,480            |
| 11           | 16,896              | 33,792            | 67,628            |
| 12           | 18,432              | 36,864            | 73,776            |
| 13           | 19,968              | 39,936            | 79,924            |
| 14           | 21,504              | 43,008            | 86,072            |
| 15           | 23,040              | 46,080            | 92,220            |
| 16           | 24,576              | 49,152            | 98,368            |
| 17           | 26,112              | 52,224            | 104,516           |
| 18           | 27,648              | 55,296            | 110,664           |
| 19           | 29,184              | 58,368            | 116,812           |
| 20           | 30,720              | 61,440            | 122,960           |

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
