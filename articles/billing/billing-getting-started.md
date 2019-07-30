---
title: Azure 'da beklenmeyen maliyetleri önleyin ve faturalandırmayı yönetin
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
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68611993"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure Faturalandırma ve maliyet yönetimiyle ilgili beklenmedik ücretleri engelleyin

Azure 'a kaydolduğunuzda, harcamalarınızın daha iyi bir fikir sahibi olmak için yapabileceğiniz birkaç şey vardır:

- [Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/) , bir Azure kaynağı oluşturmadan önce maliyetleri tahmin edebilir. 

- [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) , aboneliğiniz için geçerli maliyet dökümünü ve tahminini sağlar. 

- Farklı projeler veya takımlar için maliyetleri gruplandırmak ve anlamak isterseniz, [kaynak etiketleme](../azure-resource-manager/resource-group-using-tags.md)bölümüne bakın. Kuruluşunuzun kullanmayı tercih ettiğiniz bir raporlama sistemi varsa, [faturalandırma API 'lerine](billing-usage-rate-card-overview.md)göz atın.

- Aboneliğiniz bir Kurumsal Anlaşma (EA) ile oluşturulduysa, maliyetlerinizi Azure portal görüntüleyebilirsiniz. Aboneliğiniz bir bulut çözümü sağlayıcısı (CSP) veya Azure Sponsorluğu, aşağıdaki özelliklerden bazıları sizin için uygulanmayabilir. Daha fazla bilgi için bkz. [EA, CSP ve sponsorluk Için ek kaynaklar](#other-offers).

- Aboneliğiniz ücretsiz bir deneme teklif, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Open ile Azure (aio) veya BizSpark ise, tüm kredileriniz kullanılırken aboneliğiniz otomatik olarak devre dışı bırakılır. Aboneliğinizin beklenmedik şekilde devre dışı bırakılmasından kaçınmak için [harcama limitleri](#spending-limit) hakkında bilgi edinin.

- [Azure Ücretsiz hesabına](https://azure.microsoft.com/free/)kaydolduysanız, [en popüler Azure hizmetlerinden bazılarını 12 ay boyunca ücretsiz olarak kullanabilirsiniz](billing-create-free-services-included-free-account.md). Aşağıda listelenen önerilere birlikte, bkz. [ücretsiz hesap için ücretlendirmemeye özen gösterin](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure hizmetlerini eklemeden önce tahmini maliyetleri alın

Aşağıdaki araçları kullanarak maliyetleri tahmin etme hakkında bazı ek bilgiler aşağıda verilmiştir:
- Azure fiyatlandırma hesaplayıcısı
- Azure portal
- Harcama limiti

Aşağıdaki bölümlerde yer aldığı resimlerde ABD Doları cinsinden örnek fiyatlandırma gösterilmektedir.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcısını kullanarak maliyeti çevrimiçi olarak tahmin etme

İlgilendiğiniz hizmetin tahmini aylık maliyetini öğrenmek için [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' na göz atın. Tahmini bir maliyet almak için ilk taraf Azure kaynağı ekleyebilirsiniz. Fiyatlandırma hesaplayıcıda, para birimi türünü değiştirebilirsiniz.

![Fiyatlandırma Hesaplayıcı menüsünün ekran görüntüsü](./media/billing-getting-started/pricing-calc.png)

Örneğin, Fiyatlandırma Hesaplayıcı ' da, bir a1 Windows sanal makinesi (VM), belirli bir süre boyunca çalışır durumda bırakırsanız, işlem saatlerinde belirli bir tutarın/ayın maliyeti kadar tahmin edilir:

![Ayda bir a1 Windows VM tahmini maliyeti gösteren Fiyatlandırma Hesaplayıcı ekran görüntüsü](./media/billing-getting-started/pricing-calcvm.png)

Fiyatlandırma hakkında daha fazla bilgi için bkz. [FIYATLANDıRMA SSS](https://azure.microsoft.com/pricing/faq/). Bir Azure satış temsilcisiyle iletişim sağlamak istiyorsanız, SSS sayfasının en üstünde gösterilen telefon numarasını çağırın.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Azure portal Tahmini maliyetleri gözden geçirin

Genellikle, Azure portal bir hizmet eklediğinizde, faturalandırılan para biriminde aylık tahmini maliyet gösteren bir görünüm vardır. Örneğin, Windows VM 'nizin boyutunu seçtiğinizde, işlem saatleri için tahmini aylık maliyeti görürsünüz:

![Örnek: aylık tahmini maliyet gösteren bir a1 Windows sanal makinesi](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a>Üzerinde harcama sınırı olup olmadığını denetleyin

Kredilerin kullanıldığı bir aboneliğiniz varsa, harcama limiti sizin için varsayılan olarak açıktır. Bu şekilde, tüm kredilerinizi harcadığınızda kredi kartınız ücretlendirilmez. [Azure tekliflerinin tam listesine ve harcama limitinin kullanılabilirliğine](https://azure.microsoft.com/support/legal/offer-details/)bakın.

Ancak, harcama limitinize ulaştığınızda, hizmetleriniz devre dışı bırakılır. Bu, sanal makinelerinizin serbest bırakıldığı anlamına gelir. Hizmet kapalı kalma süresini önlemek için harcama limitini kapatmanız gerekir. Tüm fazla kullanım, kredi kartınıza dosya üzerinden ücretlendirilir.

Bir harcama limiti olup olmadığını görmek için [Hesap Merkezi 'Nde abonelikler görünümüne](https://account.windowsazure.com/Subscriptions)gidin. Harcama limiti açık ise, aşağıdakine benzer bir başlık görüntülenir:

![Hesap merkezinde harcama limiti hakkında bir uyarı gösteren ekran görüntüsü](./media/billing-getting-started/spending-limit-banner.png)

Başlık ' a tıklayıp Harcama limitini kaldırmak için yönergeleri izleyin. Kaydolduğunuzda kredi kartı bilgileri girmezseniz, harcama limitini kaldırmak için bunu girmeniz gerekir. Daha fazla bilgi için bkz. [Azure harcama limiti – nasıl çalıştığı ve nasıl etkinleştirileceği veya kaldırılacağı](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Bütçeleri ve maliyet uyarılarını kullanma

Maliyetleri yönetmek ve harcama bozuklukları ve fazla harcama riskleri ile ilgili katılımcıları otomatik olarak bildiren [Uyarılar](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) oluşturmak için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcama tabanlıdır.

## <a name="monitor-costs-when-using-azure-services"></a>Azure hizmetlerini kullanırken maliyetleri izleme
Aşağıdaki araçlarla maliyetleri izleyebilirsiniz:

- Tags
- Maliyet dökümü ve yazma oranı
- Maliyet analizi

### <a name="tags"></a>Faturalama verilerini gruplandırmak için kaynaklara etiketler ekleme

Desteklenen hizmetlere yönelik faturalandırma verilerini gruplandırmak için Etiketler kullanabilirsiniz. Örneğin, farklı takımlar için birkaç VM çalıştırırsanız, maliyetleri maliyet merkezine göre kategorilere ayırmak için etiketleri kullanabilirsiniz (örneğin: IK, pazarlama, finans vb.) ya da ortam (örneğin: üretim, ön üretim, test).

![Portalda etiketleri ayarlamayı gösteren ekran görüntüsü](./media/billing-getting-started/tags.png)

Etiketler, farklı maliyet raporlama görünümlerinin tamamında gösterilir. Örneğin, ilk fatura döneminizin ardından, [Maliyet Analizi görünümünüzde](#costs) ve AYRıNTıLı kullanım CSV dosyasında görünür hale getiriyoruz.

Daha fazla bilgi için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a>Maliyet dökümünü ve yazma hızını izleyin

Azure hizmetlerinizin çalışmasını tamamladıktan sonra düzenli olarak ücretlendirin. Azure portal geçerli harcama ve yazma oranını görebilirsiniz.

1. [Azure Portal abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ' i ziyaret edin ve bir abonelik seçin.

2. Aboneliğiniz için destekleniyorsa, maliyet dökümünü ve yazma oranını görürsünüz.

    ![Azure portal yazma oranı ve dökümü ekran görüntüsü](./media/billing-getting-started/burn-rate.PNG)

3. Kaynağa göre maliyet dökümünü görmek için soldaki listede [Maliyet Analizi](../cost-management/quick-acm-cost-analysis.md) ' ne tıklayın. Bir hizmet ekledikten sonra, verilerin görüntülenmesi için 24 saat bekleyin.

    ![Azure portal maliyet analizi görünümünün ekran görüntüsü](./media/billing-getting-started/cost-analysis.png)

4. [Etiketler](#tags), kaynak türü, kaynak grubu ve zaman aralığı gibi farklı özelliklere göre filtre uygulayabilirsiniz. Görünümü bir virgülle ayrılmış değerler (.  CSV) dosyasına aktarmak istiyorsanız, filtreleri onaylamak için **Uygula** ' ya tıklayın.

5. Ayrıca, günlük harcama geçmişinizi ve her gün kaynak maliyetlerinin ne kadarının gösterileceğini görmek için bir kaynağa tıklayabilirsiniz.

    ![Azure portal harcama geçmişi görünümünün ekran görüntüsü](./media/billing-getting-started/costhistory.png)

Hizmetleri seçtiğinizde gördüğünüz tahminlerle gördüğünüz ücretleri karşılaştırın. Maliyetler tahminlerden önemli ölçüde farklıysa, kaynaklarınız için seçtiğiniz fiyatlandırma planını kontrol edin.

## <a name="optimize-and-reduce-costs"></a>Maliyetleri iyileştirme ve azaltma
Maliyet yönetimi ilkelerine alışkın değilseniz, [Azure maliyet yönetimi ile bulut yatırımınızın nasıl iyileştirileceği hakkında](../cost-management/cost-mgt-best-practices.md)bilgi edinin.

Azure portal Ayrıca, VM 'Ler ve danışman önerileri için otomatik kapanmaya sahip Azure maliyetlerini iyileştirede azaltabilirsiniz.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>VM 'Ler için otomatik olarak kapatmalar gibi maliyet kesme özelliklerini göz önünde bulundurun

Senaryonuza bağlı olarak, Azure portal sanal makinelerinize yönelik otomatik olarak kapatılmasını yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager kullanarak VM 'ler Için otomatik olarak kapatmalar](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Portalda otomatik kapatılma seçeneğinin ekran görüntüsü](./media/billing-getting-started/auto-shutdown.png)

Otomatik kapatma, sanal makine içinde güç seçenekleriyle kapattığınız ile aynı değildir. Otomatik olarak kapatmalar, ek kullanım ücretlerini durdurmak için sanal makinelerinizi durdurur ve ayırır. Daha fazla bilgi için bkz. sanal makine durumlarıyla ilgili [Linux sanal makineleri](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows VM 'LERI](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) için fiyatlandırma SSS.

Geliştirme ve test ortamlarınız için daha fazla maliyet kesme özelliği için [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)inceleyin.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Advisor önerilerini açın ve gözden geçirin

[Azure Advisor](../advisor/advisor-overview.md) , düşük kullanımlı kaynakları tanımlayarak maliyetleri azaltmanıza yardımcı olur. Azure portal Advisor 'ı ziyaret edin:

![Azure portal 'de Azure Advisor düğmesinin ekran görüntüsü](./media/billing-getting-started/advisor-button.png)

Danışman panosundaki **Maliyet** sekmesinde eyleme dönüştürülebilir öneriler alabilirsiniz:

![Danışman maliyet önerisi örneği ekran görüntüsü](./media/billing-getting-started/advisor-action.png)

Maliyet tasarrufu Danışmanı önerileri hakkında kılavuzlu bir öğretici için [öneriler öğreticiden en iyi şekilde maliyetleri](../cost-management/tutorial-acm-opt-recommendations.md) gözden geçirin.

## <a name="review-costs-against-your-latest-invoice"></a>En son faturanızda maliyetleri gözden geçirin

Faturalandırma döngüsünün sonunda, en son faturanızda yer verilir. Ayrıca, doğru şekilde ücretlendirildiğinizden emin olmak için [faturaları ve ayrıntılı kullanım dosyalarını da indirebilirsiniz](billing-download-azure-invoice-daily-usage-date.md) . Günlük kullanımınızı faturanızlarla karşılaştırma hakkında daha fazla bilgi için bkz. [Microsoft Azure Faturanızı Anlama](billing-understand-your-bill.md).

### <a name="billing-api"></a>Faturalama API’si

Kullanım verilerini programlı bir şekilde almak için Azure Faturalandırma API 'sini kullanın. Ücretlendirme kullanımını almak için RateCard API ve kullanım API 'sini birlikte kullanın. Daha fazla bilgi için bkz. [Microsoft Azure Kaynak tüketiminiz hakkında öngörüler elde](billing-usage-rate-card-overview.md)edin.

## <a name="other-offers"></a>Ek kaynaklar ve özel durumlar

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP ve sponsorluk müşterileri
Başlamak için hesap yöneticinize veya Azure iş ortağınızla konuşun.

| Sunduğu | Kaynaklar |
|-------------------------------|-----------------------------------------------------------------------------------|
| Kurumsal Anlaşma (EA) | [EA Portalı](https://ea.azure.com/), [yardım belgeleri](https://ea.azure.com/helpdocs)ve [Power BI raporu](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Bulut Çözümü Sağlayıcısı (CSP) | Sağlayıcınızla konuşun |
| Azure Sponsorluğu | [Sponsorluk portalı](https://www.microsoftazuresponsorships.com/) |

Bunu büyük bir kuruluş için yönetiyorsanız, [Azure Kurumsal yapı iskelesi](/azure/architecture/cloud-adoption-guide/subscription-governance) ve [Kurumsal BT teknik incelemesini](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (. pdf indirme, yalnızca İngilizce) okumanızı öneririz.

#### <a name="EA"></a>Azure portal Kurumsal Anlaşma maliyet görünümleri

Kurumsal maliyet görünümleri Şu anda genel önizlemededir. Notunuza öğeler:

- Abonelik maliyetleri kullanıma dayanır ve ön ödemeli tutarlar, fazla kullanım, dahil edilen miktarlar, ayarlamalar ve vergiler dahil değildir. Gerçek ücretler kayıt düzeyinde hesaplanır.
- Azure portal gösterilen tutarlar, Enterprise Portal 'da bulunandan farklı olabilir. Enterprise Portal 'daki güncelleştirmelerin Azure portal değişikliklerin gösterilmesi birkaç dakika sürebilir.
- Maliyetleri görmüyorsanız, bunun nedeni aşağıdakilerden biri olabilir:
    - Abonelik düzeyinde izinleriniz yok. Kurumsal maliyet görünümlerini görmek için abonelik düzeyinde bir faturalandırma okuyucusu, okuyucu, katkıda bulunan veya sahip olmanız gerekir.
    - Hesap sahibisiniz ve kayıt yöneticiniz "AO görünüm ücretleri" ayarını devre dışı bıraktı.  Maliyetlere erişim sağlamak için kayıt yöneticinizle görüşün.
    - Bir departman yöneticisiyseniz ve kayıt yöneticiniz de aynı **da görüntüleme ücretleri** ayarını devre dışı bıraktı.  Erişim sağlamak için kayıt yöneticinizle görüşün.
    - Azure 'u bir kanal iş ortağı aracılığıyla satın aldınız ve iş ortağı fiyatlandırma bilgilerini serbest bırakmadı.  
- Enterprise Portal 'da maliyet erişimiyle ilgili ayarları güncelleştirirseniz, değişiklikler Azure portal gösterilmeden birkaç dakika gecikme süresi vardır.
- Harcama limiti ve fatura Kılavuzu EA abonelikleri için uygulanmaz.

### <a name="check-your-subscription-and-access"></a>Aboneliğinizi ve erişimi denetleyin

Maliyetleri görüntülemek için, [faturalandırma bilgilerine abonelik düzeyinde erişim](billing-manage-access.md)sahibi olmanız gerekir. [Hesap merkezine](https://account.azure.com/Subscriptions)yalnızca hesap yöneticisi erişebilir, faturalandırma bilgilerini değiştirebilir ve abonelikleri yönetebilirsiniz. Hesap Yöneticisi, kaydolma işleminden geçen kişidir. Daha fazla bilgi için bkz. [aboneliği veya hizmetleri yöneten Azure yönetici rolleri ekleme veya değiştirme](billing-add-change-azure-subscription-administrator.md).

Hesap Yöneticisi olup olmadığını görmek için [Azure Portal abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)' e gidin. Aboneliklerin listesini görüntüleyin ve **Rolünmi**bulun. *Hesap Yöneticisi* rol ise, tam ayrıcalıklarınız olur. *Sahip*gibi başka bir şey söyleyiyorsa, tam ayrıcalıklarınız yoktur.

![Azure portal abonelik görünümündeki rolünüzün ekran görüntüsü](./media/billing-getting-started/sub-blade-view.PNG)

Abonelikleri yönetmek ve faturalandırma bilgilerini değiştirmek için [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa)' ni bulun. Hesap yöneticisinden görevleri tamamlamasını isteyin veya [aboneliği size aktarır](billing-subscription-transfer.md).

Hesap yöneticiniz kuruluşunuz tarafından artık yoksa ve faturalandırmayı yönetmeniz gerekiyorsa [bizimle iletişim kurun](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Hizmet olayı için Hizmet Düzeyi Sözleşmesi kredisi isteme

Servis Düzeyi Sözleşmeleri (SLA), Microsoft’un çalışma süresi ve bağlantı hakkındaki taahhütlerini açıklar. Azure Hizmetleri, genellikle *kesinti*olarak adlandırılan çalışma süresini veya bağlantıyı etkileyen bir sorunla karşılaşarak bir hizmet olayı raporlanır. SLA 'da açıklandığı gibi her bir hizmetin hizmet düzeylerini elde etmeme ve korumuz, aylık hizmet ücretlerinizin bir kısmına doğru kredi için uygun olabilir.

Kredi istemek için:

1. [Azure Portal](https://portal.azure.com/)oturum açın. Birden çok hesabınız varsa, Azure kapalı kalma süresinin etkilediği bir tane kullandığınızdan emin olun. 
2. Yeni bir destek isteği oluşturun.
3. **Sorun türü**altında **faturalandırma**' i seçin.
4. **Sorun türü**altında **para iadesi isteği**' ni seçin.
5. Bir SLA Kredisi isteyip istemediğinizi belirtmek için ayrıntılar ekleyin; tarih/saat/saat dilimi ve etkilenen hizmetlerin (VM 'Ler, Web siteleri vb.) yanı sıra
6. İsteğinizi göndermek için iletişim ayrıntılarınızı doğrulayıp **Oluştur** ' u seçin.

SLA eşikleri hizmete göre farklılık gösterir. Örneğin, SQL web katmanının% 99,9 SLA 'sı vardır, VM 'Lerin% 99,95 ' de bir SLA 'sı vardır ve SQL standart katmanında% 99,99% SLA 'sı vardır.

Bazı hizmetlerde SLA 'nın uygulanması için Önkoşullar vardır. Örneğin, sanal makinelerin aynı Kullanılabilirlik kümesinde dağıtılmış iki veya daha fazla örneği olmalıdır.

Daha fazla bilgi için bkz. [Azure hizmetleri Için](https://azure.microsoft.com/support/legal/sla/summary/) [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/) ve SLA Özeti.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Fazla harcamayı engellemek için [harcama limitlerini](billing-spending-limit.md) kullanma hakkında bilgi edinin.
- [Azure maliyetlerinizi çözümlemeyi](../cost-management/quick-acm-cost-analysis.md)başlatın.
