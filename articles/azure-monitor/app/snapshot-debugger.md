---
title: .NET uygulamaları için Azure Application Insights Snapshot Debugger
description: Hata ayıklama anlık görüntülerini üretim .NET uygulamalarında özel durumlar oluşturulduğunda otomatik olarak toplanır
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/23/2019
ms.reviewer: cweining
ms.openlocfilehash: 44b4a7bb5910f7f2d89a9f76e21ccfcacda667fb
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76932566"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>.NET uygulamalarında özel durumlarda anlık görüntü hata ayıklama
Bir özel durum oluştuğunda, hata ayıklama anlık görüntüsünü canlı web uygulamanızı otomatik olarak toplayabilirsiniz. Anlık görüntü, özel durumun oluştuğu şu anda kaynak kodu ve değişkenleri durumunu gösterir. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) Snapshot Debugger Web uygulamanızdan özel durum telemetrisini izler. Böylece, üretim sorunlarını tanılamak ihtiyacınız olan bilgileri sahip anlık görüntüleri, üst özel durum atma özel durumları toplar. [Anlık görüntü toplayıcısı NuGet paketini](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza ekleyin ve isteğe bağlı olarak [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)içinde koleksiyon parametrelerini yapılandırın. Anlık görüntüler Application Insights portalındaki [özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) üzerinde görünür.

Hata ayıklama anlık görüntülerini portalda görüntüleyerek çağrı yığınını görebilir ve her bir çağrı yığını çerçevesinde değişkenleri inceleyebilirsiniz. Kaynak kodla daha güçlü bir hata ayıklama deneyimi sağlamak için, Visual Studio 2019 Enterprise ile anlık görüntüler açın. Visual Studio'da ayrıca [etkileşimli anlık görüntülerini almak için anlık görüntü noktaları ayarlamak](https://aka.ms/snappoint) olmadan için bir özel durum bekleniyor.

Hata ayıklama anlık görüntüleri 15 gün boyunca depolanır. Bu bekletme ilkesi, bir uygulama başına temelinde ayarlanır. Bu değeri arttırmak gerekiyorsa, Azure portalında bir destek talebi açarak artışı isteyebilirsiniz.

## <a name="enable-application-insights-snapshot-debugger-for-your-application"></a>Uygulamanız için Application Insights Snapshot Debugger etkinleştirin
Anlık görüntü koleksiyonu için kullanılabilir:
* .NET Framework 4.5 veya üzeri çalışan .NET framework ve ASP.NET uygulamaları.
* Windows üzerinde çalışan .NET core 2.0 ve ASP.NET Core 2.0 uygulamaları.

Şu ortamlarda desteklenir:

* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* İşletim sistemi ailesi 4 veya üstünü çalıştıran [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya üzeri sürümlerde çalışan [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya üstünü çalıştıran [Azure sanal makineleri ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya üzeri ya da Windows 8.1 veya üstünü çalıştıran [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

> [!NOTE]
> İstemci uygulamaları (örneğin, WPF, Windows Forms veya UWP) desteklenmez.

Snapshot Debugger etkinleştirdiyseniz, anlık görüntüleri görmüyorsanız, [sorun giderme kılavuzumuzu](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)kontrol edin.

## <a name="grant-permissions"></a>İzinleri verme

Anlık görüntüleri erişim, rol tabanlı erişim denetimi (RBAC) tarafından korunur. Bir anlık görüntü incelemek için önce gerekli rol için bir abonelik sahibi tarafından eklenmelidir.

> [!NOTE]
> Otomatik olarak sahipleri ve katkıda bulunanların bu role sahip değilsiniz. Bunlar anlık görüntüleri görüntülemek istiyorsanız, kendilerini rolüne eklemelisiniz.

Abonelik sahipleri Ata `Application Insights Snapshot Debugger` rol kullanıcılara anlık görüntülerini inceler. Bu rol, bireysel kullanıcılar veya gruplar için Application Insights kaynağı hedef abonelik sahipleri tarafından veya kendi kaynak grubuna veya aboneliğe atanabilir.

1. Azure portalında Application Insights kaynağına gidin.
1. **Erişim denetimi (IAM)** öğesine tıklayın.
1. Tıklayın **+ rol ataması Ekle** düğmesi.
1. Seçin **Application Insights Snapshot Debugger** gelen **rolleri** aşağı açılan listesi.
1. Arayın ve kullanıcı eklemek için bir ad girin.
1. Tıklayın **Kaydet** rolüne kullanıcı eklemek için Ekle düğmesine.


> [!IMPORTANT]
> Anlık görüntüler, potansiyel olarak değişkeni ve parametre değerlerini kişisel ve hassas bilgileri içerebilir.

## <a name="view-snapshots-in-the-portal"></a>Portalda anlık görüntüleri görüntüleme

Uygulamanızda bir özel durum oluştu ve bir anlık görüntü oluşturulduktan sonra, görüntülenecek anlık görüntülere sahip olmanız gerekir. Bir anlık görüntüye hazırlık ve portaldan görünebilen bir özel durumdan 5 ila 10 dakika sürebilir. Anlık görüntüleri görüntülemek için, **hata** bölmesinde, **Işlemler** sekmesini görüntülerken **Işlemler** düğmesini seçin veya **özel durumlar** sekmesini görüntülerken **özel durumlar** düğmesini seçin:

![Arızalar sayfası](./media/snapshot-debugger/failures-page.png)

Sağ bölmedeki bir işlem veya özel durum seçerek **uçtan uca Işlem ayrıntıları** bölmesini açın, sonra özel durum olayını seçin. Verilen özel durum için bir anlık görüntü varsa, sağdaki bölmede [özel durum](../../azure-monitor/app/asp-net-exceptions.md)ayrıntılarının bulunduğu bir **hata ayıklama anlık görüntüsü aç** düğmesi görünür.

![Özel durum hata ayıklama anlık görüntüsü düğmesinde Aç](./media/snapshot-debugger/e2e-transaction-page.png)

Anlık görüntü hata ayıklama Görünümü'nde, çağrı yığını ve değişkenleri bölmesini görürsünüz. Çağrı yığını Bölmesi'nde çerçeve çağrı yığınının seçtiğinizde, yerel değişkenleri görüntüleyebilir ve bu işlevin parametreleri değişkenleri Bölmesi'nde çağırın.

![Portalı'nda hata ayıklama anlık görüntüsünü görüntüle](./media/snapshot-debugger/open-snapshot-portal.png)

Anlık görüntüler, hassas bilgiler içerebilir ve görüntülenebilir olmayan varsayılan olarak. Anlık görüntüleri görüntülemek için olmalıdır `Application Insights Snapshot Debugger` size atanan rol.

## <a name="view-snapshots-in-visual-studio-2017-enterprise-or-above"></a>Visual Studio 2017 Enterprise veya üzeri anlık görüntüleri görüntüleme
1. Visual Studio Enterprise tarafından açılabilen bir `.diagsession` dosyası indirmek için **anlık görüntüyü indir** düğmesine tıklayın.

2. `.diagsession` dosyasını açmak için Snapshot Debugger Visual Studio bileşeninin yüklü olması gerekir. Snapshot Debugger bileşeni, Visual Studio 'daki ASP.net iş yükünün gerekli bir bileşenidir ve Visual Studio yükleyicisi 'ndeki ayrı bileşen listesinden seçilebilir. Visual Studio 2017 sürüm 15,5 ' den önceki bir Visual Studio sürümü kullanıyorsanız, uzantıyı [Visual Studio Market](https://aka.ms/snapshotdebugger)yüklemeniz gerekecektir.

3. Anlık görüntü dosyası açtıktan sonra Visual Studio'da mini döküm hata ayıklama sayfası görüntülenir. Tıklayın **hata ayıklama yönetilen kodu** anlık görüntü hata ayıklama başlatılamıyor. Anlık görüntü geçerli işlemin durumunu ayıklayabilirsiniz, burada özel durumun oluştuğu kod satırına açılır.

    ![Visual Studio'da hata ayıklama anlık görüntüsünü görüntüle](./media/snapshot-debugger/open-snapshot-visualstudio.png)

İndirilen anlık görüntü, web uygulama sunucunuzda bulunan herhangi bir sembol dosyalarını içerir. Bu sembol dosyaları, kaynak kod ile anlık görüntü verileri ilişkilendirmek için gereklidir. App Service uygulamaları için web uygulamalarınızı yayımladığınızda, sembol dağıtımını etkinleştirmek emin olun.

## <a name="how-snapshots-work"></a>Anlık görüntüleri nasıl çalışır?

Anlık görüntü toplayıcının olarak uygulanan bir [Application Insights Telemetri İşlemci](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Uygulamanız çalışırken, anlık görüntü toplayıcısı Telemetri işlemci, uygulamanızın telemetri ardışık düzenine eklenen.
Her zaman uygulama çağrılarınızı [TrackException](../../azure-monitor/app/asp-net-exceptions.md#exceptions), oluşturulan özel durum ve oluşturma yöntemini türünden bir sorun kimliği anlık görüntü toplayıcının hesaplar.
Uygulamanızı çağırır TrackException, her zaman uygun sorun kimliği için bir sayaç artırılır Sayaç ulaştığında `ThresholdForSnapshotting` değeri, sorun kimliği, bir koleksiyon planı'na eklenir.

Anlık görüntü toplayıcının bunlar abone tarafından oluşturulan gibi ayrıca özel durumları izleyen [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) olay. Bu olayı tetikler, özel durum sorun kimliği hesaplanan ve koleksiyon planı içinde sorun kimlikleri karşı karşılaştırılır.
Ardından bir eşleşme varsa, çalışan işlem anlık görüntüsünü oluşturulur. Anlık görüntü benzersiz bir tanımlayıcı atanır ve özel durum tanımlayıcı ile damgalandı. FirstChanceException işleyici döndürdükten sonra oluşan özel durum normal şekilde işlenir. Sonuç olarak, özel durum yeniden TrackException yöntemi, Application Insights anlık görüntü tanımlayıcısı ile birlikte, burada bildirilir ulaşır.

Ana işlem, çalıştırmak ve trafiğin az kesinti ile kullanıcılara hizmet devam eder. Bu arada, anlık görüntü kapatmak için anlık görüntü yükleyici işlemi devredildiği. Anlık görüntü karşıya yükleyen bir mini döküm dosyası oluşturur ve tüm ilgili sembol (.pdb) dosyalarını yanı sıra Application ınsights'ı yükler.

> [!TIP]
> - Çalışan işlemi askıya alınmış bir kopyasını işlem anlık görüntüsüdür.
> - Anlık görüntü oluşturmak yaklaşık 10-20 milisaniye cinsinden alır.
> - İçin varsayılan değer `ThresholdForSnapshotting` 1'dir. Bu ayrıca en küçük değerdir. Bu nedenle, aynı özel durum tetiklemek uygulamanızı sahip **iki kez** anlık görüntüsünü oluşturmadan önce.
> - Ayarlama `IsEnabledInDeveloperMode` Visual Studio'da hata ayıklama sırasında anlık görüntü oluşturmak istiyorsanız True.
> - Anlık görüntü oluşturma oranı sınırlıdır `SnapshotsPerTenMinutesLimit` ayarı. Varsayılan olarak, bir anlık görüntü her on dakikada bir sınırdır.
> - Günde en fazla 50 anlık görüntüleri karşıya yüklenmeyebilir.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi 15 gündür. Her bir Application Insights örneği için günde en fazla 50 anlık görüntüye izin verilir.

### <a name="publish-symbols"></a>Yayım simgeleri
Snapshot Debugger, değişkenleri çözülecek ve Visual Studio'da hata ayıklama bir deneyim sağlamak üzere üretim sunucusundaki sembol dosyalarını gerektiri.
Sürüm 15.2 (veya üstü) App Service'te yayımlarken, varsayılan olarak sürüm yapıları için Visual Studio 2017 için semboller yayımlar. Önceki sürümlerde, aşağıdaki satırı yayımlama profilinizi eklemeniz gereken `.pubxml` sembolleri yayın modunda yayımlanır böylece dosya:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure işlem ve diğer türleri için Sembol dosyaları ana uygulama .dll aynı klasörde olduğundan emin olun (genellikle `wwwroot/bin`) ya da geçerli yolda kullanılabilir.

> [!NOTE]
> Kullanılabilir farklı sembol seçenekleri hakkında daha fazla bilgi için [Visual Studio belgelerine](https://docs.microsoft.com/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp?view=vs-2019#output
)başvurun. En iyi sonuçlar için, "tam", "taşınabilir" veya "katıştırılmış" kullanmanızı öneririz.

### <a name="optimized-builds"></a>En iyi duruma getirilmiş derlemeleri
Bazı durumlarda, yerel değişkenler, JIT derleyicisi tarafından uygulanan en iyi duruma getirme nedeniyle sürüm yapılarında görüntülenemiyor.
Ancak, Azure uygulama hizmetleri, anlık görüntü toplayıcının koleksiyon planı parçası olan oluşturma yöntemleri deoptimize.

> [!TIP]
> Application Insights Site uzantısının deoptimization destek almak için App Service içinde yükleyin.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız için Application Insights Snapshot Debugger etkinleştirin:

* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineleri ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Snapshot Debugger ötesinde:
 
* [Anlık görüntü noktaları kodunuzda ayarlayın](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) için bir özel durum beklemeden anlık görüntüler alınacak.
* [Web uygulamalarınızda özel durumları tanılama](../../azure-monitor/app/asp-net-exceptions.md) daha fazla özel durum Application Insights tarafından görülebilmesi açıklanmaktadır.
* [Akıllı algılama](../../azure-monitor/app/proactive-diagnostics.md) performans anormalliklerini otomatik olarak bulur.
