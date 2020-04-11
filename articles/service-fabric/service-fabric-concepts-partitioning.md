---
title: Bölümleme Servisi Kumaş hizmetleri
description: Service Fabric'in stateful hizmetlerinin nasıl bölümlenebildiğini açıklar. Bölümler yerel makinelerde veri depolamasağlar, böylece veri ve bilgi işlem birlikte ölçeklenebilir.
ms.topic: conceptual
ms.date: 06/30/2017
ms.openlocfilehash: 4edfaa74fe109c688cad733d16031e87fff1e46f
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115157"
---
# <a name="partition-service-fabric-reliable-services"></a>Partition Service Fabric güvenilir hizmetleri
Bu makalede, Azure Hizmet Kumaşı güvenilir hizmetlerini bölümlemenin temel kavramlarına giriş sağlanmaktadır. Makalede kullanılan kaynak kodu [GitHub'da](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions)da mevcuttur.

## <a name="partitioning"></a>Bölümleme
Bölümleme, Service Fabric'e özgü değildir. Aslında, ölçeklenebilir hizmetler bina bir çekirdek desendir. Daha geniş anlamda, ölçeklenebilirlik ve performansı artırmak için durumu (verileri) bölme ve hesaplama yı daha küçük erişilebilir birimlere bölme kavramı olarak bölümleme hakkında düşünebiliriz. Bölümlemenin iyi bilinen bir biçimi, parçalama olarak da bilinen [veri bölümlemedir.][wikipartition]

### <a name="partition-service-fabric-stateless-services"></a>Bölüm Servisi Kumaş stateless hizmetleri
Durum disun hizmetler için, bir bölümün bir hizmetin bir veya daha fazla örneğini içeren mantıksal bir birim olduğunu düşünebilirsiniz. Şekil 1, bir küme üzerinde tek bir bölüm kullanılarak dağıtılan beş örneği olan devletsiz bir hizmeti gösterir.

![Devletsiz hizmet](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Gerçekten iki tür devletsiz hizmet çözümleri vardır. Bunlardan ilki, örneğin bir Azure SQL veritabanında (oturum bilgilerini ve verilerini depolayan bir web sitesi gibi) durumunu dışarıdan devam eden bir hizmettir. İkincisi, herhangi bir kalıcı durumu yönetmeyen yalnızca hesaplama hizmetleridir (hesap makinesi veya resim küçük resmi gibi).

Her iki durumda da, bir devletsiz hizmet bölümleme çok nadir bir senaryodur - ölçeklenebilirlik ve kullanılabilirlik normalde daha fazla örnek ekleyerek elde edilir. Durum dışı hizmet örnekleri için birden çok bölümü göz önünde bulundurmak istediğiniz tek zaman, özel yönlendirme isteklerini karşılamanız gerektiği zamandır.

Örnek olarak, belirli bir aralıkta olan kullanıcılara yalnızca belirli bir hizmet örneği tarafından sunulması gereken bir durum düşünün. Bir hizmeti ne zaman bölümlere alabildiğinizin bir başka örneği de, gerçekten bölümlenmiş bir arka uca sahip olduğunuzda (örn. kırık bir SQL veritabanı) ve hangi hizmet örneğinin veritabanı na yazılması gerektiğini denetlemek veya arka uçta kullanılan aynı bölümleme bilgilerini gerektiren devletsiz hizmet içinde diğer hazırlık işlerini gerçekleştirmek tir. Bu tür senaryolar da farklı şekillerde çözülebilir ve mutlaka hizmet bölümleme gerektirmez.

Bu gözden geçirmenin geri kalanı devlet hizmetlerine odaklanır.

### <a name="partition-service-fabric-stateful-services"></a>Bölüm Hizmeti Kumaş stateful hizmetleri
Service Fabric, bölüm durumu (veri) için birinci sınıf bir yol sunarak ölçeklenebilir stateful hizmetler geliştirmeyi kolaylaştırır. Kavramsal olarak, bir kümedeki düğümler arasında dağıtılan ve dengelenmiş [yinelemeler](service-fabric-availability-services.md) aracılığıyla son derece güvenilir bir ölçek birimi olarak durumlu bir hizmetin bölünmesi ni düşünebilirsiniz.

Hizmet Kumaşı devlet hizmetleri bağlamında bölümleme, belirli bir hizmet bölümünün hizmetin tam durumunun bir bölümünden sorumlu olduğunu belirleme işlemini ifade eder. (Daha önce de belirtildiği gibi, bir bölüm [yinelemeler](service-fabric-availability-services.md)kümesidir). Service Fabric ile ilgili harika bir şey, bölümleri farklı düğümlere yerleştirir. Bu, düğümün kaynak sınırına kadar büyümelerini sağlar. Veri gereksinimleri büyüdükçe, bölümler büyür ve Service Fabric düğümler arasında bölümleri yeniden dengeler. Bu, donanım kaynaklarının sürekli verimli kullanımını sağlar.

Size bir örnek vermek gerekirse, 5 düğümlü küme ve 10 bölüm ve üç yinelemehedefi olacak şekilde yapılandırılan bir hizmetle başladığınızı varsayın. Bu durumda, Hizmet Kumaşı yinelemeleri küme boyunca dengeler ve dağıtır ve düğüm başına iki birincil [yinelemeyle](service-fabric-availability-services.md) sona erer.
Şimdi kümeyi 10 düğüme ölçeklendirmeniz gerekiyorsa, Hizmet Kumaşı birincil [yinelemeleri](service-fabric-availability-services.md) 10 düğümün tümünde yeniden dengeler. Aynı şekilde, 5 düğüme küçülttüyseniz, Service Fabric 5 düğümdeki tüm kopyaları yeniden dengeler.  

Şekil 2, kümenin ölçeklenmeden önce ve sonra 10 bölüm dağılımını gösterir.

![Devlet hizmeti](./media/service-fabric-concepts-partitioning/partitions.png)

Sonuç olarak, istemcilerden gelen istekler bilgisayarlara dağıtıldığından, uygulamanın genel performansı artırıldığından ve veri yığınlarına erişim konusundaki çekişme azaltıldığı için ölçeklendirme sağlanır.

## <a name="plan-for-partitioning"></a>Bölümleme planı
Bir hizmeti uygulamadan önce, ölçeklendirmestratejisini her zaman göz önünde bulundurmalısınız. Farklı yolları vardır, ama hepsi uygulama ulaşmak için gerekenlere odaklanmak. Bu makalenin içeriği için, bazı daha önemli yönlerini ele alalım.

İyi bir yaklaşım, ilk adım olarak, bölünmesi gereken devletin yapısı hakkında düşünmektir.

Basit bir örnek verelim. Eğer ilçe çapında bir anket için bir hizmet oluşturmak için olsaydı, ilçede her şehir için bir bölüm oluşturabilirsiniz. O zaman, şehirdeki herkes için oyları o şehre karşılık gelen bölüme saklayabilirsin. Şekil 3, bir dizi insanı ve içinde ikamet ettikleri şehri göstermektedir.

![Basit bölümleme](./media/service-fabric-concepts-partitioning/cities.png)

Şehirlerin nüfusu büyük ölçüde değiştiğinden, çok fazla veri içeren bazı bölümler (örneğin Seattle) ve çok az devlete (örneğin Kirkland) sahip diğer bölümlerle karşılaşabilirsiniz. Peki devlet düzensiz miktarda bölünmeler sahip etkisi nedir?

Örneği tekrar düşünürseniz, Seattle'ın oylarını elinde bulunduran bölümün Kirkland'dan daha fazla trafik alacağını kolayca görebilirsiniz. Varsayılan olarak, Hizmet Kumaşı her düğümde yaklaşık aynı sayıda birincil ve ikincil yineleme olduğundan emin olur. Böylece daha fazla trafik ve daha az trafik hizmet diğerleri hizmet yinelemeler tutan düğümleri ile sona erebilir. Tercihen bir küme de bu gibi sıcak ve soğuk noktalar önlemek isterdim.

Bunu önlemek için, bölümleme açısından iki şey yapmalısınız:

* Tüm bölümlerarasında eşit olarak dağıtılacak şekilde durumu bölmeye çalışın.
* Hizmet için yinelemelerin her birinden gelen yükü bildirin. (Nasıl olduğu hakkında bilgi için, [Ölçümler ve Yük](service-fabric-cluster-resource-manager-metrics.md)ile ilgili bu makaleye göz atın). Service Fabric, bellek miktarı veya kayıt sayısı gibi hizmetler tarafından tüketilen yükü bildirme olanağı sağlar. Bildirilen ölçümlere bağlı olarak, Service Fabric bazı bölümlerin diğerlerinden daha yüksek yüklerde hizmet verdiğini algılar ve genel düğümün aşırı yüklenmesi için yinelemeleri daha uygun düğümlere taşıyarak kümeyi yeniden dengeler.

Bazen, belirli bir bölümde ne kadar veri olacağını bilemezsiniz. Bu nedenle genel bir öneri, her ikisini de yapmaktır-- birincisi, verileri bölümler arasında eşit olarak yayan bir bölümleme stratejisi benimseyerek ve ikincisini, yükü bildirerek.  İlk yöntem oylama örneğinde açıklanan durumları önlerken, ikincisi zaman içinde erişim veya yüklemedeki geçici farklılıkları ndüzeltilmesine yardımcı olur.

Bölüm planlamanın bir diğer yönü de başlangıçta doğru bölüm sayısını seçmektir.
Service Fabric perspektifinden, senaryonuz için beklenenden daha fazla sayıda bölümle başlamanızı engelleyen hiçbir şey yoktur.
Aslında, bölüm sayısının geçerli bir yaklaşım olduğunu varsayarsak.

Nadir durumlarda, başlangıçta seçtiğinizden daha fazla bölüme ihtiyacınız olabilir. Olaydan sonra bölüm sayısını değiştiremeyeceğiniz için, aynı hizmet türünde yeni bir hizmet örneği oluşturmak gibi bazı gelişmiş bölüm yaklaşımları uygulamanız gerekir. Ayrıca, istemci kodunun tutması gereken istemci tarafı bilgisine dayanarak istekleri doğru hizmet örneğine yönlendirir bazı istemci tarafı mantığı uygulamanız gerekir.

Bölümleme planlama için başka bir husus kullanılabilir bilgisayar kaynaklarıdır. Devlete erişilmesi ve depolanması gerektiğinden, aşağıdakileri izlemeniz gerekir:

* Ağ bant genişliği sınırları
* Sistem bellek sınırları
* Disk depolama sınırları

Çalışan bir kümede kaynak kısıtlamalarıyla karşınıza çıkarsa ne olur? Yanıt, kümeyi yeni gereksinimleri karşılamak için ölçeklendirebilirsiniz.

[Kapasite planlama kılavuzu,](service-fabric-capacity-planning.md) küme nizin kaç düğüm egereksinimlerini belirlemek için kılavuz sunar.

## <a name="get-started-with-partitioning"></a>Bölümleme ile başlayın
Bu bölümde, hizmetinizi bölmeye nasıl başlanırsınız.

Service Fabric üç bölüm düzeni seçeneği sunar:

* Aralıklı bölümleme (diğer adıyla UniformInt64Partition).
* Bölümleme adlı. Bu modeli kullanan uygulamalar genellikle sınırlı bir küme içinde kovalanabilir verilere sahiptir. Adlandırılmış bölüm anahtarları olarak kullanılan veri alanlarının bazı yaygın örnekleri bölgeler, posta kodları, müşteri grupları veya diğer iş sınırları dır.
* Singleton bölümleme. Tekton bölümleri genellikle hizmet herhangi bir ek yönlendirme gerektirmediğinde kullanılır. Örneğin, devlet ifadebilmeyen hizmetler varsayılan olarak bu bölümleme düzenini kullanır.

Adlandırılmış ve Singleton bölümleme şemaları aralıklı bölümlerin özel biçimleridir. Varsayılan olarak, Service Fabric için Visual Studio şablonları, en yaygın ve kullanışlı olan olduğu için aralıklı bölümleme kullanır. Bu makalenin geri kalanı aralıklı bölümleme şeması üzerinde duruluyor.

### <a name="ranged-partitioning-scheme"></a>Aralıklı bölümleme şeması
Bu, bir tamsayı aralığı (düşük anahtar ve yüksek anahtarla tanımlanır) ve bir dizi bölüm (n) belirtmek için kullanılır. Her biri genel bölüm anahtar aralığının çakışmayan alt aralığından sorumlu olan n bölümleri oluşturur. Örneğin, düşük anahtar 0, 99 yüksek anahtar ve 4 sayısı ile aralıklı bir bölümleme düzeni aşağıda gösterildiği gibi dört bölüm oluşturur.

![Aralık bölümleme](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Ortak bir yaklaşım, veri kümesi içinde benzersiz bir anahtara dayalı bir karma oluşturmaktır. Anahtarların bazı yaygın örnekleri bir araç kimlik numarası (VIN), bir çalışan kimliği veya benzersiz bir dize olacaktır. Bu benzersiz anahtarı kullanarak, anahtarınız olarak kullanmak üzere bir karma kod, anahtar aralığı modül oluşturursunuz. İzin verilen anahtar aralığının üst ve alt sınırlarını belirtebilirsiniz.

### <a name="select-a-hash-algorithm"></a>Karma algoritma seçin
Karma algoritmanızı seçmenin önemli bir bölümü karma algoritmanızı seçmektir. Amaç, benzer anahtarları birbirine yakın gruplandırmak mı (yerelliğe duyarlı karma) veya etkinliğin daha yaygın olan tüm bölümlere (dağıtım karma) genel olarak dağıtılması mı gerektiğidir.

İyi bir dağıtım karma algoritmasının özellikleri, hesaplamanın kolay olması, birkaç çakışması olması ve anahtarları eşit olarak dağıtmasıdır. Verimli karma algoritmasının iyi bir örneği [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) karma algoritmasıdır.

Genel karma kod algoritması seçimleri için iyi bir kaynak [karma işlevleri Vikipedi sayfasıdır.](https://en.wikipedia.org/wiki/Hash_function)

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Birden çok bölümiçeren bir devlet hizmeti oluşturma
Birden çok bölümlü ilk güvenilir durum hizmetinizi oluşturalım. Bu örnekte, aynı harfle başlayan tüm soyadlarını aynı bölümde depolamak istediğiniz çok basit bir uygulama oluşturursunuz.

Herhangi bir kod yazmadan önce, bölümleri ve bölüm tuşları hakkında düşünmek gerekir. 26 bölüm (alfabede her harf için bir tane) gerekir, ama ne düşük ve yüksek tuşları hakkında?
Biz kelimenin tam anlamıyla mektup başına bir bölüm olmasını istediğiniz gibi, her harf kendi anahtarı olduğu gibi, düşük anahtar olarak 0 ve yüksek anahtar olarak 25 kullanabilirsiniz.

> [!NOTE]
> Bu basitleştirilmiş bir senaryodur, gerçekte dağıtım eşit olacaktır gibi. "S" veya "M" harfleriyle başlayan soyadlar, "X" veya "Y" harfleriyle başlayan adlardan daha yaygındır.
> 
> 

1. Açık **Görsel Studio** > **Dosya** > **Yeni** > **Proje**.
2. Yeni **Proje** iletişim kutusunda, Hizmet Kumaşı uygulamasını seçin.
3. Projeye "Alfabe Bölümleri" deyin.
4. Hizmet **Oluştur** iletişim kutusunda, **Stateful** hizmetini seçin ve "Alphabet.Processing" adını kullanın.
5. Bölüm sayısını ayarlayın. AlphabetPartitions projesinin ApplicationPackageRoot klasöründe bulunan Applicationmanifest.xml dosyasını açın ve Processing_PartitionCount parametresini aşağıda gösterildiği gibi 26'ya güncelleyin.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Ayrıca, aşağıda gösterildiği gibi ApplicationManifest.xml'deki StatefulService öğesinin LowKey ve HighKey özelliklerini de güncellemeniz gerekir.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Hizmetin erişilebilir olması için, aşağıda gösterildiği gibi Alphabet.Processing hizmeti için ServiceManifest.xml'in (PackageRoot klasöründe bulunan) bitiş noktası öğesini ekleyerek bir bağlantı noktasında bir bitiş noktası açın:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Şimdi hizmet 26 bölümlü bir dahili bitiş noktasını dinleyecek şekilde yapılandırıldı.
7. Ardından, İşlemsınıfının yöntemini `CreateServiceReplicaListeners()` geçersiz kılmanız gerekir.
   
   > [!NOTE]
   > Bu örnek için, basit bir HttpCommunicationListener kullandığınızı varsayıyoruz. Güvenilir hizmet iletişimi hakkında daha fazla bilgi için [Güvenilir Hizmet iletişim modeline](service-fabric-reliable-services-communication.md)bakın.
   > 
   > 
8. Yinelemenin dinlediği URL için önerilen desen aşağıdaki biçimdir: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Bu nedenle, iletişim dinleyicinizi doğru uç noktalarda ve bu desenle dinleyecek şekilde yapılandırmak istiyorsunuz.
   
    Bu hizmetin birden çok yinelemesi aynı bilgisayarda barındırılabilir, bu nedenle bu adresin yinelemeye özgü olması gerekir. Bu nedenle bölüm kimliği + çoğaltma kimliği URL'de bulunmaktadır. URL öneki benzersiz olduğu sürece aynı bağlantı noktasında birden çok adreste dinleyebilirsiniz.
   
    İkincil yinelemelerin de salt okunur isteklerini dinlediği gelişmiş bir servis talebi için ekstra GUID vardır. Bu durumda, istemcileri adresi yeniden çözmeye zorlamak için birincilden ikincile geçiş yaparken yeni bir benzersiz adres kullanıldığından emin olmak istersiniz. '+' burada adres olarak kullanılır, böylece çoğaltma tüm kullanılabilir ana bilgisayarlarda (IP, FQDN, localhost, vb.) dinlenir. Aşağıdaki kod bir örnek gösterir.
   
    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Ayrıca, yayınlanan URL'nin dinleme URL önekinden biraz farklı olduğunu da belirtmekte yarar vardır.
    Dinleme URL'si HttpListener'a verilir. Yayınlanan URL, hizmet bulma için kullanılan Hizmet Kumaşı Adlandırma Hizmeti'nde yayınlanan URL'dir. İstemciler bu bulma hizmeti aracılığıyla bu adresi isteyecektir. İstemcilerin aldığı adresin bağlanmak için düğümün gerçek IP veya FQDN'sine sahip olması gerekir. Bu nedenle yukarıda gösterildiği gibi düğümün IP veya FQDN'si ile '+' değiştirmeniz gerekir.
9. Son adım, aşağıda gösterildiği gibi hizmete işleme mantığını eklemektir.
   
    ```csharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "successfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest`bölümü çağırmak için kullanılan sorgu dize parametresinin `AddUserAsync` değerlerini okur ve güvenilir `dictionary`sözlükiçin soyadını eklemek için çağrılar.
10. Belirli bir bölümü nasıl çağırabileceğinizi görmek için projeye devletsiz bir hizmet ekleyelim.
    
    Bu hizmet, soyadını sorgu dize parametresi olarak kabul eden, bölüm anahtarını belirleyen ve işleme için Alphabet.Processing hizmetine gönderen basit bir web arabirimi olarak hizmet verir.
11. Hizmet **Oluştur** iletişim kutusunda, **Stateless** hizmetini seçin ve aşağıda gösterildiği gibi "Alphabet.Web" adını alın.
    
    ![Devletsiz hizmet ekran görüntüsü](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Aşağıda gösterildiği gibi bir bağlantı noktasını açmak için Alphabet.WebApi hizmetinin ServiceManifest.xml'indeki son nokta bilgilerini güncelleştirin.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Sınıf Web'de ServiceInstanceListeners bir koleksiyon döndürmeniz gerekir. Yine, basit bir HttpCommunicationListener uygulamak için seçebilirsiniz.
    
    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. Şimdi işleme mantığını uygulamanız gerekir. Bir istek geldiğinde `ProcessInputRequest` HttpCommunicationListener arar. O zaman devam edelim ve aşağıdaki kodu ekleyelim.
    
    ```csharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Adım adım yürüyelim. Kod, sorgu dize parametresinin `lastname` ilk harfini char'a okur. Daha sonra, soyadının ilk harfinin `A` hexadecimal değerinden hexadecimal değerini çıkararak bu harfin bölüm anahtarını belirler.
    
    ```csharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Unutmayın, bu örnek için, bölüm başına bir bölüm tuşu ile 26 bölüm kullanıyoruz.
    Daha sonra, `partition` `ResolveAsync` `servicePartitionResolver` nesne üzerindeki yöntemi kullanarak bu anahtar için hizmet bölümü elde ediyoruz. `servicePartitionResolver`olarak tanımlanır
    
    ```csharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    Yöntem `ResolveAsync` hizmet URI, bölüm anahtarı ve parametre olarak bir iptal belirteci alır. İşleme hizmeti için hizmet `fabric:/AlphabetPartitions/Processing`URI olduğunu. Sonra, bölümün bitiş noktasını elde edeceğiz.
    
    ```csharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Son olarak, bitiş noktası URL'si artı sorgu dizesini oluşturur ve işleme hizmetini çağırır.
    
    ```csharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    İşlem yapıldıktan sonra çıktıyı geri yazarız.
15. Son adım hizmeti test etmektir. Visual Studio, yerel ve bulut dağıtımı için uygulama parametrelerini kullanır. Hizmeti yerel olarak 26 bölümle test etmek için, AlphabetPartitions projesinin ApplicationParameters klasöründeki dosyayı `Local.xml` aşağıda gösterildiği gibi güncelleştirmeniz gerekir:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Dağıtımı tamamladıktan sonra, Hizmet Kumaş Explorer'da hizmeti ve tüm bölümlerini denetleyebilirsiniz.
    
    ![Hizmet Kumaş Explorer ekran görüntüsü](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. Bir tarayıcıda, bölümleme mantığını girerek `http://localhost:8081/?lastname=somename`test edebilirsiniz. Aynı harfle başlayan her soyadın aynı bölümde depolandığını görürsünüz.
    
    ![Tarayıcı ekran görüntüsü](./media/service-fabric-concepts-partitioning/samplerunning.png)

Örneğin tüm kaynak kodu [GitHub'da](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions)mevcuttur.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında bilgi için aşağıdakibilgilere bakın:

* [Hizmet Kumaş ı hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Hizmet Kumaş ı hizmetlerinin ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Servis Kumaşı uygulamaları için kapasite planlaması](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
