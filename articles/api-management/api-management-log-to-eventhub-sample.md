---
title: Azure API Management, Event Hubs ve Moesif ile API 'Leri izleme
titleSuffix: Azure API Management
description: HTTP günlüğe kaydetme ve izleme için Azure API Management, Azure Event Hubs ve Moesif ' i bağlayarak, eventhub ile oturum açma ilkesini gösteren örnek uygulama
services: api-management
documentationcenter: ''
author: darrelmiller
manager: erikre
editor: ''
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2018
ms.author: apimpm
ms.openlocfilehash: 4a0717bf7a284668af4808acae3050cc7f42f836
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75442537"
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-moesif"></a>Azure API Management, Event Hubs ve Moesif ile API 'lerinizi izleme
[API Management hizmeti](api-management-key-concepts.md) , HTTP API 'NIZE gönderilen http isteklerinin işlenmesini iyileştirmek için birçok özellik sağlar. Ancak, isteklerin ve yanıtların varlığı geçicidir. İstek yapılır ve arka uç API 'nize API Management hizmeti üzerinden akar. API 'niz isteği ve bir yanıt akışını API tüketicisine geri işler. API Management hizmeti, API 'Lerle ilgili bazı önemli istatistikleri Azure portal panosunda görüntülemeye devam eder, ancak bundan sonra Ayrıntılar kayboluyor.

API Management hizmetinde eventhub ' ı kullanarak, bir [Azure Olay Hub 'ına](../event-hubs/event-hubs-what-is-event-hubs.md)istekten ve yanıttan herhangi bir ayrıntıyı gönderebilirsiniz. API 'lerinize gönderilen HTTP iletilerinden olay oluşturmak isteyebileceğiniz çeşitli nedenler vardır. Güncelleştirmelerin, kullanım analizlerinin denetim izi, özel durum uyarısı ve üçüncü taraf tümleştirmeler bazı örneklere dahildir.

Bu makalede, tüm HTTP isteği ve yanıt iletisini yakalama, bir olay hub 'ına gönderme ve bu iletiyi HTTP günlüğü ve izleme hizmetleri sağlayan bir üçüncü taraf hizmetine geçirme işlemlerinin nasıl yapılacağı gösterilir.

## <a name="why-send-from-api-management-service"></a>Neden API Management hizmetinden gönder?
Http isteklerini ve yanıtlarını yakalayıp günlüğe kaydetme ve izleme sistemlerine akışa almak için HTTP API çerçevelerine takılabilen HTTP ara yazılımı yazmak mümkündür. Bu yaklaşımın dezavantajı, HTTP ara yazılımı 'nın arka uç API 'siyle tümleştirilmesi ve API 'nin platformuyla eşleşmesi gerekir. Birden çok API varsa, her birinin ara yazılımı dağıtması gerekir. Genellikle arka uç API 'Lerinin güncelleştirilamamasının nedenleri vardır.

Azure API Management hizmetini kullanarak günlük altyapısı ile tümleştirme için, merkezi ve platformdan bağımsız bir çözüm sağlanır. Ayrıca, Azure API Management [coğrafi çoğaltma](api-management-howto-deploy-multi-region.md) özellikleri nedeniyle de ölçeklendirilebilir.

## <a name="why-send-to-an-azure-event-hub"></a>Neden bir Azure Olay Hub 'ına gönderilsin?
Bu sorun, neden Azure Event Hubs özel bir ilke oluşturmalı? İsteklerimi günlüğe kaydetmek isteyebileceğiniz birçok farklı yer vardır. İstekler yalnızca son hedefe mı doğrudan gönderilsin?  Bu bir seçenektir. Ancak, API Management hizmetinden günlüğe kaydetme istekleri yaparken, günlüğe kaydetme iletilerinin API performansını nasıl etkilediğini göz önünde bulundurmanız gerekir. Yükteki aşamalı artışlar, kullanılabilir sistem bileşenleri örnekleri arttırılarak veya coğrafi çoğaltmanın avantajlarından yararlanarak işlenebilir. Ancak, trafikte kısa ani artışlar, altyapı günlüğe kaydetme isteklerinin yük altında yavaşlamaya başlaması durumunda isteklerin gecikmesine neden olabilir.

Azure Event Hubs, çok büyük hacimlerde veri almak üzere tasarlanmıştır ve çok daha yüksek sayıda olayla, en fazla API işlem sayısından çok daha fazla olay ile ilgilenirken kapasiteye sahiptir. Olay Hub 'ı, API Yönetimi hizmetiniz ile iletileri depolayan ve işleyen altyapı arasında gelişmiş bir arabellek türü olarak davranır. Bu işlem, API performanızın günlük altyapısı nedeniyle etkilenmemesini sağlar.

Veriler bir olay hub 'ına geçirildiğinde, kalıcı hale getirilir ve Olay Hub 'ı tüketicilerinin bunu işlemesini bekler. Olay Hub 'ı, nasıl işlendiğini ele almaz. iletinin başarılı bir şekilde teslim edilmesini sağlayın.

Event Hubs birden çok tüketici grubuna olay akışı yapabilme özelliğine sahiptir. Bu, olayların farklı sistemler tarafından işlenmesini sağlar. Bu, çok sayıda tümleştirme senaryosunu desteklemek için, API Management hizmeti içindeki API isteğinin işlenmesine yalnızca bir olay oluşturulması gerekir.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Uygulama/http iletileri gönderme ilkesi
Bir olay hub 'ı, olay verilerini basit bir dize olarak kabul eder. Bu dizenin içeriği size ait. Bir HTTP isteğini paketleyip Event Hubs 'e gönderebilmek için, dizeyi istek veya Yanıt bilgileriyle biçimlendirmemiz gerekir. Bunun gibi durumlarda, yeniden kullanabileceğiniz bir biçim varsa, kendi ayrıştırma kodumuzu yazmak zorunda kalmaz. Başlangıçta HTTP istekleri ve yanıtları göndermek için [har](http://www.softwareishard.com/blog/har-12-spec/) kullanmayı kabul ediyorum. Ancak, bu biçim bir dizi HTTP isteğini JSON tabanlı biçimde depolamak için iyileştirilmiştir. HTTP iletisini kablo üzerinden geçirme senaryosunda gereksiz karmaşıklık ekleyen bir dizi zorunlu öğe içeriyordu.

Alternatif bir seçenek, HTTP belirtiminde `application/http` [RFC 7230](https://tools.ietf.org/html/rfc7230)' de açıklandığı gibi medya türünü kullanmaktır. Bu medya türü, gerçekte HTTP iletilerini kablo üzerinden göndermek için kullanılan biçimi kullanır, ancak tüm ileti başka bir HTTP isteğinin gövdesine yerleştirilebilir. Bizim örneğimizde, Event Hubs göndermek için İletimizin olarak gövdeyi kullanacağız. Kolayca, bu biçimi ayrıştırabilen ve bunu yerel `HttpRequestMessage` ve `HttpResponseMessage` nesnelere DÖNÜŞTÜREBILEN [Microsoft ASP.NET Web API 2,2 istemci](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kitaplıklarında bulunan bir Ayrıştırıcı vardır.

Bu iletiyi oluşturabilmeniz için Azure API Management 'deki C# tabanlı [ilke ifadelerinden](/azure/api-management/api-management-policy-expressions) faydalanması gerekiyor. Azure Event Hubs HTTP istek iletisi gönderen ilke aşağıda verilmiştir.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>İlke bildirimi
Bu ilke ifadesiyle ilgili birkaç belirli şey vardır. Eventhub ile oturum açma ilkesi, API Management hizmeti içinde oluşturulan günlükçü adına başvuruda bulunan günlükçü kimliği adlı bir özniteliğe sahiptir. API Management hizmetinde bir olay hub 'ı günlükçüsü ayarlama ayrıntıları, [azure API Management 'de olayları azure Event Hubs günlüğe kaydetme](api-management-howto-log-event-hubs.md)bölümünde bulunabilir. İkinci öznitelik, ' nin iletiyi hangi bölüme depolayabileceği Event Hubs yönlendiren isteğe bağlı bir parametredir. Event Hubs ölçeklenebilirliği etkinleştirmek ve en az iki tane gerektirmek için bölümleri kullanır. İletilerin sıralı teslimi yalnızca bir bölüm içinde garanti edilir. İletiyi hangi bölüme yerleştireceğiz Olay Hub 'ını söylemek mümkün değilse, yükü dağıtmak için hepsini bir kez deneme algoritması kullanır. Ancak bu, bazı iletilerimizin belirli bir sırada işlenmesine neden olabilir.

### <a name="partitions"></a>Bölümler
İletilerimizin tüketicilere göre müşterilere teslim edilmesini sağlamak ve bölümlerin yük dağıtım özelliğinden yararlanmak için, bir bölüme HTTP istek iletileri ve HTTP yanıt iletilerini ikinci bir bölüme göndermek istiyorum. Bu, hatta yük dağıtımı sağlar ve tüm isteklerin sırayla tüketileceğini ve tüm yanıtların sırayla tüketildiğini garanti edebiliyoruz. Bir yanıt, karşılık gelen istekten önce tüketilebilir, ancak bu bir sorun değildir çünkü isteklerin yanıtlara karşı bağıntılandırması için farklı bir mekanizmaya sahip olduğumuz ve isteklerin her zaman yanıtlardan önce geldiğinden emin biliyoruz.

### <a name="http-payloads"></a>HTTP yükleri
Oluşturduktan sonra `requestLine`, istek gövdesinin kesilme olup olmadığını kontrol ederiz. İstek gövdesi yalnızca 1024 olarak kesilir. Bu durum artırılabilir, ancak tek bir olay hub 'ı iletileri 256 KB ile sınırlıdır, bu nedenle bazı HTTP ileti gövdeleri tek bir iletiye sığmayacak olabilir. Günlüğe kaydetme ve analizler yaparken, yalnızca HTTP istek satırı ve üst bilgilerinden elde edilen önemli miktarda bilgi bulunabilir. Ayrıca, birçok API isteği yalnızca küçük gövdeleri geri döndürmesiyle büyük gövdeleri kesilerek bilgi kaybı, aktarım, işleme ve depolama maliyetlerinde azalmayla karşılaştırıldığında tüm gövde içeriklerini tutmak için oldukça düşüktür. Gövdeyi işlemeye yönelik bir son notta, gövde içeriğini okuduğumuz ve `true` Ayrıca arka `As<string>()` uç API 'sinin gövdesini okuyabilmesini sağlayan yönteme geçebilmemiz gerekir. Bu yönteme doğru geçirerek gövdenin ikinci kez okuyabilmesi için arabelleğe alınmasına neden olur. Büyük dosyaları karşıya yükleyen veya uzun yoklama kullanan bir API 'SI varsa, bunun farkında olmak önemlidir. Bu durumlarda, gövdenin tümünü okumaktan kaçınmak en iyisidir.

### <a name="http-headers"></a>HTTP üstbilgileri
HTTP üstbilgileri, bir basit anahtar/değer çifti biçimindeki ileti biçimine aktarılabilir. Bazı güvenlik duyarlı alanları, kimlik bilgilerinin gereksiz yere sızmasını önlemek için seçtik. API anahtarlarının ve diğer kimlik bilgilerinin analiz amaçları doğrultusunda kullanılması düşüktür. Kullanıcı ve kullandıkları belirli bir ürün üzerinde analiz yapmak istiyoruz, bundan sonra bunu `context` nesnesinden alabilir ve iletiye ekleyebilirsiniz.

### <a name="message-metadata"></a>İleti meta verileri
Olay Hub 'ına göndermek için tüm iletiyi oluştururken, ilk satır aslında `application/http` iletinin bir parçası değildir. İlk satır, iletinin istek ya da yanıt iletisi ve bir ileti KIMLIĞI olup olmadığını ve isteklerin yanıtlarla ilişkilendirilmesi için kullanılan ek meta verilerdir. İleti KIMLIĞI, aşağıdaki gibi görünen başka bir ilke kullanılarak oluşturulur:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Yanıt döndürülünceye kadar bir değişkende depolanan istek iletisini oluşturmuş ve sonra isteği ve yanıtı tek bir ileti olarak gönderdik. Bununla birlikte, istek ve yanıtı bağımsız olarak ve iki ile ilişkilendirmek üzere bir ileti kimliği kullanarak göndererek, ileti boyutunda biraz daha esneklik elde ediyoruz. ileti sırasını korumak için birden çok bölümden faydalanabilir ve istek günlük panonuzda daha önce görünür. Ayrıca, büyük olasılıkla API Management hizmetindeki önemli bir istek hatası nedeniyle, geçerli bir yanıtın Olay Hub 'ına hiçbir şekilde gönderilmediği bazı senaryolar da olabilir, ancak yine de isteğin bir kaydı var.

Yanıt HTTP iletisini gönderme ilkesi isteğe benzer ve tüm ilke yapılandırması şöyle görünür:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

`set-variable` İlke `log-to-eventhub` , `<inbound>` bölümündeki ilke ve `<outbound>` bölümünde erişilebilen bir değer oluşturur.

## <a name="receiving-events-from-event-hubs"></a>Event Hubs olayları alma
Azure Olay Hub 'ından gelen olaylar [AMQP Protokolü](https://www.amqp.org/)kullanılarak alınır. Microsoft Service Bus ekibi, istemci kitaplıklarını tüketen olayları kolaylaştırmak için kullanılabilir hale yaptı. Desteklenen iki farklı yaklaşım vardır; biri *doğrudan tüketicidir* ve diğeri `EventProcessorHost` sınıfını kullanıyor. Bu iki yaklaşımın örnekleri [Event Hubs programlama kılavuzunda](../event-hubs/event-hubs-programming-guide.md)bulunabilir. Farkların kısa sürümü, size tamamen denetim `Direct Consumer` sağlar ve `EventProcessorHost` her bir tesisat işi sizin için çalışır, ancak bu olayları nasıl işleytiğimize ilişkin belirli varsayımlar yapar.

### <a name="eventprocessorhost"></a>EventProcessorHost
Bu örnekte, basitlik `EventProcessorHost` için kullanıyoruz, ancak bu senaryo için en iyi seçim olmayabilir. `EventProcessorHost`, belirli bir olay işlemcisi sınıfında iş parçacığı sorunları hakkında endişelenmenize gerek olmadığından emin olun. Ancak, senaryolarımızda iletiyi başka bir biçime dönüştürmekte ve zaman uyumsuz bir yöntem kullanarak başka bir hizmete geçiriyoruz. Paylaşılan durumu güncelleştirme, dolayısıyla iş parçacığı oluşturma riski olmaması gerekmez. Çoğu senaryoda, `EventProcessorHost` büyük olasılıkla en iyi seçenektir ve kesinlikle daha kolay bir seçenektir.

### <a name="ieventprocessor"></a>Ieventprocessor
Kullanırken `EventProcessorHost` merkezi kavram, yöntemi `IEventProcessor` `ProcessEventAsync`içeren arabirimin bir uygulamasını oluşturmaktır. Bu yöntemin özünü burada gösterilmektedir:

```csharp
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

    foreach (EventData eventData in messages)
    {
        _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

        try
        {
            var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
            await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
        }
        catch (Exception ex)
        {
            _Logger.LogError(ex.Message);
        }
    }
    ... checkpointing code snipped ...
}
```

EventData nesnelerinin bir listesi yöntemine geçirilir ve bu listenin üzerinde yineleme yapılır. Her yöntemin baytları bir HttpMessage nesnesine ayrıştırılır ve bu nesne bir ıhttpmessageprocessor örneğine geçirilir.

### <a name="httpmessage"></a>HttpMessage
Örnek `HttpMessage` , üç veri parçası içerir:

```csharp
public class HttpMessage
{
    public Guid MessageId { get; set; }
    public bool IsRequest { get; set; }
    public HttpRequestMessage HttpRequestMessage { get; set; }
    public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

Örnek `HttpMessage` , http isteğini `MessageId` karşılık gelen HTTP yanıtına ve nesnenin bir HttpRequestMessage ve HttpResponseMessage örneği içerip içermadığını belirleyen bir Boole değeri ile bağlantı kurmamızı sağlayan bir GUID içerir. İçindeki yerleşik HTTP sınıflarını kullanarak `System.Net.Http`, içinde `application/http` `System.Net.Http.Formatting`bulunan ayrıştırma kodundan faydalanabilir.  

### <a name="ihttpmessageprocessor"></a>Ihttpmessageprocessor
`HttpMessage` Örnek daha sonra uygulamasının uygulamasına iletilir `IHttpMessageProcessor`, bu, olayın Azure Event hub 'ından alınması ve yorumlanmasını ve gerçek işlemesini ayrıştırmak için oluşturduğum bir arabirimdir.

## <a name="forwarding-the-http-message"></a>HTTP iletisini iletme
Bu örnek için, HTTP Isteğini [Moesıt API](https://www.moesif.com)analizine göndermek ilginç olacağını kararlıyorum. Moesif, HTTP analizine ve hata ayıklamaya uzmanlaşmış bir bulut tabanlı hizmettir. Bunlar ücretsiz bir katmana sahiptir ve bu sayede kolayca denenecek ve API Management hizmetimiz aracılığıyla HTTP isteklerini gerçek zamanlı olarak görmemize olanak sağlar.

Uygulama `IHttpMessageProcessor` şuna benzer.

```csharp
public class MoesifHttpMessageProcessor : IHttpMessageProcessor
{
    private readonly string RequestTimeName = "MoRequestTime";
    private MoesifApiClient _MoesifClient;
    private ILogger _Logger;
    private string _SessionTokenKey;
    private string _ApiVersion;
    public MoesifHttpMessageProcessor(ILogger logger)
    {
        var appId = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-APP-ID", EnvironmentVariableTarget.Process);
        _MoesifClient = new MoesifApiClient(appId);
        _SessionTokenKey = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-SESSION-TOKEN", EnvironmentVariableTarget.Process);
        _ApiVersion = Environment.GetEnvironmentVariable("APIMEVENTS-MOESIF-API-VERSION", EnvironmentVariableTarget.Process);
        _Logger = logger;
    }

    public async Task ProcessHttpMessage(HttpMessage message)
    {
        if (message.IsRequest)
        {
            message.HttpRequestMessage.Properties.Add(RequestTimeName, DateTime.UtcNow);
            return;
        }

        EventRequestModel moesifRequest = new EventRequestModel()
        {
            Time = (DateTime) message.HttpRequestMessage.Properties[RequestTimeName],
            Uri = message.HttpRequestMessage.RequestUri.OriginalString,
            Verb = message.HttpRequestMessage.Method.ToString(),
            Headers = ToHeaders(message.HttpRequestMessage.Headers),
            ApiVersion = _ApiVersion,
            IpAddress = null,
            Body = message.HttpRequestMessage.Content != null ? System.Convert.ToBase64String(await message.HttpRequestMessage.Content.ReadAsByteArrayAsync()) : null,
            TransferEncoding = "base64"
        };

        EventResponseModel moesifResponse = new EventResponseModel()
        {
            Time = DateTime.UtcNow,
            Status = (int) message.HttpResponseMessage.StatusCode,
            IpAddress = Environment.MachineName,
            Headers = ToHeaders(message.HttpResponseMessage.Headers),
            Body = message.HttpResponseMessage.Content != null ? System.Convert.ToBase64String(await message.HttpResponseMessage.Content.ReadAsByteArrayAsync()) : null,
            TransferEncoding = "base64"
        };

        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApimMessageId", message.MessageId.ToString());

        EventModel moesifEvent = new EventModel()
        {
            Request = moesifRequest,
            Response = moesifResponse,
            SessionToken = _SessionTokenKey != null ? message.HttpRequestMessage.Headers.GetValues(_SessionTokenKey).FirstOrDefault() : null,
            Tags = null,
            UserId = null,
            Metadata = metadata
        };

        Dictionary<string, string> response = await _MoesifClient.Api.CreateEventAsync(moesifEvent);

        _Logger.LogDebug("Message forwarded to Moesif");
    }

    private static Dictionary<string, string> ToHeaders(HttpHeaders headers)
    {
        IEnumerable<KeyValuePair<string, IEnumerable<string>>> enumerable = headers.GetEnumerator().ToEnumerable();
        return enumerable.ToDictionary(p => p.Key, p => p.Value.GetEnumerator()
                                                         .ToEnumerable()
                                                         .ToList()
                                                         .Aggregate((i, j) => i + ", " + j));
    }
}
```

, `MoesifHttpMessageProcessor` Http olay verilerini hizmetine göndermeyi kolaylaştıran bir [moesif için C# API kitaplığı](https://www.moesif.com/docs/api?csharp#events) avantajlarından yararlanır. Moesif Toplayıcı API 'sine HTTP verileri göndermek için bir hesap ve uygulama kimliği gereklidir. [Moesif 'in Web sitesinde](https://www.moesif.com) bir hesap oluşturup _sağ üst menü_ -> _uygulama kurulumuna_giderek bir moesif uygulama kimliği alırsınız.

## <a name="complete-sample"></a>Tüm örnek
Örnek için [kaynak kodu](https://github.com/dgilling/ApimEventProcessor) ve testler GitHub ' da bulunur. Örneği kendiniz çalıştırmak için bir [API Management hizmeti](get-started-create-service-instance.md), [bağlı bir olay hub](api-management-howto-log-event-hubs.md)'ı ve bir [depolama hesabı](../storage/common/storage-create-storage-account.md) gerekir.   

Örnek, yalnızca Olay Hub 'ından gelen olayları dinleyen basit bir konsol uygulamasıdır, bunları bir Moesıya `EventRequestModel` ve `EventResponseModel` nesnelerine dönüştürür ve ardından onları MOESIF Collector API 'sine iletir.

Aşağıdaki animasyonlu görüntüde, geliştirici portalındaki bir API 'ye yapılan bir istek, alınan, işlenen ve iletilen iletiyi gösteren konsol uygulaması, sonra da olay akışında gösterilen istek ve yanıt olduğunu görebilirsiniz.

![Runscope 'a iletilmekte olan isteğin gösterimi](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Özet
Azure API Management hizmeti, API 'lerinize ve API 'lerinize seyahat eden HTTP trafiğini yakalamak için ideal bir yer sağlar. Azure Event Hubs, trafiği yakalamaya ve günlüğe kaydetme, izleme ve diğer gelişmiş analizler için ikincil işleme sistemlerine beslemenin yüksek düzeyde ölçeklenebilir, düşük maliyetli bir çözümüdür. Moesıo gibi üçüncü taraf trafik izleme sistemlerine bağlanmak, birkaç düzine kod satırı kadar basittir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi
  * [Azure Event Hubs kullanmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve Event Hubs tümleştirmesi hakkında daha fazla bilgi edinin
  * [Azure Event Hubs olayları Azure 'da günlüğe kaydetme API Management](api-management-howto-log-event-hubs.md)
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [günlük-eventhub ilke başvurusu](/azure/api-management/api-management-advanced-policies#log-to-eventhub)
