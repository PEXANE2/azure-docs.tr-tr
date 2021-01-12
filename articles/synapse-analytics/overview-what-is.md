---
title: Azure Synapse Analytics nedir?
description: Azure SYNAPSE Analytics 'e genel bakış
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 10/28/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: e5b12632a60ad5580325fbcda294e1a600bb2b6b
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121711"
---
# <a name="what-is-azure-synapse-analytics"></a>Azure Synapse Analytics nedir?

Kurumsal analizler, ham, iyileştirilmiş veya yüksek düzeyde bir şekilde, her türlü veri için büyük ölçekte çalışmalıdır. Bu, genellikle kuruluşların büyük veri ve veri ambarı teknolojilerini, ilişkisel depolardaki ve veri klarındaki veriler arasında çalışan karmaşık veri işlem hatlarıyla birlikte bulundurmasına gerek duyar. Bu tür çözümlerin oluşturulması, saklanması ve güvende olması zordur. Insight işletmelerin ihtiyacı olan karmaşıklık gecikmeleri.

**Azure SYNAPSE** , veri ambarlarında ve büyük veri sistemlerinde öngörü süresini hızlandıran tümleşik bir analiz hizmetidir. Azure SYNAPSE, kurumsal veri ambarlarında kullanılan **SQL** teknolojilerinin en iyi yanı sıra, büyük veriler Için kullanılan **Spark** teknolojilerinde, veri TÜMLEŞTIRME ve etl/ELT için işlem **hatları** ve **Power BI**, **cosmosdb** ve **AzureML** gibi diğer Azure hizmetleriyle derin tümleştirme sunar.

## <a name="key-features--benefits"></a>Önemli özellikler & avantajları

### <a name="industry-leading-sql"></a>Sektör lideri SQL

* **SYNAPSE SQL** , kuruluşların standart ve tanıdık T-SQL deneyimlerini kullanarak veri depolama ve veri sanallaştırma senaryoları uygulamasına olanak tanıyan bir dağıtılmış sorgu sistemidir. Ayrıca, SQL 'in, akış ve makine öğrenimi senaryolarına yönelik yeteneklerini de genişletir.

* SYNAPSE SQL, gereksinimlerinize uyacak şekilde hem **sunucusuz** hem de **adanmış** kaynak modelleri sunarak tüketim ve faturalandırma seçenekleri sunar. Tahmin edilebilir performans ve maliyet için, SQL tablolarında depolanan verilere yönelik işlem gücü ayırmak üzere ayrılmış SQL havuzları oluşturun. Planlanmamış veya bursty iş yükleri için, her zaman kullanılabilir, sunucusuz SQL uç noktasını kullanın.
* Bulut veri kaynaklarından SQL tablolarına veri aktarmak için yerleşik **akış** yeteneklerini kullanın
* [T-SQL tahmin işlevini](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest) kullanarak verileri öğrenmek için **Machine LEARNING** MODELLERINI kullanarak AI 'yi SQL ile tümleştirin

### <a name="industry-standard-apache-spark"></a>Sektör standardı Apache Spark

**Azure SYNAPSE derin için Apache Spark** ve sorunsuz bir şekilde Apache Spark tümleştirilir; veri hazırlama, veri MÜHENDISLIĞI, ETL ve makine öğrenimi için kullanılan en popüler açık kaynaklı büyük veri motoru.

* Linux ve Apache Spark 2,4 için, Linux Foundation Delta Gölü desteğiyle yerleşik destek sayesinde, Mini tanımlı ile ML modelleri.
* Kümeleri yönetme konusunda endişelenmenize gerek kalmadan sizi serbest bırakmanızı sağlayan basitleştirilmiş kaynak modeli.
* Hızlı Spark başlangıç ve agresif otomatik ölçeklendirme.
* Spark için .NET için yerleşik destek, C# uzmanlığınızı ve mevcut .NET kodunuzu Spark uygulamasında yeniden kullanmanıza olanak tanır.

### <a name="interop-of-sql-and-apache-spark-on-your-data-lake"></a>Data Lake SQL ve Apache Spark birlikte çalışabilirliği

Azure SYNAPSE, SQL ve Spark ile birlikte kullanma arasındaki geleneksel teknoloji sınırlamalarını ortadan kaldırır. Gereksinimlerinize ve uzmanlığa göre sorunsuz bir şekilde karıştırabilir ve eşleştirebilirsiniz.

* Paylaşılan bir Hive uyumlu meta veri sistemi, Veri Gölü dosyalarda tanımlanan tabloların Spark veya Hive tarafından sorunsuz bir şekilde tüketilmesi için izin verir.
* SQL ve Spark, Veri Gölü içinde depolanan Parquet, CSV, TSV ve JSON dosyalarını doğrudan keşfedebilir ve analiz edebilir.
* SQL ve Spark veritabanları arasında devam eden veriler için hızlı ölçeklenebilir yükleme ve kaldırma

### <a name="built-in-data-integration-via-pipelines"></a>Ardışık düzen aracılığıyla yerleşik veri tümleştirmesi

Azure SYNAPSE, Azure SYNAPSE Analytics 'ten çıkmadan zengin ölçekli ETL işlem hatları oluşturmanıza olanak sağlayan aynı veri tümleştirme altyapısı ve deneyimleriyle birlikte yerleşik olarak gelir Azure Data Factory.

* 90 ' dan fazla veri kaynağından veri alma
* Veri akışı etkinlikleri ile Code-Free ETL
* Not defterlerini, Spark işlerini, saklı yordamları, SQL betiklerini ve daha fazlasını düzenleyin

### <a name="unified-management-monitoring-and-security"></a>Birleşik yönetim, izleme ve güvenlik

Azure SYNAPSE, kuruluşların analiz kaynaklarını yönetmesi, kullanımı ve etkinlikleri izlemesi ve güvenliği zorunlu kılmak için tek bir yol sunar.

* Analiz kaynaklarına erişimi basitleştirmek için kullanıcıları role atayın
* Veriler ve kod üzerinde ayrıntılı erişim denetimi
* SQL ve Spark genelinde kaynakları, kullanımı ve kullanıcıları izlemek için tek bir pano

### <a name="synapse-studio"></a>Synapse Studio

**SYNAPSE Studio** , her şeyi veri mühendislerine göre ele alan Web-Native deneyimidir ve tek bir konumda, tam bir çözüm oluşturmak için ihtiyaç duydukları her görevi yapmasına izin verir.

* Tek bir yerde uçtan uca analiz çözümü oluşturun: alma, İnceleme, hazırlama, düzenleme, görselleştirme
* SQL veya Spark kodu yazan veri mühendislerine yönelik sektör lideri üretkenlik: yazma, hata ayıklama ve performans iyileştirmesi
* Kurumsal CI/CD işlemleriyle tümleştirin

## <a name="engage-with-the-synapse-engineering-team"></a>SYNAPSE Mühendislik ekibiyle etkileşim kurun

- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse): geliştirme sorularını sorun.
- [Microsoft Q&soru sayfası](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html): Teknik sorular sorun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Synapse Analytics ile çalışmaya başlama](get-started.md)
* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [Sunucusuz SQL havuzu kullanma](quickstart-sql-on-demand.md)
