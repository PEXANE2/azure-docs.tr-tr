---
title: Apache Spark ile Azure Cosmos DB yerleşik işletimsel analizine giriş
description: İşletimsel analiz ve AI çalıştırmak için Azure Cosmos DB Apache Spark için yerleşik desteğini nasıl kullanabileceğinizi öğrenin
ms.service: cosmos-db
ms.topic: overview
ms.date: 09/26/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: 952dd084bccd67a0a8833002d73d3aaf1d5dfb25
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338620"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark"></a>Apache Spark ile Azure Cosmos DB yerleşik işlemsel analiz

Azure Cosmos DB ile, genel olarak dağıtılmış, düşük gecikme süreli analiz ve AI işlem verileri üzerinde çalıştırabilirsiniz. Apache Spark ve jupi Not defterleri için yerel destek sayesinde, Azure Cosmos DB öngörülerin süresini azaltmaya yardımcı olur. Verilerin yapıldığından, sunulan ve analizler bir Azure bölgesindeki yerel veritabanı çoğaltmasına karşı çalışır. Veri bölümlerinizde depolanan, dizinli çok modelli veriler üzerinde Apache Spark sorguları doğrudan yürütebilirsiniz.

Azure Cosmos DB Apache Spark için yerleşik destek, Azure Cosmos hesabında depolanan verilerinize karşı Apache Spark analiz çalıştırmanızı sağlar. Genel olarak dağıtılmış Cosmos veritabanlarınızı doğrudan yürütmek üzere Apache Spark işleri için yerel destek sağlar. Bu yetenekler, geliştiriciler, veri mühendisleri ve veri bilimcileri, hem **OLTP hem de OLAP/HTAP** iş yüklerini çalıştırmak için esnek, ölçeklenebilir ve performanslı bir veri platformu olarak Azure Cosmos DB kullanabilir.

Azure Cosmos DB Apache Spark desteği aşağıdaki avantajları sunmaktadır:

* Coğrafi olarak dağıtılmış kullanıcılar ve veriler için öngörülere en hızlı süreyi elde edebilirsiniz.

* Çözümünüzün mimarisini basitleştirebilir ve [toplam sahip olma maliyetini](total-cost-ownership.md) (TCO) daha düşük hale getirebilirsiniz. Sistem en az sayıda veri işleme bileşenine sahip olur ve bunlar arasında gereksiz veri hareketini önler.

* Yönetim altındaki tüm verileri kapsayan bir [güvenlik](secure-access-to-data.md), [Uyumluluk](compliance.md)ve denetim sınırı oluşturur.

* Katı SLA 'Lar tarafından desteklenen "her zaman açık" veya [yüksek oranda kullanılabilir](high-availability.md) Son Kullanıcı analizi sağlar.

![Azure Cosmos DB görselleştirmede Apache Spark desteği](./media/spark-api-introduction/spark-api-visualization.png)
 
Azure Cosmos DB Apache Spark desteğini kullanarak AI ve derin öğrenme modelleri, tahmine dayalı analiz, öneriler, IoT, müşteri 360, sahtekarlık algılama, metin yaklaşımı, tıklama akış analizi gibi çözümler oluşturabilir ve dağıtabilirsiniz. Bu çözümler doğrudan Azure Cosmos DB verilerinize göre çalışır.

Toplu iş ve akış ETL işini, veritabanı hizmetinin dışına çıkmadan veya ek işlem hizmetleri ekleyebilmek için Azure Cosmos DB ayarlayabilirsiniz. ETL işini gerçekleştirmeniz gerektiğinde, işlem ortamını esnek ölçeklendirebilir ve iş tamamlandığında ölçeği yeniden ölçeklendirmeniz gerekir.

Azure Cosmos DB Apache Spark desteği, Apache Spark çalışma zamanları içinde yerleşik Machine Learning desteği sunar. Çalışma zamanları Spark MLLib, Spark, Azure Machine Learning ve bilişsel hizmetler için Microsoft Machine Learning içerir. Bu özellikler sayesinde, veri bilimcileri, veri mühendisleri ve veri analistleri, makine öğrenimi modellerini doğrudan Azure Cosmos DB içinde ve düşük maliyetli bir süre içinde oluşturup gerçekleştirebilir.


## <a name="key-benefits"></a>Önemli avantajlar

### <a name="low-latency-operational-analytics-and-ai"></a>Düşük gecikmeli işlemsel analiz ve AI

Küresel olarak dağıtılmış Azure Cosmos veritabanında Apache Spark sayesinde dünyanın her yerindeki tüm dünyada hızlı bir anlayış elde edebilirsiniz. Azure Cosmos DB, üç anahtar teknikle esnek ölçekte **küresel olarak dağıtılmış, düşük gecikmeli işlemsel** analizler sunar:

* Azure Cosmos veritabanınız küresel olarak dağıtıldığından, verilerin üreticileri (örneğin, kullanıcılar) bulunduğu tüm veriler yerel olarak alınır. Sorgular, dünyanın neresinde olursa olsun, hem üreticileri hem de veri tüketicilerine en yakın yerel çoğaltmalara göre sunulur.

* Tüm analitik sorgularınız, hiçbir gereksiz veri hareketine gerek duymadan veri bölümlerinin içinde depolanan dizinli verilerde doğrudan yürütülür.

* Spark Azure Cosmos DB ile birlikte bulunduğundan, daha az ara çeviri ve veri hareketleri gerçekleştiğinden, daha iyi performans ve ölçeklenebilirlik elde edilir.

* Çoklu yönetici, otomatik yük devretme, Kullanılabilirlik Alanları vb. gibi tüm Azure Cosmos DB Çekirdek özellikleri Azure Cosmos DB yerleşik Apache Spark için kullanılabilir.

### <a name="unified-serverless-experience-for-apache-spark"></a>Apache Spark için Birleşik sunucusuz deneyim

Azure Cosmos DB, çok modelli bir veritabanı olarak, anahtar-değer, belge, grafik ve sütun ailesi veri modelleriyle **Apache Spark için birleştirilmiş bir sunucusuz deneyim** sağlayarak artık OSS API 'leri desteğini genişletir. Farklı veri modelleri MongoDB, Cassandra, Gremlin, Etcd ve SQL API 'Leri kullanılarak desteklenir; bunların hepsi aynı temel veriler üzerinde çalışıyor. 

Azure Cosmos DB Apache Spark desteği sayesinde, Scala, Python, Java 'da yazılmış uygulamaları yerel olarak destekleyebilir ve SQL için çok sıkı tümleşik kitaplıkları kullanabilirsiniz. Bu kitaplıklar ([Spark SQL](https://spark.apache.org/sql/)), Machine Learning (Spark [mllib](https://spark.apache.org/mllib/)), akış Işleme ([Spark yapısal akış](https://spark.apache.org/streaming/)) ve grafik işleme (Spark [graphframes]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)) içerir. Bu araçlar, çeşitli kullanım durumları için Apache Spark kullanmayı kolaylaştırır. Spark veya Spark kümelerinin yönetilmesi konusunda uğraşmanız gerekmez. Analiz ve SQL API 'SI için tanıdık Apache Spark API 'Leri ve **Jupyıter not defterlerini** veya aynı anda aynı temel veriler üzerinde işlem Işleme Için Cassandra gibi HERHANGI bir OSS NoSQL API 'sini kullanabilirsiniz.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Azure Cosmos DB, veri mühendislerinin ve veri bilimcilerinin aksine geleneksel analitik veritabanlarının aksine, artık daha fazla şema ve dizin yönetimiyle uğraşmak zorunda kalmaz. Azure Cosmos DB ' deki veritabanı altyapısı herhangi bir açık şema veya dizin yönetimi gerektirmez ve Apache Spark sorgularını hızlı bir şekilde sunacak tüm verileri otomatik olarak dizine alabilir.

### <a name="consistency-choices"></a>Tutarlılık seçenekleri

Apache Spark işleri Azure Cosmos veritabanınızın veri bölümlerinde yürütüldüğü için sorgular, [iyi tanımlanmış beş tutarlılık seçimi](consistency-levels.md)alır. Bu tutarlılık modelleri, gecikme ve yüksek kullanılabilirlikten ödün vermeden makine öğrenimi algoritmalarının en doğru sonuçlarını sağlamak üzere katı tutarlılık seçme esnekliği sunar.

### <a name="comprehensive-slas"></a>Kapsamlı SLA’lar

Apache Spark işleri, ayrı Apache Spark kümelerini yönetme ek yükü olmadan sektör lideri kapsamlı [SLA 'lar](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) gibi Azure Cosmos DB avantajlara sahip olacaktır. Bu SLA 'Lar, iş verimini, 99. yüzdebirlik, tutarlılık ve yüksek kullanılabilirliğe göre gecikme süresini kapsayabilir. 

### <a name="mixed-operational-and-analytical-workloads-with-complete-isolation"></a>Tamamen yalıtımlı karma operasyonel ve analitik iş yükleri

Apache Spark tümleştirme, genel ölçekte bulutta yerel uygulamalar oluştururken önemli bir müşteri sorun noktalarından biri olan işlem ve analitik ayrımı Azure Cosmos DB köprüleyerek. OLTP ve OLAP iş yükleri yan yana çalışır ve birbirini engellemez.

### <a name="low-latency-analytical-and-transactional-storage"></a>Düşük gecikmeli analitik ve işlem depolama

Azure Cosmos DB, verileri yerel olarak iki ayrı depolama katmanında depolar: işlemsel (satır yönelimli) ve analitik depolama (Apache Parquet dosya biçiminde sütun odaklı). Her katmandaki verileri küresel olarak çoğaltır ve kullanıcıların bu katmanlardaki verileri, bekletme ilkelerine göre bağımsız olarak yönetmesine olanak tanır.

Bu depolama mimarisi, aynı küresel olarak dağıtılan veriler üzerinde hem görev açısından kritik işlem hem de analitik iş yükleri sunar. Cosmos DB, herhangi bir ETL işlemine ihtiyacınız yoktur veya hiçbir gereksiz veri hareketini gerçekleştiremezsiniz. Yalnızca aynı temel veriler üzerinde hem işlem hem de analitik iş yüklerini çalıştırabilirsiniz. İşlem yükleri SQL, Cassandra, MongoDB, Gremlin ve Etcd dahil tanıdık işlemsel erişim API 'Lerini kullanabilir. Analiz ve AI iş yükleri yerleşik Apache Spark SQL, ML çerçeveleri ve tüm Apache Spark araç zincirini kullanabilir.

### <a name="snapshots-and-historical-analytical-queries"></a>Anlık görüntüler ve geçmiş analitik sorgular

Doğrudan belirli bir anlık görüntüye karşı analitik sorgular gerçekleştirmek için analitik katmanda depolanan sütunlu sıkıştırılmış verilerin isteğe bağlı veya zamanlamaya bağlı anlık görüntülerini oluşturabilirsiniz. Bu özellik, Flashback veya Time-seyahat analitik sorguları, geri alma, tam geçmiş denetim izleri ve tekrarlanabilir makine öğrenimi ve AI denemeleri sunar.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB avantajları hakkında bilgi edinmek için [genel bakış](introduction.md) makalesine bakın.
* [MongoDB için Azure Cosmos DB API 'SI ile çalışmaya başlama](mongodb-introduction.md)
* [Azure Cosmos DB Cassandra API kullanmaya başlayın](cassandra-introduction.md)
* [Azure Cosmos DB Gremlin API 'sini kullanmaya başlayın](graph-introduction.md)
* [Azure Cosmos DB Tablo API'si kullanmaya başlayın](table-introduction.md)