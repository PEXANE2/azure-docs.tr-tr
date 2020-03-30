---
title: Azure Monitor'dan verileri görselleştirme | Microsoft Dokümanlar
description: Azure Monitor'da depolanan metrik ve günlük verilerini görselleştirmek için kullanılabilir yöntemlerin bir özetini sağlar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: 877616f6fd31bdfbe193bd8f03efb3f79317ad42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535378"
---
# <a name="visualizing-data-from-azure-monitor"></a>Azure Monitor'dan verileri görselleştirme
Bu makalede, Azure Monitor'da depolanan günlük ve metrik verileri görselleştirmek için kullanılabilir yöntemlerin bir özetini sağlar.

Grafikler ve grafikler gibi görselleştirmeler, sorunları inceleyip desenleri belirlemek için izleme verilerinizi analiz etmenize yardımcı olabilir. Kullandığınız araca bağlı olarak, görselleştirmeleri kuruluşunuzun içindeki ve dışındaki diğer kullanıcılarla paylaşma seçeneğiniz de olabilir.

## <a name="workbooks"></a>Çalışma Kitapları
[Çalışma kitapları,](../azure-monitor/app/usage-workbooks.md) ekibinizin verilerinize, araştırmanıza ve işbirliğinize ilişkin derin bilgiler sağlayan etkileşimli belgelerdir. Çalışma kitaplarının yararlı olduğu özel örnekler, sorun giderme kılavuzları ve olay sonrası ölümdür.

![Çalışma Kitabı](media/visualizations/workbook.png)

### <a name="advantages"></a>Yararları
- Hem ölçümleri hem de günlükleri destekler.
- Tabloda bir öğe seçerek ilişkili grafikleri ve görselleştirmeleri dinamik olarak güncelleştireceği etkileşimli raporları etkinleştiren parametreleri destekler.
- Belge gibi akış.
- Kişisel veya paylaşılan çalışma kitapları için seçenek.
- Kolay, işbirlikçi-dostu yazma deneyimi.
- Şablonlar genel GitHub tabanlı şablon galerisini destekler.

### <a name="limitations"></a>Sınırlamalar
- Otomatik yenileme yok.
- Çalışma kitaplarını tek bir cam bölme olarak daha az kullanışlı hale getiren panolar gibi yoğun bir düzen yoktur. Daha derin bilgiler sağlamak için tasarlanmıştır.


## <a name="azure-dashboards"></a>Azure Panoları
[Azure panoları,](../azure-portal/azure-portal-dashboards.md) Azure için birincil panolandırma teknolojisidir. Azure altyapınız ve hizmetleriniz üzerinde önemli sorunları hızlı bir şekilde belirlemenize olanak tanıyan tek bir cam bölme sağlamada özellikle yararlıdır.

![Pano](media/visualizations/dashboard.png)

### <a name="advantages"></a>Yararları
- Azure'a derin entegrasyon. Görselleştirmeler, Metrics Explorer, Log Analytics ve Application Insights dahil olmak üzere birden çok Azure sayfalarından panolara sabitlenebilir.
- Hem ölçümleri hem de günlükleri destekler.
- [Ölçümler explorer](platform/metrics-charts.md)çıktısı, [Günlük sorguları](log-query/log-query-overview.md)ve Uygulama Öngörüleri'ndeki [haritalar](app/app-map.md) ve kullanılabilirlik dahil olmak üzere birden çok kaynaktan gelen verileri birleştirin.
- Kişisel veya paylaşılan panolar için seçenek. Azure [rol tabanlı kimlik doğrulama (RBAC)](../role-based-access-control/overview.md)ile tümleşik.
- Otomatik yenileme. Ölçümlerin yenilenmesi, en az beş dakika olan zaman aralığına bağlıdır. Günlükler, belirli bir görselleştirmedeki "yenile" simgesine tıklayarak veya tam panoyu yenileyerek isteğe bağlı manuel yenileme seçeneğiyle her saat yeniler.
- Zaman damgası ve özel parametrelere sahip parametrize ölçüm panoları.
- Esnek düzen seçenekleri.
- Tam ekran modu.


### <a name="limitations"></a>Sınırlamalar
- Veri tabloları için destek olmadan günlük görselleştirmeleri üzerinde sınırlı denetim. Toplam veri serisi sayısı 10 ile sınırlıdır ve diğer veri serileri _başka_ bir kova altında gruplandırılır.
- Günlük grafikleri için özel parametreler desteği yok.
- Günlük grafikleri son 30 günle sınırlıdır.
- Günlük grafikleri yalnızca paylaşılan panolara sabitlenebilir.
- Pano verileriyle etkileşim yok.
- Sınırlı bağlamsal sondaj.


## <a name="power-bi"></a>Power BI
[Power BI,](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) özellikle iş odaklı panolar ve raporların yanı sıra uzun vadeli KPI eğilimlerini analiz eden raporlar oluşturmak için özellikle yararlıdır. Farklı kaynaklardan gelen verileri birleştirme ve web ve mobil cihazlarda rapor paylaşımı gibi özelliklerinden yararlanabilmek için [günlük sorgusunun sonuçlarını](platform/powerbi.md) Power BI veri kümesine aktarabilirsiniz.

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>Yararları
- Zengin görseller.
- Yakınlaştırma ve çapraz filtreleme gibi kapsamlı etkileşim.
- Kuruluşunuz boyunca paylaşılabilmek kolaydır.
- Birden çok veri kaynağından gelen diğer verilerle tümleştirme.
- Bir küp önbelleğe sonuçları ile daha iyi performans.


### <a name="limitations"></a>Sınırlamalar
- Günlükleri destekler, ancak ölçümleri desteklemez.
- Azure tümleştirmesi yok. Azure Kaynak Yöneticisi aracılığıyla panoları ve modelleri yönetemezsiniz.
- Sorgu sonuçlarının yapılandırmak için Power BI modeline aktarılması gerekir. Sonuç boyutu ve yenileme sınırlaması.
- Günde sekiz kez sınırlı veri yenileme.


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) operasyonel panoları nda öne çıkan açık bir platformdur. Özellikle operasyonel olayları tespit etmek, izole etmek ve triyaj yapmak için yararlıdır. Azure metrik verilerinizi görselleştirmesi için Azure aboneliğinize [Grafana Azure Monitor veri kaynağı eklentisi](platform/grafana-plugin.md) ekleyebilirsiniz.

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>Yararları
- Zengin görseller.
- Veri kaynaklarının zengin ekosistemi.
- Yakınlaştırma dahil olmak üzere veri etkileşimi.
- Parametreleri destekler.

### <a name="limitations"></a>Sınırlamalar
- Azure tümleştirmesi yok. Azure Kaynak Yöneticisi aracılığıyla panoları ve modelleri yönetemezsiniz.
- Grafana Bulutu için ek Grafana altyapısını veya ek maliyeti desteklemek için maliyet.


## <a name="build-your-own-custom-application"></a>Kendi özel uygulamanızı oluşturun
Azure Monitor'daki günlük ve metrik verilerdeki verilere, kendi özel web sitelerinizi ve uygulamalarınızı oluşturmanıza olanak tanıyan herhangi bir REST istemcisini kullanarak API'leri aracılığıyla erişebilirsiniz.

### <a name="advantages"></a>Yararları
- UI, görselleştirme, etkileşim ve özelliklerde tam esneklik.
- Ölçümleri ve günlük verilerini diğer veri kaynaklarıyla birleştirin.

### <a name="disadvantages"></a>Dezavantajlar
- Önemli bir mühendislik çabası gereklidir.


## <a name="azure-monitor-views"></a>Azure Monitör Görünümleri

> [!IMPORTANT]
> Görüşler amortismana lı olma sürecindedir. Görünümleri çalışma kitaplarına dönüştürme konusunda rehberlik için [çalışma kitapları geçiş kılavuzuna Azure Monitor görünüm tasarımcısına](platform/view-designer-conversion-overview.md) bakın.

[Azure Monitor'daki görünümler,](platform/view-designer.md) günlük verileriyle özel görselleştirmeler oluşturmanıza olanak tanır. Topladıkları verileri sunmak için [izleme çözümleri](insights/solutions.md) tarafından kullanılırlar.


![Görünüm](media/visualizations/view.png)

### <a name="advantages"></a>Yararları
- Günlük verileri için zengin görselleştirmeler.
- Diğer kaynak gruplarına ve aboneliklere aktarmak için görünümleri dışa aktarın ve aktarın.
- Çalışma alanları ve izleme çözümleriyle Azure Monitor yönetim modeline entegre edin.
- Özel parametreler için [filtreler.](platform/view-designer-filters.md)
- Etkileşimli, çok düzeyli sondaj ı destekler (başka bir görünüme açılan görünüm)

### <a name="limitations"></a>Sınırlamalar
- Günlükleri destekler, ancak ölçümleri desteklemez.
- Kişisel görüş yok. Çalışma alanına erişimi olan tüm kullanıcılar tarafından kullanılabilir.
- Otomatik yenileme yok.
- Sınırlı düzen seçenekleri.
- Birden çok çalışma alanı veya Application Insights uygulamaları arasında sorgu için destek yok.
- Sorgular 8MB yanıt boyutu ve 110 saniye sorgu yürütme süresi sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Monitor tarafından toplanan veriler](platform/data-platform.md)hakkında bilgi edinin.
- Azure [panoları](../azure-portal/azure-portal-dashboards.md)hakkında bilgi edinin.
- Azure [Monitör'de Görünümler](platform/view-designer.md)hakkında bilgi edinin.
- Çalışma [Kitapları](../azure-monitor/app/usage-workbooks.md)hakkında bilgi edinin.
- Günlük [verilerini Power BI'ye aktarma](../azure-monitor/platform/powerbi.md)hakkında bilgi edinin.
- [Grafana Azure Monitör veri kaynağı eklentisi](../azure-monitor/platform/grafana-plugin.md)hakkında bilgi edinin.

