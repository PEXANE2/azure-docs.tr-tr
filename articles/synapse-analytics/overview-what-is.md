---
title: Azure Synapse Analytics nedir?
description: Azure SYNAPSE Analytics 'e genel bakış
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 09/12/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c4338152579170bf809577262992f0db9a1a95ff
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90524955"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Azure SYNAPSE Analytics (çalışma alanları önizlemesi) nedir?

[!INCLUDE [preview](includes/note-preview.md)]

Kurumsal analizler, ham, iyileştirilmiş veya yüksek düzeyde bir şekilde, her türlü veri üzerinde büyük ölçekli bir ölçek çalışmalıdır. Bu gerekli kuruluşlar, Spark ve SQL gibi büyük verileri ve veri ambarı teknolojilerini, ilişkisel depolardaki ve veri laklarındaki veriler arasında çalışan zengin veri işlem hatlarına birleştirmek için gereklidir. Bunun gibi çözümlerin oluşturulması, güvende olması ve korunması zordur. Karmaşıklık enteprises gereksinimini ortadan kaldırın.

**Azure SYNAPSE** , veri ambarlarında ve büyük veri analizi sistemlerinde öngörülere yönelik Zamanı hızlandıran tümleşik bir analiz hizmetidir. Azure SYNAPSE, kurumsal veri ambarlarında kullanılan **SQL** teknolojilerinin en iyi özelliklerini, büyük veriler Için kullanılan **Spark** teknolojilerini ve veri TÜMLEŞTIRME ve etl/ELT için işlem **hatlarını** kullanır. SYNAPSE, yönetim, izleme, kodlama ve güvenlik için tek bir yer sağlayan Web tabanlı bir **Studio 'ya** sahiptir. SYNAPSE, **PowerBI**, **Cosmosdb**ve **AzureML**gibi diğer Azure hizmetleriyle ayrıntılı tümleştirmeyi Özellikler.

## <a name="key-features--benefits"></a>Önemli özellikler & avantajları

### <a name="industry-leading-sql"></a>Sektör lideri SQL

* **SYNAPSE SQL** , kuruluşların veri mühendislerini bildiğiniz Standart T-SQL deneyimlerini kullanarak veri depolama ve veri sanallaştırma senaryoları uygulamasına olanak tanıyan bir dağıtılmış sorgu sistemidir. Ayrıca, SQL 'in, akış ve makine öğrenimi senaryolarına yönelik yeteneklerini de genişletir.

* SYNAPSE SQL, gereksinimlerinize uyacak şekilde hem **sunucusuz** hem de **adanmış** kaynak modelleri sunarak tüketim ve faturalandırma seçenekleri sunar. Tahmin edilebilir performans ve maliyet için, SQL tablolarında depolanan veriler için işleme gücünü ayırmak üzere adanmış SQL havuzları oluşturun. Planlanmamış veya bursty iş yükleri için, her zaman kullanılabilir, sunucusuz SQL uç noktasını kullanın.
* Bulut veri kaynaklarından SQL tablolarına veri aktarmak için yerleşik **akış** yeteneklerini kullanın
* [T-SQL tahmin işlevini](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest) kullanarak veri puanlayıp, **makine ÖĞRENIMI** MODELLERINI kullanarak AI 'yi SQL ile tümleştirin

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

Azure SYNAPSE, Azure Data Factory ile aynı veri tümleştirme altyapısı ve deneyimleriyle yerleşik olarak sunulur. böylece, SYNAPSE analizinden çıkmadan zengin ölçekli ETL işlem hatları oluşturabilirsiniz.

* 90 ' dan fazla veri kaynağından veri alma
* Veri akışı etkinlikleri ile kod ücretsiz ETL
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

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SYNAPSE Analytics 'i kullanmaya başlama](get-started.md)
* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
