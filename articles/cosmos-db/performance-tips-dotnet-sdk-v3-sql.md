---
title: .NET SDK V3 için Azure Cosmos DB performans ipuçları
description: Azure Cosmos DB .NET v3 SDK performansını geliştirmek için istemci yapılandırma seçeneklerini öğrenin.
author: j82w
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/16/2020
ms.author: jawilley
ms.openlocfilehash: a10272324a9535a0c2468d63a404f76ca56ce375
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85263526"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Azure Cosmos DB ve .NET için performans ipuçları

> [!div class="op_single_selector"]
> * [.NET SDK V3](performance-tips-dotnet-sdk-v3-sql.md)
> * [.NET SDK v2](performance-tips.md)
> * [Java SDK’sı v4](performance-tips-java-sdk-v4-sql.md)
> * [Zaman uyumsuz Java SDK v2](performance-tips-async-java.md)
> * [Zaman uyumlu Java SDK v2](performance-tips-java.md)

Azure Cosmos DB, garantili gecikme ve verimlilik ile sorunsuz bir şekilde ölçeklenen hızlı ve esnek bir dağıtılmış veritabanıdır. Veritabanınızı Azure Cosmos DB ölçeklendirmek için önemli mimari değişiklikler yapmanız veya karmaşık kod yazmanız gerekmez. Ölçeği artırma ve azaltma, tek bir API çağrısı yapmak kadar kolaydır. Daha fazla bilgi edinmek için bkz. [kapsayıcı verimini sağlama](how-to-provision-container-throughput.md) veya [veritabanı verimini sağlama](how-to-provision-database-throughput.md). Ancak Azure Cosmos DB ağ çağrıları üzerinden erişildiği için, [SQL .NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3)kullandığınızda en yüksek performansa ulaşmak için kullanabileceğiniz istemci tarafı iyileştirmeler vardır.

Bu nedenle, veritabanı performansını artırmaya çalışıyorsanız şu seçenekleri göz önünde bulundurun:

## <a name="hosting-recommendations"></a>Barındırma önerileri

**Sorgu yoğunluğu yoğun iş yükleri için Linux veya Windows 32 bit ana bilgisayar işlemesi yerine Windows 64-bit kullanın**

Gelişmiş performans için Windows 64 bit ana bilgisayar işlemesini öneririz. SQL SDK, sorguları yerel olarak ayrıştırmak ve iyileştirmek için yerel bir ServiceInterop.dll içerir. ServiceInterop.dll yalnızca Windows x64 platformunda desteklenir. ServiceInterop.dll kullanılamadığı Linux ve diğer desteklenmeyen platformlar için, iyileştirilmiş sorguyu almak üzere ağ geçidine ek bir ağ çağrısı yapılır. Aşağıdaki uygulama türleri varsayılan olarak 32 bitlik ana bilgisayar işlemeyi kullanır. Ana bilgisayar işlemesini 64-bit işleme olarak değiştirmek için, uygulamanızın türüne göre aşağıdaki adımları izleyin:

- Yürütülebilir uygulamalar için, **Yapı** sekmesindeki **Proje özellikleri** penceresinde [Platform hedefini](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) **x64** olarak ayarlayarak ana bilgisayar işlemesini değiştirebilirsiniz.

- VSTest tabanlı test projeleri için, **Test**  >  **Test Settings**  >  Visual Studio **Test** menüsünde test test ayarları**Varsayılan işlemci mimarisi x64 olarak** ' yi seçerek konak işlemeyi değiştirebilirsiniz.

- Yerel olarak dağıtılan ASP.NET Web uygulamaları için, **Araçlar**seçenekler projeler ve çözümler Web projeleri altındaki **Web siteleri ve projeleri için IIS Express 64 bitlik sürümünü kullan** ' ı seçerek konak işlemeyi değiştirebilirsiniz  >  **Options**  >  **Projects and Solutions**  >  **Web Projects**.

- Azure 'da dağıtılan ASP.NET Web uygulamaları için Azure portal **uygulama ayarlarında** **64 bitlik** platformu seçerek konak işlemeyi değiştirebilirsiniz.

> [!NOTE] 
> Varsayılan olarak, yeni Visual Studio projeleri **herhangi BIR CPU**'ya ayarlanır. Projenizi **x86**'ya geçiş yapmak için **x64** olarak ayarlamanızı öneririz. **Herhangi BIR CPU** 'ya ayarlanmış bir proje, yalnızca x86 bağımlılığı eklendiyse, kolayca **x86** 'ya geçiş yapabilir.<br/>
> ServiceInterop.dll SDK DLL 'inin yürütüldüğü klasörde olması gerekir. Bu, yalnızca dll 'Leri el ile kopyalarsanız veya özel derleme/dağıtım sistemlerine sahipseniz sorun olması gerekir.
    
**Sunucu tarafı atık toplamayı aç (GC)**

Çöp toplamanın sıklığını azaltmak bazı durumlarda yardımcı olabilir. .NET sürümünde [gcServer](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection) olarak ayarlayın `true` .

**İstemci iş yükünüzü ölçeklendirin**

Yüksek aktarım hızı düzeylerinde (50.000 RU/sn 'den fazla) test ediyorsanız, istemci uygulaması makine CPU veya ağ kullanımında kullanıma hazır hale geldiği için performans sorunlarına neden olabilir. Bu noktaya ulaştığınızda, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirerek Azure Cosmos DB hesabını daha fazla göndermeye devam edebilirsiniz.

> [!NOTE] 
> Yüksek CPU kullanımı, artan gecikme süresine ve istek zaman aşımı özel durumlarına neden olabilir.

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

**Bağlantı ilkesi: doğrudan bağlantı modunu kullan**

İstemcinin Azure Cosmos DB 'e bağlanması, özellikle de gözlemlenen istemci tarafı gecikme süresi için önemli performans etkilerine sahiptir. İstemci bağlantı ilkesini yapılandırmak için kullanılabilecek iki temel yapılandırma ayarı vardır: bağlantı *modu* ve bağlantı *Protokolü*.  Kullanılabilir iki mod şunlardır:

   * Doğrudan mod (varsayılan)

     Doğrudan mod, TCP protokolü ile bağlantıyı destekler ve [Microsoft. Azure. Cosmos/. net v3 SDK](https://github.com/Azure/azure-cosmos-dotnet-v3)kullanıyorsanız varsayılan bağlantı modudur. Bu, daha iyi performans sağlar ve ağ geçidi modundan daha az ağ atlaması gerektirir.

   * Ağ geçidi modu
      
     Uygulamanız, katı güvenlik duvarı kısıtlamalarına sahip bir kurumsal ağda çalışıyorsa, standart HTTPS bağlantı noktasını ve tek bir uç noktayı kullandığından, ağ geçidi modu en iyi seçimdir. Ancak performans zorunluluğunu getirir, ağ geçidi modunun, verilerin Azure Cosmos DB her okunışında veya üzerine yazıldığı her seferinde ek bir ağ atlaması içerir. Bu nedenle, daha az ağ atlaması olduğundan doğrudan mod daha iyi performans sunar. Ayrıca, sınırlı sayıda soket bağlantısı olan ortamlarda uygulamalar çalıştırdığınızda ağ geçidi bağlantı modunu da öneririz.

     Azure Işlevlerinde SDK kullandığınızda, özellikle [Tüketim planında](../azure-functions/functions-scale.md#consumption-plan), [bağlantılardaki geçerli limitlerin](../azure-functions/manage-connections.md)farkında olun. Bu durumda, Azure Işlevleri uygulamanızdaki diğer HTTP tabanlı istemcilerle de çalışıyorsanız ağ geçidi modu daha iyi olabilir.


Ağ Geçidi modunda, Azure Cosmos DB MongoDB için Azure Cosmos DB API kullandığınızda 443 bağlantı noktasını ve 10250, 10255 ve 10256 bağlantı noktalarını kullanır. 10250 numaralı bağlantı noktası, coğrafi çoğaltma olmadan bir varsayılan MongoDB örneğiyle eşlenir. 10255 ve 10256 bağlantı noktaları, coğrafi çoğaltma içeren MongoDB örneğiyle eşlenir.
     
Doğrudan modda TCP kullandığınızda, ağ geçidi bağlantı noktalarına ek olarak, Azure Cosmos DB dinamik TCP bağlantı noktalarını kullandığından, 10000 ve 20000 arasındaki bağlantı noktası aralığının açık olduğundan emin olmanız gerekir ( [Özel uç noktalarında](./how-to-configure-private-endpoints.md)doğrudan mod KULLANıLıRKEN, TCP bağlantı noktalarının tam aralığı-0 ile 65535 arasında). Bağlantı noktaları, standart Azure VM yapılandırması için varsayılan olarak açıktır. Bu bağlantı noktaları açık değilse ve TCP kullanmaya çalışırsanız, 503 hizmetinde kullanılamayan bir hata alırsınız. Bu tabloda, çeşitli API 'Ler ve her API için kullanılan hizmet bağlantı noktaları için kullanılabilen bağlantı modları gösterilmektedir:

|Bağlantı modu  |Desteklenen Protokol  |Desteklenen SDK 'lar  |API/hizmet bağlantı noktası  |
|---------|---------|---------|---------|
|Ağ geçidi  |   HTTPS    |  Tüm SDK 'lar    |   SQL (443), MongoDB (10250, 10255, 10256), Table (443), Cassandra (10350), Graph (443)    |
|Direct    |     TCP    |  .NET SDK    | Ortak/hizmet uç noktaları kullanılırken: 10000 ile 20000 arasında bağlantı noktaları<br>Özel uç noktalar kullanılırken: 0 ila 65535 aralığındaki bağlantı noktaları |

Azure Cosmos DB, HTTPS üzerinden basit ve açık bir yeniden programlama modeli sunar. Ayrıca, bu, iletişim modelinde da daha da fazla olan ve .NET istemci SDK 'Sı aracılığıyla kullanılabilen etkin bir TCP protokolünü sunmaktadır. TCP protokolü ilk kimlik doğrulaması ve trafiği şifreleme için TLS kullanır. En iyi performans için, mümkün olduğunda TCP protokolünü kullanın.

SDK V3 için, içinde örneğini oluştururken bağlantı modunu yapılandırırsınız `CosmosClient` `CosmosClientOptions` . Doğrudan modunun varsayılan olduğunu unutmayın.

```csharp
string connectionString = "<your-account-connection-string>";
CosmosClient client = new CosmosClient(connectionString,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

TCP yalnızca doğrudan modunda desteklendiğinden, ağ geçidi modunu kullanıyorsanız, HTTPS protokolü her zaman ağ geçidiyle iletişim kurmak için kullanılır.

:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Azure Cosmos DB bağlantı ilkesi" border="false":::

**Kısa ömürlü bağlantı noktası tükenmesi**

Örneklerinizi üzerinde yüksek bir bağlantı birimi veya yüksek bağlantı noktası kullanımı görürseniz, önce istemci örneklerinizin tekton olduğunu doğrulayın. Diğer bir deyişle, istemci örneklerinin uygulamanın ömrü için benzersiz olması gerekir.

TCP protokolünde çalışırken, istemci, uzun süreli bağlantıları kullanarak gecikme süresini en iyi duruma getirir ve bu da, 2 dakikadan sonra bağlantıları sonlandırır.

Seyrek erişiminizin olduğu senaryolarda ve ağ geçidi modu erişimiyle karşılaştırıldığında daha yüksek bir bağlantı sayısı fark ederseniz şunları yapabilirsiniz:

* [Cosmosclientoptions. PortReuseMode](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.portreusemode) özelliğini `PrivatePortPool` (Framework sürümü>= 4.6.1 ve .net Core sürümü >= 2,0) olarak yapılandırın: Bu özellik SDK 'nın farklı Azure Cosmos DB hedef uç noktaları için kısa ömürlü bağlantı noktaları havuzu kullanmasına izin verir.
* [Cosmosclientoptions. IDbir ConnectionTimeout](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.idletcpconnectiontimeout) özelliği, 10 dakikadan büyük veya buna eşit olmalıdır. Önerilen değerler 20 dakika ile 24 saat arasında.

<a id="same-region"></a>

**Performans için aynı Azure bölgesindeki istemcileri birlikte bulun**

Mümkün olduğunda, Azure Cosmos DB veritabanıyla aynı bölgedeki Azure Cosmos DB çağıran tüm uygulamaları yerleştirin. İşte yaklaşık bir karşılaştırma: aynı bölgedeki Azure Cosmos DB için yapılan çağrılar 1 MS ile 2 ms arasında tamamlanır, ancak ABD 'nin batı ve Doğu yakası arasındaki gecikme 50 MS 'den fazla olur. Bu gecikme, istemciden Azure veri merkezi sınırına geçerken istek tarafından alınan yola bağlı olarak istek üzerine değişiklik gösterebilir. Çağıran uygulamanın, sağlanan Azure Cosmos DB uç noktası ile aynı Azure bölgesinde yer almasını sağlayarak olası en düşük gecikme süresini sağlayabilirsiniz. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

:::image type="content" source="./media/performance-tips/same-region.png" alt-text="Azure Cosmos DB bağlantı ilkesi" border="false":::

   <a id="increase-threads"></a>

**İş parçacığı/görev sayısını artırma**

Azure Cosmos DB çağrıları ağ üzerinden yapıldığından, istemci uygulamanın istekler arasında en az zaman harcamasını sağlamak için isteklerinizin eşzamanlılık derecesini değiştirmeniz gerekebilir. Örneğin, .NET [görev paralel kitaplığı](https://msdn.microsoft.com//library/dd460717.aspx)kullanıyorsanız, Azure Cosmos DB okuyan veya üzerine yazılan yüzlerce görev sırasını oluşturun.

**Hızlandırılmış ağı etkinleştir**
 
 Gecikme süresi ve CPU değişimi azaltmak için istemci sanal makinelerinde hızlandırılmış ağı etkinleştirmenizi öneririz. Bkz. [hızlandırılmış ağ Ile Windows sanal makinesi oluşturma](../virtual-network/create-vm-accelerated-networking-powershell.md) veya [hızlandırılmış ağ ile Linux sanal makinesi oluşturma](../virtual-network/create-vm-accelerated-networking-cli.md).

## <a name="sdk-usage"></a>SDK kullanımı
**En son SDK 'Yı yükler**

Azure Cosmos DB SDK 'Ları, en iyi performansı sağlamak için sürekli geliştirilmiştir. En son SDK 'Yı öğrenmek ve geliştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) sayfalarına bakın.

**Stream API 'Lerini kullanma**

[.NET SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) , verileri seri hale getirmeksizin alıp döndürebilirler. 

Yanıtları doğrudan SDK 'dan tüketmeyen, ancak bunları diğer uygulama katmanlarına geçirmeyen orta katmanlı uygulamalar, Stream API 'Lerinden faydalanabilir. Akış işleme örnekleri için bkz. [öğe yönetimi](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement) örnekleri.

**Uygulamanızın ömrü boyunca tek bir Azure Cosmos DB istemcisi kullanın**

Her `CosmosClient` örnek iş parçacığı açısından güvenlidir ve doğrudan modda çalışırken verimli bağlantı yönetimi ve adres önbelleği gerçekleştirir. Etkili bağlantı yönetimine ve daha iyi SDK istemci performansına izin vermek için, `AppDomain` uygulamanın kullanım ömrü boyunca tek bir örnek kullanmanızı öneririz.

Azure Işlevleri üzerinde çalışırken örneklerin de mevcut [yönergeleri](../azure-functions/manage-connections.md#static-clients) izlemesi ve tek bir örneği koruması gerekir.

<a id="max-connection"></a>

**Yazma işlemlerinde içerik yanıtını devre dışı bırak**

Cenve Create yükleri olan iş yükleri için EnableContentResponseOnWrite istek seçeneğini false olarak ayarlayın. Hizmet artık oluşturulan veya güncellenen kaynağı SDK 'ya döndürmez. Normalde uygulama, oluşturulan nesneye sahiptir, bu nedenle hizmet bunu döndürmek için gerekli değildir. Üst bilgi değerlerine hala istek ücreti gibi erişilebilir. Bu, SDK 'nın artık bellek ayırmayı veya yanıtın gövdesini serileştirmesine gerek duymadığından performansı iyileştirebilir. Bu ayrıca performansı daha da kolaylaştırmak için ağ bant genişliği kullanımını azaltır.  

```csharp
ItemRequestOption requestOptions = new ItemRequestOptions() { EnableContentResponseOnWrite = false };
ItemResponse<Book> itemResponse = await this.container.CreateItemAsync<Book>(book, new PartitionKey(book.pk), requestOptions);
// Resource will be null
itemResponse.Resource
```

**Gecikme süresi yerine üretilen işi iyileştirmek Için toplu etkinleştirme** İş yükünün büyük miktarda işleme gerektirdiği ve gecikme süresinin önemli olmadığı senaryolarda toplu olarak etkinleştirin. Özelliği etkinleştirme hakkında daha fazla bilgi ve için hangi senaryoları kullanılması gerektiği hakkında daha fazla bilgi için bkz. [toplu 'A giriş](https://devblogs.microsoft.com/cosmosdb/introducing-bulk-support-in-the-net-sdk) .

**Ağ Geçidi modu kullanırken konak başına System.Net MaxConnections 'yi artırma**

Ağ Geçidi modunu kullandığınızda Azure Cosmos DB istekleri HTTPS/REST üzerinden yapılır. Tabi, ana bilgisayar adı veya IP adresi başına varsayılan bağlantı sınırına göre yapılır. `MaxConnections`İstemci kitaplığının Azure Cosmos DB için birden çok eş zamanlı bağlantı kullanabilmesi için daha yüksek bir değere (100 'e 1.000) ayarlamanız gerekebilir. .NET SDK 1.8.0 ve üzeri sürümlerde, [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) için varsayılan değer 50 ' dir. Değeri değiştirmek için [belgeler. Client. ConnectionPolicy. MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) değerini daha yüksek bir değere ayarlayabilirsiniz.

**Bölümlenmiş koleksiyonlar için Paralel sorguları ayarlama**

SQL .NET SDK Paralel sorguları destekler, bu, bölümlenmiş bir kapsayıcıyı paralel olarak sorgulamanızı sağlar. Daha fazla bilgi için bkz. SDK 'lar ile çalışma ile ilgili [kod örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs) . Paralel sorgular, seri karşılığından daha iyi sorgu gecikmesi ve verimlilik sağlamak üzere tasarlanmıştır. Paralel sorgular, gereksinimlerinize uyacak şekilde ayarlayabilmeniz için iki parametre sağlar: 
- `MaxConcurrency`paralel olarak sorgulanabilecek en fazla bölüm sayısını denetler. 
- `MaxBufferedItemCount`önceden getirilen sonuçların sayısını denetler.

***Eşzamanlılık derecesi ayarlama***

Paralel sorgu birden çok bölümü paralel olarak sorgulayarak işe yarar. Ancak tek bir bölümden alınan veriler sorguya göre işlem içine alınır. `MaxConcurrency` [SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) 'nin bölüm sayısına göre ayarlanması en iyi performansı elde etmek için en iyi performansa sahiptir, diğer tüm sistem koşulları aynı kalır. Bölüm sayısını bilmiyorsanız paralellik derecesini yüksek bir sayı olarak ayarlayabilirsiniz. Sistem paralellik derecesi olarak en az (bölüm sayısını, Kullanıcı tarafından girilen girişi) seçer.

Paralel sorgular, verilerin sorguya göre tüm bölümler arasında eşit bir şekilde dağıtılması halinde en avantaja sahip olur. Bölümlenmiş koleksiyon, bir sorgu tarafından döndürülen verilerin tümünün veya çoğu birkaç bölümde yoğunlaşarak (bir bölüm en kötü durumdur), bu bölümlerin sorgunun performansını performans sorunlarına neden olur.

***MaxBufferedItemCount ayarlama***
    
Paralel sorgu, geçerli sonuç toplu işi istemci tarafından işlendiği sırada sonuçları önceden getirmek üzere tasarlanmıştır. Bu önceden getirme, bir sorgunun genel gecikmesinin artırılmasına yardımcı olur. `MaxBufferedItemCount`Parametresi, önceden getirilen sonuçların sayısını sınırlar. `MaxBufferedItemCount`Sorgunun ön getirmeyi en fazla avantaj almasına izin vermek için beklenen sonuç sayısı (veya daha yüksek bir sayı) olarak ayarlayın.

Önceden getirme, paralellik derecesi ne olursa olsun aynı şekilde çalışacaktır ve tüm bölümlerdeki veriler için tek bir arabellek vardır.  

**RetryAfter aralıklarında geri alma Uygula**

Performans testi sırasında, küçük bir istek hızı kısıtlanana kadar yükü artırmanız gerekir. İstekler kısıtlanmamışsa, sunucu tarafından belirtilen yeniden deneme aralığı için istemci uygulamanın azaltılmasından sonra kapatılması gerekir. Geri alma işleminin tamamlanması, yeniden denemeler arasında bekleyen en az miktarda süre harcamanızı sağlar. 

Daha fazla bilgi için bkz. [RetryAfter](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosexception.retryafter?view=azure-dotnet#Microsoft_Azure_Cosmos_CosmosException_RetryAfter).
    
Aşağıdaki örnekte gösterildiği gibi ek tanılama bilgileri ve sorun giderme gecikme sorunlarını günlüğe kaydetme mekanizması vardır. Daha yüksek okuma gecikmesi olan istekler için tanılama dizesini günlüğe kaydedebilirsiniz. Yakalanan tanılama dizesi, belirli bir istek için kaç kez 429 hatası aldığınızı anlamanıza yardımcı olur.

```csharp
ItemResponse<Book> readItemResponse = await this.cosmosContainer.ReadItemAsync<Book>("ItemId", new PartitionKey("PartitionKeyValue"));
readItemResponse.Diagnostics.ToString(); 
```

**İş parçacığı/görev sayısını artırma**

Bkz. Bu makalenin ağ bölümünde [iş parçacığı sayısı/görevler sayısını artırın](#increase-threads) .

## <a name="indexing-policy"></a>Dizin oluşturma ilkesi
 
**Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

Azure Cosmos DB Dizin oluşturma ilkesi, dizin oluşturma yollarını (ındexingpolicy. ıncludedpaths ve ındexingpolicy. ExcludedPaths) kullanarak hangi belge yollarının dizine eklenmesini veya hangilerinin dışlanacağını belirtmenize de olanak tanır. Yalnızca ihtiyacınız olan yolların dizinlemesi, yazma performansını iyileştirebilir, yazma işlemlerinde RU ücretini azaltabilir ve sorgu desenlerinin önceden bildiği senaryolar için Dizin depolamayı azaltabilirsiniz. Bunun nedeni, dizin oluşturma maliyetlerinin doğrudan dizin oluşturulan benzersiz yolların sayısıyla ilişkilendirilmesi. Örneğin, bu kod, "*" joker karakterini kullanarak, belgelerin tamamını (bir alt ağacı) dizin oluşturma işleminden nasıl dışarıda bırakakullanacağınızı gösterir:

```csharp
var containerProperties = new ContainerProperties(id: "excludedPathCollection", partitionKeyPath: "/pk" );
containerProperties.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
containerProperties.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
Container container = await this.cosmosDatabase.CreateContainerAsync(containerProperties);
```

Daha fazla bilgi için bkz. [Azure Cosmos DB Dizin oluşturma ilkeleri](index-policy.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

**Düşük Istek birimleri/ikinci kullanım için ölçme ve ayarlama**

Azure Cosmos DB zengin bir veritabanı işlemleri kümesi sunar. Bu işlemler, bir veritabanı koleksiyonu içindeki belgeler üzerinde çalışan UDF 'ler, saklı yordamlar ve tetikleyicilerle ilişkisel ve hiyerarşik sorgular içerir. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi gerçekleştirmek için gereken CPU, GÇ ve belleğe göre değişir. Donanım kaynaklarını düşünmek ve yönetmek yerine, çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet vermek için gereken kaynaklar için bir Istek birimi (RU) tek bir ölçü olarak düşünebilirsiniz.

Aktarım hızı, her bir kapsayıcı için ayarlanan [Istek birimi](request-units.md) sayısına göre sağlanır. İstek birimi tüketimi, saniye başına bir hız olarak değerlendirilir. Kapsayıcı için sağlanan Istek birimi oranını aşan uygulamalar, oran için sağlanan düzeyin altına düşene kadar sınırlandırılır. Uygulamanız daha yüksek bir işleme düzeyi gerektiriyorsa, ek Istek birimleri sağlayarak aktarım hızını artırabilirsiniz.

Bir sorgunun karmaşıklığı, bir işlem için kaç Istek biriminin tüketildiğini etkiler. Koşulların sayısı, koşulların doğası, UDF sayısı ve kaynak veri kümesinin boyutu, sorgu işlemlerinin maliyetini etkiler.

Herhangi bir işlemin (oluşturma, güncelleştirme veya silme) yükünü ölçmek için, [x-ms-request-charge](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) `RequestCharge` `ResourceResponse\<T>` `FeedResponse\<T>` işlemler tarafından tüketilen istek birimlerinin sayısını ölçmek üzere x-MS-Request-ücret üst bilgisini (veya .NET SDK içindeki veya içinde eşdeğer özelliği) inceleyin:

```csharp
// Measure the performance (Request Units) of writes
ItemResponse<Book> response = await container.CreateItemAsync<Book>(myBook, new PartitionKey(myBook.PkValue));
Console.WriteLine("Insert of item consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
FeedIterator<Book> queryable = container.GetItemQueryIterator<ToDoActivity>(queryString);
while (queryable.HasMoreResults)
    {
        FeedResponse<Book> queryResponse = await queryable.ExecuteNextAsync<Book>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

Bu üst bilgide döndürülen istek ücreti, sağlanan aktarım hızını (yani, 2.000 ru/saniye) bir kesri. Örneğin, yukarıdaki sorgu 1.000 1 KB 'lik belgeler döndürürse, işlemin maliyeti 1.000 ' dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki istek olduğunu kabul eder. Daha fazla bilgi için bkz. [Istek birimleri](request-units.md) ve [istek birimi Hesaplayıcısı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>

**Tanıtıcı hız sınırlandırma/istek hızı çok büyük**

Bir istemci, bir hesap için ayrılan aktarım hızını aşmaya çalıştığında, sunucuda bir performans düşüşü olmaz ve ayrılan düzeyin ötesinde üretilen iş kapasitesi kullanılamaz. Sunucu isteği RequestRateTooLarge ile sona erdirmek için preemptively (HTTP durum kodu 429). Kullanıcının isteği yeniden denemeden önce beklemesi gereken süre miktarını milisaniye cinsinden belirten bir [x-MS-retry-After-MS](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üst bilgisi döndürür.

    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

SDK 'lar, sunucu tarafından belirtilen yeniden deneme üst bilgisine göre bu yanıtı dolaylı olarak yakalar ve isteği yeniden dener. Hesabınız birden çok istemci tarafından aynı anda erişilmediği takdirde, sonraki yeniden deneme başarılı olur.

İstek hızının sürekli olarak birden fazla istemciniz varsa, bu, şu anda istemci tarafından dahili olarak 9 olarak ayarlanmış olan varsayılan yeniden deneme sayısı çalışmayabilir. Bu durumda, istemci, uygulama 429 durum kodu ile CosmosException oluşturur. 

Örneği üzerinde ayarını yaparak varsayılan yeniden deneme sayısını değiştirebilirsiniz `RetryOptions` `CosmosClientOptions` . Varsayılan olarak, istek hızı üzerinde çalışmaya devam ederse, 429 durum kodu ile CosmosException, 30 saniyelik birikimli bir bekleme süresi sonra döndürülür. Bu hata, geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda, geçerli değerin varsayılan olarak 9 veya Kullanıcı tanımlı bir değer olup olmadığı halde döndürür.

Otomatik yeniden deneme davranışı çoğu uygulama için dayanıklılığı ve kullanılabilirliği artırmaya yardımcı olur. Ancak, özellikle gecikmeyi ölçmeniz durumunda performans kıyaslamaları yaparken en iyi davranış olmayabilir. Deneme sunucu tarafından azaldıysanız, istemci SDK 'sının sessizce yeniden denenmesine neden olursa istemci gözlenen gecikme süresi izlenir. Performans denemeleri sırasında gecikme sürelerini önlemek için, her bir işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılan istek oranının altında çalıştığından emin olun. Daha fazla bilgi için bkz. [Istek birimleri](request-units.md).

**Daha yüksek aktarım hızı için, daha küçük belgeler için tasarım**

Belirli bir işlemin istek ücreti (diğer bir deyişle, istek işleme maliyeti) doğrudan belgenin boyutuyla ilişkili olur. Büyük belgelerle ilgili işlemler küçük belgelerde işlemlerden daha fazla maliyetlidir.

## <a name="next-steps"></a>Sonraki adımlar
Birkaç istemci makinede yüksek performanslı senaryolar için Azure Cosmos DB değerlendirmek üzere kullanılan örnek bir uygulama için bkz. [Azure Cosmos DB Ile performans ve ölçek testi](performance-testing.md).

Uygulamanızı ölçek ve yüksek performans için tasarlama hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümleme ve ölçeklendirme](partition-data.md).
