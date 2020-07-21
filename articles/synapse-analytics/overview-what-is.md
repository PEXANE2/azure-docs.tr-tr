---
title: Azure Synapse Analytics nedir?
description: Azure SYNAPSE Analytics 'e genel bakış
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: be124bdde4427113d56f44f0c1fa19c600bd768e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496062"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Azure SYNAPSE Analytics (çalışma alanları önizlemesi) nedir?

[!INCLUDE [preview](includes/note-preview.md)]

Günümüzde Kurumsal analizler, ham, iyileştirilmiş veya yüksek düzeyde bir şekilde, her türlü veri için büyük ölçekte çalışma gerektirir. Geçmişte, bu tür analiz çözümlerinin, büyük veri ve Spark ve SQL gibi veri ambarı teknolojilerini birleştirmek için kuruluşların oluşturulması gerekir. Ardından, bunları ilişkisel depolardaki ve veri lasyonlarındaki veriler arasında çalışan zengin veri işlem hatlarıyla tümleştirmeleri gerekir.  

Bunun gibi çözümler oluşturmak, yapılandırmak, güvenli hale getirmek ve bakımını yapmak zordur ve bu da akıllı öngörü 'nin Swift ayıklamasını geciktirir.

**Azure SYNAPSE** , veri ambarları ve büyük veri analizi sistemleri arasında herhangi bir ölçekte tüm verilerden bilgi elde etmek için zamanı hızlandıran tümleşik bir analiz hizmetidir. Kurumsal veri ambarlarında kullanılan **SQL** teknolojilerinin en iyilerini, büyük veri analizlerinde kullanılan **Spark** teknolojilerini ve etkinlikleri ve veri hareketini düzenlemek için işlem **hatlarını** bir araya getirir.

Azure SYNAPSE, yönetim, izleme, kodlama ve güvenlik için tek bir deneyim ve model sağlayan bir Web Native **Studio** Kullanıcı deneyimiyle birlikte gelir.

Azure SYNAPSE, bir kuruluşun en kolay ve en hızlı yolunu, herhangi bir boyutta tüm veriler hakkında bilgi toplamak için en kolay ve en hızlı yolu sağlar. Veri mühendislerinin, Iş zekası sağlamak üzere uçtan uca çalışan analiz çözümleri oluşturmasına olanak tanıyan **Power BI** ile tümleşir.

Ayrıca, Azure SYNAPSE, bu derleme ve gelişmiş analizler ile makine öğrenimi sayesinde **AzureML**için yerleşik destek sağlar.

## <a name="key-features--benefits"></a>Önemli özellikler & avantajları

### <a name="industry-leading-sql"></a>Sektör lideri SQL

* **SYNAPSE SQL** , kuruluşların veri mühendislerini bildiğiniz Standart T-SQL deneyimlerini kullanarak veri depolama ve veri sanallaştırma senaryoları uygulamasına olanak tanıyan bir dağıtılmış sorgu sistemidir. Ayrıca, SQL 'in, akış ve makine öğrenimi senaryolarına yönelik yeteneklerini de genişletir.

* SYNAPSE SQL, gereksinimlerinize uyacak şekilde hem **sunucusuz** hem de **sağlanan** kaynak modelleri sunarak tüketim ve faturalandırma seçenekleri sunar. Tahmin edilebilir performans ve maliyet için, SQL tablolarında depolanan veriler için işleme gücünü ayırmak üzere havuzlar sağlayın. Planlanmamış veya bursty iş yükleri için sunucusuz, her zaman kullanılabilir SQL uç noktasını kullanın.
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

### <a name="built-in-orchestration-via-pipelines"></a>Ardışık düzen aracılığıyla yerleşik düzenleme

Azure SYNAPSE, aynı veri tümleştirme altyapısı ve deneyimleriyle Azure Data Factory ile yerleşik olarak sunulur ve ayrı bir düzenleme altyapısı kullanmadan zengin veri işlem hatları oluşturmanıza olanak sağlar.

* Verileri Azure SYNAPSE ile 90 + şirket içi veri kaynakları arasında taşıma
* Not defterlerini, işlem hatlarını, Spark işlerini, SQL betikleri, saklı yordamları düzenleyin
* Veri akışı etkinlikleri ile kod ücretsiz ETL

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

* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [SYNAPSE Studio 'Yu kullanma](quickstart-synapse-studio.md)
* [SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md)
