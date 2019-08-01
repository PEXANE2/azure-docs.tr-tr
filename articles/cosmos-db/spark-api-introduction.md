---
title: Apache Spark ile Azure Cosmos DB yerleşik işletimsel analizine giriş
description: İşletimsel analiz ve AI çalıştırmak için Azure Cosmos DB Apache Spark için yerleşik desteğini nasıl kullanabileceğinizi öğrenin
ms.service: cosmos-db
ms.topic: overview
ms.date: 08/01/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: 3fd7990043543b92eb3daf8ec699ec58db3387ec
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717582"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark-preview"></a>Apache Spark ile Azure Cosmos DB yerleşik işlemsel analiz (Önizleme) 

Azure Cosmos DB Apache Spark için yerleşik destek, Azure Cosmos hesabında depolanan verilerinize karşı Apache Spark analiz çalıştırmanızı sağlar. Genel olarak dağıtılmış Cosmos veritabanlarınızı doğrudan yürütmek üzere Apache Spark işleri için yerel destek sağlar. Bu yetenekler, geliştiriciler, veri mühendisleri ve veri bilimcileri, hem **OLTP hem de OLAP/HTAP** iş yüklerini çalıştırmak için esnek, ölçeklenebilir ve performanslı bir veri platformu olarak Azure Cosmos DB kullanabilir. 

Spark işlem, Azure Cosmos hesabınızla ilişkili tüm Azure bölgelerinde otomatik olarak kullanılabilir. Spark işleri Azure Cosmos DB çoklu yönetici özelliğini kullanır ve her bölgedeki yerel çoğaltmaları yazabilir veya bunlara sorgu yapabilir. 

> [!NOTE]
> Azure Cosmos DB Apache Spark için yerleşik destek şu anda sınırlı önizlemededir. Önizlemeye kaydolmak için, [önizleme sayfasına](https://portal.azure.com/?feature.customportal=false#create/Microsoft.DocumentDB)gidin. 

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

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Küresel olarak dağıtılmış, düşük gecikmeli işlemsel analiz ve AI

Küresel olarak dağıtılmış Azure Cosmos veritabanında Apache Spark sayesinde dünyanın her yerindeki tüm dünyada hızlı bir anlayış elde edebilirsiniz. Azure Cosmos DB, üç anahtar teknikle esnek ölçekte **küresel olarak dağıtılmış, düşük gecikmeli işlemsel** analizler sunar:

* Azure Cosmos veritabanınız küresel olarak dağıtıldığından, verilerin üreticileri (örneğin, kullanıcılar) bulunduğu tüm veriler yerel olarak alınır. Sorgular, dünyanın neresinde olursa olsun, hem üreticileri hem de veri tüketicilerine en yakın yerel çoğaltmalara göre sunulur. 

* Tüm analitik sorgularınız, hiçbir gereksiz veri hareketine gerek duymadan veri bölümlerinin içinde depolanan dizinli verilerde doğrudan yürütülür. 

* Spark Azure Cosmos DB ile birlikte bulunduğundan, daha az ara çeviri ve veri hareketleri gerçekleştiğinden, daha iyi performans ve ölçeklenebilirlik elde edilir.

### <a name="unified-serverless-experience-for-apache-spark"></a>Apache Spark için Birleşik sunucusuz deneyim

Azure Cosmos DB, çok modelli bir veritabanı olarak, anahtar-değer, belge, grafik ve sütun ailesi veri modelleriyle **Apache Spark için birleştirilmiş bir sunucusuz deneyim** sağlayarak artık OSS API 'leri desteğini genişletir. Farklı veri modelleri MongoDB, Cassandra, Gremlin, Etcd ve SQL API 'Leri kullanılarak desteklenir; bunların hepsi aynı temel veriler üzerinde çalışıyor. 

Azure Cosmos DB Apache Spark desteği sayesinde, Scala, Python, Java 'da yazılmış uygulamaları yerel olarak destekleyebilir ve SQL için çok sıkı tümleşik kitaplıkları kullanabilirsiniz. Bu kitaplıklar ([Spark SQL](https://spark.apache.org/sql/)), Machine Learning (Spark [mllib](https://spark.apache.org/mllib/)), akış Işleme ([Spark yapısal akış](https://spark.apache.org/streaming/)) ve grafik işleme (Spark [graphframes]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)) içerir. Bu araçlar, çeşitli kullanım durumları için Apache Spark kullanmayı kolaylaştırır. Spark veya Spark kümelerinin yönetilmesi konusunda uğraşmanız gerekmez. Analiz ve SQL API 'SI için tanıdık Apache Spark API 'Leri ve **Jupyıter not defterlerini** veya aynı anda aynı temel veriler üzerinde işlem Işleme Için Cassandra gibi HERHANGI bir OSS NoSQL API 'sini kullanabilirsiniz.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Azure Cosmos DB, veri mühendislerinin ve veri bilimcilerinin aksine geleneksel analitik veritabanlarının aksine, artık daha fazla şema ve dizin yönetimiyle uğraşmak zorunda kalmaz. Azure Cosmos DB ' deki veritabanı altyapısı herhangi bir açık şema veya dizin yönetimi gerektirmez ve Apache Spark sorgularını hızlı bir şekilde sunacak tüm verileri otomatik olarak dizine alabilir. 

### <a name="consistency-choices"></a>Tutarlılık seçenekleri

Apache Spark işleri Azure Cosmos veritabanınızın veri bölümlerinde yürütüldüğü için sorgular, [iyi tanımlanmış beş tutarlılık seçimi](consistency-levels.md)alır. Bu tutarlılık modelleri, gecikme ve yüksek kullanılabilirlikten ödün vermeden makine öğrenimi algoritmalarının en doğru sonuçlarını sağlamak üzere katı tutarlılık seçme esnekliği sunar. 

### <a name="comprehensive-slas"></a>Kapsamlı SLA’lar

Apache Spark işleri, ayrı Apache Spark kümelerini yönetme ek yükü olmadan sektör lideri kapsamlı [SLA 'lar](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) gibi Azure Cosmos DB avantajlara sahip olacaktır. Bu SLA 'Lar, iş verimini, 99. yüzdebirlik, tutarlılık ve yüksek kullanılabilirliğe göre gecikme süresini kapsayabilir. 

### <a name="mixed-workloads"></a>Karışık iş yükleri

Apache Spark tümleştirme, genel ölçekte bulutta yerel uygulamalar oluştururken önemli bir müşteri sorun noktalarından biri olan işlem ve analitik ayrımı Azure Cosmos DB köprüleyerek. 

## <a name="scenarios-for-azure-cosmos-db-spark-support"></a>Azure Cosmos DB Spark desteği senaryoları

### <a name="retail-and-consumer-goods"></a>Perakende ve tüketici malları

Gerçek zamanlı öneriler ve teklifler sunmak için Azure Cosmos DB Spark desteğini kullanabilirsiniz. Müşterilerin gerçek zamanlı kişiselleştirme ve ürün önerileriyle ihtiyaç duydukları öğeleri bulmasına yardımcı olabilirsiniz.

* Ürün kataloglarında gerçek zamanlı öneriler oluşturmak için, Apache Spark çalışma zamanı tarafından sunulan yerleşik Machine Learning desteğini kullanabilirsiniz.

* Yaşam süresi değerini sağlayan hedeflenen öneriler sağlamak için, veri akışı verileri, veri satın alma ve müşteri verileri ' ne tıklayabilirsiniz.

* Azure Cosmos DB küresel dağıtım özelliğini kullanarak, bölgelere yayılan yüksek hacimli ürün verileri, milisaniye cinsinden analiz edilebilir.

* Coğrafi olarak dağıtılmış kullanıcılar ve veriler için hızlı bir şekilde Öngörüler elde edebilirsiniz. Doğru zamanda doğru kullanıcıyı doğru zamanda sunarak yükseltme dönüştürme oranını artırabilirsiniz.

* Statik müşteri verileriyle birleştirerek canlı verileri zenginleştirmek için yerleşik Spark akış özelliğinden yararlanabilirsiniz. Bu şekilde, daha kişiselleştirilmiş ve hedeflenmiş reklamları gerçek zamanlı olarak, müşterilerin yaptığı gibi, bağlamında da dağıtabilirsiniz.

Aşağıdaki görüntüde, fiyatlandırma ve yükseltmeleri iyileştirmek için Azure Cosmos DB Spark desteğinin nasıl kullanıldığı gösterilmektedir:

![Fiyatlandırma ve yükseltmeleri iyileştirmek için Azure Cosmos DB Spark desteği](./media/spark-api-introduction/optimize-pricing-and-promotions.png)


Aşağıdaki görüntüde, Azure Cosmos DB Spark desteğinin gerçek zamanlı öneri altyapısında nasıl kullanıldığı gösterilmektedir:

![Gerçek zamanlı öneri altyapısında Spark desteğini Azure Cosmos DB](./media/spark-api-introduction/real-time-recommendation-engine.png)

### <a name="manufacturing-and-iot"></a>Üretim ve IoT

Azure Cosmos DB yerleşik analiz platformu, küresel ölçekte Milyonlarca cihazdan IoT verilerinin gerçek zamanlı analizini etkinleştirmenizi sağlar. Hava durumu desenleri, tahmine dayalı analiz ve enerji iyileştirmeleri için öngörülü Öngörüler elde edebilirsiniz.

* Azure Cosmos DB kullanarak, gerçek zamanlı varlık ölçümleri ve hava durumu faktörleri gibi verilerin içeriğini alabilir ve ardından akıllı kılavuz Analizi uygulayarak alana bağlı cihazların performansını iyileştirebilirsiniz. Akıllı kılavuz analizi, kılavuz güvenilirliğini artırmak ve tüketicilere kişiselleştirilmiş enerji hizmetleri sunmak için, işletim maliyetlerini denetleyen anahtardır.

Aşağıdaki görüntüde, IoT cihazlarından ölçümleri okumak ve akıllı kılavuz Analizi uygulamak için Azure Cosmos DB Spark desteğinin nasıl kullanıldığı gösterilmektedir:

![IoT cihazlarından ölçümleri okumak için Azure Cosmos DB Spark desteği](./media/spark-api-introduction/read-metrics-from-iot-devices.png)


### <a name="predictive-maintenance"></a>Tahmine dayalı bakım

* Küçük bir ayrıntıya gitme gibi varlıkların, derinlemesine ve derin platformlar arasında saklanması karmaşık bir Endeavor. Bu varlıklar dünya genelinde bulunur ve veri petabaytlarca oluşturur. Azure Cosmos DB kullanarak, büyük miktarlarda algılayıcı telemetrisi, mağaza varlık parçaları ve algılayıcı eşlemeleri verilerini işlemek için Spark akışını kullanan uçtan uca tahmine dayalı bir veri işlem hattı oluşturabilirsiniz.

* Varlık hatalarını gerçekleşmeden önce tahmin etmek ve hata oluşmadan önce bakım iş emirleri vermek için makine öğrenimi modelleri oluşturabilir ve dağıtabilirsiniz.

Aşağıdaki görüntüde, tahmine dayalı bakım sistemi oluşturmak için Azure Cosmos DB Spark desteğinin nasıl kullanıldığı gösterilmektedir:

![Tahmine dayalı bakım sistemi oluşturmak için Azure Cosmos DB Spark desteği](./media/spark-api-introduction/predictive-maintenance-system.png)

Aşağıdaki görüntüde gerçek zamanlı bir araç Tanılama sistemi oluşturmak için Azure Cosmos DB Spark desteğinin nasıl kullanıldığı gösterilmektedir:

![Gerçek zamanlı bir araç Tanılama sistemi oluşturmak için Azure Cosmos DB Spark desteği](./media/spark-api-introduction/real-time-vehicle-diagnostic-system.png)

### <a name="gaming"></a>Oyun

* Azure Cosmos DB yerleşik Spark desteğiyle, mümkün olan en iyi oyun deneyimini oluşturmak için gelişmiş analiz ve makine öğrenimi modellerini dakikalar içinde kolayca oluşturmanızı, ölçeklendirmenizi ve dağıtmanızı sağlar.

* Yüksek dönüştürme oranlarına ulaşmak için ilgili kişiselleştirilmiş teklifler oluşturmak üzere oynatıcı, satın alma ve davranışsal verileri çözümleyebilirsiniz.

* Spark Machine Learning 'i kullanarak, oyun telemetri verilerini analiz edebilir ve Öngörüler elde edebilirsiniz. Yavaş yükleme sürelerini ve oyun içi sorunları tanılayabilir ve engelleyebilirsiniz.

Aşağıdaki görüntüde, oyun analizinin Azure Cosmos DB Spark desteğinin nasıl kullanıldığı gösterilmektedir:

![Oyun analizinden Azure Cosmos DB Spark desteği](./media/spark-api-introduction/gaming-analytics.png)

## <a name="built-in-jupyter-notebooks-support"></a>Yerleşik jupi Not defterleri desteği

Azure Cosmos DB Cassandra, MongoDB, SQL, Gremlin ve Table gibi tüm API 'Lerde yerleşik jupi not defterlerini destekler. Jupyıter Not defterleri Azure Cosmos hesaplarında çalışır ve geliştirici deneyimini geliştirir. Tüm Azure Cosmos DB API 'Leri ve veri modelleri için yerleşik Not Defteri desteği, sorguları etkileşimli olarak çalıştırmanızı sağlar. Makine öğrenimi modellerini de yürütebilir ve Azure Cosmos veritabanlarınızda depolanan verileri çözümleyebilirsiniz. Jupyter Not defteri deneyimini kullanarak, depolanan verileri çözümleyebilir, makine öğrenimi modellerini oluşturabilir ve eğitebilirsiniz ve aşağıdaki görüntüde gösterildiği gibi Azure portal verilerde iç içe bir sınırlama gerçekleştirebilirsiniz:

![Azure Cosmos DB 'de jupi Not Defteri desteği](./media/spark-api-introduction/jupyter-notebooks-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB avantajları hakkında bilgi edinmek için [genel bakış](introduction.md) makalesine bakın.
* [MongoDB için Azure Cosmos DB API 'SI ile çalışmaya başlama](mongodb-introduction.md)
* [Azure Cosmos DB Cassandra API kullanmaya başlayın](cassandra-introduction.md)
* [Azure Cosmos DB Gremlin API 'sini kullanmaya başlayın](graph-introduction.md)
* [Azure Cosmos DB Tablo API'si kullanmaya başlayın](table-introduction.md)




