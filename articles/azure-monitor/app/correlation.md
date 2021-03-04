---
title: Azure Application Insights telemetri bağıntısı | Microsoft Docs
description: Application Insights telemetri bağıntısı
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.custom: devx-track-python, devx-track-csharp
ms.openlocfilehash: beaeb0131a2c9b326d663f6fcbb8273a9b52b412
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102100976"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application Insights telemetri bağıntısı

Mikro hizmetler dünyasında, her mantıksal işlem, hizmetin çeşitli bileşenlerinde iş yapılmasını gerektirir. [Application Insights](../../azure-monitor/app/app-insights-overview.md)kullanarak, bu bileşenlerin her birini ayrı ayrı izleyebilirsiniz. Application Insights, hangi bileşenin hatalardan veya performans düşüşüne karşı sorumlu olduğunu saptamak için kullandığınız dağıtılmış telemetri bağıntısını destekler.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin ilişkilendirilmesi için Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma tekniklerini ve protokollerini içerir. Ayrıca, farklı diller ve platformlarda bağıntı taclerin uygulanmasını da ele alır.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri bağıntısı için veri modeli

Application Insights, dağıtılmış telemetri bağıntısı için bir [veri modeli](../../azure-monitor/app/data-model.md) tanımlar. Telemetrinin bir mantıksal işlemle ilişkilendirilmesi için, her telemetri öğesinin adında bir bağlam alanı vardır `operation_Id` . Bu tanımlayıcı, dağıtılmış izlemede her telemetri öğesi tarafından paylaşılır. Bu nedenle, tek bir katmanda telemetri kaybetseniz bile, diğer bileşenler tarafından raporlanan Telemetriyi yine de ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlem, genellikle bileşenlerinden biri tarafından işlenen istekler olan daha küçük bir işlem kümesinden oluşur. Bu işlemler [istek telemetrisi](../../azure-monitor/app/data-model-request-telemetry.md)tarafından tanımlanır. Her istek telemetri öğesi `id` kendisini benzersiz ve küresel olarak tanımlayan kendi kendine sahiptir. Ve istekle ilişkili tüm telemetri öğeleri (izlemeler ve özel durumlar gibi) `operation_parentId` isteğin değerine ayarlanmalıdır `id` .

Başka bir bileşene yönelik HTTP çağrısı gibi her giden işlem, [bağımlılık telemetrisi](../../azure-monitor/app/data-model-dependency-telemetry.md)tarafından temsil edilir. Bağımlılık telemetrisi, `id` genel olarak benzersiz olan kendisini de tanımlar. Bu bağımlılık çağrısıyla başlatılan istek telemetrisi, bunu `id` olarak kullanır `operation_parentId` .

`operation_Id`, Ve ile kullanarak dağıtılmış mantıksal işlemin bir görünümünü oluşturabilirsiniz `operation_parentId` `request.id` `dependency.id` . Bu alanlar telemetri çağrılarının önem derecesine göre de tanımlar.

Mikro hizmetler ortamında, bileşenlerden izlemeler farklı depolama öğelerine gidebilir. Her bileşen Application Insights kendi izleme anahtarına sahip olabilir. Mantıksal işlem için telemetri almak için, Application Insights her depolama öğesinden verileri sorgular. Depolama öğelerinin sayısı büyükse, ileri bakabileceğiniz bir ipucu gerekir. Application Insights veri modeli, bu sorunu çözmek için iki alanı tanımlar: `request.source` ve `dependency.target` . İlk alan, bağımlılık isteğini Başlatan bileşeni tanımlar. İkinci alan, bağımlılık çağrısının yanıtını döndüren bileşeni tanımlar.

## <a name="example"></a>Örnek

Bir örneğe göz atalım. Hisse senedi fiyatları adlı bir uygulama, hisse senedi adlı bir dış API kullanarak bir stokun geçerli pazar fiyatını gösterir. Hisse senedi fiyatları uygulaması, kullanarak istemci Web tarayıcısının açtığı hisse senedi sayfası adlı bir sayfa içerir `GET /Home/Stock` . Uygulama, HTTP çağrısını kullanarak hisse senedi API 'sini sorgular `GET /api/stock/value` .

Bir sorgu çalıştırarak elde edilen telemetrisini çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda, tüm telemetri öğelerinin kökü paylaştığından emin olmanız gerektiğini unutmayın `operation_Id` . Sayfadan bir AJAX çağrısı yapıldığında, bağımlılık telemetrisine yeni bir benzersiz KIMLIK ( `qJSXU` ) atanır ve pageView kimliği olarak kullanılır `operation_ParentId` . Sunucu isteği daha sonra Ajax KIMLIĞINI olarak kullanır `operation_ParentId` .

| ItemType   | name                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görüntülemesi   | Hisse senedi sayfası                | Stilz         |                    | Stilz         |
| bağımlılık | /Home/Stock al           | qJSXU        | Stilz               | Stilz         |
| istek    | Giriş/stok al            | KqKwlrSt9PA = | qJSXU              | Stilz         |
| bağımlılık | /Api/Stock/Value al      | bBrf2L7mm2g = | KqKwlrSt9PA =       | Stilz         |

`GET /api/stock/value`Bir dış hizmete çağrı yapıldığında, alanı uygun şekilde ayarlayabilmeniz için bu sunucunun kimliğini bilmeniz gerekir `dependency.target` . Dış hizmet izlemeyi desteklemiyorsa, `target` hizmetin ana bilgisayar adına ayarlanır (örneğin, `stock-prices-api.com` ). Ancak hizmet, önceden tanımlı bir HTTP üst bilgisi döndürerek kendisini tanımlarsa, `target` Application Insights bu hizmetten telemetri 'i sorgulayarak dağıtılmış bir izleme oluşturmasına izin veren hizmet kimliğini içerir.

## <a name="correlation-headers-using-w3c-tracecontext"></a>W3C TraceContext kullanan bağıntı üstbilgileri

Application Insights, şunu tanımlayan [W3C Trace-Context](https://w3c.github.io/trace-context/)öğesine geçiyor:

- `traceparent`: Genel benzersiz işlem KIMLIĞI ve çağrının benzersiz tanımlayıcısını taşır.
- `tracestate`: Sisteme özgü izleme bağlamını taşır.

Application Insights SDK 'sının en son sürümü Trace-Context protokolünü destekler, ancak bunu kabul etmeniz gerekebilir. (Application Insights SDK tarafından desteklenen önceki Bağıntı protokolü ile geriye dönük uyumluluk korunur.)

[Istek kimliği olarak da bilinen BAĞıNTı http Protokolü](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)kullanım dışı bırakılıyor. Bu protokol iki üstbilgiyi tanımlar:

- `Request-Id`: Çağrının genel benzersiz KIMLIĞINI taşır.
- `Correlation-Context`: Dağıtılmış izleme özelliklerinin ad-değer çiftleri koleksiyonunu taşır.

Application Insights ayrıca bağıntı HTTP protokolünün [uzantısını](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) tanımlar. `Request-Context`Anında çağıran veya çağrılan tarafından kullanılan özellik koleksiyonunu yaymak için ad-değer çiftlerini kullanır. Application Insights SDK bu üstbilgiyi ve alanlarını ayarlamak için kullanır `dependency.target` `request.source` .

[W3C Trace-Context](https://w3c.github.io/trace-context/) ve Application Insights veri modelleri aşağıdaki şekilde eşlenir:

| Application Insights                   | W3C TraceContext                                      |
|------------------------------------    |-------------------------------------------------|
| `Id``Request`ve`Dependency`     | [üst öğe kimliği](https://w3c.github.io/trace-context/#parent-id)                                     |
| `Operation_Id`                         | [İzleme kimliği](https://w3c.github.io/trace-context/#trace-id)                                           |
| `Operation_ParentId`                   | Bu yayılımın üst yayılma alanının [üst kimliği](https://w3c.github.io/trace-context/#parent-id) . Bu bir kök yayılım ise, bu alan boş olmalıdır.     |

Daha fazla bilgi için bkz. [telemetri veri modeli Application Insights](../../azure-monitor/app/data-model.md).

### <a name="enable-w3c-distributed-tracing-support-for-net-apps"></a>.NET uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

W3C TraceContext tabanlı dağıtılmış izleme, eski Request-Id protokolle geriye dönük uyumluluk ile birlikte, tüm son .NET Framework/. NET Core SDK 'lerinde varsayılan olarak etkindir.

### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

#### <a name="java-30-agent"></a>Java 3,0 Aracısı

  Java 3,0 Aracısı, W3C 'ı kutudan çıkar ve ek yapılandırma gerekmez. 

#### <a name="java-sdk"></a>Java SDK
- **Gelen yapılandırma**

  - Java EE uygulamaları için aşağıdaki `<TelemetryModules>` etikete ApplicationInsights.xml ekleyin:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```

  - Spring Boot uygulamaları için şu özellikleri ekleyin:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Giden yapılandırma**

  Aşağıdakileri AI-Agent.xml ekleyin:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Geriye dönük uyumluluk modu varsayılan olarak etkindir ve `enableW3CBackCompat` parametresi isteğe bağlıdır. Bunu yalnızca geriye dönük uyumluluğu kapatmak istediğinizde kullanın.
  >
  > İdeal olarak, tüm hizmetleriniz W3C protokolünü destekleyen SDK 'ların daha yeni sürümlerine güncelleştirildiği zaman bunu devre dışı bırakabilirsiniz. Bu yeni SDK 'lara mümkün olan en kısa sürede geçiş yapmanızı önemle tavsiye ederiz.

> [!IMPORTANT]
> Gelen ve giden yapılandırmaların tam olarak aynı olduğundan emin olun.

### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Web Apps için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik ' de bulunur `Microsoft.ApplicationInsights.JavaScript` . Varsayılan olarak devre dışıdır. Etkinleştirmek için, config kullanın `distributedTracingMode` . AI_AND_W3C, Application Insights tarafından işaretlenmiş eski hizmetlerle geriye dönük uyumluluk için sağlanır.

- **[NPM tabanlı kurulum](./javascript.md#npm-based-setup)**

Aşağıdaki yapılandırmayı ekleyin:
  ```JavaScript
    distributedTracingMode: DistributedTracingModes.W3C
  ```

- **[Kod parçacığı tabanlı kurulum](./javascript.md#snippet-based-setup)**

Aşağıdaki yapılandırmayı ekleyin:
  ```
      distributedTracingMode: 2 // DistributedTracingModes.W3C
  ```
> [!IMPORTANT] 
> Bağıntıyı etkinleştirmek için gereken tüm konfigürasyonları görmek için [JavaScript bağıntı belgelerine](./javascript.md#enable-correlation)bakın.

## <a name="telemetry-correlation-in-opencensus-python"></a>OpenCensus Python 'da telemetri bağıntısı

OpenCensus Python, ek yapılandırma gerektirmeden [W3C Trace-Context](https://w3c.github.io/trace-context/) ' i destekler.

Başvuru olarak, OpenCensus veri modeli [burada](https://github.com/census-instrumentation/opencensus-specs/tree/master/trace)bulunabilir.

### <a name="incoming-request-correlation"></a>Gelen istek bağıntısı

OpenCensus Python 'da W3C Trace-Context gelen isteklerden gelen üst bilgileri, isteklerden oluşturulan yayılmaya ilişkilendirir. OpenCensus, bu popüler web uygulaması çerçeveleri için tümleştirmelerle otomatik olarak yapılır: Flask, Docgo ve piramit. Yalnızca W3C Trace-Context üst bilgilerini [doğru biçimde](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format) doldurmanız ve istekle birlikte göndermeniz gerekir. Bunu gösteren örnek bir Flask uygulaması aşağıda verilmiştir:

```python
from flask import Flask
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.ext.flask.flask_middleware import FlaskMiddleware
from opencensus.trace.samplers import ProbabilitySampler

app = Flask(__name__)
middleware = FlaskMiddleware(
    app,
    exporter=AzureExporter(),
    sampler=ProbabilitySampler(rate=1.0),
)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='localhost', port=8080, threaded=True)
```

Bu kod, yerel makinenizde bir örnek Flask uygulaması çalıştırır ve bağlantı noktasını dinler `8080` . İzleme bağlamını ilişkilendirmek için uç noktaya bir istek gönderirsiniz. Bu örnekte, bir `curl` komut kullanabilirsiniz:
```
curl --header "traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01" localhost:8080
```
[Trace-Context üst bilgi biçimine](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format)bakarak aşağıdaki bilgileri türetebilirsiniz:

`version`: `00`

`trace-id`: `4bf92f3577b34da6a3ce929d0e0e4736`

`parent-id/span-id`: `00f067aa0ba902b7`

`trace-flags`: `01`

Azure Izleyici 'ye gönderilen istek girişine bakarsanız, izleme üst bilgisi bilgileriyle doldurulmuş alanları görebilirsiniz. Bu verileri Azure Izleyici Application Insights kaynağında Günlükler (Analiz) altında bulabilirsiniz.

![Günlüklerde telemetri isteme (Analiz)](./media/opencensus-python/0011-correlation.png)

`id`Alanı, `<trace-id>.<span-id>` `trace-id` istek içinde geçirilen izleme başlığından alındığı ve `span-id` Bu yayılma için oluşturulan 8 baytlık bir dizi olan biçimindedir.

`operation_ParentId`Alanı, `<trace-id>.<parent-id>` hem hem de ' ın `trace-id` `parent-id` istekte geçirilen izleme başlığından alındığı biçimdedir.

### <a name="log-correlation"></a>Günlük bağıntısı

OpenCensus Python, kayıtları günlüğe kaydetmek için bir izleme KIMLIĞI, bir span ID ve örnekleme bayrağı ekleyerek günlüklerinizi ilişkilendirmenize olanak sağlar. Bu öznitelikleri, OpenCensus [günlük tümleştirmesi](https://pypi.org/project/opencensus-ext-logging/)' ni yükleyerek eklersiniz. Aşağıdaki öznitelikler Python `LogRecord` nesnelerine eklenecektir: `traceId` , `spanId` , ve `traceSampled` . Bu, yalnızca tümleştirmeden sonra oluşturulan Günlükçüler için geçerli olduğunu unutmayın.

Bunu gösteren örnek bir uygulama aşağıda verilmiştir:

```python
import logging

from opencensus.trace import config_integration
from opencensus.trace.samplers import AlwaysOnSampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['logging'])
logging.basicConfig(format='%(asctime)s traceId=%(traceId)s spanId=%(spanId)s %(message)s')
tracer = Tracer(sampler=AlwaysOnSampler())

logger = logging.getLogger(__name__)
logger.warning('Before the span')
with tracer.span(name='hello'):
    logger.warning('In the span')
logger.warning('After the span')
```
Bu kod çalıştırıldığında, konsolunda aşağıdaki şekilde yazdırılır:
```
2019-10-17 11:25:59,382 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 Before the span
2019-10-17 11:25:59,384 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=70da28f5a4831014 In the span
2019-10-17 11:25:59,385 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 After the span
```
`spanId`Yayılma alanındaki günlük iletisi için bir mevcut olduğuna dikkat edin. Bu, `spanId` adlı yayılmasına ait olan aynıdır `hello` .

Kullanarak günlük verilerini dışa aktarabilirsiniz `AzureLogHandler` . Daha fazla bilgi için [bu makaleye](./opencensus-python.md#logs) bakın.

Ayrıca, uygun bağıntı için izleme bilgilerini bir bileşenden diğerine geçirebiliriz. Örneğin, iki bileşeni ve ' nin bulunduğu bir senaryoyu düşünün `module1` `module2` . Module1, Module2 içindeki işlevleri çağırır ve `module1` `module2` tek bir izlemede bulunan ve tek bir izlemede günlükleri almak için aşağıdaki yaklaşımı kullanabiliriz:

```python
# module1.py
import logging

from opencensus.trace import config_integration
from opencensus.trace.samplers import AlwaysOnSampler
from opencensus.trace.tracer import Tracer
from module2 import function_1

config_integration.trace_integrations(['logging'])
logging.basicConfig(format='%(asctime)s traceId=%(traceId)s spanId=%(spanId)s %(message)s')
tracer = Tracer(sampler=AlwaysOnSampler())

logger = logging.getLogger(__name__)
logger.warning('Before the span')
with tracer.span(name='hello'):
   logger.warning('In the span')
   function_1(tracer)
logger.warning('After the span')


# module2.py

import logging

from opencensus.trace import config_integration
from opencensus.trace.samplers import AlwaysOnSampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['logging'])
logging.basicConfig(format='%(asctime)s traceId=%(traceId)s spanId=%(spanId)s %(message)s')
tracer = Tracer(sampler=AlwaysOnSampler())

def function_1(parent_tracer=None):
    if parent_tracer is not None:
        tracer = Tracer(
                    span_context=parent_tracer.span_context,
                    sampler=AlwaysOnSampler(),
                )
    else:
        tracer = Tracer(sampler=AlwaysOnSampler())

    with tracer.span("function_1"):
        logger.info("In function_1")
```

## <a name="telemetry-correlation-in-net"></a>.NET 'te telemetri bağıntısı

.NET çalışma zamanı, [etkinlik](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve [diagnosticsource](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) 'un yardımıyla dağıtılmasını destekler

Application Insights .NET SDK, `DiagnosticSource` `Activity` telemetri toplamak ve ilişkilendirmek için ve kullanır.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-java"></a>Java 'da telemetri bağıntısı

Java [aracısının](./java-in-process-agent.md) yanı sıra [Java SDK](../../azure-monitor/app/java-get-started.md) sürümü 2.0.0 veya üzeri, telemetri otomatik bağıntısını destekler. `operation_id`Bir isteğin kapsamında verilen tüm telemetri (izlemeler, özel durumlar ve özel olaylar gibi) otomatik olarak doldurulur. [Java SDK Aracısı](../../azure-monitor/app/java-agent.md) yapılandırılmışsa, hizmet-hizmet çağrıları için de bağıntı üst bilgilerini (daha önce AÇıKLANAN) http aracılığıyla yayar.

> [!NOTE]
> Application Insights Java Aracısı, JMS, Kafka, netty/Webflox ve daha fazlası için istekleri ve bağımlılıkları otomatik olarak toplar. Java SDK için yalnızca Apache HttpClient aracılığıyla yapılan çağrılar bağıntı özelliği için desteklenir. (Kafka, Kbbitmq ve Azure Service Bus gibi) mesajlaşma teknolojileri genelinde otomatik bağlam yayma SDK 'da desteklenmez. 

> [!NOTE]
> Özel telemetri toplamak için, uygulamayı Java 2,6 SDK 'Sı ile birlikte belirlemeniz gerekir. 

### <a name="role-names"></a>Rol adları

[Uygulama eşlemesinde](../../azure-monitor/app/app-map.md)bileşen adlarının görüntülenme şeklini özelleştirmek isteyebilirsiniz. Bunu yapmak için, `cloud_RoleName` aşağıdaki eylemlerden birini gerçekleştirerek el ile ayarlayabilirsiniz:

- Application Insights Java Aracısı 3,0 için, bulut rolü adını şu şekilde ayarlayın:

    ```json
    {
      "role": {
        "name": "my cloud role name"
      }
    }
    ```
    Ayrıca, ortam değişkenini kullanarak bulut rolü adını da ayarlayabilirsiniz `APPLICATIONINSIGHTS_ROLE_NAME` .

- Application Insights Java SDK 2.5.0 ve üzeri ile `cloud_RoleName` `<RoleName>` ApplicationInsights.xml dosyanıza ekleyerek belirtebilirsiniz:

  ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
     <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
     <RoleName>** Your role name **</RoleName>
     ...
  </ApplicationInsights>
  ```

- Spring Boot uygulamasını Application Insights Spring Boot Starter ile birlikte kullanıyorsanız, uygulama için özel adınızı uygulama. Özellikler dosyasında ayarlamanız yeterlidir:

  `spring.application.name=<name-of-app>`

  Spring Boot Starter, `cloudRoleName` özelliği için girdiğiniz değere otomatik olarak atar `spring.application.name` .

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md)yazın.
- ASP.NET Core ve ASP.NET ' deki gelişmiş bağıntı senaryoları için bkz. [özel Işlemleri izleme](custom-operations-tracking.md).
- Diğer SDK 'lar için [cloud_RoleName ayarlama](./app-map.md#set-or-override-cloud-role-name) hakkında daha fazla bilgi edinin.
- Mikro hizmetinizin tüm bileşenlerini Application Insights ekleyin. [Desteklenen platformları](./platforms.md)inceleyin.
- Application Insights türleri için [veri modeline](./data-model.md) bakın.
- [Telemetrinin nasıl genişletileceğini ve filtreleneceğini](./api-filtering-sampling.md)öğrenin.
- [Application Insights config başvurusunu](configuration-with-applicationinsights-config.md)gözden geçirin.