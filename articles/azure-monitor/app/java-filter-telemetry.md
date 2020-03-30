---
title: Java web uygulamanızda Azure Uygulama Öngörüleri telemetrisini filtreleyin
description: İzlemeniz gerekmeyen olayları filtreleyerek telemetri trafiğini azaltın.
ms.topic: conceptual
ms.date: 3/14/2019
ms.openlocfilehash: 020e54132e0ca0a9f9ccf0236f94515877015637
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659926"
---
# <a name="filter-telemetry-in-your-java-web-app"></a>Java web uygulamanızda telemetriyi filtreleyin

Filtreler, [Java web uygulamanızın Application Insights'a gönderdiği](java-get-started.md)telemetriyi seçmenin bir yolunu sağlar. Kullanabileceğiniz kullanıma açbazı filtreler vardır ve kendi özel filtrelerinizi de yazabilirsiniz.

Kutudan çıkan filtreler şunlardır:

* Önem düzeyini izleme
* Belirli URL'ler, anahtar kelimeler veya yanıt kodları
* Hızlı yanıtlar - diğer bir şekilde uygulamanızın hızlı bir şekilde yanıt verdiği istekler
* Belirli olay adları

> [!NOTE]
> Filtreler uygulamanızın ölçümlerini çarpıtıyor. Örneğin, yavaş yanıtları tanılamak için hızlı yanıt sürelerini atacak bir filtre ayarladığınıza karar verebilirsiniz. Ancak, Application Insights tarafından bildirilen ortalama yanıt sürelerinin gerçek hızdan daha yavaş olacağını ve istek sayısının gerçek sayıdan daha küçük olacağını unutmayın.
> Bu bir sorunsa, bunun yerine [Örnekleme'yi](../../azure-monitor/app/sampling.md) kullanın.

## <a name="setting-filters"></a>Filtreleri ayarlama

ApplicationInsights.xml'de şu `TelemetryProcessors` örneğe benzer bir bölüm ekleyin:


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want to see -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




[Dahili işlemcilerin tam setini inceleyin.](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor)

## <a name="built-in-filters"></a>Yerleşik filtreler

### <a name="metric-telemetry-filter"></a>Metrik Telemetri filtresi

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded`- Özel metrik adların virgülle ayrılmış listesi.


### <a name="page-view-telemetry-filter"></a>Sayfa Görünümü Telemetri filtresi

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS`- Süre, sayfanın yüklenmesi için alınan süreyi ifade eder. Bu ayarlanırsa, bu süreden daha hızlı yüklenen sayfalar bildirilmemiştir.
* `NotNeededNames`- Sayfa adlarının virgülle ayrılmış listesi.
* `NotNeededUrls`- URL parçalarının virgülle ayrılmış listesi. Örneğin, `"home"` URL'de "ev" bulunan tüm sayfaları filtreler.


### <a name="request-telemetry-filter"></a>İstek Telemetri Filtresi


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a>Sentetik Kaynak filtresi

SyntheticSource özelliğinde değerlere sahip tüm telemetrileri filtreler. Bunlar botlar, örümcekler ve kullanılabilirlik testleri isteklerini içerir.

Tüm sentetik istekler için telemetriyi filtreleyin:


```XML

           <Processor type="SyntheticSourceFilter" />
```

Belirli sentetik kaynaklar için telemetrifiltre:


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* `NotNeeded`- Sentetik kaynak isimlerinin virgülle ayrılmış listesi.

### <a name="telemetry-event-filter"></a>Telemetri Olay filtresi

Özel olayları filtreler [(TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)kullanılarak günlüğe kaydedilir).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames`- Etkinlik adlarının virgülle ayrılmış listesi.


### <a name="trace-telemetry-filter"></a>Telemetri filtresini izleme

Günlük izlemelerini filtreler [(TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) veya [günlük çerçeve toplayıcısı](java-trace-logs.md)kullanılarak günlüğe kaydedilir).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel`geçerli değerler şunlardır:
  *  KAPALI - TÜM izleri filtreleyin
  *  TRACE - Filtreleme yok. Trace düzeyine eşittir
  *  BİlGİ - TRACE düzeyini filtreleyin
  *  WARN - TRACE ve INFO filtreleme
  *  HATA - Filtre dışarı UYAR, BİlGİ, TRACE
  *  ELEŞTİrİ - tüm ama KRITIK filtre


## <a name="custom-filters"></a>Özel filtreler

### <a name="1-code-your-filter"></a>1. Filtrenizi kodlayın

Kodunuzda, şunları uygulayan bir `TelemetryProcessor`sınıf oluşturun:

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-the-configuration-file"></a>2. Yapılandırma dosyasındaki filtrenizi çağırma

In ApplicationInsights.xml:

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

### <a name="3-invoke-your-filter-java-spring"></a>3. Filtrenizi çağırın (Java Spring)

Yay çerçevesine dayalı uygulamalar için, özel telemetri işlemcilerinin ana uygulama sınıfınıza fasulye olarak kaydedilmesi gerekir. Uygulama başladığında otomatik olarak döşenecektir.

```Java
@Bean
public TelemetryProcessor successFilter() {
      return new SuccessFilter();
}
```

Bu parametreleri özel filtrenize `application.properties` aktarmak için kendi filtre parametrelerinizi oluşturmanız ve Bahar Önyükleme'nin dışlanmış yapılandırma çerçevesini oluşturmanız gerekir. 


## <a name="troubleshooting"></a>Sorun giderme

*Filtrem çalışmıyor.*

* Geçerli parametre değerleri sağladığınızı denetleyin. Örneğin, süreler arasayılar olmalıdır. Geçersiz değerler filtrenin yoksayılmasına neden olur. Özel filtreniz bir oluşturucu veya ayaryönteminden bir özel durum atarsa, bu durum yoksayılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md) - Örneklemeyi, ölçümlerinizi çarpıtmayan bir alternatif olarak düşünün.
