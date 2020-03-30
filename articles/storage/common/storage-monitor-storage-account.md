---
title: Azure portalında bir Azure Depolama hesabı nasıl izlenir| Microsoft Dokümanlar
description: Azure portalını kullanarak Azure'daki bir depolama hesabını nasıl izleyeceğinizi öğrenin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 43955950b2fc0544b149c67e3f8df7ca905fafed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268294"
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Azure portalında depolama hesabını izleme

[Azure Depolama Analizi,](storage-analytics.md) tüm depolama hizmetleri için ölçümler ve lekeler, kuyruklar ve tablolar için günlükler sağlar. [Azure portalını,](https://portal.azure.com) hesabınız için hangi ölçümlerin ve günlüklerin kaydedildiğini yapılandırmak ve metrik verilerinizin görsel temsillerini sağlayan grafikleri yapılandırmak için kullanabilirsiniz. 

Depolama için [Azure Monitörü'ne](../../azure-monitor/insights/storage-insights-overview.md) (önizleme) incelemenizi öneririz. Azure Depolama hizmetleri performansınızın, kapasitenizin ve kullanılabilirliğinizin birleşik bir görünümünü sunarak Azure Depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sunan Azure Monitor özelliğidir. Herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez ve bu ölçümleri önceden tanımlanmış etkileşimli grafiklerden ve dahil olan diğer görselleştirmelerden hemen görüntüleyebilirsiniz.

> [!NOTE]
> Azure portalındaki izleme verilerinin incelenmesiyle ilgili maliyetler vardır. Daha fazla bilgi için Bkz. [Depolama Analizi.](storage-analytics.md)
>
> Azure Dosyaları şu anda Depolama Analitiği ölçümlerini destekler, ancak henüz günlüğe kaydetmeyi desteklemez.
>
> Üstün performans bloğu blob depolama hesapları Depolama Analitik ölçümlerini desteklemez, ancak günlüğe kaydetmeyi destekler. REST API veya istemci kitaplığı aracılığıyla günlük günlüğe kaydetmeyi programlı olarak etkinleştirebilirsiniz. Üstün performans blob depolama hesaplarıyla ölçümleri görüntülemek istiyorsanız, [Azure Monitor'da Azure Depolama Ölçümleri'ni](storage-metrics-in-azure-monitor.md)kullanmayı düşünün.
>
> Azure Depolama ile ilgili sorunları tanımlamak, tanılamak ve sorun gidermek için Depolama Analizi'ni ve diğer araçları kullanma yla ilgili ayrıntılı bir kılavuz için Microsoft [Azure Depolama Depolamasını İzle, tanılama ve sorun giderme](storage-monitoring-diagnosing-troubleshooting.md)konusuna bakın.
>

## <a name="configure-monitoring-for-a-storage-account"></a>Depolama hesabı için izlemeyi yapılandırma

1. Azure [portalında,](https://portal.azure.com)hesap panosunu açmak için **Depolama hesapları'nı,** ardından depolama hesabı adını seçin.
1. Menü bıçağının **İzleme** bölümünde **Tanılama'yı** seçin.

    ![İzleme Seçenekleri](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)

1. İzlemek istediğiniz her **hizmet** için ölçüm verisi **türünü** ve verilerin **bekletme ilkesini** seçin. **Durum'u** **Kapalı**olarak ayarlayarak izlemeyi de devre dışı bırakabilirsiniz.

    ![İzleme Seçenekleri](./media/storage-monitor-storage-account/storage-enable-metrics-01.png)

   Veri saklama ilkesini ayarlamak için, **Bekletme (gün)** kaydırıcısını taşıyın veya saklanması gereken veri gün sayısını 1'den 365'e girin. Yeni depolama hesapları için varsayılan değer yedi gündür. Bekletme ilkesi ayarlamak istemiyorsanız, sıfır girin. Bekletme ilkesi yoksa, izleme verilerini silmek size kalmış.

   > [!WARNING]
   > Metrik verileri el ile sildiğinizde ücretlendirilirsiniz. Eski analitik verileri (saklama ilkenizden eski veriler) sistem tarafından hiçbir ücret ödemeden silinir. Hesabınız için depolama analizi verilerini ne kadar süreyle saklamak istediğinize bağlı olarak bir bekletme ilkesi ayarlamanızı öneririz. Daha fazla bilgi için [depolama ölçümlerinde](storage-analytics-metrics.md#billing-on-storage-metrics) Faturalandırma'ya bakın.
   >

1. İzleme yapılandırmasını tamamladığınızda **Kaydet'i**seçin.

Varsayılan bir ölçüm kümesi, depolama hesabı bıçağındaki grafiklerin yanı sıra tek tek servis bıçaklarında (blob, sıra, tablo ve dosya) görüntülenir. Bir hizmet için ölçümleri etkinleştirdikten sonra, verilerin grafiklerinde görünmesi bir saat kadar sürebilir. Grafikte hangi ölçümlerin görüntüleneceğini yapılandırmak için herhangi bir metrik grafikte **Edit'i** seçebilirsiniz.

**Durum'u** **Kapalı**olarak ayarlayarak ölçümlerin toplanmasını ve günlüğe kaydetmeyi devre dışı bırakabilirsiniz.

> [!NOTE]
> Azure Depolama, depolama hesabınıziçin ölçümleri depolamak için [tablo depolama](storage-introduction.md#table-storage) sını kullanır ve ölçümleri hesabınızdaki tablolarda saklar. Daha fazla bilgi için, bkz. [Ölçümler nasıl depolanır.](storage-analytics-metrics.md#how-metrics-are-stored)
>

## <a name="customize-metrics-charts"></a>Ölçüm grafiklerini özelleştirme

Bir ölçüm grafiğinde hangi depolama ölçümlerinin görüntüleneğeceğini seçmek için aşağıdaki yordamı kullanın.

1. Azure portalında bir depolama metrik grafiği görüntüleyerek başlayın. Tek bir hizmet için **depolama hesap bıçağında** ve **Metrics** bıçağında (blob, sıra, tablo, dosya) grafikler bulabilirsiniz.

   Bu örnekte, **depolama hesabı bıçak**üzerinde görünen aşağıdaki grafik kullanır:

   ![Azure portalında grafik seçimi](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Grafiği nidelemek için grafiğin herhangi bir yerine tıklayın.

1. Ardından, grafikte görüntülenecek ölçümlerin **Zaman Aralığını** ve görüntülemeyi istediğiniz **hizmetleri** (blob, sıra, tablo, dosya) seçin. Burada, blob hizmeti için görüntülemek için geçen haftanın ölçümleri seçilir:

   ![Grafik'i Edit bıçağında zaman aralığı ve servis seçimi](./media/storage-monitor-storage-account/storage-edit-metric-time-range.png)

1. Grafikte görüntülenmesini istediğiniz tek tek **ölçümleri** seçin ve **ardından Tamam'ı**tıklatın.

   ![Grafik'i Edit bıçağında tek tek metrik seçim](./media/storage-monitor-storage-account/storage-edit-metric-selections.png)

Grafik ayarlarınız, depolama hesabında izleme verilerinin toplanmasını, toplanmasını veya depolanmasını etkilemez.

### <a name="metrics-availability-in-charts"></a>Grafiklerdeki ölçümler kullanılabilirliği

Açılır listede hangi hizmeti seçtiğinize ve düzenlediğiniz grafiğin birim türüne bağlı olarak kullanılabilir ölçümlerin listesi değişir. Örneğin, yalnızca birimleri yüzde olarak görüntüleyen bir grafiği *düzenliyorsanız, YüzdeAğ Hatası* ve *YüzdeThrottlingError* gibi yüzde ölçümlerini seçebilirsiniz:

![Azure portalında hata yüzdesi grafiği isteme](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Metrik çözünürlük

**Tanılama'da** seçtiğiniz ölçümler, hesabınıziçin kullanılabilen ölçümlerin çözümünü belirler:

* **Toplu** izleme, giriş/çıkış, kullanılabilirlik, gecikme ve başarı yüzdeleri gibi ölçümler sağlar. Bu ölçümler blob, tablo, dosya ve sıra hizmetlerinden toplanır.
* **API başına,** hizmet düzeyindeki agregalara ek olarak, tek tek depolama işlemleri için kullanılabilen ölçümlerle daha ince çözünürlük sağlar.

## <a name="configure-metrics-alerts"></a>Ölçümleri yapılandırır uyarılar

Depolama kaynak ölçümleri için eşiklere ne zaman ulaşıldığını bildirmek için uyarılar oluşturabilirsiniz.

1. **Uyarı kuralları bıçak**açmak için, Menü **bıçak** **IZLEME** bölümüne aşağı kaydırın ve **Uyarılar (klasik)** seçin.
2. Uyarı kuralı kılıcı **ekle'yi** açmak için metrik uyarı **ekle (klasik)** seçeneğini belirleyin
3. Yeni uyarı kuralınız için bir **Ad** ve **Açıklama** girin.
4. Bir uyarı, uyarı **Durumu**ve **Eşik**eklemek istediğiniz **Metrik'i** seçin. Eşik birim türü seçtiğiniz metriğe bağlı olarak değişir. Örneğin, "count" *ContainerCount*için birim türüdür, *YüzdeAğ Hatası* ölçümü için birim ise bir yüzdedir.
5. **Dönemi**seçin. Dönem içinde Eşiğe ulaşan veya eşiği aşan ölçümler bir uyarı tetikler.
6. (İsteğe bağlı) **E-posta** ve **Webhook** bildirimlerini yapılandırın. Web hooks hakkında daha fazla bilgi için bkz: [Azure metrik uyarısı üzerinde bir webhook yapılandırma.](../../azure-monitor/platform/alerts-webhooks.md) E-posta veya webhook bildirimlerini yapılandırmazsanız, uyarılar yalnızca Azure portalında görünür.

![Azure portalında 'uyarı kuralı ekle' bıçak](./media/storage-monitor-storage-account/add-alert-rule.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Portal panosuna ölçüm grafikleri ekleme

Herhangi bir depolama alanı hesabınız için Azure Depolama ölçüm grafikleri ekleyebilirsiniz, portal panonuza.

1. [Azure portalında](https://portal.azure.com) **panonugörüntülerken Panoyu Seç'i** tıklatın.
1. **Kutucuk**Galerisi'nde,**Türüne** > göre **kutucukları bul'u**seçin.
1. **Tür** > **Depolama hesaplarını**seçin.
1. **Kaynaklar'da,** ölçümlerini panoya eklemek istediğiniz depolama hesabını seçin.
1. **Kategorilerİzleme** > **seçin.**
1. Grafik döşemesini, görüntülenmesini istediğiniz metrik için panonuza sürükleyin ve bırakın. Panoda görüntülenmesini istediğiniz tüm ölçümler için yineleyin. Aşağıdaki resimde, "Blobs - Toplam istekler" grafiği örnek olarak vurgulanır, ancak tüm grafikler panonuza yerleştirilmesi için kullanılabilir.

   ![Azure portalında döşeme galerisi](./media/storage-monitor-storage-account/storage-customize-dashboard.png)
1. Grafik eklemeyi bitirdiğinizde, pano üst kısmında ki **özelleştirmeyi** seçin.

Panonuza grafikler ekledikten sonra, ölçümleri özelleştir grafiklerinde açıklandığı şekilde bunları daha da özelleştirebilirsiniz.

## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma

Azure Depolama'ya, blob, tablo ve sıra hizmetleri için okuma, yazma ve silme istekleri için tanılama günlüklerini kaydetmesi talimatını verebilirsiniz. Belirlediğiniz veri saklama ilkesi bu günlükler için de geçerlidir.

> [!NOTE]
> Azure Dosyaları şu anda Depolama Analitiği ölçümlerini destekler, ancak henüz günlüğe kaydetmeyi desteklemez.
>

1. Azure [portalında,](https://portal.azure.com) **depolama hesabı**nı açmak için Depolama hesapları'nı, ardından depolama hesabının adını seçin.
1. Menü bıçağının **İzleme (klasik)** bölümünde **Tanılama ayarlarını (klasik)** seçin.

    ![Azure portalında MONITORING altında tanılama menü öğesi.](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)

1. **Durum'un** **Açık**olarak ayarlandığından emin olun ve günlüğe kaydetmeyi etkinleştirmek istediğiniz **hizmetleri** seçin.

    ![Azure portalında günlüğe kaydetmeyi yapılandırın.](./media/storage-monitor-storage-account/enable-diagnostics.png)
1. **Kaydet**'e tıklayın.

Tanılama günlükleri depolama hesabınızda *$logs* adlı bir blob kapsayıcısına kaydedilir. Günlük verilerini [Microsoft Depolama Gezgini](https://storageexplorer.com)gibi bir depolama gezgini kullanarak veya depolama istemcikitaplığını veya PowerShell'i kullanarak programlı bir şekilde görüntüleyebilirsiniz.

$logs kapsayıcısına erişim hakkında bilgi için [Depolama analizi günlüğe kaydin.](storage-analytics-logging.md)

## <a name="next-steps"></a>Sonraki adımlar

* Depolama Analitiği için [ölçümler, günlüğe kaydetme ve faturalandırma](storage-analytics.md) hakkında daha fazla ayrıntı bulabilirsiniz.
