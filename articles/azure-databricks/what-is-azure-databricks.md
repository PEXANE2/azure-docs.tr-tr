---
title: Azure Databricks nedir?
description: Azure Databricks hakkında bilgi edinin ve Azure 'a Databricks 'te Spark 'u nasıl getirirsiniz. Azure Databricks, Microsoft Azure bulut hizmetleri platformu için iyileştirilmiş Apache Spark tabanlı bir analiz platformudur.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: overview
ms.date: 05/08/2019
ms.author: mamccrea
ms.custom: mvc
ms.openlocfilehash: 149b83b9aacf23cde06a2b35aed06c164afe9647
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243319"
---
# <a name="what-is-azure-databricks"></a>Azure Databricks nedir?

Azure Databricks, Microsoft Azure bulut hizmetleri platformu için iyileştirilmiş Apache Spark tabanlı bir analiz platformudur. Apache Spark, tek tıklamayla kurulum, kolaylaştırılmış iş akışları ve veri bilimcileri, veri mühendisleri ve iş analistleri arasında işbirliğini sağlayan etkileşimli bir çalışma alanı sağlamak için, Databricks, Azure ile tümleşiktir.

![Azure Databricks nedir?](./media/what-is-azure-databricks/azure-databricks-overview.png "Azure Databricks nedir?")

Azure Databricks, hızlı, kolay ve işbirliğine dayalı Apache Spark tabanlı bir analiz hizmetidir. Büyük bir veri işlem hattı için, verilerin (ham veya yapılandırılmış) toplu Azure Data Factory aracılığıyla Azure 'a alınır veya Kafka, Olay Hub 'ı veya IoT Hub kullanılarak gerçek zamanlı olarak neredeyse bir akışa alınır. Bu veriler, Azure Blob depolama veya Azure Data Lake Storage uzun süreli kalıcı depolama için bir veri Gölü içinde yer ister. Analiz iş akışınızın bir parçası olarak, [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md), [Azure Data Lake Storage](../data-lake-store/index.md), [Azure Cosmos DB](../cosmos-db/index.yml)veya [Azure SQL veri ambarı](../sql-data-warehouse/index.yml) gibi birden çok veri kaynağından veri okumak için Azure Databricks kullanın ve bu verileri breakinto 'ya dönüştürün Spark kullanan Öngörüler.

![Databricks işlem hattı](./media/what-is-azure-databricks/databricks-pipeline.png)

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark tabanlı analiz platformu

Azure Databricks, tüm açık kaynaklı Apache Spark kümesi teknolojilerini ve yeteneklerini kapsar. Azure Databricks Spark aşağıdaki bileşenleri içerir:

![](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Azure Databricks içinde Azure Databricks Apache Spark") Apache Spark

* **Spark SQL ve DataFrames**: Spark SQL, yapılandırılmış verilerle çalışmaya yönelik Spark modülüdür. DataFrame, adlandırılmış sütunlara düzenlenmiş verilerin dağıtılmış bir koleksiyonudur. İlişkisel bir veritabanındaki bir tabloya veya R/Python 'daki veri çerçevesine kavramsal olarak eşdeğerdir.

* **Akış**: analitik ve etkileşimli uygulamalar için gerçek zamanlı veri işleme ve çözümleme. ,,, Ve Kafka ile tümleşir.

* **Mlib**: sınıflandırma, gerileme, kümeleme, işbirliğine dayalı filtreleme, boyutlandırma ve temel iyileştirme temelleri dahil olmak üzere ortak öğrenme algoritmalarından ve yardımcı programlarından oluşan Machine Learning kitaplığı.

* **GraphX**: bilişsel analizden veri araştırmasına kadar geniş bir kullanım durumu kapsamı için grafikler ve grafik hesaplama.

* **Spark Core API 'si**: R, SQL, Python, Scala ve Java için destek içerir.

## <a name="apache-spark-in-azure-databricks"></a>Azure Databricks Apache Spark

Azure Databricks, aşağıdakileri içeren sıfır yönetim bulut platformu sunarak Spark 'ın yeteneklerini oluşturur:

- Tam olarak yönetilen Spark kümeleri
- Araştırma ve görselleştirme için etkileşimli çalışma alanı
- En sevdiğiniz Spark tabanlı uygulamalarınızı güçlerinizi destekleyen bir platform

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Bulutta tam olarak yönetilen Apache Spark kümeleri

Azure Databricks, Spark uzmanları tarafından yönetilen ve desteklenen bulutta güvenli ve güvenilir bir üretim ortamına sahiptir. Şunları yapabilirsiniz:

* Saniyeler içinde kümeler oluşturun.
* Sunucusuz kümeler dahil olmak üzere kümeleri yukarı ve aşağı dinamik olarak otomatik ölçeklendirme ve takımlar genelinde paylaşma. 
* REST API 'Lerini kullanarak kümeleri programlı bir şekilde kullanın. 
* Verilerinizi merkezileşmeyi olmadan birleştirmenize olanak tanıyan, Spark üzerinde oluşturulmuş güvenli veri tümleştirme yeteneklerini kullanın. 
* Her sürümde en son Apache Spark özelliklerine anında erişim sağlayın.

### <a name="databricks-runtime"></a>Databricks Runtime
Databricks Runtime, Apache Spark üzerine kurulmuştur ve Azure bulutu için yerel olarak oluşturulmuştur. 

**Sunucusuz** seçeneğiyle, Azure Databricks altyapı karmaşıklığını ve veri altyapınızı ayarlamak ve yapılandırmak için uzman uzmanlık gereksinimini tamamen soyutlar. Sunucusuz seçeneği, veri bilimcilerinin bir ekip olarak hızla yineleme yapılmasına yardımcı olur.

Azure Databricks, üretim işlerinin performansından sorumlu olan veri mühendisleri için g/ç katmanında ve işleme katmanında (Databricks g/ç) çeşitli iyileştirmeler aracılığıyla daha hızlı ve performans sağlayan bir Spark altyapısı sağlar.

### <a name="workspace-for-collaboration"></a>İşbirliği için çalışma alanı

İşbirlikçi ve tümleşik bir ortam sayesinde Azure Databricks, verileri keşfetme, Prototipleme ve Spark 'ta veri odaklı uygulamaları çalıştırma sürecini kolaylaştırır.

* Verileri kolay bir şekilde araştırmayla nasıl kullanacağınızı öğrenin.
* İlerleme durumunu R, Python, Scala veya SQL 'deki not defterlerinde belgeleyin.
* Verileri birkaç tıklamayla görselleştirin ve Matplotlib, ggçizimi veya D3 gibi tanıdık araçları kullanın.
* Dinamik raporlar oluşturmak için etkileşimli panoları kullanın.
* Spark kullanın ve verilerle aynı anda etkileşim kurun.

## <a name="enterprise-security"></a>Kurumsal Güvenlik

Azure Databricks, Azure Active Directory tümleştirme, rol tabanlı denetimler ve verilerinizi ve işinizi koruyan SLA 'Lar dahil olmak üzere kurumsal düzeyde Azure güvenliği sağlar.

* Azure Active Directory ile tümleştirme, Azure Databricks kullanarak tam Azure tabanlı çözümler çalıştırmanızı sağlar.
* Azure Databricks rol tabanlı erişim, Not defterleri, kümeler, işler ve veriler için ayrıntılı Kullanıcı izinleri sunar.
* Kurumsal sınıf SLA 'Lar. 

## <a name="integration-with-azure-services"></a>Azure hizmetleriyle tümleştirme

Azure Databricks, Azure veritabanları ve depoları ile tümleşir: SQL veri ambarı, Cosmos DB, Data Lake Store ve BLOB depolama. 

## <a name="integration-with-power-bi"></a>Power BI ile tümleştirme
Power BI ile zengin tümleştirme sayesinde, Azure Databricks etkileyici öngörülerinizi hızlı ve kolay bir şekilde keşfetmenizi ve paylaşmanızı sağlar. JDBC/ODBC küme uç noktaları aracılığıyla Tableau yazılımı gibi diğer bı araçlarını da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure Databricks Spark işi çalıştırma](quickstart-create-databricks-workspace-portal.md)
* [Spark kümeleriyle çalışma](https://docs.azuredatabricks.net/user-guide/clusters/index.html)
* [Not defterleri ile çalışma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)
* [Spark işleri oluşturma](https://docs.azuredatabricks.net/user-guide/jobs.html)

 









