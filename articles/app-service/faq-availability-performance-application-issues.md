---
title: Uygulama performansı SSS'leri
description: Azure Uygulama Hizmeti'nde kullanılabilirlik, performans ve uygulama sorunları yla ilgili sık sorulan soruların yanıtlarını alın.
author: genlin
manager: dcscontentpm
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 433f5885c7f057226e78c4ae57e03d7619004d21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259870"
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Azure'daki Web Apps için Uygulama performansı SSS'leri

> [!NOTE]
> Aşağıdaki yönergelerden bazıları yalnızca Windows veya Linux Uygulama Hizmetleri'nde çalışabilir. Örneğin, Linux Uygulama Hizmetleri varsayılan olarak 64 bit modunda çalışır.
>

Bu makalede, [Azure Uygulama Hizmeti'nin Web Apps özelliğiiçin](https://azure.microsoft.com/services/app-service/web/)uygulama performansı sorunları yla ilgili sık sorulan soruların (SSS' ler) yanıtları bulunmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Uygulamam neden yavaş?

Birden çok etken yavaş uygulama performansına katkıda bulunabilir. Ayrıntılı sorun giderme adımları için, [Sorun Giderme yavaş web uygulaması performansı](troubleshoot-performance-degradation.md)bakın.

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Yüksek CPU tüketimi senaryosuyla nasıl sorun giderebilirim?

Bazı yüksek CPU tüketimi senaryolarında, uygulamanız gerçekten daha fazla bilgi işlem kaynağı gerektirebilir.Bu durumda, uygulamanın ihtiyacı olan tüm kaynakları alması için daha yüksek bir hizmet katmanına ölçeklemeyi düşünün. Diğer zamanlarda, yüksek CPU tüketimi kötü bir döngü veya bir kodlama uygulaması neden olabilir. Cpu tüketimini neyin tetiklediğine dair fikir edinmek iki bölümlü bir işlemdir. İlk olarak, bir işlem dökümü oluşturun ve sonra işlem dökümü analiz. Daha fazla bilgi için, [Web Apps için yüksek CPU tüketimi için bir döküm dosyayı yakalama ve çözümleme dosyasına](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/)bakın.

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Yüksek bellek tüketimi senaryosunda nasıl sorun giderebilirim?

Bazı yüksek bellek tüketimi senaryolarında, uygulamanız gerçekten daha fazla bilgi işlem kaynağı gerektirebilir.Bu durumda, uygulamanın ihtiyacı olan tüm kaynakları alması için daha yüksek bir hizmet katmanına ölçeklemeyi düşünün. Diğer zamanlarda, koddaki bir hata bellek sızıntısına neden olabilir. Kodlama uygulaması da bellek tüketimini artırabilir.Yüksek bellek tüketimini neyin tetiklediğine dair bilgi edinmek iki bölümlü bir işlemdir. İlk olarak, bir işlem dökümü oluşturun ve sonra işlem dökümü analiz. Azure Site Uzantısı Galerisi'ndeki Kilitlenme Tanılayıcısı, bu adımların her ikisini de verimli bir şekilde gerçekleştirebilir. Daha fazla bilgi için, [Web Apps için aralıklı yüksek bellek için bir döküm dosyasını yakalama ve çözümleme dosyasına](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/)bakın.

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>PowerShell'i kullanarak App Service web uygulamalarını nasıl otomatikleştirebilirim?

App Service web uygulamalarını yönetmek ve sürdürmek için PowerShell cmdlets'i kullanabilirsiniz. Blog yazımızda [PowerShell'i kullanarak Azure App Hizmeti'nde barındırılan web uygulamalarını otomatikleştirin](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), ortak görevleri otomatikleştirmek için Azure Kaynak Yöneticisi tabanlı PowerShell cmdlets'in nasıl kullanılacağını anlatıyoruz. Blog yazısı da çeşitli web uygulamaları yönetim görevleri için örnek kodu vardır. Tüm App Service web uygulamaları cmdlets için açıklamalar ve sözdizimi için [Az.Web Sitelerine](/powershell/module/az.websites)bakın.

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Web uygulamamın etkinlik günlüklerini nasıl görüntüleyebilirim?

Web uygulamanızın etkinlik günlüklerini görüntülemek için:

1. [Kudu web sitenizde](https://*yourwebsitename*.scm.azurewebsites.net)oturum açın.
2. Menüde Hata **Ayıklama Konsolu** > **CMD'yi**seçin.
3. Günlük **Dosyaları** klasörünü seçin.
4. Olay günlüklerini görüntülemek için **eventlog.xml'in**yanındaki kalem simgesini seçin.
5. Günlükleri indirmek için PowerShell cmdlet'i `Save-AzureWebSiteLog -Name webappname`çalıştırın.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Web uygulamamın kullanıcı modu bellek dökümünün nasıl yakanıa biliyorum?

Web uygulamanızın kullanıcı modu bellek dökümünün yakalanması için:

1. [Kudu web sitenizde](https://*yourwebsitename*.scm.azurewebsites.net)oturum açın.
2. İşlem **Gezgini** menüsünü seçin.
3. Sağ **w3wp.exe** işlemi veya WebJob süreci tıklayın.
4. **Bellek Dökümü** > **Tam Dump**İndir seçin.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Web uygulamamın işlem düzeyi bilgilerini nasıl görüntüleyebilirim?

Web uygulamanız için işlem düzeyi bilgilerini görüntülemek için iki seçeneğiniz vardır:

*   Azure portalında:
    1. Web uygulaması için **İşlem Gezgini'ni** açın.
    2. Ayrıntıları görmek için **w3wp.exe** işlemini seçin.
*   Kudu konsolunda:
    1. [Kudu web sitenizde](https://*yourwebsitename*.scm.azurewebsites.net)oturum açın.
    2. İşlem **Gezgini** menüsünü seçin.
    3. **w3wp.exe** işlemi için **Özellikler'i**seçin.

## <a name="when-i-browse-to-my-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Uygulamama göz atdığımda "Hata 403 - Bu web uygulaması durduruldu" diye görüyorum. Bu sorunu nasıl çözebilirim?

Üç koşul bu hataya neden olabilir:

* Web uygulaması bir faturalandırma sınırına ulaştı ve siteniz devre dışı bırakıldı.
* Web uygulaması portalda durduruldu.
* Web uygulaması, Ücretsiz veya Paylaşılan ölçek hizmet planına uygulanabilecek bir kaynak kotası sınırına ulaştı.

Hataya neyin neden olduğunu görmek ve sorunu çözmek için Web Apps'taki adımları [izleyin: "Hata 403 – Bu web uygulaması durdurulur"](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Çeşitli Uygulama Hizmeti planları için kotalar ve limitler hakkında daha fazla bilgi nereden edinebilirim?

Kotalar ve sınırlar hakkında bilgi için [Uygulama Hizmeti sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın. 

## <a name="how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time"></a>Boşta kalan süreden sonra ilk isteğin yanıt süresini nasıl kısaltebilirim?

Varsayılan olarak, belirli bir süre boşta kalırlarsa web uygulamaları boşaltılır. Bu şekilde, sistem kaynakları muhafaza edebilirsiniz. Dezavantajı, web uygulamasının yüklenmesine ve yanıt sunmaya başlamasına izin vermek için web uygulaması nın boşaltılmasından sonra ilk isteğe verilen yanıtın daha uzun olmasıdır. Temel ve Standart hizmet planlarında, uygulamayı her zaman yüklü tutmak için **Her Zaman Ayarı'nı** açabilirsiniz. Bu, uygulama boşta kaldıktan sonra daha uzun yükleme sürelerini ortadan kaldırır. **Her Zaman** Ayarı'nı değiştirmek için:

1. Azure portalında web uygulamanıza gidin.
2. **Yapılandırma'yı** seçin
3. **Genel ayarları**seçin.
4. **Always On**için , **On'u**seçin.

## <a name="how-do-i-turn-on-failed-request-tracing"></a>Başarısız istek izlemeyi nasıl açarım?

Başarısız istek izlemesini açmak için:

1. Azure portalında web uygulamanıza gidin.
3. **Tüm Ayarlar** > **Tanılama Günlükleri'ni**seçin.
4. **Başarısız İstek İzleme için,** **On'u**seçin.
5. **Kaydet'i**seçin.
6. Web uygulama bıçağında **Araçlar'ı**seçin.
7. **Visual Studio Online'ı**seçin.
8. Ayar **Ayarı Üzerinde**değilse, **A'yı**seçin.
9. **Git'i**seçin.
10. **Web.config'i**seçin.
11. system.webServer'da bu yapılandırmayı ekleyin (belirli bir URL'yi yakalamak için):

    ```xml
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. Yavaş performans sorunlarını gidermek için bu yapılandırmayı ekleyin (yakalama isteği 30 saniyeden fazla sürüyorsa):
    ```xml
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. Başarısız istek izlerini indirmek için, [portalda,](https://portal.azure.com)web sitenize gidin.
15. **Araçlar** > **Kudu** > **Go**seçin.
18. Menüde Hata **Ayıklama Konsolu** > **CMD'yi**seçin.
19. **LogFiles** klasörünü seçin ve ardından **W3SVC**ile başlayan bir ada sahip klasörü seçin.
20. XML dosyasını görmek için kalem simgesini seçin.

## <a name="i-see-the-message-worker-process-requested-recycle-due-to-percent-memory-limit-how-do-i-address-this-issue"></a>"İşçi İşlemi 'Yüzde Bellek' sınırı nedeniyle geri dönüşüm istedi" iletisini görüyorum. Bu sorunu nasıl giderebilirim?

32 bit işlem için kullanılabilir maksimum bellek miktarı (64 bit işletim sisteminde bile) 2 GB'dır. Varsayılan olarak, alt işlem App Service'de 32 bit olarak ayarlanır (eski web uygulamalarıyla uyumluluk için).

Web Çalışanı rolünüzde bulunan ek bellekten yararlanabilmek için 64 bit işlemlere geçmeyi düşünün. Bu bir web uygulaması yeniden tetikler, bu nedenle buna göre zamanlama.

Ayrıca, 64 bit ortamın Temel veya Standart hizmet planı gerektirdiğini de unutmayın. Ücretsiz ve Paylaşılan planlar her zaman 32 bit lik bir ortamda çalışır.

Daha fazla bilgi için Bkz. [Uygulama Hizmeti'ndeki web uygulamalarını yapılandır.](configure-common.md)

## <a name="why-does-my-request-time-out-after-230-seconds"></a>İsteğim neden 230 saniye sonra doluyor?

Azure Yük Dengeleyicisi'nde varsayılan olarak dört dakikalık bir zaman ayarı vardır. Bu genellikle bir web isteği için makul bir yanıt süresi sınırıdır. Web uygulamanız arka plan işleme gerektiriyorsa, Azure Web İşlerini kullanmanızı öneririz. Azure web uygulaması Web İşler'i arayabilir ve arka plan işleme tamamlandığında bilgilendirilebilir. Kuyruklar ve tetikleyiciler de dahil olmak üzere Web İşlerini kullanmak için birden çok yöntem arasından seçim yapabilirsiniz.

WebJobs arka plan işleme için tasarlanmıştır. Bir WebJob'ta istediğiniz kadar arka plan işleme yapabilirsiniz. Web İşler hakkında daha fazla bilgi için [bkz.](webjobs-create.md)

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>ASP.NET App Service'de barındırılan Core uygulamaları bazen yanıt vermeyi durdurur. Bu sorunu nasıl giderebilirim?

Daha önceki bir [Kestrel sürümüyle](https://github.com/aspnet/KestrelHttpServer/issues/1182) ilgili bilinen bir sorun, App Service'de barındırılan ASP.NET Core 1.0 uygulamasının yanıt vermeyi aralıklı olarak durdurmasına neden olabilir. Bu iletiyi de görebilirsiniz: "Belirtilen CGI Uygulaması bir hatayla karşılaştı ve sunucu işlemi sonlandırdı."

Bu sorun Kestrel sürüm 1.0.2'de giderilmiştir. Bu sürüm, Core 1.0.3 ASP.NET güncellemesine dahildir. Bu sorunu gidermek için Kestrel 1.0.2'yi kullanmak için uygulama bağımlılıklarınızı güncellediğinizden emin olun. Alternatif olarak, [App Service web uygulamalarında Core 1.0 yavaş perf sorunları ASP.NET](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites)blog gönderisinde açıklanan iki geçici çözümden birini kullanabilirsiniz.


## <a name="i-cant-find-my-log-files-in-the-file-structure-of-my-web-app-how-can-i-find-them"></a>Günlük dosyalarımı web uygulamamın dosya yapısında bulamıyorum. Onları nasıl bulabilirim?

Uygulama Hizmetinin Yerel Önbellek özelliğini kullanırsanız, Uygulama Hizmeti örneğiniz için LogFiles ve Veri klasörlerinin klasör yapısı etkilenir. Yerel Önbellek kullanıldığında, depolama LogFiles ve Veri klasörlerinde alt klasörler oluşturulur. Alt klasörler adlandırma deseni "benzersiz tanımlayıcı" + zaman damgası kullanın. Her alt klasör, web uygulamasının çalıştırıldığı veya çalıştırıldığı bir VM örneğine karşılık gelir.

Yerel Önbellek kullanıp kullanmadığınızı belirlemek için Uygulama Hizmeti **Uygulaması ayarları** sekmenizi kontrol edin. Yerel Önbellek kullanılıyorsa, uygulama `WEBSITE_LOCAL_CACHE_OPTION` ayarı `Always`.

Yerel Önbellek kullanmıyorsanız ve bu sorunla karşıyayım, bir destek isteği gönderin.

## <a name="i-see-the-message-an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>"Bir sokete erişim izinleri tarafından yasaklanmış bir şekilde erişmeye çalışılmıştır" mesajını görüyorum. Bu sorunu nasıl çözebilirim?

Bu hata genellikle VM örneğindeki giden TCP bağlantıları tükenmişse oluşur. Uygulama Hizmeti'nde, her VM örneği için yapIlebilen maksimum giden bağlantı sayısı için sınırlar uygulanır. Daha fazla bilgi için [Cross-VM sayısal sınırlarına](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits)bakın.

Uygulamanızdan yerel bir adrese erişmeye çalışırsanız, bu hata da oluşabilir. Daha fazla bilgi için [Yerel adres isteklerine](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests)bakın.

Web uygulamanızdaki giden bağlantılar hakkında daha fazla bilgi için [Azure web sitelerine giden bağlantılar](https://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/)la ilgili blog gönderisine bakın.

## <a name="how-do-i-use-visual-studio-to-remote-debug-my-app-service-web-app"></a>Uygulama Hizmeti web uygulamamı uzaktan hata ayıklamak için Visual Studio'yı nasıl kullanırım?

Visual Studio'u kullanarak web uygulamanızı nasıl hata ayıklamanız gerektiğini gösteren ayrıntılı bir gözden geçirme için, [Uygulama Hizmeti web uygulamanızı uzaktan ayıklama](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/)bölümüne bakın.
