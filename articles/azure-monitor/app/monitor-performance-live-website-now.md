---
title: Azure Application Insights ile canlı bir ASP.NET web uygulamasını izleme | Microsoft Docs
description: Yeniden dağıtmadan web sitesinin performansını izleme. Şirket içinde veya VM'lerde barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: ba17ee275a744b88f2c76e7e3f99a1ac9cc8e758
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536837"
---
# <a name="instrument-web-apps-at-runtime-with-application-insights-codeless-attach"></a>Application Insights Codeless Attach ile çalışma zamanında enstrüman web uygulamaları

> [!IMPORTANT]
> Durum İzleyicisi artık kullanım için önerilmez. Azure Monitör Uygulama Öngörüleri Aracısı (eski adıyla Status Monitor v2) tarafından değiştirildi. Şirket içi [sunucu dağıtımları](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) veya [Azure sanal makine ve sanal makine ölçeği ayarlanmış dağıtımlar](https://docs.microsoft.com/azure/azure-monitor/app/azure-vm-vmss-apps)için belgelerimize bakın.

Kodunuzu değiştirmeye veya yeniden dağıtmaya gerek olmadan canlı bir web uygulamasını Azure Application Insights ile izleyebilirsiniz. [Microsoft Azure](https://azure.com) aboneliğiniz olması gerekir.

Durum İzleyicisi, IIS'de barındırılan bir .NET uygulamasını şirket içinde veya VM'de çalmak için kullanılır.

- Uygulamanız Azure VM veya Azure sanal makine ölçeği kümesine dağıtıldıysa, [aşağıdaki yönergeleri](azure-vm-vmss-apps.md)izleyin.
- Uygulamanız Azure uygulama hizmetlerine dağıtıldıysa, [aşağıdaki yönergeleri](azure-web-apps.md)izleyin.
- Uygulamanız bir Azure VM'de dağıtılırsa, Azure denetim panelinden Uygulama Öngörüleri izlemeyi açabilirsiniz.
- (Azure [Bulut Hizmetleri'ni](../../azure-monitor/app/cloudservices.md)gadget'landırma yla ilgili ayrı makaleler de vardır.)


![Başarısız istekler, sunucu yanıt süresi ve sunucu istekleri hakkında bilgi içeren App Insights genel bakış grafiklerinin ekran görüntüsü](./media/monitor-performance-live-website-now/overview-graphs.png)

.NET web uygulamalarınıza Uygulama Öngörüleri uygulamak için iki yol seçeneğiniz vardır:

* **Derleme süresi: Web uygulaması kodunuza ** [Application Insights SDK'sını ekleyin][greenbrown].
* **Çalışma zamanı:** Kodu yeniden derlemeden ve yeniden dağıtmadan web uygulamanızı sunucu üzerinde izleyin.

> [!NOTE]
> Yapı zaman enstrümantasyonunu kullanırsanız, çalışma süresi enstrümantasyonu açık olsa bile çalışmaz.

Burada, her yöntemle kazanacaklarınızın bir özeti verilmiştir:

|  | Derleme zamanı | Çalışma zamanı |
| --- | --- | --- |
| İstekler ve özel durumlar |Evet |Evet |
| [Daha ayrıntılı özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) | |Evet |
| [Bağımlılık tanılama](../../azure-monitor/app/asp-net-dependencies.md) |.NET 4.6+ üzerinde ancak daha az ayrıntılı |Evet, tam ayrıntılı: sonuç kodları, SQL komut metni, HTTP fiili|
| [Sistem performans sayaçları](../../azure-monitor/app/performance-counters.md) |Evet |Evet |
| [Özel telemetri için API][api] |Evet |Hayır |
| [İzleme günlüğü tümleştirmesi](../../azure-monitor/app/asp-net-trace-logs.md) |Evet |Hayır |
| [Sayfa görünümü ve kullanıcı verileri](../../azure-monitor/app/javascript.md) |Evet |Hayır |
| Kodu yeniden derlemeniz gerekir |Evet | Hayır |



## <a name="monitor-a-live-iis-web-app"></a>Canlı bir IIS web uygulamasını izleme

Uygulamanız bir IIS sunucusunda barındırılıyorsa, Durum İzleyici’yi kullanarak Application Insights’ı etkinleştirin.

1. IIS web sunucunuzda yönetici kimlik bilgileriyle oturum açın.
2. Application Insights Durum İzleyicisi henüz yüklü değilse, [yükleyiciyi indirin ve çalıştırın](#download)
3. Durum İzleyicisi'nde yüklü web uygulamasını veya izlemek istediğiniz web sitesini seçin. Azure kimlik bilgilerinizle oturum açın.

    Sonuçlarını Application Insights portalında görmek istediğiniz kaynağı yapılandırın. (Normalde, yeni bir kaynak oluşturmak en iyi yöntemdir. Bu uygulama için zaten [web testleriniz][availability] veya [istemci izleme][client] özelliği varsa mevcut bir kaynağı seçin.) 

    ![Uygulama ve kaynak seçin.](./media/monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. IIS’yi yeniden başlatın.

    ![iletişim kutusunun üstündeki Yeniden Başlat’ı seçin.](./media/monitor-performance-live-website-now/appinsights-036-restart.png)

    Web hizmetiniz kısa bir süre kesintiye uğrar.

## <a name="customize-monitoring-options"></a>İzlemeyi özelleştirme seçenekleri

Application Insights’ın etkinleştirilmesi, web uygulamanıza DLL ve ApplicationInsights.config dosyasını ekler. Bazı seçenekleri değiştirmek için [.config dosyasını düzenleyebilirsiniz](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>Uygulamanızı yeniden yayımladığınızda, Application Insights’ı yeniden etkinleştirin

Uygulamanızı yeniden yayımlamadan önce [Visual Studio’daki koda Application Insights'ı eklemeyi][greenbrown] düşünün. Bu şekilde daha ayrıntılı telemetri alabilir ve özel telemetri yazabilirsiniz.

Koda Application Insights eklemeden yeniden yayımlamak istiyorsanız, dağıtım işleminin yayımlanmış web sitesinden DLL ve ApplicationInsights.config dosyasını silebileceğini unutmayın. Bu nedenle:

1. ApplicationInsights.config dosyasını düzenlediyseniz, uygulamanızı yeniden yayımlamadan önce bir kopyasını alın.
2. Uygulamanızı yeniden yayımlayın.
3. Application Insights izlemeyi yeniden etkinleştirin. (Uygun yöntemi kullanın: Azure web uygulaması denetim masası veya bir IIS konağındaki Durum İzleyicisi.)
4. .config dosyası üzerinde yaptığınız tüm düzenlemeleri yeniden devreye sokun.


## <a name="troubleshooting"></a><a name="troubleshoot"></a>Sorun giderme

### <a name="confirm-a-valid-installation"></a>Geçerli bir yüklemeyi onaylama 

Bunlar, yüklemenizin başarılı olduğunu doğrulamak için gerçekleştirebileceğiniz bazı adımlardır.

- ApplicationInsights.config dosyasının hedef uygulama dizininde bulunduğunu ve ikey'inizi içerdiğini doğrulayın.

- Verilerin eksik olduğundan şüpheleniyorsanız, şu anda telemetri gönderen tüm bulut rollerini listelemek için [Analytics'te](../log-query/get-started-portal.md) basit bir sorgu çalıştırabilirsiniz.
  ```Kusto
  union * | summarize count() by cloud_RoleName, cloud_RoleInstance
  ```

- Application Insights'ın başarıyla eklendiğini doğrulamanız gerekiyorsa, Applicationinsights.dll'nin IIS tarafından yüklendiğini doğrulamak için bir komut penceresinde [Sysinternals Handle](https://docs.microsoft.com/sysinternals/downloads/handle) çalıştırabilirsiniz.
  ```cmd
  handle.exe /p w3wp.exe
  ```


### <a name="cant-connect-no-telemetry"></a>Bağlanamıyor musunuz? Telemetri yok mu?

* Durum İzleyicisi’nin çalışmasına izin vermek için sunucunuzun güvenlik duvarında [gerekli giden bağlantı noktalarını](../../azure-monitor/app/ip-addresses.md#outgoing-ports) açın.

### <a name="unable-to-login"></a>Giriş yapamıyor

* Durum İzleyicisi oturum açamıyorsa, bunun yerine bir komut satırı yükleyin. Durum İzleyicisi ikey'inizi toplamak için oturum açmaya çalışır, ancak bu komutu kullanarak bunu el ile sağlayabilirsiniz:

```powershell
Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'
Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000
```

### <a name="could-not-load-file-or-assembly-systemdiagnosticsdiagnosticsource"></a>Dosya veya montaj 'System.Diagnostics.DiagnosticSource' yükleyemedi

Uygulama Öngörüleri'ni etkinleştirdikten sonra bu hatayı alabilirsiniz. Bunun nedeni, yükleyicinin çöp kutusu dizininizdeki bu dll'nin yerini almasıdır.
web.config güncelleme düzeltmek için:

```xml
<dependentAssembly>
    <assemblyIdentity name="System.Diagnostics.DiagnosticSource" publicKeyToken="cc7b13ffcd2ddd51"/>
    <bindingRedirect oldVersion="0.0.0.0-4.*.*.*" newVersion="4.0.2.1"/>
</dependentAssembly>
```

[Biz burada](https://github.com/Microsoft/ApplicationInsights-Home/issues/301)bu sorunu takip ediyoruz.


### <a name="application-diagnostic-messages"></a>Uygulama tanılama iletileri

* Durum İzleyicisi’ni açın ve sol bölmede uygulamanızı seçin. "Yapılandırma bildirimleri" bölümünde bu uygulamayla ilgili herhangi bir tanılama iletisi olup olmadığını denetleyin:

  ![İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın](./media/monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
  
### <a name="detailed-logs"></a>Ayrıntılı günlükler

* Varsayılan olarak Durum Monitörü tanılama günlüklerini şu anda çıkaracaktır:`C:\Program Files\Microsoft Application Insights\Status Monitor\diagnostics.log`

* Verbose günlüklerini çıktıalmak `C:\Program Files\Microsoft Application Insights\Status Monitor\Microsoft.Diagnostics.Agent.StatusMonitor.exe.config` `<add key="TraceLevel" value="All" />` için config dosyasını `appsettings`değiştirin: ve .
Ardından durum izleme yi yeniden başlatın.

* Durum Monitörü bir .NET uygulaması olduğu için [config dosyasına uygun tanılamaları ekleyerek .net izlemeyi](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/trace-debug/system-diagnostics-element)de etkinleştirebilirsiniz. Örneğin, bazı senaryolarda [ağ izleme sini yapılandırarak](https://docs.microsoft.com/dotnet/framework/network-programming/how-to-configure-network-tracing) ağ düzeyinde neler olduğunu görmek yararlı olabilir

### <a name="insufficient-permissions"></a>Yetersiz izinler
  
* "Yetersiz izinler" hakkında bir ileti görürseniz, sunucuda aşağıdakileri deneyin:
  * IIS Yöneticisi'nde, uygulama havuzunuzu seçin, **Gelişmiş Ayarlar**’ı açın ve **işlem modeli** altında kimliğini not edin.
  * Bilgisayar yönetimi denetim masasında, bu kimliği Performans İzleyicisi Kullanıcıları grubuna ekleyin.

### <a name="conflict-with-systems-center-operations-manager"></a>Sistem Merkezi Operasyon Yöneticisi ile Çakışma

* Sunucunuza yüklenmiş MMA/SCOM (System Center Operations Manager) varsa bazı sürümler çakışabilir. Hem SCOM’u, hem de Durum İzleyicisi’ni kaldırın ve en son sürümleri yeniden yükleyin.

### <a name="failed-or-incomplete-installation"></a>Başarısız veya eksik yükleme

Durum İzleyicisi yükleme sırasında başarısız olursa, Durum İzleyicisi kurtaramayacağı eksik bir yükleme ile bırakılabilir. Bunun için manuel sıfırlama gerekir.

Uygulama dizininizde bulunan bu dosyalardan herhangi birini silin:
- "Microsoft.AI" ile başlayan çöp kutusu dizinindeki herhangi bir DLs. veya "Microsoft.ApplicationInsights".
- Bu DLL sizin bin dizini "Microsoft.Web.Infrastructure.dll"
- Bu DLL sizin bin dizini "System.Diagnostics.DiagnosticSource.dll"
- Uygulama dizininizde "App_Data\packages" kaldırın
- Uygulama dizininizde "applicationinsights.config" kaldırın


### <a name="additional-troubleshooting"></a>Ek Sorun Giderme Seçenekleri

* Bkz. Ek [Sorun Giderme.][qna]

## <a name="system-requirements"></a>Sistem Gereksinimleri
Sunucuda Application Insights Durum İzleyicisi için işletim sistemi desteği:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2
* Windows Server 2016

en son SP ve .NET Framework 4.5 (Durum Monitörü çerçevenin bu sürümü üzerine inşa edilmiştir)

İstemci tarafında Windows 7, 8, 8.1 ve 10, .NET Framework 4.5 ile

IIS desteği: IIS 7, 7.5, 8, 8.5 (IIS gereklidir)

## <a name="automation-with-powershell"></a>PowerShell ile Automation
IIS sunucunuzda PowerShell'i kullanarak izlemeyi başlatabilir ve durdurabilirsiniz.

İlk olarak Application Insights modülünü içeri aktarın:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Hangi uygulamaların izlenmekte olduğunu öğrenin:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (İsteğe bağlı) Bir web uygulaması adı.
* Bu IIS sunucusundaki her web uygulaması için (veya adlandırılmış uygulama) Application Insights izleme durumunu görüntüler.
* Her uygulama için `ApplicationInsightsApplication` döndürür:

  * `SdkState==EnabledAfterDeployment`: Uygulama izleniyor ve çalışma zamanında, Durum İzleyicisi aracıyla ya da `Start-ApplicationInsightsMonitoring` ile izleme eklenmiş.
  * `SdkState==Disabled`: Uygulama, Application Insights için izleme eklenmemiş. Hiçbir zaman izlenmeyecek veya Durum İzleyicisi aracıyla veya `Stop-ApplicationInsightsMonitoring` ile çalışma zamanı izlemesi devre dışı bırakılmayacaktır.
  * `SdkState==EnabledByCodeInstrumentation`: Uygulama, kaynak koduna SDK eklenerek izleme eklenmiş. SDK’si güncelleştirilemez veya durdurulamaz.
  * `SdkVersion`, bu uygulamanın izlenmesi için kullanılan sürümü gösterir.
  * `LatestAvailableSdkVersion`, şu anda NuGet galerisinde mevcut olan sürümü gösterir. Uygulama bu sürüme yükseltmek için `Update-ApplicationInsightsMonitoring` kullanın.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Uygulamanın IIS'deki adı
* `-InstrumentationKey` Sonuçların görüntülenmesini istediğiniz Application Insights kaynağına ait iKey.
* Bu cmdlet yalnızca henüz izlenmemiş uygulamaları etkiler; başka bir deyişle zaten işaretlendiğine değil - diğer bir deyişle, SdkState==NotInstrumented.

    Cmdlet, zaten izleme eklenmiş olan uygulamayı etkilemez. Uygulamanın koda SDK eklenerek derleme zamanında izlenmesi veya cmdlet’in önceki bir kullanımıyla çalışma zamanında izlenmesi farklılık yaratmaz.

    Uygulamayı izlemek için kullanılan SDK sürümü sunucuya en son indirilen sürümdür.

    En son sürümü indirmek için Update-ApplicationInsightsVersion kullanın.
* Başarılı bir şekilde `ApplicationInsightsApplication` döndürür. Başarısız olursa, stderr’e bir izleme kaydeder.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` Bir uygulamanın IIS'deki adı
* `-All` Bu IIS sunucusunda, şu koşulu sağlayan tüm uygulamaları izlemeyi durdurur: `SdkState==EnabledAfterDeployment`
* Belirtilen uygulamaları izlemeyi durdurur ve izlemeyi kaldırır. Yalnızca, Durum İzleme aracını veya Start-ApplicationInsightsApplication kullanarak çalışma zamanında izleme eklenmiş uygulamalar için çalışır. (`SdkState==EnabledAfterDeployment`)
* ApplicationInsightsApplication döndürür.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: Bir web uygulamasının IIS'deki adı.
* `-InstrumentationKey`(İsteğe bağlı.) Uygulamanın telemetrisinin gönderildiği kaynağı değiştirmek için bunu kullanın.
* Bu cmdlet:
  * Adlandırılmış uygulamaları SDK'nin bu makineye en son indirilen sürümüne yükseltir. (Yalnızca `SdkState==EnabledAfterDeployment` ise çalışır)
  * İzleme anahtarını sağlarsanız, adlandırılmış uygulama, telemetriyi bu anahtarla kaynağa göndermek için yeniden yapılandırılır. (`SdkState != Disabled` ise çalışır)

`Update-ApplicationInsightsVersion`

* En son Application Insights SDK’sini sunucuya indirir.

## <a name="questions-about-status-monitor"></a><a name="questions"></a>Durum İzleyicisi hakkında sorular

### <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuza yüklediğiniz bir masaüstü uygulamasıdır. Web uygulamalarını izleyip yapılandırmanıza yardımcı olur. 

### <a name="when-do-i-use-status-monitor"></a>Durum İzleyicisi’ni ne zaman kullanırım?

* Halihazırda çalışıyor olsa bile, IIS sunucunuzda çalışan herhangi bir web uygulamasını izleme.
* Derleme zamanında [Application Insights SDK'sı ile derlenmiş](../../azure-monitor/app/asp-net.md) web uygulamaları için ek telemetriyi etkinleştirme. 

### <a name="can-i-close-it-after-it-runs"></a>Çalıştıktan sonra kapatabilir miyim?

Evet. Seçtiğiniz web sitelerini izledikten sonra kapatabilirsiniz.

Telemetriyi tek başına toplamaz. Yalnızca web uygulamalarını yapılandırır ve bazı izinleri ayarlar.

### <a name="what-does-status-monitor-do"></a>Durum İzleyicisi ne yapar?

Durum İzleyicisi’nin izlemesi için bir web uygulaması seçtiğinizde:

* Uygulama Öngörüleri derlemelerini ve ApplicationInsights.config dosyasını web uygulamasının ikili klasörüne indirir ve yerleştirir.
* Bağımlılık çağrılarını toplamak için CLR profil oluşturma özelliğini etkinleştirir.

### <a name="what-version-of-application-insights-sdk-does-status-monitor-install"></a>Status Monitor, Application Insights SDK'nın hangi sürümünü yükler?

Şu andan itibaren Durum Monitörü yalnızca Application Insights SDK sürümlerini 2.3 veya 2.4 olarak yükleyebilir. 

Uygulama Öngörüleri SDK Sürüm 2.4 [EOL Ocak 2016](https://devblogs.microsoft.com/dotnet/support-ending-for-the-net-framework-4-4-5-and-4-5-1/)oldu [.NET 4.0 destekleyen son sürümüdür.](https://github.com/microsoft/ApplicationInsights-dotnet/releases/tag/v2.5.0-beta1) Bu nedenle, şu andan itibaren Durum Monitörü bir .NET 4.0 uygulaması na neden olmak için kullanılabilir. 

### <a name="do-i-need-to-run-status-monitor-whenever-i-update-the-app"></a>Uygulamayı her güncelleştirdiğimde Durum İzleyicisi’ni çalıştırmam gerekiyor mu?

Artımlı olarak dağıtmanız durumunda gerekli değildir. 

Yayımlama işleminde 'mevcut dosyaları sil' seçeneğini belirlerseniz, Application Insights’ı yapılandırmak için Durum İzleyicisi’ni yeniden çalıştırmanız gerekir.

### <a name="what-telemetry-is-collected"></a>Hangi telemetri toplanır?

Durum İzleyicisi'ni kullanarak yalnızca çalışma zamanında izlediğiniz uygulamalar için:

* HTTP istekleri
* Bağımlılık çağrıları
* Özel durumlar
* Performans sayaçları

Derleme zamanında zaten izlenmekte olan uygulamalar için:

 * İşlem sayaçları.
 * Bağımlılık çağrıları (.NET 4.5); bağımlılık çağrılarındaki dönüş değerleri (.NET 4.6).
 * Özel durum yığın izleme değerleri.

[Daha fazlasını öğrenin](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="download-status-monitor"></a><a name="download"></a>Durum Monitörünü İndir

- Yeni [PowerShell Modül'u](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) kullanın
- [Durum Monitörü yükleyicisini](https://go.microsoft.com/fwlink/?LinkId=506648) indirin ve çalıştırın
- Veya [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) çalıştırın ve Uygulama Öngörüleri Durum Monitörü için arama.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performans ve kullanımı izlemek için [ölçümleri keşfedin](../../azure-monitor/platform/metrics-charts.md)
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic]
* Daha gelişmiş sorgular için [analiz](../../azure-monitor/app/analytics.md)

Daha fazla telemetri ekleyin:

* Sitenizin canlı kalması için [web testleri oluşturun][availability].
* Web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için [web istemcisi telemetrisini ekleyin][usage].
* İzleme ve günlük çağrıları ekleyebilmek için [kodunuza Application Insights SDK’sı ekleyin][greenbrown]

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[availability]: monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[usage]: ../../azure-monitor/app/javascript.md
