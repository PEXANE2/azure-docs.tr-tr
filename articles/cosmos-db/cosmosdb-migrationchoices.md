---
title: Cosmos DB Geçiş seçenekleri
description: Bu doküman, şirket içi veya bulut verilerinizi Azure Cosmos DB'ye geçirmek için çeşitli seçenekleri açıklar
author: bharathsreenivas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: bharathb
ms.openlocfilehash: 34698a215477abdd7d68c3dfe050657ecf049690
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984904"
---
# <a name="options-to-migrate-your-on-premises-or-cloud-data-to-azure-cosmos-db"></a>Şirket içi veya bulut verilerinizi Azure Cosmos DB'ye geçirme seçenekleri

Çeşitli veri kaynaklarından gelen verileri Azure Cosmos DB'ye yükleyebilirsiniz. Ayrıca, Azure Cosmos DB birden çok API'yi desteklediğinden, hedefler varolan API'lerden biri olabilir. Çeşitli kaynaklardan farklı Azure Cosmos DB API'lerine geçiş yollarını desteklemek için, her geçiş yolu için özel işleme sağlayan birden çok çözüm vardır. Bu belge, kullanılabilir çözümleri listeler ve bunların avantajlarını ve sınırlamalarını açıklar.

## <a name="factors-affecting-the-choice-of-migration-tool"></a>Geçiş aracı seçimini etkileyen faktörler

Geçiş aracının seçimini aşağıdaki etkenler belirler:
* **Çevrimiçi vs çevrimdışı geçiş**: Birçok geçiş aracı yalnızca bir kerelik geçiş yapmak için bir yol sağlar. Bu, veritabanına erişen uygulamaların bir süre kapalı kalma süresiyle karşılaşabileceği anlamına gelir. Bazı geçiş çözümleri, kaynak ve hedef arasında bir çoğaltma ardışık lığı olan canlı geçiş yapmak için bir yol sağlar.

* **Veri kaynağı**: Mevcut veriler Oracle DB2, Datastax Cassanda, Azure SQL Server, PostgreSQL gibi çeşitli veri kaynaklarında bulunabilir. Veriler aynı zamanda varolan bir Azure Cosmos DB hesabında da olabilir ve geçişin amacı veri modelini değiştirmek veya verileri farklı bir bölme anahtarıyla bir kapsayıcıdaki yeniden bölmek olabilir.

* **Azure Cosmos DB API**: Azure Cosmos DB'deki SQL API'si için, Azure Cosmos DB ekibi tarafından geliştirilen ve farklı geçiş senaryolarına yardımcı olan çeşitli araçlar vardır. Diğer API'lerin tümü, topluluk tarafından geliştirilen ve korunan kendi özel araçlar kümesine sahiptir. Azure Cosmos DB bu API'leri bir tel iletişim protokolü düzeyinde desteklediğinden, bu araçlar verileri Azure Cosmos DB'ye aktarırken olduğu gibi çalışmalıdır. Ancak, bu kavram Azure Cosmos DB'ye özgü olduğundan, azaltmalar için özel kullanım gerektirebilir.

* **Veri boyutu**: Çoğu geçiş aracı daha küçük veri kümeleri için çok iyi çalışır. Veri kümesi birkaç yüz gigabaytı aştığında, geçiş araçlarının seçenekleri sınırlıdır. 

* **Beklenen geçiş süresi**: Geçişler, daha az iş ortası tüketen veya hedef Azure Cosmos DB kapsayıcısında sağlanan tüm iş ortasını tüketebilen ve geçişi daha kısa sürede tamamlayabilen yavaş ve aşamalı bir hızda gerçekleşecek şekilde yapılandırılabilir.

## <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API
|**Geçiş türü**|**Çözüm**|**Dikkat edilmesi gerekenler**|
|---------|---------|---------|
|Çevrimdışı|[Veri Geçiş Aracı](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Büyük veri kümeleri için uygun değil|
|Çevrimdışı|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Azure Cosmos DB toplu uygulayıcı kitaplığını kullanır <br/>&bull;Büyük veri kümeleri için uygundur <br/>&bull;Kontrol noktası eksikliği - Bu, geçiş sırasında bir sorun oluşursa, tüm geçiş işlemini yeniden başlatmanız gerektiği anlamına gelir<br/>&bull;Ölü harf sırasının olmaması - Birkaç hatalı dosyanın tüm geçiş işlemini durdurabileceği anlamına gelir.|
|Çevrimdışı|[Azure Cosmos DB Kıvılcım konektörü](https://docs.microsoft.com/azure/cosmos-db/spark-connector)|&bull;Azure Cosmos DB toplu uygulayıcı kitaplığını kullanır <br/>&bull;Büyük veri kümeleri için uygundur <br/>&bull;Özel bir Kıvılcım kurulumuna ihtiyaç duyar <br/>&bull;Kıvılcım şema tutarsızlıklarına karşı hassastır ve bu geçiş sırasında bir sorun olabilir |
|Çevrimdışı|[Cosmos DB toplu uygulayıcı kitaplık ile özel araç](https://docs.microsoft.com/azure/cosmos-db/migrate-cosmosdb-data)|&bull;Geçiş esnekliğini artıran kontrol noktası, ölü harflerle yazma özellikleri sağlar <br/>&bull;Çok büyük veri kümeleri için uygundur (10 TB+)  <br/>&bull;Uygulama Hizmeti olarak çalışan bu aracın özel kurulumgerektirir |
|Çevrimiçi|[Cosmos DB Fonksiyonları + ChangeFeed API](https://docs.microsoft.com/azure/cosmos-db/change-feed-functions)|&bull;Kurulumu kolay <br/>&bull;Yalnızca kaynak Bir Azure Cosmos DB kapsayıcısıysa çalışır <br/>&bull;Büyük veri kümeleri için uygun değil <br/>&bull;Kaynak kapsayıcıdan silmeleri yakalamaz |
|Çevrimiçi|[ChangeFeed kullanarak Özel Geçiş Hizmeti](https://github.com/nomiero/CosmosDBLiveETLSample)|&bull;İlerleme izleme sağlar <br/>&bull;Yalnızca kaynak Bir Azure Cosmos DB kapsayıcısıysa çalışır <br/>&bull;Daha büyük veri kümeleri için de çalışır <br/>&bull;Kullanıcının Akış Işlemcisini Değiştir'i barındırmak için bir Uygulama Hizmeti ayarlamasını gerektirir <br/>&bull;Kaynak kapsayıcıdan silmeleri yakalamaz|
|Çevrimiçi|[Striim](https://docs.microsoft.com/azure/cosmos-db/cosmosdb-sql-api-migrate-data-striim)|&bull;Oracle, DB2, SQL Server gibi çok çeşitli kaynaklarla çalışır <br/>&bull;ETL boru hatları oluşturmak kolay ve izleme için bir pano sağlar <br/>&bull;Daha büyük veri kümelerini destekler <br/>&bull;Bu bir üçüncü taraf aracı olduğundan, pazardan satın alınması ve kullanıcının ortamına yüklenmesi gerekir|

## <a name="azure-cosmos-db-mongo-api"></a>Azure Cosmos DB Mongo API
|**Geçiş türü**|**Çözüm**|**Dikkat edilmesi gerekenler**|
|---------|---------|---------|
|Çevrimdışı|[Veri Geçiş Aracı](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Büyük veri kümeleri için uygun değil|
|Çevrimdışı|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Azure Cosmos DB toplu uygulayıcı kitaplığını kullanır <br/>&bull;Büyük veri kümeleri için uygundur <br/>&bull;Denetim noktasının olmaması, geçiş sırasında ki herhangi bir sorunun tüm geçiş sürecinin yeniden başlatılmasını gerektireceği anlamına gelir<br/>&bull;Ölü harf sırasının olmaması, birkaç hatalı dosyanın tüm geçiş işlemini durdurabileceği anlamına gelir <br/>&bull;Belirli veri kaynakları için okuma iş verisini artırmak için özel kod gerekir|
|Çevrimdışı|[Mevcut Mongo Araçları (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|&bull;Kurulumu ve entegrasyonu kolay <br/>&bull;Gazlar için özel kullanım ihtiyacı|
|Çevrimiçi|[Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db-online)|&bull;Azure Cosmos DB toplu uygulayıcı kitaplığını kullanır <br/>&bull;Büyük veri kümeleri için uygundur ve canlı değişiklikleri çoğaltma yıkıyor <br/>&bull;Yalnızca diğer MongoDB kaynaklarıyla çalışır|

## <a name="azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'si
|**Geçiş türü**|**Çözüm**|**Dikkat edilmesi gerekenler**|
|---------|---------|---------|
|Çevrimdışı|[cqlsh COPY komutu](https://docs.microsoft.com/azure/cosmos-db/cassandra-import-data#migrate-data-using-cqlsh-copy-command)|&bull;Kurulumu kolay <br/>&bull;Büyük veri kümeleri için uygun değil <br/>&bull;Yalnızca kaynak Cassandra tablosu olduğunda çalışır|
|Çevrimdışı|[Spark ile kopyalama tablosu](https://docs.microsoft.com/azure/cosmos-db/cassandra-import-data#migrate-data-using-spark) |&bull;Dönüşüm ve yutma paralelleştirmek için Kıvılcım yetenekleri nden yararlanabilir <br/>&bull;Gaz ları işlemek için özel bir yeniden deneme ilkesiyle yapılandırmaya ihtiyaç duyar|
|Çevrimiçi|[Striim (Oracle DB/Apache Cassandra'dan)](https://docs.microsoft.com/azure/cosmos-db/cosmosdb-cassandra-api-migrate-data-striim)|&bull;Oracle, DB2, SQL Server gibi çok çeşitli kaynaklarla çalışır <br/>&bull;ETL boru hatları oluşturmak kolay ve izleme için bir pano sağlar <br/>&bull;Daha büyük veri kümelerini destekler <br/>&bull;Bu bir üçüncü taraf aracı olduğundan, pazardan satın alınması ve kullanıcının ortamına yüklenmesi gerekir|
|Çevrimiçi|[Blitzz (Oracle DB/Apache Cassandra'dan)](https://docs.microsoft.com/azure/cosmos-db/oracle-migrate-cosmos-db-blitzz)|<br/>&bull;Daha büyük veri kümelerini destekler <br/>&bull;Bu bir üçüncü taraf aracı olduğundan, pazardan satın alınması ve kullanıcının ortamına yüklenmesi gerekir|

## <a name="other-apis"></a>Diğer API'ler
SQL API, Mongo API ve Cassandra API dışındaki API'ler için, API'nin mevcut ekosistemlerinin her biri tarafından desteklenen çeşitli araçlar vardır. 

**Tablo API’si** 
* [Veri Geçiş Aracı](https://docs.microsoft.com/azure/cosmos-db/table-import#data-migration-tool)
* [Azcopy](https://docs.microsoft.com/azure/cosmos-db/table-import#migrate-data-by-using-azcopy)

**Gremlin API**
* [Grafik toplu uygulayıcı kitaplığı](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-graph-dotnet)
* [Gremlin Kıvılcım](https://github.com/Azure/azure-cosmosdb-spark/blob/2.4/samples/graphframes/main.scala) 

## <a name="next-steps"></a>Sonraki adımlar

* [.NET](bulk-executor-dot-net.md) ve [Java'daki](bulk-executor-java.md)toplu yürütme kitaplığını tüketen örnek uygulamaları deneyerek daha fazla bilgi edinin. 
* Toplu uygulayıcı kitaplığı, daha fazla bilgi edinmek için Azure [Cosmos DB Spark bağlayıcısı](spark-connector.md) makalesine bakın.  
* Büyük ölçekli geçişlerde ek yardım için "Genel Danışma" sorun türü ve "Büyük (TB+) geçişler" sorun alt türü altında bir destek bileti açarak Azure Cosmos DB ürün ekibine başvurun.
