---
title: Redin için Azure önbelleği için Azure Izleyici (Önizleme) | Microsoft Docs
description: Bu makalede, performans ve kullanım sorunlarının hızlı bir şekilde anlaşılmasına izin veren Redsıs sahipleri için Azure önbelleği sağlayan Redis Cache için Azure Izleyicisi açıklanmaktadır.
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/07/2020
ms.openlocfilehash: af32459ffcb50c9f1cfdc59d2c8d355d3551e230
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210981"
---
# <a name="explore-azure-monitor-for-azure-cache-for-redis-preview"></a>Redsıs için Azure önbelleği için Azure Izleyicisini keşfet (Önizleme)

Redsıs için Azure önbelleği için Azure Izleyici (Önizleme), Birleşik etkileşimli bir deneyime yönelik tüm Azure önbelleğiniz için tüm Azure önbelleğinizin genel performans, başarısızlık, kapasite ve işlemsel sistem durumunun bir görünümünü sağlar. Bu makale, bu yeni izleme deneyiminin avantajlarını ve bu deneyimi kuruluşunuzun benzersiz ihtiyaçlarına uyacak şekilde nasıl değiştirebileceğiniz ve uyarlayabileceğinizi anlamanıza yardımcı olur.

## <a name="introduction"></a>Giriş

Deneyime girmeden önce, bilgilerin nasıl sunulduklarını ve görselleştirir.

Şunları sağlar:

* Tek bir konumdaki tüm aboneliklerinizde bulunan Redsıs kaynakları için Azure önbelleğinizin **Ölçek perspektifinde** , yalnızca değerlendirmede ilgilendiğiniz abonelikler ve kaynaklar için seçmeli kapsam yeteneği sağlar.

* Sorunları tanılamaya veya kategori kullanımı, hataları, kapasiteyi ve işlemlere göre ayrıntılı analiz gerçekleştirmeye yardımcı olmak üzere Redsıs kaynağı için belirli bir Azure önbelleğinin **detaya gitme Analizi** . Bu seçeneklerden herhangi birini seçmek ilgili ayrıntılı bir görünüm sağlar.  

* **Özelleştirilebilir** -bu deneyim, Azure izleyici çalışma kitabı şablonlarının üzerine kurulmuştur. bu deneyim, hangi ölçümlerin görüntülendiğini değiştirmenize, Sınırlarınızla hizalı olan eşikleri değiştirmenize veya ayarlamanıza ve sonra özel bir çalışma kitabına kaydetmenizi sağlar. Çalışma kitaplarındaki grafikler daha sonra Azure panolarına sabitlenebilir.  

Bu özellik herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez, redin için bu Azure önbelleği varsayılan olarak toplanır.

>[!NOTE]
>Bu özelliğe erişmek için ücret alınmaz ve [Azure izleyici fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yalnızca yapılandırdığınız veya etkinleştirdiğiniz Azure izleyici temel özellikleri için ücretlendirilirsiniz.

## <a name="view-utilization-and-performance-metrics-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği kullanım ve performans ölçümlerini görüntüleme

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve performansını görüntülemek için aşağıdaki adımları gerçekleştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **İzleme** araması yapın ve **izleyiciyi**seçin.

    !["Monitor" sözcüğünün bulunduğu ve bir hızölçer Style görüntüsüyle Hizmetleri "Monitor" adlı bir açılan pencerede arama kutusu](./media/cosmosdb-insights-overview/search-monitor.png)

3. **Redin (Önizleme) Için Azure önbelleği**' ni seçin.

Bu seçenek mevcut değilse, **daha fazla** tıklayın ve **Redsıs için Azure önbelleği**' ni seçin.

### <a name="overview"></a>Genel Bakış

**Genel bakışta**, tablo redsıs ölçümleri Için etkileşimli Azure önbelleğini görüntüler. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

* **Abonelikler** -yalnızca redin kaynağı Için bir Azure önbelleği olan abonelikler listelenir.  

* **Redin Için Azure önbelleği** -redsıs kaynağı için tümü, bir alt küme veya tek bir Azure önbelleği seçebilirsiniz.

* **Zaman aralığı** -varsayılan olarak, yapılan ilgili seçimlere göre son 4 saati bilgileri görüntüler.

Açılan listelerin altındaki sayaç kutucuğu, Redsıs kaynakları için Azure önbelleğinin toplam sayısı seçili aboneliklerdir. Çalışma kitabındaki, işlem ölçümlerini rapor eden sütunlar için koşullu renk kodlaması veya heavmaps vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir.

Redsıs kaynakları için Azure önbelleğinden birinin yanındaki açılan oku seçmek, bireysel kaynak düzeyinde performans ölçümlerinin dökümünü açığa çıkarır:

![Genel Bakış deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/overview.png)

Mavi renkle vurgulanmış Redsıs kaynak adı için Azure önbelleğinin seçilmesi sizi ilişkili hesap için varsayılan **genel bakışa** götürür. ,,,,,, `Used Memory` `Used Memory Percentage` , Gösterir `Server Load` `Server Load Timeline` `CPU` `Connected Clients` `Cache Misses` `Errors (Max)` .

### <a name="operations"></a>İşlemler

Sayfanın üst kısmındaki **işlemler** ' i seçin ve çalışma kitabı şablonunun **işlemler** bölümü açılır. ,,,, Gösterir `Total Operations` `Total Operations Timeline` `Operations Per Second` `Gets` `Sets` .

![Genel Bakış deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/operations.png)

### <a name="usage"></a>Kullanım

Sayfanın üst kısmında **kullanımı** seçin ve çalışma kitabı şablonunun **kullanım** bölümü açılır. ,,,, Gösterir `Cache Read` `Cache Read Timeline` `CacheWrite` `CacheHits` `Cache Misses` .

![Genel Bakış deneyiminin ekran görüntüsü](./media/redis-cache-insights-overview/usage.png)

Sayfanın üst kısmındaki **hataların** yanı sıra çalışma kitabı şablonunun **arızalarının** bir kısmı açılır. ,,,,,, `Total Errors` `Failover/Errors` , Gösterir `UnresponsiveClient/Errors` `RDB/Errors` `AOF/Errors` `Export/Errors` `Dataloss/Errors` `Import/Errors` .

![HTTP istek türüne göre dökümdeki hataların ekran görüntüsü](./media/redis-cache-insights-overview/failures.png)

### <a name="metric-definitions"></a>Ölçüm tanımları

Bu çalışma kitaplarını oluşturan ölçüm tanımlarının tam listesi için [kullanılabilir ölçümler ve raporlama aralıkları makalesine](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-monitor#available-metrics-and-reporting-intervals)göz atın.

## <a name="pin-export-and-expand"></a>Sabitle, dışarı aktar ve Genişlet

Bölümün sağ üst köşesindeki raptiye simgesini seçerek ölçüm bölümlerinin herhangi birini bir [Azure panosuna](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) sabitleyebilirsiniz.

![Ölçüm bölümü panoya sabitle örneği](./media/cosmosdb-insights-overview/pin.png)

Verilerinizi Excel biçiminde dışarı aktarmak için raptiye simgesinin solundaki aşağı ok simgesini seçin.

![Çalışma kitabı simgesini dışarı aktar](./media/cosmosdb-insights-overview/export.png)

Çalışma kitabındaki tüm açılan görünümleri genişletmek veya daraltmak için dışa aktarma simgesinin solundaki genişlet simgesini seçin:

![Çalışma kitabı simgesini Genişlet](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cache-for-redis-preview"></a>Redsıs için Azure önbelleği için Azure Izleyicisini özelleştirme (Önizleme)

Bu deneyim, Azure izleyici çalışma kitabı şablonlarının üzerine inşa edildiğinden, **Customize**  >  değiştirilmiş sürümünüzün bir kopyasını**düzenleme** ve bir kopyasını özel çalışma kitabına **kaydetme** olanağınız vardır. 

![Çubuğu Özelleştir](./media/cosmosdb-insights-overview/customize.png)

Çalışma kitapları, sizin için özel **Raporlarım** bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği **paylaşılan raporlar** bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kaydettikten sonra başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Çalışma kitabı galerisini komut çubuğundan Başlat](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olmak üzere otomatik uyarı ayarlamak için [ölçüm uyarılarını](../platform/alerts-metric.md) ve [hizmet durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications.md) yapılandırın.

* Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../app/usage-workbooks.md)konusunu gözden geçirin.
