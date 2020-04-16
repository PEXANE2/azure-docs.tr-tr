---
title: Hile sayfası - Azure Synapse Analytics
description: Azure Synapse Analytics'te yürüyen başvuru kılavuzu
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: aa93a816fc11158d928978bdec2dbf42119fa149
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424660"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Azure Synapse Analytics hile sayfası

[!INCLUDE [preview](includes/note-preview.md)]

Azure Synapse Analytics hile sayfası, hizmetin temel kavramları ve önemli komutları hakkında size rehberlik edecektir. Bu makale, hem yeni öğrenenler hem de temel Azure Sinaps konularının önemli anlarını vurgulamak isteyenler için yararlıdır.

## <a name="architecture"></a>Mimari

> [!div class="mx-imgBorder"]
>![Sinaps Mimarisi](media/overview-cheat-sheet/azure-synapse-architecture-cheat-sheet.png)

## <a name="concepts"></a>Kavramlar
| Isim ve fiiller                         | Ne yapar?       |
|:---                                 |:---                 |
| **Synapse Çalışma Alanı (önizleme)** | Azure'da bulut tabanlı kurumsal analitik yapmak için önemli bir işbirliği sınırı. Çalışma alanı belirli bir bölgede dağıtılır ve ilişkili bir ADLSg2 hesabı ve dosya sistemi (geçici verileri depolamak için) vardır. Çalışma alanı bir kaynak grubunun altındadır. |
| **SQL Analytics**   | Analizleri havuzlarla veya isteğe bağlı özelliklerle çalıştırın.  |
| **SQL havuzu**   | İlgili veritabanlarıile 0-N SQL sağlanan kaynaklar bir çalışma alanında dağıtılabilir. Her SQL havuzunun ilişkili bir veritabanı vardır. BIR SQL havuzu ölçeklenebilir, duraklatılmış ve el ile veya otomatik olarak devam ettirilebilir. Bir SQL havuzu 100 DWU'dan 30.000 DWU'ya kadar ölçeklenebilir.       |
| **İsteğe bağlı SQL (önizleme)**   | Veri gölündeki veriler üzerinde T-SQL sorguları çalıştırmanıza olanak tanıyan büyük ölçekli veriler için oluşturulmuş dağıtılmış veri işleme sistemi. Sunucusuzdur, böylece altyapıyı yönetmeniz gerekmez.       |
|**Apache Spark** | Kıvılcım havuzunda kullanılan çalışma süresi. Desteklenen geçerli sürüm Python 3.6.1 ile Spark 2.4, Scala 2.11.12, Apache Spark 0.5 ve Delta Lake 0.3 için .NET desteğidir.  | 
| **Apache Spark havuzu (önizleme)**  | İlgili veritabanlarıyla 0'dan N'ye Sağlanan Kaynaklar bir çalışma alanında dağıtılabilir. Bir Kıvılcım havuzu otomatik olarak duraklatılmış, devam ettirilebilir ve ölçeklenebilir.  |
| **Kıvılcım uygulaması**  |   Bir sürücü işlemi ve yürütme işlemleri kümesioluşur. Bir Kıvılcım uygulaması Bir Kıvılcım havuzunda çalışır.            |
| **Kıvılcım oturumu**  |   Bir kıvılcım uygulamasının birleşik giriş noktası. Bu Spark çeşitli işlevleri ve daha az sayıda yapılar ile etkileşim için bir yol sağlar. Not defterini çalıştırmak için bir oturumun oluşturulması gerekir. Oturum, belirli bir boyuttaki belirli sayıda yürütücüüzerinde çalışacak şekilde yapılandırılabilir. Bir not defteri oturumu için varsayılan yapılandırma 2 orta ölçekli uygulayıcıları üzerinde çalıştırmaktır. |
| **SQL İsteği**  |   SQL havuzu nda veya isteğe bağlı SQL'de çalıştırılaç sorgu gibi işlem. |
|**Veri Entegrasyonu**| Çeşitli kaynaklar arasında veri yutma ve çalışma alanı içinde veya çalışma alanı dışında çalışan etkinlikleri düzenleme olanağı verir.| 
|**Yapıtlar**| Bir kullanıcının veri kaynaklarını yönetmesi, geliştirmesi, düzenlemesi ve görselleştirmesi için gerekli tüm nesneleri kapsülleyen kavram.|
|**Not Defteri**| Scala, PySpark, C#ve SparkSQL'i destekleyen etkileşimli ve reaktif Veri Bilimi ve Mühendisliği arabirimi. |
|**Kıvılcım iş tanımı**|Kodu ve bağımlılıklarını içeren montaj kavanozuyla bir Spark işi göndermek için arabirim.|
|**Veri Akışı**|  Büyük veri dönüşümü yapmak için kodlama gerektirmeden tam görsel bir deneyim sağlar. Tüm optimizasyon ve yürütme sunucusuz bir şekilde işlenir. |
|**SQL betiği**| Bir dosyaya kaydedilmiş SQL komutları kümesi. BIR SQL komut dosyası bir veya daha fazla SQL deyimi içerebilir. SQL isteklerini SQL havuzu veya isteğe bağlı SQL üzerinden çalıştırmak için kullanılabilir.|
|**İşlem hattı**| Görevi birlikte gerçekleştiren etkinliklerin mantıksal gruplandırması.|
|**Etkinlik**| Veri kopyalama, Not Defteri çalıştırma veya SQL komut dosyası çalıştırma gibi veriler üzerinde gerçekleştirecek eylemleri tanımlar.|
|**Tetikleyici**| Bir boru hattı yürütür. El ile veya otomatik olarak çalıştırılabilir (zamanlama, yuvarlanan pencere veya olay tabanlı).|
|**Bağlı hizmet**| Çalışma alanının dış kaynaklara bağlanması için gereken bağlantı bilgilerini tanımlayan bağlantı dizeleri.|
|**Veri kümesi**|  Bir etkinlikte kullanılacak verileri giriş ve çıktı olarak işaretleyen veya başvuran verilerin görünümü adlandırılmış görünümü. Bağlantılı Bir Hizmete aittir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanı oluşturma](quickstart-create-workspace.md)
- [Synapse Studio'u kullanın](quickstart-synapse-studio.md)
- [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
- [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
- [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)

