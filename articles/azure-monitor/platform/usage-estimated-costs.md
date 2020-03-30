---
title: Azure Monitor'da kullanım ve tahmini maliyetlerizleme
description: Azure Monitörü kullanımı ve tahmini maliyetler sayfasını kullanma işlemine genel bakış
author: dalekoetke
services: azure-monitor
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: 22c7243cd966f458610c2abc67ed5540b37357b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658824"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>Azure Monitor'da kullanım ve tahmini maliyetlerizleme

> [!NOTE]
> Bu makalede, birden çok Azure izleme özelliğinde kullanım ve tahmini maliyetlernasıl görüntülenecek açıklanmaktadır. Azure Monitor'un belirli bileşenleri için ilgili makaleler şunlardır:
> - [Azure Monitor Günlükleri ile kullanımı ve maliyetleri yönetin,](manage-cost-storage.md) veri saklama sürenizi değiştirerek maliyetlerinizi nasıl kontrol edebilirsiniz ve veri kullanımınızı nasıl analiz edip uyaranınızın durumu açıklanır.
> - [Application Insights'ın kullanımını ve maliyetlerini yönet,](../../azure-monitor/app/pricing.md) Application Insights'ta veri kullanımını nasıl analiz edeceğiz açıklar.

## <a name="azure-monitor-pricing-model"></a>Azure Monitör fiyatlandırma modeli

Temel Azure Monitörü faturalandırma modeli bulut dostu, tüketime dayalı bir fiyatlandırmadır ("You-As-You-Go Öde"). Sadece kullandığınız kadar ödersiniz. Fiyatlandırma ayrıntıları [uyarı, ölçümler, bildirimler,](https://azure.microsoft.com/pricing/details/monitor/) [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Uygulama Öngörüleri](https://azure.microsoft.com/pricing/details/application-insights/)için kullanılabilir. 

Log Analytics, günlük verileri için You-As-Go öde modeline ek olarak, You-As-Go fiyatına kıyasla %25'e varan tasarruf etmenizi sağlayan Kapasite Rezervasyonları'na sahiptir. Kapasite rezervasyon fiyatlandırması, 100 GB/gün'den başlayan bir rezervasyon satın almanızı sağlar. Rezervasyon seviyesinin üzerindeki tüm kullanımlar, Ödeme Niz-Kullan fiyatından faturalandırılır. Kapasite Rezervasyonu fiyatlandırması hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/details/monitor/)

Bazı müşteriler eski [Log Analytics fiyatlandırma katmanlarına](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers) ve [eski Enterprise Application Insights fiyatlandırma katmanına](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)erişebilir. 

## <a name="understanding-your-azure-monitor-costs"></a>Azure Monitörmaliyetlerinizi Anlama

Maliyetleri anlamak için iki aşama vardır. Bunlardan ilki, Azure Monitor'u izleme çözümünüz olarak değerlendirirken. 

### <a name="estimating-the-costs-to-manage-your-environment"></a>Çevrenizi yönetmek için maliyetleri tahmin etme

Henüz Azure Monitör Günlükleri kullanmıyorsanız, Azure Monitörü kullanmanın maliyetini tahmin etmek için [Azure Monitor fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=monitor) kullanabilirsiniz. Arama kutusuna "Azure Monitörü" girerek ve ortaya çıkan Azure Monitor kutucuğuna tıklayarak başlayın. Sayfayı Azure Monitor'a kaydırın ve Tür açılır sayfasından seçeneklerden birini seçin:

- Ölçümler sorguları ve Uyarılar  
- Log Analytics
- Application Insights

Bunların her birinde, fiyatlandırma hesaplayıcısı olası maliyetlerinizi beklenen kullanımınıza göre tahmin etmenize yardımcı olur.

Örneğin, Log Analytics ile her VM'den toplamayı beklediğiniz VM sayısını ve GB verilerini girebilirsiniz. Genellikle 1 GB ile 3 GB veri ayı, tipik bir Azure VM'sinden yutulrılır. Azure Monitör Günlüklerini zaten değerlendiriyorsanız, kendi ortamınızdaki veri istatistiklerinizi kullanabilirsiniz. [İzlenen VM'lerin sayısını](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-nodes-sending-data) ve [çalışma alanınızın sindirimi](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume)olan veri hacmini belirlemek için aşağıya bakın.

Benzer şekilde, "Uygulama etkinliğine dayalı veri hacmini tahmin edin" işlevini etkinleştiriseniz, uygulamanız hakkında girişler sağlayabilirsiniz (istemci tarafı telemetrisini toplarsanız, aylık istekler ve sayfa görünümleri), ve daha sonra hesap makinesi, benzer uygulamalar tarafından toplanan verilerin ortalama ve yüzde 90'ı kadarını size söyleyecektir. Bu uygulamalar, Uygulama Öngörüleri yapılandırmaaralığını kapsar (örneğin, bazıları varsayılan örnekleme ye sahiptir, bazılarının örneklemesi yoktur vb.), bu nedenle örneklemeyi kullanarak ortanca düzeyin çok altında yutturtuğunuz veri hacmini azaltma denetimine sahipsiniz. Ama bu, diğer benzer müşterilerin ne gördüğünü anlamak için bir başlangıç noktasıdır. Uygulama Öngörüleri için maliyetleri tahmin etme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/app/pricing#estimating-the-costs-to-manage-your-application)

### <a name="understanding-your-usage-and-estimated-costs"></a>Kullanımınızı ve tahmini maliyetlerinizi anlama

Azure Monitor'u kullanarak kullanımınızı anlamak ve izlemek önemlidir ve bunu kolaylaştırmak için zengin araçlar kümesi vardır. 

Azure, [Azure Maliyet Yönetimi + Faturalandırma](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) hub'ında çok sayıda kullanışlı işlevsellik sağlar. **Azure Maliyet Yönetimi + Faturalandırma** merkezini açtıktan sonra Maliyet **Yönetimi'ni** tıklatın ve [Kapsam'ı](https://docs.microsoft.com/azure/cost-management/understand-work-scopes) (araştırılabilmek için kaynak kümesi) seçin. 

Ardından, son 30 güniçin Azure Monitör maliyetlerini görmek için **Günlük Maliyetler** döşemesini tıklatın, Nispi tarihler altında "Son 30 gün"ü seçin ve Hizmet adlarını seçen bir filtre ekleyin:

1. Azure İzleyici
2. Application Insights
3. Log Analytics
4. İçgörü ve Analiz

Bu, şu şekilde bir görünümle sonuçlanır:

![Azure Maliyet Yönetimi ekran görüntüsü](./media/usage-estimated-costs/010.png)

Buradan, "Kaynağa Göre Maliyet" görünümünde daha ince ayrıntıları almak için bu birikmiş maliyet özetinden alıştırma yapabilirsiniz. Geçerli fiyatlandırma katmanlarında, Azure Günlüğü verileri, Log Analytics veya Application Insights'tan kaynaklanıp kaynaklanmadığına bakılmaksızın aynı sayaç kümesiüzerinden ücretlendirilir. Maliyetleri Log Analytics veya Application Insights kullanımınızdan ayırmak için **Kaynak türüne**bir filtre ekleyebilirsiniz. Tüm Application Insights maliyetlerini görmek için Kaynak türünü "microsoft.insights/components" ve Log Analytics maliyetleri için "microsoft.operationalinsights/workspaces" adresine filtreleyin. 

Kullanımınızın daha ayrıntılı ayrıntılarını [Azure portalından indirerek edinebilirsiniz.](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal) İndirilen elektronik tabloda, azure kaynağı başına günlük kullanımı görebilirsiniz. Bu Excel elektronik tablosunda, "Uygulama Öngörüleri" ve "Log Analytics" göstermek için önce "Sayaç Kategorisi" sütununa filtre uygulayarak ve ardından "Örnek Id" sütununa "microsoft.insights/components içeren" bir filtre ekleyerek Application Insights kaynaklarından kullanım bulunabilir.  Tüm Azure Monitor bileşenleri için tek bir günlük arka ucu olduğundan, Çoğu Uygulama Öngörüleri kullanımı, Günlük Analitiği Metre Kategorisi ile metrelerde raporlanır.  Yalnızca eski fiyatlandırma katmanları ve çok adımlı web testleri yle ilgili Application Insights kaynakları, Uygulama Öngörüleri Metre Kategorisi ile bildirilir.  Kullanım "Tüketilen Miktar" sütununda, her giriş için birim "Ölçü Birimi" sütununda gösterilir.  [Microsoft Azure faturanızı anlamanıza](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)yardımcı olacak daha fazla ayrıntı mevcuttur. 

> [!NOTE]
> **Azure Maliyet Yönetimi + Faturalandırma** hub'ında Maliyet **Yönetimi'ni** kullanmak, izleme maliyetlerini genel olarak anlamak için tercih edilen bir yaklaşımdır.  [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understand-your-usage-and-estimate-costs) ve [Application Insights'ın](https://docs.microsoft.com/azure/azure-monitor/app/pricing#understand-your-usage-and-estimate-costs) **Kullanım ve Tahmini Maliyetler** deneyimleri, Azure Monitor'un bu bölümlerinin her biri için daha derin bilgiler sağlar.

Azure Monitörü kullanımınızı görüntülemek için başka bir seçenek de Monitör merkezindeki **Kullanım ve tahmini maliyetler** sayfasıdır. Bu, [uyarı, ölçümler, bildirimler,](https://azure.microsoft.com/pricing/details/monitor/) [Azure Günlük Analitiği](https://azure.microsoft.com/pricing/details/log-analytics/)ve Azure Uygulama [Öngörüleri](https://azure.microsoft.com/pricing/details/application-insights/)gibi temel izleme özelliklerinin kullanımını gösterir. Nisan 2018'den önce sunulan fiyatlandırma planlarında müşteriler için bu, Insights ve Analytics teklifi yle satın alınan Log Analytics kullanımını da içerir.

Bu sayfada, kullanıcılar son 31 gün içinde kaynak kullanımlarını abonelik başına toplu olarak görüntüleyebilir. `Drill-ins`31 günlük dönemde kullanım eğilimlerini gösterin. Bu tahmin için birçok verinin bir araya gelmesi gerekmektedir, bu nedenle lütfen sayfa yüklerken sabırlı olun.

Bu örnek, izleme kullanımını ve ortaya çıkan maliyetlerin tahminini gösterir:

![Kullanım ve tahmini maliyetler portalı ekran görüntüsü](./media/usage-estimated-costs/001.png)

Son 31 günlük dönemde kullanım eğilimlerini gösteren bir grafik açmak için aylık kullanım sütunundaki bağlantıyı seçin: 

![Düğüm çubuğu grafik ekran görüntüsü başına dahil](./media/usage-estimated-costs/002.png)

## <a name="operations-management-suite-subscription-entitlements"></a>Operasyon Yönetimi Paketi abonelik hakları

Microsoft Operations Management Suite E1 ve E2'yi satın alan müşteriler, [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing)için düğüm başına veri alma haklarından yararlanabilir. Belirli bir abonelikteki Log Analytics çalışma alanları veya Application Insights kaynakları için bu hakları almak için: 

- Log Analytics çalışma alanları "Düğüm Başına (OMS)" fiyatlandırma katmanını kullanmalıdır.
- Application Insights kaynakları "Kurumsal" fiyatlandırma katmanını kullanmalıdır.

Kuruluşunuzun satın aldığı paketin düğüm sayısına bağlı olarak, bazı abonelikleri Kullandıkça Öde (GB Başına) fiyatlandırma katmanına taşımak avantajlı olabilir, ancak bu dikkatli bir değerlendirme gerektirir.

> [!WARNING]
> Kuruluşunuzun geçerli Microsoft Operations Management Suite E1 ve E2'si varsa, Log Analytics çalışma alanlarınızı "Düğüm Başına (OMS)" fiyatlandırma katmanında ve "Kurumsal" fiyatlandırma katmanındaki Application Insights kaynaklarınızda tutmak genellikle en iyisidir. 
>
