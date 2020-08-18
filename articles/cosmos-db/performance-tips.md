---
title: .NET SDK v2 için Azure Cosmos DB performans ipuçları
description: .NET v2 SDK performansını Azure Cosmos DB iyileştirecek istemci yapılandırma seçeneklerini öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/26/2020
ms.author: sngun
ms.openlocfilehash: bc73292d7ed01468fc31e5a6203a4ba53a6425a2
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88505776"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net-sdk-v2"></a>Azure Cosmos DB ve .NET SDK v2 için performans ipuçları

> [!div class="op_single_selector"]
> * [.NET SDK v3](performance-tips-dotnet-sdk-v3-sql.md)
> * [.NET SDK v2](performance-tips.md)
> * [Java SDK’sı v4](performance-tips-java-sdk-v4-sql.md)
> * [Zaman uyumsuz Java SDK v2](performance-tips-async-java.md)
> * [Zaman uyumlu Java SDK v2](performance-tips-java.md)

Azure Cosmos DB, garantili gecikme ve verimlilik ile sorunsuz bir şekilde ölçeklenen hızlı ve esnek bir dağıtılmış veritabanıdır. Veritabanınızı Azure Cosmos DB ölçeklendirmek için önemli mimari değişiklikler yapmanız veya karmaşık kod yazmanız gerekmez. Ölçeği artırma ve azaltma, tek bir API çağrısı yapmak kadar kolaydır. Daha fazla bilgi edinmek için bkz. [kapsayıcı verimini sağlama](how-to-provision-container-throughput.md) veya [veritabanı verimini sağlama](how-to-provision-database-throughput.md). Ancak Azure Cosmos DB ağ çağrıları üzerinden erişildiği için, [SQL .NET SDK](sql-api-sdk-dotnet-standard.md)kullandığınızda en yüksek performansa ulaşmak için kullanabileceğiniz istemci tarafı iyileştirmeler vardır.

Bu nedenle, veritabanı performansını artırmaya çalışıyorsanız şu seçenekleri göz önünde bulundurun:

## <a name="upgrade-to-the-net-v3-sdk"></a>.NET v3 SDK 'ya yükseltme

[.Net v3 SDK](https://github.com/Azure/azure-cosmos-dotnet-v3) yayımlandı. .NET v3 SDK kullanıyorsanız, aşağıdaki bilgiler için [.net v3 performans Kılavuzu 'na](performance-tips-dotnet-sdk-v3-sql.md) bakın:

- Varsayılan olarak doğrudan TCP modu
- Akış API 'SI desteği
- Kullanım System.Text.JSizin vermek için özel seri hale getirici desteği
- Tümleşik toplu iş ve toplu destek

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

Çöp toplamanın sıklığını azaltmak bazı durumlarda yardımcı olabilir. .NET sürümünde [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) olarak ayarlayın `true` .

**İstemci iş yükünüzü ölçeklendirin**

Yüksek aktarım hızı düzeylerinde (50.000 RU/sn 'den fazla) test ediyorsanız, istemci uygulaması makine CPU veya ağ kullanımında kullanıma hazır hale geldiği için performans sorunlarına neden olabilir. Bu noktaya ulaştığınızda, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirerek Azure Cosmos DB hesabını daha fazla göndermeye devam edebilirsiniz.

> [!NOTE] 
> Yüksek CPU kullanımı, artan gecikme süresine ve istek zaman aşımı özel durumlarına neden olabilir.

## <a name="networking"></a><a id="networking"></a> İşlemleri

**Bağlantı ilkesi: doğrudan bağlantı modunu kullan**

İstemcinin Azure Cosmos DB 'e bağlanması, özellikle de gözlemlenen istemci tarafı gecikme süresi için önemli performans etkilerine sahiptir. İstemci bağlantı ilkesini yapılandırmak için kullanılabilecek iki temel yapılandırma ayarı vardır: bağlantı *modu* ve bağlantı *Protokolü*.  Kullanılabilir iki mod şunlardır:

  * Ağ Geçidi modu (varsayılan)
      
    Ağ Geçidi modu tüm SDK platformlarında desteklenir ve [Microsoft.Azure.DocumentDB SDK 'sı](sql-api-sdk-dotnet.md)için varsayılan olarak yapılandırılır. Uygulamanız, katı güvenlik duvarı kısıtlamalarına sahip bir kurumsal ağ içinde çalışıyorsa, standart HTTPS bağlantı noktasını ve tek bir DNS uç noktasını kullandığından, ağ geçidi modu en iyi seçimdir. Ancak performans zorunluluğunu getirir, ağ geçidi modunun, verilerin Azure Cosmos DB her okunışında veya üzerine yazıldığı her seferinde ek bir ağ atlaması içerir. Bu nedenle, daha az ağ atlaması olduğundan doğrudan mod daha iyi performans sunar. Ayrıca, sınırlı sayıda soket bağlantısı olan ortamlarda uygulamalar çalıştırdığınızda ağ geçidi bağlantı modunu da öneririz.

    Azure Işlevlerinde SDK kullandığınızda, özellikle [Tüketim planında](../azure-functions/functions-scale.md#consumption-plan), [bağlantılardaki geçerli limitlerin](../azure-functions/manage-connections.md)farkında olun. Bu durumda, Azure Işlevleri uygulamanızdaki diğer HTTP tabanlı istemcilerle de çalışıyorsanız ağ geçidi modu daha iyi olabilir.

  * Doğrudan mod

    Doğrudan mod, TCP protokolü üzerinden bağlantıyı destekler.
     
Doğrudan modda TCP kullandığınızda, ağ geçidi bağlantı noktalarına ek olarak, Azure Cosmos DB dinamik TCP bağlantı noktalarını kullandığından 10000 ve 20000 arasındaki bağlantı noktası aralığının açık olduğundan emin olmanız gerekir. [Özel uç noktalarında](./how-to-configure-private-endpoints.md)doğrudan mod KULLANıLıRKEN, TCP bağlantı noktalarının tam aralığı-0 ile 65535 arasında olmalıdır. Bu bağlantı noktaları açık değilse ve TCP protokolünü kullanmaya çalışırsanız, 503 hizmetinde kullanılamayan bir hata alırsınız. Aşağıdaki tabloda, çeşitli API 'Ler ve her API için kullanılan hizmet bağlantı noktaları için kullanılabilen bağlantı modları gösterilmektedir:

|Bağlantı modu  |Desteklenen Protokol  |Desteklenen SDK 'lar  |API/hizmet bağlantı noktası  |
|---------|---------|---------|---------|
|Ağ geçidi  |   HTTPS    |  Tüm SDK 'lar    |   SQL (443), MongoDB (10250, 10255, 10256), Table (443), Cassandra (10350), Graph (443) <br> 10250 numaralı bağlantı noktası, coğrafi çoğaltma olmadan MongoDB örneği için varsayılan Azure Cosmos DB API 'sine eşlenir. Ancak 10255 ve 10256 bağlantı noktaları, coğrafi çoğaltma içeren örnekle eşlenir.   |
|Direct    |     TCP    |  .NET SDK    | Ortak/hizmet uç noktaları kullanılırken: 10000 ile 20000 arasında bağlantı noktaları<br>Özel uç noktalar kullanılırken: 0 ila 65535 aralığındaki bağlantı noktaları |

Azure Cosmos DB, HTTPS üzerinden basit ve açık bir yeniden programlama modeli sunar. Ayrıca, bu, iletişim modelinde da daha da fazla olan ve .NET istemci SDK 'Sı aracılığıyla kullanılabilen etkin bir TCP protokolünü sunmaktadır. TCP protokolü ilk kimlik doğrulaması ve trafiği şifreleme için TLS kullanır. En iyi performans için, mümkün olduğunda TCP protokolünü kullanın.

Microsoft.Azure.DocumentDB SDK 'Sı için, parametresini kullanarak, örneği oluşturma sırasında bağlantı modunu yapılandırırsınız `DocumentClient` `ConnectionPolicy` . Doğrudan modu kullanırsanız, `Protocol` parametresini kullanarak da ayarlayabilirsiniz `ConnectionPolicy` .

```csharp
Uri serviceEndpoint = new Uri("https://contoso.documents.net");
string authKey = "your authKey from the Azure portal";
DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct, // ConnectionMode.Gateway is the default
   ConnectionProtocol = Protocol.Tcp
});
```

TCP yalnızca doğrudan modunda desteklendiğinden, ağ geçidi modunu kullanıyorsanız, HTTPS protokolü her zaman ağ geçidiyle iletişim kurmak için kullanılır ve `Protocol` içindeki değeri `ConnectionPolicy` yok sayılır.

:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Azure Cosmos DB bağlantı ilkesi" border="false":::

**Kısa ömürlü bağlantı noktası tükenmesi**

Örneklerinizi üzerinde yüksek bir bağlantı birimi veya yüksek bağlantı noktası kullanımı görürseniz, önce istemci örneklerinizin tekton olduğunu doğrulayın. Diğer bir deyişle, istemci örneklerinin uygulamanın ömrü için benzersiz olması gerekir.

TCP protokolünde çalışırken, istemci, uzun süreli bağlantıları kullanarak gecikme süresini en iyi duruma getirir ve bu da, 2 dakikadan sonra bağlantıları sonlandırır.

Seyrek erişiminizin olduğu senaryolarda ve ağ geçidi modu erişimiyle karşılaştırıldığında daha yüksek bir bağlantı sayısı fark ederseniz şunları yapabilirsiniz:

* [Connectionpolicy. PortReuseMode](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.portreusemode) özelliğini ' e yapılandırın `PrivatePortPool` (Framework sürümü>= 4.6.1 ve .net Core sürümü >= 2,0): Bu özellik SDK 'nın farklı Azure Cosmos DB hedef uç noktaları için kısa ömürlü bağlantı noktası havuzu kullanmasına izin verir.
* [Connectionpolicy. ıdıdconnectiontimeout](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.idletcpconnectiontimeout) özelliği 10 dakikadan büyük veya buna eşit olmalıdır. Önerilen değerler 20 dakika ile 24 saat arasında.

**İlk istekte başlangıç gecikmesini önlemek için OpenAsync çağrısı yapın**

Varsayılan olarak, ilk isteğin, adres yönlendirme tablosunu getirmesi gerektiğinden gecikme süresi daha yüksektir. [SDK v2](sql-api-sdk-dotnet.md)'yi kullandığınızda, `OpenAsync()` ilk istekte bu başlangıç gecikmesini önlemek için başlatma sırasında bir kez çağrı yapın. Çağrı şöyle görünür: `await client.OpenAsync();`

> [!NOTE]
> `OpenAsync` , hesaptaki tüm kapsayıcılar için adres yönlendirme tablosunu elde etmek üzere istek üretir. Çok sayıda kapsayıcı içeren, ancak uygulaması bunların bir alt kümesine eriştiği hesaplar için, `OpenAsync` başlatmayı yavaş hale getirmek için gereksiz bir trafik miktarı oluşturur. Bu nedenle `OpenAsync` , uygulama başlangıcını yavaşladığı için kullanmak Bu senaryoda yararlı olmayabilir.

**Performans için aynı Azure bölgesindeki istemcileri birlikte bulun**

Mümkün olduğunda, Azure Cosmos DB veritabanıyla aynı bölgedeki Azure Cosmos DB çağıran tüm uygulamaları yerleştirin. İşte yaklaşık bir karşılaştırma: aynı bölgedeki Azure Cosmos DB için yapılan çağrılar 1 MS ile 2 ms arasında tamamlanır, ancak ABD 'nin batı ve Doğu yakası arasındaki gecikme 50 MS 'den fazla olur. Bu gecikme, istemciden Azure veri merkezi sınırına geçerken istek tarafından alınan yola bağlı olarak istek üzerine değişiklik gösterebilir. Çağıran uygulamanın, sağlanan Azure Cosmos DB uç noktası ile aynı Azure bölgesinde yer almasını sağlayarak olası en düşük gecikme süresini sağlayabilirsiniz. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

:::image type="content" source="./media/performance-tips/same-region.png" alt-text="Azure Cosmos DB bağlantı ilkesi" border="false":::

**İş parçacığı/görev sayısını artırma**
<a id="increase-threads"></a>

Azure Cosmos DB çağrıları ağ üzerinden yapıldığından, istemci uygulamanın istekler arasında en az zaman harcamasını sağlamak için isteklerinizin paralellik derecesini değiştirmeniz gerekebilir. Örneğin, .NET [görev paralel kitaplığı](https://msdn.microsoft.com//library/dd460717.aspx)kullanıyorsanız, Azure Cosmos DB okuyan veya üzerine yazılan yüzlerce görev sırasını oluşturun.

**Hızlandırılmış ağı etkinleştir**
 
Gecikme süresi ve CPU değişimi azaltmak için istemci sanal makinelerinde hızlandırılmış ağı etkinleştirmenizi öneririz. Bkz. [hızlandırılmış ağ Ile Windows sanal makinesi oluşturma](../virtual-network/create-vm-accelerated-networking-powershell.md) veya [hızlandırılmış ağ ile Linux sanal makinesi oluşturma](../virtual-network/create-vm-accelerated-networking-cli.md).

## <a name="sdk-usage"></a>SDK kullanımı

**En son SDK 'Yı yükler**

Azure Cosmos DB SDK 'Ları, en iyi performansı sağlamak için sürekli geliştirilmiştir. En son SDK 'Yı öğrenmek ve geliştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](sql-api-sdk-dotnet-standard.md) sayfalarına bakın.

**Uygulamanızın ömrü boyunca tek bir Azure Cosmos DB istemcisi kullanın**

Her `DocumentClient` örnek iş parçacığı açısından güvenlidir ve doğrudan modda çalışırken verimli bağlantı yönetimi ve adres önbelleği gerçekleştirir. Etkili bağlantı yönetimine ve daha iyi SDK istemci performansına izin vermek için, `AppDomain` uygulamanın kullanım ömrü boyunca tek bir örnek kullanmanızı öneririz.

**Ağ Geçidi modu kullanırken konak başına System.Net MaxConnections 'yi artırma**

Ağ Geçidi modunu kullandığınızda Azure Cosmos DB istekleri HTTPS/REST üzerinden yapılır. Tabi, ana bilgisayar adı veya IP adresi başına varsayılan bağlantı sınırına göre yapılır. `MaxConnections`İstemci kitaplığının Azure Cosmos DB için birden çok eş zamanlı bağlantı kullanabilmesi için daha yüksek bir değere (100 'e 1.000) ayarlamanız gerekebilir. .NET SDK 1.8.0 ve üzeri sürümlerde, [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) için varsayılan değer 50 ' dir. Değeri değiştirmek için [belgeler. Client. ConnectionPolicy. MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) değerini daha yüksek bir değere ayarlayabilirsiniz.

**Bölümlenmiş koleksiyonlar için Paralel sorguları ayarlama**

SQL .NET SDK 1.9.0 ve üzeri, bölümlenmiş bir koleksiyonu paralel olarak sorgulamanızı sağlayan paralel sorguları destekler. Daha fazla bilgi için bkz. SDK 'lar ile çalışma ile ilgili [kod örnekleri](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) . Paralel sorgular, seri karşılığından daha iyi sorgu gecikmesi ve verimlilik sağlamak üzere tasarlanmıştır. Paralel sorgular, gereksinimlerinize uyacak şekilde ayarlayabilmeniz için iki parametre sağlar: 
- `MaxDegreeOfParallelism` paralel olarak sorgulanabilecek en fazla bölüm sayısını denetler. 
- `MaxBufferedItemCount` önceden getirilen sonuçların sayısını denetler.

***Paralellik derecesi ayarlama***

Paralel sorgu birden çok bölümü paralel olarak sorgulayarak işe yarar. Ancak tek bir bölümden alınan veriler sorguya göre işlem içine alınır. `MaxDegreeOfParallelism` [SDK v2](sql-api-sdk-dotnet.md) 'nin bölüm sayısına en iyi şekilde ayarlanması, diğer tüm sistem koşullarının aynı kalması şartıyla en iyi performansı elde etmek için en iyi şansınız vardır. Bölüm sayısını bilmiyorsanız paralellik derecesini yüksek bir sayı olarak ayarlayabilirsiniz. Sistem paralellik derecesi olarak en az (bölüm sayısını, Kullanıcı tarafından girilen girişi) seçer.

Paralel sorgular, verilerin sorguya göre tüm bölümler arasında eşit bir şekilde dağıtılması halinde en avantaja sahip olur. Bölümlenmiş koleksiyon, bir sorgu tarafından döndürülen verilerin tümünün veya çoğu birkaç bölümde yoğunlaşarak (bir bölüm en kötü durumdur), bu bölümlerin sorgunun performansını performans sorunlarına neden olur.

***MaxBufferedItemCount ayarlama***
    
Paralel sorgu, geçerli sonuç toplu işi istemci tarafından işlendiği sırada sonuçları önceden getirmek üzere tasarlanmıştır. Bu önceden getirme, bir sorgunun genel gecikmesinin artırılmasına yardımcı olur. `MaxBufferedItemCount`Parametresi, önceden getirilen sonuçların sayısını sınırlar. `MaxBufferedItemCount`Sorgunun ön getirmeyi en fazla avantaj almasına izin vermek için beklenen sonuç sayısı (veya daha yüksek bir sayı) olarak ayarlayın.

Önceden getirme, paralellik derecesi ne olursa olsun aynı şekilde çalışacaktır ve tüm bölümlerdeki veriler için tek bir arabellek vardır.  

**RetryAfter aralıklarında geri alma Uygula**

Performans testi sırasında, küçük bir istek hızı kısıtlanana kadar yükü artırmanız gerekir. İstekler kısıtlanmamışsa, sunucu tarafından belirtilen yeniden deneme aralığı için istemci uygulamanın azaltılmasından sonra kapatılması gerekir. Geri alma işleminin tamamlanması, yeniden denemeler arasında bekleyen en az miktarda süre harcamanızı sağlar. 

Yeniden deneme ilkesi desteği Bu SDK 'lara dahildir:
- [SQL için .NET SDK](sql-api-sdk-dotnet.md) ve [SQL için Java SDK](sql-api-sdk-java.md) 'nın sürüm 1.8.0 ve üzeri
- [SQL içinNode.js SDK](sql-api-sdk-node.md) ve [SQL için Python SDK](sql-api-sdk-python.md) 'nın sürümü 1.9.0 ve üzeri
- [.NET Core](sql-api-sdk-dotnet-core.md) SDK 'larının tüm desteklenen sürümleri 

Daha fazla bilgi için bkz. [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
    
.NET SDK 'nın 1,19 ve sonraki sürümlerinde, aşağıdaki örnekte gösterildiği gibi ek tanılama bilgileri ve sorun giderme gecikme sorunlarını günlüğe kaydetme mekanizması vardır. Daha yüksek okuma gecikmesi olan istekler için tanılama dizesini günlüğe kaydedebilirsiniz. Yakalanan tanılama dizesi, belirli bir istek için kaç kez 429 hatası aldığınızı anlamanıza yardımcı olur.

```csharp
ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
readDocument.RequestDiagnosticsString 
```

**Daha düşük okuma gecikmesi için önbellek belgesi URI 'Leri**

En iyi okuma performansı için mümkün olduğunda önbellek belgesi URI 'Leri. Kaynak oluşturduğunuzda kaynak KIMLIĞINI önbelleğe almak için mantığı tanımlamanız gerekir. Kaynak kimliklerini temel alan aramalar, ad tabanlı aramalardan daha hızlıdır, bu nedenle bu değerlerin önbelleğe alınması performansı geliştirir.

**Daha iyi performans için, sorguların/okunan akışların sayfa boyutunu ayarlayın**

Belgeleri okuma akışı işlevini (örneğin,) kullanarak toplu okuma işlemi yaptığınızda `ReadDocumentFeedAsync` veya BIR SQL sorgusu verdiğinizde, sonuç kümesi çok büyükse sonuçlar, bölümlenmiş bir biçimde döndürülür. Varsayılan olarak, sonuçlar 100 öğe veya 1 MB Öbekle döndürülür, bu sınır ilk önce dönüştürülür.

Tüm geçerli sonuçları almak için gereken ağ gidiş dönüşlerin sayısını azaltmak için, en fazla 1.000 üst bilgi istemek üzere [x-MS-Max-item-Count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) kullanarak sayfa boyutunu artırabilirsiniz. Örneğin, Kullanıcı arabirimi veya uygulama API 'niz aynı anda yalnızca 10 sonuç döndürürse, okuma ve sorgular için tüketilen aktarım hızını azaltmak için sayfa boyutunu 10 ' a da azaltabilirsiniz.

> [!NOTE] 
> `maxItemCount`Özelliği yalnızca sayfalandırma için kullanılmamalıdır. Ana kullanımı, tek bir sayfada döndürülen en fazla öğe sayısını azaltarak sorguların performansını artırmaktır.  

Ayrıca, kullanılabilir Azure Cosmos DB SDK 'larını kullanarak sayfa boyutunu ayarlayabilirsiniz. İçindeki [Maxıtemcount](/dotnet/api/microsoft.azure.documents.client.feedoptions.maxitemcount?view=azure-dotnet) özelliği, `FeedOptions` sabit listesi işleminde döndürülecek en fazla öğe sayısını ayarlamanıza olanak sağlar. `maxItemCount`-1 olarak ayarlandığında, SDK, belge boyutuna bağlı olarak en uygun değeri otomatik olarak bulur. Örneğin:
    
```csharp
IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
```
    
Bir sorgu yürütüldüğünde, sonuçta elde edilen veriler bir TCP paketi içinde gönderilir. İçin çok düşük bir değer belirtirseniz `maxItemCount` , VERILERI TCP paketi içinde göndermek için gereken gidiş sayısı yüksek olur ve bu da performansı etkiler. Bu nedenle, özelliği için hangi değeri `maxItemCount` ayarlayabileceğinize emin değilseniz, bunu-1 olarak ayarlamak ve SDK 'nın varsayılan değeri seçmesini sağlamak en iyisidir.

**İş parçacığı/görev sayısını artırma**

Bkz. Bu makalenin ağ bölümünde [iş parçacığı sayısı/görevler sayısını artırın](#increase-threads) .

## <a name="indexing-policy"></a>Dizin oluşturma ilkesi
 
**Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

Azure Cosmos DB Dizin oluşturma ilkesi, dizin oluşturma yollarını (ındexingpolicy. ıncludedpaths ve ındexingpolicy. ExcludedPaths) kullanarak hangi belge yollarının dizine dahil edileceğini veya bu dizinden dışlanacağını belirtmenize de olanak tanır. Dizin oluşturma yolları yazma performansını iyileştirebilir ve sorgu desenlerinin önceden bildiği senaryolar için Dizin depolamayı azaltabilir. Bunun nedeni, dizin oluşturma maliyetlerinin doğrudan dizin oluşturulan benzersiz yolların sayısıyla ilişkilendirilmesi. Örneğin, bu kod, "*" joker karakterini kullanarak, belgelerin tamamını (bir alt ağacı) dizin oluşturma işleminden nasıl dışarıda bırakakullanacağınızı gösterir:

```csharp
var collection = new DocumentCollection { Id = "excludedPathCollection" };
collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);
```

Daha fazla bilgi için bkz. [Azure Cosmos DB Dizin oluşturma ilkeleri](index-policy.md).

## <a name="throughput"></a><a id="measure-rus"></a> Trafiği

**Düşük Istek birimleri/ikinci kullanım için ölçme ve ayarlama**

Azure Cosmos DB zengin bir veritabanı işlemleri kümesi sunar. Bu işlemler, bir veritabanı koleksiyonu içindeki belgeler üzerinde çalışan UDF 'ler, saklı yordamlar ve tetikleyicilerle ilişkisel ve hiyerarşik sorgular içerir. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi gerçekleştirmek için gereken CPU, GÇ ve belleğe göre değişir. Donanım kaynaklarını düşünmek ve yönetmek yerine, çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet vermek için gereken kaynaklar için bir Istek birimi (RU) tek bir ölçü olarak düşünebilirsiniz.

Aktarım hızı, her bir kapsayıcı için ayarlanan [Istek birimi](request-units.md) sayısına göre sağlanır. İstek birimi tüketimi, saniye başına bir hız olarak değerlendirilir. Kapsayıcı için sağlanan Istek birimi oranını aşan uygulamalar, oran için sağlanan düzeyin altına düşene kadar sınırlandırılır. Uygulamanız daha yüksek bir işleme düzeyi gerektiriyorsa, ek Istek birimleri sağlayarak aktarım hızını artırabilirsiniz.

Bir sorgunun karmaşıklığı, bir işlem için kaç Istek biriminin tüketildiğini etkiler. Koşulların sayısı, koşulların doğası, UDF sayısı ve kaynak veri kümesinin boyutu, sorgu işlemlerinin maliyetini etkiler.

Herhangi bir işlemin (oluşturma, güncelleştirme veya silme) yükünü ölçmek için, [x-ms-request-charge](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) `RequestCharge` `ResourceResponse\<T>` `FeedResponse\<T>` işlemler tarafından tüketilen istek birimlerinin sayısını ölçmek üzere x-MS-Request-ücret üst bilgisini (veya .NET SDK içindeki veya içinde eşdeğer özelliği) inceleyin:

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

Bu üst bilgide döndürülen istek ücreti, sağlanan aktarım hızını (yani, 2.000 ru/saniye) bir kesri. Örneğin, yukarıdaki sorgu 1.000 1 KB 'lik belgeler döndürürse, işlemin maliyeti 1.000 ' dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki istek olduğunu kabul eder. Daha fazla bilgi için bkz. [Istek birimleri](request-units.md) ve [istek birimi Hesaplayıcısı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>

**Tanıtıcı hız sınırlandırma/istek hızı çok büyük**

Bir istemci, bir hesap için ayrılan aktarım hızını aşmaya çalıştığında, sunucuda bir performans düşüşü olmaz ve ayrılan düzeyin ötesinde üretilen iş kapasitesi kullanılamaz. Sunucu isteği RequestRateTooLarge ile sona erdirmek için preemptively (HTTP durum kodu 429). Kullanıcının isteği yeniden denemeden önce beklemesi gereken süre miktarını milisaniye cinsinden belirten bir [x-MS-retry-After-MS](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üst bilgisi döndürür.

```http
HTTP Status 429,
Status Line: RequestRateTooLarge
x-ms-retry-after-ms :100
```

SDK 'lar, sunucu tarafından belirtilen yeniden deneme üst bilgisine göre bu yanıtı dolaylı olarak yakalar ve isteği yeniden dener. Hesabınız birden çok istemci tarafından aynı anda erişilmediği takdirde, sonraki yeniden deneme başarılı olur.

İstek hızının sürekli olarak birden fazla istemciniz varsa, bu, şu anda istemci tarafından dahili olarak 9 olarak ayarlanmış olan varsayılan yeniden deneme sayısı çalışmayabilir. Bu durumda, istemci uygulamaya 429 durum kodu ile bir DocumentClientException oluşturur. 

Örneği üzerinde ayarını yaparak varsayılan yeniden deneme sayısını değiştirebilirsiniz `RetryOptions` `ConnectionPolicy` . Varsayılan olarak, durum kodu 429 olan DocumentClientException, istek istek hızının üzerinde çalışmaya devam ederse, 30 saniyelik birikimli bir bekleme süresi dolduktan sonra döndürülür. Bu hata, geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda, geçerli değerin varsayılan olarak 9 veya Kullanıcı tanımlı bir değer olup olmadığı halde döndürür.

Otomatik yeniden deneme davranışı çoğu uygulama için dayanıklılığı ve kullanılabilirliği artırmaya yardımcı olur. Ancak, özellikle gecikmeyi ölçmeniz durumunda performans kıyaslamaları yaparken en iyi davranış olmayabilir. Deneme sunucu tarafından azaldıysanız, istemci SDK 'sının sessizce yeniden denenmesine neden olursa istemci gözlenen gecikme süresi izlenir. Performans denemeleri sırasında gecikme sürelerini önlemek için, her bir işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılan istek oranının altında çalıştığından emin olun. Daha fazla bilgi için bkz. [Istek birimleri](request-units.md).

**Daha yüksek aktarım hızı için, daha küçük belgeler için tasarım**

Belirli bir işlemin istek ücreti (diğer bir deyişle, istek işleme maliyeti) doğrudan belgenin boyutuyla ilişkili olur. Büyük belgelerle ilgili işlemler küçük belgelerde işlemlerden daha fazla maliyetlidir.

## <a name="next-steps"></a>Sonraki adımlar

Birkaç istemci makinede yüksek performanslı senaryolar için Azure Cosmos DB değerlendirmek üzere kullanılan örnek bir uygulama için bkz. [Azure Cosmos DB Ile performans ve ölçek testi](performance-testing.md).

Uygulamanızı ölçek ve yüksek performans için tasarlama hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümleme ve ölçeklendirme](partition-data.md).
