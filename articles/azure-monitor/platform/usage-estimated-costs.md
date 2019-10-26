---
title: Azure Izleyici 'de kullanımı ve tahmini maliyetleri izleme
description: Azure Izleyici kullanımını ve tahmini maliyetleri kullanma sürecine genel bakış sayfası
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: 56dd58afa49296ab097dfd8a6560a7191ac8c644
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72932019"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>Azure Izleyici 'de kullanımı ve tahmini maliyetleri izleme

> [!NOTE]
> Bu makalede, birden çok Azure izleme özelliği genelinde kullanımı ve tahmini maliyetlerin nasıl görüntüleneceği açıklanmaktadır. Azure Izleyici 'nin belirli bileşenlerine ilişkin makaleler şunlardır:
> - [Azure Izleyici günlükleri ile kullanımı ve maliyetleri yönetme](manage-cost-storage.md) , veri saklama dönemini değiştirerek maliyetlerinizi nasıl denetleyeceğinizi ve veri kullanımınızın nasıl analiz edileceğini ve uyarılanacağını açıklar.
> - [Kullanım ve maliyetleri yönetme Application Insights](../../azure-monitor/app/pricing.md) , Application Insights veri kullanımının nasıl analiz edileceğini açıklar.

## <a name="azure-monitor-pricing-model"></a>Azure Izleyici fiyatlandırma modeli

Temel Azure Izleyici faturalandırma modeli, bulut kullanımı kolay, tüketim tabanlı bir fiyatlandırmadır ("Kullandıkça öde"). Yalnızca kullandığınız kadar ödersiniz. Fiyatlandırma ayrıntıları, [uyarı, ölçümler, bildirimler](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/)için kullanılabilir. 

Günlük verileri için Kullandıkça Öde modeline ek olarak Log Analytics, Kullandıkça Öde fiyatına kıyasla %25 ' e kadar tasarruf etmeniz için kapasite rezervasyonları vardır. Kapasite ayırma fiyatlandırması, 100 GB/gün üzerinden başlayan bir rezervasyon satın almanıza olanak sağlar. Rezervasyon düzeyinin üzerindeki tüm kullanımlar, Kullandıkça Öde fiyatı üzerinden faturalandırılır. Kapasite ayırma fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/) .

Bazı müşterilerin [eski Log Analytics fiyatlandırma katmanlarına](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers) ve [eski Kurumsal Application Insights fiyatlandırma katmanına](https://docs.microsoft.com/azure/azure-monitor/app/pricing#legacy-enterprise-per-node-pricing-tier)erişimi olur. 

## <a name="understanding-your-azure-monitor-costs"></a>Azure Izleyici maliyetlerinizi anlama

Maliyetleri anlamak için iki aşama vardır. İlk olarak Azure Izleyici 'yi izleme çözümünüz olarak düşünürken. 

### <a name="estimating-the-costs-to-manage-your-environment"></a>Ortamınızı yönetme maliyetlerini tahmin etme

Henüz Azure Izleyici günlüklerini kullanmıyorsanız, Azure izleyici 'yi kullanma maliyetini tahmin etmek için [Azure izleyici Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/?service=monitor) ' ı kullanabilirsiniz. Arama kutusuna "Azure Izleyici" girerek ve sonuçta elde edilen Azure Izleyici kutucuğuna tıklayarak başlayın. Sayfayı Azure Izleyici 'ye kaydırın ve tür açılan menüsünden seçeneklerden birini seçin:

- Ölçüm sorguları ve uyarıları  
- Log Analytics
- Application Insights 

Bunların her birinde Fiyatlandırma Hesaplayıcı, olası kullanımınıza göre olası maliyetlerinizi tahmin etmenize yardımcı olur. 

Örneğin, Log Analytics ile her bir VM 'den toplamak istediğiniz sanal makine sayısını ve GB veri miktarını girebilirsiniz. Genellikle 1 ila 3 GB veri ayı tipik bir Azure VM 'sinden alınır. Zaten Azure Izleyici günlüklerini değerlendiriyorsanız, kendi ortamınızdan veri istatistiklerinizi kullanabilirsiniz. [Izlenen sanal makinelerin sayısını](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-nodes-sending-data) ve [çalışma alanınızın veri hacmini](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume)nasıl belirleyeceğini öğrenmek için aşağıya bakın. 

Benzer şekilde Application Insights için, "uygulama etkinliğine göre verileri tahmin etme" işlevini etkinleştirirseniz, uygulamanız hakkında (istemci tarafı telemetri toplamanız durumunda ayda aylık istek ve sayfa görüntüleme istekleri) bir giriş sağlayabilirsiniz. ardından hesaplayıcı, benzer uygulamalar tarafından toplanan ortalama ve 90. yüzdebirlik veri miktarını size bildirir. Tabii ki bu uygulamalar Application Insights yapılandırma aralığını yaymıştır (bazıları varsayılan örneklemeye sahiptir, bazıları örnekleme içermez vb.), bu nedenle, örnekleme kullanarak ortanca düzeyinin altına aldığınız verilerin hacmini azaltmak için denetime sahip olursunuz. Ancak bu, benzer müşterilerin gördüğünü anlamak için bir başlangıç noktasıdır. Application Insights maliyetleri tahmin etme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/app/pricing#estimating-the-costs-to-manage-your-application) .

### <a name="understanding-your-usage-and-estimated-costs"></a>Kullanımınız ve Tahmini maliyetlerinizi anlama

Azure Izleyici 'yi kullanarak kullanımınızı anlamak ve izlemek önemlidir ve bunu kolaylaştırmak için zengin bir araç kümesi vardır. 

Azure, [Azure maliyet yönetimi + faturalandırma](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) hub 'ında yararlı bir işlevsellik sağlar. **Azure maliyet yönetimi + faturalandırma** hub 'ını açtıktan sonra, **maliyet yönetimi** ' ne tıklayın ve [kapsamı](https://docs.microsoft.com/azure/cost-management/understand-work-scopes) (araştırılacağı kaynak kümesi) seçin. 

Ardından, son 30 günün Azure Izleyici maliyetlerini görmek için, "birikmiş maliyetler" kutucuğuna tıklayın, göreli tarihler altında "son 30 gün" seçeneğini belirleyin ve hizmet adlarını seçen bir filtre ekleyin:

1. Azure İzleyici
2. Application Insights
3. Log Analytics
4. İçgörü ve Analiz

Bu, şöyle bir görünüme neden olur:

![Azure maliyet yönetimi ekran görüntüsü](./media/usage-estimated-costs/010.png)

Buradan, "kaynağa göre maliyet" görünümünde daha ayrıntılı bilgi edinmek için bu birikmiş maliyet özetinden ayrıntıya gidebilirsiniz. 

Kullanımınızı [Azure portalından indirerek](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal)kullanımınız daha derin bir şekilde anlaşılmalıdır. İndirilen elektronik tabloda günde Azure kaynağı başına kullanımı görebilirsiniz. Bu Excel elektronik tablosunda, Application Insights kaynaklarınızdan kullanım "Application Insights" ve "Log Analytics" göstermek için "ölçüm kategorisi" sütununda filtrelenebilir ve sonra "Contains" olan "örnek KIMLIĞI" sütununa bir filtre eklenerek bulunabilir Microsoft. Insights/bileşenler ".  Application Insights kullanımı, tüm Azure Izleyici bileşenleri için tek bir günlük arka ucu olduğundan, Log Analytics ölçüm kategorisiyle ölçü üzerinden raporlanır.  Yalnızca eski fiyatlandırma katmanlarında Application Insights kaynaklar ve çok adımlı Web testleri, Application Insights ölçüm kategorisiyle raporlanır.  Kullanım "tüketilen miktar" sütununda gösterilir ve her girdinin birimi "ölçü birimi" sütununda gösterilir.  [Microsoft Azure faturanızı anlamanıza](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)yardımcı olacak daha fazla ayrıntı bulabilirsiniz. 

> [!NOTE]
> **Maliyet yönetimi** 'Ni **Azure maliyet yönetimi + faturalandırma** merkezinde kullanmak, izleme maliyetlerini büyük ölçüde anlamak için tercih edilen yaklaşımdır.  [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understand-your-usage-and-estimate-costs) ve [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/pricing#understand-your-usage-and-estimate-costs) Için **kullanım ve tahmini maliyet** deneyimleri, Azure izleyici 'nin bu parçalarından her biri için daha derin öngörüler sağlar. 

Azure Izleyici kullanımınızı görüntülemeye yönelik başka bir seçenek de Izleyici hub 'ındaki **kullanım ve tahmini maliyetler** sayfasıdır. Bu, [uyarı, ölçümler, bildirimler](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/)ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/)gibi çekirdek izleme özelliklerinin kullanımını gösterir. Fiyatlandırma planlarındaki müşteriler 2018 Nisan 'dan önce kullanılabilir ve ayrıca, Öngörüler ve analiz teklifiyle satın alınan Log Analytics kullanımını da içerir.

Bu sayfada, kullanıcılar, abonelik başına toplanan son 31 gün boyunca kaynak kullanımını görüntüleyebilirler. kullanım eğilimlerini 31 günlük dönemde göstermek `Drill-ins`. Bu tahmin için çok fazla veri gelmesi gerekir, bu nedenle lütfen sayfa yüklenirken hasta olun.

Bu örnekte, izleme kullanımı ve elde edilen maliyetlerin tahmini gösterilmektedir:

![Kullanım ve tahmini maliyetler portalı ekran görüntüsü](./media/usage-estimated-costs/001.png)

Son 31 günlük dönemde kullanım eğilimlerini gösteren bir grafik açmak için aylık kullanım sütunundaki bağlantıyı seçin: 

![Düğüm başına çubuk grafik ekran görüntüsü](./media/usage-estimated-costs/002.png)

## <a name="operations-management-suite-subscription-entitlements"></a>Operations Management Suite abonelik yetkilendirmeleri

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler, [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing)için düğüm başına veri alma yetkilendirmelerinde uygun değildir. Log Analytics çalışma alanları için bu yetkilendirmeleri veya belirli bir abonelikteki kaynakları Application Insights almak için: 

- Log Analytics çalışma alanları "düğüm başına (OMS)" fiyatlandırma katmanını kullanmalıdır.
- Application Insights kaynaklar "Kurumsal" fiyatlandırma katmanını kullanmalıdır.

Kuruluşunuzun satın aldığı paketin düğüm sayısına bağlı olarak, bazı abonelikleri Kullandıkça Öde (GB başına) fiyatlandırma katmanına taşımak avantajlı olabilir, ancak bu dikkatli bir göz önünde bulundurmanız gerekir.

> [!WARNING]
> Kuruluşunuz E1 ve E2 Microsoft Operations Management Suite satın alıyorsa, genellikle Log Analytics çalışma alanlarınızı "düğüm başına (OMS)" fiyatlandırma katmanında ve "Kurumsal" fiyatlandırma katmanında Application Insights kaynaklarınızın tutulması en iyisidir. 
>

