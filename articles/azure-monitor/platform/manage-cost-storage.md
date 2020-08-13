---
title: Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme
description: Azure Izleyici 'de Log Analytics çalışma alanınızın fiyatlandırma planını değiştirme ve veri hacmini ve bekletme ilkesini yönetme hakkında bilgi edinin.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 391a5f054c5d80b255fd333ea416900c8c5ab6d1
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88135428"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme    

> [!NOTE]
> Bu makalede, Azure Izleyici günlüklerine yönelik maliyetlerinizi anlama ve denetleme işlemlerinin nasıl yapılacağı açıklanır. [Kullanımı ve tahmini maliyetleri izleyen](usage-estimated-costs.md) ilgili bir makale, farklı fiyatlandırma modelleri için birden çok Azure izleme özelliği genelinde kullanım ve tahmini maliyetlerin nasıl görüntüleneceğini açıklar. Bu makalede gösterilen tüm fiyatlar ve maliyetler yalnızca örnek amaçlıdır. 

Azure Izleyici günlükleri, kuruluşunuzda bulunan veya Azure 'da dağıtılan herhangi bir kaynakta çok büyük miktarlarda veri toplamayı, dizinlemesini ve depolamayı, ölçeklendirmek ve desteklemek üzere tasarlanmıştır.  Bu, kuruluşunuz için bir birincil sürücü olabilir, ancak maliyet verimliliği sonunda temel alınan sürücü. Bu uçta, bir Log Analytics çalışma alanının maliyetinin yalnızca toplanan verilerin hacmine dayanmadığını anlamak önemlidir, Ayrıca, seçilen plana de bağlıdır ve bağlı kaynaklarınızdan oluşturulan verileri depolamayı ne kadar tercih edersiniz.  

Bu makalede, alınan veri hacmi ve depolama büyümesinin nasıl proaktif bir şekilde izleneceğini ve ilgili maliyetleri denetlemek için sınırları tanımlamanızı inceliyoruz. 

## <a name="pricing-model"></a>Fiyatlandırma modeli

Log Analytics için varsayılan fiyatlandırma, veri hacmine dayalı ve isteğe bağlı olarak daha uzun veri saklama için bir **Kullandıkça Öde** modelidir. Veri hacmi GB cinsinden depolanacak verilerin boyutu olarak ölçülür (10 ^ 9 bayt). Her Log Analytics çalışma alanı ayrı bir hizmet olarak ücretlendirilir ve Azure aboneliğiniz için faturaya katkıda bulunur. Veri alma miktarı aşağıdaki faktörlere göre önemli ölçüde olabilir: 

  - Etkin yönetim çözümlerinin sayısı ve yapılandırmaları
  - İzlenen VM sayısı
  - İzlenen her bir VM 'den toplanan verilerin türü 
  
Kullandıkça Öde modeline ek olarak Log Analytics, Kullandıkça Öde fiyatına kıyasla %25 ' e kadar tasarruf etmeniz için **Kapasite rezervasyon** katmanlarına sahiptir. Kapasite ayırma fiyatlandırması, 100 GB/gün üzerinden başlayan bir rezervasyon satın almanıza olanak sağlar. Rezervasyon düzeyinin üzerindeki tüm kullanımlar, Kullandıkça Öde fiyatı üzerinden faturalandırılır. Kapasite rezervasyon katmanlarında 31 günlük taahhüt dönemi vardır. Taahhüt dönemi boyunca, daha yüksek düzey kapasite rezervasyon katmanına (31 günlük taahhüt dönemini yeniden başlatacak) geçebilirsiniz, ancak taahhüt dönemi tamamlanana kadar, Kullandıkça Öde veya daha düşük bir kapasite Ayırma katmanına geri dönemezsiniz. Kapasite rezervasyon katmanları için faturalandırma, günlük olarak yapılır. Log Analytics Kullandıkça öde ve kapasite rezervasyon fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/) . 

Tüm fiyatlandırma katmanlarında, bir olayın veri boyutu, bu olay için Log Analytics depolanan özelliklerin dize gösteriminden, verilerin bir aracıdan gönderilip gönderilmediği veya alma işlemi sırasında eklenip eklenmeyeceğini belirtir. Bu, veriler toplandıktan sonra eklenen tüm [özel alanları](custom-fields.md) içerir ve sonra Log Analytics depolanır. Bazı [Log Analytics standart özellikleri](log-standard-properties.md)de dahil olmak üzere tüm veri türlerinde ortak olan çeşitli özellikler, olay boyutu hesaplamasında dışarıda bırakılır. Bu,,, `_ResourceId` `_ItemId` ve içerir `_IsBillable` `_BilledSize` `Type` . Log Analytics içinde depolanan diğer tüm özellikler, olay boyutunun hesaplanmasına dahil edilir. Bazı veri türleri, veri alma ücretlerinden (örneğin, AzureActivity, sinyal ve kullanım türleri) tamamen ücretsizdir. Bir olayın veri alımı için faturalandırmaya dahil edilip edilmeyeceğini öğrenmek için, `_IsBillable` özelliği [aşağıda](#data-volume-for-specific-events)gösterildiği gibi kullanabilirsiniz. Kullanım GB (1.0 E9 bayt) olarak raporlanır. 

Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/pricing/details/security-center/), [Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/) ve [yapılandırma yönetimi](https://azure.microsoft.com/pricing/details/automation/) gibi bazı çözümlerin kendi fiyatlandırma modellerine sahip olduğunu unutmayın. 

### <a name="log-analytics-dedicated-clusters"></a>Adanmış kümeler Log Analytics

Log Analytics adanmış kümeler, [müşteri tarafından yönetilen anahtarlar](customer-managed-keys.md)gibi gelişmiş senaryoları desteklemek üzere tek bir yönetilen Azure Veri Gezgini kümesinde çalışma alanlarının koleksiyonlarıdır.  Log Analytics adanmış kümeler yalnızca, Kullandıkça Öde fiyatlandırmasıyla karşılaştırıldığında %25 oranında bir indirim ile 1000 GB/gün üzerinden başlayan bir kapasite ayırma fiyatlandırma modelini destekler. Rezervasyon düzeyinin üzerindeki tüm kullanımlar, Kullandıkça Öde fiyatı üzerinden faturalandırılır. Küme kapasitesi rezervasyonunun, ayırma düzeyi arttırılarak 31 günlük taahhüt süresi vardır. Taahhüt dönemi boyunca kapasite ayırma düzeyi düşürülemez, ancak herhangi bir zamanda artırılabilir. [Log Analytics kümeleri oluşturma](customer-managed-keys.md#create-cluster-resource) ve [çalışma alanlarını onunla ilişkilendirme](customer-managed-keys.md#workspace-association-to-cluster-resource)hakkında daha fazla bilgi edinin.  

Küme kapasitesi ayırma düzeyi, altındaki parametresi kullanılarak Azure Resource Manager ile program aracılığıyla aracılığıyla yapılandırılır `Capacity` `Sku` . `Capacity`GB cinsinden belirtilir ve 100 GB/gün artışlarla 1000 GB veya daha fazla değere sahip olabilir. Bu, [Azure izleyici müşteri tarafından yönetilen anahtarı ile](customer-managed-keys.md#create-cluster-resource)ayrıntılıdır. Kümenizin 2000 GB/gün üzerinde bir ayırmaya ihtiyacı varsa bizimle iletişime geçin [LAIngestionRate@microsoft.com](mailto:LAIngestionRate@microsoft.com) .

Bir kümede kullanıma yönelik iki faturalandırma modu vardır. Bu, `billingType` [kümeniz yapılandırılırken](customer-managed-keys.md#cmk-management)parametresi tarafından belirtilebilir. İki mod şunlardır: 

1. **Küme**: Bu durumda (varsayılan), alınan verilerin faturalandırılması küme düzeyinde yapılır. Kümeyle ilişkilendirilen her çalışma alanından alınan veri miktarları, küme için günlük faturanızı hesaplamak üzere toplanır. [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) 'ndeki düğüm başına ayırmaların, kümedeki tüm çalışma alanları genelinde toplanan verilerin toplanmasından önce çalışma alanı düzeyinde uygulandığını unutmayın. 

2. **Çalışma alanları**: kümenizin kapasite ayırma maliyeti, kümedeki çalışma alanlarıyla orantılı olarak atanır (her çalışma alanı Için [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) 'nden düğüm başına ayırmalar için hesap oluşturulduktan sonra). Bir gün için bir çalışma alanına alınan toplam veri birimi kapasite rezervasyonundan azsa, her çalışma alanı, kapasite rezervasyonunun bir kısmı faturalanarak, her bir çalışma alanı geçerli GB başına kapasite rezervasyon fiyatı üzerinden faturalandırılır ve kapasite rezervasyonunun kullanılmayan bölümü küme kaynağına faturalandırılır. Bir gün için bir çalışma alanına alınan toplam veri birimi kapasite ayırmasından daha fazla ise, her çalışma alanı, o gün için ayrılan verilerin kesiri ve her çalışma alanı ise kapasite rezervasyonunun üzerinde alınan verilerin bir kesri için her bir alan için faturalandırılır. Bir gün için bir çalışma alanına alınan toplam veri birimi kapasite rezervasyonunun üzerinde ise, küme kaynağına faturalandırılan bir şey yoktur.

Küme faturalandırma seçeneklerinde, veri saklama, çalışma alanı düzeyinde faturalandırılır. Küme faturalandırma, küme oluşturulduğunda, çalışma alanlarının kümeyle ilişkilendirilmediğine bakılmaksızın başlar. Ayrıca, bir kümeyle ilişkili çalışma alanlarının artık fiyatlandırma katmanına sahip olmadığını unutmayın.

## <a name="estimating-the-costs-to-manage-your-environment"></a>Ortamınızı yönetme maliyetlerini tahmin etme 

Henüz Azure Izleyici günlüklerini kullanmıyorsanız, Log Analytics kullanmanın maliyetini tahmin etmek için [Azure izleyici Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/?service=monitor) ' ı kullanabilirsiniz. Arama kutusuna "Azure Izleyici" girerek ve sonuçta elde edilen Azure Izleyici kutucuğuna tıklayarak başlayın. Sayfayı Azure Izleyici 'ye kaydırın ve tür açılan menüsünden Log Analytics ' yi seçin.  Buraya, her bir VM 'den toplamak istediğiniz sanal makine sayısını ve GB veri miktarını girebilirsiniz. Genellikle 1 ila 3 GB veri ayı tipik bir Azure VM 'sinden alınır. Zaten Azure Izleyici günlüklerini değerlendiriyorsanız, kendi ortamınızdan veri istatistiklerinizi kullanabilirsiniz. [Izlenen sanal makinelerin sayısını](#understanding-nodes-sending-data) ve [çalışma alanınızın veri hacmini](#understanding-ingested-data-volume)nasıl belirleyeceğini öğrenmek için aşağıya bakın. 

## <a name="understand-your-usage-and-estimate-costs"></a>Kullanımınız ve Tahmini maliyetlerinizi anlayın

Azure Izleyici günlüklerini şimdi kullanıyorsanız, maliyetlerin en son kullanım desenlerine göre büyük olasılıkla ne olduğunu anlamak kolaydır. Bunu yapmak için, veri kullanımını gözden geçirmek ve çözümlemek üzere **Log Analytics kullanımı ve tahmini maliyetleri** kullanın. Bu, her bir çözüm tarafından ne kadar veri toplandığını, ne kadar veri tutulup tutulmadığını ve dahil edilen miktarın ötesinde ek bir bekletme miktarına göre maliyetlerinizin bir tahminini gösterir.

![Kullanım ve tahmini maliyetler](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Verilerinizi daha ayrıntılı bir şekilde araştırmak için **kullanım ve tahmini maliyetler** sayfasındaki grafiklerin sağ üst köşesindeki simgeye tıklayın. Artık kullanımınız hakkında daha fazla bilgi edinmek için bu sorguyla birlikte çalışabilirsiniz.  

![Günlükler görünümü](media/manage-cost-storage/logs.png)

**Kullanım ve tahmini maliyetler** sayfasında, veri hacminin ayı için gözden geçirebilirsiniz. Bu, Log Analytics çalışma alanınızda alınan ve saklanan tüm faturalanabilir verileri içerir.  
 
Log Analytics ücretleri Azure faturanızda eklenir. Azure faturanızın ayrıntılarını Azure portal Faturalandırma bölümünde veya [Azure faturalama portalı](https://account.windowsazure.com/Subscriptions)görebilirsiniz.  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Azure faturanızda Log Analytics kullanımı görüntüleme 

Azure, [Azure maliyet yönetimi + faturalandırma](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) hub 'ında yararlı bir işlevsellik sağlar. Örneğin, "maliyet analizi" işlevi, Azure kaynakları için kullandığınız süreyi görüntülemenize olanak sağlar. İlk olarak, "kaynak türü" ile bir filtre ekleyin (Log Analytics için Microsoft. operationalınsights/çalışma alanı ve Microsoft. operationalınsights/çalışma alanı Log Analytics kümeler için), Log Analytics harcamalarınızı izlemenize imkan tanır. "Grup ölçütü" için "ölçüm kategorisi" veya "ölçüm" seçeneğini belirleyin.  Azure Güvenlik Merkezi ve Azure Sentinel gibi diğer hizmetlerin ayrıca kullanımını Log Analytics çalışma alanı kaynaklarına karşı faturalayacağını unutmayın. Hizmet adı eşlemesini görmek için bir grafik yerine tablo görünümünü seçebilirsiniz. 

Kullanımınızı daha iyi anlamak için [Azure portalından kullanım bilgilerinizi indirebilirsiniz](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal). İndirilen elektronik tabloda Azure kaynağı (örneğin, Log Analytics çalışma alanı) başına günlük kullanımınızı görebilirsiniz. Bu Excel elektronik tablosunda, Log Analytics çalışma alanlarınızın kullanımı "ölçüm kategorisi" sütununda "Log Analytics" göstermek için önce filtrelenebilir. içgörüler ve çözümlemeler "(Eski fiyatlandırma katmanlarından bazıları tarafından kullanılır) ve" Azure Izleyici "(kapasite ayırma fiyatlandırma katmanları tarafından kullanılır) ve sonra" çalışma alanı "veya" kümeyi içerir "olan" örnek KIMLIĞI "sütununa bir filtre ekleniyor (Log Analytics küme kullanımı dahil olmak üzere). Kullanım "tüketilen miktar" sütununda gösterilir ve her girdinin birimi "ölçü birimi" sütununda gösterilir.  [Microsoft Azure faturanızı anlamanıza](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) yardımcı olmak için daha fazla ayrıntı sağlanır. 

## <a name="changing-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

Çalışma alanınızın Log Analytics fiyatlandırma katmanını değiştirmek için 

1. Azure portal, çalışma alanınızdaki **kullanım ve tahmini maliyetler** ' i açarak bu çalışma alanı için kullanılabilir olan fiyatlandırma katmanlarının her birinin listesini görürsünüz.

2. Fiyatlandırma katmanlarının her biri için tahmini maliyetleri gözden geçirin. Bu tahmin, son 31 güne kadar kullanım için belirlenir. bu nedenle, bu maliyet tahmini, tipik kullanımınızın temsilciyle ilgili son 31 güne dayanır. Aşağıdaki örnekte, son 31 günden veri desenlerine bağlı olarak, bu çalışma alanının 100 GB/gün kapasite rezervasyon katmanıyla (#2) karşılaştırıldığında Kullandıkça Öde katmanında (#1) daha az ücret aldığına bakabilirsiniz.  

    ![Fiyatlandırma katmanları](media/manage-cost-storage/pricing-tier-estimated-costs.png)

3. Son 31 güne göre Tahmini maliyetleri gözden geçirdikten sonra, fiyatlandırma katmanını değiştirmeye karar verirseniz **Seç**' e tıklayın.  

Ayrıca, parametresini kullanarak (Azure Resource Manager şablonunda) [Azure Resource Manager aracılığıyla fiyatlandırma katmanını ayarlayabilirsiniz](template-workspace-configuration.md#configure-a-log-analytics-workspace) `sku` `pricingTier` . 

## <a name="legacy-pricing-tiers"></a>Eski fiyatlandırma katmanları

2 Nisan 2018 tarihinden önce Log Analytics çalışma alanına veya Application Insights kaynağına sahip olan abonelikler, 1 Şubat 2019 ' den önce başlatılan bir Kurumsal Anlaşma bağlı olmaya devam edecektir: **ücretsiz**, **tek başına (GB başına)** ve **düğüm başına (OMS)**.  Ücretsiz fiyatlandırma katmanındaki çalışma alanlarında, günlük veri alımı 500 MB ile sınırlıdır ( [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/)tarafından toplanan güvenlik verileri türleri hariç) ve veri saklama süresi 7 gün ile sınırlıdır. Ücretsiz fiyatlandırma katmanı yalnızca değerlendirme amaçlarıyla tasarlanmıştır. Tek başına veya düğüm başına fiyatlandırma katmanlarında çalışma alanlarında, 30 ila 730 güne sahip kullanıcı tarafından yapılandırılabilir bir bekletme vardır.

Tek başına fiyatlandırma katmanındaki kullanım, alınan veri hacmi tarafından faturalandırılır. **Log Analytics** hizmetinde raporlanır ve ölçer "veri çözümlendi" olarak adlandırılmıştır. 

Bir saat ayrıntı düzeyi temelinde izlenen VM başına (düğüm) düğüm başına fiyatlandırma katmanı ücretleri. İzlenen her düğüm için, çalışma alanı faturalandırılmamış günde 500 MB veri tahsis edilir. Bu ayırma, çalışma alanı düzeyinde toplanır. Günlük toplam veri ayırmanın üzerine gelen veriler, veri fazla yaşı olarak GB başına faturalandırılır. Faturanızda, çalışma alanı düğüm başına fiyatlandırma katmanındaysa hizmetin Log Analytics kullanımı için **öngörü ve analiz** olacağını unutmayın. Kullanım üç ölçüm üzerinden bildirilir:

1. Düğüm: Bu, düğüm * ay birimleri cinsinden izlenen düğümlerin (VM) sayısı için kullanımdır.
2. Düğüm başına veri fazla kullanımı: Bu, toplanan veri ayırmanın fazla olması halinde alınan veri GB sayısıdır.
3. Düğüm başına dahil edilen veri: Bu, toplanan veri ayırmanın kapsamına giren verilerin miktarıdır. Bu ölçüm, Azure Güvenlik Merkezi tarafından kapsanan veri miktarını göstermek için çalışma alanı tüm fiyatlandırma katmanlarında olduğunda da kullanılır.

> [!TIP]
> Çalışma alanınızın, **düğüm başına** fiyatlandırma katmanına erişimi varsa, ancak Kullandıkça Öde katmanında maliyeti daha az olup olmadığını merak ediyorsanız, kolayca bir öneri almak için [aşağıdaki sorguyu kullanabilirsiniz](#evaluating-the-legacy-per-node-pricing-tier) . 

2016 Nisan 'dan önce oluşturulan çalışma alanları, 30 ve 365 günün sabit veri bekletmesini içeren orijinal **Standart** ve **Premium** fiyatlandırma katmanlarına de erişebilir. Yeni çalışma alanları **Standart** veya **Premium** fiyatlandırma katmanlarında oluşturulamaz ve bir çalışma alanı bu katmanlardan taşınmışsa, geri taşınamaz. Bu eski katmanlara yönelik veri alımı ölçümleri "veri çözümlendi" olarak adlandırılır.

Ayrıca, eski Log Analytics katmanların kullanımı ve [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/)için kullanımın faturalandırılması arasında bazı davranışlar da vardır. 

1. Çalışma alanı eski standart veya Premium katmanındaysa Azure Güvenlik Merkezi, düğüm başına değil yalnızca Log Analytics veri alımı için faturalandırılır.
2. Çalışma alanı eski düğüm başına katmanındaysa Azure Güvenlik Merkezi, geçerli [Azure Güvenlik Merkezi düğüm tabanlı fiyatlandırma modeli](https://azure.microsoft.com/pricing/details/security-center/)kullanılarak faturalandırılacaktır. 
3. Diğer fiyatlandırma katmanlarında (kapasite rezervasyonları dahil), Azure Güvenlik Merkezi 19 Haziran 2017 ' den önce etkinleştirilmişse, Azure Güvenlik Merkezi yalnızca Log Analytics veri alımı için faturalandırılır. Aksi takdirde, Azure Güvenlik Merkezi, geçerli Azure Güvenlik Merkezi düğüm tabanlı fiyatlandırma modeli kullanılarak faturalandırılır.

Fiyatlandırma Katmanı sınırlamaları hakkında daha fazla ayrıntı için [Azure aboneliği ve hizmet sınırları, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces)sunulmaktadır.

Eski fiyatlandırma katmanlarının hiçbirinde bölgesel tabanlı fiyatlandırma yoktur.  

> [!NOTE]
> System Center için OMS E1 Suite, OMS E2 Suite veya OMS eklentisi satın alma işleminden gelen yetkilendirmeleri kullanmak için *düğüm başına* fiyatlandırma katmanını Log Analytics seçin.

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Aşağıdaki adımlarda, çalışma alanınızda günlük verilerinin ne kadar süreyle saklanacağını nasıl yapılandıracağınız açıklanır. Veri saklama, eski ücretsiz fiyatlandırma katmanını kullanmadıkça tüm çalışma alanları için 30 ila 730 gün (2 yıl) yapılandırılabilir. Daha uzun veri saklama fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/) . 

### <a name="default-retention"></a>Varsayılan saklama

Çalışma alanınız için varsayılan saklama alanını ayarlamak için 
 
1. Azure portal, çalışma alanınızdan sol bölmeden **kullanım ve tahmini maliyetler** ' i seçin.
2. **Kullanım ve tahmini maliyetler** sayfasında, sayfanın üst kısmından **veri bekletme** ' ye tıklayın.
3. Bölmede kaydırıcıyı kullanarak gün sayısını artırın veya azaltın ve ardından **Tamam**'a tıklayın.  *Ücretsiz* katmanınız varsa, veri saklama süresini değiştiremeyeceksiniz ve bu ayarı denetlemek için ücretli katmana yükseltmeniz gerekir.

    ![Çalışma alanı verilerini bekletme ayarını değiştir](media/manage-cost-storage/manage-cost-change-retention-01.png)

Bekletme düşürüldü ise, yeni saklama ayarından daha eski olan veriler kaldırılmadan birkaç gün yetkisiz kullanım süresi vardır. 

Saklama Ayrıca parametresi kullanılarak [Azure Resource Manager ile de ayarlanabilir](template-workspace-configuration.md#configure-a-log-analytics-workspace) `retentionInDays` . Veri bekletmesini 30 güne ayarladığınızda, parametresini kullanarak eski verilerin hemen temizliğini tetikleyebilirsiniz `immediatePurgeDataOn30Days` (birkaç günlük yetkisiz kullanım süresini ortadan kaldırır). Bu, acil veri kaldırma işleminin zorunludur uyumlulukla ilgili senaryolar için yararlı olabilir. Bu anlık Temizleme işlevselliği yalnızca Azure Resource Manager aracılığıyla sunulur. 

30 günlük tutulan çalışma alanları, verileri 31 gün boyunca koruyabilir. Verilerin yalnızca 30 gün boyunca tutulması gerekiyorsa, bekletmenin 30 güne ve parametresiyle ayarlanması için Azure Resource Manager kullanın `immediatePurgeDataOn30Days` .  

İki veri türü-- `Usage` ve `AzureActivity` --Varsayılan olarak en az 90 gün boyunca tutulur ve bu 90 günlük bekletme için ücret alınmaz. Çalışma alanı bekletme 90 gün üzerinde arttırılırsa, bu veri türlerini bekletme de artacaktır.  Bu veri türleri de veri alma ücretlerinden ücretsizdir. 

Çalışma alanı tabanlı Application Insights kaynak (,,,,,,,, `AppAvailabilityResults` `AppBrowserTimings` `AppDependencies` `AppExceptions` `AppEvents` `AppMetrics` `AppPageViews` `AppPerformanceCounters` `AppRequests` , `AppSystemEvents` ve) veri türleri `AppTraces` de varsayılan olarak 90 gün boyunca tutulur ve bu 90 günlük bekletmeye yönelik ücret alınmaz. Saklama, veri türü işlevselliğine göre bekletme kullanılarak ayarlanabilir. 

Log Analytics [Temizleme API 'sinin](https://docs.microsoft.com/rest/api/loganalytics/workspacepurge/purge) bekletme faturalandırmayı etkilemediğini ve çok sınırlı durumlar için kullanılması amaçlandığını unutmayın. Bekletme faturanızı azaltmak için bekletme döneminin, çalışma alanı ya da belirli veri türleri için azaltılmalıdır. 

### <a name="retention-by-data-type"></a>Veri türüne göre bekletme

Ayrıca, 30 ila 730 güne (eski ücretsiz fiyatlandırma katmanındaki çalışma alanları hariç) bireysel veri türleri için farklı bekletme ayarları belirtebilirsiniz. Her veri türü, çalışma alanının alt kaynağıdır. Örneğin, SecurityEvent tablosu [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şöyle çözülebilir:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Veri türünün (tablo) büyük/küçük harfe duyarlı olduğunu unutmayın.  Belirli bir veri türünün (Bu örnekte SecurityEvent) veri türü bekletme ayarlarını geçerli olarak almak için şunu kullanın:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

Çalışma alanınızdaki tüm veri türleri için geçerli veri türü bekletme ayarlarını almak için, yalnızca belirli veri türünü atlayın, örneğin:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables?api-version=2017-04-26-preview
```

Belirli bir veri türünün (Bu örnekte SecurityEvent) bekletilmesini 730 gün olarak ayarlamak için

```JSON
    PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
    {
        "properties": 
        {
            "retentionInDays": 730
        }
    }
```

İçin geçerli değerler `retentionInDays` 30 ila 730 ' dir.

`Usage`Ve `AzureActivity` veri türleri özel saklama ile ayarlanamaz. Bu, varsayılan çalışma alanı saklama veya 90 gün üst sınırını alır. 

Veri türüne göre saklama ayarlamak için Azure Resource Manager doğrudan bağlanmak için harika bir araç, OSS aracı [Armclient](https://github.com/projectkudu/ARMClient)' dir.  [David Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ve [Daniel bowbevet](https://blog.bowbyes.co.nz/2016/11/02/using-armclient-to-directly-access-azure-arm-rest-apis-and-list-arm-policy-details/)makalelerini kullanarak armclient hakkında daha fazla bilgi edinin.  ARMClient kullanarak bir örnek aşağıda, SecurityEvent verilerini 730 günlük bekletmeye ayarlamaya yönelik bir örnek verilmiştir:

```
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview "{properties: {retentionInDays: 730}}"
```

> [!TIP]
> Veri saklama maliyetlerinizi azaltmak için bireysel veri türlerinde bekletme ayarlama kullanılabilir.  2019 Ekim 'den başlayarak toplanan veriler için (Bu özellik yayınlandığında), bazı veri türlerine yönelik saklama süresini azaltmak zaman içinde bekletme maliyetinizi azaltabilir.  Daha önce toplanan veriler için, tek bir tür için daha düşük bir bekletme ayarlamak, bekletme maliyetlerinizi etkilemez.  

## <a name="manage-your-maximum-daily-data-volume"></a>Günlük veri hacminin maksimum sayısını yönetin

Bir günlük Cap yapılandırabilir ve çalışma alanınız için günlük alımı sınırlayabilirsiniz, ancak amacınız günlük sınıra ulaşmamak zorunda kalacağından dikkatli olabilirsiniz.  Aksi takdirde, günün geri kalanı için veri kaybedersiniz. Bu, işlevleri çalışma alanında kullanılabilir olan güncel verilere bağlı olabilecek diğer Azure hizmetlerini ve çözümlerini etkileyebilir.  Sonuç olarak BT hizmetlerini destekleyen sistem durumu koşullarında uyarıları gözlemleme ve alma beceriniz etkilenir.  Günlük üst sınır, yönetilen kaynaklarınızdan veri hacminde **beklenmedik bir artış** yönetmek ve sınırınızı dahilinde kalmak ya da çalışma alanınız için plansız ücretleri sınırlamak istediğinizde kullanılmak üzere tasarlanmıştır. Her gün bir çalışma alanında karşılanabilmesi için günlük üst sınır ayarlamak uygun değildir.

Her çalışma alanı günlük Cap günün farklı bir saatine uygulanır. Sıfırlama saati, **günlük uç** sayfasında gösterilir (aşağıya bakın). Bu sıfırlama saati yapılandırılamaz. 

Günlük sınıra ulaşıldığında yakında faturalandırılabilir veri türleri koleksiyonu, günün geri kalanı için duraklar. Günlük üst sınır uygulanırken devralınan gecikme, ucun kesin olarak belirtilen günlük sınır düzeyinde uygulanmadığı anlamına gelir. Seçili Log Analytics çalışma alanı için sayfanın üst kısmında bir uyarı başlığı görünür ve bir işlem olayı **Logmanagement** kategorisi altındaki *işlem* tablosuna gönderilir. *Günlük sınır*altında tanımlanan sıfırlama süresi, veri toplama işlemine devam eder. Günlük veri sınırına ulaşıldığında bildirmek üzere yapılandırılan bu işlem olayına dayalı bir uyarı kuralı tanımlamayı öneririz. 

> [!NOTE]
> Günlük sınır, özellikle belirtilen sınır düzeyi ve çok fazla veri beklendiğinden, özellikle de çalışma alanı yüksek hacime veri alıyorsa, veri toplamayı kesin olarak durduramaz.  

> [!WARNING]
> Günlük sınır, Azure Güvenlik Merkezi 'nin 19 Haziran 2017 ' den önce yüklendiği çalışma alanları dışında Azure Sentuya da Azure Güvenlik Merkezi 'nden veri toplamayı durdurmaz. 

### <a name="identify-what-daily-data-limit-to-define"></a>Hangi günlük veri sınırının tanımlanacağını belirleyin

Veri alma eğilimi ve tanımlanacak günlük hacim üst sınırı olduğunu anlamak için [Log Analytics kullanımı ve tahmini maliyetleri](usage-estimated-costs.md) gözden geçirin. Bu sınır, bu yana göz önünde bulundurulmalıdır, çünkü sınıra ulaşıldıktan sonra kaynaklarınızı izleyebilmelisiniz. 

### <a name="set-the-daily-cap"></a>Günlük ucunu ayarla

Aşağıdaki adımlarda, Log Analytics çalışma alanının günlük olarak kullanacağı veri hacmini yönetmek için bir sınırın nasıl yapılandırılacağı açıklanır.  

1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. Seçili çalışma alanı için **kullanım ve tahmini maliyetler** sayfasında sayfanın en üstündeki **veri üst sınırı** ' na tıklayın. 
3. Günlük uç varsayılan olarak **kapalı** mı? etkinleştirmek için **Açık** ' a tıklayın ve ardından GB/gün cinsinden veri hacmi sınırını ayarlayın.

    ![Log Analytics veri sınırı yapılandırma](media/manage-cost-storage/set-daily-volume-cap-01.png)
    
Günlük üst sınır, `dailyQuotaGb` `WorkspaceCapping` [çalışma alanları-oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate#workspacecapping)bölümünde açıklandığı gıbı altında parametresi ayarlanarak ARM aracılığıyla yapılandırılabilir. 

### <a name="alert-when-daily-cap-reached"></a>Günlük sınıra ulaşıldığında uyar

Veri sınırı eşiğine ulaşıldığı zaman Azure portal görsel bir ipucu sunduğumuz sürece, bu davranış anında ilgilenilmesi gereken işlem sorunlarını yönetme biçiminize uygun değildir.  Bir uyarı bildirimi almak için Azure Izleyici 'de yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi edinmek için bkz. [Uyarılar oluşturma, görüntüleme ve yönetme](alerts-metric.md).

Başlamanıza başlamak için, uyarının önerilen ayarları aşağıda verilmiştir:

- Hedef: Log Analytics kaynağını seçin
- Ölçütlere 
   - Sinyal adı: özel günlük araması
   - Arama sorgusu: Işlem | Burada ayrıntıların ' fazla kota ' olması
   - Temel alan: sonuç sayısı
   - Koşul: büyüktür
   - Eşik: 0
   - Süre: 5 (dakika)
   - Sıklık: 5 (dakika)
- Uyarı kuralı adı: günlük veri sınırına ulaşıldı
- Önem derecesi: uyarı (sev 1)

Uyarı tanımlandıktan ve sınıra ulaşıldığında, bir uyarı tetiklenir ve eylem grubunda tanımlanan yanıtı gerçekleştirir. E-posta ve SMS iletileri aracılığıyla ekibinize bildirimde bulunabilir veya Web kancaları, Otomasyon Runbook 'ları veya [bir dış ıSM çözümüyle tümleştirme](itsmc-overview.md#create-itsm-work-items-from-azure-alerts)kullanarak eylemleri otomatikleştirebilir. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics çalışma alanına beklenenden daha fazla düğüm gönderilemedi
- Log Analytics çalışma alanına gönderilmekte olan beklenenden daha fazla veri (Belki de yeni bir çözüm veya var olan bir çözüme yönelik bir yapılandırma değişikliği kullanmaya başlama nedeniyle)

## <a name="understanding-nodes-sending-data"></a>Veri gönderen düğümleri anlama

Son aydaki her gün aracıdan gelen sinyal raporlayan düğüm sayısını anlamak için şunu kullanın

```kusto
Heartbeat 
| where TimeGenerated > startofday(ago(31d))
| summarize nodes = dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```
Son 24 saat içinde veri gönderen düğümlerin sayısını Al sorguyu kullanın: 

```kusto
find where TimeGenerated > ago(24h) project Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Herhangi bir veri gönderen düğümlerin listesini almak için (ve her biri tarafından gönderilen veri miktarı) takip eden sorgu kullanılabilir:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

> [!TIP]
> `find`Veri türlerinde taramalar için [kaynakların yürütülmesi yoğun](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) olduğundan, bu sorguları dikkatli bir şekilde kullanın. **Bilgisayar başına** sonuçlara Ihtiyacınız yoksa kullanım verileri türü üzerinde sorgulama yapın (aşağıya bakın).

## <a name="understanding-ingested-data-volume"></a>Alınan veri birimini anlama

**Kullanım ve tahmini maliyetler** sayfasında, *çözüm başına veri* alma grafiği, gönderilen toplam veri hacmini ve her bir çözüm tarafından ne kadarının gönderileceğini gösterir. Bu, genel veri kullanımının (veya belirli bir çözümün kullanımı) büyümesi, sürekli olarak geri kalan veya azalan gibi eğilimleri belirlemenizi sağlar. 

### <a name="data-volume-for-specific-events"></a>Belirli olaylar için veri hacmi

Belirli bir olay kümesi için alınan verilerin boyutuna bakmak için, belirli bir tabloyu sorgulayabilir (Bu örnekte `Event` ) ve sonra sorguyu ilgilendiğiniz olaylarla sınırlandırabilirsiniz (Bu örnekte olay kimliği 5145 veya 5156):

```kusto
Event
| where TimeGenerated > startofday(ago(31d)) and TimeGenerated < startofday(now()) 
| where EventID == 5145 or EventID == 5156
| where _IsBillable == true
| summarize count(), Bytes=sum(_BilledSize) by EventID, bin(TimeGenerated, 1d)
``` 

Yan tümcesinin, `where _IsBillable = true` alma ücreti olmayan belirli çözümlerden veri türlerini filtreleyeceğini unutmayın. Hakkında [daha fazla bilgi edinin](log-standard-properties.md#_isbillable) `_IsBillable` .

### <a name="data-volume-by-solution"></a>Çözüme göre veri hacmi

Son ayda (son kısmi gün hariç) faturalandırılabilir veri hacmi görüntülemek için kullanılan sorgu:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution | render barchart
```

Yan tümcesi `TimeGenerated` yalnızca Azure Portal içindeki sorgu deneyiminin varsayılan 24 saatin ötesine geri baktığından emin olur. Kullanım verileri türünü kullanırken `StartTime` ve `EndTime` sonuçların sunulduğu zaman demetlerini temsil eder. 

### <a name="data-volume-by-type"></a>Türe göre veri hacmi

Veri türüne göre veri eğilimlerini görmek için daha fazla ayrıntıya gidebilirsiniz:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType | render barchart
```

Ya da bir tabloyu çözüm ve son ayın türüne göre görmek için

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Bilgisayara göre veri hacmi

`Usage`Veri türü, bilgisayar düzeyinde bilgi içermez. Bilgisayar başına alınan verilerin **boyutunu** görmek için, `_BilledSize` boyutu bayt cinsinden sağlayan [özelliğini](log-standard-properties.md#_billedsize)kullanın:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, _IsBillable, Computer
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName 
| sort by BillableDataBytes nulls last
```

`_IsBillable` [Özelliği](log-standard-properties.md#_isbillable) , alınan verilerin ücretlendirip ödemeyeceğini belirtir. 

Bilgisayar başına alınan faturalandırılabilir olay **sayısını** görmek için şunu kullanın: 

```kusto
find where TimeGenerated > ago(24h) project _IsBillable, Computer
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  
| sort by eventCount nulls last
```

> [!TIP]
> `find`Veri türlerinde taramalar için [kaynakların yürütülmesi yoğun](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) olduğundan, bu sorguları dikkatli bir şekilde kullanın. **Bilgisayar başına** sonuçlara gerek yoksa kullanım verileri türü üzerinde sorgulama yapın.

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Azure kaynağına, kaynak grubuna veya aboneliğe göre veri hacmi

Azure 'da barındırılan düğümlerdeki veriler için __bilgisayar başına__alınan verilerin **boyutunu** alabilir, kaynağın tam yolunu sağlayan _ResourceId [özelliğini](log-standard-properties.md#_resourceid)kullanın:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by BillableDataBytes nulls last
```

Azure 'da barındırılan düğümlerdeki veriler için __Azure aboneliği başına__alınan verilerin **boyutunu** alabilir, abonelik kimliğini şu `_ResourceId` şekilde alın:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId
| extend subscriptionId = tostring(split(_ResourceId, "/")[2]) 
| summarize BillableDataBytes = sum(BillableDataBytes) by subscriptionId | sort by BillableDataBytes nulls last
```

Benzer şekilde, kaynak grubuna göre veri hacmi almak için bu şöyle olacaktır:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId
| extend resourceGroup = tostring(split(_ResourceId, "/")[4] )
| summarize BillableDataBytes = sum(BillableDataBytes) by resourceGroup | sort by BillableDataBytes nulls last
```

Ayrıca, gerekirse daha kapsamlı bir şekilde de ayrıştırılabilir `_ResourceId`

```Kusto
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
```

> [!TIP]
> `find`Veri türlerinde taramalar için [kaynakların yürütülmesi yoğun](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) olduğundan, bu sorguları dikkatli bir şekilde kullanın. Abonelik başına sonuçlara, kaynak grubuna veya kaynak adına ihtiyaç duymayın, ardından kullanım veri türü üzerinde sorgulama yapın.

> [!WARNING]
> Kullanım verileri türünün bazı alanları şemada hala kullanım dışı bırakılmıştır ve değerleri artık doldurulmayacaktır. Bunlar, **bilgisayar** ve alma (**totaltoplu işler**, **batcheswithınsla**, **batchesoutsidesla**, **batchescaıda** **averageprocessingtimems**) ile ilgili alanlardır.


### <a name="querying-for-common-data-types"></a>Ortak veri türleri sorgulanıyor

Belirli bir veri türü için veri kaynağını daha ayrıntılı bir şekilde incelemek için bazı yararlı örnek sorgular aşağıda verilmiştir:

+ **Çalışma alanı tabanlı Application Insights** kaynakları
  - [Application Insights için kullanımı ve maliyetleri yönetme](../app/pricing.md#data-volume-for-workspace-based-application-insights-resources) hakkında daha fazla bilgi edinin
+ **Güvenlik** çözümü
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Günlük Yönetimi** çözümü
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **Perf** veri türü
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **Event** veri türü
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog** veri türü
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics** veri türü
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

## <a name="tips-for-reducing-data-volume"></a>Veri hacmi azaltma ipuçları

Toplanan günlüklerin hacmini azaltmaya yönelik bazı öneriler şunlardır:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Kapsayıcı öngörüleri         | [Kapsayıcı öngörülerini](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-cost#controlling-ingestion-to-reduce-cost) yalnızca gerekli verileri toplayacak şekilde yapılandırın. |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) kullanın. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Düğüm başına fiyatlandırma katmanında düğümleri faturalandırılan alma

Düğüm olarak faturalandırılacak bilgisayarların listesini almak için, çalışma alanı, eski düğüm başına fiyatlandırma katmanındaysa, **faturalandırılan veri türlerini** gönderen düğümleri arayın (bazı veri türleri ücretsizdir). Bunu yapmak için, `_IsBillable` [özelliğini](log-standard-properties.md#_isbillable) kullanın ve tam etki alanı adının en solundaki alanı kullanın. Bu, saat başına faturalandırılan bilgisayar sayısını (düğümlerin sayıldığı ve faturalandırılabileceği ayrıntı düzeyi) döndürür:

```kusto
find where TimeGenerated > ago(24h) project Computer, TimeGenerated
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Güvenlik ve otomasyon düğüm sayılarını alma

"Düğüm başına (OMS)" Fiyatlandırma Katmanı kullanıyorsanız, kullandığınız düğüm ve çözüm sayısına göre ücretlendirilirsiniz, faturalandırılmakta olduğunuz Öngörüler ve analiz düğümlerinin sayısı **kullanım ve tahmini maliyet** sayfasındaki tabloda gösterilir.  

Farklı güvenlik düğümlerinin sayısını görmek için sorguyu kullanabilirsiniz:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Farklı Otomasyon düğümlerinin sayısını görmek için sorguyu kullanın:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="evaluating-the-legacy-per-node-pricing-tier"></a>Eski düğüm başına fiyatlandırma katmanını değerlendirme

Eski **düğüm başına** fiyatlandırma katmanına erişimi olan çalışma alanlarının bu katmanda veya geçerli **Kullandıkça Öde** ya da **Kapasite rezervasyon** katmanının daha iyi bir şekilde değerlendirilmesi, müşterilerin değerlendirmesi için genellikle zordur.  Bu, düğüm başına fiyatlandırma katmanındaki izlenen düğüm başına sabit maliyet ve 500 MB/düğüm/gün dahil olmak üzere veri tahsisatı ve yalnızca Kullandıkça Öde (GB başına) katmanındaki verilerin ödenme maliyeti arasındaki ticaretin kullanımını anlamayı içerir. 

Bu değerlendirmeyi kolaylaştırmak için aşağıdaki sorgu, çalışma alanının kullanım desenlerine göre en uygun fiyatlandırma katmanına yönelik bir öneri oluşturmak için kullanılabilir.  Bu sorgu, son 7 gün içinde bir çalışma alanına alınan izlenen düğümlere ve verilere bakar ve her gün için hangi fiyatlandırma katmanının en uygun olduğunu değerlendirir. Sorguyu kullanmak için şunu belirtmeniz gerekir

1. çalışma alanının, `workspaceHasSecurityCenter` veya olarak ayarlayarak Azure Güvenlik Merkezi kullanıp kullanmadığını `true` `false` 
2. belirli indirimleriniz varsa fiyatları güncelleştirin ve
3. ayarlar ile geri Aranacak ve analiz edilecek gün sayısını belirtin `daysToEvaluate` . Bu, sorgunun 7 güne kadar veri almaya çalışırken çok uzun sürmesi durumunda yararlıdır. 

Fiyatlandırma Katmanı öneri sorgusu aşağıda verilmiştir:

```kusto
// Set these parameters before running query
let workspaceHasSecurityCenter = true;  // Specify if the workspace has Azure Security Center
let PerNodePrice = 15.; // Enter your montly price per monitored nodes
let PerNodeOveragePrice = 2.30; // Enter your price per GB for data overage in the Per Node pricing tier
let PerGBPrice = 2.30; // Enter your price per GB in the Pay-as-you-go pricing tier
let daysToEvaluate = 7; // Enter number of previous days look at (reduce if the query is taking too long)
// ---------------------------------------
let SecurityDataTypes=dynamic(["SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent", "Update", "UpdateSummary"]);
let StartDate = startofday(datetime_add("Day",-1*daysToEvaluate,now()));
let EndDate = startofday(now());
union * 
| where TimeGenerated >= StartDate and TimeGenerated < EndDate
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodesPerHour = dcount(computerName) by bin(TimeGenerated, 1h)  
| summarize nodesPerDay = sum(nodesPerHour)/24.  by day=bin(TimeGenerated, 1d)  
| join kind=leftouter (
    Heartbeat 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where Computer != ""
    | summarize ASCnodesPerHour = dcount(Computer) by bin(TimeGenerated, 1h) 
    | extend ASCnodesPerHour = iff(workspaceHasSecurityCenter, ASCnodesPerHour, 0)
    | summarize ASCnodesPerDay = sum(ASCnodesPerHour)/24.  by day=bin(TimeGenerated, 1d)   
) on day
| join (
    Usage 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where IsBillable == true
    | extend NonSecurityData = iff(DataType !in (SecurityDataTypes), Quantity, 0.)
    | extend SecurityData = iff(DataType in (SecurityDataTypes), Quantity, 0.)
    | summarize DataGB=sum(Quantity)/1000., NonSecurityDataGB=sum(NonSecurityData)/1000., SecurityDataGB=sum(SecurityData)/1000. by day=bin(StartTime, 1d)  
) on day
| extend AvgGbPerNode =  NonSecurityDataGB / nodesPerDay
| extend PerGBDailyCost = iff(workspaceHasSecurityCenter,
             (NonSecurityDataGB + max_of(SecurityDataGB - 0.5*ASCnodesPerDay, 0.)) * PerGBPrice,
             DataGB * PerGBPrice)
| extend OverageGB = iff(workspaceHasSecurityCenter, 
             max_of(DataGB - 0.5*nodesPerDay - 0.5*ASCnodesPerDay, 0.), 
             max_of(DataGB - 0.5*nodesPerDay, 0.))
| extend PerNodeDailyCost = nodesPerDay * PerNodePrice / 31. + OverageGB * PerNodeOveragePrice
| extend Recommendation = iff(PerNodeDailyCost < PerGBDailyCost, "Per Node tier", 
             iff(NonSecurityDataGB > 85., "Capacity Reservation tier", "Pay-as-you-go (Per GB) tier"))
| project day, nodesPerDay, ASCnodesPerDay, NonSecurityDataGB, SecurityDataGB, OverageGB, AvgGbPerNode, PerGBDailyCost, PerNodeDailyCost, Recommendation | sort by day asc
//| project day, Recommendation // Comment this line to see details
| sort by day asc
```

Bu sorgu, kullanımın nasıl hesaplanmasının tam bir yinelemesi değildir, ancak çoğu durumda fiyatlandırma katmanı önerileri sağlamaya çalışır.  

> [!NOTE]
> System Center için OMS E1 Suite, OMS E2 Suite veya OMS eklentisi satın alma işleminden gelen yetkilendirmeleri kullanmak için *düğüm başına* fiyatlandırma katmanını Log Analytics seçin.

## <a name="create-an-alert-when-data-collection-is-high"></a>Veri toplama işlemi yüksekse uyarı oluştur

Bu bölümde, son 24 saat içindeki veri hacmi, Azure Izleyici [günlük uyarılarını](alerts-unified-log.md)kullanarak belirtilen miktarı aşmış bir uyarının nasıl oluşturulacağı açıklanmaktadır. 

Son 24 saat içinde alınan faturalandırılabilir veri hacmi 50 GB 'den büyükse uyarı almak için şu adımları izleyin: 

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Sorgunun aranacağı sorgu** `Usage | where IsBillable | summarize DataGB = sum(Quantity / 1000.) | where DataGB > 50` . Farklı bir ETN istiyorsanız 
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - *1440* dakikalık **süre** ve her *1440* **dakikadaki Uyarı sıklığı** , günde bir kez çalıştırılır.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - *24 saat içinde 50 GB 'tan büyük faturalandırılabilir veri hacmi* **adı**
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

Bir uyarı aldığınızda, kullanımın neden beklenenden yüksek olduğunu nasıl giderebileceğiniz hakkında yukarıdaki bölümde yer alan adımları kullanın.

## <a name="data-transfer-charges-using-log-analytics"></a>Log Analytics kullanarak veri aktarımı ücretleri

Verilerin Log Analytics gönderilmesi veri bant genişliği ücretlerine neden olabilirler. [Azure bant genişliği fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/bandwidth/)açıklandığı gibi, iki bölgede bulunan Azure hizmetleri arasındaki veri aktarımı, normal fiyata giden veri aktarımı olarak ücretlendirilir. Gelen veri aktarımı ücretsizdir. Ancak, bu ücret çok küçük (az%) Log Analytics veri alımı maliyetleriyle karşılaştırılır. Sonuç olarak Log Analytics maliyetlerinin denetlenmesi, alınan [veri hacminin](#understanding-ingested-data-volume)odaklanılmasını gerektirir. 


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Log Analytics neden artık veri toplamadığına ilişkin sorun giderme

Eski ücretsiz fiyatlandırma katmanınız varsa ve günde 500 MB 'tan fazla veri gönderdikten sonra, veri toplama günün geri kalanı için duraklar. Günlük sınıra ulaşmak Log Analytics veri toplamayı durdurmasının ve verilerin eksik göründüğünden yaygın bir nedenidir.  Log Analytics, veri toplama başladığında ve durdurulduğunda Işlem türünde bir olay oluşturur. Günlük sınıra ve eksik verilere ulaşıp ulaşılmayacağını denetlemek için aramada aşağıdaki sorguyu çalıştırın: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Veri toplama durdurulduğunda OperationStatus değeri **Uyarı** olur. Veri toplama başlatıldığında, OperationStatus değeri **Başarılı** olur. Aşağıdaki tabloda veri koleksiyonunun durduğu nedenler ve veri toplamayı sürdürmesi için önerilen bir eylem açıklanmaktadır:  

|Neden koleksiyonun durması| Çözüm| 
|-----------------------|---------|
|Çalışma alanınızın günlük tepesine ulaşıldı|Koleksiyonun otomatik olarak yeniden başlatılmasını bekleyin veya en fazla günlük veri birimini yönetme bölümünde açıklanan günlük veri birimi sınırını artırın. Günlük uç sıfırlama zamanı, **günlük uç** sayfasında gösterilir. |
| Çalışma alanınız [veri alma birimi oranına](https://docs.microsoft.com/azure/azure-monitor/service-limits#log-analytics-workspaces) ulaştı | Tanılama ayarları kullanılarak Azure kaynaklarından gönderilen veriler için varsayılan Alım birimi hız sınırı, çalışma alanı başına yaklaşık 6 GB/dak. Bu, gerçek boyutun günlük uzunluğuna ve sıkıştırma oranına bağlı olarak veri türleri arasında değişiklik gösterebileceğinden yaklaşık bir değerdir. Bu sınır, aracılardan veya veri toplayıcı API 'sinden gönderilen verilere uygulanmaz. Verileri daha yüksek bir fiyata tek bir çalışma alanına gönderirseniz, bazı veriler bırakılır ve eşik aşılmaya devam edilirken her 6 saatte bir olay, çalışma alanınızda Işlem tablosuna gönderilir. Alım biriminiz, hız sınırını aşmaya devam ediyorsa veya bir süre önce bu sınıra ulaşmayı bekliyorsanız, bir e-posta göndererek LAIngestionRate@microsoft.com veya bir destek isteği açarak çalışma alanınıza bir artış isteğinde bulunabilir. Bir veri alma hızı sınırının sorgu tarafından bulunamayacağını belirten Aranacak olay `Operation | where OperationCategory == "Ingestion" | where Detail startswith "The rate of data crossed the threshold"` . |
|Eski ücretsiz fiyatlandırma katmanının günlük sınırına ulaşıldı |Koleksiyonun otomatik olarak yeniden başlatılmasını veya ücretli bir fiyatlandırma katmanına geçiş yapmak için aşağıdaki güne kadar bekleyin.|
|Azure aboneliği şu nedenlerle askıya alınmış durumda:<br> Ücretsiz deneme süresi sona erdi<br> Azure Pass 'nin süre geçti<br> Aylık harcama sınırına ulaşıldı (örneğin, bir MSDN veya Visual Studio aboneliği)|Ücretli aboneliğe dönüştürme<br> Limiti Kaldır veya sınır sıfırlanana kadar bekle|

Veri toplama durdurulduğunda uyarılmak için, veri toplama durdurulduğunda bildirim almak üzere *günlük veri Cap uyarısı oluşturma* bölümünde açıklanan adımları kullanın. Uyarı kuralı için bir e-posta, Web kancası veya Runbook eylemi yapılandırmak üzere [eylem grubu oluşturma](action-groups.md) bölümünde açıklanan adımları kullanın. 

## <a name="limits-summary"></a>Limit Özeti

Bazıları Log Analytics fiyatlandırma katmanına bağlı olan bazı ek Log Analytics limitleri vardır. Bunlar [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces)konusunda belgelenmiştir.


## <a name="next-steps"></a>Sonraki adımlar

- Arama dilinin nasıl kullanılacağını öğrenmek için bkz. [Azure Izleyici günlüklerinde günlük aramaları](../log-query/log-query-overview.md) . Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
- Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.
- Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) kullanın.
- Etkin bir olay toplama ilkesini yapılandırmak için [Azure Güvenlik Merkezi filtreleme ilkesini](../../security-center/security-center-enable-data-collection.md)gözden geçirin.
- [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) değiştirin.
- Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](data-sources-windows-events.md) konusunu gözden geçirin.
- Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](data-sources-syslog.md) konusunu gözden geçirin.