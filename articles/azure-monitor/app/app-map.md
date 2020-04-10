---
title: Azure Uygulama Öngörülerinde Uygulama Haritası | Microsoft Dokümanlar
description: Uygulama haritası ile karmaşık uygulama topolojilerini izleme
ms.topic: conceptual
ms.date: 03/15/2019
ms.reviewer: sdash
ms.openlocfilehash: 7c5c9173704535b1e34ffde5867bd512e3e02ed8
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80989536"
---
# <a name="application-map-triage-distributed-applications"></a>Uygulama Haritası: Triyaj Dağıtılmış Uygulamalar

Uygulama Haritası, dağıtılmış uygulamanızın tüm bileşenlerinde performans darboğazlarını veya arıza etkin noktalarını tespit edilemenize yardımcı olur. Haritadaki her düğüm bir uygulama bileşenini veya bağımlılıklarını temsil eder; ve sağlık KPI ve uyarıları durumu vardır. Uygulama Öngörüleri olayları gibi daha ayrıntılı tanılama için herhangi bir bileşenden tıklatabilirsiniz. Uygulamanız Azure hizmetlerini kullanıyorsa, SQL Veritabanı Danışmanı önerileri gibi Azure tanılama larını da tıklatabilirsiniz.

## <a name="what-is-a-component"></a>Bileşen Nedir?

Bileşenler, dağıtılmış/mikro hizmetler uygulamanızın bağımsız olarak dağıtılabilir parçalarıdır. Geliştiriciler ve operasyon ekipleri kod düzeyinde görünürlüğe veya bu uygulama bileşenleri tarafından oluşturulan telemetriye erişime sahiptir. 

* Bileşenler, takımınızın/kuruluşunuzun erişemeyebilecekleri SQL, EventHub gibi "gözlenen" dış bağımlılıklardan farklıdır (kod veya telemetri).
* Bileşenler herhangi bir sayıda sunucu/rol/kapsayıcı örneklerinde çalışır.
* Bileşenler ayrı Application Insights enstrümantasyon anahtarları (abonelikler farklı olsa bile) veya farklı roller tek bir Application Insights enstrümantasyon anahtarına raporlama olabilir. Önizleme haritası deneyimi, bileşenlerin nasıl ayarlandıklarına bakılmaksızın gösterilir.

## <a name="composite-application-map"></a>Kompozit Uygulama Haritası

İlgili uygulama bileşenlerinin birden çok düzeyinde tam uygulama topolojisi görebilirsiniz. Bileşenler farklı Application Insights kaynakları veya tek bir kaynaktaki farklı roller olabilir. Uygulama haritası, Uygulama Öngörüleri SDK yüklü sunucular arasında yapılan HTTP bağımlılık çağrılarını izleyerek bileşenleri bulur. 

Bu deneyim bileşenlerin aşamalı keşfi ile başlar. Uygulama eşlesini ilk yüklediğinizde, bu bileşenle ilgili bileşenleri bulmak için bir sorgu kümesi tetiklenir. Sol üst köşedeki bir düğme, keşfedildikleri gibi uygulamanızdaki bileşenlerin sayısıyla güncellenir. 

"Harita bileşenlerini güncelleştir" seçeneğini tıklattığınızda, harita bu noktaya kadar keşfedilen tüm bileşenlerle yenilenir. Uygulamanızın karmaşıklığına bağlı olarak, bu yükün yüklenmesi bir dakika sürebilir.

Bileşenlerin tümü tek bir Application Insights kaynağındaki rollerse, bu bulma adımı gerekli değildir. Böyle bir uygulama için ilk yük tüm bileşenlerine sahip olacaktır.

![Uygulama Haritası Ekran Görüntüsü](media/app-map/app-map-001.png)

Bu deneyimin temel hedeflerinden biri, karmaşık topolojileri yüzlerce bileşenle görselleştirebilmektir.

İlgili öngörüleri görmek için herhangi bir bileşene tıklayın ve bu bileşenin performans ve hata triajı deneyimine gidin.

![Geçici Açılır Öğe](media/app-map/application-map-002.png)

### <a name="investigate-failures"></a>Hataları araştırma

Hataları **bölmesini** başlatmak için hataları araştır'ı seçin.

![Araştırma hataları düğmesinin ekran görüntüsü](media/app-map/investigate-failures.png)

![Hata deneyiminin ekran görüntüsü](media/app-map/failures.png)

### <a name="investigate-performance"></a>Performansı araştırma

Performans sorunlarını gidermek **için, araştırma performansını**seçin.

![Performansı araştırma düğmesinin ekran görüntüsü](media/app-map/investigate-performance.png)

![Performans deneyiminin ekran görüntüsü](media/app-map/performance.png)

### <a name="go-to-details"></a>Ayrıntılara git

Arama yığını düzeyine kadar görünümler sunabilecek uçdan uca işlem deneyimini keşfetmek için **ayrıntılara git'i** seçin.

![Ayrıntılara git düğmesinin ekran görüntüsü](media/app-map/go-to-details.png)

![Uçtan uca işlem ayrıntılarının ekran görüntüsü](media/app-map/end-to-end-transaction.png)

### <a name="view-logs-analytics"></a>Günlükleri Görüntüle (Analytics)

Uygulama verilerinizi daha fazla sorgulamak ve araştırmak için **Günlükler'de (Analytics) görünüm'e**tıklayın.

![Analitik düğmesinde görüntü ekran görüntüsü](media/app-map/view-logs.png)

![Analitik deneyiminin ekran görüntüsü. Satır grafiği, bir isteğin son 12 saat içinde ortalama yanıt süresini özetler.](media/app-map/log-analytics.png)

### <a name="alerts"></a>Uyarılar

Etkin uyarıları ve uyarıların tetiklenemesine neden olan temel kuralları görüntülemek için **uyarıları**seçin.

![Uyarılar düğmesinin ekran görüntüsü](media/app-map/alerts.png)

![Analitik deneyiminin ekran görüntüsü](media/app-map/alerts-view.png)

## <a name="set-cloud-role-name"></a>Bulut rol adını ayarlama

Uygulama Haritası, haritadaki bileşenleri tanımlamak için **bulut rol adı** özelliğini kullanır. Application Insights SDK, bileşenler tarafından yayılan telemetriye bulut rol adı özelliğini otomatik olarak ekler. Örneğin, SDK bulut rol adı özelliğine bir web sitesi adı veya hizmet rol adı ekler. Ancak, varsayılan değeri geçersiz kılmak isteyebileceğiniz durumlar vardır. Bulut rol adını geçersiz kılmak ve Uygulama Haritası'nda görüntülenenleri değiştirmek için:

# <a name="netnetcore"></a>[.NET/.NetCore](#tab/net)

**Aşağıdaki gibi özel TelemetryInitializer yazın.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "Custom RoleName";
                telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```

**ASP.NET uygulamaları: Etkin TelemetriYapılandırmasına baş harflerini yükleyin**

In ApplicationInsights.config :

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

web uygulamalarını ASP.NET için alternatif bir yöntem, koddaki baş harflerini anlık olarak Global.aspx.cs aşağıdakiler gibi:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

> [!NOTE]
> Kullanarak veya kullanarak `ApplicationInsights.config` `TelemetryConfiguration.Active` baş harf ekleme ASP.NET Core uygulamaları için geçerli değildir. 

**ASP.NET Core uygulamaları: TelemetriConfiguration'a baş harflerini yükleyin**

[ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) uygulamaları için, `TelemetryInitializer` aşağıda gösterildiği gibi Bağımlılık Enjeksiyonu kabına ekleyerek yeni bir ekleme yapılır. Bu sizin `Startup.cs` `ConfigureServices` sınıf yöntemi yapılır.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

# <a name="java"></a>[Java](#tab/java)

**Java aracısı**

[Java aracısı 3.0](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) için bulut rol adı aşağıdaki gibi ayarlanır:

```json
{
  "instrumentationSettings": {
    "preview": {
      "roleName": "my cloud role name"
    }
  }
}
```

Ayrıca, ortam değişkenini ```APPLICATIONINSIGHTS_ROLE_NAME```kullanarak bulut rol adını da ayarlayabilirsiniz.

**Java SDK**

SDK kullanıyorsanız, Application Insights Java SDK 2.5.0 ile başlayarak, dosyanıza `<RoleName>` `ApplicationInsights.xml` ekleyerek bulut rol adını belirtebilirsiniz, örneğin.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
   <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
   <RoleName>** Your role name **</RoleName>
   ...
</ApplicationInsights>
```

Uygulama Öngörüleri İlkbahar Önyükleme başlatıcısı ile Bahar Önyükleme kullanıyorsanız, gerekli olan tek değişiklik application.properties dosyasındaki uygulama için özel adınızı ayarlamaktır.

`spring.application.name=<name-of-app>`

İlkbahar Önyükleme başlatıcısı, spring.application.name özelliği için girdiğiniz değere otomatik olarak bulut rol adı atar.

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Düğüm.js için alternatif yöntem

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
appInsights.queue.push(() => {
appInsights.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```
---

### <a name="understanding-cloud-role-name-within-the-context-of-the-application-map"></a>Uygulama Haritası bağlamında bulut rol adının anlaşılması

**Bulut rol adı**hakkında nasıl düşüneceğiniz konusunda, birden çok bulut rol adı bulunan bir Uygulama Haritasına bakmak yararlı olabilir:

![Uygulama Haritası Ekran Görüntüsü](media/app-map/cloud-rolename.png)

Yeşil kutulardaki adların her biri yukarıdaki Uygulama Haritası'nda, bu özel dağıtılmış uygulamanın farklı yönleri için bulut rol adı değerleri yer alır. Yani bu uygulama için rolleri `Authentication` `acmefrontend`oluşur: , , `Inventory Management`, a `Payment Processing Worker Role`. 

Bu uygulama söz konusu olduğunda, bu bulut rol adlarının her biri, kendi enstrümantasyon anahtarlarıyla farklı bir benzersiz Application Insights kaynağını da temsil eder. Bu uygulamanın sahibi bu dört farklı Application Insights kaynaklarının her birine erişebildiği için, Uygulama Haritası temel ilişkilerin bir haritasını bir araya getirebiliyor.

Resmi [tanımlar](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/39a5ef23d834777eefdd72149de705a016eb06b0/Schema/PublicSchema/ContextTagKeys.bond#L93)için:

```
   [Description("Name of the role the application is a part of. Maps directly to the role name in azure.")]
    [MaxStringLength("256")]
    705: string      CloudRole = "ai.cloud.role";
    
    [Description("Name of the instance where the application is running. Computer name for on-premises, instance name for Azure.")]
    [MaxStringLength("256")]
    715: string      CloudRoleInstance = "ai.cloud.roleInstance";
```

Alternatif olarak, **bulut rolü örneği,** **bulut rol adının** sorunun web ön uçunuzda bir yerde olduğunu söylediği senaryolar için yararlı olabilir, ancak web ön uçunuzu birden çok yük dengeli sunucuda çalıştırıyor olabilirsiniz, böylece Kusto sorguları aracılığıyla daha derin bir katmanda sondaj yapabilmek ve sorunun tüm web ön uç sunucularını/örneklerini mi etkilediğini bilmek son derece önemli olabilir.

Bulut rolü örneği için değeri geçersiz kılmak isteyebileceğiniz bir senaryo, uygulamanız yalnızca tek bir sunucunun belirli bir sorunu bulmak için yeterli bilgi olmadığı kapsayıcı bir ortamda çalışıyorsa olabilir.

Telemetri başharfleriyle bulut rol adı özelliğini nasıl geçersiz kılabilir hakkında daha fazla bilgi için [bkz: ITelemetryInitializer.](api-filtering-sampling.md#addmodify-properties-itelemetryinitializer)

## <a name="troubleshooting"></a>Sorun giderme

Uygulama Haritası'nın beklendiği gibi çalışmasını sağlamakta sorun yaşıyorsanız, şu adımları deneyin:

### <a name="general"></a>Genel

1. Resmi olarak desteklenen bir SDK kullandığınızdan emin olun. Desteklenmeyen/Topluluk SDK’ları bağıntıyı desteklemeyebilir.

    Desteklenen SDK'ların listesi için bu [makaleye](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) bakın.

2. Tüm bileşenleri en son SDK sürümüne yükseltin.

3. Azure Fonksiyonlarını C# ile kullanıyorsanız, [V2 Işlevleri'ne](https://docs.microsoft.com/azure/azure-functions/functions-versions)yükseltin.

4. [Bulut rol adının](#set-cloud-role-name) doğru şekilde yapılandırıldığıdoğrulayın.

5. Bir bağımlılık eksikse, bunun [otomatik olarak toplanan bağımlılıklar](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies) listesinde bulunduğundan emin olun. Listede yoksa, bunu yine de [izleme bağımlılık çağrısı](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency) kullanarak el ile izleyebilirsiniz.

### <a name="too-many-nodes-on-the-map"></a>Haritada çok fazla düğüm

Uygulama Haritası, istek telemetrinizde bulunan her benzersiz bulut rol adı için bir uygulama düğümü ve bağımlılık telemetrinizde tür, hedef ve bulut rol adının her benzersiz birleşimi için bir bağımlılık düğümü oluşturur. Telemetrinizde 10.000'den fazla düğüm varsa, Uygulama Haritası tüm düğümleri ve bağlantıları getiremez, bu nedenle haritanız eksik kalır. Bu durumda, haritayı görüntülerken bir uyarı iletisi görüntülenir.

Buna ek olarak, Uygulama Haritası yalnızca aynı anda işlenen en fazla 1000 ayrı gruplandırılmamış düğümü destekler. Uygulama Eşlemi, aynı türve arayanlara sahip bağımlılıkları gruplayarak görsel karmaşıklığı azaltır, ancak telemetrinizde çok fazla benzersiz bulut rol adı veya çok fazla bağımlılık türü varsa, bu gruplandırma yetersiz kalır ve harita işleyemez.

Bunu düzeltmek için, bulut rol adını, bağımlılık türünü ve bağımlılık hedef alanlarını düzgün bir şekilde ayarlamak için enstrümantasyonunuzu değiştirmeniz gerekir.

* Bağımlılık hedefi, bir bağımlılığın mantıksal adını temsil etmelidir. Çoğu durumda, bu bağımlılık sunucu veya kaynak adı eşdeğerdir. Örneğin, HTTP bağımlılıkları söz konusu olduğunda, ana bilgisayar adına ayarlanır. Bir istekten diğerine değişen benzersiz t'ler veya parametreler içermemelidir.

* Bağımlılık türü, mantıksal bağımlılık türünü temsil etmelidir. Örneğin, HTTP, SQL veya Azure Blob tipik bağımlılık türleridir. Benzersiz tbm'ler içermemelidir.

* Bulut rol adının amacı yukarıdaki [bölümde](https://docs.microsoft.com/azure/azure-monitor/app/app-map#set-cloud-role-name)açıklanmıştır.

## <a name="portal-feedback"></a>Portal geri bildirimi

Geri bildirim sağlamak için geri bildirim seçeneğini kullanın.

![MapLink-1 görüntüsü](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama Öngörüleri'nde korelasyonun nasıl çalıştığı hakkında daha fazla bilgi edinmek için [telemetri korelasyon makalesine](correlation.md)başvurun.
* [Uçtan uca işlem tanılama deneyimi,](transaction-diagnostics.md) tüm Application Insights tarafından izlenen bileşenleritek bir görünümde sunucu tarafı telemetrisini ilişkilendirer.
* ASP.NET Core ve ASP.NET'daki gelişmiş korelasyon senaryoları [için parça özel işlemler](custom-operations-tracking.md) makalesine başvurun.
