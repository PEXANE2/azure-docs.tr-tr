---
title: Azure Synapse Analytics nedir?
description: Azure SYNAPSE Analytics 'e genel bakış
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 03/24/2021
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 78b31cb32e3df9b0d8e198d8c2122e492e609283
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105625827"
---
# <a name="what-is-azure-synapse-analytics"></a>Azure Synapse Analytics nedir?

**Azure SYNAPSE** , veri ambarlarında ve büyük veri sistemlerinde öngörü süresini hızlandıran bir kurumsal analiz hizmetidir. Azure SYNAPSE, kurumsal veri ambarlarında kullanılan **SQL** teknolojilerinin en iyi yanı sıra, büyük veriler Için kullanılan **Spark** teknolojilerinde, veri TÜMLEŞTIRME ve etl/ELT için işlem **hatları** ve **Power BI**, **cosmosdb** ve **AzureML** gibi diğer Azure hizmetleriyle derin tümleştirme sunar.

![Azure SYNAPSE Analytics mimarisinin diyagramı.](./media/overview-what-is/synapse-architecture.png)

## <a name="industry-leading-sql"></a>Sektör lideri SQL

**SYNAPSE SQL** , veri depolama ve veri sanallaştırma senaryolarına olanak tanıyan ve t-SQL ' i, akış ve makine öğrenimi senaryolarına yönelik olarak genişleten t-SQL için dağıtılmış bir sorgu sistemidir.

* SYNAPSE SQL, **sunucusuz** ve **ayrılmış** kaynak modellerini sunar. Tahmin edilebilir performans ve maliyet için, SQL tablolarında depolanan verilere yönelik işlem gücü ayırmak üzere ayrılmış SQL havuzları oluşturun. Planlanmamış veya bursty iş yükleri için, her zaman kullanılabilir, sunucusuz SQL uç noktasını kullanın.
* Bulut veri kaynaklarından SQL tablolarına veri aktarmak için yerleşik **akış** yeteneklerini kullanın
* [T-SQL tahmin işlevini](/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest&preserve-view=true) kullanarak verileri öğrenmek için **Machine LEARNING** MODELLERINI kullanarak AI 'yi SQL ile tümleştirin

## <a name="industry-standard-apache-spark"></a>Sektör standardı Apache Spark

**Azure SYNAPSE derin için Apache Spark** ve sorunsuz bir şekilde Apache Spark tümleştirilir; veri hazırlama, veri MÜHENDISLIĞI, ETL ve makine öğrenimi için kullanılan en popüler açık kaynaklı büyük veri motoru.

* Linux ve Apache Spark 2,4 için, Linux Foundation Delta Gölü desteğiyle yerleşik destek sayesinde, Mini tanımlı ile ML modelleri.
* Kümeleri yönetme konusunda endişelenmenize gerek kalmadan sizi serbest bırakmanızı sağlayan basitleştirilmiş kaynak modeli.
* Hızlı Spark başlangıç ve agresif otomatik ölçeklendirme.
* Spark için .NET için yerleşik destek, C# uzmanlığınızı ve mevcut .NET kodunuzu Spark uygulamasında yeniden kullanmanıza olanak tanır.

## <a name="working-with-your-data-lake"></a>Data Lake çalışma

Azure SYNAPSE, SQL ve Spark ile birlikte kullanma arasındaki geleneksel teknoloji sınırlamalarını ortadan kaldırır. Gereksinimlerinize ve uzmanlığa göre sorunsuz bir şekilde karıştırabilir ve eşleştirebilirsiniz.

* Paylaşılan bir Hive uyumlu meta veri sistemi, Veri Gölü dosyalarda tanımlanan tabloların Spark veya Hive tarafından sorunsuz bir şekilde tüketilmesi için izin verir.
* SQL ve Spark, Veri Gölü içinde depolanan Parquet, CSV, TSV ve JSON dosyalarını doğrudan keşfedebilir ve analiz edebilir.
* SQL ve Spark veritabanları arasında devam eden veriler için hızlı ölçeklenebilir yükleme ve kaldırma

## <a name="built-in-data-integration"></a>Yerleşik veri tümleştirmesi

Azure SYNAPSE, Azure SYNAPSE Analytics 'ten çıkmadan zengin ölçekli ETL işlem hatları oluşturmanıza olanak sağlayan aynı veri tümleştirme altyapısını ve deneyimlerini Azure Data Factory.

* 90 ' dan fazla veri kaynağından veri alma
* Veri akışı etkinlikleri ile Code-Free ETL
* Not defterlerini, Spark işlerini, saklı yordamları, SQL betiklerini ve daha fazlasını düzenleyin

## <a name="unified-management-monitoring-and-security"></a>Birleşik yönetim, izleme ve güvenlik

Azure SYNAPSE, kuruluşların analiz kaynaklarını yönetmesi, kullanımı ve etkinlikleri izlemesi ve güvenliği zorunlu kılmak için tek bir yol sunar.

* Analiz kaynaklarına erişimi basitleştirmek için kullanıcıları role atayın
* Veriler ve kod üzerinde ayrıntılı erişim denetimi
* SQL ve Spark genelinde kaynakları, kullanımı ve kullanıcıları izlemek için tek bir pano

## <a name="unified-experience"></a>Birleşik deneyim

**SYNAPSE Studio** , veri mühendislerine yönelik her şeyi birbirine bağlayan kullanıcı deneyimidir. Tüm analiz çözümü oluşturmak için ihtiyaç duydukları her görevi yapmasına izin verir.

* Anahtar veri mühendisinin görevleri tek bir yerde: alma, İnceleme, hazırlama, düzenleme, görselleştirme
* SQL veya Spark kodu yazmak için sektör lideri üretkenlik: yazma, hata ayıklama ve performans iyileştirmesi
* Kurumsal CI/CD işlemiyle tümleştirin

## <a name="engage-with-the-synapse-engineering-team"></a>SYNAPSE Mühendislik ekibiyle etkileşim kurun

- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-synapse): geliştirme sorularını sorun.
- [Microsoft Q&soru sayfası](/answers/topics/azure-synapse-analytics.html): Teknik sorular sorun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Synapse Analytics ile çalışmaya başlama](get-started.md)
* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [Sunucusuz SQL havuzu kullanma](quickstart-sql-on-demand.md)
