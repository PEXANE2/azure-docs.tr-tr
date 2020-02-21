---
title: Azure Işlevleri 2. x için Host. JSON başvurusu
description: V2 çalışma zamanına sahip Azure Işlevleri Host. JSON dosyası için başvuru belgeleri.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: b9c57378df1510179c5a45b6aa669bab804aca5e
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484443"
---
# <a name="hostjson-reference-for-azure-functions-2x-and-later"></a>Azure Işlevleri 2. x ve üzeri için Host. JSON başvurusu 

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Işlevleri çalışma zamanının sürümünü seçin: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2 +](functions-host-json.md)

*Host. JSON* meta veri dosyası, bir işlev uygulaması için tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu makalede, Azure Işlevleri çalışma zamanının 2. x sürümünden itibaren kullanılabilen ayarlar listelenir.  

> [!NOTE]
> Bu makale, Azure Işlevleri 2. x ve sonraki sürümleri için kullanılır.  1\. x Işlevleri içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](functions-host-json-v1.md).

Diğer işlev uygulama yapılandırma seçenekleri, [uygulama ayarlarınızda](functions-app-settings.md) (dağıtılan uygulamalar için) veya [yerel. Settings. JSON](functions-run-local.md#local-settings-file) dosyanızda (yerel geliştirme için) yönetilir.

Ana bilgisayar. json ' daki bağlamalarla ilgili yapılandırma işlevleri işlev uygulamasındaki her işleve eşit olarak uygulanır. 

## <a name="sample-hostjson-file"></a>Örnek Host. JSON dosyası

Sürüm 2. x + için aşağıdaki örnek *Host. JSON* dosyasında tüm olası seçenekler belirtilmiştir (yalnızca iç kullanım için olan).

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 20,
              "evaluationInterval": "01:00:00",
              "initialSamplingPercentage": 100.0, 
              "samplingPercentageIncreaseTimeout" : "00:00:01",
              "samplingPercentageDecreaseTimeout" : "00:00:01",
              "minSamplingPercentage": 0.1,
              "maxSamplingPercentage": 100.0,
              "movingAverageRatio": 1.0,
              "excludedTypes" : "Dependency;Event",
              "includedTypes" : "PageView;Trace"
            },
            "enableLiveMetrics": true,
            "enableDependencyTracking": true,
            "enablePerformanceCountersCollection": true,            
            "httpAutoCollectionOptions": {
                "enableHttpTriggerExtendedInfoCollection": true,
                "enableW3CDistributedTracing": true,
                "enableResponseHeaderInjection": true
            },
            "snapshotConfiguration": {
                "agentEndpoint": null,
                "captureSnapshotMemoryWeight": 0.5,
                "failedRequestLimit": 3,
                "handleUntrackedExceptions": true,
                "isEnabled": true,
                "isEnabledInDeveloperMode": false,
                "isEnabledWhenProfiling": true,
                "isExceptionSnappointsEnabled": false,
                "isLowPrioritySnapshotUploader": true,
                "maximumCollectionPlanSize": 50,
                "maximumSnapshotsRequired": 3,
                "problemCounterResetInterval": "24:00:00",
                "provideAnonymousTelemetry": true,
                "reconnectInterval": "00:15:00",
                "shadowCopyFolder": null,
                "shareUploaderProcess": true,
                "snapshotInLowPriorityThread": true,
                "snapshotsPerDayLimit": 30,
                "snapshotsPerTenMinutesLimit": 1,
                "tempFolder": null,
                "thresholdForSnapshotting": 1,
                "uploaderProxy": null
            }
        }
    },
    "managedDependency": {
        "enabled": true
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

Bu makalenin aşağıdaki bölümlerinde her üst düzey özellik açıklanmaktadır. Aksi belirtilmedikçe tümü isteğe bağlıdır.

## <a name="aggregator"></a>'yı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Bu ayar [günlüğe kaydetme](#logging)işleminin bir alt öğesidir.

[Örnekleme seçenekleri](./functions-monitoring.md#configure-sampling)de dahil olmak üzere Application Insights seçeneklerini denetler.

Tüm JSON yapısı için, önceki [örnek Host. JSON dosyasına](#sample-hostjson-file)bakın.

> [!NOTE]
> Günlük örnekleme, bazı yürütmelerin Application Insights İzleyicisi dikey penceresinde gösterilmemesine neden olabilir. Günlük örneklemeyi önlemek için `applicationInsights` değerine `samplingExcludedTypes: "Request"` ekleyin.

| Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| samplingSettings | yok | Bkz. [ApplicationInsights. samplingSettings](#applicationinsightssamplingsettings). |
| samplingExcludedTypes | null | Örneklemeyi istemediğiniz türlerin noktalı virgülle ayrılmış listesi. Tanınan türler şunlardır: bağımlılık, olay, özel durum, PageView, Istek, Izleme. Belirtilen türlerin tüm örnekleri iletilir; belirtilmeyen türler örneklenir. |
| Samplingıncludedtypes | null | Örneklemeyi yapmak istediğiniz türlerin noktalı virgülle ayrılmış listesi; boş liste tüm türleri gösterir. Burada listelenen `samplingExcludedTypes` geçersiz kılma türlerinde listelenen tür. Tanınan türler şunlardır: bağımlılık, olay, özel durum, PageView, Istek, Izleme. Belirtilen türlerin tüm örnekleri iletilir; belirtilmeyen türler örneklenir. |
| Enableliveölçümleri | true | Canlı ölçüm toplamayı etkin bir şekilde sunar. |
| enableDependencyTracking | true | Bağımlılık izlemeyi etkinleştirilir. |
| enablePerformanceCountersCollection | true | Kudu performans sayaçlarını toplamayı etkinleştirilir. |
| liveMetricsInitializationDelay | 00:00:15 | Yalnızca dahili kullanım içindir. |
| httpAutoCollectionOptions | yok | Bkz. [ApplicationInsights. httpAutoCollectionOptions](#applicationinsightshttpautocollectionoptions). |
| snapshotConfiguration | yok | Bkz. [ApplicationInsights. snapshotConfiguration](#applicationinsightssnapshotconfiguration). |

### <a name="applicationinsightssamplingsettings"></a>ApplicationInsights. samplingSettings

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| isEnabled | true | Örneklemeyi etkinleştirilir veya devre dışı bırakır. | 
| maxTelemetryItemsPerSecond | 20 | Her bir sunucu konağında saniyede günlüğe kaydedilen telemetri öğelerinin hedef sayısı. Uygulamanız birçok ana bilgisayarda çalışıyorsa, bu değeri, genel hedef trafik hızında kalacak şekilde küçültün. | 
| evaluationInterval | 01:00:00 | Geçerli telemetri hızının yeniden değerlendirilme aralığı. Değerlendirme, hareketli ortalama olarak gerçekleştirilir. Telemetriyi ani bursts 'e tabi olursa bu aralığı kısaltmak isteyebilirsiniz. |
| ınitialörneklingpercentage| 1.0 | Örnekleme işleminin başlangıcında uygulanan ilk örnekleme yüzdesi, yüzdeyi dinamik olarak farklılık gösterir. Hata ayıklarken değeri küçültmeyin. |
| samplingPercentageIncreaseTimeout | 00:00:01 | Örnekleme yüzdesi değeri değiştiğinde, bu özellik daha sonra Application Insights daha sonra örnekleme yüzdesini daha fazla veri yakalamaya ne kadar süre sonra yeniden tetiklemesine izin verileceğini belirler. |
| samplingPercentageDecreaseTimeout | 00:00:01 | Örnekleme yüzdesi değeri değiştiğinde, bu özellik daha sonra daha az veri yakalamak için örnekleme yüzdesini yeniden ne kadar küçük bir süre sonra Application Insights izin verileceğini belirler. |
| Minörneklingpercentage | 0.1 | Örnekleme yüzdesi farklılık gösterdiğinden, bu özellik izin verilen minimum örnekleme yüzdesini belirler. |
| maxSamplingPercentage | 0.1 | Örnekleme yüzdesi farklılık gösterdiğinden, bu özellik izin verilen en fazla örnekleme yüzdesini belirler. |
| movingAverageRatio | 1.0 | Hareketli ortalama hesaplamasında, en son değere atanan ağırlık. 1 ' den küçük veya buna eşit bir değer kullanın. Daha küçük değerler, algoritmayı ani değişikliklere karşı daha az reaktif hale getirir. |

### <a name="applicationinsightshttpautocollectionoptions"></a>ApplicationInsights. httpAutoCollectionOptions

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| Enablehttptriggerextendedınınfocollection | true | HTTP Tetikleyicileri için genişletilmiş HTTP isteği bilgilerini etkinleştirilir veya devre dışı bırakır: gelen istek bağıntı üstbilgileri, çok izleme anahtarları desteği, HTTP yöntemi, yol ve yanıt. |
| enableW3CDistributedTracing | true | W3C dağıtılmış izleme protokolünün desteğini etkinleştirir veya devre dışı bırakır (ve eski bağıntı şemasını etkinleştirir). `enableHttpTriggerExtendedInfoCollection` true ise varsayılan olarak etkindir. `enableHttpTriggerExtendedInfoCollection` false ise, bu bayrak gelen istekler için değil yalnızca giden istekler için geçerlidir. |
| Enableresponseheaderınjection | true | Çoklu bileşen bağıntı üstbilgilerinin yanıtlara eklenmesine izin vermez veya devre dışı bırakır. Ekleme özelliğinin etkinleştirilmesi, birden çok izleme anahtarı kullanıldığında Application Insights bir uygulama eşlemesi oluşturmasına izin verir. `enableHttpTriggerExtendedInfoCollection` true ise varsayılan olarak etkindir. `enableHttpTriggerExtendedInfoCollection` false ise, bu ayar uygulanmaz. |

### <a name="applicationinsightssnapshotconfiguration"></a>ApplicationInsights. snapshotConfiguration

Anlık görüntüler hakkında daha fazla bilgi için bkz. [.NET uygulamalarında özel durumlarla Ilgili hata ayıklama anlık görüntüleri](/azure/azure-monitor/app/snapshot-debugger) ve [Application Insights Snapshot Debugger veya anlık görüntüleri görüntüleme sorunlarını giderme](/azure/azure-monitor/app/snapshot-debugger-troubleshoot).

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| Tendtendpoint | null | Application Insights Snapshot Debugger hizmetine bağlanmak için kullanılan uç nokta. Null ise, varsayılan bir uç nokta kullanılır. |
| captureSnapshotMemoryWeight | 0,5 | Anlık görüntü almak için yeterli bellek olup olmadığı denetlenirken geçerli işlem belleği boyutuna verilen ağırlık. Beklenen değer 0 ' dan büyük bir doğru kesenden (0 < CaptureSnapshotMemoryWeight < 1) fazla. |
| failedRequestLimit | 3 | Telemetri işlemcisi devre dışı bırakılmadan önce anlık görüntü istemek için başarısız istek sayısı sınırı.|
| handleUntrackedExceptions | true | Application Insights telemetri tarafından izlenmeyen özel durumların izlenmesini mümkün veya devre dışı bırakır. |
| isEnabled | true | Anlık görüntü toplamayı etkinleştirilir veya devre dışı bırakır | 
| isEnabledInDeveloperMode | false | Geliştirici modunda anlık görüntü toplamayı etkinleştirme veya devre dışı bırakır. |
| Isenabledwhenprofilleme | true | Application Insights Profiler ayrıntılı bir profil oluşturma oturumu toplasa bile, anlık görüntü oluşturmayı etkinleştirilir veya devre dışı bırakır. |
| ısexceptionsnappointsenabled | false | Özel durumların filtrelenmesini mümkün veya devre dışı bırakır. |
| ılowprioritysnapshotuploader | true | Anlık görüntüyle ayarlana Tuploader işleminin normal öncelikte çalıştırılıp çalıştırılmayacağını belirler. |
| maximumCollectionPlanSize | 50 | Bire 9999 arasında bir aralıkta izleyediğimiz en fazla sorun sayısı. |
| Maximumanlık görüntüyle gerekli | 3 | Tek bir sorun için toplanan en fazla anlık görüntü sayısı, bire 999 arasındadır. Bir sorun, uygulamanızda tek bir throw deyimleri olarak düşünülebilir. Bir sorun için toplanan anlık görüntü sayısı bu değere ulaştığında, sorun sayaçları sıfırlanana kadar bu sorun için daha fazla anlık görüntü toplanmaz (bkz. `problemCounterResetInterval`) ve `thresholdForSnapshotting` sınırına yeniden ulaşıldı. |
| Problemcounterresetınterval | 24:00:00 | Bir aralıktaki sorun sayaçlarını bir dakikadan yedi güne sıfırlama sıklığı. Bu aralığa ulaşıldığında, tüm sorun sayıları sıfıra sıfırlanır. Anlık görüntü yapma eşiğine zaten ulaşmış olan ancak `maximumSnapshotsRequired`anlık görüntü sayısı henüz üretilmemiş olan sorunlar, etkin durumda kaldı. |
| Provideanonymoustelemetrisi | true | Microsoft 'a anonim kullanım ve hata telemetrisi gönderileceğini belirler. Bu telemetri, Snapshot Debugger sorunları gidermeye yardımcı olmak için Microsoft 'a başvurdıysanız kullanılabilir. Kullanım düzenlerini izlemek için de kullanılır. |
| Reconnectınterval | 00:15:00 | Snapshot Debugger uç noktasına ne sıklıkta yeniden bağlanıyoruz. İzin verilen Aralık, bir güne kadar bir dakikadır. |
| shadowCopyFolder | null | Gölge kopyalama ikilileri için kullanılacak klasörü belirtir. Ayarlanmamışsa, aşağıdaki ortam değişkenleri tarafından belirtilen klasörler sırayla denenir: Fabric_Folder_App_Temp, LOCALAPPDATA, APPDATA, TEMP. |
| shareUploaderProcess | true | True ise, bir Snapshot Tuploader 'ın yalnızca bir örneği, ınstrumentationkey 'i paylaşan birden çok uygulama için anlık görüntüleri toplayıp karşıya yükler. False olarak ayarlanırsa, anlık görüntüyle ilgili Tuploader her bir (ProcessName, ınstrumentationkey) tanımlama grubu için benzersiz olacaktır. |
| Anlık Ileti Tinlowprioritythread | true | Düşük GÇ öncelikli iş parçacığında anlık görüntülerin işleneceğini belirler. Anlık görüntü oluşturma hızlı bir işlemdir, ancak Snapshot Debugger hizmetine bir anlık görüntü yüklemek için öncelikle diske bir mini döküm olarak yazılması gerekir. Bu, anlık görüntüyle ilgili Tuploader sürecinde meydana gelir. Bu değerin true olarak ayarlanması, mini döküm yazmak için düşük öncelikli GÇ kullanır ve bu, kaynaklar için uygulamanızla rekabet etmenize izin vermez. Bu değerin false olarak ayarlanması, uygulamanızı yavaşlatarak mini döküm oluşturma işlemini hızlandırır. |
| Anlık görüntüyle (gün sayısı) | 30 | Bir gün içinde izin verilen en fazla anlık görüntü sayısı (24 saat). Bu sınır Application Insights hizmet tarafında da zorlanır. Karşıya yüklemeler, uygulama başına günlük 50 (yani, izleme anahtarı başına) ile sınırlıdır. Bu değer, karşıya yükleme sırasında reddedilecek ek anlık görüntülerin oluşturulmasına engel olur. Sıfır değeri sınırı tamamen kaldırır, önerilmez. |
| snapshotsPerTenMinutesLimit | 1 | 10 dakika içinde izin verilen en fazla anlık görüntü sayısı. Bu değerde üst sınır olmasa da, uygulamanızın performansını etkileyebileceğinden, üretim iş yükleri üzerinde dikkatli davranmaya dikkat edin. Anlık görüntü oluşturmak hızlıdır, ancak anlık görüntünün bir mini döküm oluşturması ve Snapshot Debugger hizmetine yüklenmesi, kaynak (hem CPU hem de g/ç) için uygulamanızla rekabet edecek çok daha yavaş bir işlemdir. |
| tempFolder | null | Mini dökümler ve Uploader günlük dosyalarının yazılacağı klasörü belirtir. Ayarlanmamışsa, *%Temp%\dökümler* kullanılır. |
| Thresholdfori biçimlendirme | 1 | Anlık görüntü sorulmadan önce kaç Application Insights kez bir özel durum görmeniz gerekir. |
| uploaderProxy | null | Snapshot Uploader işleminde kullanılan proxy sunucusunu geçersiz kılar. Uygulamanız bir ara sunucu üzerinden İnternet 'e bağlanıyorsa bu ayarı kullanmanız gerekebilir. Snapshot Collector uygulamanızın sürecinde çalışır ve aynı ara sunucu ayarlarını kullanacaktır. Ancak, anlık görüntü yükleyicisi ayrı bir işlem olarak çalışır ve proxy sunucusunu el ile yapılandırmanız gerekebilir. Bu değer null ise Snapshot Collector, sistem .net. WebRequest. DefaultWebProxy ' yi inceleyerek ve değeri Snapshot Uploader ' e geçirerek proxy 'nin adresini otomatik olarak yeniden dener. Bu değer null değilse, oto algılaması kullanılmaz ve burada belirtilen proxy sunucusu, anlık görüntü yükleyicisi 'nde kullanılacaktır. |

## <a name="cosmosdb"></a>cosmosDb

Yapılandırma ayarı, [Cosmos DB Tetikleyiciler ve bağlamalarda](functions-bindings-cosmosdb-v2.md#host-json)bulunabilir.

## <a name="durabletask"></a>durableTask

Yapılandırma ayarı, [dayanıklı işlevler bağlamalarında](durable/durable-functions-bindings.md#host-json)bulunabilir.

## <a name="eventhub"></a>eventHub

Yapılandırma ayarları, [Olay Hub 'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md#host-json)' nda bulunabilir. 

## <a name="extensions"></a>uzantılar

[Http](#http) ve [eventHub](#eventhub)gibi bağlamaya özgü tüm ayarları içeren bir nesne döndüren özellik.

## <a name="extensionbundle"></a>Extensiondemeti 

Uzantı paketleri, işlev uygulamanıza uygun bir Işlev kümesi bağlama uzantısı eklemenize olanak sağlar. Daha fazla bilgi için bkz. [yerel geliştirme Için uzantı paketleri](functions-bindings-register.md#extension-bundles).

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="functions"></a>işlevleri

İş konağının çalıştığı işlevlerin listesi. Boş bir dizi tüm işlevleri Çalıştır anlamına gelir. Yalnızca [yerel olarak çalışırken](functions-run-local.md)kullanılmak üzere tasarlanmıştır. Azure 'daki işlev uygulamaları ' nda, bu ayarı kullanmak yerine belirli işlevleri devre dışı bırakmak için [Azure işlevlerinde işlevleri devre dışı](disable-function.md) bırakma bölümündeki adımları izlemeniz gerekir.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm işlevler için zaman aşımı süresini gösterir. TimeSpan dize biçimini izler. Sunucusuz tüketim planında geçerli Aralık 1 saniye ila 10 dakika ve varsayılan değer 5 dakikadır.  

Premium planda geçerli Aralık 1 saniye ila 60 dakikaya ve varsayılan değer 30 dakikadır.

Adanmış bir (App Service) planında, genel bir sınır yoktur ve varsayılan değer 30 dakikadır. `-1` değeri, sınırsız yürütmeyi gösterir, ancak sabit bir üst sınırı korumak önerilir.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[Konak sistem durumu izleyicisinin](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)yapılandırma ayarları.

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|enabled|true|Özelliğin etkinleştirilip etkinleştirilmeyeceğini belirtir. | 
|Healthcheckınterval|10 saniye|Düzenli arka plan sistem durumu denetimleri arasındaki zaman aralığı. | 
|healthCheckWindow|2 dakika|`healthCheckThreshold` ayarıyla birlikte kullanılan bir kayan zaman penceresi.| 
|healthCheckThreshold|6|Konak geri dönüşüm başlatılmadan önce sistem durumu denetiminin başarısız olması için en fazla sayı.| 
|Onay eşiği|0,80|Performans sayacının sağlıksız olduğu kabul edilecek eşik.| 

## <a name="http"></a>http

Yapılandırma ayarları, [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook-output.md#hostjson-settings)içinde bulunabilir.

## <a name="logging"></a>günlük kaydı

Application Insights dahil olmak üzere, işlev uygulamasının günlük davranışlarını denetler.

```json
"logging": {
    "fileLoggingMode": "debugOnly"
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "console": {
        ...
    },
    "applicationInsights": {
        ...
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|fileLoggingMode|yalnızca Debug|Hangi dosya günlüğü düzeyinin etkin olduğunu tanımlar.  Seçenekler `never`, `always``debugOnly`. |
|logLevel|yok|Uygulamadaki işlevler için günlük kategorisi filtrelemeyi tanımlayan nesne. Sürüm 2. x ve üzeri, günlük kategorisi filtrelemesi için ASP.NET Core düzeni izler. Bu ayar, belirli işlevler için günlüğü filtrelemenizi sağlar. Daha fazla bilgi için ASP.NET Core belgelerine [günlük filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) bölümüne bakın. |
|console|yok| [Konsol](#console) günlüğü ayarı. |
|applicationInsights|yok| [ApplicationInsights](#applicationinsights) ayarı. |

## <a name="console"></a>console

Bu ayar [günlüğe kaydetme](#logging)işleminin bir alt öğesidir. Hata ayıklama modunda olmadığında konsol günlüğünü denetler.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|isEnabled|false|Konsol günlüğünü etkinleştir veya devre dışı bırakır.| 

## <a name="manageddependency"></a>managedDependency bağımlılığı

Yönetilen bağımlılık Şu anda yalnızca PowerShell tabanlı işlevlerde desteklenen bir özelliktir. Bağımlılıkların hizmet tarafından otomatik olarak yönetilmesine olanak sağlar. `enabled` özelliği `true`olarak ayarlandığında `requirements.psd1` dosyası işlenir. Tüm küçük sürümler bırakıldığında bağımlılıklar güncelleştirilir. Daha fazla bilgi için bkz. PowerShell makalesinde [yönetilen bağımlılık](functions-reference-powershell.md#dependency-management) .

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="queues"></a>sıralar

Yapılandırma ayarları, [depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue-output.md#host-json)bölümünde bulunabilir.  

## <a name="sendgrid"></a>sendGrid

Yapılandırma ayarı, [SendGrid Tetikleyicileri ve bağlamaları](functions-bindings-sendgrid.md#host-json)içinde bulunabilir.

## <a name="servicebus"></a>serviceBus

Yapılandırma ayarı, [Service Bus Tetikleyiciler ve bağlamalarda](functions-bindings-service-bus-output.md#host-json)bulunabilir.

## <a name="singleton"></a>Adet

Tek kilit davranışı için yapılandırma ayarları. Daha fazla bilgi için bkz. [Singleton desteği hakkında GitHub sorunu](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|Kilit dönemi|00:00:15|İşlev düzeyi kilitlerinin alındığı dönem için. Kilitleri otomatik yenileme.| 
|listenerLockPeriod|00:01:00|Dinleyici kilitlerinin alındığı dönem.| 
|listenerLockRecoveryPollingInterval|00:01:00|Başlangıçta dinleyici kilidi alınamadığından, dinleyici kilidi kurtarma için kullanılan zaman aralığı.| 
|Locktanışılationtimeout|00:01:00|Çalışma zamanının kilit edinmeye çalışacak en uzun süre.| 
|Locktanışmalationpollingınterval|yok|Kilit alma denemeleri arasındaki Aralık.| 

## <a name="version"></a>version

Bu değer Host. JSON şema sürümünü gösterir. Sürüm dizesi `"version": "2.0"`, v2 çalışma zamanını veya sonraki bir sürümünü hedefleyen bir işlev uygulaması için gereklidir. Ana bilgisayar yok. v2 ve v3 arasında JSON şeması değişikliği.

## <a name="watchdirectories"></a>watchDirectories

Değişiklikler için izlenmesi gereken bir [paylaşılan kod dizinleri](functions-reference-csharp.md#watched-directories) kümesi.  Bu dizinlerdeki kod değiştirildiğinde, değişikliklerin işlevleriniz tarafından çekilmesini sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Host. json dosyasını güncelleştirme hakkında bilgi edinin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ortam değişkenlerinde genel ayarları gör](functions-app-settings.md)
