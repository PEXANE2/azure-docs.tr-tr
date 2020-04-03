---
title: Bellek ve eşzamanlılık sınırları
description: Azure Synapse Analytics'teki çeşitli performans düzeylerine ve kaynak sınıflarına ayrılan bellek ve eşzamanlılık sınırlarını görüntüleyin.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: c427c832eb613dddbff33ef6e67af63112e2f136
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586061"
---
# <a name="memory-and-concurrency-limits-for-azure-synapse-analytics"></a>Azure Synapse Analytics için bellek ve eşzamanlılık sınırları
Azure Synapse Analytics'teki çeşitli performans düzeylerine ve kaynak sınıflarına ayrılan bellek ve eşzamanlılık sınırlarını görüntüleyin.  

## <a name="data-warehouse-capacity-settings"></a>Veri ambarı kapasite ayarları
Aşağıdaki tablolar, farklı performans düzeylerinde veri ambarı için maksimum kapasiteyi gösterir. Performans düzeyini değiştirmek için bkz: [Ölçek hesaplama - portal.](quickstart-scale-compute-portal.md)

### <a name="service-levels"></a>Hizmet Düzeyleri

Hizmet düzeyleri DW100c ile DW30000c arasında değişir.

| Performans düzeyi | İşlem düğümleri | İşlem düğümü başına dağılımlar | Veri ambarı başına bellek (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100c            | 1             | 60                             |    60                          |
| DW200c            | 1             | 60                             |   120                          |
| DW300c            | 1             | 60                             |   180                          |
| DW400c            | 1             | 60                             |   240                          |
| DW500c'yi seçin            | 1             | 60                             |   300                          |
| DW1000c           | 2             | 30                             |   600                          |
| DW1500c           | 3             | 20                             |   900                          |
| DW2000c           | 4             | 15                             |  1200                          |
| DW2500c           | 5             | 12                             |  1500                          |
| DW3000c           | 6             | 10                             |  1800                          |
| DW5000c           | 10            | 6                              |  3000                          |
| DW6000c           | 12            | 5                              |  3600                          |
| DW7500c           | 15            | 4                              |  4500                          |
| DW10000c          | 20            | 3                              |  6000                          |
| DW15000c          | 30            | 2                              |  9000                          |
| DW30000c          | 60            | 1                              | 18000                          |

Maksimum hizmet düzeyi, 60 İşlem düğümü ve İşlem düğümü başına bir dağılıma sahip DW30000c'dir. Örneğin, DW30000c'deki 600 TB veri ambarı, Bilgi İşlem düğümü başına yaklaşık 10 TB'yi işler.

## <a name="concurrency-maximums-for-workload-groups"></a>İş yükü grupları için eşzamanlılık maksimumları
İş yükü [gruplarının](sql-data-warehouse-workload-isolation.md)devreye girmesiyle, eşzamanlılık yuvaları kavramı artık geçerli değildir.  İstek başına kaynaklar yüzde bazında ayrılır ve iş yükü grubu tanımında belirtilir.  Ancak, eşzamanlılık yuvaları kaldırılsa bile, hizmet düzeyine bağlı olarak sorgubaşına gereken en az kaynak miktarı vardır.  Aşağıdaki tabloda, hizmet düzeyleri arasında sorgu başına gereken minimum kaynak miktarı ve elde edilebilen ilişkili eşzamanlılık tanımlanmıştır. 

|Hizmet Düzeyi|Maksimum eşzamanlı sorgular|Min % REQUEST_MIN_RESOURCE_GRANT_PERCENT için desteklendi|
|---|---|---|
|DW100c|4|%25|
|DW200c|8|12.5%|
|DW300c|12|%8|
|DW400c|16|6.25%|
|DW500c'yi seçin|20|%5|
|DW1000c|32|%3|
|DW1500c|32|%3|
|DW2000c|48|%2|
|DW2500c|48|%2|
|DW3000c|64|1.5%|
|DW5000c|64|1.5%|
|DW6000c|128|%0,75|
|DW7500c|128|%0,75|
|DW10000c|128|%0,75|
|DW15000c|128|%0,75|
|DW30000c|128|%0,75|
||||

## <a name="concurrency-maximums-for-resource-classes"></a>Kaynak sınıfları için eşzamanlılık maksimumları
Her sorgunun verimli bir şekilde yürütülmesi için yeterli kaynağa sahip olduğundan emin olmak için, kaynak kullanımı her sorguya eşzamanlılık yuvaları atayarak izlenir. Sistem sorguları önem ve eşzamanlılık yuvalarına göre sıraya koyar. Sorgular, yeterli eşzamanlılık yuvası kullanılabilir olana kadar kuyrukta bekler. [Önem](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-importance) ve eşzamanlılık yuvaları CPU önceliklendirmesi belirler. Daha fazla bilgi için [bkz.](analyze-your-workload.md)

**Statik kaynak sınıfları**

Aşağıdaki tabloda, her [statik kaynak sınıfı](resource-classes-for-workload-management.md)için en fazla eşzamanlı sorgu ve eşzamanlılık yuvaları gösterilmektedir.  

| Hizmet Düzeyi | Maksimum eşzamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | staticrc10 tarafından kullanılan yuvalar | staticrc20 tarafından kullanılan yuvalar | staticrc30 tarafından kullanılan yuvalar | staticrc40 tarafından kullanılan yuvalar | staticrc50 tarafından kullanılan yuvalar | staticrc60 tarafından kullanılan yuvalar | staticrc70 tarafından kullanılan yuvalar | staticrc80 tarafından kullanılan yuvalar |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100c        |  4                         |    4                        | 1         | 2          | 4          | 4          | 4         |  4         |  4         |  4         |
| DW200c        |  8                         |    8                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |  8        |
| DW300c        | 12                         |   12                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400c        | 16                         |   16                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500c'yi seçin        | 20                         |   20                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dinamik kaynak sınıfları**

Aşağıdaki tablo, her [dinamik kaynak sınıfı](resource-classes-for-workload-management.md)için en fazla eşzamanlı sorguları ve eşzamanlılık yuvalarını gösterir. Dinamik kaynak sınıfları, tüm hizmet düzeylerinde küçük-orta-büyük-büyük kaynak sınıfları için 3-10-22-70 bellek yüzdesi ayırma kullanır.

| Hizmet Düzeyi | Maksimum eşzamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | smallrc tarafından kullanılan yuvalar | Mediumrc tarafından kullanılan yuvalar | Biggerc tarafından kullanılan yuvalar | xbiggerc tarafından kullanılan yuvalar |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW100c        |  4                         |    4                        | 1                     |  1                     |  1                    |   2                    |
| DW200c        |  8                         |    8                        | 1                     |  1                     |  1                    |   5                    |
| DW300c        | 12                         |   12                        | 1                     |  1                     |  2                    |   8                    |
| DW400c        | 16                         |   16                        | 1                     |  1                     |  3                    |  11                    |
| DW500c'yi seçin        | 20                         |   20                        | 1                     |  2                     |  4                    |  14                    |
| DW1000c       | 32                         |   40                        | 1                     |  4                     |  8                    |  28                    |
| DW1500c       | 32                         |   60                        | 1                     |  6                     |  13                   |  42                    |
| DW2000c       | 32                         |   80                        | 2                     |  8                     |  17                   |  56                    |
| DW2500c       | 32                         |  100                        | 3                     | 10                     |  22                   |  70                    |
| DW3000c       | 32                         |  120                        | 3                     | 12                     |  26                   |  84                    |
| DW5000c       | 32                         |  200                        | 6                     | 20                     |  44                   | 140                    |
| DW6000c       | 32                         |  240                        | 7                     | 24                     |  52                   | 168                    |
| DW7500c       | 32                         |  300                        | 9                     | 30                     |  66                   | 210                    |
| DW10000c      | 32                         |  400                        | 12                    | 40                     |  88                   | 280                    |
| DW15000c      | 32                         |  600                        | 18                    | 60                     | 132                   | 420                    |
| DW30000c      | 32                         | 1200                        | 36                    | 120                    | 264                   | 840                    |


Sorgu yürütmeyi başlatmak için yeterli eşzamanlılık yuvası olmadığında, sorgular öneme bağlı olarak sıraya alınır ve yürütülür.  Eşdeğer önem varsa, sorgular ilk katılım, ilk çıkış esasına göre yürütülür.  Sorgular biterken ve sorgu ve yuva sayısı sınırların altına düştükçe, SQL Veri Ambarı sıralı sorgular bırakır. 

## <a name="next-steps"></a>Sonraki adımlar

İş yükünüzü daha da optimize etmek için kaynak sınıfları hakkında daha fazla bilgi edinmek için lütfen aşağıdaki makaleleri inceleyin:
* [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)
* [İş yükünüzü analiz etme](analyze-your-workload.md)