---
title: Azure İşlevler için host.json başvurusu 2.x
description: V2 çalışma zamanı ile Azure İşlemeleri ana bilgisayar.json dosyası için başvuru belgeleri.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 7b3c9d15fc8cf3a1651c44a5656f731a7820e344
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405478"
---
# <a name="hostjson-reference-for-azure-functions-2x-and-later"></a>Azure İşlevler 2.x ve sonrası için host.json başvurusu 

> [!div class="op_single_selector" title1="Kullandığınız Azure İşlevleri çalışma zamanı sürümünü seçin: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2+](functions-host-json.md)

*Host.json* meta veri dosyası, bir işlev uygulamasının tüm işlevlerini etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, Azure İşlevleri çalışma zamanının sürüm 2.x'inden başlayarak kullanılabilen ayarlar listelenir.  

> [!NOTE]
> Bu makale, Azure İşlevler 2.x ve sonraki sürümler içindir.  Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](functions-host-json-v1.md)bakın.

Diğer işlev uygulaması yapılandırma seçenekleri [uygulama ayarlarınızda](functions-app-settings.md) (dağıtılan uygulamalar için) veya [local.settings.json](functions-run-local.md#local-settings-file) dosyanızda (yerel geliştirme için) yönetilir.

Bağlamalarla ilgili host.json'daki yapılandırmalar, işlev uygulamasındaki her işleve eşit olarak uygulanır. 

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Sürüm 2.x+ için aşağıdaki örnek *host.json* dosyası nda belirtilen tüm olası seçenekler vardır (yalnızca dahili kullanım için olanlar hariç).

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

Bu makalenin aşağıdaki bölümleri her üst düzey özelliği açıklar. Aksi belirtilmedikçe hepsi isteğe bağlıdır.

## <a name="aggregator"></a>Toplayıcı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>uygulamaInsights

Bu [ayar, günlüğe kaydetmenin](#logging)bir alt nedenidir.

[Örnekleme seçenekleri](./functions-monitoring.md#configure-sampling)de dahil olmak üzere Uygulama Öngörüleri seçeneklerini denetler.

Tam JSON yapısı için önceki [örnek host.json dosyasına](#sample-hostjson-file)bakın.

> [!NOTE]
> Günlük örneklemesi, bazı yürütmelerin Application Insights izleme bıçağında görünmemesi nedeniyle olabilir. Günlük örneklemesini önlemek `excludedTypes: "Request"` için `samplingSettings` değere ekleyin.

| Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| örneklemeAyarlar | yok | Bkz. [applicationInsights.samplingSettings](#applicationinsightssamplingsettings). |
| enableLiveMetrics | true | Canlı ölçümler insaedilmesini sağlar. |
| enableDependencyTracking | true | Bağımlılık izleme sağlar. |
| enablePerformanceCountersCollection | true | Kudu performans sayaçları toplamasağlar. |
| liveMetricsInitializationDelay | 00:00:15 | Yalnızca dahili kullanım içindir. |
| httpAutoCollectionOptions | yok | Bkz. [applicationInsights.httpAutoCollectionOptions](#applicationinsightshttpautocollectionoptions). |
| anlık Yapılandırma | yok | Bkz. [applicationInsights.snapshotConfiguration](#applicationinsightssnapshotconfiguration). |

### <a name="applicationinsightssamplingsettings"></a>applicationInsights.samplingAyarlar

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| ısenabled | true | Örneklemeyi etkinleştiriveya devre dışı kılabilir. | 
| maxTelemetryItemsPerSecond | 20 | Her sunucu ana bilgisayarında saniyede günlüğe kaydedilen telemetri öğelerinin hedef sayısı. Uygulamanız çok sayıda ana bilgisayarda çalışıyorsa, genel hedef trafik oranıiçinde kalmak için bu değeri azaltın. | 
| değerlendirmeInterval | 01:00:00 | Geçerli telemetri hızının yeniden değerlendirildiği aralık. Değerlendirme hareketli bir ortalama olarak gerçekleştirilir. Telemetriniz ani patlamalara karşı sorumluysa bu aralığı kısaltmak isteyebilirsiniz. |
| ilkÖrnekleme Yüzdesi| 1.0 | Örnekleme işleminin başlangıcında uygulanan ilk örnekleme yüzdesi, yüzdeyi dinamik olarak değiştirin. Hata ayıklarken değeri düşürmeyin. |
| örneklemePercentageIncreaseTimeout | 00:00:01 | Örnekleme yüzdesi değeri değiştiğinde, bu özellik, daha fazla veri yakalamak için Uygulama Öngörüleri'nin örnekleme yüzdesini yeniden artırmasına ne kadar kısa süre sonra izin verilebildiğini belirler. |
| örneklemePercentageDecreaseTimeout | 00:00:01 | Örnekleme yüzdesi değeri değiştiğinde, bu özellik daha az veri yakalamak için Uygulama Öngörüleri'nin örnekleme yüzdesini ne kadar kısa sürede düşürecek lerini belirler. |
| minSamplingYüzdesi | 0.1 | Örnekleme yüzdesi değiştiğinden, bu özellik izin verilen minimum örnekleme yüzdesini belirler. |
| maxSamplingYüzdesi | 0.1 | Örnekleme yüzdesi değiştiğinden, bu özellik izin verilen maksimum örnekleme yüzdesini belirler. |
| movingAverageRatio | 1.0 | Hareketli ortalamanın hesaplanmasında, en son değere atanan ağırlık. 1'e eşit veya daha az bir değer kullanın. Daha küçük değerler algoritmayı ani değişikliklere karşı daha az tepkiverir. |
| dışlanmışTürleri | null | Örneklenmeyi istemediğiniz türlerin yarı nokta noktalı sınırlı bir listesi. Tanınan türleri `Dependency`şunlardır: `PageView` `Request`, `Event` `Exception` `Trace`, , , , ve . Belirtilen türlerin tüm örnekleri aktarılır; belirtilmeyen türler örneklenir. |
| dahilTürleri | null | Örneklenmek istediğiniz türlerin yarı kolonlu sınırlı listesi; boş bir liste tüm türleri ima eder. Burada listelenen `excludedTypes` geçersiz kılma türlerinde listelenen tür. Tanınan türleri `Dependency`şunlardır: `PageView` `Request`, `Event` `Exception` `Trace`, , , , ve . Belirtilen türörnekleri örneklenir; belirtilmeyen veya ima edilen türler örnekleme yapılmadan iletilir. |

### <a name="applicationinsightshttpautocollectionoptions"></a>uygulamaInsights.httpAutoCollectionOptions

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| enableHttpTriggerExtendedInfoCollection | true | HTTP tetikleyicileri için genişletilmiş HTTP istek bilgilerini etkinleştirir veya devre dışı kılabilir: gelen istek korelasyon üstbilgileri, çoklu enstrümantasyon tuşları desteği, HTTP yöntemi, yol ve yanıt. |
| etkinleştirinW3CDistributedTracing | true | W3C dağıtılmış izleme protokolünün desteğini sağlar veya devre dışı eder (ve eski korelasyon şemasını açar). Doğruysa varsayılan `enableHttpTriggerExtendedInfoCollection` olarak etkinleştirilir. Yanlışsa, `enableHttpTriggerExtendedInfoCollection` bu bayrak yalnızca gelen istekler için değil, giden istekler için geçerlidir. |
| enableResponseHeaderInjection | true | Çok bileşenli korelasyon üstlelerinin yanıtlara yapılmasını sağlar veya devre dışı kılabilir. Enjeksiyonu etkinleştirmek, Uygulama Öngörüleri'nin birkaç enstrümantasyon anahtarının kullanıldığı zamana kadar bir Uygulama Haritası oluşturmasını sağlar. Doğruysa varsayılan `enableHttpTriggerExtendedInfoCollection` olarak etkinleştirilir. Bu ayar `enableHttpTriggerExtendedInfoCollection` yanlışsa geçerli değildir. |

### <a name="applicationinsightssnapshotconfiguration"></a>uygulamaInsights.snapshotConfiguration

Anlık görüntüler hakkında daha fazla bilgi için [,.NET uygulamalarındaki özel durumlar](/azure/azure-monitor/app/snapshot-debugger) ve [Uygulama Bilgileri Anlık Hata Ayıklama'yı etkinleştiren sorun giderme sorunları veya anlık görüntüleri görüntüleme](/azure/azure-monitor/app/snapshot-debugger-troubleshoot)hata ayıklama anlık görüntülerine bakın.

|Özellik | Varsayılan | Açıklama |
| --------- | --------- | --------- | 
| ajanEndpoint | null | Uygulama Öngörüleri Anlık Hata Ayıklama hizmetine bağlanmak için kullanılan bitiş noktası. Null ise, varsayılan bir uç noktası kullanılır. |
| yakalamaSnapshotMemoryWeight | 0,5 | Anlık görüntü almak için yeterli bellek olup olmadığını kontrol ederken geçerli işlem bellek boyutuna verilen ağırlık. Beklenen değer 0'dan büyük bir uygun kesir (0 < CaptureSnapshotMemoryWeight < 1). |
| failedRequestLimit | 3 | Telemetri işlemcisi devre dışı bırakılmadan önce anlık görüntü istemek için başarısız isteklerin sayısısınırı.|
| handleUntrackedExceptions | true | Application Insights telemetrisi tarafından izlenmeyen özel durumların izlenmesini sağlar veya devre dışı kılabilir. |
| ısenabled | true | Anlık görüntü koleksiyonunu etkinleştirir veya devre dışı kılabilir | 
| isEnabledInDeveloperMode | yanlış | Geliştirici modunda anlık görüntü koleksiyonunu etkinleştirer veya devre dışı kılabilir. |
| isEnabledWhenProfiling | true | Uygulama Öngörüleri Profil Oluşturucusu ayrıntılı bir profil oluşturma oturumu topluyor olsa bile anlık görüntü oluşturmayı etkinleştiriyor veya devre dışı kalıyor. |
| isExceptionSnappointsEtkin | yanlış | Özel durumların filtrelemesini sağlar veya devre dışı eder. |
| isLowPrioritySnapshotUploader | true | SnapshotUploader işlemini normal önceliğin altında çalıştırıp çalıştırmayacağını belirler. |
| maksimumCollectionPlanSize | 50 | 1 ile 9999 arasında bir aralıkta herhangi bir zamanda izleyebilirsiniz sorunların maksimum sayısı. |
| maximumSnapshotsGerekli | 3 | Tek bir sorun için toplanan en fazla anlık görüntü sayısı, bir ile 999 arasında bir aralıkta. Bir sorun, uygulamanızda tek tek bir atma deyimi olarak düşünülebilir. Bir sorun için toplanan anlık görüntü sayısı bu değere ulaştığında, sorun sayaçları sıfırlanana (bakınız) `problemCounterResetInterval`ve `thresholdForSnapshotting` sınıra yeniden ulaşılAna kadar bu sorun için başka anlık görüntü toplanmaz. |
| sorunCounterResetInterval | 24:00:00 | Ne sıklıkta bir dakika ile yedi gün arasında bir aralıkta sorun sayaçları sıfırlamak için. Bu aralıkulaşıldığında, tüm sorun sayıları sıfırlanır. Anlık görüntü yapmak için eşiğe zaten ulaşmış, ancak henüz anlık görüntü `maximumSnapshotsRequired`sayısını oluşturmuş olan varolan sorunlar etkin kalır. |
| AnonymousTelemetry sağlar | true | Microsoft'a anonim kullanım ve hata telemetrisi gönderip göndermeyeceklerini belirler. Bu telemetri, Anlık Görüntü Hata Ayıklama ile ilgili sorunları gidermeye yardımcı olmak için Microsoft'a başvurursanız kullanılabilir. Ayrıca kullanım desenleri izlemek için kullanılır. |
| yeniden bağlanmaInterval | 00:15:00 | Anlık Görüntü Hata Ayıklama bitiş noktasına ne sıklıkta yeniden bağlanıyoruz. İzin verilebilen aralık bir dakika ile bir gün arasındadır. |
| gölgeCopyFolder | null | Gölge kopyalama ikilileri için kullanılacak klasörü belirtir. Ayarlanmamışsa, aşağıdaki ortam değişkenleri tarafından belirtilen klasörler sırayla denenmektedir: Fabric_Folder_App_Temp, LOCALAPPDATA, APPDATA, TEMP. |
| shareUploaderProcess | true | Doğruysa, SnapshotUploader'ın yalnızca bir örneği InstrumentationKey'i paylaşan birden çok uygulama için anlık görüntü toplar ve yükler. Yanlış olarak ayarlanırsa, SnapshotUploader her (ProcessName, InstrumentationKey) tuple için benzersiz olacaktır. |
| snapshotInLowPriorityThread | true | Anlık görüntüleri düşük IO öncelikli iş parçacığında işleyip işlemeyeceklerini belirler. Anlık görüntü oluşturmak hızlı bir işlemdir, ancak Anlık Görüntü Hata Ayıklama hizmetine anlık görüntü yüklemek için önce diske minidökümü olarak yazılması gerekir. Bu SnapshotUploader sürecinde olur. Bu değeri gerçeğe ayarlamak, kaynak uygulamanızla rekabet etmeyecek olan mini dökümü yazmak için düşük öncelikli IO kullanır. Bu değeri yanlış olarak ayarlamak, uygulamanızı yavaşlatmak pahasına minidökümü oluşturmayı hızlandırAr. |
| snapshotsPerDayLimit | 30 | Bir günde (24 saat) izin verilen en fazla anlık görüntü sayısı. Bu sınır, Application Insights hizmet tarafında da uygulanır. Yüklemeler uygulama başına günlük 50 ile sınırlıdır (diğer bir şekilde enstrümantasyon anahtarı başına). Bu değer, yükleme sırasında reddedilecek ek anlık görüntüler oluşturulmasını önlemeye yardımcı olur. Sıfır değeri, önerilmez sınırı tamamen kaldırır. |
| snapshotsPerTenMinutesLimit | 1 | 10 dakika içinde izin verilen en fazla anlık görüntü sayısı. Bu değerüzerinde üst sınır olmamasına rağmen, uygulamanızın performansını etkileyebileceğinden, üretim iş yüklerinde bunu artırarak dikkatli olun. Anlık görüntü oluşturmak hızlıdır, ancak anlık görüntünün mini döküme'sini oluşturmak ve anlık görüntü hata ayıklama hizmetine yüklemek, kaynaklar için uygulamanızla (hem CPU hem de G)) rekabet edecek çok daha yavaş bir işlemdir. |
| tempFolder | null | Minidumps ve yükleyici günlük dosyaları yazmak için klasörbelirtir. Ayarlanmazise % *TEMP%\Dumps* kullanılır. |
| eşikForSnapshotting | 1 | Enstantane ler için sormadan önce Application Insights'ın bir özel durumu kaç kez görmesi gerekir? |
| uploaderProxy | null | Snapshot Uploader işleminde kullanılan proxy sunucusunu geçersiz kılar. Uygulamanız bir proxy sunucusu üzerinden internete bağlanırsa bu ayarı kullanmanız gerekebilir. Anlık Görüntü Toplayıcısı uygulamanızın işlemi içinde çalışır ve aynı proxy ayarlarını kullanır. Ancak, Snapshot Uploader ayrı bir işlem olarak çalışır ve proxy sunucusunu el ile yapılandırmanız gerekebilir. Bu değer null ise, daha sonra Snapshot Toplayıcı System.Net.WebRequest.DefaultWebProxy inceleyerek ve Snapshot Uploader değeri geçirerek proxy adresini otomatik algılamak için çalışacağız. Bu değer null değilse, otomatik algılama kullanılmaz ve burada belirtilen proxy sunucusu Snapshot Uploader kullanılacaktır. |

## <a name="cosmosdb"></a>cosmosDb

Yapılandırma ayarı [Cosmos DB tetikleyicileri ve bağlamaları](functions-bindings-cosmosdb-v2-output.md#host-json)bulunabilir.

## <a name="durabletask"></a>durableTask

Yapılandırma ayarı [Dayanıklı Fonksiyonlar için bağlamalarda](durable/durable-functions-bindings.md#host-json)bulunabilir.

## <a name="eventhub"></a>eventHub

Yapılandırma ayarları Event [Hub tetikleyicileri ve bağlamaları](functions-bindings-event-hubs-output.md#host-json)bulunabilir. 

## <a name="extensions"></a>Uzantı -ları

[http](#http) ve [eventHub](#eventhub)gibi bağlamaya özgü ayarların tümlerini içeren bir nesneyi döndüren özellik.

## <a name="extensionbundle"></a>uzantıBundle 

Uzantı paketleri, işlev uygulamanıza uyumlu bir Fonksiyon bağlama uzantıları kümesi eklemenize izin sağlar. Daha fazla bilgi için [yerel geliştirme için Uzantı paketlerine](functions-bindings-register.md#extension-bundles)bakın.

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="functions"></a> işlevleri

İş ana bilgisayarının çalıştırdığı işlevlerin listesi. Boş bir dizi tüm işlevleri çalıştırmak anlamına gelir. Yalnızca [yerel olarak çalışırken](functions-run-local.md)kullanılmak üzere tasarlanmıştır. Azure'daki işlev uygulamalarında, bunun yerine, bu ayarı kullanmak yerine belirli işlevleri devre dışı bırakarak [Azure İşlevlerinde işlevleri devre dışı bırakabilme](disable-function.md) adımlarını izlemeniz gerekir.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>fonksiyonTimeout

Tüm işlevler için zaman adabı süresini gösterir. Bu zaman pan dize biçimini izler. Sunucusuz Tüketim planında geçerli aralık 1 saniye ile 10 dakika arasındadır ve varsayılan değer 5 dakikadır.  

Premium planda geçerli aralık 1 saniye ile 60 dakika arasındadır ve varsayılan değer 30 dakikadır.

Özel (App Service) planında genel bir sınır yoktur ve varsayılan değer 30 dakikadır. Bir değer `-1` sınırsız yürütmeyi gösterir, ancak sabit bir üst sınır tutulması önerilir.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[Ana bilgisayar durumu monitörü](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)için yapılandırma ayarları.

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
|enabled|true|Özelliğin etkin olup olmadığını belirtir. | 
|sağlıkCheckInterval|10 saniye|Periyodik arka plan sağlık kontrolleri arasındaki zaman aralığı. | 
|sağlıkCheckWindow|2 dakika|`healthCheckThreshold` Ayarı ile birlikte kullanılan bir sürgülü zaman penceresi.| 
|sağlıkCheckThreshold|6|Ana bilgisayar geri dönüşümü başlatılmadan önce en fazla kaç kez sistem durumu denetimi başarısız olabilir.| 
|karşıEşik|0,80|Performans sayacının sağlıksız olarak kabul ediliş eşiği.| 

## <a name="http"></a>http

Yapılandırma ayarları http [tetikleyiciler ve bağlamalar](functions-bindings-http-webhook-output.md#hostjson-settings)bulunabilir.

## <a name="logging"></a>günlüğe kaydetme

Uygulama Öngörüleri de dahil olmak üzere işlev uygulamasının günlük davranışlarını denetler.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
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
|fileLoggingMode|hataAyıkSadece|Dosya günlüğe kaydetme düzeyinin etkinleştirilen düzeyini tanımlar.  Seçenekler `never`, `always` `debugOnly`, . |
|Loglevel|yok|Uygulamadaki işlevler için günlük kategorisi filtrelemesini tanımlayan nesne. Sürüm 2.x ve daha sonra günlük kategori filtreleme için ASP.NET Core düzenini izleyin. Bu ayar, belirli işlevler için günlüğe kaydetmeyi filtrelemenizi sağlar. Daha fazla bilgi için ASP.NET Core belgelerinde [Günlük filtreleme'ye](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) bakın. |
|console|yok| [Konsol](#console) günlüğe kaydetme ayarı. |
|uygulamaInsights|yok| [UygulamaInsights](#applicationinsights) ayarı. |

## <a name="console"></a>console

Bu [ayar, günlüğe kaydetmenin](#logging)bir alt nedenidir. Hata ayıklama modunda değilken konsol günlüğe kaydetmeyi denetler.

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
|ısenabled|yanlış|Konsol günlüğe kaydetmeyi etkinleştirer veya devre dışı kılabilir.| 

## <a name="manageddependency"></a>yönetilen Bağımlılık

Yönetilen bağımlılık şu anda yalnızca PowerShell tabanlı işlevlerle desteklenen bir özelliktir. Bağımlılıkların hizmet tarafından otomatik olarak yönetilmesini sağlar. `enabled` Özellik `true`ayarlandığında, `requirements.psd1` dosya işlenir. Bağımlılıklar, küçük sürümler yayımlandığında güncelleştirilir. Daha fazla bilgi için PowerShell makalesinde [Yönetilen bağımlılık'a](functions-reference-powershell.md#dependency-management) bakın.

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="queues"></a>Sıra

Yapılandırma ayarları Depolama [sıra tetikleyicileri ve bağlamaları](functions-bindings-storage-queue-output.md#host-json)bulunabilir.  

## <a name="sendgrid"></a>sendGrid

Yapılandırma ayarı [SendGrid tetikleyicileri ve bağlamaları](functions-bindings-sendgrid.md#host-json)bulunabilir.

## <a name="servicebus"></a>serviceBus

Yapılandırma ayarı [Servis Veri Servisi tetikleyicileri ve bağlamaları](functions-bindings-service-bus-output.md#host-json)bulunabilir.

## <a name="singleton"></a>Singleton

Singleton kilit davranışı için yapılandırma ayarları. Daha fazla bilgi için [singleton desteği yle ilgili GitHub sorununa](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)bakın.

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
|kilitDönem|00:00:15|İşlev düzeyi kilitlerinin alındığı dönem. Kilitler otomatik yenileme.| 
|dinleyiciLockPeriod|00:01:00|Dinleyici kilitlerinin alındığı süre.| 
|dinleyiciLockRecoveryPollingInterval|00:01:00|Dinleyici kilidi başlangıçta elde edilemeseydi dinleyici kilidi kurtarma için kullanılan zaman aralığı.| 
|lockAcquisitionTimeout|00:01:00|Çalışma süresinin kilit elde etmeye çalışacağı maksimum süre.| 
|lockAcquisitionPollingInterval|yok|Kilit edinme denemeleri arasındaki aralık.| 

## <a name="version"></a>version

Bu değer, host.json'un şema sürümünü gösterir. Sürüm dizesi, `"version": "2.0"` v2 çalışma süresini veya daha sonraki bir sürümü hedefleyen bir işlev uygulaması için gereklidir. V2 ve v3 arasında host.json şeması değişikliği yoktur.

## <a name="watchdirectories"></a>watchDirectories

Değişiklikler için izlenmesi gereken [paylaşılan kod dizinleri](functions-reference-csharp.md#watched-directories) kümesi.  Bu dizinlerde kod değiştirildiğinde, değişikliklerin işlevleriniz tarafından alınmasını sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [host.json dosyasını nasıl güncelleştirin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ortam değişkenlerinde genel ayarları görün](functions-app-settings.md)
