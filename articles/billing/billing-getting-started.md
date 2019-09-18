---
title: Azure'da beklenmeyen maliyetleri önleme ve faturalandırmayı yönetme
description: Azure faturanızda beklenmeyen ücretleri nasıl önleyeceğinizi öğrenin. Bir Azure aboneliği için maliyet izleme ve yönetim özelliklerini kullanın.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: b64e84c3fff27675029ff35f27972a4aca014ec3
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "68611993"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure faturasında beklenmeyen ücretlendirmeleri önleme ve maliyet yönetimi

Azure'a kaydolduğunuzda, harcamalarınız hakkında daha iyi bir fikir sahibi olmak için yapabileceğiniz birkaç şey vardır:

- [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) bir Azure kaynağı oluşturmadan önce maliyet tahmini sunabilir. 

- [Azure portalı](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), aboneliğiniz için geçerli maliyet dökümünü ve tahminini sağlar. 

- Farklı projeler veya takımlar için maliyetleri gruplandırmak ve anlamak isterseniz [kaynak etiketleme](../azure-resource-manager/resource-group-using-tags.md) bölümüne bakın. Kuruluşunuzun kullanmayı tercih ettiğiniz bir raporlama sistemi varsa, [faturalandırma API’lerine](billing-usage-rate-card-overview.md) göz atın.

- Aboneliğiniz bir Kurumsal Anlaşma (EA) ile oluşturulduysa, maliyetlerinizi Azure portalından görüntüleyebilirsiniz. Aboneliğiniz bir Bulut Çözümü Sağlayıcısı (CSP) veya Azure Sponsorluğu üzerinden yapıldıysa aşağıdaki özelliklerden bazıları sizin için geçerli olmayabilir. Daha fazla bilgi için bkz. [EA, CSP ve Sponsorluk için ek kaynaklar](#other-offers).

- Aboneliğiniz bir ücretsiz deneme teklifi, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Open ile Azure (AIO) veya BizSpark ise, tüm kredileriniz kullanıldığında aboneliğiniz otomatik olarak devre dışı bırakılır. Aboneliğinizin beklenmedik şekilde devre dışı bırakılmasından kaçınmak için [harcama limitleri](#spending-limit) hakkında bilgi edinin.

- Bir [ücretsiz Azure hesabına](https://azure.microsoft.com/free/) kaydolduysanız, [en popüler Azure hizmetlerinden bazılarını 12 ay boyunca ücretsiz kullanabilirsiniz](billing-create-free-services-included-free-account.md). Aşağıda listelenen önerilere birlikte, bkz. [Ücretsiz hesap için ücret yansıtılmasını önleme](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure hizmetlerini eklemeden önce tahmini maliyetleri alma

Aşağıdaki araçları kullanarak maliyetleri tahmin etme hakkında bazı ek bilgiler aşağıda verilmiştir:
- Azure fiyatlandırma hesaplayıcısı
- Azure portal
- Harcama limiti

Aşağıdaki bölümlerde yer alan görüntüler, örnek fiyatlandırmayı ABD Doları cinsinden göstermektedir.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcısını kullanarak maliyeti çevrimiçi olarak tahmin etme

İlgilendiğiniz hizmetin tahmini aylık maliyetini öğrenmek için [fiyatlandırma hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/) göz atın. Tahmini bir maliyet almak için herhangi bir birinci taraf Azure kaynağı ekleyebilirsiniz. Fiyatlandırma hesaplayıcısında para birimi türünü değiştirebilirsiniz.

![Fiyatlandırma hesaplayıcısı menüsünün ekran görüntüsü](./media/billing-getting-started/pricing-calc.png)

Örneğin, fiyatlandırma hesaplayıcısında bir A1 Windows Sanal Makinesi’nin (VM), sürekli çalışır durumda bırakırsanız aylık belirli bir tutar maliyetinin olacağı tahmin edilir:

![Bir A1 Windows sanal makinesinin aylık tahmini maliyetini gösteren fiyatlandırma hesaplayıcısının ekran görüntüsü](./media/billing-getting-started/pricing-calcvm.png)

Fiyatlandırma hakkında daha fazla bilgi için bkz. [ Fiyatlandırma hakkında SSS](https://azure.microsoft.com/pricing/faq/). Bir Azure satış temsilcisiyle iletişime geçmek istiyorsanız, SSS sayfasının en üstünde gösterilen telefon numarasını arayın.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Azure portalında tahmini maliyetleri gözden geçirme

Genellikle, Azure portalında bir hizmet eklediğinizde faturalandırılan para biriminde aylık tahmini maliyet gösteren bir görünüm gösterilir. Örneğin, Windows sanal makinenizin boyutunu seçtiğinizde işlem saatleri için tahmini aylık maliyeti görürsünüz:

![Örnek: aylık tahmini maliyeti gösteren bir A1 Windows sanal makinesi](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a> Üzerinde harcama limitinizin olup olmadığını denetleyin

Kredi kullanan bir aboneliğiniz varsa, harcama limiti sizin için varsayılan olarak açıktır. Bu şekilde, tüm kredilerinizi harcadığınızda kredi kartınızdan ücret alınmaz. [Tüm Azure tekliflerini ve harcama limiti kullanılabilirliğini gösteren listeye](https://azure.microsoft.com/support/legal/offer-details/) göz atın.

Ancak, harcama limitinize ulaştığınızda hizmetleriniz devre dışı bırakılır. Böylece sanal makineleriniz serbest bırakılır. Hizmet kapalı kalma süresini önlemek için harcama limitini kapatmanız gerekir. Tüm fazla kullanımların ücreti kayıtlı kredi kartınıza yansıtılır.

Bir harcama limitinizin olup olmadığını görmek için [Hesap Merkezi’ndeki Abonelikler görünümüne](https://account.windowsazure.com/Subscriptions) gidin. Harcama limitiniz açıksa aşağıdakine benzer bir başlık görüntülenir:

![Hesap Merkezi’nde harcama limitinin açık olması hakkında uyarı gösteren ekran görüntüsü](./media/billing-getting-started/spending-limit-banner.png)

Başlığa tıklayıp harcama limitini kaldırma yönergelerini izleyin. Kaydolduğunuzda kredi kartı bilgilerini girmediyseniz, harcama limitini kaldırmak için girmeniz gerekir. Daha fazla bilgi için bkz. [Azure harcama limiti – Nasıl çalışır, nasıl etkinleştirilir ve nasıl kaldırılır?](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren [uyarılar](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir.

## <a name="monitor-costs-when-using-azure-services"></a>Azure hizmetlerini kullanırken maliyetleri izleme
Maliyetleri aşağıdaki araçlarla izleyebilirsiniz:

- Etiketler
- Maliyet dökümü ve yazma hızı
- Maliyet analizi

### <a name="tags"></a> Faturalama verilerini gruplandırmak için kaynaklara etiketler ekleme

Desteklenen hizmetlere yönelik faturalama verilerini gruplandırmak için etiketleri kullanabilirsiniz. Örneğin, farklı takımlar için birkaç VM çalıştırıyorsanız, maliyetleri maliyet merkezine (örneğin: İK, pazarlama, finans vb.) ya da ortama (örneğin: üretim, üretim öncesi, test) göre kategorilere ayırmak için etiketleri kullanabilirsiniz.

![Portalda etiketleri ayarlamayı gösteren ekran görüntüsü](./media/billing-getting-started/tags.png)

Etiketler, farklı maliyet raporlama görünümlerinin tamamında gösterilir. Örneğin, ilk fatura döneminizin ardından, [maliyet analizi görünümünüzde](#costs) ve ayrıntılı kullanım CSV dosyasında görünür.

Daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Maliyet dökümü ve yazma hızını izleme

Azure hizmetlerinizi çalışır duruma getirdikten sonra ücretleri düzenli olarak denetleyin. Geçerli harcama ve yazma hızını Azure portalından görebilirsiniz.

1. [Azure portalındaki Abonelikler sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ziyaret edip bir abonelik seçin.

2. Aboneliğiniz için destekleniyorsa, maliyet dökümünü ve yazma hızını görürsünüz.

    ![Azure portalındaki yazma hızı ve dökümün ekran görüntüsü](./media/billing-getting-started/burn-rate.PNG)

3. Kaynağa göre maliyet dökümünü görmek için soldaki listede [Maliyet analizi](../cost-management/quick-acm-cost-analysis.md)’ne tıklayın. Bir hizmet ekledikten sonra, verilerin görüntülenmesi için 24 saat bekleyin.

    ![Azure portalında maliyet analizi görünümünün ekran görüntüsü](./media/billing-getting-started/cost-analysis.png)

4. [Etiketler](#tags), kaynak türü, kaynak grubu ve zaman aralığı gibi farklı özelliklere göre filtreleme yapabilirsiniz. Filtreleri onaylamak için **Uygula**’ya ve görünümü virgülle ayrılmış değerler (.csv) dosyasına aktarmak isterseniz **İndir**’e tıklayın.

5. Ayrıca, günlük harcama geçmişinizi ve kaynağın günlük maliyetini görmek için bir kaynağa tıklayabilirsiniz.

    ![Azure portalında harcama geçmişi görünümünün ekran görüntüsü](./media/billing-getting-started/costhistory.png)

Gördüğünü maliyetleri, hizmetleri seçtiğinizde gördüğünüz tahminlerle karşılaştırın. Maliyetler tahminlerden çok farklıysa, kaynaklarınız için seçtiğiniz fiyatlandırma planını kontrol edin.

## <a name="optimize-and-reduce-costs"></a>Maliyetleri iyileştirme ve azaltma
Maliyet yönetimi ilkelerini bilmiyorsanız, [Azure Maliyet Yönetimi ile bulut yatırımınızı iyileştirme](../cost-management/cost-mgt-best-practices.md) makalesini okuyun.

Azure portalında ayrıca VM’ler için otomatik kapatma ve Danışman önerileri ile Azure maliyetlerini iyileştirebilir ve azaltabilirsiniz.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>VM'ler için otomatik olarak kapatma gibi maliyet kesme özelliklerini göz önünde bulundurun

Senaryonuza bağlı olarak, Azure portalında sanal makineleriniz için otomatik kapatmayı yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager kullanarak VM'ler için otomatik kapatma](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Portalda otomatik kapatma seçeneğinin ekran görüntüsü](./media/billing-getting-started/auto-shutdown.png)

Otomatik kapatma, sanal makine içinde güç seçenekleriyle yaptığınız kapatma işlemiyle aynı değildir. Otomatik kapatma, VM’lerinizi durdurup serbest bırakır ve ek kullanım ücretleri yansıtılmasını durdurur. VM durumları hakkında daha fazla bilgi için [Linux VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows VM](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) için fiyatlandırma hakkında SSS bölümüne bakın.

Geliştirme ve test ortamlarınıza yönelik daha fazla maliyet azaltma özelliği için [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)’i inceleyin.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Danışmanı önerilerini açma ve gözden geçirme

[Azure Danışmanı](../advisor/advisor-overview.md), düşük kullanımlı kaynakları tanımlayarak maliyetleri azaltmanıza yardımcı olur. Azure portalında Danışman’ı ziyaret edin:

![Azure portalında Azure Danışmanı düğmesinin ekran görüntüsü](./media/billing-getting-started/advisor-button.png)

Danışman panosundaki **Maliyet** sekmesinden eyleme dönüştürülebilir öneriler alabilirsiniz:

![Danışman maliyet önerisi örneğinin ekran görüntüsü](./media/billing-getting-started/advisor-action.png)

Maliyet tasarrufu sağlayan Danışmanı önerileri hakkında kılavuzlu bir öğretici için [Önerilerle maliyetleri iyileştirme](../cost-management/tutorial-acm-opt-recommendations.md) öğreticisini gözden geçirin.

## <a name="review-costs-against-your-latest-invoice"></a>Maliyetleri en son faturanıza göre gözden geçirme

Faturalama döneminin sonunda en son faturanız oluşturulur. Ayrıca, doğru ücretin yansıtıldığından emin olmak için [faturaları ve ayrıntılı kullanım dosyalarını indirebilirsiniz](billing-download-azure-invoice-daily-usage-date.md). Günlük kullanımınızı faturanızla karşılaştırma hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md).

### <a name="billing-api"></a>Faturalama API’si

Kullanım verilerini programlı bir şekilde almak için Azure faturalandırma API'sini kullanın. RateCard API'si ve Kullanım API'sini birlikte kullanarak faturalandırılan kullanımınızı alabilirsiniz. Daha fazla bilgi için bkz. [Microsoft Azure kaynak kullanımınızla ilgili içgörüler edinin](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Ek kaynaklar ve özel durumlar

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP ve Sponsorluk müşterileri
Başlamak için hesap yöneticiniz veya Azure iş ortağınızla konuşun.

| Sunduğu | Kaynaklar |
|-------------------------------|-----------------------------------------------------------------------------------|
| Kurumsal Anlaşma (EA) | [EA portalı](https://ea.azure.com/), [yardım belgeleri](https://ea.azure.com/helpdocs) ve [Power BI raporu](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Bulut Çözümü Sağlayıcısı (CSP) | Sağlayıcınızla konuşun |
| Azure Sponsorluğu | [Sponsorluk portalı](https://www.microsoftazuresponsorships.com/) |

Büyük bir kuruluşun BT yöneticisiyseniz, [Azure kurumsal iskelesi](/azure/architecture/cloud-adoption-guide/subscription-governance) ve [kurumsal BT teknik incelemesini](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf indirme, yalnızca İngilizce) okumanızı öneririz.

#### <a name="EA"></a> Azure portalındaki Kurumsal Anlaşma maliyet görünümleri

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

Maliyetleri görüntülemek için [fatura bilgilerine abonelik düzeyinde erişim](billing-manage-access.md) izninizin olması gerekir. [Hesap Merkezi](https://account.azure.com/Subscriptions)’ne yalnızca Hesap Yöneticisi erişebilir, faturalandırma bilgilerini değiştirebilir ve abonelikleri yönetebilir. Hesap Yöneticisi, kaydolma işleminden geçen kişidir. Daha fazla bilgi için bkz. [Aboneliği veya hizmetleri yöneten Azure yöneticisi rollerini ekleme veya değiştirme](billing-add-change-azure-subscription-administrator.md).

Hesap Yöneticisi olup olmadığınızı görmek için [Azure portalında Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) sayfasına gidin. Aboneliklerin listesini görüntüleyin ve **Rolüm** seçeneğini bulun. Rol *Hesap yöneticisi* ise, tam ayrıcalıklara sahip olursunuz. *Sahip* gibi bundan başka bir rol gösteriliyorsa tam ayrıcalıklarınız yoktur.

![Azure portalındaki Abonelikler görünümünde rolünüzün ekran görüntüsü](./media/billing-getting-started/sub-blade-view.PNG)

Abonelikleri yönetmek ve faturalandırma bilgilerini değiştirmek için [Hesap Yöneticisini bulun](billing-subscription-transfer.md#whoisaa). Hesap Yöneticisinden görevleri tamamlamasını isteyin veya [aboneliği size aktarmasını](billing-subscription-transfer.md) isteyin.

Hesap yöneticiniz kuruluşunuzda çalışmıyorsa ve faturalandırmayı yönetmeniz gerekiyorsa [bizimle iletişime geçin](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Bir hizmet olayı için Hizmet Düzeyi Sözleşmesi kredisi isteme

Hizmet Düzeyi Sözleşmeleri (SLA), Microsoft’un çalışma süresi ve bağlantı hakkındaki taahhütlerini açıklar. Azure hizmetleri, genellikle *kesinti*olarak adlandırılan, çalışma süresini veya bağlantıyı etkileyen bir sorunla karşılaştığında bir hizmet olayı bildirilir. SLA'da açıklandığı gibi her bir hizmetin Hizmet Düzeylerini sağlayıp sürdürmezsek, aylık hizmet ücretlerinizin bir kısmını kredi olarak kullanmaya uygun olabilirsiniz.

Kredi istemek için:

1. [Azure portalında](https://portal.azure.com/) oturum açın. Birden çok hesabınız varsa, Azure’un kapalı kaldığı süreden etkilenen bir hesabı kullandığınızdan emin olun. 
2. Yeni bir destek isteği oluşturun.
3. **Sorun türü** altında **Faturalandırma**’yı seçin.
4. **Sorun türü** altında **Para İadesi İsteği**’ni seçin.
5. Bir SLA kredisi istediğinizi belirtmek için ayrıntılar ekleyin; tarih/saat/saat dilimi ve etkilenen hizmetleri (VM'ler, Web Siteleri vb.) belirtin
6. İletişim bilgilerinizi doğrulayın ve isteğinizi göndermek için **Oluştur**'u seçin.

SLA eşikleri hizmete göre farklılık gösterir. Örneğin, SQL Web Katmanı %99,9, VM’ler %99,95 ve SQL Standard Katmanı %99,99 SLA’ya sahiptir.

Bazı hizmetlerde SLA'nın uygulanması önkoşullara bağlıdır. Örneğin, sanal makinelerin aynı Kullanılabilirlik Kümesinde dağıtılmış iki veya daha fazla örneği olmalıdır.

Daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) ve [Azure hizmetleri için SLA özeti](https://azure.microsoft.com/support/legal/sla/summary/) belgelerine bakın.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Fazla harcamayı önlemek için [harcama limitlerini](billing-spending-limit.md) kullanma hakkında bilgi edinin.
- [Azure maliyetlerinizi analiz etmeye](../cost-management/quick-acm-cost-analysis.md) başlayın.
