---
title: Azure API Yönetimi, Etkinlik Hub'ları ve Moesif ile API'leri izleyin
titleSuffix: Azure API Management
description: HTTP günlüğe kaydetme ve izleme için Azure API Yönetimi, Azure Olay Hub'ları ve Moesif'i birbirine bağlayarak günlük-olay hub ilkesini gösteren örnek uygulama
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442537"
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-moesif"></a>API'lerinizi Azure API Yönetimi, Etkinlik Hub'ları ve Moesif ile izleme
[API Yönetimi hizmeti,](api-management-key-concepts.md) HTTP API'nize gönderilen HTTP isteklerinin işlenmesini geliştirmek için birçok özellik sağlar. Ancak, istek ve yanıtların varlığı geçicidir. İstek yapılır ve API Yönetimi hizmeti nden arka uç API'nize akar. API'niz isteği işler ve yanıt API tüketicisine geri akar. API Yönetimi hizmeti, Azure portalı panosunda görüntülenmek üzere API'ler hakkında bazı önemli istatistikleri tutar, ancak bunun ötesinde ayrıntılar gitmiş durumda.

API Yönetimi hizmetinde eventhub'a giriş ilkesini kullanarak, bir [Azure Etkinlik Hub'ına](../event-hubs/event-hubs-what-is-event-hubs.md)istek ve yanıttaki tüm ayrıntıları gönderebilirsiniz. API'lerinize gönderilen HTTP iletilerinden olay oluşturmak istemenizin çeşitli nedenleri vardır. Bazı örnekler arasında güncelleştirmelerin denetim izi, kullanım analitiği, özel durum uyarısı ve üçüncü taraf tümleştirmeleri sayılabilir.

Bu makalede, TÜM HTTP isteği ve yanıt iletisinin nasıl yakalanıp bir Olay Hub'ına gönderilen ve sonra bu iletiyi HTTP günlüğe kaydetme ve izleme hizmetleri sağlayan bir üçüncü taraf hizmetine nasıl iletilir gösterin.

## <a name="why-send-from-api-management-service"></a>Neden API Yönetim Hizmeti nden Gönder?
HTTP istek ve yanıtlarını yakalamak ve bunları günlük ve izleme sistemlerine beslemek için HTTP API çerçevelerine bağlanabilen HTTP ara yazılımları yazmak mümkündür. Bu yaklaşımın dezavantajı, HTTP ara yazılımının arka uç API'sine entegre edilmesi ve API platformuyla eşleşmesi gerektiğidir. Birden çok API varsa, her biri ara ware dağıtmak gerekir. Genellikle arka uç API'lerinin güncelleştirilememe nedeni vardır.

Günlüğe kaydetme altyapısıyla tümleştirmek için Azure API Yönetimi hizmetini kullanmak merkezi ve platformdan bağımsız bir çözüm sağlar. Kısmen Azure API Yönetimi'nin [coğrafi çoğaltma](api-management-howto-deploy-multi-region.md) yetenekleri nedeniyle ölçeklenebilir.

## <a name="why-send-to-an-azure-event-hub"></a>Neden bir Azure Etkinlik Hub'ına göndermissin?
Neden Azure Etkinlik Hub'larına özgü bir ilke oluşturduğunu sormak mantıklıdır? İsteklerimi günlüğe kaydetmek isteyebileceğim birçok farklı yer var. Neden istekleri doğrudan son hedefe göndermedin?  Bu da bir seçenek. Ancak, bir API yönetim hizmetinden günlük istekleri ni getirirken, günlük iletilerinin API'nin performansını nasıl etkilediğini göz önünde bulundurmak gerekir. Yükteki kademeli artışlar, sistem bileşenlerinin mevcut örneklerini artırarak veya coğrafi çoğaltmadan yararlanarak işlenebilir. Ancak, trafikteki kısa ani artışlar, yükleme altında günlüğe kaydetme altyapısına isteklerin yavaşlamaya başlaması durumunda isteklerin gecikmesine neden olabilir.

Azure Etkinlik Hub'ları, en çok API'deki HTTP isteği sayısından çok daha fazla sayıda olayla başa çıkma kapasitesine sahip büyük hacimli verileri girebilmek üzere tasarlanmıştır. Olay Hub'ı, API yönetim hizmetiniz ile iletileri depolayan ve işleyen altyapı arasında bir tür gelişmiş arabellek görevi görür. Bu, günlük altyapısı nedeniyle API performansınızın zarar lanmamasını sağlar.

Veriler bir Olay Hub'ına aktarıldıktan sonra, bu işlem devam edilir ve Olay Hub tüketicilerinin bu işlemi işlemesini bekler. Olay Hub'ı nasıl işlendiğiyle ilgilenmez, sadece iletinin başarıyla iletilmesini sağlamaya önem vermez.

Olay Hub'ları olayları birden çok tüketici grubuna akış yeteneğine sahiptir. Bu, olayların farklı sistemler tarafından işlenmesine olanak sağlar. Bu, api yönetimi hizmeti içinde API yönetimi hizmeti içinde ek gecikmeler koymadan birçok tümleştirme senaryosunu desteklemeyi sağlar, çünkü yalnızca bir olay oluşturulması gerekir.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Uygulama/http iletileri göndermek için bir ilke
Olay Hub'ı olay verilerini basit bir dize olarak kabul eder. O siyenin içindekiler sana bağlı. Bir HTTP isteğini paketleyip Olay Hub'larına gönderebilmek için dizeyi istek veya yanıt bilgileriyle biçimlendirmemiz gerekir. Bu gibi durumlarda, yeniden kullanabileceğimiz mevcut bir biçim varsa, kendi ayrıştırma kodumuzu yazmak zorunda olmayabiliriz. Başlangıçta HTTP istekleri ve yanıtları göndermek için [HAR](http://www.softwareishard.com/blog/har-12-spec/) kullanarak kabul etti. Ancak, bu biçim, Bir Dizi HTTP isteğini JSON tabanlı bir biçimde depolamak için optimize edin. Bu tel üzerinden HTTP mesajı geçen senaryo için gereksiz karmaşıklık ekledi zorunlu öğeleri bir dizi içeriyordu.

Alternatif bir seçenek, `application/http` HTTP belirtimi [RFC 7230'da](https://tools.ietf.org/html/rfc7230)açıklandığı gibi ortam türünü kullanmaktı. Bu ortam türü, aslında tel üzerinden HTTP iletileri göndermek için kullanılan tam biçimi kullanır, ancak tüm ileti başka bir HTTP isteğinin gövdesine konabilir. Bizim durumumuzda, biz sadece Olay Hub'larına göndermek için bizim mesaj olarak vücut kullanmak için gidiyoruz. Uygun olarak, Microsoft ASP.NET Web [API 2.2 İstemci](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kitaplıklarında bulunan ve bu biçimi `HttpRequestMessage` ayrıştırabilen ve yerel ve `HttpResponseMessage` nesnelere dönüştürebilen bir ayrıştırıcı vardır.

Bu iletiyi oluşturabilmek için Azure API Yönetimi'ndeki C# tabanlı [İlke ifadelerinden](/azure/api-management/api-management-policy-expressions) yararlanmamız gerekir. Azure Etkinlik Hub'larına BIR HTTP isteği iletisi gönderen ilke burada veda eder.

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
Bu ilke ifadesi hakkında bahsetmeye değer birkaç özel şey vardır. Oturum açma-olay hub ilkesi, API Yönetimi hizmeti içinde oluşturulan logger adını ifade eden logger-id adlı bir özniteliğe sahiptir. API Yönetimi hizmetinde bir Olay Hub logger'ının nasıl ayarlanabildiğini ayrıntıları belgede, [azure API Yönetiminde'daki Azure Etkinlik Hub'larına nasıl günlüğe kaydedilir.](api-management-howto-log-event-hubs.md) İkinci öznitelik, iletinin depolandığı bölüm olan Olay Hub'larına talimat veren isteğe bağlı bir parametredir. Olay Hub'ları ölçeklenebilirliği sağlamak için bölümler kullanır ve en az iki tane gerektirir. İletilerin sıralı teslimi yalnızca bir bölüm içinde garanti edilir. Event Hub'a iletiyi yerleştirmek için hangi bölüme talimat vermezsek, yükü dağıtmak için bir round-robin algoritması kullanır. Ancak, bu bazı iletilerimizin sıra dışı işlenmesine neden olabilir.

### <a name="partitions"></a>Bölümler
İletilerimizin tüketicilere sırayla teslim edilmesini sağlamak ve bölümlerin yük dağıtım yeteneğinden yararlanmak için, http istek iletilerini tek bir bölüme ve HTTP yanıt iletilerini ikinci bir bölüme göndermeyi seçtim. Bu eşit bir yük dağılımı sağlar ve tüm isteklerin sırayla tüketilmesini ve tüm yanıtların sırayla tüketilmesini garanti edebiliriz. Yanıtın ilgili istekten önce tüketilmesi mümkündür, ancak bu bir sorun olmadığı için, yanıtlarla ilgili istekleri ilişkilendirmek için farklı bir mekanizmaya sahibiz ve isteklerin her zaman yanıttan önce geldiğini biliyoruz.

### <a name="http-payloads"></a>HTTP yükleri
Bina sonra `requestLine`, biz istek vücut kesilir olup olmadığını görmek için kontrol edin. İstek gövdesi sadece 1024'e kesilir. Bu artırılabilir, ancak tek tek Olay Hub iletileri 256 KB ile sınırlıdır, bu nedenle bazı HTTP ileti gövdelerinin tek bir iletiye sığmaması olasıdır. Günlük ve analitiği yaparken önemli miktarda bilgi yalnızca HTTP istek satırından ve üstbilgilerden türetilebilir. Ayrıca, birçok API yalnızca küçük gövdeleri iade etmek ve böylece büyük gövdeleri kesildirerek bilgi değerinin kaybı, tüm vücut içeriğini tutmak için aktarım, işleme ve depolama maliyetlerinin azaltılmasına kıyasla oldukça düşüktür. Vücut işleme hakkında son bir not biz `true` vücut `As<string>()` içeriğini okuyorçünkü yönteme geçmek gerekir, ama aynı zamanda arka uç API vücut okumak mümkün olması gerekiyordu. Bu yönteme doğru geçerek, ikinci kez okunabilmesi için vücudun arabelleğe alınmasına neden oluruz. Bu, büyük dosyaların yüklenmesi veya uzun yoklama kullanan bir API'niz olup olmadığının farkında olmak önemlidir. Bu gibi durumlarda, hiç vücut okumaktan kaçınmak için en iyisi olacaktır.

### <a name="http-headers"></a>HTTP üstbilgi
HTTP Üstbilgi, basit bir anahtar/değer çifti biçiminde ileti biçimine aktarılabilir. Kimlik bilgilerini gereksiz yere sızdırmamak için bazı güvenlik hassas alanlarını sökmeyi seçtik. API anahtarlarının ve diğer kimlik bilgilerinin analitik amaçlarla kullanılması olası değildir. Kullanıcı ve kullandıkları belirli ürün üzerinde analiz yapmak istersek, bunu `context` nesneden alıp mesaja ekleyebiliriz.

### <a name="message-metadata"></a>İleti Meta verileri
Olay merkezine göndermek için iletinin tamamını yaparken, ilk satır aslında `application/http` iletinin bir parçası değildir. İlk satır, iletinin bir istek veya yanıt iletisi olup olmadığını ve istekleri yanıtla ilişkilendirmek için kullanılan bir ileti kimliğinden oluşan ek meta verilerdir. İleti kimliği, aşağıdaki gibi görünen başka bir ilke kullanılarak oluşturulur:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

İstek iletisini oluşturmuş, yanıt döndürülene kadar bir değişkende depolayabilir ve sonra isteği ve yanıtı tek bir ileti olarak göndermiş olabiliriz. Ancak, isteği ve yanıtı bağımsız olarak göndererek ve ikisini ilişkilendirmek için bir ileti kimliği kullanarak, ileti boyutunda biraz daha fazla esneklik elde ediyoruz, ileti sırasını korurken birden fazla bölümden yararlanabilme yeteneği ve istek görüntülenir daha erken bizim günlük panosunda. Geçerli bir yanıtın olay merkezine gönderilmediği bazı senaryolar da olabilir, büyük olasılıkla API Yönetimi hizmetindeki önemli bir istek hatası nedeniyle, ancak yine de isteğin kaydı var.

Yanıt http iletisi göndermek için ilke isteği ne kadar benzer ve böylece tam ilke yapılandırması aşağıdaki gibi görünüyor:

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

İlke, `set-variable` hem `log-to-eventhub` `<inbound>` bölümdeki hem de `<outbound>` bölümdeki ilke tarafından erişilebilen bir değer oluşturur.

## <a name="receiving-events-from-event-hubs"></a>Etkinlik Hub'larından etkinlik alma
Azure Event Hub'ından etkinlikler [AMQP protokolü](https://www.amqp.org/)kullanılarak alınır. Microsoft Service Bus ekibi, tüketen olayları kolaylaştırmak için istemci kitaplıklarını kullanıma sunmuş. Desteklenen iki farklı yaklaşım vardır, biri *Doğrudan Tüketici* olmak, diğeri `EventProcessorHost` ise sınıfı kullanmaktır. Bu iki yaklaşıma örnek olarak [Olay Hub'ları Programlama Kılavuzu'nda](../event-hubs/event-hubs-programming-guide.md)bulunabilir. Farklılıkların kısa versiyonu, `Direct Consumer` tam kontrol sağlar ve `EventProcessorHost` bazı sıhhi tesisat sizin için çalışır ama bu olayları işlemek hakkında bazı varsayımlar yapar.

### <a name="eventprocessorhost"></a>EventProcessorHost
Bu örnekte, basitlik `EventProcessorHost` için kullanırız, ancak bu senaryo için en iyi seçim olmayabilir. `EventProcessorHost`belirli bir olay işlemci sınıfı içinde iş parçacığı sorunları hakkında endişelenmenize gerek olmadığından emin olmak için zor iş yok. Ancak, bizim senaryoda, biz sadece başka bir biçime ileti dönüştürme ve başka bir hizmet için bir async yöntemi kullanarak geçen. Paylaşılan durumu güncelleştirmeye gerek yoktur ve bu nedenle iş parçacığı sorunları riski yoktur. Çoğu senaryo için, `EventProcessorHost` muhtemelen en iyi seçimdir ve kesinlikle daha kolay bir seçenektir.

### <a name="ieventprocessor"></a>IEventProcessor
Kullanırken `EventProcessorHost` temel kavram yöntemi `IEventProcessor` `ProcessEventAsync`içeren arabirim, bir uygulama oluşturmaktır. Bu yöntemin özü burada gösterilmiştir:

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

EventData nesnelerinin listesi yönteme aktarılır ve bu liste üzerinde yineleniriz. Her yöntemin baytları bir HttpMessage nesnesine ayrıştırılır ve bu nesne IHttpMessageProcessor örneğine aktarılır.

### <a name="httpmessage"></a>HttpMesaj
Örnek `HttpMessage` üç veri parçası içerir:

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

Örnek, `HttpMessage` HTTP `MessageId` isteğini ilgili HTTP yanıtına bağlamamızı sağlayan bir GUID ve nesnenin bir HttpRequestMessage ve HttpResponseMessage örneğini içermesi gerektiğini tanımlayan bir boolean değeri içerir. Dahili HTTP sınıflarını `System.Net.Http`kullanarak, ben dahil `application/http` ayrıştırma kodu yararlanmak başardı . `System.Net.Http.Formatting`  

### <a name="ihttpmessageprocessor"></a>IhttpMessageProcessor
Örnek `HttpMessage` daha sonra, etkinliğin `IHttpMessageProcessor`alıcı ve yorumunu Azure Event Hub'ından ayırmak ve gerçek olarak işlenmesini ayırmak için oluşturduğum bir arabirim olan uygulamaya iletilir.

## <a name="forwarding-the-http-message"></a>HTTP iletisini iletme
Bu örnek için, [ben Moesif API Analytics](https://www.moesif.com)için HTTP İstek itmek için ilginç olacağına karar verdi. Moesif, HTTP analizi ve hata ayıklama konusunda uzmanlaşmış bulut tabanlı bir hizmettir. Onlar ücretsiz bir katman var, bu yüzden denemek kolaydır ve bize GERÇEK zamanlı olarak API Yönetimi hizmeti üzerinden akan HTTP isteklerini görmenizi sağlar.

Uygulama `IHttpMessageProcessor` şuna benziyor.

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

Moesif `MoesifHttpMessageProcessor` için, HTTP olay verilerini kendi hizmetine taşımayı kolaylaştıran bir [C# API kitaplığından](https://www.moesif.com/docs/api?csharp#events) yararlanır. Moesif Collector API'ye HTTP verilerini göndermek için bir hesap ve Uygulama Kimliği gerekir. [Moesif'in web sitesinde](https://www.moesif.com) bir hesap oluşturarak Moesif Uygulama Kimliği'ni alır sınız ve ardından _Sağ Üst Menü_ -> _Uygulama Kurulumu'na_gidersiniz.

## <a name="complete-sample"></a>Tam örnek
Örnek için [kaynak kodu](https://github.com/dgilling/ApimEventProcessor) ve testleri GitHub'dadır. Örneği kendiniz çalıştırmak için bir [API Yönetim Hizmetine,](get-started-create-service-instance.md) [bağlı bir Olay Hub'ına](api-management-howto-log-event-hubs.md)ve bir Depolama [Hesabı'na](../storage/common/storage-create-storage-account.md) ihtiyacınız var.   

Örnek olay hub gelen olayları dinler, bir Moesif `EventRequestModel` ve `EventResponseModel` nesneleri dönüştürür ve daha sonra Moesif Collector API onları iletir basit bir Konsol uygulamasıdır.

Aşağıdaki animasyonlu resimde, Geliştirici Portalı'ndaki bir API'ye yapılan bir istek, alınan, işlenen ve iletilen iletiyi gösteren Konsol uygulaması ve ardından Olay Akışı'nda görünen istek ve yanıtı görebilirsiniz.

![İsteğin Runscope'a iletilmesinin gösterimi](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Özet
Azure API Yönetimi hizmeti, API'lerinize gidip gelen HTTP trafiğini yakalamak için ideal bir yer sağlar. Azure Etkinlik Hub'ları, bu trafiği yakalamak ve günlüğe kaydetme, izleme ve diğer gelişmiş analizler için ikincil işleme sistemlerine beslemek için yüksek ölçeklenebilir, düşük maliyetli bir çözümdür. Moesif gibi üçüncü taraf trafik izleme sistemlerine bağlanmak birkaç düzine kod satırı kadar basittir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Etkinlik Hub'ları hakkında daha fazla bilgi edinin
  * [Azure Etkinlik Hub'ları ile başlayın](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Yönetimi ve Olay Hub'ları entegrasyonu hakkında daha fazla bilgi edinin
  * [Azure API Yönetimi'nde etkinlikleri Azure Etkinlik Hub'larına kaydetme](api-management-howto-log-event-hubs.md)
  * [Logger varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [log-to-eventhub ilke başvurusu](/azure/api-management/api-management-advanced-policies#log-to-eventhub)
