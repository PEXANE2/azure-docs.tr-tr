---
title: Birden çok örnekle ölçeklendirin - Azure SignalR Hizmeti
description: Birçok ölçeklendirme senaryosunda, müşterinin büyük ölçekli bir dağıtım oluşturmak için genellikle birden çok örneği sağlaması ve bunları birlikte kullanmak üzere yapılandırması gerekir. Örneğin, parçalama birden çok örnek desteği gerektirir.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: zhshang
ms.openlocfilehash: 43d703312cbc1fc067a2d51d5623ed028ba01405
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74158164"
---
# <a name="how-to-scale-signalr-service-with-multiple-instances"></a>SignalR Hizmeti birden çok örnekle nasıl ölçeklendirilir?
En son SignalR Hizmeti SDK, SignalR Hizmeti örnekleri için birden çok uç noktayı destekler. Bu özelliği eşzamanlı bağlantıları ölçeklendirmek veya bölgeler arası ileti için kullanabilirsiniz.

## <a name="for-aspnet-core"></a>ASP.NET Çekirdek için

### <a name="how-to-add-multiple-endpoints-from-config"></a>Config'den birden çok uç nokta nasıl eklenir?

Anahtarlı `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR Service bağlantı dizesi için config.

Anahtar `Azure:SignalR:ConnectionString:`, nesnenin özellikleri, `Azure:SignalR:ConnectionString:{Name}:{EndpointType}` `Name` `EndpointType` `ServiceEndpoint` biçimi ve özellikleri olmalıdır ve koddan erişilebilir olarak başlar.

Aşağıdaki `dotnet` komutları kullanarak birden çok örnek bağlantı dizeleri ekleyebilirsiniz:

```batch
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-a <ConnectionString1>
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-b:primary <ConnectionString2>
dotnet user-secrets set Azure:SignalR:ConnectionString:backup:secondary <ConnectionString3>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Koddan birden çok uç nokta nasıl eklenir?

Azure `ServicEndpoint` Sinyal Hizmeti bitiş noktasının özelliklerini açıklamak için bir sınıf tanıtılır.
Azure SignalR Service SDK'yı kullanırken birden çok örnek uç noktanızı yapılandırabilirsiniz:
```cs
services.AddSignalR()
        .AddAzureSignalR(options => 
        {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString0>"),
                new ServiceEndpoint("<ConnectionString1>", type: EndpointType.Primary, name: "east-region-a"),
                new ServiceEndpoint("<ConnectionString2>", type: EndpointType.Primary, name: "east-region-b"),
                new ServiceEndpoint("<ConnectionString3>", type: EndpointType.Secondary, name: "backup"),
            };
        });
```

### <a name="how-to-customize-endpoint-router"></a>Uç nokta yönlendiricinasıl özelleştirilir?

Varsayılan olarak, SDK uç noktaları almak için [Varsayılan EndpointRouter](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/EndpointRouters/DefaultEndpointRouter.cs) kullanır.

#### <a name="default-behavior"></a>Varsayılan davranış 
1. İstemci istek yönlendirme

    Ne `/negotiate` zaman uygulama sunucusu ile istemci. Varsayılan olarak, SDK kullanılabilir hizmet bitiş noktaları kümesinden rasgele bir bitiş noktası **seçer.**

2. Sunucu ileti yönlendirme

    *belirli bir **bağlantıya***ileti gönderdiğinde ve hedef bağlantı geçerli sunucuya yönlendirildiğinde, ileti doğrudan bağlı uç noktasına gider. Aksi takdirde, iletiler her Azure SinyalR bitiş noktasına yayınlanır.

#### <a name="customize-routing-algorithm"></a>Yönlendirme algoritması özelleştirme
İletilerin hangi uç noktalara gitmesi gerektiğini belirlemek için özel bir bilgiye sahip olduğunuzda kendi yönlendiricinizi oluşturabilirsiniz.

Özel bir yönlendirici, aşağıdaki örnek olarak tanımlanır. `east-` `east`

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override IEnumerable<ServiceEndpoint> GetEndpointsForGroup(string groupName, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the group broadcast behavior, if the group name starts with "east-", only send messages to endpoints inside east
        if (groupName.StartsWith("east-"))
        {
            return endpoints.Where(e => e.Name.StartsWith("east-"));
        }

        return base.GetEndpointsForGroup(groupName, endpoints);
    }
}
```

Uç noktaları seçmek için varsayılan anlaşma zama davranışını geçersiz kılan aşağıdaki başka bir örnek, uygulama sunucusunun bulunduğu yere bağlıdır.

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(HttpContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (endpointName.Count == 0)
        {
            context.Response.StatusCode = 400;
            var response = Encoding.UTF8.GetBytes("Invalid request");
            context.Response.Body.Write(response, 0, response.Length);
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

Yönlendiriciyi AŞAĞıDAKIleri kullanarak DI konteynerine kaydetmeyi unutmayın:

```cs
services.AddSingleton(typeof(IEndpointRouter), typeof(CustomRouter));
services.AddSignalR()
        .AddAzureSignalR(
            options => 
            {
                options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
            });
```

## <a name="for-aspnet"></a>ASP.NET için

### <a name="how-to-add-multiple-endpoints-from-config"></a>Config'den birden çok uç nokta nasıl eklenir?

Anahtarlı `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR Service bağlantı dizesi için config.

Anahtar `Azure:SignalR:ConnectionString:`, nesnenin özellikleri, `Azure:SignalR:ConnectionString:{Name}:{EndpointType}` `Name` `EndpointType` `ServiceEndpoint` biçimi ve özellikleri olmalıdır ve koddan erişilebilir olarak başlar.

Birden çok örnek bağlantı dizeleri `web.config`ekleyebilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>
    <add name="Azure:SignalR:ConnectionString" connectionString="<ConnectionString1>"/>
    <add name="Azure:SignalR:ConnectionString:en-us" connectionString="<ConnectionString2>"/>
    <add name="Azure:SignalR:ConnectionString:zh-cn:secondary" connectionString="<ConnectionString3>"/>
    <add name="Azure:SignalR:ConnectionString:Backup:secondary" connectionString="<ConnectionString4>"/>
  </connectionStrings>
  ...
</configuration>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Koddan birden çok uç nokta nasıl eklenir?

Azure `ServicEndpoint` Sinyal Hizmeti bitiş noktasının özelliklerini açıklamak için bir sınıf tanıtılır.
Azure SignalR Service SDK'yı kullanırken birden çok örnek uç noktanızı yapılandırabilirsiniz:

```cs
app.MapAzureSignalR(
    this.GetType().FullName, 
    options => {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString1>"),
                new ServiceEndpoint("<ConnectionString2>"),
                new ServiceEndpoint("<ConnectionString3>"),
            }
        });
```

### <a name="how-to-customize-router"></a>Yönlendirici nasıl özelleştirilir?

ASP.NET SignalR ve ASP.NET Core SignalR arasındaki tek `GetNegotiateEndpoint`fark için http bağlam türüdür. SignalR ASP.NET için [IOwinContext](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR.AspNet/EndpointRouters/DefaultEndpointRouter.cs#L19) türündedir.

Aşağıda ASP.NET SignalR için özel müzakere örneği:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(IOwinContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (string.IsNullOrEmpty(endpointName))
        {
            context.Response.StatusCode = 400;
            context.Response.Write("Invalid request.");
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

Yönlendiriciyi AŞAĞıDAKIleri kullanarak DI konteynerine kaydetmeyi unutmayın:

```cs
var hub = new HubConfiguration();
var router = new CustomRouter();
hub.Resolver.Register(typeof(IEndpointRouter), () => router);
app.MapAzureSignalR(GetType().FullName, hub, options => {
    options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
});
```

## <a name="configuration-in-cross-region-scenarios"></a>Bölgeler arası senaryolarda yapılandırma

Nesnenin `ServiceEndpoint` değeri `EndpointType` `primary` olan bir `secondary`özelliği vardır veya .

`primary`uç noktaları istemci trafiğini almak için tercih edilen uç noktalarıdır ve daha güvenilir ağ bağlantılarına sahip olarak kabul edilir; `secondary` uç noktaların daha az güvenilir ağ bağlantıları olduğu kabul edilir ve yalnızca sunucunun istemci trafiğine götürülmesi için kullanılır, örneğin, iletileri yayınlamak için değil, istemciyi sunucu trafiğine götürmek için değil.

Bölgeler arası durumlarda ağ kararsız olabilir. *Doğu ABD'de*bulunan bir uygulama sunucusu için, aynı Doğu *ABD* bölgesinde bulunan SignalR `primary` Hizmeti bitiş noktası olarak `secondary`yapılandırılabilir ve diğer bölgelerde uç noktalar olarak işaretlenmiştir. Bu yapılandırmada, diğer bölgelerdeki hizmet bitiş noktaları bu *Doğu ABD* uygulama sunucusundan ileti **alabilir,** ancak bu uygulama sunucusuna yönlendirilen **bölgeler arası** istemciler olmayacaktır. Mimari aşağıdaki diyagramda gösterilmiştir:

![Çapraz Geo Infra](./media/signalr-howto-scale-multi-instances/cross_geo_infra.png)

İstemci `/negotiate` varsayılan yönlendiriciyle uygulama sunucusuyla denediğinde, SDK kullanılabilir `primary` uç noktalar kümesinden rasgele bir bitiş noktası **seçer.** Birincil bitiş noktası kullanılamıyorsa, SDK tüm kullanılabilir `secondary` uç noktalardan rasgele **seçer.** Sunucu ve hizmet bitiş noktası arasındaki bağlantı canlı olduğunda bitiş noktası **kullanılabilir** olarak işaretlenir.

Bölgeler arası senaryoda, bir `/negotiate` istemci *Doğu ABD'de*barındırılan uygulama sunucusuyla denemeler yaptığında, varsayılan olarak her zaman aynı bölgede bulunan `primary` bitiş noktasını döndürür. Tüm *Doğu ABD* uç noktaları kullanılamıyorsa, istemci diğer bölgelerdeki uç noktalara yönlendirilir. Aşağıdaki fail-over bölümü senaryoyu ayrıntılı olarak açıklamaktadır.

![Normal Müzakere](./media/signalr-howto-scale-multi-instances/normal_negotiate.png)

## <a name="fail-over"></a>Başarısız

Tüm `primary` uç noktalar kullanılamıyorsa, `/negotiate` istemcinin kullanılabilir `secondary` uç noktalardan seçtiği noktalar. Bu hata mekanizması, her bitiş noktasının `primary` en az bir uygulama sunucusu için bitiş noktası olarak hizmet etmesini gerektirir.

![Başarısız](./media/signalr-howto-scale-multi-instances/failover_negotiate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, ölçekleme, parçalama ve bölgeler arası senaryolar için aynı uygulamada birden çok örneği yapılandırmayı öğrendiniz.

Birden çok uç nokta destekleri, yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryolarında da kullanılabilir.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ve yüksek kullanılabilirlik için Kurulum SignalR Hizmeti](./signalr-concept-disaster-recovery.md)
