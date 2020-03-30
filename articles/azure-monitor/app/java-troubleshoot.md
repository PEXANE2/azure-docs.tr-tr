---
title: Java web projesinde Uygulama Öngörüleri Sorun Giderme
description: Sorun giderme kılavuzu - Application Insights ile canlı Java uygulamalarını izleme.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 04e98938bc5dd17816ae873f122073212275a414
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657189"
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Java için Application Insights Sorun Giderme, Soru ve Yanıt
[Java'daki Azure Uygulama Öngörüleri][java]ile ilgili sorularınız veya sorunlar? Aşağıda birkaç ipucu verilmiştir.

## <a name="build-errors"></a>Yapı hataları
**Eclipse veya Intellij Idea, Maven veya Gradle üzerinden Uygulama Insights SDK eklerken, ben yapı veya checksum doğrulama hataları olsun.**

* Bağımlılık `<version>` öğesi joker karakter (örneğin (Örneğin(Maven) `<version>[2.0,)</version>` veya (Gradle) `version:'2.0.+'`içeren bir desen kullanıyorsa, bunun `2.0.1`yerine belirli bir sürümü belirtmeyi deneyin. En son sürümün [sürüm notlarına](https://github.com/Microsoft/ApplicationInsights-Java/releases) bakın.

## <a name="no-data"></a>Veri yok
**Application Insights'ı başarıyla ekledim ve uygulamamı çalıştırdım, ancak portalda hiç veri görmedim.**

* Bir dakika bekleyin ve Yenile'yi tıklatın. Grafikler kendilerini düzenli aralıklarla yeniler, ancak el ile de yenileyebilirsiniz. Yenileme aralığı grafiğin zaman aralığına bağlıdır.
* ApplicationInsights.xml dosyasında (projenizdeki kaynaklar klasöründe) tanımlanan veya Çevre değişkeni olarak yapılandırılan bir enstrümantasyon anahtarınız olup olmadığını denetleyin.
* xml dosyasında `<DisableTelemetry>true</DisableTelemetry>` düğüm olmadığını doğrulayın.
* Güvenlik duvarınızda, giden trafiğin dc.services.visualstudio.com için TCP bağlantı noktalarını 80 ve 443'e açmanız gerekebilir. Güvenlik [duvarı özel durumlarının tam listesine](../../azure-monitor/app/ip-addresses.md) bakın
* Microsoft Azure başlangıç panosunda hizmet durumu haritasına bakın. Bazı uyarı göstergeleri varsa, tamam'a dönene kadar bekleyin ve ardından Application Insights uygulama bıçaklarınızı kapatıp yeniden açın.
* ApplicationInsights.xml `<SDKLogger />` dosyasındaki kök düğümünün altına (projenizdeki kaynaklar klasöründe) bir öğe ekleyerek [günlüğe kaydetmeyi açın](#debug-data-from-the-sdk) ve şüpheli günlükler için AI: INFO/WARN/ERROR ile önceden yapılan girişleri denetleyin. 
* Doğru ApplicationInsights.xml dosyasının Java SDK tarafından başarıyla yüklendiğinden emin olun, konsolun çıkış iletilerine bakarak "Yapılandırma dosyası başarıyla bulundu" ifadesi ne kadar önemli.
* Config dosyası bulunamazsa, config dosyasının nerede arandığını görmek için çıktı iletilerini denetleyin ve ApplicationInsights.xml'in bu arama konumlarından birinde bulunduğundan emin olun. Kural olarak config dosyasını Application Insights SDK JARs'in yakınına yerleştirebilirsiniz. Örneğin: Tomcat'ta bu, WEB-INF/classes klasörü anlamına gelir. Geliştirme sırasında ApplicationInsights.xml'i web projenizin kaynaklar klasörüne yerleştirebilirsiniz.
* Ayrıca SDK ile bilinen sorunlar için [GitHub sorunları sayfasına](https://github.com/Microsoft/ApplicationInsights-Java/issues) bakın.
* Lütfen herhangi bir sürüm çakışması sorunları önlemek için Uygulama Öngörüleri çekirdek, web, aracı ve günlük appenders aynı sürümünü kullandığınızdan emin olun.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Eskiden verileri görürdüm, ama durdu.
* Durum [blogunu](https://blogs.msdn.com/b/applicationinsights-status/)kontrol edin.
* Aylık veri puanı kotanıza ulaştınız mı? Öğrenmek için Ayarlar/Kota ve Fiyatlandırma'yı açın. Bu nedenle, planınızı yükseltebilir veya ek kapasite için ödeme yapabilirsiniz. Fiyatlandırma [şemasına](https://azure.microsoft.com/pricing/details/application-insights/)bakın.
* Son zamanlarda SDK yükseltilmiş mi? Lütfen proje dizininde yalnızca Benzersiz SDK kavanozlarının bulunduğundan emin olun. SDK'nın iki farklı versiyonu bulunmamalıdır.
* Doğru ai kaynağına mı bakıyorsun? Lütfen uygulamanızın iKey'ini telemetri beklediğiniz kaynakla eşleştirin. Aynı olmalılar.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Beklediğim tüm verileri göremiyorum.
* Kullanım ve tahmini maliyet sayfasını açın ve [örneklemenin](../../azure-monitor/app/sampling.md) çalışıp çalışmadığını kontrol edin. (%100 iletim, örneklemenin çalışmadığı anlamına gelir.) Application Insights hizmeti, uygulamanızdan gelen telemetrinin yalnızca bir kısmını kabul etmek üzere ayarlanabilir. Bu, aylık telemetri kotanızı korumanıza yardımcı olur.
* SDK Örneklemesi açık mı? Evet ise, veriler tüm geçerli türler için belirtilen oranda örneklenir.
* Java SDK'nın eski bir sürümünü çalıştırıyor musunuz? Sürüm 2.0.1 ile başlayarak, yerel sürücülerde veri kalıcılığının yanı sıra aralıklı ağ ve arka uç arızalarını işlemek için hata tolerans mekanizması nı kullanıma sunduk.
* Aşırı telemetri yüzünden mi daralıyorsun? INFO günlüğünü açarsanız, "Uygulama daraltılmış" bir günlük iletisi görürsünüz. Mevcut limitimiz 32k telemetri öğeleri/saniyedir.

### <a name="java-agent-cannot-capture-dependency-data"></a>Java Aracısı bağımlılık verilerini yakalayamaz
* [Java Agent'ı Yapılandırma'yı](java-agent.md) takip ederek Java aracısı yapılandırdın mı?
* Hem java aracısı kavanozu hem de AI-Agent.xml dosyasının aynı klasöre yerleştirildiğinden emin olun.
* Otomatik toplamaya çalıştığınız bağımlılığın otomatik toplama için desteklendirdiğinden emin olun. Şu anda yalnızca Redis bağımlılık koleksiyonu için MySQL, MsSQL, Oracle DB ve Azure Önbelleği'ni destekliyoruz.

## <a name="no-usage-data"></a>Kullanım verisi yok
**İstekler ve yanıt süreleri ile ilgili verileri görüyorum, ancak sayfa görünümü, tarayıcı veya kullanıcı verileri yok.**

Uygulamanızı sunucudan telemetri göndermek için başarıyla ayarlarsınız. Şimdi bir sonraki adım [web tarayıcısından telemetri göndermek için web sayfalarını kurmaktır.][usage]

Alternatif olarak, istemciniz bir telefon veya başka bir [cihazda][platforms]bir uygulamaise, oradan telemetri gönderebilirsiniz.

Hem istemcinizi hem de sunucu telemetrinizi ayarlamak için aynı enstrümantasyon anahtarını kullanın. Veriler aynı Application Insights kaynağında görünür ve istemci ve sunucudaki olayları ilişkilendirebilirsiniz.


## <a name="disabling-telemetry"></a>Telemetrinin devre dışı bırakılması
**Telemetri koleksiyonunu nasıl devre dışı dışı bebilirim?**

Kod:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Veya**

ApplicationInsights.xml'i güncelleştirin (projenizdeki kaynaklar klasöründe). Kök düğümünün altına aşağıdakileri ekleyin:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

XML yöntemini kullanarak, değeri değiştirdiğinizde uygulamayı yeniden başlatmanız gerekir.

## <a name="changing-the-target"></a>Hedefi değiştirme
**Projemin veri gönderdiği Azure kaynağını nasıl değiştirebilirim?**

* [Yeni kaynağın enstrümantasyon anahtarını alın.][java]
* Eclipse için Azure Araç Seti'ni kullanarak projenize Uygulama Öngörüleri eklediyseniz, web projenizi sağ tıklatın, **Azure**, **Uygulama Öngörülerini Yapılandır'ı**seçin ve anahtarı değiştirin.
* Enstrümantasyon Anahtarını ortam değişkeni olarak yapılandırmışsanız, lütfen ortam değişkeninin değerini yeni iKey ile güncelleyin.
* Aksi takdirde, Project'inizdeki kaynaklar klasöründeki ApplicationInsights.xml'deki anahtarı güncelleştirin.

## <a name="debug-data-from-the-sdk"></a>SDK'dan veri ayıklama

**SDK'nın ne yaptığını nasıl öğrenebilirim?**

API'de neler olduğu hakkında daha fazla `<SDKLogger/>` bilgi edinmek için ApplicationInsights.xml yapılandırma dosyasının kök düğümünün altına ekleyin.

### <a name="applicationinsightsxml"></a>UygulamaInsights.xml

Kaydediciyi bir dosyaya çıktı için de talimat verebilirsiniz:

```XML
  <SDKLogger type="FILE"><!-- or "CONSOLE" to print to stderr -->
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AISDK</BaseFolderPath>
</SDKLogger>
```

### <a name="spring-boot-starter"></a>Bahar Boot Starter

Application Insights Bahar Önyükleme Başlatıcısı'nı kullanarak SDK'nın `application.properties` Bahar Önyükleme Uygulamaları ile oturum açmasını etkinleştirmek için dosyaya aşağıdakileri ekleyin:

```yaml
azure.application-insights.logger.type=file
azure.application-insights.logger.base-folder-path=C:/agent/AISDK
azure.application-insights.logger.level=trace
```

veya standart hataya yazdırmak için:

```yaml
azure.application-insights.logger.type=console
azure.application-insights.logger.level=trace
```

### <a name="java-agent"></a>Java Aracısı

JVM Aracısı Günlük [Etkinleştirmek için AI-Agent.xml dosyasını](java-agent.md)güncelleyin:

```xml
<AgentLogger type="FILE"><!-- or "CONSOLE" to print to stderr -->
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AIAGENT</BaseFolderPath>
</AgentLogger>
```

### <a name="java-command-line-properties"></a>Java Komut Satırı Özellikleri
_Sürüm 2.4.0'dan beri_

Yapılandırma dosyalarını değiştirmeden komut satırı seçeneklerini kullanarak günlüğe kaydetmeyi etkinleştirmek için:

```
java -Dapplicationinsights.logger.file.level=trace -Dapplicationinsights.logger.file.uniquePrefix=AI -Dapplicationinsights.logger.baseFolderPath="C:/my/log/dir" -jar MyApp.jar
```

veya standart hataya yazdırmak için:

```
java -Dapplicationinsights.logger.console.level=trace -jar MyApp.jar
```

## <a name="the-azure-start-screen"></a>Azure başlangıç ekranı
**[Azure portalına](https://portal.azure.com)bakıyorum. Harita bana uygulamamla ilgili bir şey söylüyor mu?**

Hayır, tüm dünyadaki Azure sunucularının durumunu gösterir.

*Azure başlangıç panosundan (ana ekran) uygulamamla ilgili verileri nasıl bulabilirim?*

Uygulama [Öngörüleri için uygulamanızı ayarladığınızı][java]varsayarsak, Gözat'ı tıklatın, Uygulama Öngörüleri'ni seçin ve uygulamanız için oluşturduğunuz uygulama kaynağını seçin. Gelecekte oraya daha hızlı ulaşmak için uygulamanızı başlangıç panosuna sabitleyebilirsiniz.

## <a name="intranet-servers"></a>İntranet sunucuları
**Intranetimdeki bir sunucuya göz atabilir miyim?**

Evet, sunucunuzun telemetriyi genel internet üzerinden Application Insights portalına gönderebildiği koşuluyla.

Güvenlik duvarınızda, dc.services.visualstudio.com giden trafik için TCP bağlantı noktalarını 80 ve 443'dc.services.visualstudio.com açmanız gerekebilir ve f5.services.visualstudio.com.

## <a name="data-retention"></a>Veri saklama
**Veriler portalda ne kadar süreyle saklanır? Güvenli mi?**

Bkz. [Veri saklama ve gizlilik.][data]

## <a name="debug-logging"></a>Hata ayıklama günlüğü
Uygulama Öngörüleri `org.apache.http`kullanır. Bu ad alanı `com.microsoft.applicationinsights.core.dependencies.http`altında Application Insights çekirdek kavanoz içinde taşınır. Bu, Uygulama Öngörüleri'nin aynı `org.apache.http` nın farklı sürümlerinin tek bir kod tabanında bulunduğu senaryoları işlemesini sağlar.

>[!NOTE]
>Uygulamadaki tüm ad alanları için HATA Ayıklama düzeyi günlüğe kaydetmeyi etkinleştiriyorsanız, `org.apache.http` bu `com.microsoft.applicationinsights.core.dependencies.http`işlem yeniden adlandırılmış dahil olmak üzere tüm çalıştıran modüller tarafından onurlandırılacaktır. Günlük araması Apache kitaplığı tarafından yapıldığından, Uygulama Öngörüleri bu aramalar için filtreleme uygulayamaz. Hata ayıklama düzeyi günlüğü önemli miktarda günlük verisi üretir ve canlı üretim örnekleri için önerilmez.


## <a name="next-steps"></a>Sonraki adımlar
**Java sunucu uygulamam için Application Insights'ı kurdum. Başka ne yapabilirim ki?**

* [Web sayfalarınızın kullanılabilirliğini izleme][availability]
* [Web sayfası kullanımını izleme][usage]
* [Cihazınızdaki uygulamalarınızdaki kullanımı izleme ve tanılama sorunları][platforms]
* [Uygulamanızın kullanımını izlemek için kod yazın][track]
* [Tanılama günlüklerini yakalama][javalogs]

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)
* [GitHub'da sorun dosyala](https://github.com/Microsoft/ApplicationInsights-Java/issues)

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[data]: ../../azure-monitor/app/data-retention-privacy.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[platforms]: ../../azure-monitor/app/platforms.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
[usage]: javascript.md

