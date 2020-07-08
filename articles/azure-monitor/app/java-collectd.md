---
title: Linux 'ta Java Web uygulaması performansını izleme-Azure | Microsoft Docs
description: Application Insights için CollectD eklentisi ile Java Web sitenizin genişletilmiş uygulama performansı izlemesi.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 62a723dad7e9f6c2bfdabde159968d507d2d5d41
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81537534"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: Application Insights Linux performans ölçümleri


[Application Insights](../../azure-monitor/app/app-insights-overview.md)'deki Linux sistem performans ölçümlerini araştırmak için, Application Insights eklentisiyle [toplanan](https://collectd.org/)'yi birlikte yüklemelisiniz. Bu açık kaynaklı çözüm çeşitli sistem ve ağ istatistiklerini toplar.

Genellikle, [Java Web hizmetinizi Application Insights][java]zaten belirlediyseniz toplanan kullanırsınız. Uygulamanızın performansını geliştirmenize veya sorunları tanılamanıza yardımcı olacak daha fazla veri sunar. 

## <a name="get-your-instrumentation-key"></a>İzleme anahtarınızı alın
[Microsoft Azure Portal](https://portal.azure.com), verilerin görünmesini istediğiniz [Application Insights](../../azure-monitor/app/app-insights-overview.md) kaynağını açın. (Veya [Yeni bir kaynak oluşturun](../../azure-monitor/app/create-new-resource.md ).)

Kaynak tanımlayan izleme anahtarının bir kopyasını alın.

![Tümüne göz atın, kaynağınızı açın ve temel bileşenler açılan penceresinde, Izleme anahtarını seçin ve kopyalayın](./media/java-collectd/instrumentation-key-001.png)

## <a name="install-collectd-and-the-plug-in"></a>Toplanan ve eklentiyi yükler
Linux sunucu makinelerinizde:

1. [Toplanan](https://collectd.org/) sürüm 5.4.0 veya üstünü yükler.
2. [Application Insights toplanan yazıcı eklentisini](https://github.com/microsoft/ApplicationInsights-Java/tree/master/collectd/src/main/java/com/microsoft/applicationinsights/collectd/internal)indirin. Sürüm numarasını aklınızda yapın.
3. Eklenti JAR dosyasını içine kopyalayın `/usr/share/collectd/java` .
4. Düzenle `/etc/collectd/collectd.conf` :
   * [Java eklentisinin](https://collectd.org/wiki/index.php/Plugin:Java) etkinleştirildiğinden emin olun.
   * Aşağıdaki JAR 'yi dahil etmek için Java. Class. Path için JVMArg 'yi güncelleştirin. Sürüm numarasını indirdiğiniz bir sürümle eşleşecek şekilde güncelleştirin:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Bu kod parçacığını, kaynağınızın Izleme anahtarını kullanarak ekleyin:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Örnek yapılandırma dosyasının bir parçası aşağıda verilmiştir:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Farklı kaynaklardan çeşitli verileri toplayabilen diğer [toplanan eklentilerini](https://collectd.org/wiki/index.php/Table_of_Plugins)yapılandırın.

[El ile](https://collectd.org/wiki/index.php/First_steps)için toplanan 'yi yeniden başlatın.

## <a name="view-the-data-in-application-insights"></a>Application Insights verileri görüntüleme
Application Insights kaynağınız içinde, ölçümler ' i açın [ve grafikler ekleyin ve][metrics]özel kategoriden görmek istediğiniz ölçümleri seçin.

Varsayılan olarak ölçümler, ölçümlerin toplandığı tüm ana makineler arasında toplanır. Konak başına ölçümleri görüntülemek için, grafik ayrıntıları dikey penceresinde gruplamayı açın ve ardından CollectD-Host ' a göre gruplandırmayı seçin.

## <a name="to-exclude-upload-of-specific-statistics"></a>Belirli istatistiklerin karşıya yüklenmesini dışlamak için
Varsayılan olarak, Application Insights eklentisi tüm etkin toplanan ' okuma ' eklentileri tarafından toplanan tüm verileri gönderir. 

Belirli eklentilerden veya veri kaynaklarından veri dışlamak için:

* Yapılandırma dosyasını düzenleyin. 
* İçinde `<Plugin ApplicationInsightsWriter>` , aşağıdaki gibi yönerge satırları ekleyin:

| Deki | Efekt |
| --- | --- |
| `Exclude disk` |Eklenti tarafından toplanan tüm verileri Dışla `disk` |
| `Exclude disk:read,write` |Ve adlı kaynakları eklentiden hariç tutun `read` `write` `disk` . |

Yönergeleri bir yeni satır ile ayırın.

## <a name="problems"></a>Sorunlarınız mı var?
*Portalda veri görmüyorum*

* Ham olayların ulaşıp ulaşmadığını görmek için [arama][diagnostic] ' yı açın. Bazen Ölçüm Gezgini 'nde görünmesi daha uzun sürer.
* [Giden veriler için güvenlik duvarı özel durumları ayarlamanız](../../azure-monitor/app/ip-addresses.md) gerekebilir
* Application Insights eklentisinde izlemeyi etkinleştirin. Bu satırı içine ekleyin `<Plugin ApplicationInsightsWriter>` :
  * `SDKLogger true`
* Raporlama yaptığı sorunları görmek için bir Terminal açın ve toplanan 'yi ayrıntılı modda başlatın:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bilinen sorun

Application Insights yazma eklentisi, belirli okuma eklentileri ile uyumsuzdur. Bazı Eklentiler bazen Application Insights eklentisinin kayan noktalı bir sayı beklediği "NaN" değerini gönderir.

Belirti: toplanan günlüğünde "AI:... SyntaxError: beklenmeyen belirteç yok.

Geçici çözüm: sorun yazma eklentileri tarafından toplanan verileri dışlayın. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md


