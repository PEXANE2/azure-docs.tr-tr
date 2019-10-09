---
title: Azure SQL veri ambarı 'nda bellek ve eşzamanlılık sınırları | Microsoft Docs
description: Azure SQL veri ambarı 'nda çeşitli performans düzeylerine ve kaynak sınıflarına ayrılan bellek ve eşzamanlılık sınırlarını görüntüleyin.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 10/04/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 73bb5b75a440755e088a4d9a294b5b97b0b7199e
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035138"
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için bellek ve eşzamanlılık sınırları
Azure SQL veri ambarı 'nda çeşitli performans düzeylerine ve kaynak sınıflarına ayrılan bellek ve eşzamanlılık sınırlarını görüntüleyin. Daha fazla bilgi edinmek ve bu özellikleri iş yükü yönetimi planınıza uygulamak için bkz. [iş yükü yönetimi Için kaynak sınıfları](resource-classes-for-workload-management.md). 

## <a name="data-warehouse-capacity-settings"></a>Veri ambarı kapasite ayarları
Aşağıdaki tablolarda, farklı performans düzeylerinde veri ambarı için maksimum kapasite gösterilmektedir. Performans düzeyini değiştirmek için bkz. [Ölçek işlem-portal](quickstart-scale-compute-portal.md).

### <a name="service-levels"></a>Hizmet düzeyleri

Hizmet düzeyleri DW100c ile DW30000c arasında değişir.

| Performans düzeyi | İşlem düğümleri | Işlem düğümü başına dağıtımlar | Veri ambarı başına bellek (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100c            | 1             | 60                             |    60                          |
| DW200c            | 1             | 60                             |   120                          |
| DW300c            | 1             | 60                             |   180                          |
| DW400c            | 1             | 60                             |   240                          |
| DW500c            | 1             | 60                             |   300                          |
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

En yüksek hizmet düzeyi, 60 Işlem düğümlerine ve Işlem düğümü başına bir dağıtıma sahip DW30000c. Örneğin, DW30000c adresindeki 600 TB veri ambarı, Işlem düğümü başına yaklaşık 10 TB işler.

## <a name="concurrency-maximums"></a>Eşzamanlılık en yüksek
Her sorgunun etkili şekilde yürütmek için yeterli kaynağa sahip olduğundan emin olmak için SQL veri ambarı her sorguya eşzamanlılık yuvaları atayarak kaynak kullanımını izler. Sistem, sorguları önem ve eşzamanlılık yuvaları temelinde bir kuyruğa koyar. Sorgular, yeterli eşzamanlılık yuvası olmadığından sırada bekler. [Önem](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-importance) ve EŞZAMANLıLıK yuvaları CPU önceliği belirlemeyi tespit. Daha fazla bilgi için bkz. [iş yükünüzü çözümleme](analyze-your-workload.md)

**Statik kaynak sınıfları**

Aşağıdaki tabloda her [statik kaynak sınıfı](resource-classes-for-workload-management.md)için en fazla eşzamanlı sorgu ve eşzamanlılık yuvaları gösterilmektedir.  

| Hizmet Düzeyi | En fazla eşzamanlı sorgu | Eşzamanlılık yuvaları kullanılabilir | Staticrc10 tarafından kullanılan yuvalar | Staticrc20 tarafından kullanılan yuvalar | Staticrc30 tarafından kullanılan yuvalar | Staticrc40 tarafından kullanılan yuvalar | Staticrc50 tarafından kullanılan yuvalar | Kullanıcıyı staticrc60 tarafından kullanılan yuvalar | Staticrc70 tarafından kullanılan yuvalar | Staticrc80 tarafından kullanılan yuvalar |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100c        |  4                         |    4                        | 1         | 2          | 4          | 4          | 4         |  4         |  4         |  4         |
| DW200c        |  8                         |    8                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |  8        |
| DW300c        | 12                         |   12                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400c        | 16                         |   16                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500c        | 20                         |   20                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
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

Aşağıdaki tabloda her [dinamik kaynak sınıfı](resource-classes-for-workload-management.md)için en fazla eşzamanlı sorgu ve eşzamanlılık yuvaları gösterilmektedir. Dinamik kaynak sınıfları, tüm hizmet düzeylerinde küçük orta ölçekli-xbüyük kaynak sınıfları için 3-10-22-70 bellek yüzdesi ayırması kullanır.

| Hizmet Düzeyi | En fazla eşzamanlı sorgu | Eşzamanlılık yuvaları kullanılabilir | Smallrc tarafından kullanılan yuvalar | Düz RC tarafından kullanılan yuvalar | Largerc tarafından kullanılan yuvalar | Xlargerc tarafından kullanılan yuvalar |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW100c        |  4                         |    4                        | 1                     |  1                     |  1                    |   2                    |
| DW200c        |  8                         |    8                        | 1                     |  1                     |  1                    |   5                    |
| DW300c        | 12                         |   12                        | 1                     |  1                     |  2                    |   8                    |
| DW400c        | 16                         |   16                        | 1                     |  1                     |  3                    |  11                    |
| DW500c        | 20                         |   20                        | 1                     |  2                     |  4                    |  14                    |
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


Sorgu yürütmeyi başlatmak için yeterli eşzamanlılık yuvası olmadığında sorgular kuyruğa alınır ve önem derecesine göre yürütülür.  Eşdeğer önem derecesi varsa, sorgular ilk kez, ilk çıkar temelinde yürütülür.  Sorgular bitene ve sorguların ve yuvaların sayısı limitlerin altına düşecek şekilde, SQL veri ambarı sıraya alınmış sorguları serbest bırakır. 

## <a name="next-steps"></a>Sonraki adımlar

İş yükünüzü iyileştirmek için kaynak sınıflarından yararlanma hakkında daha fazla bilgi edinmek için lütfen aşağıdaki makalelere göz atın:
* [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)
* [İş yükünüzü çözümleme](analyze-your-workload.md)