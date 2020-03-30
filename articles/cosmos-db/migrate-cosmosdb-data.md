---
title: Azure Cosmos DB’ye yüzlerce terabaytlık verileri geçirme
description: Bu doküman, 100'lerdir terabaytlık veriyi Cosmos DB'ye nasıl geçirebileceğinizi açıklar
author: bharathsreenivas
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: bharathb
ms.openlocfilehash: 69b400eb7838c986ac6f275da58c7457179ebea6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72880214"
---
# <a name="migrate-hundreds-of-terabytes-of-data-into-azure-cosmos-db"></a>Azure Cosmos DB’ye yüzlerce terabaytlık verileri geçirme 

Azure Cosmos DB terabaytlarca veriyi depolayabilir. Üretim iş yükünüzü Azure Cosmos DB’ye taşımak için büyük ölçekli bir veri geçişi yapabilirsiniz. Bu makalede büyük ölçekli verileri Azure Cosmos DB’ye geçirmenin zorlukları açıklanır; zorluklarda size yardımcı olan ve verileri Azure Cosmos DB’ye geçiren araç tanıtılır. Müşteri, bu örnek olay incelemesinde Cosmos DB SQL API kullandı.  

İş yükünün tamamını Azure Cosmos DB'ye geçirmeden önce, bölüm anahtar seçimi, sorgu performansı ve veri modelleme gibi bazı yönlerini doğrulamak için bir veri alt kümesini geçirebilirsiniz. Kavram kanıtını doğruladıktan sonra, tüm iş yükünü Azure Cosmos DB'ye taşıyabilirsiniz.  

## <a name="tools-for-data-migration"></a>Veri geçişi için araçlar 

Azure Cosmos DB geçiş stratejileri şu anda API seçimine ve verilerin boyutuna bağlı olarak farklılık göstermektedir. Veri modellemesini, sorgu performansını, bölüm anahtar seçimini vb. doğrulamak için daha küçük veri kümelerini geçirmek için [Veri Geçiş Aracı'nı](import-data.md) veya [Azure Veri Fabrikası'nın Azure Cosmos DB bağlayıcısını](../data-factory/connector-azure-cosmos-db.md)seçebilirsiniz. Spark'ı biliyorsanız, verileri geçirmek için [Azure Cosmos DB Spark bağlayıcısını](spark-connector.md) da kullanmayı seçebilirsiniz.

## <a name="challenges-for-large-scale-migrations"></a>Büyük ölçekli göçler için zorluklar 

Verileri Azure Cosmos DB'ye geçirmek için varolan araçlar, özellikle büyük ölçeklerde belirgin hale gelen bazı sınırlamalara sahiptir:

 * **Sınırlı ölçeklendirme yetenekleri**: Terabaytlarca veriyi mümkün olan en kısa sürede Azure Cosmos DB'ye geçirmek ve sağlanan tüm iş ortasını etkin bir şekilde tüketmek için, geçiş istemcilerinin süresiz olarak ölçeklendirme yeteneğine sahip olması gerekir.  

* **İlerleme izleme ve kontrol işaretleme eksikliği**: Geçiş ilerlemesini izlemek ve büyük veri kümelerini geçirerken kontrol işaretetme yapmak önemlidir. Aksi takdirde, geçiş sırasında oluşan herhangi bir hata geçişi durdurur ve işlemi sıfırdan başlatmanız gerekir. %99'u tamamlanmışken tüm geçiş sürecini yeniden başlatmak verimli olmaz.  

* **Ölü harf sırası nın olmaması**: Büyük veri kümeleri içinde, bazı durumlarda kaynak verilerin bölümlerinde sorunlar olabilir. Ayrıca, istemci veya ağ ile geçici sorunlar olabilir. Bu durumlardan biri tüm geçiş başarısız neden olmamalıdır. Geçiş araçlarının çoğu, aralıklı sorunlara karşı koruma sağlayan sağlam yeniden deneme özelliklerine sahip olsa da, her zaman yeterli değildir. Örneğin, kaynak veri belgelerinin %0,01'inden azının boyutu 2 MB'dan büyükse, belge yazmanın Azure Cosmos DB'de başarısız olmasına neden olur. İdeal olarak, geçiş aracının bu 'başarısız' belgeleri geçiş sonrası işlenebilen başka bir ölü harf kuyruğuna devam etmesi yararlıdır. 

Bu sınırlamaların çoğu Azure Veri fabrikası, Azure Veri Geçişi hizmetleri gibi araçlar için düzeltiliyor. 

## <a name="custom-tool-with-bulk-executor-library"></a>Toplu yürütme kitaplığı ile özel araç 

Yukarıdaki bölümde açıklanan zorluklar, kolayca birden fazla örnek arasında ölçeklenebilir ve geçici hatalara dayanıklı özel bir araç kullanılarak çözülebilir. Ayrıca, özel araç duraklatmak ve çeşitli denetim noktalarında geçiş devam edebilirsiniz. Azure Cosmos DB, bu özelliklerden bazılarını içeren [toplu yürütme kitaplığını](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-overview) zaten sağlar. Örneğin, toplu yürütme kitaplığı zaten geçici hataları işlemek için işlevselliğe sahiptir ve düğüm başına yaklaşık 500 K RUs tüketmek için tek bir düğümdeki iş parçacıklarını ölçeklendirebilir. Toplu yürütme kitaplığı da bir denetim noktası biçimi olarak bağımsız olarak işletilen mikro toplu olarak kaynak veri kümesi bölümlere.  

Özel araç toplu yürütme kitaplığını kullanır ve birden çok istemci arasında ölçekleme ve yutma işlemi sırasında hataları izlemek için destekler. Bu aracı kullanmak için kaynak verilerin Azure Veri Gölü Depolama (ADLS)'deki farklı dosyalara bölünmesi gerekir, böylece farklı geçiş çalışanları her dosyayı alıp Azure Cosmos DB'ye alabilir. Özel araç, ADLS'deki her bir kaynak dosya için geçiş ilerlemesiyle ilgili meta verileri depolayan ve bunlarla ilişkili hataları izleyen ayrı bir koleksiyon kullanır.  

Aşağıdaki resimde bu özel aracı kullanarak geçiş işlemi açıklanır. Araç bir sanal makine kümesiüzerinde çalışıyor ve her sanal makine, kaynak veri bölümlerinden birini kiralamak için Azure Cosmos DB'deki izleme koleksiyonunu sorgular. Bu işlem yapıldıktan sonra, kaynak veri bölümü araç tarafından okunur ve toplu yürütme kitaplığı kullanılarak Azure Cosmos DB'ye yutulrülür. Ardından, izleme koleksiyonu veri alım işleminin ilerlemesini ve karşılaşılan hataları kaydetmek için güncelleştirilir. Bir veri bölümü işlendikten sonra, araç bir sonraki kullanılabilir kaynak bölümü için sorgu yapmaya çalışır. Tüm veriler geçirilene kadar sonraki kaynak bölümü işlemeye devam ediyor. Aracın kaynak [koduna buradan](https://github.com/Azure-Samples/azure-cosmosdb-bulkingestion)ulaşabilirsiniz.  

 
![Geçiş Aracı Kurulumu](./media/migrate-cosmosdb-data/migrationsetup.png)
 

 

İzleme koleksiyonu aşağıdaki örnekte gösterildiği gibi belgeleri içerir. Bu tür belgeleri kaynak verilerdeki her bölüm için bir tane görürsünüz.  Her belge, konumu, geçiş durumu ve hataları (varsa) gibi kaynak veri bölümü için meta verileri içerir:  

```json
{ 
  "owner": "25812@bulkimporttest07", 
  "jsonStoreEntityImportResponse": { 
    "numberOfDocumentsReceived": 446688, 
    "isError": false, 
    "totalRequestUnitsConsumed": 3950252.2800000003, 
    "errorInfo": [], 
    "totalTimeTakenInSeconds": 188, 
    "numberOfDocumentsImported": 446688 
  }, 
  "storeType": "AZURE_BLOB", 
  "name": "sourceDataPartition", 
  "location": "sourceDataPartitionLocation", 
  "id": "sourceDataPartitionId", 
  "isInProgress": false, 
  "operation": "unpartitioned-writes", 
  "createDate": { 
    "seconds": 1561667225, 
    "nanos": 146000000 
  }, 
  "completeDate": { 
    "seconds": 1561667515, 
    "nanos": 180000000 
  }, 
  "isComplete": true 
} 
```
 

## <a name="prerequisites-for-data-migration"></a>Veri geçişi için ön koşullar 

Veri geçişi başlamadan önce göz önünde bulundurulması gereken birkaç ön koşul vardır:  

#### <a name="estimate-the-data-size"></a>Veri boyutunu tahmin edin:  

Kaynak veri boyutu, Azure Cosmos DB'deki veri boyutuyla tam olarak eşlenemeyebilir. Azure Cosmos DB'deki veri boyutlarını denetlemek için kaynaktan birkaç örnek belge eklenebilir. Örnek belge boyutuna bağlı olarak, Azure Cosmos DB geçiş sonrası toplam veri boyutu tahmin edilebilir. 

Örneğin, Azure Cosmos DB'de geçişten sonraki her belge 1 KB civarındaysa ve kaynak veri kümesinde yaklaşık 60 milyar belge varsa, Bu, Azure Cosmos DB'deki tahmini boyutun 60 TB'ye yakın olacağı anlamına gelir. 

 

#### <a name="pre-create-containers-with-enough-rus"></a>Yeterli RUs'lu kapları önceden oluşturun: 

Azure Cosmos DB depolama alanını otomatik olarak ölçeklendirse de, en küçük kapsayıcı boyutundan başlamak tavsiye edilmez. Daha küçük kapsayıcılar daha düşük iş kullanılabilirliğine sahiptir, bu da geçişin tamamlanmasının çok daha uzun süreceği anlamına gelir. Bunun yerine, son veri boyutuna (önceki adımda tahmin edildiği gibi) sahip kapsayıcılar oluşturmak ve geçiş iş yükünün sağlanan iş kaynağının tam olarak tüketdiğinden emin olmak yararlıdır.  

Önceki adımda. veri boyutunun 60 TB civarında olduğu tahmin edildiğinden, tüm veri kümesini karşılamak için en az 2,4 M RUs'luk bir kapsayıcı gereklidir.  

 

#### <a name="estimate-the-migration-speed"></a>Geçiş hızını tahmin edin: 

Geçiş iş yükünün, sağlanan iş yükünün tamamını tüketebileceğini varsayarsak, bu arada sağlanan geçiş hızının bir tahminini sağlar. Önceki örneği devam ederken, Azure Cosmos DB SQL API hesabına 1 KB belge yazmak için 5 RUs gerekir.  2,4 milyon RUs saniyede 480.000 belge (veya 480 MB/sn) transferine izin verir. Bu, 60 TB'nin tam geçişinin 125.000 saniye veya yaklaşık 34 saat süreceği anlamına gelir.  

Geçişin bir gün içinde tamamlanmasını istiyorsanız, sağlanan iş buzun 5 milyon RUs'a çıkarılması gerekir. 

 

#### <a name="turn-off-the-indexing"></a>Dizin oluşturmayı kapatın:  

Geçişin mümkün olan en kısa sürede tamamlanması gerektiğinden, alınan belgelerin her biri için dizin oluşturmak için harcanan zamanı ve RUS'ları en aza indirmesi tavsiye edilir.  Azure Cosmos DB tüm özellikleri otomatik olarak dizine alır, dizin oluşturmayı seçili birkaç terime en aza indirmek veya geçiş seyri için tamamen kapatmak faydalıdır. IndexingMode'u aşağıda gösterildiği gibi yok olarak değiştirerek kapsayıcının dizin oluşturma ilkesini kapatabilirsiniz:  

 
```
  { 
        "indexingMode": "none" 
  } 
```
 

Geçiş tamamlandıktan sonra dizin oluşturmayı güncelleştirebilirsiniz.  

## <a name="migration-process"></a>Geçiş süreci 

Ön koşullar tamamlandıktan sonra, verileri aşağıdaki adımlarla geçirebilirsiniz:  

1. Önce verileri kaynaktan Azure Blob Depolama'ya aktarın. Geçiş hızını artırmak için, farklı kaynak bölümleri arasında paralelleştirmek yararlıdır. Geçişe başlamadan önce, kaynak veri kümesi 200 MB boyutunda büyüklüğe sahip dosyalara bölümlere kaydedilmelidir.   

2. Toplu yürütme kitaplığı, tek bir istemci VM'de 500.000 RUs tüketmek için ölçeklenebilir. Kullanılabilir iş ortası 5 milyon RUs olduğundan, Azure Cosmos veritabanınızın bulunduğu bölgede 10 Ubuntu 16,04 VM (Standard_D32_v3) sağlanmalıdır. Bu VM'leri geçiş aracı ve ayarları dosyasıyla birlikte hazırlamalısınız.  

3. İstemci sanal makinelerinden birinde sıra adımını çalıştırın. Bu adım, ADLS kapsayıcısını tarayan ve kaynak veri kümesinin bölüm dosyalarının her biri için bir ilerleme izleme belgesi oluşturan izleme koleksiyonu oluşturur.  

4. Ardından, tüm istemci VM'lerde alma adımını çalıştırın. İstemcilerin her biri bir kaynak bölümüne sahip çıkabilir ve verilerini Azure Cosmos DB'ye alabilir. Tamamlandıktan ve durumu izleme koleksiyonunda güncelleştirildikten sonra, istemciler izleme koleksiyonundaki bir sonraki kullanılabilir kaynak bölümü için sorgu layabilir.  

5. Bu işlem, kaynak bölüm kümesi nin tamamı yutulana kadar devam eder. Tüm kaynak bölümleri işlendikten sonra, araç aynı izleme koleksiyonundaki hata düzeltme modunda yeniden çalıştırılmalıdır. Bu adım, hatalar nedeniyle yeniden işlenmesi gereken kaynak bölümleri tanımlamak için gereklidir.  

6. Bu hatalardan bazıları kaynak verilerdeki yanlış belgelerden kaynaklanıyor olabilir. Bunlar tanımlanmalı ve düzeltilmelidir. Ardından, bunları reingest için başarısız bölümlerde alma adımı yeniden gerekir. 

Geçiş tamamlandıktan sonra, Azure Cosmos DB'deki belge sayısının kaynak veritabanındaki belge sayısıyla aynı olduğunu doğrulayabilirsiniz. Bu örnekte, Azure Cosmos DB'deki toplam boyut 65 terabayt'a çıktı. Geçiş sonrası, dizin oluşturma seçici olarak açılabilir ve RUS iş yükünün işlemlerinin gerektirdiği düzeye düşürülebilir.

## <a name="contact-the-azure-cosmos-db-team"></a>Azure Cosmos DB ekibiyle iletişim kurun
Büyük veri kümelerini Azure Cosmos DB'ye başarıyla geçirmek için bu kılavuzu izleyebiliyor olsanız da, büyük ölçekli geçişler için veri modellemesini ve genel bir mimari incelemesini doğrulamak için Azure Cosmos DB ürün ekibine ulaşmanız önerilir. Ürün ekibi, veri kümenize ve iş yükünüzü temel alınca, sizin için geçerli olabilecek diğer performans ve maliyet optimizasyonları da önerebilir. Büyük ölçekli geçişlerle ilgili yardım için Azure Cosmos DB ekibine başvurmak için, aşağıda gösterildiği gibi "Genel Danışma" sorun türü ve "Büyük (TB+) geçişler" alt türü altında bir destek bileti açabilirsiniz.

![Geçiş Destek Konusu](./media/migrate-cosmosdb-data/supporttopic.png)


## <a name="next-steps"></a>Sonraki adımlar

* [.NET](bulk-executor-dot-net.md) ve [Java'daki](bulk-executor-java.md)toplu yürütme kitaplığını tüketen örnek uygulamaları deneyerek daha fazla bilgi edinin. 
* Toplu uygulayıcı kitaplığı, daha fazla bilgi edinmek için Azure [Cosmos DB Spark bağlayıcısı](spark-connector.md) makalesine bakın.  
* Büyük ölçekli geçişlerde ek yardım için "Genel Danışma" sorun türü ve "Büyük (TB+) geçişler" sorun alt türü altında bir destek bileti açarak Azure Cosmos DB ürün ekibine başvurun. 
