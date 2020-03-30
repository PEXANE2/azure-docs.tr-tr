---
title: HTTP 502 ve HTTP 503 hatalarını düzeltin
description: Azure Uygulama Hizmeti'nde barındırılan uygulamanızda 502 kötü ağ geçidi ve 503 hizmet kullanılamayan hataları giderin.
tags: top-support-issue
keywords: 502 kötü ağ geçidi, 503 hizmet kullanılamıyor, hata 503, hata 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.topic: article
ms.date: 07/06/2016
ms.custom: seodec18
ms.openlocfilehash: 9345b6fb28aa282e85f1167f6f2531e5f990e3a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688320"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde HTTP hatalarını "502 kötü ağ geçidi" ve "503 hizmeti kullanılamıyor" hatalarını giderme
"502 kötü ağ geçidi" ve "503 hizmeti kullanılamıyor" [Azure Uygulama Hizmeti'nde](https://go.microsoft.com/fwlink/?LinkId=529714)barındırılan uygulamanızda sık karşılaşılan hatalardır. Bu makale, bu hataları gidermenize yardımcı olur.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**tıklatın.

## <a name="symptom"></a>Belirti
Uygulamaya göz attığınızda, bir HTTP "502 Bad Gateway" hatası veya HTTP "503 Hizmet Kullanılamıyor" hatasını döndürür.

## <a name="cause"></a>Nedeni
Bu sorun genellikle uygulama düzeyi sorunları, örneğin neden olur:

* istekleri uzun zaman alıyor
* yüksek bellek/CPU kullanarak uygulama
* uygulama bir istisna nedeniyle çöküyor.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>"502 kötü ağ geçidi" ve "503 hizmet kullanılamıyor" hatalarını çözmek için sorun giderme adımları
Sorun giderme, sırayla üç ayrı görev olarak ayrılabilir:

1. [Uygulama davranışını gözlemleme ve izleme](#observe)
2. [Veri toplama](#collect)
3. [Sorunu azletmek](#mitigate)

[App Service](overview.md) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Uygulama davranışını gözlemlemek ve izlemek
#### <a name="track-service-health"></a>Servis durumunu takip et
Microsoft Azure, bir hizmet kesintisi veya performans bozulması olduğunda her kez genelkullanıma sunar. [Azure Portalı'nda](https://portal.azure.com/)hizmetin durumunu takip edebilirsiniz. Daha fazla bilgi için [bkz.](../monitoring-and-diagnostics/insights-service-health.md)

#### <a name="monitor-your-app"></a> Uygulamanızı izleme
Bu seçenek, uygulamanızda herhangi bir sorun olup olmadığını öğrenmenizi sağlar. Uygulamanızın **kılıcında, İstekler ve hatalar** döşemesini tıklatın. **Metrik** bıçak ekleyebileceğiniz tüm ölçümleri gösterir.

Uygulamanız için izlemek isteyebileceğiniz ölçümlerden bazıları şunlardır:

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![502 kötü ağ geçidi ve 503 hizmet kullanılamaz HTTP hataları çözmeye yönelik izleme uygulaması](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure Uygulama Hizmeti'ndeki uygulamaları izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
#### <a name="use-the-diagnostics-tool"></a>Tanılama aracını kullanma
Uygulama Hizmeti, yapılandırma gerektirmeden uygulamanızı sorun gidermenize yardımcı olmak için akıllı ve etkileşimli bir deneyim sağlar. Uygulamanızla ilgili sorunlarla karşılaştığınızda, tanılama aracı sorunu daha kolay ve hızlı bir şekilde gidermek ve çözmek için doğru bilgilere rehberlik etmek için neyin yanlış olduğunu belirtecektir.

Uygulama Hizmeti tanılamalarına erişmek için [Azure portalındaki](https://portal.azure.com)Uygulama Hizmeti uygulamanıza veya Uygulama Hizmeti Ortamınıza gidin. Sol navigasyonda **Tanıla'ya**tıklayın ve sorunları çözün.

#### <a name="use-the-kudu-debug-console"></a>Kudu Hata Ayıklama Konsolu'nu kullanma
Uygulama Hizmeti, hata ayıklama, araştırma, dosya yükleme gibi hata ayıklama konsolunun yanı sıra ortamınız hakkında bilgi almak için JSON uç noktalarıyla birlikte gelir. Buna *Kudu Konsolu* veya uygulamanız için *SCM Panosu* denir.

Bu panoya **uygulama adınız&lt;>.scm.azurewebsites.net/ https://** linkine giderek erişebilirsiniz.

Kudu'nun sağladığı şeylerden bazıları şunlardır:

* uygulamanız için ortam ayarları
* günlük akışı
* tanılama dökümü
* Powershell cmdlets ve temel DOS komutları çalıştırabilirsiniz hata ayıklama konsolu.

Kudu'nun bir diğer yararlı özelliği de, uygulamanızın ilk şans özel durumları atması durumunda, bellek dökümleri oluşturmak için Kudu ve SysInternals aracı Procdump'ı kullanabilirsiniz. Bu bellek dökümleri işlemin anlık görüntüleridir ve genellikle uygulamanızla ilgili daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu'da bulunan özellikler hakkında daha fazla bilgi için Azure [Web Siteleri çevrimiçi araçları hakkında bilgi edinmeniz gereken araçlara](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/)bakın.

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Sorunu azaltmak
#### <a name="scale-the-app"></a>Uygulamayı ölçeklendirin
Azure Uygulama Hizmeti'nde, artan performans ve iş artışı için, uygulamanızı çalıştırdığınız ölçeği ayarlayabilirsiniz. Bir uygulamayı ölçeklendirmek iki ilgili eylem içerir: Uygulama Hizmeti planınızı daha yüksek bir fiyatlandırma katmanına değiştirmek ve daha yüksek fiyatlandırma katmanına geçtikten sonra belirli ayarları yapılandırmak.

Ölçeklendirme hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'nde bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın.

Ayrıca, uygulamanızı birden fazla örnekte çalıştırmayı seçebilirsiniz. Bu sadece daha fazla işlem yeteneği sağlar, ama aynı zamanda hata toleransı bir miktar verir. İşlem bir durumda aşağı giderse, diğer örnek yine de isteklere hizmet vermeye devam eder.

Ölçekleme'yi Manuel veya Otomatik olarak ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal'ı kullan
AutoHeal, seçtiğiniz ayarlara (yapılandırma değişiklikleri, istekler, bellek tabanlı sınırlar veya bir isteği yürütmek için gereken süre) bağlı olarak uygulamanız için alt işlemi geri dönüştürür. Çoğu zaman, işlemi geri dönüştürme bir sorundan kurtarmak için en hızlı yoldur. Uygulamayı doğrudan Azure Portalı'ndan yeniden başlatabilirsiniz, ancak AutoHeal bunu sizin için otomatik olarak yapar. Yapmanız gereken tek şey, uygulamanız için root web.config bazı tetikleyiciler eklemektir. Uygulamanız bir .NET olmasa bile bu ayarların aynı şekilde çalışacağını unutmayın.

Daha fazla bilgi için Bkz. [Otomatik İyileştirme Azure Web Siteleri.](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/)

#### <a name="restart-the-app"></a>Uygulamayı yeniden başlatın
Bu genellikle tek seferlik sorunlardan kurtarmak için en basit yoludur. Azure [Portalı'nda,](https://portal.azure.com/)uygulamanızın kılıcında, uygulamanızı durdurma veya yeniden başlatma seçenekleriniz vardır.

 ![502 kötü ağ geçidi ve 503 hizmet kullanılamaz HTTP hatalarını çözmek için uygulamayı yeniden başlatın](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Azure Powershell'i kullanarak da uygulamanızı yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).

