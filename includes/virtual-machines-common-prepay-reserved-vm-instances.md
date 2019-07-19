---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 07/11/2019
ms.openlocfilehash: 766856438b22661b961bfbadc0b63376031622f6
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850781"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances-ri"></a>Azure ayrılmış VM örnekleri (RI) ile sanal makineler için ön ödeme

Azure ayrılmış sanal makine (VM) örnekleri ile sanal makineler için ön ödeme yapın ve tasarruf edin. Daha fazla bilgi için bkz. [Azure ayrılmış VM örnekleri sunumu](https://azure.microsoft.com/pricing/reserved-vm-instances/).

[Azure Portal](https://portal.azure.com)ayrılmış bir sanal makine örneği satın alabilirsiniz. Örnek satın almak için:

- En az bir kurumsal abonelik veya Kullandıkça Öde tarifesine sahip bir abonelik için sahip rolünde olmanız gerekir.
- Kurumsal abonelikler için, **ayrılmış örneklerin eklenmesi** [EA portalında](https://ea.azure.com)etkinleştirilmelidir. Ya da bu ayar devre dışı bırakılırsa, abonelikte bir EA yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetici aracıları veya satış aracıları ayırmaları satın alabilir.

Ayırma indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen çalışan sanal makine sayısına otomatik olarak uygulanır. [Azure Portal](https://portal.azure.com), POWERSHELL, CLı veya API aracılığıyla ayırma kapsamını güncelleştirebilirsiniz.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Satın almadan önce doğru VM boyutunu belirleme

Bir ayırma satın almadan önce, ihtiyacınız olan sanal makinenin boyutunu belirlemeniz gerekir. Aşağıdaki bölümler, doğru VM boyutunu belirlemenize yardımcı olur.

### <a name="use-reservation-recommendations"></a>Ayırma önerilerini kullanma

Satın almanız gereken rezervasyonları belirlemede yardımcı olması için rezervasyon önerilerini kullanabilirsiniz.

- Satın alma önerileri ve önerilen miktar, Azure portal bir VM ayrılmış örneği satın alırken gösterilmektedir.
- Azure Advisor bireysel abonelikler için satın alma önerileri sağlar.  
- Hem paylaşılan kapsam hem de tek abonelik kapsamı için satın alma önerileri almak üzere API 'Leri kullanabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal müşterilere yönelik ayrılmış örnek satın alma önerisi API 'leri](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).
- EA müşterileri için, [Azure Consumption Insights Power BI içerik paketi](/power-bi/service-connect-to-azure-consumption-insights)ile paylaşılan ve tek abonelik kapsamları için satın alma önerileri sunulmaktadır.

### <a name="services-that-get-vm-reservation-discounts"></a>VM rezervasyon iskontolarını alan hizmetler

VM ayırmaları, yalnızca VM dağıtımlarınız için değil, birden çok hizmetten yayılan VM kullanımına uygulanabilir. Rezervasyon indirimlerini alan kaynaklar, örnek boyutu esneklik ayarına bağlı olarak değişir.

#### <a name="instance-size-flexibility-setting"></a>Örnek boyutu esneklik ayarı

Örnek boyutu esneklik ayarı, hangi hizmetlerin ayrılmış örnek iskontolarını elde edildiğini belirler.

Ayarın açık veya kapalı olup olmadığı, tüketim iskontoları, *Tüketim hizmeti* `Microsoft.Compute`olduğunda, eşleşen tüm sanal makine kullanımına otomatik olarak uygulanır. Bu nedenle, *Tüketimedservıce* değeri için kullanım verilerinizi kontrol edin. Bazı örnekler:

- Sanal makineler
- Sanal makine ölçek kümeleri
- Kapsayıcı hizmeti
- Azure Batch dağıtımları (Kullanıcı abonelikleri modunda)
- Azure Kubernetes Hizmeti (AKS)
- Service Fabric

Ayar açık olduğunda, *Tüketimedservice* aşağıdaki öğelerden herhangi biri olduğunda, rezervasyon iskontoları otomatik olarak eşleşen VM kullanımı için geçerlidir:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft. MachineLearningServices
- Microsoft. kusto

Kullanımın rezervasyon indirimlere uygun olup olmadığını öğrenmek için kullanım verilerinizde *tüketilebilecek hizmet* değerini kontrol edin.

Örnek boyutu esnekliği hakkında daha fazla bilgi için bkz. [ayrılmış VM örnekleri Ile sanal makine boyutu esnekliği](../articles/virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

### <a name="analyze-your-usage-information"></a>Kullanım bilgilerinizi çözümleyin
Satın almanız gereken rezervasyonları belirlemede yardımcı olması için kullanım bilgilerinizi çözümleyin.

Kullanım verileri, kullanım dosyası ve API 'lerde kullanılabilir. Hangi ayırmayı satın almak istediğinizi öğrenmek için bunları birlikte kullanın. Satın alınacak rezervasyonların miktarını öğrenmek için günlük olarak yüksek kullanıma sahip sanal makine örneklerini denetleyin.

Kullanım verilerinde alt `Product`Kategorive alanları kullanmaktan kaçının. `Meter` Premium depolama kullanan VM boyutları arasında ayrım yapmazlar. Bu alanları, rezervasyon satın alma için VM boyutunu tespit etmek üzere kullanırsanız, yanlış boyut satın alabilirsiniz. Daha sonra, beklediğinizi rezervasyon indirimi almazsınız. Bunun yerine, doğru VM `AdditionalInfo` boyutunu öğrenmek için kullanım dosyanızda veya kullanım API 'sindeki alana bakın.

### <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması konuları

Ayrılmış VM örnekleri, bazı özel durumlarla birçok VM boyutu için kullanılabilir. Aşağıdaki VM 'Ler için rezervasyon iskontoları uygulanmaz:

- **VM Serisi** -A serisi, AV2 serisi veya G serisi.

- **Önizlemedeki VM 'ler** -önizlemede olan tüm VM serisi veya boyutları.

- **Bulutlar** -rezervasyonlar Almanya veya Çin bölgelerinde satın alınabilir.

- **Yetersiz kota** -tek bir abonelik kapsamındaki bir ayırma, yeni RI Için abonelikte vCPU kotası kullanılabilir olmalıdır. Örneğin, hedef aboneliğin D serisi için 10 vCPU kota sınırı varsa, 11 Standard_D1 örnek için bir ayırma satın alamazsınız. Rezervasyonların kota denetimi abonelikte zaten dağıtılan VM 'Leri içerir. Örneğin, aboneliğin D serisi için 10 vCPU kotası varsa ve iki standard_D1 örneği dağıtılmışsa, bu abonelikte 10 standard_D1 örnekleri için bir ayırma satın alabilirsiniz. Bu sorunu çözmek için [teklif artışı isteği oluşturabilirsiniz](../articles/azure-supportability/resource-manager-core-quotas-request.md) .

- **Kapasite kısıtlamaları** -nadir koşullarda Azure, bir bölgedeki düşük KAPASITE nedeniyle VM boyutlarının alt kümesi için yeni rezervasyonların satın alınmasını sınırlandırır.

## <a name="buy-a-reserved-vm-instance"></a>Ayrılmış bir sanal makine örneği satın alma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Tüm hizmet** > **ayırmaları**' ni seçin.
1. **Ekle** ' yi seçerek yeni bir ayırma satın alın ve ardından **sanal makine**' ye tıklayın.
1. Gerekli alanları girin. Ayırma iskontosunu almak için, seçtiğiniz özniteliklerle eşleşen VM örnekleri çalıştırma. İskontoyu alan sanal makine örneklerinizin gerçek sayısı, seçilen kapsama ve miktara göre değişir.

| Alan      | Açıklama|
|------------|--------------|
|Subscription|Ayırma için ödeme yapmak üzere kullanılan abonelik. Abonelikteki ödeme yöntemi, rezervasyonun ön maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Kullandıkça Öde tarifesine sahip bireysel bir abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine sahip bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.|    
|`Scope`       |Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz: <ul><li>**Tek kaynak grubu kapsamı** — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.</li><li>**Tek abonelik kapsamı** — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.</li><li>**Paylaşılan kapsam** — fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. Kurumsal Anlaşma müşteriler için, faturalandırma bağlamı kayıt olur. Kullandıkça Öde tarifelerine sahip bireysel abonelikler için faturalandırma kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.</li></ul>|
|Bölge    |Ayırma kapsamındaki Azure bölgesi.|    
|VM Boyutu     |Sanal makine örneklerinin boyutu.|
|İçin iyileştirin     |VM örnek boyutu esnekliği varsayılan olarak seçilidir. Aynı [VM Boyut grubundaki](../articles/virtual-machines/windows/reserved-vm-instance-size-flexibility.md)diğer VM 'lere rezervasyon iskontosunu uygulamak için örnek boyutu esneklik değerini değiştirmek için **Gelişmiş ayarlar** ' a tıklayın. Kapasite önceliği, dağıtımlarınız için veri merkezi kapasitesini önceliklendirir. İhtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneizin daha fazla güvenilirlik sağlar. Kapasite önceliği yalnızca rezervasyon kapsamı tek abonelik olduğunda kullanılabilir. |
|Terim        |Bir yıl veya üç yıl.|
|Miktar    |Ayırma içinde satın alınan örneklerin sayısı. Miktar, fatura iskontosunu elde eden çalışan sanal makine örneklerinin sayısıdır. Örneğin, Doğu ABD 10 Standard_D2 VM çalıştırıyorsanız, çalışan tüm VM 'Lerin avantajını en üst düzeye çıkarmak için miktarı 10 olarak belirtirsiniz. |

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2PjmT]

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir ayırma üzerinde aşağıdaki değişiklik türlerini yapabilirsiniz:

- Ayırma kapsamını Güncelleştir
- Örnek boyutu esnekliği (varsa)
- Fazlasının

Ayrıca, bir ayırmayı daha küçük parçalara bölebilir ve zaten bölünmüş rezervasyonları birleştirebilirsiniz. Değişikliklerden hiçbiri yeni bir ticari işleme neden olmaz ya da ayırmanın bitiş tarihini değiştirmez.

Satın alma işleminden sonra aşağıdaki değişiklik türlerini yapamazsınız:

- Mevcut bir ayırmanın bölgesi
- SKU
- Miktar
- Duration

Ancak, değişiklik yapmak istiyorsanız bir rezervasyon *alışverişi* yapabilirsiniz.

## <a name="cancellations-and-exchanges"></a>İptaller ve alışverişlerde

Ayırma işlemini iptal etmeniz gerekiyorsa %12 erken sonlandırma ücreti tahsil edilebilir. Para iadeleri satın aldığınız fiyattan veya geçerli rezervasyon fiyatından düşük olana göre hesaplanır. Para iadesi yıllık 50.000 ABD doları ile sınırlıdır. Yapılacak para iadesi, %12 erken sonlandırma ücreti kesildikten sonra kalan kullanım süresine göre hesaplanır. İptal istemek için Azure portal rezervasyonuna gidin ve bir destek isteği oluşturmak için **para iadesi** ' ni seçin.

Ayrılmış VM örnekleri ayırmayı başka bir bölgeye, VM boyut grubuna veya terime değiştirmeniz gerekiyorsa, bunu değiş tokuş edebilirsiniz. Exchange, eşit veya daha büyük değer olan başka bir ayırma için olmalıdır. Yeni ayırma işleminin başlangıç tarihi değiştirilen ayırma işleminin başlangıç tarihiyle aynı olmaz. Yeni ayırmayı oluştururken bir veya üç yıllık dönem, öğesinden başlar. Bir Exchange istemek için Azure portal ayırma bölümüne gidin ve bir destek isteği oluşturmak için **Exchange** ' i seçin.

Rezervasyonları Exchange veya para iadesi hakkında daha fazla bilgi için bkz. [rezervasyon alışverişi ve para](../articles/billing/billing-azure-reservations-self-service-exchange-and-refund.md)iadesi.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Bir ayırmayı yönetme hakkında bilgi edinmek için bkz. [Azure ayırmalarını yönetme](../articles/billing/billing-manage-reserved-vm-instance.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure ayırmaları nelerdir?](../articles/billing/billing-save-compute-costs-reservations.md)
    - [Azure 'da ayırmaları yönetme](../articles/billing/billing-manage-reserved-vm-instance.md)
    - [Rezervasyon iskontosunun nasıl uygulanacağını anlayın](../articles/billing/billing-understand-vm-reservation-charges.md)
    - [Kullandıkça Öde tarifesine sahip bir abonelik için rezervasyon kullanımını anlayın](../articles/billing/billing-understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri rezervasyonlar içermez](../articles/billing/billing-reserved-instance-windows-software-costs.md)
    - [Iş Ortağı Merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmaları](https://docs.microsoft.com/partner-center/azure-reservations)
