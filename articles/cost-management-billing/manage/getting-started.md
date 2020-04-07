---
title: Azure Maliyet Yönetimi ve Faturalama ile beklenmeyen ücretleri önleme ve analiz etme
description: Azure faturanızdaki beklenmeyen ücretleri önleme ve Azure hesabınızda maliyet izleme ile yönetim özellikleri hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 3/30/2020
ms.author: banders
ms.openlocfilehash: 79af6f78e8e9bf93c49deafe79f6a421cbb77d1a
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475268"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure faturasında beklenmeyen ücretlendirmeleri önleme ve maliyet yönetimi

Azure'a kaydolduğunuzda, harcamalarınız hakkında daha iyi bir fikir sahibi olmak için yapabileceğiniz birkaç şey vardır:

- Hizmet eklemeden önce [fiyat hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) veya Azure fiyat listesini kullanarak ya da Azure portalında hizmet eklerken tahmini maliyetler alın.
- [Bütçeler](../costs/tutorial-acm-create-budgets.md), [uyarılar](../costs/cost-mgt-alerts-monitor-usage-spending.md) ve [maliyet analizi](../costs/quick-acm-cost-analysis.md) ile maliyetleri izleyin.
- Faturanızdaki ücretleri [ayrıntılı kullanım dosyaları](download-azure-invoice-daily-usage-date.md) ile karşılaştırarak gözden geçirin.
- [Fatura](https://docs.microsoft.com/rest/api/billing/) ve [tüketim](https://docs.microsoft.com/rest/api/consumption/) API’lerini kullanarak fatura ve maliyet verilerini kendi raporlama sisteminizle tümleştirin.
- Kurumsal Anlaşma (EA), Bulut Çözümü Sağlayıcısı (CSP) ve Azure Sponsorluğu müşterilerine yönelik ek kaynakları ve araçları kullanın.
- [Ücretsiz Azure hesabı](create-free-services.md) ile sunulan [en popüler Azure hizmetlerinden bazılarını 12 ay boyunca ücretsiz](https://azure.microsoft.com/free/) kullanın. Aşağıda listelenen önerilere birlikte, bkz. [Ücretsiz hesap için ücret yansıtılmasını önleme](avoid-charges-free-account.md).

Azure aboneliğinizi iptal etmeniz gerekiyorsa bkz. [Azure aboneliğinizi iptal etme](cancel-azure-subscription.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure hizmetlerini eklemeden önce tahmini maliyetleri alma

Bir Azure hizmetini kullanmanın maliyetini tahmin etmek için aşağıdaki araçlardan birini kullanın:
- Azure fiyatlandırma hesaplayıcısı
- Azure fiyat listesi
- Azure portal

Aşağıdaki bölümlerde yer alan görüntüler, örnek fiyatlandırmayı ABD Doları cinsinden göstermektedir.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcısını kullanarak maliyeti çevrimiçi olarak tahmin etme

Eklemek istediğiniz hizmetin tahmini aylık maliyetini öğrenmek için [fiyat hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/) göz atın. Yerel para biriminizde tahmin almak için para birimini değiştirebilirsiniz.

![Fiyatlandırma hesaplayıcısı menüsünün ekran görüntüsü](./media/getting-started/pricing-calc.png)

Herhangi bir birinci taraf Azure hizmeti için tahmini maliyeti görüntüleyebilirsiniz. Örneğin, aşağıdaki ekran görüntüsünde, bir A1 Windows Sanal Makinesi’ni (VM) sürekli çalışır durumda bırakırsanız aylık 66,96 USD maliyetinin olacağı tahmin edilir:

![Bir A1 Windows sanal makinesinin aylık tahmini maliyetini gösteren fiyatlandırma hesaplayıcısının ekran görüntüsü](./media/getting-started/pricing-calcvm.png)

Fiyatlandırma hakkında daha fazla bilgi için bkz. [ Fiyatlandırma hakkında SSS](https://azure.microsoft.com/pricing/faq/). Bir Azure satış temsilcisiyle iletişime geçmek istiyorsanız, SSS sayfasının en üstünde gösterilen telefon numarasını arayın.

### <a name="view-and-download-azure-price-sheet"></a>Azure fiyat listesini görüntüleme ve indirme

Azure'a bir Kurumsal Anlaşma (EA) veya bir Microsoft Müşteri Sözleşmesi (MCA) aracılığıyla erişiminiz varsa, Azure hesabınızın fiyat listesini görüntüleyebilir ve indirebilirsiniz. Fiyat listesi tüm Azure hizmetlerinin fiyatlarını içeren bir excel dosyasıdır. Daha fazla bilgi için bkz. [Azure fiyatlandırmasını görüntüleme ve indirme](ea-pricing.md).

### <a name="review-estimated-costs-in-the-azure-portal"></a>Azure portalında tahmini maliyetleri gözden geçirme

Azure portalında bir hizmet eklerken aylık tahmini maliyeti görüntüleyebilirsiniz. Örneğin, Windows sanal makinenizin boyutunu seçtiğinizde işlem saatleri için tahmini aylık maliyeti görürsünüz:

![Örnek: aylık tahmini maliyeti gösteren bir A1 Windows sanal makinesi](./media/getting-started/vm-size-cost.png)

## <a name="monitor-costs-when-using-azure-services"></a>Azure hizmetlerini kullanırken maliyetleri izleme
Maliyetleri aşağıdaki araçlarla izleyebilirsiniz:

- Bütçe ve maliyet uyarıları
- Maliyet analizi

### <a name="track-costs-with-budgets-and-cost-alerts"></a>Bütçeleri ve maliyet uyarılarını izleme

Maliyetleri yönetmek için [bütçeler](../costs/tutorial-acm-create-budgets.md) oluşturun ve paydaşların harcama anormalliklerini ve fazla harcamalarını size otomatik olarak bildiren [uyarılar](../costs/cost-mgt-alerts-monitor-usage-spending.md) oluşturun.

### <a name="explore-and-analyze-costs-with-cost-analysis"></a><a name="costs"></a> Maliyet analiziyle maliyetleri araştırma ve analiz etme

Azure hizmetlerinizi çalışır duruma getirdikten sonra Azure harcamanızı izlemek için maliyetleri düzenli olarak denetleyin. Maliyet analizini, Azure kullanımınıza ilişkin maliyetlerin nereden kaynaklandığını anlamak için kullanabilirsiniz.

1. [Azure portalında Maliyet Yönetimi + Faturalama sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade) ziyaret edin.

2. Hizmet, konum ve abonelik gibi çeşitli esaslara göre ayrılmış geçerli maliyeti görmek için ekranın sol tarafındaki **Maliyet analizi**'ne tıklayın. Bir hizmet ekledikten veya satın alma işlemi yaptıktan sonra verilerin gösterilmesi için 24 saat bekleyin. Varsayılan olarak, maliyet analizi, içinde olduğunuz kapsamın maliyetini gösterir. Örneğin, aşağıdaki ekran görüntüsünde Contoso ödeme hesabının maliyeti gösterilmiştir. Maliyet analizinde farklı bir kapsama geçiş yapmak için Kapsam hapını kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](../costs/understand-work-scopes.md#scopes)

    ![Azure portalında maliyet analizi görünümünün ekran görüntüsü](./media/getting-started/cost-analysis.png)

4. Etiketler, kaynak türü ve zaman aralığı gibi çeşitli özelliklere göre filtre uygulayabilirsiniz. Bir özellik için filtre eklemek üzere **Filtre ekle**’ye tıklayın ve filtrelenecek değerleri seçin. Görünümü bir virgülle ayrılmış değerler (.csv) dosyasına aktarmak için **Dışarı Aktar**’ı seçin.

5. Ayrıca, bu etiketin günlük harcama geçmişini görmek için grafiğin etiketlerine tıklayabilirsiniz. Örneğin: Aşağıdaki ekran görüntüsünde, sanal makinelere tıklandığında VM'lerinizi çalıştırmanın günlük maliyeti gösterilir.

    ![Azure portalında harcama geçmişi görünümünün ekran görüntüsü](./media/getting-started/costhistory.png)

## <a name="optimize-and-reduce-costs"></a>Maliyetleri iyileştirme ve azaltma
Maliyet yönetimi ilkelerini bilmiyorsanız, [Azure Maliyet Yönetimi ile bulut yatırımınızı iyileştirme](../costs/cost-mgt-best-practices.md) makalesini okuyun.

Azure portalında ayrıca VM’ler için otomatik kapatma ve Danışman önerileri ile Azure maliyetlerini iyileştirebilir ve azaltabilirsiniz.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>VM'ler için otomatik olarak kapatma gibi maliyet kesme özelliklerini göz önünde bulundurun

Senaryonuza bağlı olarak, Azure portalında sanal makineleriniz için otomatik kapatmayı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager kullanarak VM'ler için otomatik kapatma](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Portalda otomatik kapatma seçeneğinin ekran görüntüsü](./media/getting-started/auto-shutdown.png)

Otomatik kapatma, sanal makine içinde güç seçenekleriyle yaptığınız kapatma işlemiyle aynı değildir. Otomatik kapatma, VM’lerinizi durdurup serbest bırakır ve ek kullanım ücretleri yansıtılmasını durdurur. VM durumları hakkında daha fazla bilgi için [Linux VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows VM](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) için fiyatlandırma hakkında SSS bölümüne bakın.

Geliştirme ve test ortamlarınıza yönelik daha fazla maliyet azaltma özelliği için [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)’i inceleyin.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Danışmanı önerilerini açma ve gözden geçirme

[Azure Danışmanı](../../advisor/advisor-overview.md), düşük kullanımlı kaynakları tanımlayarak maliyetleri azaltmanıza yardımcı olur. Azure portalında **Danışman**’ı arayın:

![Azure portalında Azure Danışmanı düğmesinin ekran görüntüsü](./media/getting-started/advisor-button.png)

Sol taraftan **Maliyet**’i seçin. **Maliyet** sekmesinde eyleme dönüştürülebilir öneriler görürsünüz:

![Danışman maliyet önerisi örneğinin ekran görüntüsü](./media/getting-started/advisor-action.png)

Maliyet tasarrufu sağlayan Danışmanı önerileri hakkında kılavuzlu bir öğretici için [Önerilerle maliyetleri iyileştirme](../costs/tutorial-acm-opt-recommendations.md) öğreticisini gözden geçirin.


## <a name="integrate-with-billing-and-consumption-apis"></a>Fatura ve tüketim API'leriyle tümleştirme

Fatura ve maliyet verilerini program aracılığıyla almak için Azure [fatura](https://docs.microsoft.com/rest/api/billing/) ve [tüketim](https://docs.microsoft.com/rest/api/consumption/) API’lerini kullanın. RateCard API'si ve Kullanım API'sini birlikte kullanarak faturalandırılan kullanımınızı alabilirsiniz. Daha fazla bilgi için bkz. [Microsoft Azure kaynak kullanımınızla ilgili içgörüler edinin](usage-rate-card-overview.md).

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

Maliyetleri görüntülemek için maliyet veya fatura bilgilerine hesap ya da abonelik düzeyinde erişim gerekir. Erişim, ödeme hesabı türüne göre farklılık gösterir. Ödeme hesapları hakkında daha fazla bilgi edinmek ve ödeme hesabınızın türünü kontrol etmek için bkz. [Azure portalında ödeme hesaplarını görüntüleme](view-all-accounts.md).

Azure'a bir Microsoft Çevrimiçi Hizmet Programı (MOSP) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure fatura bilgilerine erişimi yönetme](manage-billing-access.md).

Azure'a bir Kurumsal Anlaşma (EA) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure’da Azure Kurumsal Anlaşma yönetim rollerini anlama](understand-ea-roles.md).

Azure'a bir Microsoft Müşteri Sözleşmesi (MCA) ödeme hesabı üzerinden erişiminiz varsa bkz. [Azure’da Microsoft Müşteri Sözleşmesi yönetim rollerini anlama](understand-mca-roles.md).

### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Bir hizmet olayı için Hizmet Düzeyi Sözleşmesi kredisi isteme

Hizmet Düzeyi Sözleşmeleri (SLA), Microsoft’un çalışma süresi ve bağlantı hakkındaki taahhütlerini açıklar. Azure hizmetleri, genellikle *kesinti*olarak adlandırılan, çalışma süresini veya bağlantıyı etkileyen bir sorunla karşılaştığında bir hizmet olayı bildirilir. SLA'da açıklandığı gibi her bir hizmetin Hizmet Düzeylerini sağlayıp sürdürmezsek, aylık hizmet ücretlerinizin bir kısmını kredi olarak kullanmaya uygun olabilirsiniz.

Kredi istemek için:

1. [Azure Portal](https://portal.azure.com/) oturum açın. Birden çok hesabınız varsa, Azure’un kapalı kaldığı süreden etkilenen bir hesabı kullandığınızdan emin olun.
2. Yeni bir destek isteği oluşturun.
3. **Sorun türü** altında **Faturalandırma**’yı seçin.
4. **Sorun türü** altında **Para İadesi İsteği**’ni seçin.
5. SLA kredisi istediğinizi belirtmek için ayrıntılar ekleyin; tarih/saat/saat dilimi ve etkilenen hizmetleri (VM'ler, Web Siteleri vb.) belirtin
6. İletişim bilgilerinizi doğrulayın ve isteğinizi göndermek için **Oluştur**'u seçin.

SLA eşikleri hizmete göre farklılık gösterir. Örneğin, SQL Web Katmanı %99,9, VM’ler %99,95 ve SQL Standard Katmanı %99,99 SLA’ya sahiptir.

Bazı hizmetlerde SLA'nın uygulanması önkoşullara bağlıdır. Örneğin, sanal makinelerin aynı Kullanılabilirlik Kümesinde dağıtılmış iki veya daha fazla örneği olmalıdır.

Daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) ve [Azure hizmetleri için SLA özeti](https://azure.microsoft.com/support/legal/sla/summary/) belgelerine bakın.

## <a name="analyze-unexpected-charges"></a>Beklenmeyen ücretleri analiz etme

Kuruluşunuz için oluşturduğunuz bulut kaynak altyapısı büyük olasılıkla karmaşıktır. Azure kaynak türlerinin pek çoğu farklı ücret türlerine sahip olabilir. Azure kaynakları, kuruluşunuzdaki farklı ekiplere ait olabilir ve çeşitli kaynaklara uygulanan farklı faturalama modeli türlerine sahip olabilir. Ücretleri daha iyi anlamak için, aşağıdaki bölümlerde yer alan bir veya daha çok stratejiden yararlanarak analizinizi başlatın.

### <a name="review-your-invoice-and-identify-the-resource-that-is-responsible-for-the-charge"></a>Faturanızı gözden geçirme ve ücretten sorumlu olan kaynağı belirleme

Azure hizmetlerini satın alma yönteminiz, ücretle ilişkili kaynağı belirledikçe kullanabileceğiniz metodoloji ve araçlara da karar vermenize yardımcı olur. Sizin için geçerli olan metodolojiye karar vermek için öncelikle [Azure teklif türünüze karar verin](../costs/understand-cost-mgt-data.md#determine-your-offer-type). Daha sonra, [desteklenen Azure teklifleri](../costs/understand-cost-mgt-data.md#supported-microsoft-azure-offers) listesinden müşteri kategorinizi belirleyebilirsiniz.

Aşağıdaki makalelerde, müşteri türüne göre faturanızı nasıl gözden geçireceğinizi açıklayan ayrıntılı adımlar sağlanmaktadır. Her bir makalede, verilen faturalama dönemine ait kullanım ve maliyet ayrıntılarını içeren bir CSV dosyasını indirmeye ilişkin yönergeler yer alır.

- [Kullandıkça Öde fatura gözden geçirme süreci](../understand/review-individual-bill.md#compare-invoiced-charges-with-usage-file)
- [Kurumsal Sözleşme fatura gözden geçirme süreci](../understand/review-enterprise-agreement-bill.md)
- [Microsoft Müşteri Sözleşmesini gözden geçirme süreci](../understand/review-customer-agreement-bill.md#analyze-your-azure-usage-charges)
- [Microsoft İş Ortağı Sözleşmesini gözden geçirme süreci](../understand/review-partner-agreement-bill.md#analyze-your-azure-usage-charges)

Azure faturanızda, o aya ait ücretlerinizi _ölçüm_ başına değerlendirilerek toplanır. Ölçümler, bir kaynağın zaman içinde kullanımını izlemek ve faturanızı hesaplamak için kullanılır. Örneğin, sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda bu kaynağa ait bir veya birden fazla ölçüm örneği oluşturulur.

CSV biçimindeki kullanım dosyasını, analiz etmek istediğiniz fatura üzerinde gösterilen _MeterName_’e göre filtreleyip ilgili ölçüme uygulanan tüm satır öğelerini görün. Satır öğesine ait _InstanceID_, ücrete neden olan Azure kaynağına karşılık gelir.

Söz konusu kaynağı tanımladıktan sonra, kaynakla ilgili maliyetleri daha derin analiz etmek için Azure Maliyet Yönetimi’nde maliyet analizini kullanabilirsiniz. Maliyet analizini kullanma hakkında daha fazla bilgi için bkz. [Maliyetleri analiz etmeye başlama](../costs/quick-acm-cost-analysis.md).

### <a name="identify-spikes-in-cost-over-time"></a>Zaman içindeki maliyette oluşan ani artışları belirleme

Bazen faturalanan ücretlerinizde değişikliğe neden olan son maliyetleri bilemeyebilirsiniz. Değişiklikleri anlamak için, [zaman içindeki maliyetlerin günlük veya aylık olarak dökümünü görmek](../costs/cost-analysis-common-uses.md#view-costs-per-day-or-by-month) amacıyla maliyet analizini kullanabilirsiniz. Görünümü oluşturduktan sonra, değişiklikleri tanımlamak için ücretlerinizi **Hizmet** veya **Kaynak** seçeneklerinden biriyle gruplandırın. Dilerseniz verileri daha iyi görselleştirmek için görünümünüzü **Çizgi** grafiği olarak değiştirebilirsiniz.

![Maliyet analizinde zaman içindeki maliyeti gösteren örnek](./media/getting-started/costs-over-time.png)

### <a name="determine-resource-pricing-and-understand-its-billing-model"></a>Kaynak fiyatlandırmasını belirleme ve faturalama modelini anlama

Tek bir kaynak, birden çok Azure ürünü ve hizmeti arasındaki ücretleri tahakkuk edebilir. Her bir Azure hizmetine ait fiyatlandırma hakkında daha fazla bilgi edinmek için [ürüne göre Azure fiyatlandırması](https://azure.microsoft.com/pricing/#product-pricing) sayfasına göz atın. Örneğin, Azure’da oluşturulan tek bir sanal makinenin (VM) kullanımını izlemek için aşağıdaki ölçümler oluşturulmuş olabilir. Her birinin fiyatlandırması farklı olabilir.

- İşlem Saatleri
- IP Adresi Saatleri
- Gelen Veri Aktarımı
- Giden Veri Aktarımı
- Standart Yönetilen Diskler
- Standart Yönetilen Disk İşlemleri
- Standart GÇ-Disk
- Standart GÇ-Blok Blobu Okuma
- Standart GÇ-Blok Blobu Yazma
- Standart GÇ-Blok Blobu Silme

VM oluşturulduğunda her ölçüm kullanım kayıtları üretmeye başlar. Kullanım ve ölçümün fiyatı Azure ölçüm sisteminde izlenir. CSV biçimindeki kullanım dosyanızda faturanızın hesaplanması için kullanılan ölçümleri görebilirsiniz.

### <a name="find-the-people-responsible-for-the-resource-and-engage-them"></a>Kaynaktan sorumlu kişileri bulma ve bu kişilerin katılımını sağlama

Belirli bir kaynaktan sorumlu ekip sıklıkla bu kaynağa yapılan değişiklikler hakkında bilgi sahibidir. Ücretlerin neden oluşabileceğini belirlediğinizden, bu kişilerin katılımını sağlamak yararlıdır. Örneğin, kaynağa sahip olan ekip kaynağı yeni oluşturmuş, SKU’sunu güncelleştirmiş (doğal olarak kaynak ücretini de değiştirmiş) veya kod değişikliklerinden dolayı kaynak üzerindeki yükü artırmış olabilir. Kaynağa sahip olan kişileri belirlemeye yönelik diğer teknikler için aşağıdaki bölümleri okumaya devam edin.

#### <a name="analyze-the-audit-logs-for-the-resource"></a>Kaynağa ait denetim günlüklerini analiz etme

Bir kaynağı görüntüleme iznine sahipseniz kaynağın denetim günlüklerine de erişebilmeniz gerekir. Kaynakta yapılan son değişikliklerden sorumlu olan kullanıcıyı bulmak için günlükleri inceleyin. Daha fazla bilgi edinmek için bkz. [Azure Etkinlik günlüğü olaylarını görüntüleme ve alma](../../azure-monitor/platform/activity-log-view.md).

#### <a name="analyze-user-permissions-to-the-resources-parent-scope"></a>Kaynağın üst kapsamına yönelik kullanıcı izinlerini analiz etme

Bir aboneliğe veya kaynak grubuna yazma erişimi olan kişiler, genellikle oluşturulan kaynaklar hakkında bilgi sahibidir. Bu kişiler, bir kaynağın amacını açıklayabilir veya bilen bir kişiye sizi yönlendirebilir. Bir Abonelik kapsamına yönelik izinlere sahip olan kişileri belirlemek için bkz. [Rol atamalarını görüntüleme](../../role-based-access-control/check-access.md#view-role-assignments). Kaynak grupları için benzer bir süreçten yararlanabilirsiniz.

### <a name="get-help-to-identify-charges"></a>Ücretleri belirlemek için yardım alma

Önceki stratejileri kullandıysanız ve hala neden ücret aldığınızı anlamadıysanız veya faturalama sorunlarıyla ilgili başka konularda yardıma ihtiyacınız varsa lütfen [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Fazla harcamayı önlemek için [harcama limitlerini](spending-limit.md) kullanma hakkında bilgi edinin.
- [Azure maliyetlerinizi analiz etmeye](../costs/quick-acm-cost-analysis.md) başlayın.
