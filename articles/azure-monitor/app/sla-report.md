---
title: Kapalı kalma süresi, SLA ve kesinti çalışma kitabı-Application Insights
description: Application Insights kaynaklarınız ve Azure aboneliklerinizde tek bir cam bölmeden Web testi için SLA 'yı hesaplayın ve rapor edin.
ms.topic: conceptual
ms.date: 02/8/2021
ms.openlocfilehash: d225627a27bffd9088956e5aee37ca543e528d4a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101714060"
---
# <a name="downtime-sla-and-outages-workbook"></a>Kapalı kalma süresi, SLA ve kesintiler çalışma kitabı

Application Insights kaynaklarınız ve Azure aboneliklerinizde tek bir cam bölmeden Web testleri için SLA 'yı (hizmet düzeyi sözleşmesi) hesaplamak ve raporlamak için basit bir yol ile tanışın. Kapalı kalma süresi ve kesinti raporu, müşterinizin bağlantısının, tipik uygulama yanıt süresinin ve daha fazla geçen sürenin bilinmesini geliştirmek için, önceden oluşturulmuş güçlü sorgular ve veri görselleştirmeleri sağlar.

SLA çalışma kitabı şablonuna, Application Insights kaynağınızın çalışma kitabı Galerisi aracılığıyla veya en üstteki **SLA raporları** ' nı seçerek kullanılabilirlik sekmesi aracılığıyla erişilebilir.
:::image type="content" source="./media/sla-report/availability.png" alt-text="SLA raporlarının vurgulanarak kullanılabilirlik sekmesinin ekran görüntüsü." lightbox="./media/sla-report/availability.png":::

:::image type="content" source="./media/sla-report/workbook-gallery.png" alt-text="Çalışma kitabı galerisinin kapalı kalma süresi ve kesinti çalışma kitabı vurgulanmış ekran görüntüsü." lightbox ="./media/sla-report/workbook-gallery.png":::

## <a name="parameter-flexibility"></a>Parametre esnekliği

Çalışma kitabında ayarlanan parametreler raporunuzun geri kalanını etkiler.

:::image type="content" source="./media/sla-report/outages.png" alt-text=" Kesinti süresi ve kesintiler çalışma kitabındaki kesinti/bakım parametreleri sekmesinin ekran görüntüsü." lightbox ="./media/sla-report/outages.png":::

`Subscriptions`, `App Insights Resources` ve `Web Test` parametreleri üst düzey kaynak seçeneklerinizi belirleme. Bu parametreler, Log Analytics sorgularını temel alır ve her rapor sorgusunda kullanılır.

`Failure Threshold` ve `Outage Window` bir hizmet kesintisi için kendi ölçütlerinizi belirlemenizi sağlar. Örneğin, uygulama öngörüleri için ölçütler uyarısı, seçilen bir dönemde başarısız konum sayacını temel alır. Tipik eşik, beş dakikalık bir pencere üzerinde üç konumdadır.

`Maintenance Period` , tipik bakım sıklığını seçmenizi sağlar ve örnek bir `Maintenance Window` bakım dönemi için bir tarih saat seçicidir. Belirlenen süre boyunca oluşan tüm veriler, sonuçlarınızda yok sayılır.

`Availability Target 9s` Hedef 9 hedefini iki 9 ile beş 9 arasında belirtir.

## <a name="overview-page"></a>Genel bakış sayfası

Genel Bakış sayfası toplam SLA 'ağınız (tanımlanmışsa bakım dönemleri hariç), uçtan uca kesinti örnekleri ve uygulama kesinti süresi hakkında üst düzey bilgiler içerir. Kesinti örnekleri, kesinti parametreleriniz temelinde başarılı olana kadar bir test başarısız olarak başladığında tanımlanır. Bir test 8:00 ' de başarısız oluyor ve 10:00 ' de başarılı olursa, tüm veri dönemi aynı kesinti olarak değerlendirilir.

:::image type="content" source="./media/sla-report/overview.gif" alt-text=" Teste göre genel bakış tablosunu gösteren GIF genel bakış sayfası." lightbox="./media/sla-report/overview.gif":::

Ayrıca, raporlama döneminizin üzerinde oluşan en uzun kesintilerinizi de araştırabilirsiniz.

Bazı testler daha fazla araştırma için Application Insights kaynaklarına gönderilir ancak yalnızca [çalışma alanı tabanlı Application Insights kaynağında](create-workspace-resource.md)mümkündür.

## <a name="downtime-outages-and-failures"></a>Kapalı kalma süresi, kesintiler ve başarısızlık

**Kesintiler ve kesinti** süresi sekmesinde, Toplam kesinti örnekleri ve test tarafından kesilen toplam süre ile ilgili bilgiler bulunur. **Konuma göre sorunlar** sekmesi, olası sorun bağlantı alanlarının tanımlanmasına yardımcı olmak için başarısız test konumlarının coğrafi eşlemesine sahiptir.

:::image type="content" source="./media/sla-report/outages-failures.gif" alt-text=" Kapalı kalma süresi ve kesintiler çalışma kitabındaki kesintiler ve kapalı kalma süresinin GIF 'i ve konuma göre başarısızlık sekmesi." lightbox="./media/sla-report/outages-failures.gif":::

## <a name="edit-the-report"></a>Raporu düzenleme

Raporu diğer [Azure Izleyici çalışma kitapları](../visualize/workbooks-overview.md)gibi düzenleyebilirsiniz. Ekibinizin ihtiyaçlarına bağlı olarak sorguları veya görselleştirmeleri özelleştirebilirsiniz.

:::image type="content" source="./media/sla-report/edit.gif" alt-text=" Görselleştirmeyi bir pasta grafiğine dönüştürmek için Düzenle düğmesini seçen GIF." lightbox="./media/sla-report/edit.gif":::

### <a name="log-analytics"></a>Log Analytics

Sorgular [Log Analytics](../logs/log-analytics-overview.md) ' de çalıştırılabilir ve diğer raporlarda veya panolarda kullanılıyor olabilir. Parametre kısıtlamasını kaldırın ve çekirdek sorguyu yeniden kullanın.

:::image type="content" source="./media/sla-report/logs.gif" alt-text=" Günlük sorgusunun GIF 'i." lightbox="./media/sla-report/logs.gif":::

## <a name="access-and-sharing"></a>Erişim ve paylaşma

Rapor, ekipleriniz, liderle paylaşılabilir veya daha fazla kullanım için bir panoya sabitlenebilir. Kullanıcının, gerçek çalışma kitabının depolandığı Applications Insights kaynağına okuma izni/erişimi olması gerekir.

:::image type="content" source="./media/sla-report/share.png" alt-text=" Bu şablonu paylaşma ekran görüntüsü." lightbox= "./media/sla-report/share.png":::

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu iyileştirme ipuçları Log Analytics](../logs/query-optimization.md).
- [Çalışma kitaplarında grafik oluşturmayı](../visualize/workbooks-chart-visualizations.md)öğrenin.
- [Kullanılabilirlik testleriyle](monitor-web-app-availability.md)Web sitenizi nasıl izleyeceğinizi öğrenin.