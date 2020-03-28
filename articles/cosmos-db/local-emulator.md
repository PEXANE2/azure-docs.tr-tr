---
title: Azure Cosmos Emülatörü ile yerel olarak geliştirin
description: Azure Cosmos Emülatörü'ni kullanarak, Azure aboneliği oluşturmadan uygulamanızı yerel olarak ücretsiz olarak geliştirebilir ve test edebilirsiniz.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 01/31/2020
ms.openlocfilehash: 287933de6403d680c5aa5b6c78df49abe5f2ac56
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238513"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos Emülatörü'ni kullanın

Azure Cosmos Emülatörü, geliştirme amacıyla Azure Cosmos DB hizmetini taklit eden yerel bir ortam sağlar. Azure Cosmos Emülatörü'ni kullanarak, azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamanızı yerel olarak geliştirebilir ve test edebilirsiniz. Uygulamanızın Azure Cosmos Emülatörü'nde çalışma şeklinden memnun olduğunuzda, bulutta bir Azure Cosmos hesabı kullanmaya geçebilirsiniz.

[SQL,](local-emulator.md#sql-api) [Cassandra,](local-emulator.md#cassandra-api) [MongoDB,](local-emulator.md#azure-cosmos-dbs-api-for-mongodb) [Gremlin](local-emulator.md#gremlin-api)ve [Tablo](local-emulator.md#table-api) API hesapları ile Azure Cosmos Emulator kullanarak geliştirebilirsiniz. Ancak şu anda emülatördeki Veri Gezgini görünümü yalnızca SQL API istemcilerini tam olarak destekler. 

## <a name="how-the-emulator-works"></a>Öykünücü nasıl çalışır?

Azure Cosmos Emülatörü, Azure Cosmos DB hizmetinin yüksek doğrulukta öykünmesini sağlar. Veri oluşturma ve sorgulama, kapsayıcısağlama ve ölçekleme desteği ve depolanan yordamları ve tetikleyicileri yürütme de dahil olmak üzere Azure Cosmos DB ile aynı işlevselliği destekler. Azure Cosmos Emülatörü kullanarak uygulamaları geliştirebilir ve sınayabilir ve Azure Cosmos DB'nin bağlantı bitiş noktasında tek bir yapılandırma değişikliği yaparak bunları genel ölçekte Azure'a dağıtabilirsiniz.

Azure Cosmos DB hizmetinin öykünmesi aslına sadık olsa da, öykünücünün uygulaması hizmetten farklıdır. Örneğin öykünücü, kalıcılık için yerel dosya sistemi gibi standart işletim sistemi bileşenlerini ve bağlantı için HTTPS protokolü yığınını kullanır. Genel çoğaltma, okuma/yazma için tek basamaklı milisaniye gecikmesi ve tamıyorum tutarlılık düzeyleri gibi Azure altyapısına dayanan işlevler geçerli değildir.

Azure Cosmos Emülatörü ile Azure Cosmos DB hizmeti arasında [Azure Cosmos DB Veri Geçiş Aracı'nı](https://github.com/azure/azure-documentdb-datamigrationtool)kullanarak verileri aktarabilirsiniz.

Azure Cosmos Emülatör'üni Windows Docker kapsayıcısında çalıştırabilir, docker çekme komutu `Dockerfile` için [Docker Hub'ına](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) ve daha fazla bilgi için [GitHub'a](https://github.com/Azure/azure-cosmos-db-emulator-docker) bakabilirsiniz.

## <a name="differences-between-the-emulator-and-the-service"></a>Öykünücü ile hizmet arasındaki farklar

Azure Cosmos Emülatörü yerel geliştirici iş istasyonunda çalışan taklit edilmiş bir ortam sağladığından, buluttaki emülatör ile Azure Cosmos hesabı arasında bazı işlevsellik farklılıkları vardır:

* Şu anda emülatördeki Veri Gezgini, SQL API istemcilerini destekler. MongoDB, Tablo, Grafik ve Cassandra API'leri gibi Azure Cosmos DB API'lerinin Veri Gezgini görünümü ve işlemleri tam olarak desteklenmez.
* Azure Cosmos Emülatörü yalnızca tek bir sabit hesabı ve tanınmış bir ana anahtarı destekler. Azure Cosmos Emülatörü'nde anahtar rejenerasyonu mümkün değildir, ancak varsayılan anahtar komut satırı seçeneği kullanılarak değiştirilebilir.
* Azure Cosmos Emülatörü ölçeklenebilir bir hizmet değildir ve çok sayıda kapsayıcıyı desteklemez.
* Azure Cosmos Emülatörü farklı [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md)sunmuyor.
* Azure Cosmos Emülatörü [çok bölgeli çoğaltma](distribute-data-globally.md)sunmaz.
* Azure Cosmos Emülatörü kopyanız Azure Cosmos DB hizmetindeki en son değişikliklerle her zaman güncel olmayabilir, uygulamanızın üretim üretim üretim (RUs) gereksinimlerini doğru bir şekilde tahmin etmek için [Azure Cosmos DB kapasite planlayıcısına](https://www.documentdb.com/capacityplanner) başvurmanız gerekir.
* Azure Cosmos Emülatörü kullanırken varsayılan olarak, en fazla 25 sabit boyutlu kapsayıcı (yalnızca Azure Cosmos DB SDK'lar kullanılarak desteklenen) veya Azure Cosmos Emülatörü kullanarak 5 sınırsız kapsayıcı oluşturabilirsiniz. Bu değeri değiştirme hakkında daha fazla bilgi için bkz. [PartitionCount değerini ayarlama](#set-partitioncount).

## <a name="system-requirements"></a>Sistem gereksinimleri

Azure Cosmos Emülatörü aşağıdaki donanım ve yazılım gereksinimlerine sahiptir:

* Yazılım gereksinimleri
  * Windows Server 2012 R2, Windows Server 2016 veya Windows 10
  * 64 bit işletim sistemi
* Minimum Donanım gereksinimleri
  * 2-GB RAM
  * 10 GB kullanılabilir sabit disk alanı

## <a name="installation"></a>Yükleme

Azure Cosmos Emülatör'ü [Microsoft Download Center'dan](https://aka.ms/cosmosdb-emulator) indirip yükleyebilir veya Emülatör'ü Windows için Docker'da çalıştırabilirsiniz. Windows için Docker'daki emülatörün kullanımıyla ilgili talimatlar için [Docker'da Çalıştırma'ya](#running-on-docker)bakın.

> [!NOTE]
> Azure Cosmos Emülatör'üne yüklemek, yapılandırmak ve çalıştırmak için bilgisayarda yönetim ayrıcalıklarına sahip olmalısınız. Emülatör bir sertifika oluşturur/ekler ve hizmetlerini çalıştırmak için güvenlik duvarı kurallarını da ayarlar; bu nedenle emülatörün bu tür işlemleri yürütebilmesi gerekir.

## <a name="running-on-windows"></a>Windows’da çalıştırma

Azure Cosmos Emülatörü'nü başlatmak için Başlat düğmesini seçin veya Windows tuşuna basın. Azure **Cosmos Emülatörü**yazmaya başlayın ve uygulamalar listesinden emülatörünü seçin.

![Başlat düğmesini seçin veya Windows tuşuna basın,**Azure Cosmos Emülatörü** yazmaya başlayın ve uygulamalar listesinden emülatörünü seçin](./media/local-emulator/database-local-emulator-start.png)

Öykünücü çalıştırıldığında, Windows görev çubuğu bildirim alanında bir simge görürsünüz. ![Azure Cosmos DB yerel emülatör görev çubuğu bildirimi](./media/local-emulator/database-local-emulator-taskbar.png)

Azure Cosmos Emülatörü varsayılan olarak 8081 portunda dinleyen yerel makinede ("localhost") çalışır.

Azure Cosmos Emülatörü varsayılan `C:\Program Files\Azure Cosmos DB Emulator` olarak yüklenir. Komut satırından da öykünücüyü başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için bkz. [komut satırı aracı başvurusu](#command-line).

## <a name="start-data-explorer"></a>Veri Gezgini’ni Başlat

Azure Cosmos Emülatörü kullanıma açıldığında, tarayıcınızda Azure Cosmos Veri Gezgini'ni otomatik olarak açar. Adres, `https://localhost:8081/_explorer/index.html` olarak görüntülenir. Gezgini kapatır ve daha sonra yeniden açmak isterseniz, URL'yi tarayıcınızda açabilir veya aşağıda gösterildiği gibi Windows Tepsi Simgesi'ndeki Azure Cosmos Emülatörü'nden başlatabilirsiniz.

![Azure Cosmos yerel emülatör veri gezgini başlatıcısı](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Güncelleştirmeleri denetleme

Veri Gezgini, indirilebilir yeni bir güncelleştirme olup olmadığını belirtir.

> [!NOTE]
> Azure Cosmos Emülatörü'nün bir sürümünde oluşturulan verilerin (bkz. %LOCALAPPDATA%\CosmosDBEmulator veya veri yolu isteğe bağlı ayarları) farklı bir sürüm kullanırken erişilebilir olması garanti edilmez. Verilerinizi uzun süre bekletmeniz gerekiyorsa, bu verileri Azure Cosmos Emülatörü yerine bir Azure Cosmos hesabında depolamanız önerilir.

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama

Buluttaki Azure Cosmos DB'de olduğu gibi, Azure Cosmos Emülatör'e karşı yaptığınız her istek kimlik doğrulaması yapılmalıdır. Azure Cosmos Emülatörü, tek bir sabit hesabı ve ana anahtar kimlik doğrulaması için iyi bilinen bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, Azure Cosmos Emülatörü ile kullanılmasına izin verilen tek kimlik bilgileridir. Bunlar:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Azure Cosmos Emülatörü tarafından desteklenen ana anahtar yalnızca emülatörle kullanılmak üzere tasarlanmıştır. Azure Cosmos DB hesabınızı ve anahtarınızı Azure Cosmos Emülatörü ile kullanamazsınız.

> [!NOTE]
> Emülatöre /Key seçeneğiyle başladıysanız, oluşturulan tuşu 'nun yerine `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`'yi kullanın. /Key seçeneği hakkında daha fazla bilgi için [Komut satırı araç başvurusuna bakın.](#command-line)

Azure Cosmos DB'de olduğu gibi Azure Cosmos Emülatörü de yalnızca SSL üzerinden güvenli iletişimi destekler.

## <a name="running-on-a-local-network"></a>Yerel ağ üzerinde çalışma

Yerel bir ağ üzerinde öykünücüyü çalıştırabilirsiniz. Ağ erişimini etkinleştirmek `/AllowNetworkAccess` için, [komut satırındaki](#command-line-syntax)seçeneği belirtin `/Key=key_string` , `/KeyFile=file_name`bu seçeneği belirtmenizi de gerektirir veya . Rasgele bir `/GenKeyFile=file_name` anahtar ön bir dosya oluşturmak için kullanabilirsiniz. O zaman bunu. `/KeyFile=file_name` `/Key=contents_of_file`

Ağ erişimini ilk kez etkinleştirmek için kullanıcı emülatörünü kapatmalı ve emülatörün veri dizini silmelidir (%LOCALAPPDATA%\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Öykünücü ile geliştirme

### <a name="sql-api"></a>SQL API’si

Masaüstünüzde Azure Cosmos Emülatör çalıştırıldıktan sonra, emülatörle etkileşimde kalmak için desteklenen [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) veya [Azure Cosmos DB REST API'sini](/rest/api/cosmos-db/) kullanabilirsiniz. Azure Cosmos Emülatörü ayrıca Mongo DB API için SQL API veya Cosmos DB için kapsayıcılar oluşturmanıza ve öğeleri kod yazmadan görüntülemenize ve görüntülemenize olanak tanıyan yerleşik bir Veri Gezgini içerir.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

Masaüstünüzde Azure Cosmos Emülatör çalıştırdıktan sonra, emülatörle etkileşimde kalmak [için Azure Cosmos DB'nin MongoDB apisini](mongodb-introduction.md) kullanabilirsiniz. Emülatör'ü komut isteminden "/EnableMongoDbEndpoint" ile yönetici olarak başlatın. Ardından MongoDB API hesabına bağlanmak için aşağıdaki bağlantı dizesini kullanın:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>Tablo API’si

Masaüstünüzde Azure Cosmos Emülatör çalıştırıldıktan sonra, emülatörle etkileşimde kalmak için [Azure Cosmos DB Table API SDK'yı](table-storage-how-to-use-dotnet.md) kullanabilirsiniz. Emülatöre komut isteminden "/EnableTableEndpoint" ile bir yönetici olarak başlayın. Sonraki tablo API hesabına bağlanmak için aşağıdaki kodu çalıştırın:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Cassandra API’si

Emülatöre bir yönetici komut isteminden "/EnableCassandraEndpoint" ile başlayın. Alternatif olarak, ortam değişkenini `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`de ayarlayabilirsiniz.

* [Python 2.7'yi yükle](https://www.python.org/downloads/release/python-2716/)

* [Cassandra CLI/CQLSH'ı yükleyin](https://cassandra.apache.org/download/)

* Aşağıdaki komutları normal komut istemi penceresinde çalıştırın:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* CQLSH kabuğunda, Cassandra bitiş noktasına bağlanmak için aşağıdaki komutları çalıştırın:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>Gremlin API

Emülatöre bir yönetici komut isteminden "/EnableGremlinEndpoint" ile başlayın. Alternatif olarak, ortam değişkenini de ayarlayabilirsiniz`AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

* [Apache-tinkerpop-gremlin-konsol-3.3.4 yükleyin.](https://archive.apache.org/dist/tinkerpop/3.3.4)

* Emülatör'ün Veri Gezgini'nde "db1" veritabanı ve "coll1" koleksiyonu oluşturulur; bölüm anahtarı için "/name" seçeneğini belirleyin

* Aşağıdaki komutları normal komut istemi penceresinde çalıştırın:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* Gremlin kabuğunda Gremlin uç noktasına bağlanmak için aşağıdaki komutları çalıştırın:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-ssl-certificate"></a>SSL sertifikasını dışarı aktarma

.NET dilleri ve çalışma zamanı, Azure Cosmos DB yerel öykünücüsüne güvenli şekilde bağlanmak için Windows Sertifika Deposunu kullanır. Diğer dillerin kendi sertifikaları yönetme ve kullanma yöntemi vardır. Java kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanırken Python ise [yuva sarmalayıcılarını](https://docs.python.org/2/library/ssl.html) kullanır.

Windows Sertifika Deposu ile tümleştirilmeyen çalışma zamanları ve dillerle kullanmak üzere bir sertifika edinmek için Windows Sertifika Yöneticisi’ni kullanarak bunu dışarı aktarmanız gerekir. Certlm.msc çalıştırarak başlatabilir veya [Azure Kozmos Emülatör Sertifikalarını Dışa Aktar'da](./local-emulator-export-ssl-certificates.md)adım adım yönergeleri takip edebilirsiniz. Sertifika yöneticisi çalıştırıldıktan sonra aşağıda gösterildiği gibi Kişisel Sertifikaları açın ve sertifikayı BASE-64 kodlu X.509 (.cer) dosyası olarak "DocumentDBEmulatorCertificate" kolay adıyla dışarı aktarın.

![Azure Cosmos DB yerel öykünücüsü SSL sertifikası](./media/local-emulator/database-local-emulator-ssl_certificate.png)

X.509 sertifikası, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeler izlenerek Java sertifika deposuna içeri aktarılabilir. Sertifika sertifika deposuna alındıktan sonra, SQL ve Azure Cosmos DB'nin MongoDB için API'si için istemciler Azure Cosmos Emülatörü'ne bağlanabilecektir.

Python ve Node.js SDK’larından öykünücüye bağlanırken SSL doğrulaması devre dışı bırakılır.

## <a name="command-line-tool-reference"></a><a id="command-line"></a>Komut satırı aracı başvurusu
Yükleme konumundan, emülatörbaşlatmak ve durdurmak, seçenekleri yapılandırmak ve diğer işlemleri gerçekleştirmek için komut satırı kullanabilirsiniz.

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

    Microsoft.Azure.Cosmos.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]

Seçenek listesini görüntülemek için komut satırına `Microsoft.Azure.Cosmos.Emulator.exe /?` yazın.

|**Seçeneği** | **Açıklama** | **Komut**| **Bağımsız Değişkenler**|
|---|---|---|---|
|[Bağımsız değişken yok] | Varsayılan ayarlarla Azure Cosmos Emülatörü'nu başlatır. |Microsoft.Azure.Cosmos.Emulator.exe| |
|[Yardım] |Desteklenen komut satırı bağımsız değişkenleri listesini görüntüler.|Microsoft.Azure.Cosmos.Emulator.exe /? | |
| GetStatus |Azure Cosmos Emülatörü'nün durumunu alır. Durum, çıkış kodu tarafından belirtilir: 1 = Başlatılıyor, 2 = Çalıştırılıyor, 3 = Durduruldu. Negatif çıkış kodu, bir hata oluştuğunu gösterir. Başka bir çıktı üretilmez. | Microsoft.Azure.Cosmos.Emulator.exe /GetStatus| |
| Kapat| Azure Cosmos Emülatörü'nu kapatır.| Microsoft.Azure.Cosmos.Emulator.exe /Kapatma | |
|DataPath | Veri dosyalarının depolanacağı yolu belirtir. Varsayılan değer %LocalAppdata%\CosmosDBEmulator'dur. | Microsoft.Azure.Cosmos.Emulator.exe /DataPath=\<datapath\> | \<datapath\>: Erişilebilir bir yol |
|Bağlantı noktası | Öykünücü için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan değer 8081'dir. |Microsoft.Azure.Cosmos.Emulator.exe /Port=\<bağlantı noktası\> | \<port\>: Tek bir bağlantı noktası numarası |
| ComputePort | İşlem Interop Ağ Geçidi hizmeti için kullanılacak bağlantı noktası numarası belirtilmiş. Ağ Geçidi'nin HTTP uç nokta sondası bağlantı noktası ComputePort + 79 olarak hesaplanır. Bu nedenle, ComputePort ve ComputePort + 79 açık ve kullanılabilir olmalıdır. Varsayılan değer 8900'dür. | Microsoft.Azure.Cosmos.Emulator.exe /ComputePort=\<computeport\> | \<computeport\>: Tek bağlantı noktası numarası |
| EnableMongoDbEndpoint=3.2 | MongoDB API 3.2 sağlar | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.2 | |
| EnableMongoDbEndpoint=3.6 | MongoDB API 3.6 sağlar | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.6 | |
| MongoPort | MongoDB uyumluluk API’si için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan değer 10255'tir. |Microsoft.Azure.Cosmos.Emulator.exe /MongoPort=\<mongoport\>|\<mongoport\>: Tek bir bağlantı noktası numarası|
| EtkinleştirCassandraEndpoint | Cassandra API sağlar | Microsoft.Azure.Cosmos.Emulator.exe /EnableCassandraEndpoint | |
| CassandraPort | Cassandra bitiş noktası için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan değer 10350'dir. | Microsoft.Azure.Cosmos.Emulator.exe /CassandraPort=\<cassandraport\> | \<cassandraport\>: Tek bağlantı noktası numarası |
| EtkinleştirinGremlinEndpoint | Gremlin API sağlar | Microsoft.Azure.Cosmos.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Gremlin Endpoint için kullanılacak bağlantı noktası numarası. Varsayılan değer 8901'dir. | Microsoft.Azure.Cosmos.Emulator.exe /GremlinPort=\<bağlantı noktası\> | \<port\>: Tek bir bağlantı noktası numarası |
|TableEndpoint'i etkinleştir | Azure Tablo API'sini etkinleştirir | Microsoft.Azure.Cosmos.Emulator.exe /EnableTableEndpoint | |
|TablePort | Azure Tablo Bitiş Noktası için kullanılacak bağlantı noktası numarası. Varsayılan değer 8902'dir. | Microsoft.Azure.Cosmos.Emulator.exe /TablePort=\<bağlantı noktası\> | \<port\>: Tek bir bağlantı noktası numarası|
| Keyfile | Belirtilen dosyadan yetkilendirme anahtarını okuyun. Anahtar dosyası oluşturmak için /GenKeyFile seçeneğini kullanma | Microsoft.Azure.Cosmos.Emulator.exe /KeyFile=\<file_name\> | \<file_name\>: Dosyaya giden yol |
| ResetDataPath | Yinelemeli olarak belirtilen yoldaki tüm dosyaları kaldırır. Bir yol belirtmezseniz, varsayılan olarak %LOCALAPPDATA%\CosmosDbEmulator | Microsoft.Azure.Cosmos.Emulator.exe /ResetDataPath=\<yol> | \<path\>: Dosya yolu  |
| Başlangıç İzleme Ler  |  LOGMAN'ı kullanarak hata ayıklama günlüklerini toplamaya başlayın. | Microsoft.Azure.Cosmos.Emulator.exe /StartTraces | |
| StopTraces     | LOGMAN kullanarak hata ayıklama izleme günlüklerini toplamayı durdurun. | Microsoft.Azure.Cosmos.Emulator.exe /StopTraces  | |
| BaşlangıçWprTraces  |  Windows Performans Kaydı aracını kullanarak hata ayıklama izleme günlükleri toplamaya başlayın. | Microsoft.Azure.Cosmos.Emulator.exe /StartWprTraces | |
| StopWprTraces     | Windows Performans Kaydı aracını kullanarak hata ayıklama izleme günlüklerini toplamayı durdurun. | Microsoft.Azure.Cosmos.Emulator.exe /StopWprTraces  | |
|FailOnSslCertificateNameMismatch | Varsayılan olarak Emülatör, sertifikanın SAN'ı Emülatör'ün etki alanı adını, yerel IPv4 adresini, 'localhost'u ve '127.0.0.1'i içermiyorsa, kendi imzaladığı SSL sertifikasını yeniler. Bu seçenekle, emülatör başlangıçta başarısız olur. Daha sonra /GenCert seçeneğini kullanarak yeni bir kendi imzalı SSL sertifikası oluşturup yüklemeniz gerekir. | Microsoft.Azure.Cosmos.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Kendi imzalı yeni bir SSL sertifikası oluşturun ve yükleyin. ağ üzerinden Emülatöre erişmek için virgülden ayrılmış ek DNS adlarının listesini içeren isteğe bağlı olarak. | Microsoft.Azure.Cosmos.Emulator.exe /GenCert=\<dns-names\> |\<dns-names\>: İsteğe bağlı virgülayrılmış ek dns adları listesi  |
| DirectPorts |Doğrudan bağlantı için kullanılacak bağlantı noktalarını belirtir. Varsayılan değerler: 10251,10252,10253,10254. | Microsoft.Azure.Cosmos.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: 4 bağlantı noktasından oluşan virgülle ayrılmış liste |
| Anahtar |Öykünücü için yetkilendirme anahtarı. Anahtar, 64 bayt vektörün base 64 kodlaması olmalıdır. | Microsoft.Azure.Cosmos.Emulator.exe /Key:\<anahtar\> | \<key\>: Anahtar, 64 bayt vektörün base 64 kodlaması olmalıdır|
| EnableRateLimiting | İstek oranını sınırlama davranışının etkinleştirildiğini belirtir. |Microsoft.Azure.Cosmos.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |İstek oranını sınırlama davranışının devre dışı bırakıldığını belirtir. |Microsoft.Azure.Cosmos.Emulator.exe /DisableRateLimiting | |
| NoUI | Öykünücü kullanıcı arabirimini gösterme. | Microsoft.Azure.Cosmos.Emulator.exe /NoUI | |
| NoExplorer | Başlangıçta veri gezginini gösterme. |Microsoft.Azure.Cosmos.Emulator.exe /NoExplorer | | 
| PartitionCount | En fazla bölümlenmiş kapsayıcı sayısını belirtir. Bkz. Daha fazla bilgi için [kapsayıcı sayısını değiştirin.](#set-partitioncount) | Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=\<bölüm sayısı\> | \<partitioncount\>: İzin verilen tek bölme kapsayıcılarının maksimum sayısı. Varsayılan değer 25’tir. Maksimum izin verilen: 250.|
| DefaultPartitionCount| Bölümlenmiş bir kapsayıcı için varsayılan bölüm sayısını belirtir. | Microsoft.Azure.Cosmos.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> Varsayılan değeri 25'tir.|
| AllowNetworkAccess | Bir ağ üzerinden öykünücüye erişilmesini sağlar. Ağ erişimini etkinleştirmek için /Key=\<key_string\> veya /KeyFile=\<file_name\> öğesini de geçirmeniz gerekir. | Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> veya Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | /AllowNetworkAccess seçeneği kullanıldığında güvenlik duvarı kurallarını ayarlamayın. |Microsoft.Azure.Cosmos.Emulator.exe /NoFirewall | |
| GenKeyFile | Yeni bir yetkilendirme anahtarı oluşturun ve belirtilen dosyaya kaydedin. Oluşturulan anahtar, /Key veya /KeyFile seçenekleri ile kullanılabilir. | Microsoft.Azure.Cosmos.Emulator.exe /GenKeyFile=\<anahtar dosyasına giden yol\> | |
| Tutarlılık | Hesap için varsayılan tutarlılık düzeyini ayarlayın. | Microsoft.Azure.Cosmos.Emulator.exe /Tutarlılık=\<tutarlılık\> | \<consistency\>: Değer, şu [tutarlılık düzeylerinden](consistency-levels.md) biri olmalıdır: Oturum, Güçlü, Son veya Sınırlanmış Eskime Durumu. Varsayılan değer: Oturum. |
| ? | Yardım iletisini gösterin.| | |

## <a name="change-the-number-of-containers"></a><a id="set-partitioncount"></a>Kapsayıcı sayısını değiştirme

Varsayılan olarak, en fazla 25 sabit boyutlu kapsayıcı (yalnızca Azure Cosmos DB SDK'lar kullanılarak desteklenen) veya Azure Cosmos Emülatörü kullanarak 5 sınırsız kapsayıcı oluşturabilirsiniz. **PartitionCount** değerini değiştirerek, en fazla 250 sabit boyutlu kapsayıcı veya 50 sınırsız kapsayıcı veya 250 sabit boyutlu kapsayıcıyı aşmayan iki kapsayıcının (bir sınırsız kapsayıcı = 5 sabit boyutlu kapsayıcı) herhangi bir birleşimini oluşturabilirsiniz. Ancak, emülatörün 200'den fazla sabit boyutlu kapsayıcıyla çalışacak şekilde ayarlanmaları önerilmez. Disk IO işlemlerine eklediği ek yükü nedeniyle, uç nokta API'leri kullanırken öngörülemeyen zaman aşımlarına neden olur.

Geçerli bölüm sayısı aşıldıktan sonra bir kapsayıcı oluşturmaya çalışırsanız, emülatör aşağıdaki iletiyle birlikte bir ServiceUnavailable özel durum atar.

"Üzgünüz, şu anda bu bölgede yüksek talep yaşıyoruz ve şu anda isteğinizi yerine getiremiyoruz. Çevrimiçi daha fazla kapasite getirmek için sürekli çalışıyoruz ve sizi yeniden denemeniz için teşvik ediyoruz.
ActivityId: 12345678-1234-1234-1234-123456789abc"

Azure Cosmos Emülatörü'nde bulunan kapsayıcı sayısını değiştirmek için aşağıdaki adımları izleyin:

1. Sistem tepsisindeki **Azure Cosmos DB Emulator** simgesine sağ tıklayarak ve ardından **Verileri Sıfırla'yı**tıklatarak tüm yerel Azure Cosmos Emülatör verilerini silin...
2. Bu klasördeki `%LOCALAPPDATA%\CosmosDBEmulator`tüm emülatör verilerini silin.
3. Sistem tepsisindeki **Azure Cosmos DB Öykünücüsü** simgesine sağ tıklayıp **Çıkış**’a tıklayarak tüm açık örneklerden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.
4. [Azure Cosmos Emülatörü'nün](https://aka.ms/cosmosdb-emulator)en son sürümünü yükleyin.
5. 250 veya daha düşük bir değer ayarlayarak PartitionCount bayrağı ile öykünücüyü başlatın. Örneğin: `C:\Program Files\Azure Cosmos DB Emulator> Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Öykünücüyü denetleme

Emülatör, hizmetin durumunu başlatmak, durdurmak, kaldırmak ve almak için bir PowerShell modülüyle birlikte gelir. PowerShell modüllerini kullanmak için aşağıdaki cmdlet'i çalıştırın:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

veya dizini `PSModules` sizin `PSModulesPath` üzerine yerleştirin ve aşağıdaki komutta gösterildiği gibi içe aktarın:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Aşağıda, PowerShell’den öykünücüyü denetlemeye ilişkin komutların özeti verilmiştir:

### `Get-CosmosDbEmulatorStatus`

**Sözdizimi**

`Get-CosmosDbEmulatorStatus`

**Açıklamalar**

Şu ServiceControllerStatus değerlerinden birini döndürür: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running veya ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Sözdizimi**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Açıklamalar**

Öykünücüyü başlatır. Varsayılan olarak komut, öykünücünün istekleri kabul etmeye hazır olmasını bekler. Cmdlet’in öykünücüyü başlattığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Stop-CosmosDbEmulator`

**Sözdizimi**

 `Stop-CosmosDbEmulator [-NoWait]`

**Açıklamalar**

Öykünücüyü durdurur. Varsayılan olarak bu komut, öykünücünün tamamen kapanmasını bekler. Cmdlet’in öykünücü kapanmaya başladığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Uninstall-CosmosDbEmulator`

**Sözdizimi**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Açıklamalar**

Öykünücüyü kaldırır ve isteğe bağlı olarak $env:LOCALAPPDATA\CosmosDbEmulator içeriklerinin tamamını kaldırır.
Cmdlet, öykünücüyü kaldırmadan önce öykünücünün tamamen durdurulduğundan emin olur.

## <a name="running-on-docker"></a>Docker’da çalıştırma

Azure Cosmos Emülatörü Windows için Docker'da çalıştırılabilir. Öykünücü Docker for Oracle Linux üzerinde çalışmaz.

[Docker for Windows](https://www.docker.com/docker-windows) yüklendikten sonra, araç çubuğundaki Docker simgesine sağ tıklayıp **Windows kapsayıcılarına geç** seçeneğini belirleyerek Windows kapsayıcılarına geçin.

Daha sonra sık kullandığınız kabuktan aşağıdaki komutu çalıştırarak Docker Hub'dan Öykünücü görüntüsünü çekin.

```bash
docker pull mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```
Görüntüyü başlatmak için aşağıdaki komutları çalıştırın.

Komut satırından:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```

> [!NOTE]
> Docker run komutunu çalıştırdığınızda bir bağlantı noktası çakışması hatası (belirtilen bağlantı noktası zaten kullanılıyor) görürseniz, bağlantı noktası numaralarını değiştirerek özel bir bağlantı noktasını geçebilirsiniz. Örneğin, "-p 8081:8081" ile "-p 443:8081" olarak değiştirebilirsiniz.

PowerShell’den:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator

```

Yanıt şuna benzer:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Şimdi istemcinizdeki yanıttan uç noktayı ve ana anahtar girişini kullanın ve SSL sertifikasını ana bilgisayarınıza içeri aktarın. SSL sertifikasını içeri aktarmak için, yönetici komut isteminden aşağıdakileri yapın:

Komut satırından:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

PowerShell’den:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Öykünücü başlatıldıktan sonra etkileşimli kabuğu kapatmak öykünücünün kapsayıcısını kapatır.

Veri Gezgini’ni açmak için tarayıcınızda aşağıdaki URL’ye gidin. Yukarıda gösterilen yanıt iletisinde öykünücü uç noktası sağlanır.

    https://<emulator endpoint provided in response>/_explorer/index.html

Bir Linux docker kapsayıcısı üzerinde çalışan bir .NET istemci uygulamanız varsa ve bir ana makinede Azure Cosmos emülatörü çalıştırıyorsanız, sertifikayı Linux docker konteynerine aktarması için lütfen aşağıdaki bölümü izleyin.

## <a name="running-on-mac-or-linux"></a>Mac veya Linux'ta çalıştırma<a id="mac"></a>

Şu anda Cosmos emülatörü yalnızca Windows'da çalıştırılabilir. Mac veya Linux çalıştıran kullanıcılar, Paraleller veya VirtualBox gibi bir hipervizörü barındıran bir Windows sanal makinesinde emülatör çalıştırabilir. Bunu etkinleştirmek için aşağıdaki adımlar verilmiştir.

Windows VM içinde aşağıdaki komutu çalıştırın ve IPv4 adresi not olun.

```cmd
ipconfig.exe
```

Uygulamanızda, uç nokta olarak kullanılan URI'yi değiştirmek yerine döndürülen `ipconfig.exe` IPv4 adresini kullanmanız `localhost`gerekir.

Windows VM içinden bir sonraki adım, aşağıdaki seçenekleri kullanarak komut satırından Cosmos emülatörü başlatın.

```cmd
Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

Son olarak, Emülatör CA sertifikasını Linux veya Mac ortamına aktarmamız gerekir.

### <a name="linux"></a>Linux

Linux üzerinde çalışıyorsanız, .NET doğrulamayı yapmak için OpenSSL'de geçer:

1. [Sertifikayı PFX biçiminde dışa aktarın](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-ssl-certificate) (özel anahtarı dışa aktarmayı seçerken PFX kullanılabilir). 

1. PfX dosyasını Linux ortamınıza kopyalayın.

1. PFX dosyasını CRT dosyasına dönüştürme

   ```bash
   openssl pkcs12 -in YourPFX.pfx -clcerts -nokeys -out YourCTR.crt
   ```

1. CRT dosyasını Linux dağıtımınızda özel sertifikalar içeren klasöre kopyalayın. Yaygın Olarak Debian dağılımları üzerinde `/usr/local/share/ca-certificates/`yer almaktadır.

   ```bash
   cp YourCTR.crt /usr/local/share/ca-certificates/
   ```

1. Klasörü güncelleştirecek CA sertifikalarını güncelleştirin. `/etc/ssl/certs/`

   ```bash
   update-ca-certificates
   ```

### <a name="mac-os"></a>Mac OS

Mac üzerinde çalışıyorsanız aşağıdaki adımları kullanın:

1. [Sertifikayı PFX biçiminde dışa aktarın](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-ssl-certificate) (özel anahtarı dışa aktarmayı seçerken PFX kullanılabilir).

1. PfX dosyasını Mac ortamınıza kopyalayın.

1. *Anahtarlık Erişimi* uygulamasını açın ve PFX dosyasını içe aktarın.

1. Sertifikalistesini açın ve adı `localhost`olan olanı tanımlayın.

1. Söz konusu öğeiçin bağlam menüsünü açın, *Öğe Al'ı* ve *Güven* > altında bu sertifika seçeneğini*kullanırken* *Her Zaman Güven'i*seçin. 

   ![Söz konusu öğeiçin bağlam menüsünü açın, Öğe Al'ı ve Güven altında 'nu seçin - Bu sertifika seçeneğini kullanırken Her Zaman Güven'i seçin](./media/local-emulator/mac-trust-certificate.png)

Bu adımları izledikten sonra, ortamınız IP adresine bağlanırken Emülatör tarafından `/AllowNetworkAccess`kullanılan sertifikaya güvenecektir.

## <a name="troubleshooting"></a>Sorun giderme

Azure Cosmos Emülatörü ile karşılaştığınız sorunları gidermek için aşağıdaki ipuçlarını kullanın:

- Öykünücünün yeni bir sürümünü yüklediyseniz ve hatalarla karşılaşıyorsanız, verilerinizi sıfırladığınızdan emin olun. Sistem tepsisindeki Azure Cosmos Emülatör simgesine sağ tıklayarak ve ardından Verileri Sıfırla'yı tıklatarak verilerinizi sıfırlayabilirsiniz.... Bu hataları düzeltmezse, bulunursa emülatörün ve emülatörün eski sürümlerini kaldırabilir, "C:\Program dosyaları\Azure Cosmos DB Emulator" dizinini kaldırAbilir ve emülatörü yeniden yükleyebilirsiniz. Yönergeler için bkz. [Yerel öykünücüden kaldırma](#uninstall).

- Azure Cosmos Emülatörü çökerse, '%LOCALAPPDATA%\CrashDumps' klasöründen döküm dosyaları toplayın, sıkıştırın ve [Azure portalından](https://portal.azure.com)bir destek bileti açın.

- Çökmeler `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`yaşarsanız, bu Performans Sayaçları bozuk durumda bir belirti olabilir. Genellikle bir yönetici komut istemi aşağıdaki komutu çalıştırarak sorunu giderir:

  ```cmd
  lodctr /R
   ```

- Bir bağlantı sorunuyla karşılaşırsanız, [izleme dosyalarını toplayın,](#trace-files)sıkıştırın ve [Azure portalında](https://portal.azure.com)bir destek bileti açın.

- **Hizmet Kullanılamıyor** iletisi alırsanız öykünücü, ağ yığınını başlatamıyor olabilir. Pulse secure istemcisinin veya Juniper networks istemcisinin yüklü olup olmadığını denetleyin; bunların ağ filtresi sürücüleri soruna yol açıyor olabilir. Genellikle üçüncü taraf ağ filtresi sürücüleri kaldırıldığında sorun düzeltilir. Alternatif olarak, emülatör ağ iletişimini normal Winsock'a çevirecek olan /DisableRIO ile başlatın. 

- Öykünücü çalışırken bilgisayarınız uyku moduna geçer veya herhangi bir işletim sistemi güncelleştirmesi çalıştırırsa, bir **Service is currently unavailable** (Hizmet şu anda kullanılamıyor) iletisi alabilirsiniz. Windows bildirim tepsisinde görünen simgeye sağ tıklayarak emülatörün verilerini sıfırlayın ve **Verileri Sıfırla'yı**seçin.

### <a name="collect-trace-files"></a><a id="trace-files"></a>İzleme dosyalarını toplama

Hata ayıklama izlemelerini toplamak için bir yönetici komut isteminden aşağıdaki komutları çalıştırın:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `Microsoft.Azure.Cosmos.Emulator.exe /shutdown`. Programın kapatıldığından emin olmak için sistem tepsisine bakın; bu bir dakika sürebilir. Azure Cosmos Emülatör kullanıcı arabiriminde **Çık'ı** da tıklatabilirsiniz.
3. `Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces`
4. `Microsoft.Azure.Cosmos.Emulator.exe`
5. Sorunu yeniden oluşturun. Veri Gezgini çalışmıyorsa yalnızca hatayı yakalamak için tarayıcının birkaç saniye boyunca açılmasını beklemeniz gerekir.
6. `Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces`
7. `%ProgramFiles%\Azure Cosmos DB Emulator` konumuna gidin ve docdbemulator_000001.etl dosyasını bulun.
8. [Azure portalında](https://portal.azure.com) bir destek bileti açın ve .etl dosyasını repro adımları ile birlikte ekleyin.

### <a name="uninstall-the-local-emulator"></a><a id="uninstall"></a>Yerel emülatörün kaldırma

1. Sistem tepsisindeki Azure Cosmos Emülatör simgesine sağ tıklayarak ve sonra Çık'ı tıklatarak yerel emülatörün tüm açık örneklerinden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.
2. Windows arama kutusuna **Uygulamalar ve özellikler** yazın ve **Uygulamalar ve özellikler (Sistem ayarları)** sonucuna tıklayın.
3. Uygulamalar listesinde **Azure Cosmos DB Öykünücüsü**’ne gidip bunu seçin, **Kaldır**’a tıklayın, daha sonra onaylayıp yeniden **Kaldır**’a tıklayın.
4. Uygulama kaldırıldığında `%LOCALAPPDATA%\CosmosDBEmulator` klasörüne gidip klasörü silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide ücretsiz yerel geliştirme için yerel öykünücünün nasıl kullanılacağını öğrendiniz. Artık sonraki öğreticiye devam edebilir ve öykünücü SSL sertifikalarının nasıl dışarı aktarılacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos Emülatör sertifikalarını dışa aktarma](local-emulator-export-ssl-certificates.md)
