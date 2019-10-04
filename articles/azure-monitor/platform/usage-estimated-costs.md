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
ms.openlocfilehash: 787618b59cd18dd4c38892ddf0861808211671cb
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936617"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>Azure Izleyici 'de kullanımı ve tahmini maliyetleri izleme

> [!NOTE]
> Bu makalede, farklı fiyatlandırma modelleri için birden çok Azure izleme özelliği genelinde kullanım ve tahmini maliyetlerin nasıl görüntüleneceği açıklanmaktadır. Azure Izleyici 'nin belirli bileşenlerine ilişkin makaleler şunlardır:
> - [Azure Izleyici günlükleri ile kullanımı ve maliyetleri yönetme](manage-cost-storage.md) , veri saklama dönemini değiştirerek maliyetlerinizi nasıl denetleyeceğinizi ve veri kullanımınızın nasıl analiz edileceğini ve uyarılanacağını açıklar.
> - [Kullanım ve maliyetleri yönetme Application Insights](../../azure-monitor/app/pricing.md) , Application Insights veri kullanımının nasıl analiz edileceğini açıklar.

Azure portal Monitor hub 'ında **kullanım ve tahmini maliyetler** sayfası, [uyarı, ölçümler, bildirimler](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/)ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/)gibi çekirdek izleme özelliklerinin kullanımını açıklar. Fiyatlandırma planlarındaki müşteriler 2018 Nisan 'dan önce kullanılabilir ve ayrıca, Öngörüler ve analiz teklifiyle satın alınan Log Analytics kullanımını da içerir.

Bu sayfada, kullanıcılar, abonelik başına toplanan son 31 gün boyunca kaynak kullanımını görüntüleyebilirler. Detaya gitme, 31 günlük dönem boyunca kullanım eğilimlerini gösterir. Bu tahmin için çok fazla veri gelmesi gerekir, bu nedenle lütfen sayfa yüklenirken hasta olun.

Bu örnekte, izleme kullanımı ve elde edilen maliyetlerin tahmini gösterilmektedir:

![Kullanım ve tahmini maliyetler portalı ekran görüntüsü](./media/usage-estimated-costs/001.png)

Son 31 günlük dönemde kullanım eğilimlerini gösteren bir grafik açmak için aylık kullanım sütunundaki bağlantıyı seçin:

![Düğüm başına çubuk grafik ekran görüntüsü](./media/usage-estimated-costs/002.png)

Diğer bir benzer kullanım ve maliyet özeti aşağıda verilmiştir. Bu örnekte, yeni Nisan 2018 tüketim tabanlı fiyatlandırma modelinde bir abonelik gösterilmektedir. Düğüm tabanlı tüm faturalandırmaya yer olmadığını göz önünde edin. Log Analytics ve Application Insights veri alımı ve saklama, artık yeni bir ortak ölçüm üzerinden raporlanır.

![Kullanım ve tahmini maliyetler portalı ekran görüntüsü-Nisan 2018 fiyatlandırması](./media/usage-estimated-costs/003.png)

## <a name="pricing-model"></a>Fiyatlandırma modeli

Nisan 2018 ' de [Yeni bir izleme fiyatlandırma modeli yayımlanmıştır](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Bu özellikler bulutta kullanımı kolay, tüketim tabanlı fiyatlandırma ("Kullandıkça öde"). Düğüm tabanlı taahhütler olmadan yalnızca kullandığınız kadar ödersiniz. Yeni fiyatlandırma modelinin ayrıntıları, [uyarı, ölçümler, bildirimler](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/)için kullanılabilir. 

Kullandıkça Öde modeline ek olarak, Eylül 2019 ' de, Kullandıkça Öde fiyatına kıyasla% 25 ' e kadar tasarruf etmeniz için kapasite ayırmaları ekledik Log Analytics. Kapasite ayırma fiyatlandırması, 100 GB/gün üzerinden başlayan bir rezervasyon satın almanıza olanak sağlar. Rezervasyon düzeyinin üzerindeki tüm kullanımlar, Kullandıkça Öde fiyatı üzerinden faturalandırılır. Kapasite ayırma fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/) .

2 Nisan 2018 ' den sonra Log Analytics veya Application Insights olan müşteriler için yeni fiyatlandırma modeli tek seçenektir. Bu hizmetleri zaten kullanan müşteriler için yeni fiyatlandırma modeline taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modelinin etkisi değerlendiriliyor
Yeni fiyatlandırma modelinin, izleme kullanım düzenlerine göre her müşteri üzerinde farklı etkileri olacaktır. 2 Nisan 2018 tarihinden önce Log Analytics veya Application Insights kullanan müşteriler için, Azure Izleyici 'de **kullanım ve tahmini maliyet** sayfası, yeni fiyatlandırma modeline taşındıklarında maliyetlerdeki tüm değişiklikleri tahmin eder. Aboneliği yeni modele taşımanın yolunu sağlar. Çoğu müşteri için yeni fiyatlandırma modeli avantajlı olacaktır. Özellikle yüksek veri kullanımı desenleri veya daha yüksek maliyetli bölgelerde olan müşteriler için bu durum olmayabilir.

**Kullanım ve tahmini maliyetler** sayfasında seçtiğiniz Aboneliklerle ilgili maliyetlerinizin bir tahminini görmek için sayfanın üst kısmındaki mavi başlık ' ı seçin. Bu, yeni fiyatlandırma modelinin benimseme düzeyi olduğundan, bu aboneliği tek seferde yapmak en iyisidir.

![Yeni fiyatlandırma modelinde kullanımı ve tahmini maliyetleri izleme ekran görüntüsü](./media/usage-estimated-costs/004.png)

Yeni sayfada yeşil bir başlık içeren önceki sayfanın benzer bir sürümü gösterilmektedir:

![Geçerli fiyatlandırma modelinde kullanımı ve tahmini maliyetleri izleme ekran görüntüsü](./media/usage-estimated-costs/005.png)

Bu sayfada Ayrıca yeni fiyatlandırma modeline karşılık gelen farklı ölçüm kümesi de gösterilmektedir. Bu liste bir örnektir:

- Her düğüm için Öngörüler ve analiz Tics\fazla kullanım
- Her düğüm için Öngörüler ve analiz ticiyi
- Application ınsilars\temel Fazla Kullanım Verileri
- Uygulama ınsilars\eklenen veriler

Yeni fiyatlandırma modelinde düğüm tabanlı dahil edilen veri ayırmaları yok. Bu nedenle, bu veri alma ölçümleri, **paylaşılan Services\Data**alımı adlı yeni bir ortak veri alımı ölçesinde birleştirilir. 

Log Analytics veya daha yüksek maliyetlere sahip bölgelerde Application Insights veri için başka bir değişiklik vardır. Bu yüksek maliyetli bölgelere yönelik veriler, yeni bölgesel ölçümler ile gösterilir. Veri alımı bir örnektir **(ABD Orta Batı)** .

> [!NOTE]
> Abonelik başına tahmini maliyetler, Operations Management Suite (OMS) aboneliğinin düğüm başına hesap düzeyi yetkilendirmelerine göre hesaba katmaz. Bu durumda yeni fiyatlandırma modelinin daha ayrıntılı bir tartışmasını öğrenmek için hesap temsilcinize başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite abonelik yetkilendirmeleri

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler, [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing)için düğüm başına veri alma yetkilendirmelerinde uygun değildir. Log Analytics çalışma alanları için bu yetkilendirmeleri veya belirli bir abonelikteki kaynakları Application Insights almak için: 

- Aboneliğin fiyatlandırma modelinin, Nisan 2018 modelinde kalması gerekir.
- Log Analytics çalışma alanları "düğüm başına (OMS)" fiyatlandırma katmanını kullanmalıdır.
- Application Insights kaynaklar "Kurumsal" fiyatlandırma planını kullanmalıdır.

Kuruluşunuzun satın aldığı paketin düğüm sayısına bağlı olarak, bazı aboneliklerin yeni fiyatlandırma modeline taşınması avantajlı olabilir, ancak bu dikkatli bir göz önünde bulundurmanız gerekir. Genel olarak, yukarıda açıklandığı gibi önceden Nisan 2018 modelinde kalmak önerilir.

> [!WARNING]
> Kuruluşunuz E1 ve E2 Microsoft Operations Management Suite satın alıyorsa, genellikle aboneliklerinizi önceden Nisan 2018 fiyatlandırma modelinde tutmak en iyisidir. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıdığınız zaman değişir

Yeni fiyatlandırma modeli Log Analytics ve Application Insights fiyatlandırma seçeneklerini yalnızca tek bir katmana (veya plana) basitleştirir. Bir aboneliği yeni fiyatlandırma modeline taşımak şu şekilde olur:

- Her bir Log Analytics fiyatlandırma katmanını yeni bir GB başına katmana değiştirin (Azure Resource Manager "pergb2018" olarak adlandırılır)
- Kurumsal plandaki Application Insights kaynaklar temel plana dönüştürülür.

Maliyet tahmini, bu değişikliklerin etkilerini gösterir.

> [!WARNING]
> Yeni fiyatlandırma modeline taşıdığınız bir abonelikte [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) veya [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) dağıtmak üzere Azure Resource Manager veya PowerShell kullanıyorsanız, burada önemli bir notun olması gerekir. Log Analytics veya "temel Application Insights" için "pergb2018" dışında bir fiyatlandırma katmanı/planı belirtirseniz, geçersiz bir fiyatlandırma katmanı/plan belirtilmesi nedeniyle dağıtım başarısız olmak yerine, bu işlem başarılı olur **ancak yalnızca geçerli fiyatlandırma katmanını kullanır/ plan** (Bu, geçersiz bir fiyatlandırma katmanı iletisinin oluşturulduğu Log Analytics ücretsiz katman için geçerli değildir).
>

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Belirli bir abonelik için yeni fiyatlandırma modelini benimsemeye karar verdiyseniz, her bir Application Insights kaynağına gidin, **kullanımı ve tahmini maliyetleri** açın ve temel fiyatlandırma katmanında olduğundan emin olun ve her bir Log Analytics çalışma alanına gidin.  **Fiyatlandırma Katmanı** sayfası ve **GB başına (2018)** fiyatlandırma katmanında değişiklik. 

> [!NOTE]
> Belirli bir abonelik içindeki tüm Application Insights kaynaklarının ve Log Analytics çalışma alanlarının en yeni fiyatlandırma modelini benimseme gereksinimi, daha fazla esneklik ve daha kolay yapılandırmaya olanak tanıyacak şekilde kaldırılmıştır. 

## <a name="automate-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline geçmeyi otomatikleştirme

Yukarıda belirtildiği gibi, bir abonelikteki tüm izleme kaynaklarını aynı anda yeni fiyatlandırma modeline taşıma gereksinimi yoktur ve bu nedenle ``migratetonewpricingmodel`` eylemi artık herhangi bir etkiye sahip olmayacaktır. Artık Application Insights kaynakları ve Log Analytics çalışma alanlarını en yeni fiyatlandırma katmanlarına ayrı olarak taşıyabilirsiniz.  

Bu değişikliği otomatik hale getirmek Application Insights, [set-AzureRmApplicationInsightsPricingPlan](https://docs.microsoft.com/powershell/module/azurerm.applicationinsights/set-azurermapplicationinsightspricingplan) ile ``-PricingPlan "Basic"`` ve [set-Azurermoperationalınsightsworkspace](https://docs.microsoft.com/powershell/module/AzureRM.OperationalInsights/Set-AzureRmOperationalInsightsWorkspace) kullanılarak ``-sku "PerGB2018"`` ile Log Analytics için belgelenmiştir. 
