---
title: Azure Cosmos DB, avantajları ve ne zaman kullanılacağı için Azure SYNAPSE bağlantısı
description: Azure Cosmos DB için Azure SYNAPSE bağlantısı hakkında bilgi edinin. SYNAPSE bağlantısı, Azure Cosmos DB üzerinde işletimsel veriler üzerinde Azure SYNAPSE Analytics kullanarak neredeyse gerçek zamanlı analizler (HTAP) çalıştırmanızı sağlar.
author: srchi
ms.author: srchi
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.reviewer: sngun
ms.openlocfilehash: 714d587cc047887685f7f7c7f4f5a2bbc2310aa2
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598629"
---
# <a name="what-is-azure-synapse-link-for-azure-cosmos-db-preview"></a>Azure Cosmos DB (Önizleme) için Azure SYNAPSE bağlantısı nedir?

> [!IMPORTANT]
> Azure Cosmos DB için Azure SYNAPSE bağlantısı şu anda önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Cosmos DB için Azure SYNAPSE bağlantısı, Azure Cosmos DB ' de işletimsel veriler üzerinde neredeyse gerçek zamanlı analizler çalıştırmanıza olanak tanıyan, bulut tabanlı bir karma işlem ve analitik işleme (HTAP) özelliğidir. Azure SYNAPSE link, Azure Cosmos DB ile Azure SYNAPSE Analytics arasında sıkı bir şekilde sorunsuz bir tümleştirme oluşturur.

Tam yalıtılmış bir sütun deposu olan [Azure Cosmos DB analitik depo](analytical-store-introduction.md)kullanarak Azure SYNAPSE link, [Azure SYNAPSE Analytics](../synapse-analytics/overview-what-is.md) 'Te hiçbir ayıklama-dönüştürme-yükleme (ETL) analizinin ölçeğe karşı işletimsel verilerinize karşı izin vermez. İş analistleri, veri mühendisleri ve veri bilimcileri artık gerçek zamanlı iş zekası, analiz ve makine öğrenimi işlem hatlarını çalıştırmak için SYNAPSE Spark veya SYNAPSE SQL birbirinin yerine kullanabilir. Bunu, Azure Cosmos DB üzerindeki işlem iş yüklerinizin performansını etkilemeden elde edebilirsiniz. 

Aşağıdaki görüntüde Azure Cosmos DB ve Azure SYNAPSE Analytics ile Azure SYNAPSE bağlantısı tümleştirmesi gösterilmektedir: 

![Azure Cosmos DB ile Azure SYNAPSE Analytics tümleştirmesi için mimari diyagramı](./media/synapse-link/synapse-analytics-cosmos-db-architecture.png)

## <a name="benefits"></a><a id="synapse-link-benefits"></a>Larından

Geleneksel olarak çalışan iş yüklerinin performansına etkisini en aza indirerek büyük işletimsel veri kümelerini analiz etmek için, Azure Cosmos DB 'deki işletimsel veriler ayıklama-Dönüştür-yükle (ETL) işlem hatları tarafından ayıklanıp işlenir. ETL işlem hatları, çok operasyonel karmaşıklığa yol açmış birçok veri hareketi katmanı ve işlem iş yüklerinizde performans etkisi gerektirir. Ayrıca, işletimsel verileri kaynak zamanından çözümleme gecikmesini de artırır.

Azure Cosmos DB için Azure SYNAPSE bağlantısı, geleneksel ETL tabanlı çözümlerle karşılaştırıldığında, şöyle çeşitli avantajlar sunar:  

### <a name="reduced-complexity-with-no-etl-jobs-to-manage"></a>Yönetilecek ETL işi olmayan Azaltılan karmaşıklık

Azure SYNAPSE bağlantısı, karmaşık veri hareketi olmadan Azure SYNAPSE Analytics kullanarak analitik depoya doğrudan Azure Cosmos DB erişmenizi sağlar. İşletimsel verilere yapılan tüm güncelleştirmeler, bir ETL veya değişiklik akışı olmadan neredeyse gerçek zamanlı olarak analitik depoda görünür. SYNAPSE Analytics 'ten gelen ve ek veri dönüştürmesi olmadan büyük ölçekli analiz, analitik depoya karşı çalıştırabilirsiniz.

### <a name="near-real-time-insights-into-your-operational-data"></a>İşletimsel verileriniz hakkında neredeyse gerçek zamanlı Öngörüler

Artık Azure SYNAPSE link kullanarak, neredeyse gerçek zamanlı olarak işletimsel verileriniz hakkında zengin Öngörüler elde edebilirsiniz. Çalışma verilerinin ayıklanmasına, dönüştürülmesinden ve yüklenmesine yönelik birçok katman nedeniyle, ETL tabanlı sistemler, işletimsel verilerinizi analiz etmeye yönelik daha yüksek gecikme süresine sahiptir. Azure SYNAPSE Analytics ile Azure Cosmos DB analitik deponun yerel tümleştirmesiyle, işletimsel verileri neredeyse gerçek zamanlı olarak analiz ederek yeni iş senaryolarına olanak sağlayabilirsiniz. 


### <a name="no-impact-on-operational-workloads"></a>İşlemsel iş yükleri üzerinde hiçbir etkisi yoktur

Azure SYNAPSE bağlantısı ile, işlem işlemleri işlem yükü (satır tabanlı bir işlem deposu) için sağlanan aktarım hızı kullanılarak işlenirken, analitik sorguları Azure Cosmos DB analitik depoya (ayrı bir sütun deposu) karşı çalıştırabilirsiniz.  Analitik iş yükü, işletimsel verileriniz için sağlanan aktarım hızını tüketmeksizin işlem iş yükü trafiğinden bağımsız olarak sunulur.

### <a name="optimized-for-large-scale-analytics-workloads"></a>Büyük ölçekli analiz iş yükleri için iyileştirildi

Azure Cosmos DB analitik depo, işlem çalıştırma sürelerine hiçbir bağımlılığı olmadan analitik iş yükleri için ölçeklenebilirlik, esneklik ve performans sağlamak üzere iyileştirilmiştir. Depolama teknolojisi, analiz iş yüklerinizi iyileştirmek için kendini yönetilirler. Azure SYNAPSE Analytics 'te yerleşik destek sayesinde, bu depolama katmanına erişmek basitlik ve yüksek performans sağlar.

### <a name="cost-effective"></a>Düşük maliyet

Azure SYNAPSE bağlantısı ile, işletimsel analizler için uygun maliyetli, tam olarak yönetilen bir çözüm edinebilirsiniz. İşletimsel verileri çözümlemek için geleneksel ETL işlem hatları 'nda gereken ek depolama ve işlem katmanlarını ortadan kaldırır. 

Analitik depo Azure Cosmos DB, veri depolama ve analitik okuma/yazma operationkum sorguları yürütülen tüketim tabanlı fiyatlandırma modelini izler. Bu işlem, işlemsel iş yükleri için bugün yaptığınız şekilde herhangi bir üretilen iş sağlama gerektirmez. Azure SYNAPSE Analytics 'ten yüksek düzeyde esnek işlem altyapılarıyla verilerinize erişmek, depolama ve işlem çalıştırmanın genel maliyetini çok verimli hale getirir.


### <a name="analytics-for-locally-available-globally-distributed-multi-master-data"></a>Yerel olarak kullanılabilir, global olarak dağıtılmış, çok ana veriler için analiz

Analitik sorguları, Azure Cosmos DB verilerin en yakın bölgesel kopyasına göre etkili bir şekilde çalıştırabilirsiniz. Azure Cosmos DB, genel olarak dağıtılmış analitik iş yüklerini birlikte etkin-etkin bir şekilde çalıştırmak için son derece özelliği sağlar.

## <a name="enable-htap-scenarios-for-your-operational-data"></a>İşletimsel verileriniz için HTAP senaryolarını etkinleştirin

SYNAPSE link, Azure SYNAPSE Analytics Runtime desteği ile analitik depo Azure Cosmos DB birlikte getirir. Bu tümleştirme, büyük veri kümelerinde işletimsel verilerinize gerçek zamanlı güncelleştirmeler temelinde Öngörüler oluşturan bulut Yerel HTAP (karma işlem/analitik işleme) çözümleri oluşturmanıza olanak sağlar. Canlı eğilimler temelinde uyarı oluşturmak, neredeyse gerçek zamanlı panolar ve kullanıcı davranışına göre iş deneyimleri oluşturmak için yeni iş senaryolarının kilidini açar.

### <a name="azure-cosmos-db-analytical-store"></a>Analitik depo Azure Cosmos DB

Azure Cosmos DB analitik depo, Azure Cosmos DB işletimsel verilerinizin sütun odaklı bir gösterimidir. Bu analitik depo, verileri kopyalamadan ve işlem iş yüklerinizin performansını etkilemeden, büyük işletimsel veri kümelerinde hızlı, ekonomik bir sorgu için uygundur.

Analitik depo, Azure Cosmos DB tam olarak yönetilen bir yetenek ("otomatik eşitleme") olarak, işlemsel iş yüklerinizde neredeyse gerçek zamanlı olarak yüksek frekansta ekler, güncelleştirmeler, siler. Değişiklik akışı veya ETL gerekli değildir. 

Küresel olarak dağıtılmış bir Azure Cosmos DB hesabınız varsa, bir kapsayıcı için analitik depoyu etkinleştirdikten sonra, bu hesap için tüm bölgelerde kullanılabilir olacaktır. Analitik depo hakkında daha fazla bilgi için bkz. [Azure Cosmos DB analitik depoya genel bakış](analytical-store-introduction.md) makalesi.

### <a name="integration-with-azure-synapse-analytics"></a><a id="synapse-link-integration"></a>Azure SYNAPSE Analytics ile tümleştirme

SYNAPSE bağlantısı ile artık Azure SYNAPSE Analytics 'ten Azure Cosmos DB kapsayıcılarınıza doğrudan bağlanabilir ve analitik depoya ayrı bağlayıcı olmadan erişebilirsiniz. Azure SYNAPSE Analytics Şu anda [Synapse Apache Spark](../synapse-analytics/spark/apache-spark-concepts.md) ve [SQL sunucusuz SYNAPSE](../synapse-analytics/sql/on-demand-workspace-overview.md)ile SYNAPSE bağlantısını desteklemektedir.

Azure SYNAPSE Analytics tarafından desteklenen farklı analiz çalışma zamanları genelinde birlikte çalışabilirliğine sahip Azure Cosmos DB analitik mağazadan verileri eşzamanlı olarak sorgulayabilirsiniz. İşletimsel verileri çözümlemek için ek veri dönüştürmeleri gerekmez. Analitik depo verilerini kullanarak sorgulayabilir ve analiz edebilirsiniz:

* Scala, Python, Mini SQL ve C# için tam destek içeren SYNAPSE Apache Spark. SYNAPSE Spark, veri Mühendisliği ve veri bilimi senaryolarına orta

* T-SQL diliyle SQL sunucusuz ve tanıdık BI araçları desteği (örneğin, Power BI Premium vb.)

> [!NOTE]
> Azure SYNAPSE Analytics 'ten Azure Cosmos DB kapsayıcıınızda hem analitik hem de işlem depolarına erişebilirsiniz. Ancak, işlemsel verilerinize büyük ölçekli analiz veya tarama çalıştırmak istiyorsanız, işlem iş yüklerinde performans etkisini önlemek için analitik depo kullanmanızı öneririz.

> [!NOTE]
> Azure Cosmos DB kapsayıcınızı ilgili bölgedeki SYNAPSE çalışma zamanına bağlayarak bir Azure bölgesinde düşük gecikme süresine sahip analiz çalıştırabilirsiniz.

Bu tümleştirme, farklı kullanıcılar için aşağıdaki HTAP senaryolarına izin vermez:

* Doğrudan SYNAPSE SQL üzerinden Azure Cosmos DB işletimsel verilere erişmek üzere bir rapor modellemek ve yayımlamak isteyen bir bı mühendisi.

* Bir Azure Cosmos DB kapsayıcısında işletimsel verilerden SYNAPSE SQL ile sorgulama yaparak Öngörüler elde etmek isteyen bir veri analisti, ölçekteki verileri okuyun ve bu bulguları diğer veri kaynaklarıyla birleştirin.

* Bir veri bilimconu, modelini geliştirmek ve bu modeli karmaşık veri Mühendisliği yapmadan eğtirecek bir özellik bulmak için SYNAPSE Spark 'ı kullanmak istiyor. Ayrıca, Spark SYNAPSE aracılığıyla veri üzerindeki gerçek zamanlı Puanlama için Azure Cosmos DB, model gönderi çıkarımı sonuçlarını da yazabilir.

* El ile ETL işlemi olmadan Azure Cosmos DB kapsayıcıları üzerinde SQL veya Spark tabloları oluşturarak, müşteriler için veri erişilebilir hale getirmek isteyen bir veri mühendisi.

Azure Cosmos DB için Azure SYNAPSE Analytics çalışma zamanı desteği hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics for Cosmos DB support]().

## <a name="when-to-use-azure-synapse-link-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure SYNAPSE bağlantısı ne zaman kullanılır?

SYNAPSE bağlantısı aşağıdaki durumlarda önerilir:

* Azure Cosmos DB müşterisiyseniz ve işletimsel verileriniz üzerinde analiz, bı ve makine öğrenimi çalıştırmak istiyorsanız. Bu gibi durumlarda, SYNAPSE link, işlem mağazalarınızın sağlanan verimini etkilemeden daha tümleşik bir analiz deneyimi sağlar. Örnek:

  * Azure Cosmos DB işletimsel verilerinize doğrudan ayrı bağlayıcılar kullanarak analiz veya bı çalıştırıyorsanız veya

  * İşletimsel verileri ayrı bir analiz sistemine ayıklamak için ETL süreçlerini çalıştırıyorsanız.
 
Bu gibi durumlarda, SYNAPSE link, işlem mağazalarınızın sağlanan verimini etkilemeden daha tümleşik bir analiz deneyimi sağlar.

Yüksek eşzamanlılık, iş yükü yönetimi ve birden çok veri kaynağında toplamaların sürekliliği gibi geleneksel veri ambarı gereksinimlerini arıyorsanız SYNAPSE bağlantısı önerilmez. Daha fazla bilgi için, [Azure Cosmos DB Için Azure SYNAPSE bağlantısı ile desteklenen genel senaryolar](synapse-link-use-cases.md)bölümüne bakın.


## <a name="limitations"></a>Sınırlamalar

* Genel Önizleme sırasında Azure SYNAPSE link yalnızca Azure Cosmos DB SQL (Core) API 'SI için desteklenir. Azure Cosmos DB MongoDB & Cassandra API API 'sine yönelik destek şu anda geçitli bir önizleme aşamasındadır. Geçitli önizlemeye erişim istemek için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)e-posta gönderin.

* Şu anda analitik depo yalnızca yeni kapsayıcılar için etkinleştirilebilir (hem yeni hem de mevcut Azure Cosmos DB hesaplarında).

* SYNAPSE SQL sunucusuz ile Azure Cosmos DB analitik deposuna erişilmesi Şu anda geçitli önizleme aşamasındadır. Erişim istemek için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)e-posta gönderin.

* Sağlanan SYNAPSE SQL ile Azure Cosmos DB Analytics deposuna erişim şu anda kullanılamıyor. 

## <a name="pricing"></a>Fiyatlandırma

Azure SYNAPSE bağlantısının faturalandırma modeli, Azure Cosmos DB analitik mağaza ve SYNAPSE çalışma zamanını kullanarak tahakkuk eden maliyetleri dönüştürür. Daha fazla bilgi edinmek için bkz. [analitik mağaza fiyatlandırması](analytical-store-introduction.md#analytical-store-pricing) ve [Azure SYNAPSE Analytics fiyatlandırma]() makaleleri Azure Cosmos DB.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Cosmos DB analitik depoya genel bakış](analytical-store-introduction.md)

* [Azure Cosmos DB için Azure SYNAPSE bağlantısı ile çalışmaya başlama](configure-synapse-link.md) 
 
* [Azure SYNAPSE Analytics çalışma zamanında desteklenen özellikler]()

* [Azure Cosmos DB için Azure SYNAPSE bağlantısı hakkında sık sorulan sorular](synapse-link-frequently-asked-questions.md)

* [Azure Cosmos DB kullanım örnekleri için Azure SYNAPSE bağlantısı](synapse-link-use-cases.md)
