---
title: Tanılama ve çözme aracı
description: Azure portalındaki tanılama ve çözüm aracıyla Azure Uygulama Hizmeti'ndeki uygulamanızla ilgili sorunları nasıl giderebileceğinizi öğrenin.
keywords: uygulama hizmeti, azure uygulama hizmeti, tanılama, destek, web uygulaması, sorun giderme, kendi kendine yardım
author: jen7714
ms.topic: article
ms.date: 10/18/2019
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: df8f68e47776f46ae2d1331f85bd76d6cfe17b80
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869936"
---
# <a name="azure-app-service-diagnostics-overview"></a>Azure Uygulama Hizmeti tanılama genel bakış

Bir web uygulaması çalıştırırken, 500 hatadan kullanıcılarınızın sitenizin çöktüğını söylemesine kadar ortaya çıkabilecek sorunlara karşı hazırlıklı olmak istersiniz. Uygulama Hizmeti tanılama, yapılandırma gerektirmeden uygulamanızı sorun gidermenize yardımcı olan akıllı ve etkileşimli bir deneyimdir. Uygulamanızla ilgili sorunlarla karşılaştığınızda, Uygulama Hizmeti tanılama, sorunu daha kolay ve hızlı bir şekilde sorun giderme ve çözmeniz için doğru bilgilere rehberlik etmek için neyin yanlış olduğunu işaret eder.

Bu deneyim, son 24 saat içinde uygulamanızla ilgili sorunlar yaşıyorsanız en yararlı olsa da, tüm tanılama grafiklerini analiz etmek için her zaman kullanılabilir.

Uygulama Hizmeti tanılama, yalnızca Windows'daki uygulamanız için değil, [Linux/kapsayıcılar,](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) [Uygulama Hizmet Ortamı](https://docs.microsoft.com/azure/app-service/environment/intro)ve Azure [İşlerinden](https://docs.microsoft.com/azure/azure-functions/functions-overview)başlayan uygulamalar için de çalışır.

## <a name="open-app-service-diagnostics"></a>Uygulama Hizmeti tanılamayı aç

Uygulama Hizmeti tanılamalarına erişmek için [Azure portalındaki](https://portal.azure.com)Uygulama Hizmeti web uygulamanıza veya Uygulama Hizmeti Ortamınıza gidin. Sol navigasyonda **Tanıla'ya**tıklayın ve sorunları çözün.

Azure İşlevler için işlev uygulamanıza gidin ve üst gezintide **Platform özelliklerini**tıklayın ve **Kaynak yönetimi** bölümünden **Tanıla ve sorunları çöz'ü** seçin.

App Service tanılama ana sayfasında, her ana sayfa döşemesinde anahtar kelimeleri kullanarak uygulamanızla ilgili sorunu en iyi açıklayan kategoriyi seçebilirsiniz. Ayrıca, bu sayfa Windows uygulamaları için **Tanılama Araçları'nı** bulabileceğiniz yerdir. [Tanılama araçları (yalnızca Windows uygulaması için)](#diagnostic-tools-only-for-windows-app)bakın.

![Giriş sayfası](./media/app-service-diagnostics/app-service-diagnostics-homepage-1.png)

> [!NOTE]
> Uygulamanız kapanıyorsa veya yavaş performans gösteriyorsa, sorunun temel nedenini belirlemek için [bir profil oluşturma izleme sini toplayabilirsiniz.](https://azure.github.io/AppService/2018/06/06/App-Service-Diagnostics-Profiling-an-ASP.NET-Web-App-on-Azure-App-Service.html) Profil oluşturma hafiftir ve üretim senaryoları için tasarlanmıştır.
>

## <a name="interactive-interface"></a>Etkileşimli arayüz

Uygulamanızın sorunuyla en iyi hizalayan bir ana sayfa kategorisi seçtiğinizde, App Service tanılamanın etkileşimli arayüzü Cin, uygulamanızla ilgili sorunları tanılama ve çözme konusunda size rehberlik edebilir. İlgilendiğiniz sorun kategorisinin tam tanılama raporunu görüntülemek için Cin tarafından sağlanan döşeme kısayollarını kullanabilirsiniz. Döşeme kısayolları, tanılama ölçümlerinize doğrudan erişmenizi sağlar.

![Döşeme kısayolları](./media/app-service-diagnostics/tile-shortcuts-2.png)

Bu kutucuklara tıkladıktan sonra, kutucukta açıklanan sorunla ilgili konuların listesini görebilirsiniz. Bu konular, raporun tamamından kayda değer bilgiler sağlar. Sorunları daha fazla araştırmak için bu konulardan herhangi birini tıklayabilirsiniz. Ayrıca, tek bir sayfadaki tüm konuları incelemek için **Tam Raporu Görüntüle'yi** tıklatabilirsiniz.

![Konu başlıkları](./media/app-service-diagnostics/application-logs-insights-3.png)

![Tam Raporu Görüntüle](./media/app-service-diagnostics/view-full-report-4.png)

## <a name="diagnostic-report"></a>Tanı lama raporu

Bir konuya tıklayarak konuyu daha fazla araştırmayı seçtikten sonra, genellikle grafikler ve işaretlemelerle desteklenen konu yla ilgili daha fazla ayrıntıyı görüntüleyebilirsiniz. Tanılama raporu, uygulamanızla ilgili sorunu saptamak için güçlü bir araç olabilir.

![Tanı lama raporu](./media/app-service-diagnostics/full-diagnostic-report-5.png)

## <a name="health-checkup"></a>Sağlık kontrolü

Uygulamanızda sorunun ne olduğunu bilmiyorsanız veya sorun gidermeye nereden başlayacağınızı bilmiyorsanız, sağlık kontrolü başlamak için iyi bir yerdir. Sağlık kontrolü, size neyin sağlıklı ve neyin yanlış olduğunu gösteren hızlı ve etkileşimli bir genel bakış sunmak için uygulamalarınızı analiz eder ve sorunu nerede araştırmanız gerektiğini söyler. Akıllı ve etkileşimli arayüzü, sorun giderme sürecinde size rehberlik sağlar. Sağlık kontrolü, Windows uygulamaları için Cin deneyimi ve Linux uygulamaları için tanılama raporunu aşağı web uygulamasıyla entegre edilmiştir.

### <a name="health-checkup-graphs"></a>Sağlık kontrol grafikleri

Sağlık kontrolünde dört farklı grafik vardır.

- **istekler ve hatalar:** SON 24 saat içinde yapılan istek sayısını VE HTTP sunucu hatalarını gösteren bir grafik.
- **uygulama performansı:** Çeşitli yüzdelik gruplar için son 24 saat içinde yanıt süresini gösteren bir grafik.
- **CPU kullanımı:** Son 24 saat içinde örnek başına genel yüzde CPU kullanımını gösteren bir grafik.  
- **bellek kullanımı:** Son 24 saat içinde örnek başına genel yüzde fiziksel bellek kullanımını gösteren bir grafik.

![Sağlık kontrolü](./media/app-service-diagnostics/health-checkup-6.png)

### <a name="investigate-application-code-issues-only-for-windows-app"></a>Uygulama kodu sorunlarını araştırma (yalnızca Windows uygulaması için)

Birçok uygulama sorunu uygulama kodunuzdaki sorunlarla ilgili olduğundan, Uygulama Hizmeti tanılama, özel durumları ve bağımlılık sorunlarını seçili kapalı kalma süresiyle ilişkilendirmek için vurgulamak için [Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) ile tümleşir. Uygulama Öngörüleri ayrı olarak etkinleştirilmelidir.

![Application Insights](./media/app-service-diagnostics/application-insights-7.png)

Uygulama Öngörüleri özel durumlarını ve bağımlılıklarını görüntülemek için **web uygulamasını aşağı** veya web uygulaması **yavaş** döşeme kısayollarını seçin.

### <a name="troubleshooting-steps-only-for-windows-app"></a>Sorun giderme adımları (yalnızca Windows uygulaması için)

Son 24 saat içinde belirli bir sorun kategorisi yle ilgili bir sorun algılanırsa, tam tanılama raporunu görüntüleyebilirsiniz ve Uygulama Hizmeti tanılama daha fazla sorun giderme tavsiyesini ve daha yönlendirilmiş bir deneyim için sonraki adımları görüntülemenizi isteyebilir.

![Uygulama Öngörüleri ve Sorun Giderme ve Sonraki Adımlar](./media/app-service-diagnostics/troubleshooting-and-next-steps-8.png)

## <a name="diagnostic-tools-only-for-windows-app"></a>Tanılama araçları (yalnızca Windows uygulaması için)

Tanılama Araçları, uygulama kodu sorunlarını, yavaşlığı, bağlantı dizelerini ve daha fazlasını araştırmanıza yardımcı olan daha gelişmiş tanılama araçları içerir. ve CPU kullanımı, istekleri ve bellekle ilgili sorunları azaltmanıza yardımcı olan proaktif araçlar.

### <a name="proactive-cpu-monitoring"></a>Proaktif CPU izleme

Proaktif CPU izleme, uygulamanız veya alt işleminiz yüksek CPU kaynaklarını tüketirken bir eylemde bulunmanız için kolay ve proaktif bir yol sağlar. Beklenmeyen sorunun gerçek nedeni bulunana kadar yüksek CPU sorununu geçici olarak azaltmak için kendi CPU eşik kurallarınızı ayarlayabilirsiniz. Daha fazla bilgi için [cpu sorunlarınızı gerçekleşmeden önce azaltı'ya](https://azure.github.io/AppService/2019/10/07/Mitigate-your-CPU-problems-before-they-even-happen.html)bakın.

![Proaktif CPU izleme](./media/app-service-diagnostics/proactive-cpu-monitoring-9.png)

### <a name="auto-healing-and-proactive-auto-healing"></a>Otomatik iyileşme ve proaktif otomatik iyileşme

Otomatik iyileştirme, uygulamanız beklenmeyen davranışlarda yken yapabileceğiniz bir azaltma eylemidir. Azaltma eylemlerini tetiklemek için istek sayısı, yavaş istek, bellek sınırı ve HTTP durum kodunu temel alan kendi kurallarınızı ayarlayabilirsiniz. Temel nedeni bulana kadar beklenmeyen bir davranışı geçici olarak azaltmak için aracı kullanın. Daha fazla bilgi için bkz: [Uygulama hizmeti tanılamada yeni otomatik iyileştirme deneyimini duyurmak.](https://azure.github.io/AppService/2018/09/10/Announcing-the-New-Auto-Healing-Experience-in-App-Service-Diagnostics.html)

![Otomatik iyileşme](./media/app-service-diagnostics/auto-healing-10.png)

Proaktif CPU izleme gibi, proaktif otomatik iyileştirme de uygulamanızın beklenmeyen davranışlarını hafifletmek için anahtar teslimi bir çözümdür. Uygulama Hizmeti uygulamanızın kurtarılamaz durumda olduğunu belirlediğinde, proaktif otomatik iyileştirme uygulamanızı yeniden başlatır. Daha fazla bilgi için, [Proaktif Auto Heal Tanıtın](https://azure.github.io/AppService/2017/08/17/Introducing-Proactive-Auto-Heal.html)bakın.

## <a name="navigator-and-change-analysis-only-for-windows-app"></a>Gezinme ve değişiklik çözümlemesi (yalnızca Windows uygulaması için)

Sürekli tümleştirmesi olan ve uygulamanızın birçok bağımlılığı olan büyük bir ekipte, sağlıksız bir davranışa neden olan belirli değişikliği saptamak zor olabilir. Navigator, uygulamanızın bağımlılık haritasını ve aynı abonelikteki tüm kaynakları otomatik olarak oluşturarak uygulamanızın topolojisinde görünürlük elde edilebilmenize yardımcı olur. Gezgin, uygulamanız ve bağımlılıkları tarafından yapılan değişikliklerin birleştirilmiş listesini görüntülemenize ve sağlıksız davranışlara neden olan bir değişikliği daraltmamanızı sağlar. Ana sayfa **döşemesi Navigator** üzerinden erişilebilir ve ilk kez kullanmadan önce etkin olması gerekir. Daha fazla bilgi için bkz. [Uygulamanızın Navigator'a olan bağımlılıkları hakkında görünürlük elde](https://azure.github.io/AppService/2019/08/06/Bring-visibility-to-your-app-and-its-dependencies-with-Navigator.html)edin.

![Gezgin varsayılan sayfası](./media/app-service-diagnostics/navigator-default-page-11.png)

![Diff görünümü](./media/app-service-diagnostics/diff-view-12.png)

Uygulama değişiklikleri için değişiklik analizine, **kullanılabilirlik ve performansta** kiremit kısayolları, **Uygulama Değişiklikleri** ve **Uygulama Kilitlenmeleri** yoluyla erişilebilir, böylece diğer ölçümlerle aynı anda kullanabilirsiniz. Özelliği kullanmadan önce etkinleştirmeniz gerekir. Daha fazla bilgi için bkz: [Uygulama Hizmeti Tanılama'da yeni değişiklik analizi deneyimini duyurun.](https://azure.github.io/AppService/2019/05/07/Announcing-the-new-change-analysis-experience-in-App-Service-Diagnostics-Analysis.html)

Başlığına "[Diag]" ekleyerek sorularınızı veya geri bildirimlerinizi [UserVoice'ta](https://feedback.azure.com/forums/169385-web-apps) yayınlayın.
