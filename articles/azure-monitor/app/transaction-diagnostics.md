---
title: Azure Uygulama Öngörüleri İşlem Tanılama | Microsoft Dokümanlar
description: Uygulama Öngörüleri uçtan uca işlem tanılama
ms.topic: conceptual
ms.date: 01/19/2018
ms.reviewer: sdash
ms.openlocfilehash: 39c4c51a1bc84e06efac3674b1ee5b487f9e6729
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671146"
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Birleştirilmiş çapraz bileşen işlem tanılama

Birleştirilmiş tanılama deneyimi, tüm Application Insights tarafından izlenen bileşenleriniz arasında sunucu tarafındaki telemetriyi otomatik olarak tek bir görünümde ilişkilendirer. Ayrı enstrümantasyon tuşlarına sahip birden fazla kaynağınız olması önemli değildir. Uygulama Öngörüleri altta yatan ilişkiyi algılar ve işlem yavaşlamasına veya başarısızlığa neden olan uygulama bileşenini, bağımlılığı nı veya özel durumu kolayca tanılamanızı sağlar.

## <a name="what-is-a-component"></a>Bileşen Nedir?

Bileşenler, dağıtılmış/mikro hizmetler uygulamanızın bağımsız olarak dağıtılabilir parçalarıdır. Geliştiriciler ve operasyon ekipleri kod düzeyinde görünürlüğe veya bu uygulama bileşenleri tarafından oluşturulan telemetriye erişime sahiptir.

* Bileşenler, takımınızın/kuruluşunuzun erişemeyebilecekleri SQL, EventHub gibi "gözlenen" dış bağımlılıklardan farklıdır (kod veya telemetri).
* Bileşenler herhangi bir sayıda sunucu/rol/kapsayıcı örneklerinde çalışır.
* Bileşenler ayrı Application Insights enstrümantasyon anahtarları (abonelikler farklı olsa bile) veya farklı roller tek bir Application Insights enstrümantasyon anahtarına raporlama olabilir. Yeni deneyim, nasıl kurulduklarına bakılmaksızın tüm bileşenlerdeki ayrıntıları gösterir.

> [!NOTE]
> * **İlgili öğe bağlantılarını mı kaçırıyorsun?** İlgili telemetrinin tümü sol tarafın [üst](#cross-component-transaction-chart) ve [alt](#all-telemetry-with-this-operation-id) kısımlarındadır. 

## <a name="transaction-diagnostics-experience"></a>İşlem tanılama deneyimi
Bu görünüm dört ana bölümden oluşur: sonuç listesi, bir çapraz bileşen işlem grafiği, bu işlemle ilgili tüm telemetrilerin zaman sıralı listesi ve soldaki seçili telemetri öğesinin ayrıntı bölmesi.

![Önemli parçalar](media/transaction-diagnostics/4partsCrossComponent.png)

## <a name="cross-component-transaction-chart"></a>Çapraz bileşen hareket grafiği

Bu grafik, bileşenler arasında istek ler ve bağımlılıklar süresi için yatay çubuklar içeren bir zaman çizelgesi sağlar. Toplanan tüm özel durumlar da zaman çizelgesinde işaretlenir.

* Bu grafikteki üst satır, giriş noktasını, bu harekette çağrılan ilk bileşene gelen isteği temsil eder. Süre, işlemin tamamlanması için alınan toplam süredir.
* Dış bağımlılıklara yapılan tüm çağrılar, bağımlılık türünü temsil eden simgelerle birlikte basit katlanabilir olmayan satırlardır.
* Diğer bileşenlere yapılan çağrılar katlanabilir satırlardır. Her satır, bileşende çağrılan belirli bir işlemle karşılık gelir.
* Varsayılan olarak, seçtiğiniz istek, bağımlılık veya özel durum sağ tarafta görüntülenir.
* [Ayrıntıları sağda](#details-of-the-selected-telemetry)görmek için herhangi bir satır seçin. 

> [!NOTE]
> Diğer bileşenlere yapılan aramalarda iki satır vardır: bir satır, arayan bileşenden giden aramayı (bağımlılık) temsil eder ve diğer satır çağrılan bileşendeki gelen isteğe karşılık gelir. Önde gelen simge ve süre çubuklarının farklı stil aralarında ayrım yardımcı olur.

## <a name="all-telemetry-with-this-operation-id"></a>Bu Operasyon Id ile tüm telemetri

Bu bölümde, bu işlemle ilgili tüm telemetrilerin zaman sırasına göre düz liste görünümü gösterilir. Ayrıca, özel olayları ve hareket grafiğinde görüntülenmeyen izleri de gösterir. Bu listeyi belirli bir bileşen/çağrı tarafından oluşturulan telemetriye filtreleyebilirsiniz. Sağdaki ilgili ayrıntıları görmek için bu listedeki herhangi bir telemetri [öğesini](#details-of-the-selected-telemetry)seçebilirsiniz.

![Tüm telemetrilerin zaman sırası](media/transaction-diagnostics/allTelemetryDrawerOpened.png)

## <a name="details-of-the-selected-telemetry"></a>Seçilen telemetrinin ayrıntıları

Bu katlanabilir bölme, hareket grafiğinden veya listeden seçili herhangi bir öğenin ayrıntılarını gösterir. "Tümünü göster" toplanan tüm standart öznitelikleri listeler. Herhangi bir özel öznitelik, standart kümenin altında ayrı ayrı listelenir. "..." düğmesine tıklayın. izlemeyi kopyalama seçeneği almak için yığın izleme penceresinin altında. "Profil oluşturucu izlemelerini aç" veya "Hata ayıklama anlık görüntüsünü aç" ilgili ayrıntı bölmelerinde kod düzeyi tanılamalarını gösterir.

![Özel durum ayrıntısı](media/transaction-diagnostics/exceptiondetail.png)

## <a name="search-results"></a>Arama sonuçları

Bu katlanabilir bölme, filtre ölçütlerini karşılayan diğer sonuçları gösterir. Yukarıda listelenen 3 bölümle ilgili ayrıntıları güncellemek için herhangi bir sonuca tıklayın. Herhangi birinde örnekleme geçerli olsa bile, tüm bileşenlerden ayrıntılara sahip olma olasılığı en yüksek örnekleri bulmaya çalışırız. Bunlar "önerilen" örnekler olarak gösterilir.

![Arama sonuçları](media/transaction-diagnostics/searchResults.png)

## <a name="profiler-and-snapshot-debugger"></a>Profil oluşturucu ve anlık görüntü alıcısı

[Uygulama Öngörüleri profilleyicisi](../../azure-monitor/app/profiler.md) veya [anlık görüntü ayıklayıcı](snapshot-debugger.md) performans ve hata sorunlarının kod düzeyinde tanılanmasına yardımcı olur. Bu deneyimle, tek bir tıklamayla herhangi bir bileşenden profil oluşturucu izlemelerini veya anlık görüntülerini görebilirsiniz.

Profiler'ı çalıştıramadıysanız, lütfen **serviceprofilerhelp microsoft.com\@**

Snapshot Debugger'ı çalıştıramadıysanız, lütfen **snapshothelp microsoft.com\@**

![Profiler Entegrasyonu](media/transaction-diagnostics/profilerTraces.png)

## <a name="faq"></a>SSS

*Grafikte tek bir bileşen görüyorum ve diğerleri yalnızca bu bileşenlerin içinde ne olduğuna dair herhangi bir ayrıntı olmadan dışa bağımlılıklar olarak gösteriliyor.*

Olası nedenler:

* Diğer bileşenler Application Insights ile enstrümante mi?
* En son kararlı Application Insights SDK kullanıyor musunuz?
* Bu bileşenler ayrı Application Insights kaynaklarıysa, bunların telemetrilerine erişiminiz gerekli midir?

Erişiminiz varsa ve bileşenler en son Application Insights SDK'ları ile cihaza sahipse, sağ üst geri bildirim kanalı aracılığıyla bize bildirin.

*Bağımlılıklar için yinelenen satırlar görüyorum. Bu beklenen bir şey mi?*

Şu anda, giden bağımlılık çağrısını gelen istekten ayrı olarak gösteriyoruz. Tipik olarak, iki çağrı, yalnızca ağ gidiş-dönüş nedeniyle farklı olan süre değeri yle aynı görünür. Önde gelen simge ve süre çubuklarının farklı stil aralarında ayrım yardımcı olur. Bu veri sunumu kafa karıştırıcı mı? Geri bildiriminizi bize iletin!

*Peki ya saat farklı bileşen örnekleri arasında eğrilmelere ne delalı?*

Zaman çizelgeleri, hareket grafiğindeki saat eğrilmelerine göre ayarlanır. Ayrıntılar bölmesinde veya Analytics'i kullanarak tam zaman damgalarını görebilirsiniz.

*Neden yeni deneyim ilgili öğelerin sorgularının çoğu eksik?*

Bu tasarım gereğidir. Tüm bileşenler arasında ilgili öğelerin tümü sol tarafta (üst ve alt bölümler) zaten mevcuttur. Yeni deneyim, sol tarafın kapsamadığı iki ilgili öğeye sahiptir: bu olaydan beş dakika önce ve sonra gelen tüm telemetriler ve kullanıcı zaman çizelgesi.
