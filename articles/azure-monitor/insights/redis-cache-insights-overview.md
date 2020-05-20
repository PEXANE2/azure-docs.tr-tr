---
title: Redin için Azure önbelleği için Azure Izleyici (Önizleme) | Microsoft Docs
description: Bu makalede, performans ve kullanım sorunlarını hızlı bir şekilde anlamak için önbellek sahipleri sağlayan Azure Redis Cache özelliği için Azure Izleyicisi açıklanmaktadır.
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/07/2020
ms.openlocfilehash: a6a8499a13e5c14869f9c9063528cea4ee82f419
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650422"
---
# <a name="explore-azure-monitor-for-azure-cache-for-redis-preview"></a>Redsıs için Azure önbelleği için Azure Izleyicisini keşfet (Önizleme)

Redsıs kaynaklarına yönelik Azure önbelleğinizin tümünde, redin (Önizleme) için Azure önbelleği için Azure Izleyici, birleştirilmiş ve etkileşimli bir görünüm sağlar:

- Genel performans
- Hatalar
- Kapasite
- İşlemsel sistem durumu

Bu makale, bu yeni izleme deneyiminin avantajlarını anlamanıza yardımcı olur. Ayrıca, bir deneyimin, kuruluşunuzun benzersiz ihtiyaçlarını karşılayacak şekilde nasıl değiştirileceğini ve uyarlandığını gösterir.

## <a name="introduction"></a>Giriş

Deneyimi başlatmadan önce, Redsıs için Azure Izleyici 'nin bilgileri görsel olarak nasıl sağladığını anlamanız gerekir.

Şunları sağlar:

- Tüm aboneliklerinizde tek bir konumdaki Redsıs kaynakları için Azure önbelleğinizin **Ölçek perspektifinde** . Yalnızca değerlendirmek istediğiniz abonelikler ve kaynaklar için seçmeli kapsam yapabilirsiniz.

- Redsıs kaynağı için belirli bir Azure önbelleğinin **detaya gitme Analizi** . Sorunları tanılayabilir ve kullanım, başarısızlık, kapasite ve işlemlerin ayrıntılı analizini görebilirsiniz. İlgili bilgilerin derinlemesine bir görünümünü görmek için bu kategorilerden herhangi birini seçin.  

- Bu deneyimin, en üst Azure Izleyici çalışma kitabı şablonları olan **özelleştirmesi** . Deneyim, hangi ölçümlerin görüntülendiğini değiştirmenize ve sınırlarınız ile hizalı eşikleri değiştirmenize veya ayarlamanıza olanak sağlar. Değişiklikleri özel bir çalışma kitabına kaydedebilir ve sonra çalışma kitabı grafiklerini Azure panolarına sabitleyebilirsiniz.

- Bu deneyimin **özelleştirilmesi** , en üst Azure izleyici çalışma kitabı şablonlarından oluşur. Deneyim, hangi ölçümlerin görüntülendiğini değiştirmenize ve sınırlarınız ile hizalı eşikleri değiştirmenize veya ayarlamanıza olanak sağlar. Değişiklikleri özel bir çalışma kitabına kaydedebilir ve sonra çalışma kitabı grafiklerini Azure panolarına sabitleyebilirsiniz.

Bu özellik, herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez. Redsıs bilgileri için Azure önbelleği varsayılan olarak toplanır.

>[!NOTE]
>Bu özelliğe erişmek için ücret alınmaz. [Azure izleyici fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yalnızca yapılandırdığınız veya etkinleştirdiğiniz Azure izleyici temel özellikleri için ücretlendirilirsiniz.

## <a name="view-utilization-and-performance-metrics-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği kullanım ve performans ölçümlerini görüntüleme

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve performansını görüntülemek için aşağıdaki adımları uygulayın:

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. **İzleyici**araması yapın ve **izleyici**' yi seçin.

    !["Monitor" sözcüğünün ve hızölçer simgesiyle "Monitor" ifadesini gösteren Hizmetler Arama sonucuyla arama kutusu](./media/cosmosdb-insights-overview/search-monitor.png)

1. **Redin (Önizleme) Için Azure önbelleği**' ni seçin. Bu seçenek mevcut değilse **More**  >  **redsıs için diğer Azure önbelleği**' ni seçin.

### <a name="overview"></a>Genel Bakış

**Genel bakışta**, tablo redsıs ölçümleri Için etkileşimli Azure önbelleğini görüntüler. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

- **Abonelikler**: yalnızca redin kaynağı Için bir Azure önbelleği olan abonelikler listelenir.  

- **Redsıs Için Azure önbelleği**: redsıs kaynağı için tümü, bir alt küme veya tek bir Azure önbelleği seçebilirsiniz.

- **Zaman aralığı**: varsayılan olarak, tabloda ilgili seçimlere göre son dört saat bilgileri görüntülenir.

Açılan listelerin altında bir sayaç kutucuğu vardır. Kutucuk, seçili aboneliklerdeki Redsıs kaynakları için Azure önbelleğinin toplam sayısını gösterir. Çalışma kitabı sütunları için koşullu renk kodları veya ısı haritaları rapor işlem ölçümleri. En derin renk en yüksek değeri temsil eder. Daha açık renkler daha düşük değerleri temsil eder.

Redsıs kaynakları için Azure önbelleğinden birinin yanındaki açılan liste okunu seçmek, tek kaynak düzeyindeki performans ölçümlerinin bir dökümünü ortaya çıkarır.

![Genel Bakış deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/overview.png)

Mavi renkle vurgulanmış Redsıs kaynak adı için Azure önbelleğini seçtiğinizde, ilişkili hesap için varsayılan **genel bakış** tablosunu görürsünüz. Şu sütunları gösterir:

- **Kullanılan Bellek**
- **Kullanılan Bellek Yüzdesi**
- **Sunucu Yükü**
- **Sunucu Yükü Zaman Çizelgesi**
- **CPU**
- **Bağlı İstemciler**
- **İsabetsiz Önbellek Okuma Sayısı**
- **Hata Sayısı (En Fazla)**

### <a name="operations"></a>İşlemler

Sayfanın üst kısmında **işlemler** ' i seçtiğinizde, çalışma kitabı şablonunun **işlemler** tablosu açılır. Şu sütunları gösterir:

- **Toplam İşlem Sayısı**
- **Toplam İşlem Sayısı Zaman Çizelgesi**
- **Saniye Başına İşlem**
- **Alınanlar**
- **Kümeler**

![İşletim deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/operations.png)

### <a name="usage"></a>Kullanım

Sayfanın üst kısmında **kullanımı** seçtiğinizde, çalışma kitabı şablonunun **kullanım** tablosu açılır. Şu sütunları gösterir:

- **Önbellek Okuması**
- **Önbellek Okuması Zaman Çizelgesi**
- **Önbellek Yazması**
- **İsabetli Önbellek Okuma Sayısı**
- **İsabetsiz Önbellek Okuma Sayısı**

![Kullanım deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/usage.png)

### <a name="failures"></a>Hatalar

Sayfanın üst kısmında bulunan **hataların** seçtiğinizde, çalışma kitabı şablonunun **arızalar** tablosu açılır. Şu sütunları gösterir:

- **Toplam Hata Sayısı**
- **Yük devretme/hatalar**
- **UnresponsiveClient/hatalar**
- **RDB/hatalar**
- **AOF/hata**
- **Dışarı aktarma/hatalar**
- **DataLoss/hata**
- **İçeri/dışarı aktarma**

![HTTP istek türüne göre döküm içeren hataların ekran görüntüsü](./media/redis-cache-insights-overview/failures.png)

### <a name="metric-definitions"></a>Ölçüm tanımları

Bu çalışma kitaplarını oluşturan ölçüm tanımlarının tam listesi için, [kullanılabilir ölçümler ve raporlama aralıklarıyla ilgili makaleye](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-monitor#available-metrics-and-reporting-intervals)göz atın.

## <a name="pin-export-and-expand"></a>Sabitle, dışarı aktar ve Genişlet

Herhangi bir ölçüm bölümünü bir [Azure panosuna](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)sabitlemek için bölümün sağ üst kısmında raptiye simgesini seçin.

![Raptiye simgesi vurgulanmış bir ölçüm bölümü](./media/cosmosdb-insights-overview/pin.png)

Verilerinizi bir Excel biçiminde dışarı aktarmak için raptiye simgesinin solundaki aşağı ok simgesini seçin.

![Vurgulanmış dışarı aktarma-çalışma kitabı simgesi](./media/cosmosdb-insights-overview/export.png)

Çalışma kitabındaki tüm görünümleri genişletmek veya daraltmak için dışa aktarma simgesinin solundaki genişlet simgesini seçin.

![Vurgulanan Genişlet-çalışma kitabı simgesi](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cache-for-redis-preview"></a>Redsıs için Azure önbelleği için Azure Izleyicisini özelleştirme (Önizleme)

Bu deneyim, en üstteki Azure izleyici çalışma kitabı şablonları olduğundan, **Customize**  >  **Edit**  >  değiştirilmiş sürümünüzün bir kopyasını özel bir çalışma kitabına kaydetmek için**Kaydet** Düzenle 'yi Özelleştir ' i seçebilirsiniz.

![Vurgulanmış özelleştirme içeren bir komut çubuğu](./media/cosmosdb-insights-overview/customize.png)

Çalışma kitapları, **Raporlarım** bölümünde veya **paylaşılan raporlar** bölümünde bir kaynak grubu içine kaydedilir. **Raporlarım** yalnızca sizin için kullanılabilir. **Paylaşılan raporlar** , kaynak grubuna erişim izni olan herkes tarafından kullanılabilir.

Özel bir çalışma kitabını kaydettikten sonra açmak için çalışma kitabı galerisine gidin.

![Gallery vurgulanmış bir komut çubuğu](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olan otomatik uyarılar ayarlamak için [ölçüm uyarılarını](../platform/alerts-metric.md) ve [hizmet durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications.md) yapılandırın.

* Çalışma kitaplarının desteklediği senaryoları, raporları yazmak veya özelleştirmek ve [Azure izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../app/usage-workbooks.md)konusunu inceleyerek daha fazla bilgi edinin.
