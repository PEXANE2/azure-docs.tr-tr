---
title: Servis Kumaşında C# kullanılarak servis remotingi
description: Service Fabric remoting, istemcilerin ve hizmetlerin uzaktan yordam çağrısı kullanarak C# hizmetleriyle iletişim kurmasını sağlar.
author: vturecek
ms.topic: conceptual
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: 0d59275f25931a11b2d551a2e9eb019838e4c1b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433879"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>Güvenilir Hizmetler le C#'da servis remotingi

> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Web API, Windows Communication Foundation veya diğerleri gibi belirli bir iletişim protokolüne veya yığınına bağlı olmayan hizmetler için Güvenilir Hizmetler çerçevesi, uzaktan yordam çağrılarının hızlı ve kolay bir şekilde ayarlanması için bir yeniden moting mekanizması sağlar Hizmetleri. Bu makalede, C# ile yazılmış hizmetler için uzaktan yordam çağrılarının nasıl ayarlır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmette remoting ayarlama

Bir hizmet için remoting'i iki basit adımda ayarlayabilirsiniz:

1. Hizmetinizin uygulayacağı bir arayüz oluşturun. Bu arabirim, hizmetinizde uzaktan yordam çağrısı için kullanılabilen yöntemleri tanımlar. Yöntemler görev döndüren eşzamanlı yöntemler olmalıdır. Arabirimin, `Microsoft.ServiceFabric.Services.Remoting.IService` hizmetin bir remoting arabirimi olduğunu bildirmek için uygulaması gerekir.
2. Hizmetinizde bir remoting dinleyici kullanın. Remoting dinleyici, remoting yetenekleri sağlayan bir `ICommunicationListener` uygulamadır. Ad `Microsoft.ServiceFabric.Services.Remoting.Runtime` alanı, varsayılan `CreateServiceRemotingInstanceListeners` remoting aktarım iletişim kuralını kullanarak bir remoting dinleyici oluşturmak için kullanılabilecek hem durumsuz hem de durum lu hizmetler için uzantı yöntemini içerir.

>[!NOTE]
>Ad `Remoting` alanı, ayrı bir NuGet `Microsoft.ServiceFabric.Services.Remoting`paketi olarak kullanılabilir.

Örneğin, aşağıdaki stateless hizmeti uzak bir yordam çağrısı üzerinden "Hello World" almak için tek bir yöntem ortaya çıkarır.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task<string> HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
     return this.CreateServiceRemotingInstanceListeners();
    }
}
```

> [!NOTE]
> Hizmet arabirimindeki bağımsız değişkenler ve iade türleri basit, karmaşık veya özel türler olabilir, ancak .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx)tarafından serihale edilebilmeleri gerekir.
>
>

## <a name="call-remote-service-methods"></a>Uzaktan servis yöntemlerini arayın

Remoting yığınını kullanarak bir hizmetteki arama yöntemlerini `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıf aracılığıyla hizmete yerel bir proxy kullanılarak yapılır. Yöntem, `ServiceProxy` hizmetin uyguladığı aynı arabirimi kullanarak yerel bir proxy oluşturur. Bu proxy ile, uzaktan arabirimdeki yöntemleri arayabilirsiniz.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Remoting çerçevesi, hizmettarafından istemciye atılan özel durumları yayıltır. Sonuç olarak, `ServiceProxy`kullanıldığında, istemci hizmet tarafından atılan özel durumları işlemekten sorumludur.

## <a name="service-proxy-lifetime"></a>Hizmet proxy ömrü

Servis proxy oluşturma hafif bir işlemdir, böylece istediğiniz kadar çok oluşturabilirsiniz. Hizmet proxy örnekleri, gerekli oldukları sürece yeniden kullanılabilir. Uzak yordam çağrısı bir özel durum atarsa, yine de aynı proxy örneğini yeniden kullanabilirsiniz. Her hizmet proxy'si, iletileri kablo üzerinden göndermek için kullanılan bir iletişim istemcisi içerir. Uzaktan arama çağırırken, iletişim istemcisinin geçerli olup olmadığını belirlemek için iç denetimler gerçekleştirilir. Bu denetimlerin sonuçlarına bağlı olarak, gerekirse iletişim istemcisi yeniden oluşturulur. Bu nedenle, bir özel durum oluşursa, `ServiceProxy`yeniden oluşturmanız gerekmez.

### <a name="service-proxy-factory-lifetime"></a>Hizmet proxy fabrika ömrü

[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) farklı remoting arabirimleri için proxy örnekleri oluşturan bir fabrikadır. Proxy oluşturmak için `ServiceProxyFactory.CreateServiceProxy` API'yi kullanırsanız, çerçeve bir singleton hizmet proxy'si oluşturur.
[IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v1.client.iserviceremotingclientfactory) özelliklerini geçersiz kılmanız gerektiğinde el ile bir tane oluşturmak yararlıdır.
Fabrika oluşturma pahalı bir işlemdir. Bir hizmet proxy fabrikası iletişim istemcisinin iç önbelleğini tutar.
En iyi uygulama, hizmet proxy fabrikasını mümkün olduğunca uzun süre önbelleğe almaktır.

## <a name="remoting-exception-handling"></a>Özel durum işlemeyi remoting

Hizmet API'si tarafından atılan tüm uzak özel durumlar, AggregateException olarak istemciye geri gönderilir. Uzak özel durumlar DataContract tarafından serihale edilebilmelidir. Değilse, proxy API içinde serileştirme hatası ile [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) atar.

Hizmet proxy'si, oluşturulduğu hizmet bölümü için tüm başarısız lık istisnalarını işler. Başarısız lık özel durumları (geçici olmayan özel durumlar) varsa uç noktaları yeniden çözer ve aramayı doğru bitiş noktasıyla yeniden dener. Başarısız istisnalar için yeniden deneme sayısı belirsizdir.
Geçici özel durumlar oluşursa, proxy aramayı yeniden dener.

Varsayılan yeniden deneme parametreleri [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings)tarafından sağlanır.

Kullanıcı, OperationRetrySettings nesnesini ServiceProxyFactory oluşturucuya geçirerek bu değerleri yapılandırabilir.

## <a name="use-the-remoting-v2-stack"></a>Remoting V2 yığınını kullanma

NuGet remoting paketinin sürüm 2.8 itibariyle, remoting V2 yığınını kullanma seçeneğiniz var. Remoting V2 yığını daha iyi performans gösterir. Ayrıca özel serileştirme ve daha takılabilir API'ler gibi özellikler sağlar.
Şablon kodu remoting V1 yığınını kullanmaya devam ediyor.
V2 remoting V1 (önceki remoting yığını) ile uyumlu değildir. Hizmet kullanılabilirliği üzerindeki etkileri önlemek [için V1'den V2'ye Yükseltme](#upgrade-from-remoting-v1-to-remoting-v2) makalesindeki yönergeleri izleyin.

V2 yığınını etkinleştirmek için aşağıdaki yaklaşımlar kullanılabilir.

### <a name="use-an-assembly-attribute-to-use-the-v2-stack"></a>V2 yığınını kullanmak için montaj özniteliği kullanma

Bu adımlar, bir derleme özniteliği kullanarak V2 yığınını kullanmak için şablon kodunu değiştirir.

1. Bitiş noktası kaynağını `"ServiceEndpoint"` hizmet `"ServiceEndpointV2"` bildiriminden değiştirin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
   </Resources>
   ```

2. Remoting dinleyicileri oluşturmak için `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners` uzantı yöntemini kullanın (hem V1 hem de V2 için eşittir).

   ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
   ```

3. Remoting arabirimlerini öznitelik içeren `FabricTransportServiceRemotingProvider` derlemeyi işaretleyin.

   ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
   ```

İstemci projesinde kod değişikliği gerekmez.
Daha önce gösterilen derleme özniteliğinin kullanıldığından emin olmak için arabirim derlemesiyle istemci derlemesini oluşturun.

### <a name="use-explicit-v2-classes-to-use-the-v2-stack"></a>V2 yığınını kullanmak için açık V2 sınıflarını kullanma

Bir derleme özniteliği kullanmaya alternatif olarak, V2 yığını da açık V2 sınıfları kullanılarak etkinleştirilebilir.

Bu adımlar, açık V2 sınıfları kullanarak V2 yığınını kullanmak için şablon kodunu değiştirir.

1. Bitiş noktası kaynağını `"ServiceEndpoint"` hizmet `"ServiceEndpointV2"` bildiriminden değiştirin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
   </Resources>
   ```

2. [Namespace'ten FabricTransportServiceRemotingListener'ı](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime` kullanın.

   ```csharp
   protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                return new FabricTransportServiceRemotingListener(c, this);

            })
        };
    }
   ```

3. İstemci oluşturmak için `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` ad alanından [FabricTransportServiceRemotingClientFactory'yi](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) kullanın.

   ```csharp
   var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
   ```

## <a name="upgrade-from-remoting-v1-to-remoting-v2"></a>V1'i remoting'den V2'yi remotinge yükseltme

V1'den V2'ye yükseltme yapmak için iki aşamalı yükseltmeler gerekir. Bu sırada ki adımları izleyin.

1. Bu özelliği kullanarak V1 hizmetini V2 hizmetine yükseltin.
Bu değişiklik, hizmetin V1 ve V2 dinleyicisini dinlemesini sağlar.

    a. Hizmet bildiriminde "ServiceEndpointV2" adıyla bir bitiş noktası kaynağı ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b. Remoting dinleyicisi oluşturmak için aşağıdaki uzantı yöntemini kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. V1 ve V2 dinleyicisi ve V2 istemcisini kullanmak için remoting arabirimlerine bir derleme özniteliği ekleyin.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2|RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2)]

      ```
2. V2 istemci özniteliğini kullanarak V1 istemcisini V2 istemcisine yükseltin.
Bu adım, istemcinin V2 yığınını kullandığından emin olmasını sağlar.
İstemci projesinde/hizmetinde değişiklik gerekmez. Güncelleştirilmiş arabirim montajı ile istemci projeleri oluşturmak yeterlidir.

3. Bu adım isteğe bağlıdır. V2 dinleyici özniteliğini kullanın ve ardından V2 hizmetini yükseltin.
Bu adım, hizmetin yalnızca V2 dinleyicisini dinlemesini sağlar.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
    ```


## <a name="use-the-remoting-v2-interface-compatible-stack"></a>Remoting V2 (arayüz uyumlu) yığınını kullanma

 Remoting V2 (arayüz uyumlu, V2_1 olarak bilinen) yığını V2 remoting yığınının tüm özelliklerine sahiptir. Arabirim yığını remoting V1 yığını ile uyumludur, ancak V2 ve V1 ile geriye doğru uyumlu değildir. Hizmet kullanılabilirliğini etkilemeden V1'den V2_1 yükseltme yapmak için, V1'den V2'ye Yükseltme (arabirim uyumlu) makalesindeki adımları izleyin.


### <a name="use-an-assembly-attribute-to-use-the-remoting-v2-interface-compatible-stack"></a>Remoting V2 (arayüz uyumlu) yığınını kullanmak için bir montaj özniteliği kullanın

V2_1 yığınına değiştirmek için aşağıdaki adımları izleyin.

1. Hizmet bildiriminde "ServiceEndpointV2_1" adında bir bitiş noktası kaynağı ekleyin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. Remoting dinleyicisi oluşturmak için remoting uzantı yöntemini kullanın.

   ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
   ```

3. Remoting arabirimleri üzerinde bir [derleme özniteliği](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportserviceremotingproviderattribute?view=azure-dotnet) ekleyin.

   ```csharp
    [assembly:  FabricTransportServiceRemotingProvider(RemotingListenerVersion=  RemotingListenerVersion.V2_1, RemotingClientVersion= RemotingClientVersion.V2_1)]

   ```

İstemci projesinde değişiklik gerekmez.
Önceki derleme özniteliğinin kullanıldığından emin olmak için arabirim derlemesiyle istemci derlemesini oluşturun.

### <a name="use-explicit-remoting-classes-to-create-a-listenerclient-factory-for-the-v2-interface-compatible-version"></a>V2 (arayüz uyumlu) sürümü için bir dinleyici/istemci fabrikası oluşturmak için açık remoting sınıfları kullanın

Şu adımları uygulayın:

1. Hizmet bildiriminde "ServiceEndpointV2_1" adında bir bitiş noktası kaynağı ekleyin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. [Remoting V2 dinleyici](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotinglistener?view=azure-dotnet)kullanın. Kullanılan varsayılan hizmet uç noktası kaynak adı "ServiceEndpointV2_1." Hizmet bildiriminde tanımlanmalıdır.

   ```csharp
   protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                var settings = new FabricTransportRemotingListenerSettings();
                settings.UseWrappedMessage = true;
                return new FabricTransportServiceRemotingListener(c, this,settings);

            })
        };
    }
   ```

3. V2 [istemci fabrikasını](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet)kullanın.
   ```csharp
   var proxyFactory = new ServiceProxyFactory((c) =>
          {
            var settings = new FabricTransportRemotingSettings();
            settings.UseWrappedMessage = true;
            return new FabricTransportServiceRemotingClientFactory(settings);
          });
   ```

## <a name="upgrade-from-remoting-v1-to-remoting-v2-interface-compatible"></a>V1'i remoting'den remoting V2'ye yükseltme (arayüz uyumlu)

V1'den V2'ye (V2_1 olarak bilinen arayüz uyumlu) yükseltmek için iki aşamalı yükseltmeler gereklidir. Bu sırada ki adımları izleyin.

> [!NOTE]
> V1'den V2'ye yükseltme yaparken, ad alanının V2 kullanacak şekilde güncelleştirildiğinden `Remoting` emin olun. Örnek: 'Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client'
>
>

1. Aşağıdaki özniteliği kullanarak V1 hizmetini V2_1 hizmetine yükseltin.
Bu değişiklik, hizmetin V1 ve V2_1 dinleyiciyi dinlemesini sağlar.

    a. Hizmet bildiriminde "ServiceEndpointV2_1" adında bir bitiş noktası kaynağı ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2_1" />  
        </Endpoints>
      </Resources>
      ```

    b. Remoting dinleyicisi oluşturmak için aşağıdaki uzantı yöntemini kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. V1, V2_1 dinleyici ve V2_1 istemcisini kullanmak için yeniden ifade birimlerine bir derleme özniteliği ekleyin.
    ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1 | RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2_1)]

      ```
2. V2_1 istemci özniteliğini kullanarak V1 istemcisini V2_1 istemcisine yükseltin.
Bu adım, istemcinin V2_1 yığınını kullandığından emin olur.
İstemci projesinde/hizmetinde değişiklik gerekmez. Güncelleştirilmiş arabirim montajı ile istemci projeleri oluşturmak yeterlidir.

3. Bu adım isteğe bağlıdır. V1 dinleyici sürümünü öznitelikten çıkarın ve ardından V2 hizmetini yükseltin.
Bu adım, hizmetin yalnızca V2 dinleyicisini dinlemesini sağlar.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1, RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```
  
### <a name="use-custom-serialization-with-a-remoting-wrapped-message"></a>Remoting sarılmış iletiyle özel serileştirme kullanma

Remoting sarılmış ileti için, içinde alan olarak tüm parametreleri içeren tek bir sarılmış nesne oluştururuz.
Şu adımları uygulayın:

1. Özel `IServiceRemotingMessageSerializationProvider` serileştirme için uygulama sağlamak için arabirimi uygulayın.
    Bu kod parçacığı, uygulamanın nasıl göründüğünü gösterir.

      ```csharp
      public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
      {
        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
          return new JsonMessageFactory();
        }

        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> requestWrappedType, IEnumerable<Type> requestBodyTypes = null)
        {
          return new ServiceRemotingRequestJsonMessageBodySerializer();
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> responseWrappedType, IEnumerable<Type> responseBodyTypes = null)
        {
          return new ServiceRemotingResponseJsonMessageBodySerializer();
        }
      }
      ```
      ```csharp
        class JsonMessageFactory : IServiceRemotingMessageBodyFactory
            {

              public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName, int numberOfParameters, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }

              public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }
            }
      ```
      ```csharp
      class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
        {
            private JsonSerializer serializer;

            public ServiceRemotingRequestJsonMessageBodySerializer()
            {
              serializer = JsonSerializer.Create(new JsonSerializerSettings()
              {
                TypeNameHandling = TypeNameHandling.All
                });
              }

              public IOutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
             {
               if (serviceRemotingRequestMessageBody == null)
               {
                 return null;
               }          
               using (var writeStream = new MemoryStream())
               {
                 using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
                 {
                   serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);
                   jsonWriter.Flush();
                   var bytes = writeStream.ToArray();
                   var segment = new ArraySegment<byte>(bytes);
                   var segments = new List<ArraySegment<byte>> { segment };
                   return new OutgoingMessageBody(segments);
                 }
               }
              }

              public IServiceRemotingRequestMessageBody Deserialize(IIncomingMessageBody messageBody)
             {
               using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
               {
                 using (JsonReader reader = new JsonTextReader(sr))
                 {
                   var ob = serializer.Deserialize<JsonBody>(reader);
                   return ob;
                 }
               }
             }
            }
      ```
      ```csharp
      class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
       {
         private JsonSerializer serializer;

        public ServiceRemotingResponseJsonMessageBodySerializer()
        {
          serializer = JsonSerializer.Create(new JsonSerializerSettings()
          {
              TypeNameHandling = TypeNameHandling.All
            });
          }

          public IOutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
          {
            if (responseMessageBody == null)
            {
              return null;
            }

            using (var writeStream = new MemoryStream())
            {
              using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
              {
                serializer.Serialize(jsonWriter, responseMessageBody);
                jsonWriter.Flush();
                var bytes = writeStream.ToArray();
                var segment = new ArraySegment<byte>(bytes);
                var segments = new List<ArraySegment<byte>> { segment };
                return new OutgoingMessageBody(segments);
              }
            }
          }

          public IServiceRemotingResponseMessageBody Deserialize(IIncomingMessageBody messageBody)
          {

             using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
             {
               using (var reader = new JsonTextReader(sr))
               {
                 var obj = serializer.Deserialize<JsonBody>(reader);
                 return obj;
               }
             }
           }
       }
    ```
    ```csharp
    class JsonBody : WrappedMessage, IServiceRemotingRequestMessageBody, IServiceRemotingResponseMessageBody
    {
          public JsonBody(object wrapped)
          {
            this.Value = wrapped;
          }

          public void SetParameter(int position, string parameName, object parameter)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object GetParameter(int position, string parameName, Type paramType)
          {
            //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public void Set(object response)
          { //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object Get(Type paramType)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }
    }
    ```

2. Bir remoting dinleyicisi ile `JsonSerializationProvider` varsayılan serileştirme sağlayıcısı geçersiz kılın.

   ```csharp
   protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
   {
       return new[]
       {
           new ServiceInstanceListener((c) =>
           {
               return new FabricTransportServiceRemotingListener(c, this,
                   new ServiceRemotingJsonSerializationProvider());
           })
       };
   }
   ```

3. Bir remoting istemci `JsonSerializationProvider` seserisi için varsayılan serileştirme sağlayıcısı geçersiz kılın.

    ```csharp
    var proxyFactory = new ServiceProxyFactory((c) =>
    {
        return new FabricTransportServiceRemotingClientFactory(
        serializationProvider: new ServiceRemotingJsonSerializationProvider());
      });
      ```

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenilir Hizmetlerde OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir Hizmetler ile Windows Communication Foundation iletişimi](service-fabric-reliable-services-communication-wcf.md)
* [Güvenilir Hizmetler için güvenli iletişim](service-fabric-reliable-services-secure-communication.md)
