---
title: Cosmos DB geçiş seçenekleri
description: Bu belge, şirket içi veya bulut verilerinizi Azure Cosmos DB 'e geçirmeye yönelik çeşitli seçenekleri açıklar.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 4de6d4ba019af75b0f6179b2794ddb6c1e35e0c1
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030081"
---
# <a name="options-to-migrate-your-on-premises-or-cloud-data-to-azure-cosmos-db"></a>Şirket içi veya bulut verilerinizi Azure Cosmos DB 'e geçirmeye yönelik seçenekler

Azure Cosmos DB için çeşitli veri kaynaklarından veri yükleyebilirsiniz. Azure Cosmos DB birden çok API 'yi desteklediğinden, hedefler mevcut API 'lerden herhangi biri olabilir. Aşağıda Azure Cosmos DB verileri geçirebileceğiniz bazı senaryolar verilmiştir:

* Verileri bir Azure Cosmos kapsayıcısından aynı veritabanındaki başka bir kapsayıcıya veya farklı bir veritabanına taşıyın. s
* Ayrılmış kapsayıcılar arasında verileri paylaşılan veritabanı kapsayıcılarına taşıma.
* Region1 içinde bulunan bir Azure Cosmos hesabındaki verileri, aynı veya farklı bir bölgedeki başka bir Azure Cosmos hesabına taşıyın.
* Azure Blob depolama, bir JSON dosyası, Oracle Database, Couşbase, DynamoDB gibi bir kaynaktan verileri Azure Cosmos DB taşıyın.

Çeşitli kaynaklardan farklı Azure Cosmos DB API 'Lerine geçiş yollarını desteklemek için, her geçiş yolu için özel işleme sağlayan birden çok çözüm vardır. Bu belgede, kullanılabilir çözümler listelenmekte ve bunların avantajları ve sınırlamaları açıklanmaktadır.

## <a name="factors-affecting-the-choice-of-migration-tool"></a>Geçiş Aracı seçimini etkileyen faktörler

Aşağıdaki etmenler geçiş aracının seçimini belirlemektir:

* **Çevrimiçi ve çevrimdışı geçiş**: birçok geçiş aracı yalnızca bir kerelik geçiş yapmak için bir yol sağlar. Bu, veritabanına erişen uygulamaların bir süre kapalı kalma süresine neden olabileceği anlamına gelir. Bazı geçiş çözümleri, kaynak ve hedef arasında bir çoğaltma işlem hattının ayarlandığı dinamik geçiş yapmak için bir yol sağlar.

* **Veri kaynağı**: mevcut VERILER Oracle DB2, DataStax Cassanda, Azure SQL veritabanı, PostgreSQL vb. gibi çeşitli veri kaynaklarında bulunabilir. Veriler de mevcut bir Azure Cosmos DB hesabında olabilir ve geçiş amacı veri modelini değiştirebilir veya bir kapsayıcıdaki verileri farklı bir bölüm anahtarıyla yeniden bölümleyebilir.

* **Azure Cosmos DB API**: Azure Cosmos db SQL API 'si için, farklı geçiş senaryolarına yardımcı olan Azure Cosmos DB ekibi tarafından geliştirilen çeşitli araçlar vardır. Diğer tüm API 'Lerde, topluluk tarafından geliştirilen ve tutulan kendi özelleştirilmiş araç kümesi vardır. Azure Cosmos DB, bu API 'Leri bir kablo protokol düzeyinde desteklediğinden, bu araçların verileri Azure Cosmos DB geçirirken olduğu gibi çalışması gerekir. Ancak, bu kavram Azure Cosmos DB özel olarak, bu kavram için özel işleme gerektirebilir.

* **Verilerin boyutu**: çoğu geçiş araçları daha küçük veri kümeleri için çok iyi çalışır. Veri kümesi birkaç yüz gigabayttan geçtiğinde, geçiş araçlarının seçimleri sınırlıdır. 

* **Geçiş süresi bekleniyordu**: geçişler, daha az üretilen iş harcayan veya hedef Azure Cosmos DB kapsayıcısında sağlanan tüm aktarım hızını tüketebilir ve geçişi daha az zamanda tamamlayabileceği yavaş, artımlı bir hızda gerçekleşecek şekilde yapılandırılabilir.

## <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API

|Geçiş türü|Çözüm|Desteklenen kaynaklar|Desteklenen hedefler|Dikkat edilmesi gerekenler|
|---------|---------|---------|---------|---------|
|Çevrimdışı|[Veri Geçişi Aracı](import-data.md)| &bull;JSON/CSV dosyaları<br/>&bull;Azure Cosmos DB SQL API<br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Tablo Depolama<br/>&bull;AWS DynamoDB<br/>&bull;Azure Blob depolama|&bull;Azure Cosmos DB SQL API<br/>&bull;Azure Cosmos DB tabloları API 'SI<br/>&bull;JSON dosyaları |&bull; Daha kolay bir şekilde ayarlanır ve birden çok kaynağı destekler. <br/>&bull; Büyük veri kümeleri için uygun değildir.|
|Çevrimdışı|[Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)| &bull;JSON/CSV dosyaları<br/>&bull;Azure Cosmos DB SQL API<br/>&bull;MongoDB için Azure Cosmos DB API<br/>&bull;MongoDB <br/>&bull;SQL Server<br/>&bull;Tablo Depolama<br/>&bull;Azure Blob depolama <br/> <br/>Desteklenen diğer kaynaklar için [Azure Data Factory](../data-factory/connector-overview.md) makalesine bakın.|&bull;Azure Cosmos DB SQL API<br/>&bull;MongoDB için Azure Cosmos DB API<br/>&bull;JSON dosyaları <br/><br/> Desteklenen diğer hedefler için [Azure Data Factory](../data-factory/connector-overview.md) makalesine bakın. |&bull; Daha kolay bir şekilde ayarlanır ve birden çok kaynağı destekler.<br/>&bull; Azure Cosmos DB toplu yürütücü Kitaplığı ' nı kullanır. <br/>&bull; Büyük veri kümeleri için uygundur. <br/>&bull; Denetimsiz işaret olmaması-geçiş işlemi sırasında bir sorun oluşursa, tüm geçiş sürecini yeniden başlatmanız gerekir.<br/>&bull; Geçerliliği kalmamış bir sıra yok-bu, birkaç hatalı dosyanın tüm geçiş sürecini durduramaması anlamına gelir.|
|Çevrimdışı|[Azure Cosmos DB Spark Bağlayıcısı](spark-connector.md)|SQL API Azure Cosmos DB. <br/><br/>Diğer kaynakları Spark ekosistemindeki ek bağlayıcılarla birlikte kullanabilirsiniz.| SQL API Azure Cosmos DB. <br/><br/>Diğer hedefleri Spark ekosistemindeki ek bağlayıcılarla birlikte kullanabilirsiniz.| &bull; Azure Cosmos DB toplu yürütücü Kitaplığı ' nı kullanır. <br/>&bull; Büyük veri kümeleri için uygundur. <br/>&bull; Özel bir Spark kurulumuna ihtiyaç duyuyor. <br/>&bull; Spark, şema tutarsızlıklarına duyarlıdır ve bu, geçiş sırasında bir sorun olabilir. |
|Çevrimdışı|[Cosmos DB toplu yürütücü kitaplığı ile özel araç](migrate-cosmosdb-data.md)| Kaynak, özel kodunuza bağlıdır | Azure Cosmos DB SQL API| &bull; Geçiş dayanıklılığını artıran denetim noktası, ölü Özellik özellikleri sağlar. <br/>&bull; Çok büyük veri kümeleri (10 TB +) için uygundur.  <br/>&bull; App Service olarak çalışan bu aracın özel kurulumunu gerektirir. |
|Çevrimiçi|[Cosmos DB Işlevleri + ChangeFeed API 'SI](change-feed-functions.md)| Azure Cosmos DB SQL API | Azure Cosmos DB SQL API| &bull; Kolayca ayarlanabilir. <br/>&bull; Yalnızca kaynak bir Azure Cosmos DB kapsayıcısı ise geçerlidir. <br/>&bull; Büyük veri kümeleri için uygun değildir. <br/>&bull; Kaynak kapsayıcıdan silmeleri yakalamaz. |
|Çevrimiçi|[Değişiklik akışını kullanarak özel geçiş hizmeti](https://github.com/Azure-Samples/azure-cosmosdb-live-data-migrator)| Azure Cosmos DB SQL API | Azure Cosmos DB SQL API| &bull; İlerleme durumunu izleme sağlar. <br/>&bull; Yalnızca kaynak bir Azure Cosmos DB kapsayıcısı ise geçerlidir. <br/>&bull; Daha büyük veri kümeleri için de geçerlidir.<br/>&bull; Kullanıcının değişiklik akışı işlemcisini barındırmak için bir App Service ayarlaması gerekir. <br/>&bull; Kaynak kapsayıcıdan silmeleri yakalamaz.|
|Çevrimiçi|[Anlık ileti](cosmosdb-sql-api-migrate-data-striim.md)| &bull;Oracle <br/>&bull;Apache Cassandra<br/><br/> Desteklenen diğer kaynaklar için bkz. [anlık ileti Web sitesi](https://www.striim.com/sources-and-targets/) . |&bull;Azure Cosmos DB SQL API <br/>&bull; Azure Cosmos DB Cassandra API<br/><br/> Desteklenen diğer hedefler için bkz. [anlık ileti Web sitesi](https://www.striim.com/sources-and-targets/) . | &bull; Oracle, DB2, SQL Server gibi çok çeşitli kaynaklarla birlikte çalışarak.<br/>&bull; ETL işlem hatlarını kolayca oluşturun ve izleme için bir pano sağlar. <br/>&bull; Daha büyük veri kümelerini destekler. <br/>&bull; Bu bir üçüncü taraf araç olduğundan Market 'ten satın alınması ve kullanıcının ortamına yüklenmesi gerekir.|

## <a name="azure-cosmos-db-mongo-api"></a>Azure Cosmos DB Mongo API 'SI

|Geçiş türü|Çözüm|Desteklenen kaynaklar|Desteklenen hedefler|Dikkat edilmesi gerekenler|
|---------|---------|---------|---------|---------|
|Çevrimiçi|[Azure Veritabanı Geçiş Hizmeti](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB|MongoDB için Azure Cosmos DB API |&bull; Azure Cosmos DB toplu yürütücü Kitaplığı ' nı kullanır. <br/>&bull; Büyük veri kümeleri için uygundur ve canlı değişiklikleri çoğaltmaya özen gösterin. <br/>&bull; Yalnızca diğer MongoDB kaynaklarıyla birlikte geçerlidir.|
|Çevrimdışı|[Azure Veritabanı Geçiş Hizmeti](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB| MongoDB için Azure Cosmos DB API| &bull; Azure Cosmos DB toplu yürütücü Kitaplığı ' nı kullanır. <br/>&bull; Büyük veri kümeleri için uygundur ve canlı değişiklikleri çoğaltmaya özen gösterin. <br/>&bull; Yalnızca diğer MongoDB kaynaklarıyla birlikte geçerlidir.|
|Çevrimdışı|[Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)| &bull;JSON/CSV dosyaları<br/>&bull;Azure Cosmos DB SQL API<br/>&bull;MongoDB için Azure Cosmos DB API <br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Tablo Depolama<br/>&bull;Azure Blob depolama <br/><br/> Desteklenen diğer kaynaklar için [Azure Data Factory](../data-factory/connector-overview.md) makalesine bakın. | &bull;Azure Cosmos DB SQL API<br/>&bull;MongoDB için Azure Cosmos DB API <br/>&bull; JSON dosyaları <br/><br/> Desteklenen diğer hedefler için [Azure Data Factory](../data-factory/connector-overview.md) makalesine bakın.| &bull; Daha kolay bir şekilde ayarlanır ve birden çok kaynağı destekler. <br/>&bull; Azure Cosmos DB toplu yürütücü Kitaplığı ' nı kullanır. <br/>&bull; Büyük veri kümeleri için uygundur. <br/>&bull; CheckIn olmaması, geçiş işlemi sırasında herhangi bir sorunun tüm geçiş sürecinin yeniden başlatılmasını gerektirmeyeceği anlamına gelir.<br/>&bull; Sahipsiz bir sıra olmaması, birkaç hatalı dosyanın tüm geçiş sürecini durduramaması anlamına gelir. <br/>&bull; Belirli veri kaynakları için okuma aktarım hızını artırmak için özel koda ihtiyaç duyuyor.|
|Çevrimdışı|[Mevcut Mongo araçları (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|MongoDB | MongoDB için Azure Cosmos DB API| &bull; Kolayca ayarlama ve tümleştirme. <br/>&bull; Kısıtlar için özel işlemeye ihtiyaç duyuyor.|

## <a name="azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'si

|Geçiş türü|Çözüm|Desteklenen kaynaklar|Desteklenen hedefler|Dikkat edilmesi gerekenler|
|---------|---------|---------|---------|---------|
|Çevrimdışı|[csqlsh COPY komutu](cassandra-import-data.md#migrate-data-using-cqlsh-copy-command)|CSV dosyaları | Azure Cosmos DB Cassandra API'si| &bull; Kolayca ayarlanabilir. <br/>&bull; Büyük veri kümeleri için uygun değildir. <br/>&bull; Yalnızca kaynak bir Cassandra tablosu olduğunda geçerlidir.|
|Çevrimdışı|[Spark ile tablo kopyalama](cassandra-import-data.md#migrate-data-using-spark) | &bull;Apache Cassandra<br/>&bull;Azure Cosmos DB Cassandra API'si| Azure Cosmos DB Cassandra API'si | &bull; , Paralel hale getirmek dönüşüm ve alımı için Spark yeteneklerini kullanabilir. <br/>&bull; Azaltıcı lıkları işlemek için özel bir yeniden deneme ilkesiyle yapılandırma gerekir.|
|Çevrimiçi|[Anlık ileti (Oracle DB/Apache Cassandra 'dan)](cosmosdb-cassandra-api-migrate-data-striim.md)| &bull;Oracle<br/>&bull;Apache Cassandra<br/><br/> Desteklenen diğer kaynaklar için bkz. [anlık ileti Web sitesi](https://www.striim.com/sources-and-targets/) .|&bull;Azure Cosmos DB SQL API<br/>&bull;Azure Cosmos DB Cassandra API'si <br/><br/> Desteklenen diğer hedefler için bkz. [anlık ileti Web sitesi](https://www.striim.com/sources-and-targets/) .| &bull; Oracle, DB2, SQL Server gibi çok çeşitli kaynaklarla birlikte çalışarak. <br/>&bull; ETL işlem hatlarını kolayca oluşturun ve izleme için bir pano sağlar. <br/>&bull; Daha büyük veri kümelerini destekler. <br/>&bull; Bu bir üçüncü taraf araç olduğundan Market 'ten satın alınması ve kullanıcının ortamına yüklenmesi gerekir.|
|Çevrimiçi|[Blitzz (Oracle DB/Apache Cassandra 'dan)](oracle-migrate-cosmos-db-blitzz.md)|&bull;Oracle<br/>&bull;Apache Cassandra<br/><br/>Desteklenen diğer kaynaklar için [Blitzz Web sitesine](https://www.blitzz.io/) bakın. |Azure Cosmos DB Cassandra API. <br/><br/>Desteklenen diğer hedefler için [Blitzz Web sitesine](https://www.blitzz.io/) bakın. | &bull; Daha büyük veri kümelerini destekler. <br/>&bull; Bu bir üçüncü taraf araç olduğundan Market 'ten satın alınması ve kullanıcının ortamına yüklenmesi gerekir.|

## <a name="other-apis"></a>Diğer API'ler

SQL API, Mongo API ve Cassandra API dışındaki API 'Ler için, her API 'nin mevcut ekosistemlerinin her biri tarafından desteklenen çeşitli araçlar vardır. 

**Tablo API’si** 

* [Veri Geçişi Aracı](table-import.md#data-migration-tool)
* [AzCopy](table-import.md#migrate-data-by-using-azcopy)

**Gremlin API 'SI**

* [Graph toplu yürütücü kitaplığı](bulk-executor-graph-dotnet.md)
* [Gremlin Spark](https://github.com/Azure/azure-cosmosdb-spark/blob/2.4/samples/graphframes/main.scala) 

## <a name="next-steps"></a>Sonraki adımlar

* [.Net](bulk-executor-dot-net.md) ve [Java](bulk-executor-java.md)'daki toplu yürütücü kitaplığını kullanan örnek uygulamaları deneyerek daha fazla bilgi edinin. 
* Toplu yürütücü kitaplığı, Cosmos DB Spark Bağlayıcısı ile tümleşiktir, daha fazla bilgi edinmek için bkz. [Azure Cosmos DB Spark Bağlayıcısı](spark-connector.md) makalesi.  
* Büyük ölçekli geçişlerle ilgili ek yardım için "genel Danışma belgesi" sorun türü ve "büyük (TB +) geçişleri" sorun alt türü altında bir destek bileti açarak Azure Cosmos DB ürün ekibine başvurun.
