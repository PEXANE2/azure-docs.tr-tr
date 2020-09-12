---
title: Birden çok örnek ile ölçeklendirme-Azure SignalR hizmeti
description: Birçok ölçeklendirme senaryosunda, müşterinin genellikle birden çok örnek sağlaması ve büyük ölçekli bir dağıtım oluşturmak için bunları birlikte kullanmak üzere yapılandırması gerekir. Örneğin, parça birden çok örnek desteği gerektirir.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 03/27/2019
ms.author: zhshang
ms.openlocfilehash: fd6ac8c4d4fc4c3fec4f549f8ef4f955e2b1c637
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89439223"
---
# <a name="how-to-scale-signalr-service-with-multiple-instances"></a>SignalR hizmetini birden çok örnek ile ölçeklendirme
En son SignalR hizmeti SDK 'Sı, SignalR hizmet örnekleri için birden çok uç noktayı destekler. Bu özelliği, eş zamanlı bağlantıları ölçeklendirmek veya bölgeler arası mesajlaşma için kullanmak üzere kullanabilirsiniz.

## <a name="for-aspnet-core"></a>ASP.NET Core için

### <a name="how-to-add-multiple-endpoints-from-config"></a>Yapılandırmadan birden fazla uç nokta nasıl eklenir?

Anahtar `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR hizmeti bağlantı dizesi ile yapılandırma.

Anahtar ile başlıyorsa, `Azure:SignalR:ConnectionString:` `Azure:SignalR:ConnectionString:{Name}:{EndpointType}` burada `Name` ve `EndpointType` nesnesinin özellikleri, `ServiceEndpoint` ve koddan erişilebilir olmalıdır.

Aşağıdaki komutları kullanarak birden çok örnek bağlantı dizesi ekleyebilirsiniz `dotnet` :

```cmd
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-a <ConnectionString1>
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-b:primary <ConnectionString2>
dotnet user-secrets set Azure:SignalR:ConnectionString:backup:secondary <ConnectionString3>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Koddan birden fazla uç nokta nasıl eklenir?

Bir `ServicEndpoint` Azure SignalR hizmeti uç noktasının özelliklerini açıklamaya yönelik bir sınıf sunulmuştur.
Azure SignalR Service SDK kullanırken şu şekilde birden çok örnek uç noktası yapılandırabilirsiniz:
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

### <a name="how-to-customize-endpoint-router"></a>Uç nokta yönlendirici nasıl özelleştirilir?

Varsayılan olarak SDK, son noktaları almak için [Defaultendpointrouter](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/EndpointRouters/DefaultEndpointRouter.cs) kullanır.

#### <a name="default-behavior"></a>Varsayılan davranış 
1. İstemci isteği yönlendirme

    `/negotiate`Uygulama sunucusuyla istemci olduğunda. Varsayılan olarak SDK, kullanılabilir hizmet uç noktaları kümesinden bir uç noktayı **rastgele seçer** .

2. Sunucu iletisi yönlendirme

    * Belirli bir * * bağlantısına ileti gönderiyor * * * ve hedef bağlantı geçerli sunucuya yönlendirilse, ileti doğrudan o bağlı uç noktaya gider. Aksi takdirde, iletiler her Azure SignalR uç noktasına alınır.

#### <a name="customize-routing-algorithm"></a>Yönlendirme algoritmasını özelleştirme
İletilerin gitmesi gereken uç noktaları belirlemek için özel bilginiz varsa, kendi yönlendiricinizi oluşturabilirsiniz.

Bir özel yönlendirici aşağıda, ile başlayan gruplar `east-` her zaman adlı bir uç noktaya git örneği olarak tanımlanmıştır `east` :

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

Aşağıdaki diğer bir örnek, uç noktaların seçimi için varsayılan anlaşma davranışını geçersiz kılan, uygulama sunucusunun bulunduğu yere bağlıdır.

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

Şunu kullanarak şu şekilde yönlendirici için olan yönlendiriciyi kaydetmeyi unutmayın:

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

### <a name="how-to-add-multiple-endpoints-from-config"></a>Yapılandırmadan birden fazla uç nokta nasıl eklenir?

Anahtar `Azure:SignalR:ConnectionString` veya `Azure:SignalR:ConnectionString:` SignalR hizmeti bağlantı dizesi ile yapılandırma.

Anahtar ile başlıyorsa, `Azure:SignalR:ConnectionString:` `Azure:SignalR:ConnectionString:{Name}:{EndpointType}` burada `Name` ve `EndpointType` nesnesinin özellikleri, `ServiceEndpoint` ve koddan erişilebilir olmalıdır.

Öğesine birden çok örnek bağlantı dizesi ekleyebilirsiniz `web.config` :

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

### <a name="how-to-add-multiple-endpoints-from-code"></a>Koddan birden fazla uç nokta nasıl eklenir?

Bir `ServicEndpoint` Azure SignalR hizmeti uç noktasının özelliklerini açıklamaya yönelik bir sınıf sunulmuştur.
Azure SignalR Service SDK kullanırken şu şekilde birden çok örnek uç noktası yapılandırabilirsiniz:

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

ASP.NET SignalR ve ASP.NET Core SignalR arasındaki tek fark, için http bağlam türüdür `GetNegotiateEndpoint` . ASP.NET SignalR için, bu, [ıowincontext](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR.AspNet/EndpointRouters/DefaultEndpointRouter.cs#L19) türüdür.

Aşağıda ASP.NET SignalR için özel anlaşma örneği verilmiştir:

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

Şunu kullanarak şu şekilde yönlendirici için olan yönlendiriciyi kaydetmeyi unutmayın:

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

`ServiceEndpoint`Nesne, `EndpointType` veya değerine sahip bir özelliğe `primary` sahiptir `secondary` .

`primary` uç noktalar, istemci trafiği almak için tercih edilen uç noktalardır ve daha güvenilir ağ bağlantılarına sahip olduğu kabul edilir; `secondary` uç noktalar, daha az güvenilir ağ bağlantılarına sahip olduğu kabul edilir ve yalnızca, istemci trafiği için sunucu trafiği almak için değil, bir yayın iletileri almak için kullanılır.

Bölgeler arası durumlarda ağ kararsız olabilir. *Doğu ABD*bulunan bir uygulama sunucusu için, aynı *Doğu ABD* bölgesinde yer alan SignalR hizmeti uç noktası, `primary` olarak işaretlenen diğer bölgelerde ve uç noktalar olarak yapılandırılabilir `secondary` . Bu yapılandırmada, diğer bölgelerdeki hizmet uç noktaları bu *Doğu ABD* App Server 'dan iletiler **alabilir** , ancak bu uygulama sunucusuna hiçbir **çapraz bölge** istemcisi yönlendirilmeyecektir. Mimari aşağıdaki diyagramda gösterilmiştir:

![Çapraz coğrafi bölgeler](./media/signalr-howto-scale-multi-instances/cross_geo_infra.png)

Bir istemci, `/negotiate` varsayılan yönlendirici ile App Server ile çalıştığında, SDK, kullanılabilir uç noktalar kümesinden bir uç noktayı **rastgele seçer** `primary` . Birincil uç nokta kullanılabilir olmadığında, SDK, tüm kullanılabilir uç noktalardan **rastgele seçer** `secondary` . Uç nokta, sunucu ve hizmet uç noktası arasındaki bağlantı etkin olduğunda **kullanılabilir** olarak işaretlenir.

Bölgeler arası senaryoda, bir istemci `/negotiate` *Doğu ABD*içinde barındırılan uygulama sunucusunu denediğinde, varsayılan olarak her zaman `primary` aynı bölgede bulunan uç noktayı döndürür. Tüm *Doğu ABD* uç noktaları kullanılabilir olmadığında, istemci diğer bölgelerdeki uç noktalara yönlendirilir. Aşağıdaki yük devretme bölümünde senaryo ayrıntılı olarak açıklanmaktadır.

![Normal anlaşma](./media/signalr-howto-scale-multi-instances/normal_negotiate.png)

## <a name="fail-over"></a>Yük devretme

Tüm `primary` uç noktalar kullanılabilir olmadığında, istemci, `/negotiate` kullanılabilir `secondary` uç noktalardan seçer. Bu yük devretme mekanizması her bir uç noktanın `primary` en az bir uygulama sunucusu için uç nokta olarak sunması gerekir.

![Yük devretme](./media/signalr-howto-scale-multi-instances/failover_negotiate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, aynı uygulamada ölçeklendirme, parçalama ve çapraz bölge senaryoları için birden çok örneği nasıl yapılandıracağınızı öğrendiniz.

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryolarında de birden fazla uç nokta desteklenir.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ve yüksek kullanılabilirlik için SignalR hizmetini kurma](./signalr-concept-disaster-recovery.md)
