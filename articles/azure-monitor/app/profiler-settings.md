---
title: Azure Application Insights Profiler ayarları bölmesini kullanma | Microsoft Docs
description: Profil Oluşturucu durumunu görün ve profil oluşturma oturumlarını başlatın
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 6b6f98f3c7f540e00743ae27b532b24d2310453e
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86205714"
---
# <a name="configure-application-insights-profiler"></a>Application Insights Profiler Yapılandır

## <a name="updated-profiler-agent"></a>Profil Oluşturucu aracısı güncelleştirildi
Tetikleyici özellikleri, profil oluşturucu aracısının yalnızca sürüm 2,6 veya daha yeni bir sürümüyle çalışır. Bir Azure App Service çalıştırıyorsanız, aracınız otomatik olarak güncelleştirilir. Web sitenizin kudu URL 'sine giderseniz ve aşağıdaki gibi \DiagnosticServices öğesini sonuna eklemek için aracının hangi sürümünü çalıştırdığınızı görebilirsiniz: `https://yourwebsite.scm.azurewebsites.net/diagnosticservices` . Application Insights Profiler WebJob sürümü 2,6 veya daha yeni bir sürüm olmalıdır. Web uygulamanızı yeniden başlatarak yükseltmeyi zorlayabilirsiniz. 

Profil oluşturucuyu bir VM veya bulut hizmetinde çalıştırıyorsanız, Windows Azure Tanılama (WAD) uzantısı sürüm 16.0.4 veya daha yeni bir sürümü yüklü olmalıdır. SANAL makinenizde oturum açarak ve şu dizine bakarak WAD 'nin sürümünü denetleyebilirsiniz: C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.16.0.4. Dizin adı, yüklü olan WAD 'nin sürümüdür. Yeni sürümler kullanılabilir olduğunda, Azure VM Aracısı WAD 'yi otomatik olarak güncelleştirir.

## <a name="profiler-settings-page"></a>Profil Oluşturucu ayarları sayfası

Azure Application Insights Profiler ayarları bölmesini açmak için Application Insights performans bölmesine gidin ve **profil oluşturucuyu Yapılandır** düğmesini seçin.

![Profil Oluşturucu ayarlarını açma sayfasına bağlantı][configure-profiler-entry]

Bu, şöyle görünen bir sayfa açar:

![Profil Oluşturucu ayarları sayfası][configure-profiler-page]

**Application Insights Profiler Yapılandır** sayfası şu özelliklere sahiptir:

| Özellik | Açıklama |
|-|-|
Şimdi profil | Bu Application Insights örneğiyle bağlantılı tüm uygulamalar için profil oluşturma oturumlarını başlatır.
Tetikleyiciler | Profil oluşturucunun çalışmasına neden olan Tetikleyicileri yapılandırmanızı sağlar. 
Son profil oluşturma oturumları | Geçmiş profil oluşturma oturumları hakkındaki bilgileri görüntüler.

## <a name="profile-now"></a>Şimdi profil
Bu seçenek, isteğe bağlı olarak profil oluşturma oturumu başlatabilmeniz için izin verir. Bu bağlantıya tıkladığınızda, bu Application Insights örneğine veri gönderen tüm profil oluşturucu aracıları bir profili yakalamaya başlayacaktır. 5 ila 10 dakika sonra profil oturumu aşağıdaki listede görünür.

Bir kullanıcının profil oluşturucu oturumunu el ile tetiklemesi için, Application Insights bileşeni için rolünde en az "yazma" erişiminin olması gerekir. Çoğu durumda, bu erişimi otomatik olarak alır ve ek bir iş gerekmez. Sorun yaşıyorsanız, eklenecek abonelik kapsamı rolü "Application Insights bileşen katılımcısı" rolü olacaktır. [Azure izleme ile rol erişimi denetimi hakkında daha fazla](https://docs.microsoft.com/azure/azure-monitor/app/resources-roles-access-control)bilgi için bkz..

## <a name="trigger-settings"></a>Tetikleyici ayarları
![Tetikleyici ayarları açılır menüsü][trigger-settings-flyout]

Menü çubuğundaki Tetikleyiciler düğmesine tıkladığınızda tetikleyici ayarları kutusu açılır. CPU veya bellek kullanımı yüzdesi ayarladığınız düzeyi ziyaret edildiğinde profil oluşturmayı başlatacak şekilde tetikleyebilirsiniz.

| Ayar | Açıklama |
|-|-|
Açık/kapalı düğmesi | Açık: Profiler, bu tetikleyici tarafından başlatılabilir; Kapalı: Profil Oluşturucu bu tetikleyici tarafından başlatılmayacak.
Bellek eşiği | Bu bellek yüzdesi kullanımda olduğunda, profil oluşturucu başlatılır.
Süre | Profiler 'ın tetiklendiğinde çalışacağı sürenin uzunluğunu ayarlar.
Coolaşağı | Profil oluşturucunun, bellek veya CPU kullanımını, tetiklendikten sonra tekrar denetlemeden önce bekleyeceği sürenin uzunluğunu ayarlar.

## <a name="recent-profiling-sessions"></a>Son profil oluşturma oturumları
Sayfanın bu bölümü, son profil oluşturma oturumları hakkında bilgi gösterir. Profil oluşturma oturumu, profil oluşturucu aracısının uygulamanızı barındıran makinelerden birinde bir profil aldığı zaman dilimini temsil eder. Satırlardan birine tıklayarak profilleri bir oturumdan açabilirsiniz. Her oturum için şunu göstereceğiz:

| Ayar | Açıklama |
|-|-|
Tetikleyen | Oturumun nasıl başlatıldığı, bir tetikleyici, şimdi profil veya varsayılan örnekleme. 
Uygulama Adı | Profili oluşturulan uygulamanın adı.
Makine örneği | Profil Oluşturucu aracısının çalıştırıldığı makinenin adı.
Timestamp | Profilin yakalandığı zaman.
Tracee | Bireysel isteklere eklenmiş izleme sayısı.
CPU % | Profil Oluşturucu çalışırken kullanılmakta olan CPU yüzdesi.
Bellek | Profil Oluşturucu çalışırken kullanılmakta olan bellek yüzdesi.

## <a name="use-web-performance-tests-to-generate-traffic-to-your-application"></a><a id="profileondemand"></a>Uygulamanıza trafik oluşturmak için Web performans testlerini kullanın

Profil oluşturucuyu tek bir tıklama ile el ile tetikleyebilirsiniz. Web performans testi kullandığınızı varsayalım. Web uygulamanızın yük altında nasıl çalıştığını anlamanıza yardımcı olması için izlemeler yapmanız gerekir. İzleme, yük testinin ne zaman çalıştığını bildiğiniz için çok önemlidir. Ancak rastgele örnekleme aralığı onu kaçırmayabilir.

Sonraki bölümlerde bu senaryonun nasıl çalıştığı gösterilmektedir:

### <a name="step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>1. Adım: Web performans testini başlatarak Web uygulamanıza trafik oluşturma

Web uygulamanızda zaten gelen trafik varsa veya yalnızca el ile trafik oluşturmak istiyorsanız, bu bölümü atlayın ve adım 2 ' ye geçin.

1. Application Insights portalında, **Configure**  >  **performans testini**Yapılandır ' ı seçin. 

1. Yeni bir performans testi başlatmak için **Yeni** düğmesini seçin.

   ![Yeni performans testi oluştur][create-performance-test]

1. **Yeni performans testi** bölmesinde, test hedefi URL 'sini yapılandırın. Tüm varsayılan ayarları kabul edin ve ardından Yük testini çalıştırmaya başlamak için **Testi Çalıştır** ' ı seçin.

    ![Yük testini yapılandırma][configure-performance-test]

    Yeni test önce kuyruğa alınır ve sonrasında *ilerleme*durumu gelir.

    ![Yük testi gönderildi ve kuyruğa alındı][load-test-queued]

    ![Yük testi devam ediyor][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>2. Adım: Profil Oluşturucu isteğe bağlı oturumu başlatma

1. Yük testi çalışırken, yük alırken Web uygulamasındaki izlemeleri yakalamak için profil oluşturucuyu başlatın.

1. **Profil oluşturucuyu Yapılandır** bölmesine gidin.


### <a name="step-3-view-traces"></a>3. Adım: izlemeleri görüntüleme

Profil Oluşturucu çalışmayı tamamladıktan sonra, performans bölmesine gitmek ve izlemeleri görüntülemek için bildirim sayfasındaki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Profil oluşturucuyu etkinleştirme ve izlemeleri görüntüleme](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[configure-profiler-page]: ./media/profiler-settings/configureBlade.png
[trigger-settings-flyout]: ./media/profiler-settings/CPUTrigger.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
