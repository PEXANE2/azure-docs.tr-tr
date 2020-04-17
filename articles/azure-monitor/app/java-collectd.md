---
title: Linux'ta Java web uygulaması performansını izleyin - Azure | Microsoft Dokümanlar
description: Uygulama Öngörüleri için CollectD eklentisi ile Java web sitenizin genişletilmiş uygulama performansı izleme.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 62a723dad7e9f6c2bfdabde159968d507d2d5d41
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537534"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>toplanan: Application Insights Linux performans ölçümleri


[Application Insights'ta](../../azure-monitor/app/app-insights-overview.md)Linux sistem performans ölçümlerini keşfetmek için, Application Insights eklentisi ile birlikte [toplanan](https://collectd.org/)yüklemeyi yükler. Bu açık kaynak çözümü çeşitli sistem ve ağ istatistikleri toplar.

Java [web hizmetinizi Uygulama Öngörüleri ile zaten enstrümante][java]kullandıysanız, genellikle toplananı kullanırsınız. Uygulamanızın performansını artırmanıza veya sorunları tanılamanıza yardımcı olacak daha fazla veri sağlar. 

## <a name="get-your-instrumentation-key"></a>Enstrümantasyon anahtarınızı alın
Microsoft [Azure portalında,](https://portal.azure.com)verilerin görünmesini istediğiniz [Uygulama Öngörüleri](../../azure-monitor/app/app-insights-overview.md) kaynağını açın. (Veya [yeni bir kaynak oluşturun.)](../../azure-monitor/app/create-new-resource.md )

Kaynağı tanımlayan enstrümantasyon anahtarının bir kopyasını alın.

![Tümüne göz atın, kaynağınızı açın ve ardından Essentials açılır açılır, seçin ve Enstrümantasyon Anahtarı'nı kopyalayın](./media/java-collectd/instrumentation-key-001.png)

## <a name="install-collectd-and-the-plug-in"></a>Toplanan ve eklentisini yükleyin
Linux sunucu makinelerinizde:

1. [Toplanan](https://collectd.org/) sürüm 5.4.0 veya daha sonra yükleyin.
2. Uygulama [Insights toplanan yazar eklentisi](https://github.com/microsoft/ApplicationInsights-Java/tree/master/collectd/src/main/java/com/microsoft/applicationinsights/collectd/internal)indirin. Sürüm numarasına dikkat edin.
3. Eklenti JAR'ı `/usr/share/collectd/java`kopyala.
4. Edit `/etc/collectd/collectd.conf`:
   * Java [eklentisinin](https://collectd.org/wiki/index.php/Plugin:Java) etkin olduğundan emin olun.
   * Java.class.path için JVMArg'ı aşağıdaki JAR'ı içerecek şekilde güncelleştirin. İndirdiğiniz sürüm numarasıyla eşleşecek şekilde güncelleştirin:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Kaynağınızdaki Enstrümantasyon Anahtarı'nı kullanarak bu parçacığı ekleyin:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Örnek yapılandırma dosyasının bir bölümü aşağıda vereyim:

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

Farklı kaynaklardan çeşitli veriler toplayabilen diğer [toplanan eklentileri](https://collectd.org/wiki/index.php/Table_of_Plugins)yapılandırın.

Yeniden başlat [ın kılavuzuna](https://collectd.org/wiki/index.php/First_steps)göre toplanır.

## <a name="view-the-data-in-application-insights"></a>Uygulama Öngörüleri'ndeki verileri görüntüleme
Uygulama Öngörüleri kaynağınızda, Özel kategorisinden görmek istediğiniz ölçümleri seçerek Ölçümleri açın [ve grafikler ekleyin.][metrics]

Varsayılan olarak, ölçümler, ölçümlerin toplandığı tüm ana bilgisayar makinelerinde toplanır. Ana bilgisayar başına ölçümleri görüntülemek için Grafik ayrıntıları bıçaklarında Gruplandırma'yı açın ve ardından CollectD-Host'a göre gruplandırmayı seçin.

## <a name="to-exclude-upload-of-specific-statistics"></a>Belirli istatistiklerin yüklenmesini hariç tutmak için
Varsayılan olarak, Application Insights eklentisi, etkin toplanan tüm 'okuma' eklentileri tarafından toplanan tüm verileri gönderir. 

Belirli eklentilerden veya veri kaynaklarından verileri hariç tutmak için:

* Yapılandırma dosyasını düzenleme. 
* In `<Plugin ApplicationInsightsWriter>`, bu gibi yönerge satırları ekleyin:

| Yönergesi | Etki |
| --- | --- |
| `Exclude disk` |`disk` Eklenti tarafından toplanan tüm verileri hariç tutma |
| `Exclude disk:read,write` |Adı geçen `read` kaynakları `write` ve `disk` eklentiden hariç tut. |

Yeni bir satırla ayrı yönergeler.

## <a name="problems"></a>Sorunlarınız mı var?
*Portalda veri göremiyorum.*

* Ham olayların gelip gelmediğini görmek için [Arama'yı][diagnostic] açın. Bazen ölçümler explorer görünmesi daha uzun sürer.
* Giden veriler [için güvenlik duvarı özel durumları ayarlamanız](../../azure-monitor/app/ip-addresses.md) gerekebilir
* Application Insights eklentisinde izleme yi etkinleştirin. Bu satırı `<Plugin ApplicationInsightsWriter>`ekleyin:
  * `SDKLogger true`
* Bildirdiği sorunları görmek için bir terminal açın ve ayrıntılı modda toplamaya başlayın:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bilinen sorun

Uygulama Öngörüleri Yazma eklentisi belirli Okuma eklentileriyle uyumsuzdur. Bazı eklentiler bazen Application Insights eklentisinin kayan nokta numarası beklediği "NaN"ı gönderir.

Belirti: Toplanan günlük "AI: ... SözdizimiHatası: Beklenmeyen belirteç N".

Geçici Çözüm: Sorun Tarafından toplanan verileri hariç tut Eklentileri Yazın. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md


