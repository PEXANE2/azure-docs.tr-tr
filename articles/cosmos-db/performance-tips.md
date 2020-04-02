---
title: .NET için Azure Cosmos DB performans ipuçları
description: Azure Cosmos DB performansını artırmak için istemci yapılandırma seçeneklerini öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: sngun
ms.openlocfilehash: ee2743af2f8499aec04d8b6b733e1ba4c2a82083
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546071"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Azure Cosmos DB ve .NET için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB, garantili gecikme sonu ve iş ortası ile sorunsuz ölçeklendirilebilen hızlı ve esnek dağıtılmış bir veritabanıdır. Veritabanınızı Azure Cosmos DB ile ölçeklendirmek için büyük mimari değişiklikleri yapmak veya karmaşık kod yazmak zorunda değilsiniz. Yukarı ve aşağı ölçekleme tek bir API araması yapmak kadar kolaydır. Daha fazla bilgi edinmek [için, kapsayıcı iş verisini nasıl sağtayabildiğini](how-to-provision-container-throughput.md) veya [veritabanı iş verisini nasıl sağlarınızı](how-to-provision-database-throughput.md)görün. Ancak Azure Cosmos DB'ye ağ aramaları üzerinden erişildiğinden, [SQL .NET SDK'yı](sql-api-sdk-dotnet-standard.md)kullandığınızda en yüksek performansı elde etmek için yapabileceğiniz istemci tarafı optimizasyonları vardır.

Bu nedenle, veritabanı performansınızı artırmaya çalışıyorsanız, şu seçenekleri göz önünde bulundurun:

## <a name="hosting-recommendations"></a>Hosting önerileri

**Sorgu yoğun iş yükleri için Linux veya Windows 32 bit ana bilgisayar işleme yerine Windows 64-bit'i kullanın**

Gelişmiş performans için Windows 64 bit ana bilgisayar işlemeyi öneririz. SQL SDK, sorguları yerel olarak ayrışdırmak ve optimize etmek için yerel serviceInterop.dll içerir. ServiceInterop.dll yalnızca Windows x64 platformunda desteklenir. Linux ve ServiceInterop.dll'nin kullanılamadığı diğer desteklenmeyen platformlar için, optimize edilmiş sorguyu almak için ağ geçidine ek bir ağ çağrısı yapılır. Aşağıdaki uygulama türleri varsayılan olarak 32 bit ana bilgisayar işleme kullanır. Ana bilgisayar işlemeyi 64 bit işlemeye değiştirmek için, uygulamanızın türüne bağlı olarak aşağıdaki adımları izleyin:

- Çalıştırılabilir uygulamalar için, **Yapı** sekmesinde **Proje Özellikleri** penceresinde ki [platform hedefini](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) **x64** olarak ayarlayarak ana bilgisayar işlemeyi değiştirebilirsiniz.

- VSTest tabanlı test projeleri için, Visual Studio **Test** menüsünde **Test** > **Test Ayarları** > Varsayılan**İşlemci Mimarisi'ni X64 olarak** seçerek ana bilgisayar işlemini değiştirebilirsiniz.

- Yerel olarak dağıtılan ASP.NET web uygulamaları için, **Araçlar** > **Seçenekleri** > **Projeleri ve Çözümleri** > **Web Projeleri**kapsamındaki web siteleri ve projeler **için IIS Express'in 64 bit sürümünü kullanın'ı** seçerek ana bilgisayar işlemlerini değiştirebilirsiniz.

- Azure'da dağıtılan ASP.NET web uygulamaları için, Azure portalındaki **Uygulama ayarlarında** **64 bit** platform'u seçerek ana bilgisayar işlemlerini değiştirebilirsiniz.

> [!NOTE] 
> Varsayılan olarak, yeni Visual Studio projeleri **Herhangi bir CPU**olarak ayarlanır. Projenizi **x64** olarak ayarlamanızı öneririz, böylece **x86'ya**geçmez. Herhangi bir **CPU'ya** ayarlanan bir proje, yalnızca x86 bağımlılık eklenirse **kolayca x86'ya** geçebilir.<br/>
> ServiceInterop.dll'nin SDK DLL'nin yürütüldettiği klasörde olması gerekir. Bu, yalnızca DL'leri el ile kopyalarsanız veya özel yapı/dağıtım sistemlerine sahipseniz sorun olmalıdır.
    
**Sunucu tarafındaki çöp toplamayı açma (GC)**

Çöp toplama sıklığını azaltmak bazı durumlarda yardımcı olabilir. .NET'te [gcServer'ı](https://msdn.microsoft.com/library/ms229357.aspx) `true`.

**İstemci iş yükünüzü ölçeklendirin**

Yüksek iş düzeyi düzeylerinde (50.000'den fazla RU/s) test ediyorsanız, istemci uygulaması işlemci veya ağ kullanımında dışarı atlama makine nedeniyle darboğaz haline gelebilir. Bu noktaya ulaşırsanız, istemci uygulamalarınızı birden çok sunucuda ölçekleyerek Azure Cosmos DB hesabını daha da zorlamaya devam edebilirsiniz.

> [!NOTE] 
> Yüksek CPU kullanımı gecikme süresinin artmasına ve zaman adabı isteğine neden olabilir.

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

**Bağlantı ilkesi: Doğrudan bağlantı modunu kullanma**

Bir istemcinin Azure Cosmos DB'ye nasıl bağlandığı, özellikle gözlenen istemci tarafı gecikmesi için önemli performans etkileri vardır. İstemci bağlantı ilkesini yapılandırmak için iki temel yapılandırma ayarı vardır: bağlantı *modu* ve bağlantı *protokolü.*  Kullanılabilir iki mod şunlardır:

   * Ağ geçidi modu
      
     Ağ geçidi modu tüm SDK platformlarında desteklenir ve [Microsoft.Azure.DocumentDB SDK](sql-api-sdk-dotnet.md)için yapılandırılan varsayılandır. Uygulamanız sıkı güvenlik duvarı kısıtlamaları olan bir şirket ağında çalışıyorsa, standart HTTPS bağlantı noktasını ve tek bir bitiş noktasını kullandığından ağ geçidi modu en iyi seçimdir. Ancak performans dengelemesi, ağ geçidi modunun azure Cosmos DB'den her veri okunduveya yazıldığında ek bir ağ atlama içerir. Yani daha az ağ atlamaları olduğundan doğrudan mod daha iyi performans sunar. Sınırlı sayıda soket bağlantısı olan ortamlarda uygulamaları çalıştırdığınızda ağ geçidi bağlantı modunu da öneririz.

     SDK'yı Azure İşlevlerinde, özellikle [Tüketim planında](../azure-functions/functions-scale.md#consumption-plan)kullandığınızda, bağlantılardaki geçerli [sınırlamalara](../azure-functions/manage-connections.md)dikkat edin. Bu durumda, Azure İşlevleri uygulamanızdaki diğer HTTP tabanlı istemcilerle de çalışıyorsanız ağ geçidi modu daha iyi olabilir.

   * Doğrudan mod

     Doğrudan mod, TCP protokolü aracılığıyla bağlantıyı destekler ve [Microsoft.Azure.Cosmos/.NET V3 SDK](sql-api-sdk-dotnet-standard.md)kullanıyorsanız varsayılan bağlantı modudur.

Ağ geçidi modunda, Azure Cosmos DB, MongoDB için Azure Cosmos DB API'sini kullanırken 443 ve 10250, 10255 ve 10256 bağlantı noktalarını kullanır. Port 10250, varsayılan bir MongoDB örneğine coğrafi çoğaltma olmadan eşler. Port10255 ve 10256 coğrafi çoğaltma olan MongoDB örneğine harita.
     
TCP'yi doğrudan modda kullandığınızda, ağ geçidi bağlantı noktalarına ek olarak, Azure Cosmos DB dinamik TCP bağlantı noktalarını kullandığından, 10000 ile 20000 arasındaki bağlantı noktası aralığının açık olduğundan emin olmanız gerekir. Bu bağlantı noktaları açık değilse ve TCP'yi kullanmaya çalışırsanız, 503 Hizmet Kullanılamıyor hatası alırsınız. Bu tablo, çeşitli API'ler için kullanılabilir bağlantı modlarını ve her API için kullanılan servis bağlantı noktalarını gösterir:

|Bağlantı modu  |Desteklenen protokol  |Desteklenen SDK'lar  |API/Servis bağlantı noktası  |
|---------|---------|---------|---------|
|Ağ geçidi  |   HTTPS    |  Tüm SDK'lar    |   SQL (443), MongoDB (10250, 10255, 10256), Tablo (443), Cassandra (10350), Grafik (443)    |
|Direct    |     TCP    |  .NET SDK    | 10000 ile 20000 aralığındaki bağlantı noktaları |

Azure Cosmos DB, HTTPS üzerinden basit ve açık bir yeniden programlama modeli sunar. Ayrıca, iletişim modelinde de etkili bir TCP protokolü sunar ve .NET istemciSi SDK üzerinden kullanılabilir. TCP protokolü, ilk kimlik doğrulama ve trafiği şifrelemek için TLS kullanır. En iyi performans için mümkün olduğunda TCP protokolünü kullanın.

SDK V3 için, `CosmosClient` 'deki `CosmosClientOptions`örneği oluştururken bağlantı modunu yapılandırırsınız. Doğrudan modun varsayılan olduğunu unutmayın.

```csharp
var serviceEndpoint = new Uri("https://contoso.documents.net");
var authKey = "your authKey from the Azure portal";
CosmosClient client = new CosmosClient(serviceEndpoint, authKey,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

Microsoft.Azure.DocumentDB SDK için, `DocumentClient` örneğin yapısı sırasında bağlantı modunu `ConnectionPolicy` parametreyi kullanarak yapılandırırsınız. Doğrudan mod kullanıyorsanız, parametreyi `Protocol` `ConnectionPolicy` kullanarak da ayarlayabilirsiniz.

```csharp
var serviceEndpoint = new Uri("https://contoso.documents.net");
var authKey = "your authKey from the Azure portal";
DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct, // ConnectionMode.Gateway is the default
   ConnectionProtocol = Protocol.Tcp
});
```

TCP yalnızca doğrudan modda desteklendirilmeden, ağ geçidi modunu kullanırsanız, HTTPS `Protocol` protokolü `ConnectionPolicy` her zaman ağ geçidiyle iletişim kurmak için kullanılır ve değer yoksayılır.

![Azure Cosmos DB bağlantı ilkesi](./media/performance-tips/connection-policy.png)

**İlk istekte başlangıç gecikmesini önlemek için OpenAsync'i arayın**

Varsayılan olarak, adres yönlendirme tablosunu getirmesi gerektiğinden ilk istek daha yüksek gecikme ye sahiptir. [SDK V2](sql-api-sdk-dotnet.md)kullandığınızda, `OpenAsync()` ilk istekte bu başlangıç gecikmesini önlemek için başlatma sırasında bir kez arayın:

    await client.OpenAsync();

> [!NOTE] 
> `OpenAsync`hesaptaki tüm kapsayıcılar için adres yönlendirme tablosunu almak için istekler oluşturur. Çok sayıda kapsayıcısı olan ancak uygulaması bunların bir `OpenAsync` alt kümesine erişen hesaplar için gereksiz miktarda trafik oluşturur ve bu da başlatmayı yavaşlatır. Bu `OpenAsync` nedenle, uygulama başlatmayı yavaşlattığı için bu senaryoda kullanmak yararlı olmayabilir.

   <a id="same-region"></a>
**Performans için, aynı Azure bölgesindeki istemcileri harmanla**

Mümkün olduğunda, Azure Cosmos DB olarak adlandırdığı uygulamaları Azure Cosmos DB veritabanıyla aynı bölgeye yerleştirin. Burada yaklaşık bir karşılaştırma: aynı bölgede Azure Cosmos DB aramaları 1 ms ile 2 ms içinde tamamlanır, ancak ABD'nin Batı ve Doğu kıyıları arasındaki gecikme 50 ms'den fazladır. Bu gecikme sonu, istemciden Azure veri merkezi sınırına geçerken isteğin aldığı rotaya bağlı olarak istekten isteğe değişebilir. Arama uygulamasının, sağlanan Azure Cosmos DB bitiş noktasıyla aynı Azure bölgesinde bulunmasını sağlayarak mümkün olan en düşük gecikme süresine ulaşabilirsiniz. Kullanılabilir bölgelerin listesi için [Azure bölgelerine](https://azure.microsoft.com/regions/#services)bakın.

![Azure Cosmos DB](./media/performance-tips/same-region.png) bağlantı ilkesi<a id="increase-threads"></a>

**İş parçacığı/görev sayısını artırma**

Azure Cosmos DB'ye çağrılar ağ üzerinden yapıldığından, istemci uygulamasının istekler arasında en az süreyi beklemek için isteklerinizin paralellik derecesini değiştirmeniz gerekebilir. Örneğin, .NET [Görev Paralel Kitaplığını](https://msdn.microsoft.com//library/dd460717.aspx)kullanıyorsanız, Azure Cosmos DB'den okuyan veya yazan yüzlerce görevin sırasına göre oluşturun.

**Hızlandırılmış ağ etkinleştirme**
 
 Gecikme süresini ve CPU gerginliğini azaltmak için, istemci sanal makinelerde hızlandırılmış ağ etkinleştirmenizi öneririz. Bkz. [Hızlandırılmış ağ ile bir Windows sanal makine oluşturun](../virtual-network/create-vm-accelerated-networking-powershell.md) veya [hızlandırılmış ağ ile bir Linux sanal makine oluşturun.](../virtual-network/create-vm-accelerated-networking-cli.md)

## <a name="sdk-usage"></a>SDK kullanımı
**En son SDK'yı yükleyin**

Azure Cosmos DB SDK'lar en iyi performansı sağlamak için sürekli olarak geliştirilmektedir. En son SDK'yı belirlemek ve iyileştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](sql-api-sdk-dotnet-standard.md) sayfalarına bakın.

**Akış API'lerini kullanma**

[.NET SDK V3,](sql-api-sdk-dotnet-standard.md) serileştirmeden veri alabilen ve döndürebilen akış API'leri içerir. 

Yanıtları doğrudan SDK'dan tüketmeyen ancak bunları diğer uygulama katmanlarına aktaran orta katman uygulamaları akış API'lerinden yararlanabilir. Akış işleme örnekleri için [madde yönetimi](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement) örneklerine bakın.

**Uygulamanızın kullanım ömrü boyunca tek ton Azure Cosmos DB istemcisi kullanın**

Her `DocumentClient` `CosmosClient` ve örnek iş parçacığı güvenlidir ve doğrudan modda çalışırken verimli bağlantı yönetimi ve adres önbelleğe alma gerçekleştirir. Verimli bağlantı yönetimi ve daha iyi SDK istemci performansı sağlamak `AppDomain` için, uygulamanın kullanım ömrü için başına tek bir örnek kullanmanızı öneririz.

   <a id="max-connection"></a>

**Ağ geçidi modunu kullanırken ana bilgisayar başına System.Net MaxConnections'ı artırın**

Azure Cosmos DB istekleri, ağ geçidi modunu kullandığınızda HTTPS/REST üzerinden yapılır. Ana bilgisayar adı veya IP adresi başına varsayılan bağlantı sınırına tabi tutulurlar. İstemci kitaplığı Azure Cosmos DB'ye birden çok eşzamanlı bağlantı kullanabilmek için daha yüksek bir değere (100 ila 1.000) ayarlamanız `MaxConnections` gerekebilir. .NET SDK 1.8.0 ve sonraki durumlarda [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) için varsayılan değer 50'dir. Değeri değiştirmek için [Documents.Client.ConnectionPolicy.MaxConnectionLimit'i](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) daha yüksek bir değere ayarlayabilirsiniz.

**Bölümlenmiş koleksiyonlar için paralel sorguları ayarlama**

SQL .NET SDK 1.9.0 ve daha sonra paralel sorguları destekler, bu da bölümlenmiş bir koleksiyonu paralel olarak sorgulayabilmenizi sağlar. Daha fazla bilgi için, SDK'larla çalışmayla ilgili [kod örneklerine](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) bakın. Paralel sorgular, seri karşılıklarından daha iyi sorgu gecikmesi ve iş sonu sağlamak üzere tasarlanmıştır. Paralel sorgular, gereksinimlerinize uyacak şekilde ayarlayabileceğiniz iki parametre sağlar: 
- `MaxDegreeOfParallelism`paralel olarak sorgulanabilecek en fazla bölüm sayısını denetler. 
- `MaxBufferedItemCount`önceden getirilen sonuç sayısını denetler.

***Paralelliğin tuning derecesi***

Paralel sorgu, birden çok bölümü paralel olarak sorgulayarak çalışır. Ancak, tek bir bölümden gelen veriler sorguyla ilgili olarak seri olarak getirilir. `MaxDegreeOfParallelism` [SDK V2'de](sql-api-sdk-dotnet.md) veya `MaxConcurrency` [SDK V3'te](sql-api-sdk-dotnet-standard.md) bölüm sayısına ayar yapmak, diğer tüm sistem koşullarının aynı kalması koşuluyla en çok performans gösteren sorguyu elde etme şansına sahiptir. Bölüm sayısını bilmiyorsanız, paralellik derecesini yüksek bir sayıya ayarlayabilirsiniz. Sistem paralellik derecesi olarak minimum (bölüm sayısı, kullanıcı sağlanan giriş) seçecektir.

Veriler sorguyla ilgili olarak tüm bölümlere eşit olarak dağıtılırsa paralel sorguların en fazla faydayı sağladığını unutmayın. Bölümlenmiş koleksiyon, sorgu tarafından döndürülen verilerin tamamının veya çoğunun birkaç bölümde yoğunlaştırılacak şekilde bölümlenirse (bir bölüm en kötü durumdur), bu bölümler sorgunun performansını darboğaza sokar.

***Tuning MaxBufferedItemCount***
    
Paralel sorgu, geçerli sonuç grubu istemci tarafından işlenirken sonuçları önceden almak üzere tasarlanmıştır. Bu ön alma, bir sorgunun genel gecikmesini artırmaya yardımcı olur. Parametre, `MaxBufferedItemCount` önceden getirilen sonuç sayısını sınırlar. Sorgunun ön almadan maksimum faydayı almasına izin vermek için döndürülen beklenen sonuç sayısına (veya daha yüksek bir sayıya) ayarlayın. `MaxBufferedItemCount`

Ön alma, paralelliğin derecesine bakılmaksızın aynı şekilde çalışır ve tüm bölümlerden gelen veriler için tek bir arabellek vardır.  

**RetryAfter aralıklarında geri tepme uygulayın**

Performans testi sırasında, küçük bir istek oranı azaltılına kadar yükü artırmalısınız. İstekler azaltılırsa, istemci uygulaması sunucu tarafından belirtilen yeniden deneme aralığı için azaltma da geri almalıdır. Geri tepmeye saygı duymak, yeniden denemeler arasında en az miktarda beklemenizi sağlar. 

Yeniden deneme ilkesi desteği bu SDK'lara dahildir:
- Sql [için .NET SDK](sql-api-sdk-dotnet.md) ve SQL için Java [SDK](sql-api-sdk-java.md) sürüm 1.8.0 ve daha sonra
- Sürüm 1.9.0 ve daha sonra [SQL için Node.js SDK](sql-api-sdk-node.md) ve [SQL için Python SDK](sql-api-sdk-python.md)
- [.NET Core](sql-api-sdk-dotnet-core.md) SDK'ların tüm desteklenen sürümleri 

Daha fazla bilgi için [RetryAfter'a](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx)bakın.
    
.NET SDK'nın 1.19 sürümünde ve daha sonrasında, aşağıdaki örnekte gösterildiği gibi ek tanılama bilgilerini ve sorun giderme gecikmesi sorunlarını günlüğe kaydetmeye yönelik bir mekanizma vardır. Daha yüksek okuma gecikmesi olan istekler için tanılama dizesini kaydedebilirsiniz. Yakalanan tanılama dizesi, belirli bir istek için kaç kez 429 hata aldığınızı anlamanıza yardımcı olur.

```csharp
ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
readDocument.RequestDiagnosticsString 
```

**Daha düşük okuma gecikmesi için önbellek belge URI'leri**

En iyi okuma performansı için mümkün olduğunda Önbellek belge URI'leri. Kaynak oluştururken kaynak kimliğini önbelleğe almak için mantık tanımlamanız gerekir. Kaynak aramalarına dayalı aramalar ad tabanlı aramalardan daha hızlıdır, bu nedenle bu değerlerin önbelleğe getirilmesi performansı artırır.

   <a id="tune-page-size"></a>
**Daha iyi performans için sorgular/okuma akışları için sayfa boyutunu ayarlama**

Okuma akışı işlevini kullanarak (örneğin, `ReadDocumentFeedAsync`) veya bir SQL sorgusu verdiyseniz, sonuç kümesi çok büyükse, sonuçlar parçalı bir şekilde döndürülür. Varsayılan olarak, sonuçlar 100 öğe veya 1 MB'den oluşan parçalar halinde döndürülür (önce hangi sınıra vurulursa).

Tüm geçerli sonuçları almak için gereken ağ turu gezileri sayısını azaltmak için, 1.000 üstbilgi istemek için [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) kullanarak sayfa boyutunu artırabilirsiniz. Yalnızca birkaç sonuç görüntülemeniz gerektiğinde, örneğin, kullanıcı arabirimi veya uygulama API'niz aynı anda yalnızca 10 sonuç döndürürse, okuma lar ve sorgular için tüketilen iş buzunazaltılması için sayfa boyutunu 10'a düşürebilirsiniz.

> [!NOTE] 
> Mülkiyet `maxItemCount` sadece pagination için kullanılmamalıdır. Ana kullanımı, tek bir sayfada döndürülen en fazla öğe sayısını azaltarak sorguların performansını artırmaktır.  

Ayrıca, kullanılabilir Azure Cosmos DB SDK'larını kullanarak sayfa boyutunu da ayarlayabilirsiniz. [MaxItemCount](/dotnet/api/microsoft.azure.documents.client.feedoptions.maxitemcount?view=azure-dotnet) özelliği, `FeedOptions` numaralandırma işleminde döndürülecek en fazla öğe sayısını ayarlamanızı sağlar. -1 olarak ayarlandığında, `maxItemCount` SDK belge boyutuna bağlı olarak otomatik olarak en uygun değeri bulur. Örnek:
    
```csharp
IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
```
    
Sorgu yürütüldüğünde, elde edilen veriler bir TCP paketine gönderilir. Çok düşük bir değer `maxItemCount`belirtirseniz, TCP paketindeki verileri göndermek için gereken gezi sayısı yüksektir ve bu da performansı etkiler. Bu nedenle, özellik için hangi değeri `maxItemCount` ayarladığınızdan emin değilseniz, en iyisi -1 olarak ayarlamak ve SDK'nın varsayılan değeri seçmesine izin vermek.

**İş parçacığı/görev sayısını artırma**

Bkz. Bu makalenin Ağ [bölümündeki iş parçacığı/görev sayısını artırın.](#increase-threads)

## <a name="indexing-policy"></a>Dizin oluşturma ilkesi
 
**Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

Azure Cosmos DB dizin oluşturma ilkesi, dizin oluşturma yollarını (IndexingPolicy.IncludedPaths and IndexingPolicy.ExcludedPaths) kullanarak hangi belge yollarının eklenecek veya dizin oluşturma dışında belirtebilirsiniz. Dizin oluşturma yolları yazma performansını artırabilir ve sorgu desenleri önceden bilinen senaryolar için dizin depolama azaltabilir. Bunun nedeni, dizin oluşturma maliyetlerinin doğrudan dizine eklenmiş benzersiz yolların sayısıyla ilişkili olmasıdır. Örneğin, bu kod, "*" joker kartını kullanarak belgelerin (bir alt ağaç) tüm bölümünün dizin oluşturmadan nasıl dışlanır:

```csharp
var collection = new DocumentCollection { Id = "excludedPathCollection" };
collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
```

Daha fazla bilgi için [Azure Cosmos DB dizin oluşturma ilkelerine](index-policy.md)bakın.

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

**Daha düşük İstek Birimleri/saniye kullanımı için ölçün ve ayarlayın**

Azure Cosmos DB zengin bir veritabanı işlemleri kümesi sunar. Bu işlemler, udf'ler, depolanan yordamlar ve tetikleyiciler ile ilişkisel ve hiyerarşik sorguları içerir, hepsi bir veritabanı koleksiyonundaki belgeler üzerinde çalışan. Bu işlemlerin her biriyle ilişkili maliyet, işlemi tamamlamak için gereken CPU, IO ve belleğe bağlı olarak değişir. Donanım kaynaklarını düşünmek ve yönetmek yerine, bir İstek Birimi'ni (RU) çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet etmek için gereken kaynaklar için tek bir ölçü olarak düşünebilirsiniz.

Elde etme, her konteyner için ayarlanan [İstek Birimleri](request-units.md) sayısına göre sağlanır. İstek Birimi tüketimi saniye başına bir oran olarak değerlendirilir. Konteynerleri için verilen Talep Birimi oranını aşan uygulamalar, konteyner için verilen seviyenin altına düşene kadar sınırlıdır. Uygulamanız daha yüksek bir üretim düzeyi gerektiriyorsa, ek İstek Birimleri sağlayarak iş inizi artırabilirsiniz.

Sorgunun karmaşıklığı, bir işlem için kaç İstek Birimi nin tüketilmesini etkiler. Yüklemlerin sayısı, yüklemlerin yapısı, UDF sayısı ve kaynak veri kümesinin boyutu sorgu işlemlerinin maliyetini etkiler.

Herhangi bir işlemin genel yükünü ölçmek için (oluşturma, güncelleme veya silme), işlemler tarafından tüketilen İstek `ResourceResponse\<T>` Birimi `FeedResponse\<T>` sayısını ölçmek için [x-ms-request-charge](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üstbilgisini (veya .NET SDK'daki veya .NET SDK'daki eşdeğer `RequestCharge` özelliği) inceleyin:

```csharp
// Measure the performance (Request Units) of writes
ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
while (queryable.HasMoreResults)
    {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

Bu üstbilgide döndürülen istek ücreti, sağlanan iş ınızın bir kısmıdır (yani 2.000 RUs / saniye). Örneğin, önceki sorgu 1.000 1-KB belgedöndürürse, işlemin maliyeti 1.000'dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki tür isteği onurlandırır. Daha fazla bilgi için, [İstek Birimleri](request-units.md) ve [İstek Birimi hesap makinesine](https://www.documentdb.com/capacityplanner)bakın.
<a id="429"></a>

**Oran sınırlama/istek oranını çok büyük işleme**

İstemci bir hesap için ayrılmış iş düzeyini aşmaya çalıştığında, sunucuda performans bozulması olmaz ve ayrılmış düzeyin ötesinde iş gücü kapasitesi kullanılmaz. Sunucu, RequestRateTooLarge (HTTP durum kodu 429) ile isteği önceden sona erdirecektir. Kullanıcının isteği yeniden denemeden önce beklemesi gereken süreyi milisaniye cinsinden gösteren bir [x-ms-retry-after-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üstbilgisini döndürür.

    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

SDK'ların tümü bu yanıtı dolaylı olarak yakalar, sunucu tarafından belirtilen yeniden deneme üstbilgine saygı duyar ve isteği yeniden deneyin. Hesabınıza aynı anda birden çok istemci erişilmediği sürece, bir sonraki yeniden deneme başarılı olur.

İstem oranının üzerinde sürekli olarak çalışan birden fazla istemciniz varsa, istemci tarafından dahili olarak şu anda 9 olarak ayarlanan varsayılan yeniden deneme sayısı yeterli olmayabilir. Bu durumda, istemci uygulamaya durum kodu 429 içeren bir DocumentClientException atar. 

`ConnectionPolicy` Örneği ayarlayarak `RetryOptions` varsayılan yeniden deneme sayısını değiştirebilirsiniz. Varsayılan olarak, durum kodu 429'a sahip DocumentClientException, istek isteği oranının üzerinde çalışmaya devam ederse, 30 saniyelik bir toplu bekleme süresinden sonra döndürülür. Bu hata, geçerli yeniden deneme sayısı, geçerli değerin varsayılan 9 veya kullanıcı tanımlı değeri olup olmadığı en yüksek yeniden deneme sayısından az olsa bile geri döner.

Otomatik yeniden deneme davranışı, çoğu uygulama için esnekliği ve kullanılabilirliği artırmaya yardımcı olur. Ancak performans kriterleri yaparken, özellikle gecikme yi ölçerken en iyi davranış olmayabilir. Deneme sunucu gaza çarparsa ve istemci SDK'nın sessizce yeniden denemesine neden olursa istemcitarafından gözlenen gecikme söker. Performans denemeleri sırasında gecikme artışlarını önlemek için, her işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılmış istek oranının altında çalıştığından emin olun. Daha fazla bilgi için [İstek Birimleri'ne](request-units.md)bakın.

**Daha yüksek iş için, daha küçük belgeler için tasarım**

Belirli bir işlemin istek ücreti (diğer bir şekilde, istek işleme maliyeti) doğrudan belgenin boyutuyla ilişkilidir. Büyük belgelerdeki işlemler, küçük belgelerdeki işlemlerden daha pahalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Birkaç istemci makinede yüksek performanslı senaryolar için Azure Cosmos DB'yi değerlendirmek için kullanılan örnek bir uygulama için Azure [Cosmos DB ile Performans ve ölçek testine](performance-testing.md)bakın.

Uygulamanızı ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de Bölümleme ve ölçeklendirme](partition-data.md)bölümüne bakın.
