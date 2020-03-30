---
title: Azure Uygulama Öngörüleri IP adres koleksiyonu | Microsoft Dokümanlar
description: Azure Uygulama Öngörüleri ile IP adreslerinin ve coğrafi konumun nasıl işleneceğini anlama
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 969061ec89ddd0f13caa675bc324207c6c5d8843
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77656526"
---
# <a name="geolocation-and-ip-address-handling"></a>Coğrafi konum ve IP adresi işleme

Bu makalede, varsayılan davranışın nasıl değiştirilen ile birlikte Application Insights'ta coğrafi konum aramave IP adresi işlemenin nasıl oluştuğu açıklanmaktadır.

## <a name="default-behavior"></a>Varsayılan davranış

Varsayılan olarak IP adresleri geçici olarak toplanır, ancak Uygulama Öngörüleri'nde depolanmaz. Temel işlem aşağıdaki gibidir:

IP adresleri, telemetri verilerinin bir parçası olarak Application Insights'a gönderilir. Azure'da alım bitiş noktasına ulaştıktan sonra, IP adresi [MaxMind'tan GeoLite2'yi](https://dev.maxmind.com/geoip/geoip2/geolite2/)kullanarak coğrafi konum araması yapmak için kullanılır. Bu aramanın sonuçları aşağıdaki alanları `client_City`doldurmak için `client_StateOrProvince`kullanılır `client_CountryOrRegion`. Bu noktada, IP adresi atılır `0.0.0.0` ve `client_IP` alana yazılır.

* Tarayıcı telemetrisi: Gönderenin IP adresini geçici olarak toplarız. IP adresi yutma bitiş noktasına göre hesaplanır.
* Sunucu telemetrisi: Application Insights modülü istemci IP adresini geçici olarak toplar. Ayarlanırsa `X-Forwarded-For` toplanmaz.

Bu davranış, gereksiz kişisel verilerin toplanmasını önlemeye yardımcı olmak için tasarım gereğidir. Mümkün olduğunda, kişisel verilerin toplanmasından kaçınmanızı öneririz. 

## <a name="overriding-default-behavior"></a>Varsayılan davranışı geçersiz kılma

Varsayılan davranış kişisel verilerin toplanmasını en aza indirmek olsa da, IP adresi verilerini toplama ve depolama esnekliği sunuyoruz. IP adresleri gibi herhangi bir kişisel veriyi depolamayı seçmeden önce, bunun tabi olabileceğiniz herhangi bir uyumluluk gereksinimini veya yerel düzenlemeleri bozmadığını doğrulamanızı şiddetle öneririz. Application Insights'ta kişisel veri işleme hakkında daha fazla bilgi edinmek [için, kişisel veriler için kılavuza](https://docs.microsoft.com/azure/azure-monitor/platform/personal-data-mgmt)başvurun.

## <a name="storing-ip-address-data"></a>IP adresi verilerinin depolanması

IP toplama ve depolamayı etkinleştirmek için, Application Insights bileşeninin `DisableIpMasking` özelliği . `true` Bu özellik, Azure Kaynak Yöneticisi şablonları aracılığıyla veya REST API'yi arayarak ayarlanabilir. 

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu

```json
{
       "id": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/microsoft.insights/components/<resource-name>",
       "name": "<resource-name>",
       "type": "microsoft.insights/components",
       "location": "westcentralus",
       "tags": {
              
       },
       "kind": "web",
       "properties": {
              "Application_Type": "web",
              "Flow_Type": "Redfield",
              "Request_Source": "IbizaAIExtension",
              // ...
              "DisableIpMasking": true
       }
}
```

### <a name="portal"></a>Portal 

Yalnızca tek bir Application Insights kaynağının davranışını değiştirmeniz gerekiyorsa, bunu gerçekleştirmenin en kolay yolu Azure portalı üzerindendir.  

1. Uygulama Öngörüleri kaynağınızı > **Ayarlar** > **Dışa Aktarma Şablonu'na** gidin 

    ![Dışa Aktarma Şablonu](media/ip-collection/export-template.png)

2. **Dağıt'ı** seçin

    ![Kırmızı ile vurgulanan dağıtma düğmesi](media/ip-collection/deploy.png)

3. **Şablonu Edit'i**seçin. (Şablonunuzun bu örnek şablonda görünmeyen ek özellikleri veya kaynakları varsa, tüm kaynakların şablon dağıtımını artımlı bir değişiklik/güncelleştirme olarak kabul edeceğine emin olmak için dikkatli olun.)

    ![Şablon düzenleme](media/ip-collection/edit-template.png)

4. Kaynağınız için json'da aşağıdaki değişiklikleri yapın ve sonra **Kaydet'i**tıklatın:

    ![Ekran görüntüsü "IbizaAIExtension" sonra virgül ekler ve "DisableIpMasking" ile aşağıdaki yeni bir satır ekleyin: doğru](media/ip-collection/save.png)

    > [!WARNING]
    > Şöyle bir hata yla karşılaşırsanız: ** _Kaynak grubu şablondaki bir veya daha fazla kaynak tarafından desteklenmeyen bir konumdadır. Lütfen farklı bir kaynak grubu seçin._** Geçici olarak açılır dosyadan farklı bir kaynak grubu seçin ve ardından hatayı gidermek için özgün kaynak grubunuzu yeniden seçin.

5. Ben > **Satın** **katılıyorum**seçin . 

    ![Şablon düzenleme](media/ip-collection/purchase.png)

    Bu durumda yeni bir şey satın alınıyor, biz sadece mevcut Application Insights kaynağının config güncelleniyor.

6. Dağıtım tamamlandıktan sonra yeni telemetri verileri kaydedilir.

    Şablonu yeniden seçip yeniden düzenlenecekolursa, yalnızca varsayılan şablonu görürsünüz ve yeni eklenen özelliğinizi ve ilişkili değerini görmezsiniz. IP adresi verilerini görmüyorsanız ve bunun `"DisableIpMasking": true` ayarlandığını onaylamak istiyorsanız. Aşağıdaki PowerShell'i çalıştırın: (Uygun kaynak ve kaynak grubu adı ile değiştirin.) `Fabrikam-dev`
    
    ```powershell
    # If you aren't using the cloud shell you will need to connect to your Azure account
    # Connect-AzAccount 
    $AppInsights = Get-AzResource -Name 'Fabrikam-dev' -ResourceType 'microsoft.insights/components' -ResourceGroupName 'Fabrikam-dev'
    $AppInsights.Properties
    ```
    
    Sonuç olarak özelliklerin listesi döndürülür. Özelliklerden biri okumalısınız. `DisableIpMasking: true` Yeni özelliği Azure Kaynak Yöneticisi ile dağıtmadan önce PowerShell'i çalıştırıyorsanız, özellik yok olmaz.

### <a name="rest-api"></a>Dinlenme API'si

Aynı değişiklikleri yapmak için [Geri Kalan API](https://docs.microsoft.com/rest/api/azure/) yükü aşağıdaki gibidir:

```
PATCH https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/microsoft.insights/components/<resource-name>?api-version=2018-05-01-preview HTTP/1.1
Host: management.azure.com
Authorization: AUTH_TOKEN
Content-Type: application/json
Content-Length: 54

{
       "location": "<resource location>",
       "kind": "web",
       "properties": {
              "Application_Type": "web",
              "DisableIpMasking": true
       }
}
```

## <a name="telemetry-initializer"></a>Telemetri başlandırıcı

IP adreslerinin tamamını veya `DisableIpMasking` bir kısmını kaydetmekten daha esnek bir alternatife ihtiyacınız varsa, IP'nin tamamını veya ip'i özel bir alana kopyalamak için [bir telemetri baş harferini](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#addmodify-properties-itelemetryinitializer) kullanabilirsiniz. 

### <a name="aspnet--aspnet-core"></a>ASP.NET / ASP.NET Çekirdek

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;

namespace MyWebApp
{
    public class CloneIPAddress : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            ISupportProperties propTelemetry = telemetry as ISupportProperties;

            if (propTelemetry !=null && !propTelemetry.Properties.ContainsKey("client-ip"))
            {
                string clientIPValue = telemetry.Context.Location.Ip;
                propTelemetry.Properties.Add("client-ip", clientIPValue);
            }
        }
    } 
}
```

> [!NOTE]
> Uygulama Öngörüleri SDK'nın en son kararlı sürümüne erişemiyorsanız, `ISupportProperties`kontrol edin ve çalıştırdığınızdan emin olun. `ISupportProperties`yüksek kardinallik değerleri için tasarlanmıştır, oysa `GlobalProperties` bölge adı, çevre adı, vb. gibi düşük kardinallik değerleri için daha uygundur. 

### <a name="enable-telemetry-initializer-for-aspnet"></a>ASP.NET için telemetri başlandırıcısını etkinleştirin

```csharp
using Microsoft.ApplicationInsights.Extensibility;


namespace MyWebApp
{
    public class MvcApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
              //Enable your telemetry initializer:
              TelemetryConfiguration.Active.TelemetryInitializers.Add(new CloneIPAddress());
        }
    }
}

```

### <a name="enable-telemetry-initializer-for-aspnet-core"></a>ASP.NET Core için telemetri başlandırıcısını etkinleştirin

Telemetri başharfünüzü ASP.NET ile aynı şekilde ASP.NET Core olarak oluşturabilirsiniz, ancak başharfi etkinleştirmek için aşağıdaki başvuru örneğini kullanabilirsiniz:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, CloneIPAddress>();
}
```

### <a name="nodejs"></a>Node.js

```javascript
appInsights.defaultClient.addTelemetryProcessor((envelope) => {
    const baseData = envelope.data.baseData;
    if (appInsights.Contracts.domainSupportsProperties(baseData)) {
        const ipAddress = envelope.tags[appInsights.defaultClient.context.keys.locationIp];
        if (ipAddress) {
            baseData.properties["client-ip"] = ipAddress;
        }
    }
});
```

### <a name="client-side-javascript"></a>İstemci tarafı JavaScript

Sunucu tarafındaki SDK'ların aksine, istemci tarafındaki Javascript SDK IP adresini hesaplamaz. İstemci tarafı telemetrisi için varsayılan IP adresi hesaplaması, telemetri nin gelişisırasında Azure'daki yutma bitiş noktasında gerçekleştirilir. Bu, istemci tarafındaki verileri bir proxy'ye gönderip daha sonra son noktaya yönlendiriyorsanız, IP adresi hesaplamasının istemcinin değil, proxy'nin IP adresini gösterebileceği anlamına gelir. Proxy kullanılmazsa, bu bir sorun olmamalıdır.

IP adresini doğrudan istemci tarafında hesaplamak istiyorsanız, bu hesaplamayı gerçekleştirmek için kendi özel mantığınızı eklemeniz ve `ai.location.ip` etiketi ayarlamak için sonucu kullanmanız gerekir. Ayarlandığında, `ai.location.ip` IP adresi hesaplaması yutma bitiş noktasına göre yapılmaz ve sağlanan IP adresi onurlandırılır ve coğrafi arama nın gerçekleştirilmesi için kullanılır. Bu senaryoda, IP adresi varsayılan olarak yine de sıfırlanır. 

Özel mantığınızdan hesaplanan tüm IP adresini korumak `ai.location.ip` için, sağladığınız IP adresi verilerini ayrı bir özel alana kopyalayan bir telemetri başlatmalaştırıcısı kullanabilirsiniz. Ama yine sunucu tarafı SDKs aksine, 3 parti kütüphaneleri veya kendi özel istemci tarafı IP toplama mantığı güvenmeden istemci tarafı SDK sizin için IP hesaplamak olmaz.    


```javascript
appInsights.addTelemetryInitializer((item) => {
    const ipAddress = item.tags && item.tags["ai.location.ip"];
    if (ipAddress) {
        item.baseData.properties = {
            ...item.baseData.properties,
            "client-ip": ipAddress
        };
    }
});

```  

### <a name="view-the-results-of-your-telemetry-initializer"></a>Telemetri başharfünüzün sonuçlarını görüntüleyin

Daha sonra sitenize karşı yeni bir trafik tetikler seniz ve yutulması için zaman olduğundan emin olmak için yaklaşık 2-5 dakika beklerseniz, IP adresi toplama nın çalışıp çalışmadığını görmek için bir Kusto sorgusu çalıştırabilirsiniz:

```kusto
requests
| where timestamp > ago(1h) 
| project appName, operation_Name, url, resultCode, client_IP, customDimensions.["client-ip"]
```

Yeni toplanan IP adresleri `customDimensions_client-ip` sütunda görünmelidir. Varsayılan `client-ip` sütun, bileşen düzeyinde IP adresi koleksiyonunu nasıl yapılandırdığınıza bağlı olarak, yalnızca ilk üç sekizlinin sıfırlanmış veya yalnızca ilk üç sekizlisini gösteren 4 sekizlinin tümüne sahip olacaktır. Telemetri başharfini uyguladıktan sonra yerel olarak test ediyorsanız `customDimensions_client-ip` `::1` ve gördüğünüz değer bu beklenen davranıştır. `::1`IPv6'daki geri dönüş adresini temsil eder. Bu IPv4 eşdeğerdir `127.0.01` ve localhost test ederken göreceksiniz sonucudur.

## <a name="next-steps"></a>Sonraki Adımlar

* Application Insights'ta [kişisel veri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/personal-data-mgmt) hakkında daha fazla bilgi edinin.

* Application Insights'ta [IP adresi koleksiyonunun](https://apmtips.com/blog/2016/07/05/client-ip-address/) nasıl çalıştığı hakkında daha fazla bilgi edinin. (Bu bizim mühendislerden biri tarafından yazılmış eski bir dış blog yazısı. IP adresinin kaydedildiği `0.0.0.0`geçerli varsayılan davranıştan önce , ancak yerleşik in mekaniğinde daha `ClientIpHeaderTelemetryInitializer`fazla derinliğe gider .)
