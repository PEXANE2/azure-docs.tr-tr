---
title: Azure İşlevleri uygulamasını geliştirme & yapılandırma - Azure SignalR
description: Azure İşlevleri ve Azure SinyalR Hizmeti kullanarak sunucusuz gerçek zamanlı uygulamaların nasıl geliştirilip yapılandırılabildiğini anlatan ayrıntılar
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.openlocfilehash: e1157a695d34c75b237391427b37365421366ef8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523179"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma

Azure İşlevleri uygulamaları, gerçek zamanlı özellikler eklemek için [Azure Sinyal Hizmeti bağlayıcılarından](../azure-functions/functions-bindings-signalr-service.md) yararlanabilir. İstemci uygulamaları, Azure Sinyal Hizmeti'ne bağlanmak ve gerçek zamanlı iletiler almak için çeşitli dillerde kullanılabilen istemci SDK'larını kullanır.

Bu makalede, SignalR Hizmeti ile tümleşik bir Azure İşlevi uygulaması geliştirme ve yapılandırma kavramları açıklanmaktadır.

## <a name="signalr-service-configuration"></a>SignalR Service yapılandırması

Azure SignalR Hizmeti farklı modlarda yapılandırılabilir. Azure İşlevleri ile kullanıldığında, hizmetin *Serverless* modunda yapılandırılması gerekir.

Azure portalında SignalR Hizmeti kaynağınızın *Ayarlar* sayfasını bulun. Hizmet *modunu* *Serverless*olarak ayarlayın.

![SignalR Servis Modu](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Azure İşlevleri geliştirme

Azure İşlevleri ve Azure SignalR Hizmeti ile oluşturulan sunucusuz bir gerçek zamanlı uygulama için genelde iki Azure İşlevi gerekir:

* Müşterinin geçerli bir SignalR Hizmeti erişim belirteci ve hizmet uç noktası URL’si almak için çağırdığı “negotiate” işlevi
* İleti gönderen veya grup üyeliğini yöneten bir ya da daha fazla işlev

### <a name="negotiate-function"></a>müzakere fonksiyonu

Müşteri uygulaması, Azure SignalR Hizmetine bağlanmak için geçerli bir erişim jetonu gerektirir. Erişim jetonu anonim olabilir veya belirli bir kullanıcı kimliğine kimlik doğrulaması yapılabilir. Sunucusuz SignalR Hizmeti uygulamaları, SignalR Hizmeti bitiş noktası URL'si gibi bir belirteç ve diğer bağlantı bilgilerini elde etmek için "anlaşma" adlı bir HTTP bitiş noktası gerektirir.

Bağlantı bilgileri nesnesini oluşturmak için HTTP tetiklenen Azure İşi'ni ve *SignalRConnectionInfo* giriş bağlamayı kullanın. İşlevin `/negotiate`' de biten bir HTTP rotası olmalıdır.

Müzakere işlevinin nasıl oluşturulacığı hakkında daha fazla bilgi için [ *SignalRConnectionInfo* input bağlayıcı bağışçılıÄ](../azure-functions/functions-bindings-signalr-service-input.md)ına bakın.

Kimlik doğrulaması bir belirteç oluşturma hakkında bilgi edinmek [için, Uygulama Hizmeti Kimlik Doğrulamasını Kullanma'ya](#using-app-service-authentication)bakın.

### <a name="sending-messages-and-managing-group-membership"></a>İleti gönderme ve grup üyeliğini yönetme

Azure *SignalR* Hizmetine bağlı istemcilere ileti göndermek için SignalR çıkış bağlamayı kullanın. İletileri tüm istemcilere yayınlayabilir veya belirli bir kullanıcı kimliğiyle kimlik doğrulaması yapılan veya belirli bir gruba eklenen bir istemci alt kümesine gönderebilirsiniz.

Kullanıcılar bir veya daha fazla gruba eklenebilir. Ayrıca, kullanıcıları gruplara/gruplara eklemek veya kaldırmak için *SignalR* çıkış bağlamasını da kullanabilirsiniz.

Daha fazla bilgi için [ *SignalR* çıkış bağlama referansına](../azure-functions/functions-bindings-signalr-service-output.md)bakın.

### <a name="signalr-hubs"></a>Sinyal Merkezi Hub'ları

SignalR "hub" kavramına sahiptir. Azure İşlevleri'nden gönderilen her istemci bağlantısı ve her ileti belirli bir hub'a kapsamlıdır. Bağlantılarınızı ve iletilerinizi mantıksal ad alanlarına ayırmak için hub'ları kullanabilirsiniz.

## <a name="client-development"></a>Müşteri geliştirme

SignalR istemci uygulamaları, Azure SignalR Hizmeti'ne kolayca bağlanmak ve iletialmak için SignalR istemci SDK'dan birkaç dilden birinde kullanabilir.

### <a name="configuring-a-client-connection"></a>İstemci bağlantısını yapılandırma

SignalR Hizmetine bağlanmak için istemcinin aşağıdaki adımlardan oluşan başarılı bir bağlantı anlaşması tamamlaması gerekir:

1. Geçerli bağlantı bilgilerini elde etmek için yukarıda tartışılan *müzakere* http bitiş noktası için bir istekte bulunun
1. Hizmet bitiş noktası URL'sini ve *anlaşma* bitiş noktasından elde edilen erişim belirteci'ni kullanarak SignalR Hizmetine bağlanın

SignalR istemci SDK'ları zaten anlaşma el sıkışması gerçekleştirmek için gereken mantığı içerir. Müzakere bitiş noktasının URL'sini, `negotiate` segmenti eksi, SDK'nınurl'sine `HubConnectionBuilder`geçirin. JavaScript'te bir örnek aşağıda verilmiştir:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Sözleşmeye göre, SDK URL'ye otomatik olarak eklenir `/negotiate` ve bu url'yi görüşmeye başlamak için kullanır.

> [!NOTE]
> Bir tarayıcıda JavaScript/TypeScript SDK kullanıyorsanız, İşlev Uygulamanızda [kökenler arası kaynak paylaşımını (CORS) etkinleştirmeniz](#enabling-cors) gerekir.

SignalR istemci SDK'nın nasıl kullanılacağı hakkında daha fazla bilgi için dilinizle ilgili belgelere bakın:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [Javascript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>İstemciden hizmete ileti gönderme

SignalR SDK istemci uygulamalarının bir SignalR hub'ında arka uç mantığını çağırmasına izin vermesine rağmen, Azure İşleriyle SignalR Hizmetini kullandığınızda bu işlevsellik henüz desteklenmez. Azure İşlevlerini çağırmak için HTTP isteklerini kullanın.

## <a name="azure-functions-configuration"></a>Azure Fonksiyonları yapılandırması

Azure SinyalR Hizmeti ile tümleşen Azure İşlevi uygulamaları, [sürekli dağıtım,](../azure-functions/functions-continuous-deployment.md) [zip dağıtımı](../azure-functions/deployment-zip-push.md)ve [paketten çalıştırma](../azure-functions/run-functions-from-deployment-package.md)gibi teknikler kullanılarak, herhangi bir tipik Azure İşlevi uygulaması gibi dağıtılabilir.

Ancak, SignalR Hizmeti bağlamalarını kullanan uygulamalar için birkaç özel husus vardır. İstemci bir tarayıcıda çalışıyorsa, CORS etkinleştirilmelidir. Uygulama kimlik doğrulaması gerektiriyorsa, anlaşma bitiş noktasını App Service Authentication ile tümleştirebilirsiniz.

### <a name="enabling-cors"></a>CORS'u Etkinleştirme

JavaScript/TypeScript istemcisi, bağlantı anlaşmasını başlatmak için anlaşma işlevine HTTP isteklerini yapar. İstemci uygulaması Azure İşlevi uygulamasından farklı bir etki alanında barındırıldığında, İşlev uygulamasında çapraz kaynak paylaşımının (CORS) etkinleştirilmesi gerekir veya tarayıcı istekleri engeller.

#### <a name="localhost"></a>Localhost

Yerel bilgisayarınızda İşlev uygulamasını çalıştırırken, CORS'ü etkinleştirmek için `Host` *local.settings.json'a* bir bölüm ekleyebilirsiniz. Bölüme `Host` iki özellik ekleyin:

* `CORS`- istemci uygulamasının kaynağı olan temel URL'yi girin
* `CORSCredentials`- "Kimlik `true` Bilgileri" isteklerine izin verecek şekilde ayarlayın

Örnek:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="cloud---azure-functions-cors"></a>Bulut - Azure İşlerİ CORS

Bir Azure İşlevi uygulamasında CORS'i etkinleştirmek için, Azure portalındaki İşlev uygulamanızın *Platform özellikleri* sekmesialtındaki CORS yapılandırma ekranına gidin.

> [!NOTE]
> CORS yapılandırması henüz Azure İşlevler Linux Tüketim planında kullanıma sunulmadı. CORS'i etkinleştirmek için [Azure API Yönetimi'ni](#cloud---azure-api-management) kullanın.

SignalR istemcisinin anlaşma işlevini çağırabilmesi için Access-Control-Allow-Credentials özellikli CORS etkinleştirilmelidir. Etkinleştirmek için onay kutusunu seçin.

İzin *Verilen başlangıçlar* bölümünde, web uygulamanızın başlangıç temel URL'sini içeren bir giriş ekleyin.

![CORS'un Yapılandırılması](media/signalr-concept-serverless-development-config/cors-settings.png)

#### <a name="cloud---azure-api-management"></a>Bulut - Azure API Yönetimi

Azure API Yönetimi, varolan arka uç hizmetlerine özellikler ekleyen bir API ağ geçidi sağlar. İşlev uygulamanıza CORS eklemek için kullanabilirsiniz. Bu ödeme başına fiyatlandırma ve aylık ücretsiz hibe ile bir tüketim katmanı sunuyor.

[Azure İşlevi uygulamasının](../api-management/import-function-app-as-api.md)nasıl içe aktarılabildiğini öğrenmek için API Yönetimi belgelerine bakın. İçe aktarıladıktan sonra, Access-Control-Allow-Credentials desteğiyle CORS'i etkinleştirmek için gelen bir ilke ekleyebilirsiniz.

```xml
<cors allow-credentials="true">
  <allowed-origins>
    <origin>https://azure-samples.github.io</origin>
  </allowed-origins>
  <allowed-methods>
    <method>GET</method>
    <method>POST</method>
  </allowed-methods>
  <allowed-headers>
    <header>*</header>
  </allowed-headers>
  <expose-headers>
    <header>*</header>
  </expose-headers>
</cors>
```

SignalR istemcilerinizi API Yönetimi URL'sini kullanacak şekilde yapılandırın.

### <a name="using-app-service-authentication"></a>Uygulama Hizmeti Kimlik Doğrulamasını Kullanma

Azure İşlemeler, Facebook, Twitter, Microsoft Hesabı, Google ve Azure Active Directory gibi popüler sağlayıcıları destekleyen yerleşik kimlik doğrulamaya sahiptir. Bu özellik, kullanıcı kimliğine kimlik doğrulanmış Olan Azure SinyalR Hizmetine bağlantılar oluşturmak için *SignalRConnectionInfo* bağlama ile tümleştirilebilir. Uygulamanız, o kullanıcı kimliğini hedefleyen *SignalR* çıkış bağlamasını kullanarak ileti gönderebilir.

Azure portalında, İşlev uygulamanızın *Platform özellikleri* sekmesinde Kimlik *Doğrulama/yetkilendirme* ayarları penceresini açın. Seçtiğiniz bir kimlik sağlayıcısını kullanarak kimlik doğrulamasını yapılandırmak için [Uygulama Hizmeti Kimlik Doğrulaması](../app-service/overview-authentication-authorization.md) için belgeleri izleyin.

Yapılandırıldıktan sonra, kimlik doğrulaması `x-ms-client-principal-name` `x-ms-client-principal-id` http istekleri, sırasıyla kimlik doğrulaması yapılankimliğin kullanıcı adı ve kullanıcı kimliğini içeren üstbilgiler içerir ve üstbilgiler içerir.

Bu üstbilgileri *SignalRConnectionInfo* bağlama yapılandırmanızda kimlik doğrulaması bağlantılar oluşturmak için kullanabilirsiniz. Burada `x-ms-client-principal-id` üstbilgi kullanan bir örnek C# müzakere işlevi verilmiştir.

```csharp
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

Daha sonra bir SignalR iletisinin `UserId` özelliğini ayarlayarak bu kullanıcıya ileti gönderebilirsiniz.

```csharp
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

Diğer diller hakkında bilgi için Azure İşlevleri başvurusu için [Azure Sinyal RHizmeti bağlayıcılarına](../azure-functions/functions-bindings-signalr-service.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure İşlevlerini kullanarak sunucusuz SignalR Hizmeti uygulamalarını nasıl geliştirip yapılandırabileceğinizi öğrendiniz. [SignalR Hizmetine genel bakış sayfasındaki](index.yml)hızlı başlangıçlardan veya öğreticilerden birini kullanarak kendiniz bir uygulama oluşturmayı deneyin.