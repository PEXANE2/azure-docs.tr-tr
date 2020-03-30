---
title: Azure İşlevler için zamanlayıcı tetikleyicisi
description: Azure İşlevlerinde zamanlayıcı tetikleyicilerinin nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.topic: reference
ms.date: 09/08/2018
ms.author: cshoe
ms.custom: ''
ms.openlocfilehash: 566d6ccf43024692e19bcd6639fe5cfbbba0660d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80056405"
---
# <a name="timer-trigger-for-azure-functions"></a>Azure İşlevler için zamanlayıcı tetikleyicisi 

Bu makalede, Azure İşlevlerinde zamanlayıcı tetikleyicileriyle nasıl çalışılalış lar açıklanmaktadır. Zamanlayıcı tetikleyicisi, bir işlevi zamanlamada çalıştırmanızı sağlar. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

Zamanlayıcı tetikleyicisi [Microsoft.Azure.WebJobs.Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi sürüm 2.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Timers/) GitHub deposundadır.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler - Fonksiyonlar 2.x ve üzeri

Zamanlayıcı tetikleyicisi [Microsoft.Azure.WebJobs.Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketinde, sürüm 3.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub deposundadır.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnek, dakikaların beşe bölünebilen bir değeri olduğunda çalıştırılan bir [C# işlevini](functions-dotnet-class-library.md) gösterir (örneğin işlev 18:57:00'de başlarsa, bir sonraki performans 19:00:00'da olacaktır). Nesne [`TimerInfo`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) işleve aktarılır.

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
{
    if (myTimer.IsPastDue)
    {
        log.LogInformation("Timer is running late!");
    }
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında bir zamanlayıcı tetikleyicisi bağlamave bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev, bu işlev çağırmasının cevapsız bir zamanlama oluşumundan kaynaklanıp kaynaklanmasından kaynaklanıp kaynaklanmasına neden olduğunu belirten bir günlük yazar. Nesne [`TimerInfo`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) işleve aktarılır.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

İşte C# komut dosyası kodu:

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
{
    if (myTimer.IsPastDue)
    {
        log.LogInformation("Timer is running late!");
    }
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında bir zamanlayıcı tetikleyicisi bağlamayı ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, bu işlev çağırmasının cevapsız bir zamanlama oluşumundan kaynaklanıp kaynaklanmasından kaynaklanıp kaynaklanmasına neden olduğunu belirten bir günlük yazar. Bir [zamanlayıcı nesne](#usage) işlevine geçirilir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

İşte JavaScript kodu:

```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if (myTimer.IsPastDue)
    {
        context.log('Node is running late!');
    }
    context.log('Node timer trigger function ran!', timeStamp);   

    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, *işlevi.json* dosyasında yapılandırması açıklanan bir zamanlayıcı tetikleyici bağlama kullanır. Bağlamayı kullanan gerçek [Python işlevi](functions-reference-python.md) * __init__.py* dosyasında açıklanmıştır. İşlev içine geçirilen nesne türü [azure.functions.TimerRequest nesnesidir.](/python/api/azure-functions/azure.functions.timerrequest) İşlev mantığı, geçerli çağırmanın cevapsız zamanlama oluşumundan kaynaklanıp kaynaklanması gerektiğini belirten günlüklere yazar. 

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "name": "mytimer",
    "type": "timerTrigger",
    "direction": "in",
    "schedule": "0 */5 * * * *"
}
```

Python kodu aşağıdavelvere vermiştir:

```python
import datetime
import logging

import azure.functions as func


def main(mytimer: func.TimerRequest) -> None:
    utc_timestamp = datetime.datetime.utcnow().replace(
        tzinfo=datetime.timezone.utc).isoformat()

    if mytimer.past_due:
        logging.info('The timer is past due!')

    logging.info('Python timer trigger function ran at %s', utc_timestamp)
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek işlev her beş dakikada bir tetikler ve yürütür. `@TimerTrigger` İşlev üzerindeki ek açıklama [CRON ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression)ile aynı dize biçimini kullanarak zamanlama tanımlar.

```java
@FunctionName("keepAlive")
public void keepAlive(
  @TimerTrigger(name = "keepAliveTrigger", schedule = "0 */5 * * * *") String timerInfo,
      ExecutionContext context
 ) {
     // timeInfo is a JSON string, you can deserialize it to an object using your favorite JSON library
     context.getLogger().info("Timer is triggered: " + timerInfo);
}
```

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs)kullanın.

Özniteliğin oluşturucusu cron ifadesi veya `TimeSpan`bir . Yalnızca işlev `TimeSpan` uygulaması bir Uygulama Hizmeti planında çalışıyorsa kullanabilirsiniz. `TimeSpan`Tüketim veya Elastik Premium Fonksiyonlar için desteklenmez.

Aşağıdaki örnekte CRON ifadesi gösterilmektedir:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
{
    if (myTimer.IsPastDue)
    {
        log.LogInformation("Timer is running late!");
    }
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`@TimerTrigger` İşlev üzerindeki ek açıklama [CRON ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression)ile aynı dize biçimini kullanarak zamanlama tanımlar.

```java
@FunctionName("keepAlive")
public void keepAlive(
  @TimerTrigger(name = "keepAliveTrigger", schedule = "0 */5 * * * *") String timerInfo,
      ExecutionContext context
 ) {
     // timeInfo is a JSON string, you can deserialize it to an object using your favorite JSON library
     context.getLogger().info("Timer is triggered: " + timerInfo);
}
```

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `TimerTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | "TimerTrigger" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön** | yok | "In" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki zamanlayıcı nesnesini temsil eden değişkenin adı. | 
|**Zamanlama**|**Zamanlamaİfade**|[CRON ifadesi](#ncrontab-expressions) veya [TimeSpan](#timespan) değeri. A `TimeSpan` yalnızca Bir Uygulama Hizmet Planı üzerinde çalışan bir işlev uygulaması için kullanılabilir. Zamanlama ifadesini bir uygulama ayarına koyup bu özelliği, aşağıdaki **%** örnekte olduğu gibi, "%ScheduleAppSetting%". |
|**runOnStartup**|**RunonStartup**|Çalışma `true`zamanı başladığında işlev çağrılır. Örneğin, çalışma süresi, işlev uygulaması hareketsizlik nedeniyle boşta kaldıktan sonra uyandığında başlar. işlev değişiklikleri nedeniyle işlev uygulaması yeniden başlatıldığında ve işlev uygulaması ölçeklendiğinde. Yani **runOnStartup** nadiren hiç `true`ayarlanmış olmalıdır , özellikle üretim. |
|**useMonitor**|**UseMonitor**|`true` Zamanlamanın `false` izlenip izlenmemesi gerektiğini belirtmek için ayarlayın. Zamanlama izleme, işlev uygulaması örnekleri yeniden başlatılsa bile zamanlamanın doğru şekilde tutulmasını sağlamaya yardımcı olmak için zamanlama oluşumlarını sürdürür. Açıkça ayarlanmazsa, `true` varsayılan değer, yineleme aralığı 1 dakikadan büyük veya eşit olan zamanlamalar içindir. Dakikada birden fazla tetikleyen zamanlamalar için `false`varsayılan değer.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!CAUTION]
> **RunOnStartup'ı** `true` üretimde ayarlamanızı öneririz. Bu ayarı kullanmak, kodun son derece öngörülemeyen zamanlarda yürütülmesini sağlar. Belirli üretim ayarlarında, bu ekstra yürütmeler Tüketim planlarında barındırılan uygulamalar için önemli ölçüde daha yüksek maliyetlere neden olabilir. Örneğin, **runOnStartup** etkin olduğunda, işlev uygulamanız ölçeklendiğinde tetikleyici çağrılır. Üretimde **runOnStartup'ı** etkinleştirmeden önce işlevlerinizin üretim davranışını tam olarak anladığınızdan emin olun.   

## <a name="usage"></a>Kullanım

Zamanlayıcı tetikleyici işlevi çağrıldığızaman, bir zamanlayıcı nesnesi işleve aktarılır. Aşağıdaki JSON zamanlayıcı nesnesinin örnek bir gösterimidir.

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00+00:00",
        "LastUpdated":"2016-10-04T10:16:00+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

Özellik, `IsPastDue` `true` geçerli işlev çağırmasının zamanlanandan daha geç olmasıdır. Örneğin, bir işlev uygulaması yeniden başlatma bir çağrının kaçırılmasına neden olabilir.

## <a name="ncrontab-expressions"></a>NCRONTAB ifadeleri 

Azure İşlevleri, NCRONTAB ifadelerini yorumlamak için [NCronTab](https://github.com/atifaziz/NCrontab) kitaplığını kullanır. NCRONTAB ifadesi CRON ifadesine benzer, ancak başlangıçta saniyeler içinde zaman hassasiyeti için kullanmak üzere ek bir altıncı alan içerir:

`{second} {minute} {hour} {day} {month} {day-of-week}`

Her alan aşağıdaki değer türlerinden birine sahip olabilir:

|Tür  |Örnek  |Tetiklendiğinde  |
|---------|---------|---------|
|Belirli bir değer |<nobr>"0 5 * * * *"</nobr>|hh:05:00 de hh her saat (saatte bir kez)|
|Tüm değerler`*`( )|<nobr>"0 * 5 * * *"</nobr>|her gün 5:mm:00'da, mm saatin her dakikası (günde 60 kez)|
|Bir aralık`-` (operatör)|<nobr>"5-7 * * * * *"</nobr>|hh:mm:05,hh:mm:06 ve hh:mm:07 her saatin her dakikası (dakikada 3 kez)|
|Bir değer kümesi`,` (işleç)|<nobr>"5,8,10 * * * * *"</nobr>|hh:mm:05,hh:mm:08 ve hh:mm:10 her saatin her dakikası (dakikada 3 kez)|
|Bir aralık`/` değeri (işleç)|<nobr>"0 */5 * * * *"</nobr>|hh:00:00, hh:05:00, hh:10:00, ve benzeri hh:55:00 ile nerede hh her saat (12 kez bir saat)|

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="ncrontab-examples"></a>NCRONTAB örnekleri

Azure İşlevlerinde zamanlayıcı tetikleyicisi için kullanabileceğiniz NCRONTAB ifadelerine bazı örnekler aşağıda verilmiştir.

|Örnek|Tetiklendiğinde  |
|---------|---------|
|`"0 */5 * * * *"`|her beş dakikada bir|
|`"0 0 * * * *"`|her saatin başında bir kez|
|`"0 0 */2 * * *"`|her iki saatte bir|
|`"0 0 9-17 * * *"`|09:00-17:00 arasında her saat bir kez|
|`"0 30 9 * * *"`|Her gün 09:30'da|
|`"0 30 9 * * 1-5"`|Hafta içi her gün sabah 9:30'da|
|`"0 30 9 * Jan Mon"`|Ocak ayının her Pazartesi günü 09:30'da|


### <a name="ncrontab-time-zones"></a>NCRONTAB saat dilimleri

CRON ifadesindeki sayılar, zaman aralığına değil, bir zaman ve tarihe başvurur. Örneğin, alandaki `hour` 5, her 5 saatte bir değil, 5:00'yi ifade eder.

CRON ifadeleri ile kullanılan varsayılan saat dilimi Eşgüdümlü Evrensel Saat (UTC) idi. CRON ifadenizin başka bir saat dilimini temel alması `WEBSITE_TIME_ZONE`için, işlev uygulamanız için bir uygulama ayarı oluşturun. [Microsoft Saat Dilimi Dizini'nde](https://technet.microsoft.com/library/cc749073)gösterildiği gibi istenen saat diliminin adına değeri ayarlayın.

  > [!NOTE]
  > `WEBSITE_TIME_ZONE`şu anda Linux Tüketim planında desteklenmez.

Örneğin, *Doğu Standart Saati* UTC-05:00'dir. Zamanlayıcınızın her gün SAAT 10:00 EST'de ateş itetiklemesi için UTC saat dilimini oluşturan aşağıdaki NCRONTAB ifadesini kullanın:

```
"0 0 15 * * *"
``` 

Veya adlandırılmış `WEBSITE_TIME_ZONE` işlev uygulamanız için bir uygulama ayarı oluşturun ve değeri **Doğu Standart Saati**olarak ayarlayın.  Sonra aşağıdaki NCRONTAB ifadesini kullanır: 

```
"0 0 10 * * *"
``` 

Kullandığınızda, `WEBSITE_TIME_ZONE`saat, gün ışığından yararlanma saati gibi belirli saat dilimindeki saat değişiklikleri için ayarlanır. 

## <a name="timespan"></a>TimeSpan

 A `TimeSpan` yalnızca Bir Uygulama Hizmet Planı üzerinde çalışan bir işlev uygulaması için kullanılabilir.

CRON ifadesinin aksine, bir `TimeSpan` değer her işlev çağırması arasındaki zaman aralığını belirtir. Bir işlev belirtilen aralıktan daha uzun süre çalıştırdıktan sonra tamamlandığında, zamanlayıcı işlevi hemen yeniden çağırır.

Bir dize olarak `TimeSpan` ifade `hh:mm:ss` `hh` edilen biçim, 24'ten küçük olduğunda dır. İlk iki basamak 24 veya daha büyük `dd:hh:mm`olduğunda, biçim . İşte bazı örnekler:

|Örnek |Tetiklendiğinde  |
|---------|---------|
|"01:00:00" | her saat        |
|"00:01:00"|her dakika         |
|"24:00:00" | her 24 günde bir        |
|"1.00:00:00" | Her gün        |

## <a name="scale-out"></a>Ölçeği genişletme

Bir işlev uygulaması birden çok örneğe ölçeklenirse, zamanlayıcı tarafından tetiklenen işlevin yalnızca tek bir örneği tüm örneklerde çalıştırılır.

## <a name="function-apps-sharing-storage"></a>Depolamayı paylaşan işlev uygulamaları

Uygulama hizmetine dağıtılmayan işlev uygulamaları arasında depolama hesapları paylaşıyorsanız, her uygulamaya açıkça ana bilgisayar kimliği atamanız gerekebilir.

| Fonksiyonlar sürümü | Ayar                                              |
| ----------------- | ---------------------------------------------------- |
| 2.x (ve daha yüksek)  | `AzureFunctionsWebHost__hostid`çevre değişkeni |
| 1.x               | `id`in *host.json*                                  |

Tanımlayıcı değeri atlayabilir veya her işlev uygulamasının tanımlayıcı yapılandırmasını el ile farklı bir değere ayarlayabilirsiniz.

Zamanlayıcı tetikleyicisi, bir işlev uygulaması birden çok örneğe ölçeklendiğinde yalnızca bir zamanlayıcı örneği olduğundan emin olmak için bir depolama kilidi kullanır. İki işlev uygulaması aynı tanımlayıcı yapılandırmayı paylaşıyorsa ve her biri bir zamanlayıcı tetikleyicisi kullanıyorsa, yalnızca bir zamanlayıcı çalışır.

## <a name="retry-behavior"></a>Davranışı yeniden deneyin

Sıra tetikleyicisinin aksine, zamanlayıcı tetikleyicisi bir işlev başarısız olduktan sonra yeniden denmez. Bir işlev başarısız olduğunda, zamanlamada bir sonraki zamana kadar yeniden çağrılmaz.

## <a name="troubleshooting"></a>Sorun giderme

Zamanlayıcı tetikleyicisi beklendiği gibi çalışmadığında ne yapmanız gerekenler hakkında bilgi [için](https://github.com/Azure/azure-functions-host/wiki/Investigating-and-reporting-issues-with-timer-triggered-functions-not-firing)bkz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Zamanlayıcı tetikleyicisi kullanan hızlı bir başlatmaya gitme](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
