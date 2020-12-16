---
title: Azure maliyetlerin yönetmeyi planlama
description: Azure maliyetlerinizi yönetme planı yapmayı ve Azure hesabınızda maliyet izleme ile yönetim özelliklerini kullanmayı öğrenin.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: common
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: banders
ms.custom: contperf-fy21q1
ms.openlocfilehash: c640478170049bd06e11f7f89b1e213b7b305642
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97032822"
---
# <a name="plan-to-manage-azure-costs"></a>Azure maliyetlerin yönetmeyi planlama

Bu makale Azure maliyetlerinizi yönetme planına başlamanıza yardımcı olur. Azure'a kaydolduğunuzda, harcamalarınız hakkında daha iyi bir fikir sahibi olmak için yapabileceğiniz birkaç şey vardır:

- Hizmet eklemeden önce [fiyat hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) veya Azure fiyat listesini kullanarak ya da Azure portalında hizmet eklerken tahmini maliyetler alın.
- [Bütçeler](../costs/tutorial-acm-create-budgets.md), [uyarılar](../costs/cost-mgt-alerts-monitor-usage-spending.md) ve [maliyet analizi](../costs/quick-acm-cost-analysis.md) ile maliyetleri izleyin.
- Faturanızdaki ücretleri [ayrıntılı kullanım dosyaları](../manage/download-azure-invoice-daily-usage-date.md) ile karşılaştırarak gözden geçirin.
- [Fatura](/rest/api/billing/) ve [tüketim](/rest/api/consumption/) API’lerini kullanarak fatura ve maliyet verilerini kendi raporlama sisteminizle tümleştirin.
- Kurumsal Anlaşma (EA), Bulut Çözümü Sağlayıcısı (CSP) ve Azure Sponsorluğu müşterilerine yönelik ek kaynakları ve araçları kullanın.
- [Ücretsiz Azure hesabı](../manage/create-free-services.md) ile sunulan [en popüler Azure hizmetlerinden bazılarını 12 ay boyunca ücretsiz](https://azure.microsoft.com/free/) kullanın. Aşağıda listelenen önerilere birlikte, bkz. [Ücretsiz hesap için ücret yansıtılmasını önleme](../manage/avoid-charges-free-account.md).

Azure aboneliğinizi iptal etmeniz gerekiyorsa bkz. [Azure aboneliğinizi iptal etme](../manage/cancel-azure-subscription.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure hizmetlerini eklemeden önce tahmini maliyetleri alma

Bir Azure hizmetini kullanmanın maliyetini tahmin etmek için aşağıdaki araçlardan birini kullanın:
- Azure fiyatlandırma hesaplayıcısı
- Azure fiyat listesi
- Azure portal

Aşağıdaki bölümlerde yer alan görüntüler, örnek fiyatlandırmayı ABD Doları cinsinden göstermektedir. Belirtilen fiyatlar yalnızca örnek amaçlıdır. Gerçek fiyatlara işaret etmeleri amaçlanmamıştır. 

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcısını kullanarak maliyeti çevrimiçi olarak tahmin etme

Eklemek istediğiniz hizmetin tahmini aylık maliyetini öğrenmek için [fiyat hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/) göz atın. Yerel para biriminizde tahmin almak için para birimini değiştirebilirsiniz.

![Fiyatlandırma hesaplayıcısı menüsünün ekran görüntüsü](./media/plan-manage-costs/pricing-calc.png)

Herhangi bir birinci taraf Azure hizmeti için tahmini maliyeti görüntüleyebilirsiniz. Örneğin, aşağıdaki ekran görüntüsünde, bir A1 Windows Sanal Makinesi’ni (VM) sürekli çalışır durumda bırakırsanız aylık 66,96 USD maliyetinin olacağı tahmin edilir:

![Bir A1 Windows sanal makinesinin aylık tahmini maliyetini gösteren fiyatlandırma hesaplayıcısının ekran görüntüsü](./media/plan-manage-costs/pricing-calc-vm.png)

Belirtilen fiyatlar yalnızca örnek amaçlıdır. Gerçek fiyatlara işaret etmeleri amaçlanmamıştır.

Fiyatlandırma hakkında daha fazla bilgi için bkz. [ Fiyatlandırma hakkında SSS](https://azure.microsoft.com/pricing/faq/). Bir Azure satış temsilcisiyle iletişime geçmek istiyorsanız, SSS sayfasının en üstünde gösterilen telefon numarasını arayın.

### <a name="review-prices"></a>Fiyatları gözden geçirme

Azure'a bir Kurumsal Anlaşma (EA) veya bir Microsoft Müşteri Sözleşmesi (MCA) aracılığıyla erişiminiz varsa, Azure hesabınızın fiyat listesini görüntüleyebilir ve indirebilirsiniz. Fiyat listesi tüm Azure hizmetlerinin fiyatlarını içeren bir excel dosyasıdır. Daha fazla bilgi için bkz. [Azure fiyatlandırmasını görüntüleme ve indirme](../manage/ea-pricing.md).

Diğer abonelik türlerinde [Azure Perakende Fiyatları API'si](/rest/api/cost-management/retail-prices/azure-retail-prices) ile standart perakende fiyatlarını alabilirsiniz.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Azure portalında tahmini maliyetleri gözden geçirme

Azure portalında bir hizmet eklerken aylık tahmini maliyeti görüntüleyebilirsiniz. Örneğin, Windows sanal makinenizin boyutunu seçtiğinizde işlem saatleri için tahmini aylık maliyeti görürsünüz:

![Örnek: aylık tahmini maliyeti gösteren bir A1 Windows sanal makinesi](./media/plan-manage-costs/vm-size-cost.png)

Belirtilen fiyatlar yalnızca örnek amaçlıdır. Gerçek fiyatlara işaret etmeleri amaçlanmamıştır.

## <a name="monitor-costs-when-using-azure-services"></a>Azure hizmetlerini kullanırken maliyetleri izleme
Maliyetleri aşağıdaki araçlarla izleyebilirsiniz:

- Bütçe ve maliyet uyarıları
- Maliyet analizi

### <a name="track-costs-with-budgets-and-cost-alerts"></a>Bütçeleri ve maliyet uyarılarını izleme

Maliyetleri yönetmek için [bütçeler](../costs/tutorial-acm-create-budgets.md) oluşturun ve paydaşların harcama anormalliklerini ve fazla harcamalarını size otomatik olarak bildiren [uyarılar](../costs/cost-mgt-alerts-monitor-usage-spending.md) oluşturun.

### <a name="explore-and-analyze-costs-with-cost-analysis"></a><a name="costs"></a> Maliyet analiziyle maliyetleri araştırma ve analiz etme

Azure hizmetlerinizi çalışır duruma getirdikten sonra Azure harcamanızı izlemek için maliyetleri düzenli olarak denetleyin. Maliyet analizini, Azure kullanımınıza ilişkin maliyetlerin nereden kaynaklandığını anlamak için kullanabilirsiniz.

[Azure portalında Maliyet Yönetimi + Faturalama sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade) ziyaret edin.

Hizmet, konum ve abonelik gibi çeşitli esaslara göre ayrılmış geçerli maliyeti görmek için ekranın sol tarafındaki **Maliyet analizi**'ne tıklayın. Bir hizmet ekledikten veya satın alma işlemi yaptıktan sonra verilerin gösterilmesi için 24 saat bekleyin. Varsayılan olarak, maliyet analizi, içinde olduğunuz kapsamın maliyetini gösterir. Örneğin, aşağıdaki ekran görüntüsünde Contoso ödeme hesabının maliyeti gösterilmiştir. Maliyet analizinde farklı bir kapsama geçiş yapmak için Kapsam hapını kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](../costs/understand-work-scopes.md#scopes)

![Azure portalında maliyet analizi görünümünün ekran görüntüsü](./media/plan-manage-costs/cost-analysis.png)

Etiketler, kaynak türü ve zaman aralığı gibi çeşitli özelliklere göre filtre uygulayabilirsiniz. Bir özellik için filtre eklemek üzere **Filtre ekle**’ye tıklayın ve filtrelenecek değerleri seçin. Görünümü bir virgülle ayrılmış değerler (.csv) dosyasına aktarmak için **Dışarı Aktar**’ı seçin.

Ayrıca, bu etiketin günlük harcama geçmişini görmek için grafiğin etiketlerine tıklayabilirsiniz. Örneğin aşağıdaki ekran görüntüsünde, sanal makine seçildiğinde VM'lerinizi çalıştırmanın günlük maliyeti gösterilir.

:::image type="content" source="./media/plan-manage-costs/cost-history.png" alt-text="Azure portalında harcama geçmişi görünümünün ekran görüntüsü" lightbox="./media/plan-manage-costs/cost-history.png" :::

## <a name="optimize-and-reduce-costs"></a>Maliyetleri iyileştirme ve azaltma

Maliyet yönetimi ilkelerini bilmiyorsanız, [Azure Maliyet Yönetimi ile bulut yatırımınızı iyileştirme](../costs/cost-mgt-best-practices.md) makalesini okuyun.

Azure portalında ayrıca VM’ler için otomatik kapatma ve Danışman önerileri ile Azure maliyetlerini iyileştirebilir ve azaltabilirsiniz.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>VM'ler için otomatik olarak kapatma gibi maliyet kesme özelliklerini göz önünde bulundurun

Senaryonuza bağlı olarak, Azure portalında sanal makineleriniz için otomatik kapatmayı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager kullanarak VM'ler için otomatik kapatma](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Portalda otomatik kapatma seçeneğinin ekran görüntüsü](./media/plan-manage-costs/auto-shutdown.png)

Otomatik kapatma, sanal makine içinde güç seçenekleriyle yaptığınız kapatma işlemiyle aynı değildir. Otomatik kapatma, VM’lerinizi durdurup serbest bırakır ve ek kullanım ücretleri yansıtılmasını durdurur. VM durumları hakkında daha fazla bilgi için [Linux VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows VM](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) için fiyatlandırma hakkında SSS bölümüne bakın.

Geliştirme ve test ortamlarınıza yönelik daha fazla maliyet azaltma özelliği için [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)’i inceleyin.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Danışmanı önerilerini açma ve gözden geçirme

[Azure Danışmanı](../../advisor/advisor-overview.md), düşük kullanımlı kaynakları tanımlayarak maliyetleri azaltmanıza yardımcı olur. Azure portalında **Danışman**’ı arayın:

![Azure portalında Azure Danışmanı düğmesinin ekran görüntüsü](./media/plan-manage-costs/advisor-button.png)

Sol taraftan **Maliyet**’i seçin. **Maliyet** sekmesinde eyleme dönüştürülebilir öneriler görürsünüz:

![Danışman maliyet önerisi örneğinin ekran görüntüsü](./media/plan-manage-costs/advisor-action.png)

Belirtilen fiyatlar yalnızca örnek amaçlıdır. Gerçek fiyatlara işaret etmeleri amaçlanmamıştır.

Maliyet tasarrufu sağlayan Danışmanı önerileri hakkında kılavuzlu bir öğretici için [Önerilerle maliyetleri iyileştirme](../costs/tutorial-acm-opt-recommendations.md) öğreticisini gözden geçirin.

## <a name="integrate-with-billing-and-consumption-apis"></a>Fatura ve tüketim API'leriyle tümleştirme

Fatura ve maliyet verilerini program aracılığıyla almak için Azure [fatura](/rest/api/billing/) ve [tüketim](/rest/api/consumption/) API’lerini kullanın. RateCard API'si ve Kullanım API'sini birlikte kullanarak faturalandırılan kullanımınızı alabilirsiniz. Daha fazla bilgi için bkz. [Microsoft Azure kaynak kullanımınızla ilgili içgörüler edinin](../manage/usage-rate-card-overview.md).

## <a name="additional-resources-and-special-cases"></a><a name="other-offers"></a> Ek kaynaklar ve özel durumlar

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP ve Sponsorluk müşterileri
Başlamak için hesap yöneticiniz veya Azure iş ortağınızla konuşun.

| Sunduğu | Kaynaklar |
|-------------------------------|-----------------------------------------------------------------------------------|
| Kurumsal Anlaşma (EA) | [EA portalı](https://ea.azure.com/), [yardım belgeleri](https://ea.azure.com/helpdocs) ve [Power BI raporu](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Bulut Çözümü Sağlayıcısı (CSP) | Sağlayıcınızla konuşun |
| Azure Sponsorluğu | [Sponsorluk portalı](https://www.microsoftazuresponsorships.com/) |

Büyük bir kuruluşun BT yöneticisiyseniz, [Azure kurumsal iskelesi](/azure/architecture/cloud-adoption-guide/subscription-governance) ve [kurumsal BT teknik incelemesini](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf indirme, yalnızca İngilizce) okumanızı öneririz.

### <a name="enterprise-agreement-cost-views-in-the-azure-portal"></a><a name="EA"></a> Azure portalındaki Kurumsal Anlaşma maliyet görünümleri

Kurumsal maliyet görünümleri şu anda Genel Önizleme aşamasındadır. Dikkat edilmesi gereken noktalar:

- Abonelik maliyetleri kullanıma dayanır ve ön ödemeli tutarları, fazla kullanımları, ürüne dahil olan miktarları, ayarlamaları ve vergileri içermez. Gerçek ücretler Kayıt düzeyinde hesaplanır.
- Azure portalında gösterilen tutarlar, Enterprise portalında gösterilenden farklı olabilir. Enterprise portalındaki güncelleştirmelerin Azure portalında gösterilmesi birkaç dakika sürebilir.
- Maliyetleri görmüyorsanız, bunun nedeni aşağıdakilerden biri olabilir:
    - Abonelik düzeyinde izinleriniz yok. Kurumsal maliyet görünümlerini görmek için abonelik düzeyinde bir Faturalama Okuyucusu, Okuyucu, Katkıda Bulunan veya Sahip olmanız gerekir.
    - Hesap Sahibisiniz ve Kayıt Yöneticiniz "AO görünüm ücretleri" ayarını devre dışı bıraktı.  Maliyetlere erişim elde etmek için Kayıt Yöneticinizle görüşün.
    - Bölüm Yöneticisisiniz ve Kayıt Yöneticiniz **DA görünüm ücretleri** ayarını devre dışı bıraktı.  Erişim elde etmek için Kayıt Yöneticinizle görüşün.
    - Azure’u bir kanal iş ortağı aracılığıyla satın aldınız ve iş ortağı fiyatlandırma bilgilerini yayınlamadı.  
- Enterprise portalında maliyet erişimiyle ilgili ayarları güncelleştirirseniz, değişikliklerin Azure portalında gösterilmesi birkaç dakika gecikebilir.
- Harcama limiti ve fatura kılavuzu, EA Abonelikleri için geçerli değildir.

### <a name="check-your-subscription-and-access"></a>Aboneliğinizi ve erişiminizi kontrol edin

Maliyetleri görüntülemek için maliyet veya fatura bilgilerine hesap ya da abonelik düzeyinde erişim gerekir. Erişim, ödeme hesabı türüne göre farklılık gösterir. Ödeme hesapları hakkında daha fazla bilgi edinmek ve ödeme hesabınızın türünü kontrol etmek için bkz. [Azure portalında ödeme hesaplarını görüntüleme](../manage/view-all-accounts.md).

Azure'a bir Microsoft Çevrimiçi Hizmet Programı (MOSP) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure fatura bilgilerine erişimi yönetme](../manage/manage-billing-access.md).

Azure'a bir Kurumsal Anlaşma (EA) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure’da Azure Kurumsal Anlaşma yönetim rollerini anlama](../manage/understand-ea-roles.md).

Azure'a bir Microsoft Müşteri Sözleşmesi (MCA) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure’da Microsoft Müşteri Sözleşmesi yönetim rollerini anlama](../manage/understand-mca-roles.md).

### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Bir hizmet olayı için Hizmet Düzeyi Sözleşmesi kredisi isteme

Hizmet Düzeyi Sözleşmeleri (SLA), Microsoft’un çalışma süresi ve bağlantı hakkındaki taahhütlerini açıklar. Azure hizmetleri, genellikle *kesinti* olarak adlandırılan, çalışma süresini veya bağlantıyı etkileyen bir sorunla karşılaştığında bir hizmet olayı bildirilir. SLA'da açıklandığı gibi her bir hizmetin Hizmet Düzeylerini sağlayıp sürdürmezsek, aylık hizmet ücretlerinizin bir kısmını kredi olarak kullanmaya uygun olabilirsiniz.

Kredi istemek için:

[Azure Portal](https://portal.azure.com/) oturum açın. Birden çok hesabınız varsa, Azure’un kapalı kaldığı süreden etkilenen bir hesabı kullandığınızdan emin olun. Ardından yeni bir destek isteği oluşturun.

**Sorun türü**'nün altında **Faturalama**'yı ve **Sorun türü**'nün altında **Para İadesi İsteği**'ni seçin.

SLA kredisi istediğinizi belirtmek için ayrıntılar ekleyin; tarih/saat/saat dilimi ve etkilenen hizmetleri (VM'ler, Web Siteleri vb.) belirtin

Son olarak iletişim bilgilerinizi doğrulayın ve isteğinizi göndermek için **Oluştur**'u seçin.

Bazı hizmetlerde SLA'nın uygulanması önkoşullara bağlıdır. Örneğin, sanal makinelerin aynı Kullanılabilirlik Kümesinde dağıtılmış iki veya daha fazla örneği olmalıdır.

Daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) ve [Azure hizmetleri için SLA özeti](https://azure.microsoft.com/support/legal/sla/summary/) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar
- Fazla harcamayı önlemek için [harcama limitlerini](../manage/spending-limit.md) kullanma hakkında bilgi edinin.
- [Azure maliyetlerinizi analiz etmeye](../costs/quick-acm-cost-analysis.md) başlayın.