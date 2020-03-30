---
title: .NET uygulamaları için Azure Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama
description: Özel durumlar üretime geçtiğinde hata ayıklama anlık görüntüleri otomatik olarak toplanır .NET uygulamaları
ms.topic: conceptual
ms.date: 10/23/2019
ms.reviewer: cweining
ms.openlocfilehash: 18f43ba90157d71ec9488b6858fa9f41b2ee42a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275769"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>.NET uygulamalarında özel durumlarda anlık görüntü hatalarını ayıklama
Bir özel durum oluştuğunda, canlı web uygulamanızdan otomatik olarak hata ayıklama anlık görüntüsü toplayabilirsiniz. Anlık görüntü, özel durum atıldığı anda kaynak kodun ve değişkenlerin durumunu gösterir. [Azure Uygulama İstatistikleri'ndeki](../../azure-monitor/app/app-insights-overview.md) Anlık Görüntü Hata Ayıklama, web uygulamanızdan özel durum telemetrisini izler. Üretimdeki sorunları tanılamak için ihtiyacınız olan bilgilere sahip olmak için en çok atan özel durumlarınızda anlık görüntüler toplar. Uygulamanıza [Anlık Görüntü toplayıcınuNuGet paketini](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) ekleyin ve isteğe bağlı olarak [ApplicationInsights.config'de](../../azure-monitor/app/configuration-with-applicationinsights-config.md)koleksiyon parametrelerini yapılandırın. Anlık görüntüler, Uygulama [Öngörüleri](../../azure-monitor/app/asp-net-exceptions.md) portalındaki özel durumlar üzerinde görünür.

Hata ayıklama anlık görüntülerini portalda görüntüleyerek çağrı yığınını görebilir ve her bir çağrı yığını çerçevesinde değişkenleri inceleyebilirsiniz. Kaynak koduyla daha güçlü bir hata ayıklama deneyimi elde etmek için Visual Studio 2019 Enterprise ile anlık görüntüleri açın. Visual Studio'da, [Snappoints'i](https://aka.ms/snappoint) bir istisna beklemeden etkileşimli olarak anlık görüntüler alacak şekilde de ayarlayabilirsiniz.

Hata ayıklama anlık görüntüleri 15 gün boyunca saklanır. Bu bekletme ilkesi, uygulama başına olarak ayarlanır. Bu değeri artırmanız gerekiyorsa, Azure portalında bir destek örneği açarak bir artış isteyebilirsiniz.

## <a name="enable-application-insights-snapshot-debugger-for-your-application"></a>Uygulamanız için Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama'yı etkinleştirme
Anlık görüntü koleksiyonu şu lar için kullanılabilir:
* .NET Framework ve ASP.NET .NET Framework 4.5 veya sonraki uygulamalar.
* .NET Core 2.0 ve ASP.NET Core 2.0 uygulamaları Windows'da çalışır.

Aşağıdaki ortamlar desteklenir:

* [Azure Uygulama Hizmeti](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* İşletim Sistemi ailesini 4 veya sonraki olarak çalıştıran [Azure Bulut Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya sonraki saatlerde çalışan [Azure Hizmet Kumaşı hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya sonraki çalışma azure [Sanal Makineler ve sanal makine ölçeği kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* Windows Server 2012 R2 veya sonraki veya Windows 8.1 veya sonraki çalıştıran [şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

> [!NOTE]
> İstemci uygulamaları (örneğin, WPF, Windows Formları veya UWP) desteklenmez.

Anlık Görüntü Hata Ayıklayıcı'yı etkinleştirdiyseniz ancak anlık görüntü görmüyorsanız, [Sorun Giderme kılavuzumuzu kontrol edin.](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)

## <a name="grant-permissions"></a>İzinleri verme

Anlık görüntülere erişim rol tabanlı erişim denetimi (RBAC) tarafından korunur. Anlık görüntünün incelenmesi için, önce bir abonelik sahibi tarafından gerekli role eklenmeniz gerekir.

> [!NOTE]
> Sahipleri ve katkıda bulunanlar otomatik olarak bu role sahip değildir. Anlık görüntüleri görüntülemek istiyorlarsa, kendilerini role eklemeleri gerekir.

Abonelik `Application Insights Snapshot Debugger` sahipleri, rolü anlık görüntüleri inceleyecek kullanıcılara atamalıdır. Bu rol, hedef Application Insights kaynağı veya kaynak grubu veya aboneliği için abonelik sahipleri tarafından tek tek kullanıcılara veya gruplara atanabilir.

1. Azure portalındaki Uygulama Öngörüleri kaynağına gidin.
1. **Erişim denetimi (IAM)** öğesine tıklayın.
1. **+Rol ataması ekle düğmesini** tıklatın.
1. **Roller** açılır listesinden **Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama'yı** seçin.
1. Kullanıcının ekleyebileceği bir ad arayın ve girin.
1. Kullanıcıyı role eklemek için **Kaydet** düğmesini tıklatın.


> [!IMPORTANT]
> Anlık görüntüler, değişken ve parametre değerlerinde kişisel ve diğer hassas bilgileri içerebilir.

## <a name="view-snapshots-in-the-portal"></a>Portaldaki Anlık Görüntüleri Görüntüle

Uygulamanızda bir özel durum oluştuktan ve anlık görüntü oluşturulduktan sonra, görüntüleneniz gereken anlık görüntüler olmalıdır. Bir özel durum, portaldan hazır ve görüntülenebilir bir anlık görüntüye kadar 5 ila 10 dakika sürebilir. Anlık görüntüleri görüntülemek **için, Hata** bölmesinde, **İşlemler** sekmesini görüntülerken **Operasyonlar** düğmesini seçin veya **Özel Durumlar** sekmesini görüntülerken Özel **Durumlar** düğmesini seçin:

![Hatalar Sayfası](./media/snapshot-debugger/failures-page.png)

**Uçuçlu İşlem Ayrıntıları** bölmesini açmak için sağ bölmede bir işlem veya özel durum seçin ve ardından özel durum olayını seçin. Verilen özel durum için anlık görüntü varsa, [özel durum](../../azure-monitor/app/asp-net-exceptions.md)ayrıntıları yla birlikte sağ bölmede Hata **Ayıklama Anlık Görüntü** düğmesi görüntülenir.

![Özel durum hata ayıklama anlık açma düğmesi](./media/snapshot-debugger/e2e-transaction-page.png)

Hata Ayıklama Anlık Görüntü görünümünde bir çağrı yığını ve değişkenler bölmesi görürsünüz. Çağrı yığını bölmesinde çağrı yığınının çerçevelerini seçtiğinizde, değişkenler bölmesinde bu işlev çağrısıiçin yerel değişkenleri ve parametreleri görüntüleyebilirsiniz.

![Portalda Hata Ayıklama Anlık Görüntü'nün görüntüle](./media/snapshot-debugger/open-snapshot-portal.png)

Anlık görüntüler hassas bilgiler içerebilir ve varsayılan olarak görüntülenebilir değildir. Anlık görüntüleri görüntülemek için, `Application Insights Snapshot Debugger` rolün size atanmış olması gerekir.

## <a name="view-snapshots-in-visual-studio-2017-enterprise-or-above"></a>Visual Studio 2017 Enterprise veya üzeri görüntüler görüntüleyin
1. Visual **Download Snapshot** Studio Enterprise tarafından `.diagsession` açIlebilen bir dosyayı indirmek için Anlık Görüntü İndir düğmesini tıklatın.

2. Dosyayı `.diagsession` açmak için Anlık Görüntü Debugger Visual Studio bileşeninin yüklü olması gerekir. Anlık Görüntü Hata Ayıklama bileşeni Visual Studio'daki ASP.net iş yükünün gerekli bir bileşenidir ve Visual Studio yükleyicisindeki Tek Tek Bileşen listesinden seçilebilir. Visual Studio 2017 sürüm 15.5'ten önce Visual Studio'nun bir sürümünü kullanıyorsanız, uzantıyı [Visual Studio Marketplace'ten](https://aka.ms/snapshotdebugger)yüklemeniz gerekir.

3. Anlık görüntü dosyasını açtıktan sonra Visual Studio'daki Minidump Hata Ayıklama sayfası görüntülenir. Anlık görüntünün hata ayıklamaya başlamak için **Hata Ayıklama Yönetilen Kodu'nu** tıklatın. Anlık görüntü, işlemin geçerli durumunu ayıklamanız için özel durum atılan kod satırına açılır.

    ![Görsel Studio'da hata ayıklama anlık görüntüsünü görüntüleme](./media/snapshot-debugger/open-snapshot-visualstudio.png)

İndirilen anlık görüntü, web uygulama sunucunuzda bulunan tüm simge dosyalarını içerir. Anlık görüntü verilerini kaynak koduyla ilişkilendirmek için bu sembol dosyaları gereklidir. Uygulama Hizmeti uygulamaları için, web uygulamalarınızı yayınlarken sembol dağıtımı etkinleştirdiğinizden emin olun.

## <a name="how-snapshots-work"></a>Anlık görüntüler nasıl çalışır?

Snapshot Toplayıcı bir Uygulama [Insights Telemetri İşlemcisi](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-processors-aspnet)olarak uygulanır. Uygulamanız çalıştığında, Snapshot Collector Telemetri İşlemcisi uygulamanızın telemetri ardışık hattına eklenir.
Uygulamanız [TrackException'ı](../../azure-monitor/app/asp-net-exceptions.md#exceptions)her aradığında, Anlık Görüntü Toplayıcısı atılan özel durum türünden ve fırlatma yönteminden bir Sorun Kimliği oluşturur.
Uygulamanız TrackException'ı her aradığında, uygun Sorun Kimliği için bir sayaç artınız olur. Sayaç değere `ThresholdForSnapshotting` ulaştığında, Sorun Kimliği Bir Tahsilat Planı'na eklenir.

Snapshot Collector, [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) etkinliğine abone olarak atılan istisnaları da izler. Bu olay çıktığında, özel durum Sorun Kimliği hesaplanır ve Toplama Planı'ndaki Sorun Kimlikleri ile karşılaştırılır.
Bir eşleşme varsa, çalışan işlemin anlık görüntüsü oluşturulur. Anlık görüntüye benzersiz bir tanımlayıcı atanır ve özel durum bu tanımlayıcıyla damgalanır. FirstChanceException işleyicisi döndükten sonra atılan özel durum normal olarak işlenir. Sonunda, özel durum, anlık görüntü tanımlayıcısıyla birlikte Uygulama Öngörüleri'ne bildirildiğinde TrackException yöntemine tekrar ulaşır.

Ana işlem çok az kesinti ile kullanıcılara çalıştırmak ve hizmet vermeye devam ediyor. Bu arada, anlık görüntü Snapshot Uploader işlemine teslim edilir. Snapshot Uploader bir minidump oluşturur ve ilgili sembol (.pdb) dosyaları ile birlikte Application Insights yükler.

> [!TIP]
> - İşlem anlık görüntüsü, yürütme işleminin askıya alınmış bir klonudur.
> - Anlık görüntü oluşturma yaklaşık 10 ila 20 milisaniye sürer.
> - Varsayılan değer `ThresholdForSnapshotting` 1'dir. Bu da minimum değerdir. Bu nedenle, anlık görüntü oluşturulmadan önce uygulamanızın aynı özel durumu **iki kez** tetiklemeniz gerekiyor.
> - Visual `IsEnabledInDeveloperMode` Studio'da hata ayıklama yaparken anlık görüntü oluşturmak istiyorsanız doğru ayarlayın.
> - Anlık görüntü oluşturma oranı `SnapshotsPerTenMinutesLimit` ayar ile sınırlıdır. Varsayılan olarak, sınır her on dakikada bir anlık görüntüdür.
> - Günde en fazla 50 anlık görüntü yüklenebilir.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi 15 gündür. Her Uygulama Öngörüleri örneği için günde en fazla 50 anlık görüntüye izin verilir.

### <a name="publish-symbols"></a>Sembolleri yayımlama
Anlık Görüntü Hata Ayıklayıcı, değişkenleri çözmek ve Visual Studio'da hata ayıklama deneyimi sağlamak için üretim sunucusundaki sembol dosyaları gerektirir.
Visual Studio 2017 sürümü 15.2 (veya üzeri) Uygulama Hizmeti'nde yayımlandığında varsayılan olarak sürüm oluşturma sembolleri yayımlar. Önceki sürümlerde, sembollerin sürüm modunda yayınlanması `.pubxml` için yayımlama profil dosyanıza aşağıdaki satırı eklemeniz gerekir:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure İşlemi ve diğer türler için, sembol dosyalarının ana uygulama .dll'nin aynı `wwwroot/bin`klasöründe olduğundan (genellikle, geçerli yolda) olduğundan emin olun.

> [!NOTE]
> Mevcut farklı sembol seçenekleri hakkında daha fazla bilgi için [Visual Studio belgelerine](https://docs.microsoft.com/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp?view=vs-2019#output
)başvurun. En iyi sonuçları elde etmek için "Tam", "Taşınabilir" veya "Gömülü" kullanmanızı öneririz.

### <a name="optimized-builds"></a>Optimize edilmiş yapılar
Bazı durumlarda, JIT derleyicisi tarafından uygulanan optimizasyonlar nedeniyle sürüm yapılarında yerel değişkenler görüntülenemez.
Ancak, Azure Uygulama Hizmetleri'nde, Anlık Görüntü Toplayıcı, Toplama Planı'nın bir parçası olan atma yöntemlerini en iyi duruma getirebilir.

> [!TIP]
> Deoptimization desteği almak için Uygulama Öngörüleri Site Uzantısı'nı Uygulama Hizmetinizde yükleyin.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız için Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama'yı etkinleştirin:

* [Azure Uygulama Hizmeti](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Hizmet Kumaşı hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler ve sanal makine ölçek setleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama Ötesinde:
 
* Özel durum beklemeden anlık görüntü almak için [kodunuzda anlık noktaları ayarlayın.](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications)
* [Web uygulamalarınızdaki özel durumları tanılamak,](../../azure-monitor/app/asp-net-exceptions.md) Uygulama Öngörüleri için daha fazla özel durumu nasıl görünür hale getirilen açıklar.
* [Smart Detection](../../azure-monitor/app/proactive-diagnostics.md) performans anormalliklerini otomatik olarak keşfeder.
