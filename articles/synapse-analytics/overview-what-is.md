---
title: Azure Synapse Analytics nedir?
description: Azure Synapse Analytics'e Genel Bakış
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 65f6c6627093358f8cbc66055bb9b16561f7c610
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424667"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Azure Synapse Analytics (çalışma alanları önizlemesi) nedir?

[!INCLUDE [preview](includes/note-preview.md)]

Günümüzde kurumsal analitik, ham, rafine veya yüksek küratörlük olsun, her türlü veri üzerinde büyük ölçekte çalışma gerektirir. Geçmişte, bu tür analiz çözümleri oluşturmak, işletmelerin Spark ve SQL gibi büyük veri ve veri depolama teknolojilerini bir araya getiremelerini gerektiriyor. Daha sonra, bunları ilişkisel mağazalarda ve veri göllerindeki veriler arasında çalışan zengin veri ardışık larına entegre etmeleri gerekir.  

Bu gibi çözümler oluşturmak, yapılandırmak, güvenli hale getirmek ve korumak zordur, bu da akıllı içgörülerin hızlı bir şekilde çıkarılmasını geciktirir.

**Azure Synapse,** veri ambarları ve büyük veri analitiği sistemleri arasında, her ölçekteki tüm verilerden bilgi alma süresini hızlandıran entegre bir analiz hizmetidir. Kurumsal veri depolamada kullanılan en iyi **SQL** teknolojilerini, büyük veri analitiğinde kullanılan **Kıvılcım** teknolojilerini ve faaliyetleri ve veri hareketini düzenlemek için **Pipelines'ı** bir araya getirir. 

Azure Synapse, yönetim, izleme, kodlama ve güvenlik için tek bir deneyim ve model sağlayan web tabanlı **bir Studio** kullanıcı deneyimiyle birlikte gelir.

Azure Synapse, bir işletmenin en çok aşina oldukları analitiği kullanarak herhangi bir boyuttaki veri hakkında bilgi toplaması için en basit ve en hızlı yolu sağlar. **Power BI** ile derinden entegre edilerek veri mühendislerinin İş Zekası sağlamak için uçlardan uca çalışan analiz çözümleri oluşturmasına olanak sağlar. 

Ayrıca Azure Synapse, **AzureML'e**verdiği yerleşik destek le makine öğrenimi ile tahmine dayalı model oluşturmayı ve gelişmiş analizleri kolaylaştırır.

## <a name="key-features--benefits"></a>Temel özellikler & faydaları

### <a name="industry-leading-sql"></a>Sektör lideri SQL

* **Synapse SQL,** işletmelerin veri mühendislerine tanıdık standart T-SQL deneyimlerini kullanarak veri depolama ve veri sanallaştırma senaryolarını uygulamalarını sağlayan dağıtılmış bir sorgu sistemidir. Ayrıca, akış ve makine öğrenimi senaryolarını ele almak için SQL'in yeteneklerini genişletir.

* Synapse SQL, ihtiyaçlarınıza uygun tüketim ve faturalandırma seçenekleri sunarak hem **sunucusuz** hem de **tedarik edilmiş** kaynak modelleri sunar. Öngörülebilir performans ve maliyet için, SQL tablolarında depolanan veriler için işlem gücü ayırmak için sağlama havuzları. Planlanmamış veya patlamalı iş yükleri için sunucusuz, her zaman kullanılabilir OLAN SQL bitiş noktasını kullanın.
* Bulut veri kaynaklarından sql tablolarına veri aktarmak için yerleşik **akış** özelliklerini kullanma
* T-SQL PREDICT işlevini kullanarak veri elde etmek için **makine öğrenme** modellerini kullanarak AI'yi SQL ile tümleştirin

### <a name="industry-standard-apache-spark"></a>Endüstri standardı Apache Spark

**Synapse Spark** derin ve sorunsuz apache Spark entegre - veri hazırlama, veri mühendisliği, ETL ve makine öğrenimi için kullanılan en popüler açık kaynak büyük veri motoru.

* Linux Foundation Delta Lake için yerleşik destek le Apache Spark 2.4 için SparkML algoritmalarına ve AzureML tümleştirmelerine sahip ML modelleri.
* Kümeleri yönetme konusunda endişelenmenize gerek kalmadan sizi serbest leştiren basitleştirilmiş kaynak modeli.
* Hızlı Kıvılcım başlatma ve agresif otomatik ölçeklendirme.
* Bir Spark uygulaması nda C# uzmanlığınızı ve mevcut .NET kodunuzu yeniden kullanmanıza olanak tanıyan Spark için .NET için yerleşik destek.

### <a name="interop-of-sql-and-spark-on-your-data-lake"></a>Veri Gölünüzde SQL ve Kıvılcım Interop

Azure Synapse, SQL ve Spark'ı birlikte kullanmak arasındaki geleneksel teknoloji engellerini kaldırır. İhtiyaçlarınıza ve uzmanlığınıza göre sorunsuz bir şekilde karıştırAbilir ve eşleştirebilirsiniz.

* Paylaşılan Hive uyumlu meta veri sistemi, veri gölündeki dosyalarda tanımlanan tabloların Spark veya Hive tarafından sorunsuz bir şekilde tüketilmesine olanak tanır.
* SQL ve Spark, veri gölünde depolanan Parke, CSV, TSV ve JSON dosyalarını doğrudan keşfedebilir ve analiz edebilir.
* SQL ve Spark veritabanları arasında giden veriler için hızlı ölçeklenebilir yük ve boşaltma

### <a name="built-in-orchestration-via-pipelines"></a>Boru hatları ile yerleşik orkestrasyon

Azure Synapse, azure veri fabrikasıyla aynı Veri Tümleştirme motoruve deneyimleriyle birlikte gelir ve ayrı bir düzenleme motoru kullanmadan zengin veri ardışık alanları oluşturmanıza olanak tanır.

* Verileri Synapse ve 85+ şirket içi veri kaynakları arasında taşıma
* Defterleri, Boru Hatlarını, Kıvılcım işlerini, SQL Komut Dosyalarını, Depolanmış yordamları düzenleme
* Veri akışı faaliyetleri ile Kodsuz ETL

### <a name="unified-management-monitoring-and-security"></a>Birleşik yönetim, izleme ve güvenlik

Azure Synapse, işletmelerin analitik kaynaklarını yönetmesi, kullanımını ve etkinliğini izlemesi ve güvenliği uygulaması için tek bir yol sağlar.

* Analitik kaynaklara erişimi kolaylaştırmak için kullanıcıları Role atama
* Veri ve kod üzerinde ince taneli erişim denetimi
* SQL ve Spark'ta kaynakları, kullanımı ve kullanıcıları izlemek için tek bir pano

### <a name="synapse-studio"></a>Synapse Stüdyosu

**Synapse Studio,** veri mühendisleri için her şeyi birbirine bağlayan ve tam bir çözüm oluşturmak için gereken her görevi tek bir yerde yapmalarına olanak tanıyan web-yerel bir deneyimdir.

* Tek bir yerde uçtan uca bir analiz çözümü oluşturun: yutmak, keşfetmek, hazırlamak, düzenlemek, görselleştirin
* SQL veya Spark kodu yazan veri mühendisleri için sektör lideri üretkenlik: yazma, hata ayıklama ve performans optimizasyonu
* Kurumsal CI/CD süreçleriyle tümleştirme

## <a name="next-steps"></a>Sonraki adımlar

* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [Synapse Studio'u kullanın](quickstart-synapse-studio.md)
* [SQL havuzu oluşturma](quickstart-create-sql-pool.md)
* [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
* [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)
