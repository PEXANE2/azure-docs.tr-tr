---
title: Azure Uygulama Öngörüleri Profil oluşturucu ayarları bölmesini kullanın | Microsoft Dokümanlar
description: Profiler durumuna bakın ve profil oluşturma oturumlarını başlatın
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: f66a23c0562ec9f1987bd119a45b7b767f2dfe46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671639"
---
# <a name="configure-application-insights-profiler"></a>Uygulama Öngörülerini Profil oluştur

## <a name="updated-profiler-agent"></a>Güncelleştirilmiş Profiler Aracısı
Tetikleyici özellikleri yalnızca sürüm 2.6 veya profil oluşturucu aracısının daha yeni sürümüyle çalışır. Bir Azure Uygulama Hizmeti çalıştırıyorsanız, aracınız otomatik olarak güncelleştirilir. Web sitenizin Kudu URL'sine giderseniz ve sonuna kadar \DiagnosticServices eklerseniz, çalıştırdığınız aracının hangi https://yourwebsite.scm.azurewebsites.net/diagnosticservicessürümünü görebilirsiniz: . Uygulama Öngörüleri Profiler Webjob sürüm 2.6 veya daha yeni olmalıdır. Web uygulamanızı yeniden başlatarak yükseltmeyi zorlayabilirsiniz. 

Profil oluşturucuyu bir VM veya Bulut Hizmeti'nde çalıştırıyorsanız, Windows Azure Diagnostics (WAD) uzantısı sürümü 16.0.4 veya daha yeni yüklü olması gerekir. VM'nize giriş yaparak ve şu dizine bakarak WAD sürümünü denetleyebilirsiniz: C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.16.0.4. Dizin adı WAD'ın yüklenen sürümüdür. Azure VM aracısı, yeni sürümler kullanılabilir olduğunda WAD'yi otomatik olarak günceller.

## <a name="profiler-settings-page"></a>Profiler ayarları sayfası

Azure Application Insights Profiler ayarları bölmesini açmak için Application Insights Performance bölmesine gidin ve ardından **Profil Oluşturucuyu Yapılandır** düğmesini seçin.

![Profiler ayarları sayfasını açmak için bağlantı][configure-profiler-entry]

Bu, şuna benzer bir sayfa açar:

![Profiler ayarları sayfası][configure-profiler-page]

**Yapılandırılan Uygulama Öngörüleri Profiler** sayfasında şu özellikler vardır:

| | |
|-|-|
Profili Şimdi | Bu Uygulama Öngörüleri örneğine bağlı tüm uygulamalar için profil oluşturma oturumları başlatır.
Tetikleyiciler | Profilleyicinin çalışmasına neden olan tetikleyicileri yapılandırmanızı sağlar. 
Son profil oluşturma oturumları | Geçmiş profil oluşturma oturumları hakkındaki bilgileri görüntüler.

## <a name="profile-now"></a>Profili Şimdi
Bu seçenek, isteğe bağlı bir profil oluşturma oturumu başlatmanızı sağlar. Bu bağlantıyı tıklattığınızda, bu Uygulama Öngörüleri örneğine veri gönderen tüm profil oluşturucu aracılar bir profil yakalamaya başlar. 5 ila 10 dakika sonra profil oturumu aşağıdaki listede gösterecektir.

Bir kullanıcının bir profil oluşturucu oturumunu el ile tetiklemesi için, Uygulama Öngörüleri bileşeniiçin rollerine en az "yazma" erişimi gerekir. Çoğu durumda, bu erişimi otomatik olarak alırsınız ve ek bir çalışma gerekmez. Sorun yaşıyorsanız, eklenecek abonelik kapsamı rolü "Uygulama Öngörüleri Bileşeni Katılımcısı" rolü olacaktır. [Azure İzleme ile rol erişim denetimi hakkında daha fazla bilgi](https://docs.microsoft.com/azure/azure-monitor/app/resources-roles-access-control)edinin.

## <a name="trigger-settings"></a>Tetikleyici Ayarları
![Tetik Ayarları Flyout][trigger-settings-flyout]

Menü çubuğundaki Tetikleyiciler düğmesini tıklattığınızda tetikleyici ayarlar kutusunu açar. CPU veya Bellek kullanımı yüzdesi ayarladığınız düzeye ulaştığında profil oluşturmayı başlatmak için tetikleyici ayarlayabilirsiniz.

| | |
|-|-|
Açma / Kapatma Düğmesi | On: profil oluşturucu bu tetikleyici tarafından başlatılabilir; Kapalı: profil oluşturucu bu tetikleyici tarafından başlatılmaz.
Bellek eşiği | Bu bellek yüzdesi kullanımda olduğunda, profil oluşturucu başlatılır.
Süre | Tetiklendiğinde profiloluşturucunun çalışacağı süreyi ayarlar.
Cooldown | Profiloluşturucunun, bellek veya CPU kullanımını tetiklendikten sonra tekrar denetlemeden önce bekleyeceği süreyi ayarlar.

## <a name="recent-profiling-sessions"></a>Son Profil Oluşturma Oturumları
Sayfanın bu bölümünde son profil oluşturma oturumları hakkında bilgi gösterilmektedir. Profil oluşturma oturumu, profil oluşturucu aracının uygulamanızı barındıran makinelerden birinde profil aldığı süreyi gösterir. Satırlardan birine tıklayarak bir oturumdaki profilleri açabilirsiniz. Her oturum için şunları gösteririz:

| | |
|-|-|
Tarafından tetiklenen | Selem, tetikleyici, Profil Şimdi veya varsayılan örnekleme ile nasıl başlatıldığını. 
Uygulama Adı | Profili olan uygulamanın adı.
Makine Örneği | Profilci ajanının çalıştırdAşık makinesinin adı.
Zaman damgası | Profilin yakalandığı zaman.
Tracee | Tek tek isteklere eklenen iz sayısı.
CPU % | Profil oluşturucu çalışırken kullanılan CPU yüzdesi.
Bellek % | Profil oluşturucu çalışırken kullanılan bellek yüzdesi.

## <a name="use-web-performance-tests-to-generate-traffic-to-your-application"></a><a id="profileondemand"></a>Uygulamanız için trafik oluşturmak için web performans testlerini kullanma

Profiler'ı tek bir tıklamayla el ile tetikleyebilirsiniz. Bir web performans testi çalıştırdığınızı varsayalım. Web uygulamanızın nasıl yük altında çalıştığını anlamanıza yardımcı olmak için izlemelere ihtiyacınız vardır. İzlemelerin ne zaman yakalandığını denetlemeye sahip olmak çok önemlidir, çünkü yük testinin ne zaman çalışır olacağını bilirsiniz. Ama rasgele örnekleme aralığı bunu kaçırabilir.

Sonraki bölümlerde bu senaryonun nasıl çalıştığı gösteriş verilmiştir:

### <a name="step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>Adım 1: Web performans testi başlatarak web uygulamanıza trafik oluşturun

Web uygulamanızda zaten gelen trafik varsa veya yalnızca manuel olarak trafik oluşturmak istiyorsanız, bu bölümü atlayın ve Adım 2'ye devam edin.

1. Application Insights portalında > **Performans** **Testini Yapılandır'ı**seçin. 

1. Yeni bir performans testi başlatmak için **Yeni** düğmesini seçin.

   ![yeni performans testi oluşturma][create-performance-test]

1. Yeni **performans testi** bölmesinde, test hedef URL'sini yapılandırın. Tüm varsayılan ayarları kabul edin ve ardından yük testini çalıştırmaya başlamak için **Testi Çalıştır'ı** seçin.

    ![Yük testini yapılandır][configure-performance-test]

    Yeni test önce sıraya alınır ve ardından *devam eden*bir durum vardır.

    ![Yük testi gönderilir ve sıraya alınır][load-test-queued]

    ![Yük testi devam ediyor][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>Adım 2: Profiler isteğe bağlı oturum başlatma

1. Yükleme testi çalışırken Profiler'ı başlatArak yük alırken web uygulamasındaki izleri yakalayın.

1. **Profil Oluşturucuyu Yapılandır** bölmesine gidin.


### <a name="step-3-view-traces"></a>Adım 3: İzlemeleri görüntüle

Profiler çalışma larını tamamladıktan sonra, Performans bölmesine gitmek ve izlemeleri görüntülemek için bildirimdeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Profiler'ı etkinleştirme ve izlemeleri görüntüleme](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

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
