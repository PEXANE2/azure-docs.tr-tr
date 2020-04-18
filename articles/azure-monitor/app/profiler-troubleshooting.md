---
title: Azure Application Insights Profiler ile ilgili sorun giderme
description: Bu makalede, Uygulama Öngörüleri Profil Oluşturucusu'nun etkinleştirilmesi veya kullanılmasında sorun yaşayan geliştiricilere yardımcı olmak için sorun giderme adımları ve bilgiler yer alıyor.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 55bc4ff05b650884ef17e0de10d7156cbf458a9c
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640956"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Uygulama Öngörüleri Profiler'ı etkinleştirme veya görüntüleme sorunlarını giderme

## <a name="active-issues"></a>Etkin sorunlar

* ASP.NET Core 3.x uygulamaları için profil oluşturma artık Azure Uygulama Hizmetleri'nde desteklenir.

## <a name="general-troubleshooting"></a><a id="troubleshooting"></a>Genel sorun giderme

### <a name="profiles-are-uploaded-only-if-there-are-requests-to-your-application-while-profiler-is-running"></a>Profiller yalnızca Profiler çalışırken uygulamanız için istek varsa yüklenir

Azure Application Insights Profiler profil oluşturma verilerini saatte iki dakika toplar. **Ayrıca, Yapıl'ı Uygulama Öngörüleri Profil oluşturbölmesinde** **Profil Şimdi** düğmesini seçtiğinizde de veri toplar. Ancak profil oluşturma verileri yalnızca Profiler çalışırken gerçekleşen bir isteğe eklenebileceğinde yüklenir. 

Profiler, Application Insights kaynağınıza izleme iletileri ve özel olaylar yazar. Profiler'ın nasıl çalıştığını görmek için bu olayları kullanabilirsiniz. Profiler'ın izlemeleri çalıştırıp yakalaması gerektiğini düşünüyorsanız, ancak bunlar **Performans** bölmesinde görüntülenmiyorsa, Profiler'ın nasıl çalıştığını görmek için denetleyebilirsiniz:

1. Profiler tarafından Application Insights kaynağınıza gönderilen izleme iletilerini ve özel olayları arayın. İlgili verileri bulmak için bu arama dizesini kullanabilirsiniz:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Aşağıdaki resim, iki AI kaynağından iki arama örneği görüntüler: 
    
   * Profil oluşturucu çalışırken uygulama solda istek almıyor. İleti, yüklemenin hiçbir etkinlik olmadığı için iptal edildiğini açıklar. 

   * Sağda Profiler, Profiler çalışırken gerçekleşen istekleri algıladığında özel olaylar başlattı ve gönderdi. ServiceProfilerSample özel olayı görüntülenirse, Profiler'ın bir isteğe izleme iliştirdi ve **izlemeyi Uygulama Öngörüleri Performans** bölmesinde görüntüleyebilirsiniz.

     Telemetri görüntülenmiyorsa Profiler çalışmıyor. Sorun gidermek için, bu makalenin ilerleyen bölümlerinde belirli uygulama türünüz için sorun giderme bölümlerine bakın.  

     ![Profil oluşturucu telemetrisini ara][profiler-search-telemetry]

1. Profiler çalışırken istekler varsa, isteklerin uygulamanızın Profiler etkinleştirilmiş bölümü tarafından işlendiğinden emin olun. Uygulamalar bazen birden çok bileşenden oluşsa da, Profiler bileşenlerin yalnızca bazıları için etkinleştirilir. **Yapılandır Uygulama Öngörüleri Profiler** bölmesi, izleme yüklemiş bileşenleri görüntüler.

### <a name="other-things-to-check"></a>Kontrol etmek için diğer şeyler
* Uygulamanızın .NET Framework 4.6'da çalışırken olduğundan emin olun.
* Web uygulamanız ASP.NET Core uygulamasıysa, en az Core 2.0 ASP.NET çalışıyor olmalıdır.
* Görüntülemeye çalıştığınız veriler birkaç haftadan eskiyse, zaman filtrenizi sınırlamayı deneyin ve yeniden deneyin. İzler yedi gün sonra silinir.
* Yakınlık veya güvenlik duvarının erişimi engellemediğinden emin https://gateway.azureserviceprofiler.netolun.
* Profiler ücretsiz veya paylaşılan uygulama hizmeti planlarında desteklenmez. Bu planlardan birini kullanıyorsanız, temel planlardan birine kadar ölçeklemayı deneyin ve Profiler çalışmaya başlamalıdır.

### <a name="double-counting-in-parallel-threads"></a><a id="double-counting"></a>Paralel iş parçacıklarında çift sayma

Bazı durumlarda, yığın görüntüleyicideki toplam zaman ölçümü istek süresinden fazladır.

Bu durum, iki veya daha fazla iş parçacığı bir istekle ilişkili olduğunda ve bunlar paralel olarak çalıştıklarında oluşabilir. Bu durumda, toplam iş parçacığı süresi geçen süreden daha fazladır. Bir iş parçacığı tamamlanmak üzere diğer bekliyor olabilir. İzleyici bu durumu algılamaya çalışır ve ilginç olmayan beklemeyi atlar. Bunu yaparken, kritik bilgi olabilecek bilgileri atlayabilmek yerine çok fazla bilgi görüntüleme tarafında hatalıdır.

İzlemelerinizde paralel iş parçacıkları gördüğünüzde, istek için kritik yolu tespit etmek için hangi iş parçacıklarının beklediğini belirleyin. Genellikle, hızlı bir şekilde bekleme durumuna gider iş parçacığı sadece diğer iş parçacığı bekliyor. Diğer iş parçacıklarına odaklanın ve bekleyen iş parçacıklarındaki zamanı yoksayın.

### <a name="error-report-in-the-profile-viewer"></a>Profil görüntüleyicide hata raporu
Portala bir destek bileti gönderin. Hata iletisinden korelasyon kimliğini eklediğinden emin olun.

## <a name="troubleshoot-profiler-on-azure-app-service"></a>Azure Uygulama Hizmetinde Sorun Giderme Profilleyicisi
Profiler'ın düzgün çalışması için:
* Web uygulaması hizmet planınız Temel katman veya daha yüksek olmalıdır.
* Web uygulamanız Uygulama Öngörüleri etkin olmalıdır.
* Web uygulamanız aşağıdaki uygulama ayarlarına sahip olmalıdır:

    |Uygulama Ayarı    | Değer    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | uygulama öngörüleri kaynağınız için iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |


* **ApplicationInsightsProfiler3** webjob çalışıyor olmalıdır. Web job'u kontrol etmek için:
   1. [Kudu'ya](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/)git.
   1. **Araçlar** menüsünde **Web İşler Panosu'nu**seçin.  
      **Webİşler** bölmesi açılır. 
   
      ![profiler-webjob]   
   
   1. Günlük de dahil olmak üzere web işinin ayrıntılarını görüntülemek için **ApplicationInsightsProfiler3** bağlantısını seçin.  
     **Sürekli Webİş Ayrıntıları** bölmesi açılır.

      ![profiler-webjob-log]

Profiler'ın neden sizin için çalışmadığını çözemezseniz, günlüğü indirebilir ve yardım için ekibimize serviceprofilerhelp@microsoft.comgönderebilirsiniz. 
    
### <a name="manual-installation"></a>El ile yükleme

Profiler'ı yapılandırdığınızda, web uygulamasının ayarlarında güncelleştirmeler yapılır. Ortamınız gerektiriyorsa, güncelleştirmeleri el ile uygulayabilirsiniz. Uygulamanızın PowerApps için bir Web Apps ortamında çalışıyor olması bir örnek olabilir. Güncelleştirmeleri el ile uygulamak için:

1. Web **App Control** bölmesinde **Ayarlar'ı**açın.

1. **.NET Framework sürümünü** **v4.6**olarak ayarlayın.

1. Her Zaman **Anına**Ayarlayın. **Always On**
1. Bu uygulama ayarlarını oluşturun:

    |Uygulama Ayarı    | Değer    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | uygulama öngörüleri kaynağınız için iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |

### <a name="too-many-active-profiling-sessions"></a>Çok fazla etkin profil oluşturma oturumu

Şu anda Profiler'ı aynı hizmet planında çalışan en fazla dört Azure web uygulaması ve dağıtım yuvasında etkinleştirebilirsiniz. Bir uygulama hizmet planında çalışan birden fazla web uygulamanız varsa, Profiler bir *Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException*atabilir. Profiler her web uygulaması için ayrı ayrı çalışır ve her uygulama için bir Etkinlik Windows İzleme (ETW) oturumu başlatmaya çalışır. Ancak sınırlı sayıda ETW oturumu aynı anda etkin olabilir. Profiler webjob çok fazla etkin profil oluşturma oturumu bildiriyorsa, bazı web uygulamalarını farklı bir hizmet planına taşıyın.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootapp_datajobs"></a>Dağıtım hatası: D:\\ev\\sitesi\\wwwroot\\\\App_Data işleri' Boş Değil

Web uygulamanızı Profiler etkinleştirilmiş bir Web Apps kaynağına yeniden dağıtıyorsanız, aşağıdaki iletiyi görebilirsiniz:

*\\D: ev\\sitesi\\wwwroot\\App_Data\\işleri'*

Bu hata, web dağıtımı komut dosyalarından veya Azure DevOps dağıtım ardışık ardışık ardışık kaynaktan çalıştırırsanız oluşur. Çözüm, Web Dağıtım görevine aşağıdaki ek dağıtım parametrelerini eklemektir:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler, Application Insights Profiler tarafından kullanılan klasörü siler ve yeniden dağıtma işleminin engelini kaldır. Şu anda çalışan Profiler örneğini etkilemez.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Application Insights Profiler'ın çalışıp çalışmadığını nasıl belirleyebilirim?

Profiler web uygulamasında sürekli bir web işi olarak çalışır. Azure [portalında](https://portal.azure.com)web uygulaması kaynağını açabilirsiniz. **Webİşler** bölmesinde **ApplicationInsightsProfiler'ın**durumunu kontrol edin. Çalışmıyorsa, daha fazla bilgi almak için **Günlükler'i** açın.

## <a name="troubleshoot-problems-with-profiler-and-azure-diagnostics"></a>Profiler ve Azure Tanılama ile ilgili sorun giderme sorunları

>**Bulut Hizmetleri için WAD'da bulunan profil oluşturucudaki hata düzeltildi.** Bulut Hizmetleri için WAD'ın (1.12.2.0) en son sürümü, App Insights SDK'nın tüm son sürümleriyle çalışır. Bulut Hizmeti ana bilgisayarları WAD'ı otomatik olarak yükseltir, ancak hemen yükseltmeyapmaz. Yükseltmeyi zorlamak için, hizmetinizi yeniden dağıtabilir veya düğümü yeniden başlatabilirsiniz.

Profiler'ın Azure Tanılama tarafından doğru şekilde yapılandırılıp yapılandırılmadığını görmek için aşağıdaki üç şeyi yapın: 
1. İlk olarak, dağıtılan Azure Tanılama yapılandırmasının içeriğinin beklediğiniz şey olup olmadığını kontrol edin. 

1. İkinci olarak, Azure Diagnostics'in Profiler komut satırında uygun iKey'i geçtiğinden emin olun. 

1. Üçüncü olarak, Profiler'ın çalışıp çalışmadığını ancak bir hata döndürüp döndürmediğini görmek için Profiler günlük dosyasını denetleyin. 

Azure Tanılama'yı yapılandırmak için kullanılan ayarları denetlemek için:

1. Sanal makinede (VM) oturum açın ve ardından bu konumdaki günlük dosyasını açın. (Sürücü c olabilir: veya d: ve eklenti sürümü farklı olabilir.)

    ```
    c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log  
    ```
    or
    ```
    c:\WindowsAzure\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log
    ```

1. Dosyada, Azure Tanılama'yı yapılandırmak için VM'ye geçirilen ayarları bulmak için **WadCfg** dizesini arayabilirsiniz. Profiler lavabosu tarafından kullanılan iKey'in doğru olup olmadığını kontrol edebilirsiniz.

1. Profiler'ı başlatmak için kullanılan komut satırını kontrol edin. Profiler'ı başlatmak için kullanılan bağımsız değişkenler aşağıdaki dosyada yer alıyor. (Sürücü c olabilir: veya d:)

    ```
    D:\ProgramData\ApplicationInsightsProfiler\config.json
    ```

1. Profiler komut satırındaki iKey'in doğru olduğundan emin olun. 

1. Önceki *config.json* dosyasında bulunan yolu kullanarak Profiler günlük dosyasını denetleyin. Profiler'ın kullandığı ayarları gösteren hata ayıklama bilgilerini görüntüler. Ayrıca Profiler'dan gelen durum ve hata iletilerini de görüntüler.  

    Uygulamanız istek alırken Profiler çalışıyorsa, aşağıdaki ileti görüntülenir: *iKey'den algılanan etkinlik.* 

    İzleme yüklenirken aşağıdaki ileti görüntülenir: *İzlemeyi yüklemeye başlayın.* 


## <a name="edit-network-proxy-or-firewall-rules"></a>Ağ proxy'si veya güvenlik duvarı kurallarını edin

Uygulamanız bir proxy veya güvenlik duvarı aracılığıyla Internet'e bağlanıyorsa, uygulamanızın Application Insights Profiler hizmetiyle iletişim kurabilmesi için kuralları yeniden oluşturmanız gerekebilir. Application Insights Profiler tarafından kullanılan IP'ler Azure Monitor hizmet etiketine dahildir.


[profiler-search-telemetry]:./media/profiler-troubleshooting/Profiler-Search-Telemetry.png
[profiler-webjob]:./media/profiler-troubleshooting/Profiler-webjob.png
[profiler-webjob-log]:./media/profiler-troubleshooting/Profiler-webjob-log.png








