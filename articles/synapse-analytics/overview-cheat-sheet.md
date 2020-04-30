---
title: Tek bir sayfa-Azure SYNAPSE Analytics
description: Azure SYNAPSE Analytics aracılığıyla başvuru kılavuzu yürüyen Kullanıcı
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: aa93a816fc11158d928978bdec2dbf42119fa149
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424660"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Azure SYNAPSE Analytics, yemek sayfası

[!INCLUDE [preview](includes/note-preview.md)]

Azure SYNAPSE Analytics, hizmet ve önemli komutların temel kavramları boyunca size kılavuzluk eder. Bu makale, hem yeni öğrenenlere hem de temel Azure SYNAPSE konularının vurgulamaları isteyen kişilere yardımcı olur.

## <a name="architecture"></a>Mimari

> [!div class="mx-imgBorder"]
>![SYNAPSE mimarisi](media/overview-cheat-sheet/azure-synapse-architecture-cheat-sheet.png)

## <a name="concepts"></a>Kavramlar
| Adlar ve fiiller                         | Ne yapar?       |
|:---                                 |:---                 |
| **SYNAPSE çalışma alanı (Önizleme)** | Azure 'da bulut tabanlı kurumsal analizler gerçekleştirmek için güvenli kılınabilir bir işbirliği sınırı. Çalışma alanı belirli bir bölgeye dağıtılır ve ilişkili bir ADLSg2 hesabı ve dosya sistemi vardır (geçici verileri depolamak için). Çalışma alanı bir kaynak grubu altında. |
| **SQL Analytics**   | Analytics 'i havuzlarla veya isteğe bağlı yetenekler ile çalıştırın.  |
| **SQL havuzu**   | karşılık gelen veritabanları ile birlikte 0-N SQL sağlanan kaynaklar bir çalışma alanına dağıtılabilir. Her SQL havuzunun ilişkili bir veritabanı vardır. Bir SQL havuzu el ile veya otomatik olarak ölçeklendirilebilir, duraklatılıp devam edebilir. Bir SQL havuzu 100 DWU 'dan 30.000 DWU 'ya kadar ölçeklendirebilir.       |
| **İsteğe bağlı SQL (Önizleme)**   | Veri Gölü veri üzerinde T-SQL sorguları çalıştırmanıza olanak sağlayan, büyük ölçekli veriler için oluşturulmuş dağıtılmış veri işleme sistemi. Bu, sunucusuz olduğundan altyapıyı yönetmeniz gerekmez.       |
|**Apache Spark** | Spark havuzunda kullanılan Spark çalışma zamanı. Desteklenen geçerli sürüm, Python 3.6.1, Scala 2.11.12, Apache Spark 0,5 ve Delta Lake 0,3 için .NET desteğiyle Spark 2,4.  | 
| **Apache Spark Havuzu (Önizleme)**  | karşılık gelen veritabanları ile 0--N Spark tarafından sağlanan kaynaklar bir çalışma alanında dağıtılabilir. Spark havuzu otomatik duraklatılabilir, devam ettirilebilir ve ölçeklendirilebilir.  |
| **Spark uygulaması**  |   Bir sürücü işlemi ve bir yürütücü işlemleri kümesinden oluşur. Spark uygulaması Spark havuzunda çalışır.            |
| **Spark oturumu**  |   Spark uygulamasının Birleşik giriş noktası. Spark 'ın çeşitli işlevleri ve daha az sayıda yapı ile etkileşime geçmek için bir yol sağlar. Bir not defteri çalıştırmak için bir oturumun oluşturulması gerekir. Bir oturum, belirli bir boyuttaki belirli sayıda yürüticiler üzerinde çalışacak şekilde yapılandırılabilir. Bir not defteri oturumunun varsayılan yapılandırması 2 orta ölçekli yürütmeçiler üzerinde çalıştırılır. |
| **SQL Isteği**  |   Sorgu gibi işlem, SQL havuzu veya isteğe bağlı SQL üzerinden çalışır. |
|**Veri tümleştirme**| Çeşitli kaynaklar arasında veri alma ve çalışma alanı içinde veya çalışma alanı dışında çalışan etkinlikleri düzenleme özelliği sağlar.| 
|**Yapıtlar**| Bir kullanıcının veri kaynaklarını yönetmesi, geliştirilmesi, düzenlemeleri ve görselleştirmeleri için gereken tüm nesneleri kapsülleyen kavram.|
|**Not Defteri**| Scala, PySpark, C# ve mini SQL destekleyen etkileşimli ve reaktif veri bilimi ve mühendislik arabirimi. |
|**Spark iş tanımı**|Kodu ve bağımlılıklarını içeren derleme jar ile Spark işi göndermek için arabirim.|
|**Veri Akışı**|  Büyük veri dönüştürmesi yapmak için hiçbir kodlamaya gerek olmadan tam bir görsel deneyimi sağlar. Tüm iyileştirme ve yürütme işlemleri sunucusuz bir biçimde işlenir. |
|**SQL betiği**| Bir dosyaya kaydedilmiş SQL komutları kümesi. Bir SQL betiği bir veya daha fazla SQL deyimi içerebilir. SQL havuzu veya istek üzerine SQL istekleri aracılığıyla SQL istekleri çalıştırmak için kullanılabilir.|
|**İşlem hattı**| Bir görevi birlikte gerçekleştiren etkinliklerin mantıksal gruplandırması.|
|**Etkinlik**| Verilerin kopyalanması, bir not defteri veya bir SQL betiği çalıştırması gibi verilerde gerçekleştirilecek eylemleri tanımlar.|
|**Tetikleyici**| Bir işlem hattı yürütür. El ile veya otomatik olarak çalıştırılabilir (zamanlama, pencere veya olay tabanlı).|
|**Bağlı hizmet**| Dış kaynaklara bağlanmak için çalışma alanı için gereken bağlantı bilgilerini tanımlayan bağlantı dizeleri.|
|**Veri kümesi**|  Bir etkinlikte giriş ve çıkış olarak kullanılacak verileri işaret eden veya başvuruda bulunan verilerin adlandırılmış görünümü. Bu, bağlı bir hizmete aittir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanı oluşturma](quickstart-create-workspace.md)
- [SYNAPSE Studio 'Yu kullanma](quickstart-synapse-studio.md)
- [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
- [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
- [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)

