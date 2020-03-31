---
title: Güvenilir Hizmetler iletişimine genel bakış
description: Hizmetler de dahil olmak üzere dinleyicilerin hizmetlere açılmasını, uç noktaların çözülmesini ve hizmetler arasında iletişim kurma dahil olmak üzere Güvenilir Hizmetler iletişim modeline genel bakış.
author: vturecek
ms.topic: conceptual
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 3c1a6cfa5227369bf1cde4af087019727c22c0c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75462961"
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Güvenilir Hizmetler iletişim API'leri nasıl kullanılır?
Azure Hizmet Kumaşı bir platform olarak hizmetler arasındaki iletişim konusunda tamamen agnostiktir. UDP'den HTTP'ye kadar tüm protokoller ve yığınlar kabul edilebilir. Hizmetlerin nasıl iletişim kurmasını seçmek hizmet geliştiricisine bağlıdır. Güvenilir Hizmetler uygulama çerçevesi, özel iletişim bileşenlerinizi oluşturmak için kullanabileceğiniz yerleşik iletişim yığınlarının yanı sıra API'ler de sağlar.

## <a name="set-up-service-communication"></a>Hizmet iletişimini ayarlama
Güvenilir Hizmetler API'si, hizmet iletişimi için basit bir arabirim kullanır. Hizmetiniz için bir bitiş noktası açmak için şu arabirimi uygulamanız yeterlidir:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

Daha sonra, iletişim dinleyiciuygulamanızı hizmet tabanlı sınıf yöntemi geçersiz kılmada döndürerek ekleyebilirsiniz.

Devletsiz hizmetler için:

```csharp
public class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

Devlet hizmetleri için:

```java
    @Override
    protected List<ServiceReplicaListener> createServiceReplicaListeners() {
        ...
    }
    ...
```

```csharp
public class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Her iki durumda da, dinleyici koleksiyonu döndürün. Bu, hizmetinizin birden çok dinleyici kullanarak farklı protokoller kullanarak birden çok uç noktada dinlemesini sağlar. Örneğin, bir HTTP dinleyiciniz ve ayrı bir WebSocket dinleyiciniz olabilir. Her dinleyicibir ad alır ve ortaya çıkan *ad koleksiyonu: adres* çiftleri, bir hizmet örneği veya bölüm için dinleme adreslerini istediğinde JSON nesnesi olarak temsil edilir.

Geçersiz kılma, devletsiz bir hizmette ServiceInstanceListeners koleksiyonunu döndürür. A `ServiceInstanceListener` oluşturmak için bir `ICommunicationListener(C#) / CommunicationListener(Java)` işlev içerir ve ona bir ad verir. Devlet hizmetleri için geçersiz kılma ServiceReplicaListeners bir koleksiyon döndürür. Bu, ikincil yinelemeler üzerinde bir `ServiceReplicaListener` `ICommunicationListener` seçenek vardır, çünkü bu, onun stateless muadili biraz farklıdır. Bir hizmette birden çok iletişim dinleyicisi kullanmakla birlikte, hangi dinleyicilerin ikincil yinelemelerde istekleri kabul etmediğini ve yalnızca birincil yinelemelerde hangilerinin dinlediğini de belirtebilirsiniz.

Örneğin, yalnızca birincil yinelemeler üzerinde RPC çağrıları alır bir ServiceRemotingListener ve ikinci, özel dinleyici http üzerinden ikincil yinelemeler okuma isteklerialır olabilir:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> Bir hizmet için birden çok dinleyici oluştururken, her **dinleyiciye** benzersiz bir ad verilmelidir.
>
>

Son olarak, son noktalardaki bölümün altındaki [hizmet bildiriminde hizmet](service-fabric-application-and-service-manifests.md) için gerekli olan uç noktaları açıklayın.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

İletişim dinleyicisi, ona ayrılan uç nokta kaynaklarına `CodePackageActivationContext` `ServiceContext`' daki nden erişebilir. Dinleyici daha sonra açıldığında istekleri dinlemeye başlayabilir.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Uç nokta kaynakları tüm servis paketi için ortaktır ve servis paketi etkinleştirildiğinde Servis Kumaşı tarafından tahsis edilir. Aynı ServiceHost'ta barındırılan birden çok hizmet yinelemesi aynı bağlantı noktasını paylaşabilir. Bu, iletişim dinleyicisinin bağlantı noktası paylaşımını desteklemesi gerektiği anlamına gelir. Bunu yapmanın önerilen yolu, dinleme adresini oluştururken iletişim dinleyicisinin bölüm kimliği ve çoğaltma/örnek kimliğini kullanmasıdır.
>
>

### <a name="service-address-registration"></a>Servis adresi kaydı
*Adlandırma Hizmeti* adlı bir sistem hizmeti Hizmet Kumaş kümeleri üzerinde çalışır. Adlandırma Hizmeti, hizmetlerin her örneğinin veya yinelemesinin dinlediği hizmetler ve adresleri için bir kayıt şirketidir. Bir `ICommunicationListener(C#) / CommunicationListener(Java)` `OpenAsync(C#) / openAsync(Java)` yöntem tamamlandığında, iade değeri Adlandırma Hizmeti'ne kaydedilir. Adlandırma Hizmeti'nde yayınlanan bu iade değeri, değeri herhangi bir şey olabilecek bir dizedir. Bu dize değeri, istemcilerin Adlandırma Hizmeti'nden hizmet için bir adres istediklerinde gördükleri değerdir.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

Service Fabric, istemcilerin ve diğer hizmetlerin bu adresi hizmet adına göre istemesine olanak tanıyan API'ler sağlar. Hizmet adresi statik olmadığından bu önemlidir. Hizmetler, kaynak dengeleme ve kullanılabilirlik amacıyla kümede taşınır. Bu, istemcilerin bir hizmetin dinleme adresini çözmesine olanak tanıyan mekanizmadır.

> [!NOTE]
> Bir iletişim dinleyicisi yazmak için tam bir walk-through için, C # için [OWIN self-hosting ile Service Fabric Web API hizmetleri](service-fabric-reliable-services-communication-webapi.md) bakın, https://github.com/Azure-Samples/service-fabric-java-getting-startedJava için kendi HTTP sunucu uygulaması yazabilirsiniz ise, EchoServer uygulama örneğine bakın .
>
>

## <a name="communicating-with-a-service"></a>Bir hizmetle iletişim kurma
Güvenilir Hizmetler API hizmetleri ile iletişim istemcileri yazmak için aşağıdaki kitaplıklar sağlar.

### <a name="service-endpoint-resolution"></a>Hizmet bitiş noktası çözünürlüğü
Bir hizmetle iletişim in ilk adımı, konuşmak istediğiniz hizmetin bölümü veya örneğinin bitiş noktası adresini çözümlemektir. Yardımcı `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` program sınıfı, istemcilerin çalışma zamanında bir hizmetin bitiş noktasını belirlemelerine yardımcı olan temel bir ilkel sınıftır. Hizmet Kumaş terminolojisinde, bir hizmetin bitiş noktasını belirleme *işlemi, hizmet bitiş noktası çözünürlüğü*olarak adlandırılır.

Bir küme içindeki hizmetlere bağlanmak için, ServicePartitionResolver varsayılan ayarları kullanılarak oluşturulabilir. Bu, çoğu durum için önerilen kullanımdır:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

Farklı bir kümedeki hizmetlere bağlanmak için ServicePartitionResolver küme ağ geçidi bitiş noktaları kümesiyle oluşturulabilir. Ağ geçidi uç noktalarının aynı kümeye bağlanmak için farklı uç noktaları olduğunu unutmayın. Örnek:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternatif olarak, `ServicePartitionResolver` dahili olarak kullanmak `FabricClient` için bir işlev verilebilir:

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

`FabricClient`kümedeki çeşitli yönetim işlemleri için Hizmet Kumaşı kümesiyle iletişim kurmak için kullanılan nesnedir. Bu, bir hizmet bölümü çözümleyicisi küme ile nasıl etkileşim ilerler üzerinde daha fazla denetim istediğinizde yararlıdır. `FabricClient`dahili önbelleğe alma gerçekleştirir ve oluşturmak genellikle pahalıdır, `FabricClient` bu nedenle örnekleri mümkün olduğunca yeniden kullanmak önemlidir.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Daha sonra bir hizmetin adresini veya bölümlenmiş hizmetler için bir hizmet bölümü adresini almak için çözümleme yöntemi kullanılır.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

Bir hizmet adresi ServicePartitionResolver kullanılarak kolayca çözülebilir, ancak çözülen adresin doğru şekilde kullanılabildiğinden emin olmak için daha fazla çalışma gereklidir. İstemcinizin geçici bir hata nedeniyle bağlantı girişiminin başarısız olup olmadığını ve yeniden denenip denenemeyeceğini (örn. hizmet taşınMış veya geçici olarak kullanılamıyor) veya kalıcı bir hata (örn. hizmet silinmiş veya istenen kaynak artık yok) algılaması gerekir. Hizmet örnekleri veya yinelemeler, birden çok nedenden dolayı herhangi bir zamanda düğümden düğüme geçebilir. ServicePartitionResolver aracılığıyla çözülen hizmet adresi, istemci kodunuz bağlanmaya çalıştığında bayatlayabilir. Bu durumda yine istemci adresi yeniden çözmek gerekir. Önceki `ResolvedServicePartition` belirtilmesi, önbelleğe alınmış bir adresi almak yerine çözümleyicinin yeniden denemesi gerektiğini gösterir.

Genellikle, istemci kodu doğrudan ServicePartitionResolver ile çalışması gerekmez. Oluşturulan ve Güvenilir Hizmetler API iletişim istemci fabrikalarına aktarılır. Fabrikalar, hizmetlerle iletişim kurmak için kullanılabilecek bir istemci nesnesi oluşturmak için işleyiciyi dahili olarak kullanır.

### <a name="communication-clients-and-factories"></a>İletişim müşterileri ve fabrikalar
İletişim fabrikası kitaplığı, çözülen hizmet uç noktalarına yeniden bağlanmayı kolaylaştıran tipik bir hata işleme yeniden deneme deseni uygular. Siz hata işleyicileri sağlarken, fabrika kitaplığı yeniden deneme mekanizmasını sağlar.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Bir Service Fabric hizmetiyle konuşabilen istemciler üreten bir iletişim istemcisi fabrikası tarafından uygulanan temel arabirimi tanımlar. CommunicationClientFactory'nin uygulanması, istemcinin iletişim kurmak istediği Servis Kumaşı hizmeti tarafından kullanılan iletişim yığınına bağlıdır. Güvenilir Hizmetler API'si bir `CommunicationClientFactoryBase<TCommunicationClient>`. Bu, CommunicationClientFactory arabiriminin temel uygulamasını sağlar ve tüm iletişim yığınları için ortak olan görevleri gerçekleştirir. (Bu görevler, hizmet bitiş noktasını belirlemek için ServicePartitionResolver'ı kullanmayı içerir). İstemciler genellikle iletişim yığınına özgü mantığı işlemek için soyut CommunicationClientFactoryBase sınıfını uygularlar.

İletişim istemcisi yalnızca bir adres alır ve bir hizmete bağlanmak için kullanır. İstemci istediği protokolü kullanabilir.

```csharp
public class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

Müşteri fabrikası öncelikle iletişim istemcileri oluşturmaktan sorumludur. BIR HTTP istemcisi gibi kalıcı bir bağlantı sürdürmeyen istemciler için, fabrikanın yalnızca istemciyi oluşturması ve döndürmesi gerekir. Bazı ikili protokoller gibi kalıcı bir bağlantı tutan diğer protokoller de bağlantının yeniden oluşturulması gerekip gerekmediğini belirlemek için fabrika tarafından doğrulanmalıdır.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

Son olarak, bir özel durum oluştuğunda hangi eylemin yapılacağı belirlemekten bir özel durum işleyicisi sorumludur. Özel durumlar yeniden **denilebilir** ve **yeniden denilemez**olarak kategorize edilir.

* **Yeniden denemeyen** özel durumlar, arayana yeniden atılmasını yeterlidir.
* **yeniden denilebilir** özel durumlar daha geçici **ve** **geçici olmayan**olarak kategorize edilir.
  * **Geçici** özel durumlar, hizmet bitiş noktası adresini yeniden çözmeden yeniden denenebilen durumlardır. Bunlar, hizmet bitiş noktası adresinin bulunmadığını gösterenler dışında geçici ağ sorunlarını veya hizmet hatası yanıtlarını içerir.
  * **Geçici olmayan** özel durumlar, hizmet bitiş noktası adresinin yeniden çözülmesini gerektiren özel durumlardır. Bunlar, hizmet bitiş noktasının ulaşılamadığını ve hizmetin farklı bir düğüme taşındığını belirten özel durumları içerir.

Belirli `TryHandleException` bir istisna hakkında bir karar verir. Bir özel durum hakkında ne kararlar **verilemeyeceğini** bilmiyorsa, **yanlış**döndürmelidir. Hangi kararı veracağını **biliyorsa,** sonucu buna göre belirlemeli ve **doğru**olarak geri dönmelidir.

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Bir `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, `IExceptionHandler(C#) / ExceptionHandler(Java)` ve bir iletişim `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` protokolü etrafında inşa, bir hep birlikte sarar ve bu bileşenlerin etrafında hata işleme ve hizmet bölümü adresi çözümleme döngüsü sağlar.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Hizmetlerle ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)
* [Güvenilir Hizmetler remoting ile uzaktan yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Güvenilir Hizmetler kullanarak WCF iletişimi](service-fabric-reliable-services-communication-wcf.md)
