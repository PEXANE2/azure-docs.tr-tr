---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/27/2020
ms.openlocfilehash: d41affc55134ad34c325c12ab4a14f4013c58f9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371662"
---
Azure rezerve edilmiş bir VM örneğine bağlandığınızda paradan tasarruf edebilirsiniz. Rezervasyon indirimi, rezervasyon kapsamı ve öznitelikleriyle eşleşen çalışan sanal makine sayısına otomatik olarak uygulanır. İndirimleri almak için sanal bir makineye rezervasyon atamanız gerekmez. Ayrılmış bir örnek satın alma işlemi, VM kullanımınızın yalnızca bilgi işlem kısmını kapsar. Windows VM'ler için kullanım ölçer iki ayrı metreye bölünür. Linux metre ile aynı bir işlem ölçer ve Windows IP sayacı var. Satın alma işlemi yaparken gördüğünüz ücretler yalnızca işlem maliyetleri içindir. Ücretlere Windows yazılım maliyetleri dahil değildir. Yazılım maliyetleri hakkında daha fazla bilgi için Azure [Ayrılmış VM Örneklerine dahil olmayan Yazılım maliyetlerine](../articles/cost-management-billing/reservations/reserved-instance-windows-software-costs.md)bakın.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Satın almadan önce doğru VM boyutunu belirleme

Rezervasyon satın almadan önce, ihtiyacınız olan VM'nin boyutunu belirlemeniz gerekir. Aşağıdaki bölümler, doğru VM boyutunu belirlemenize yardımcı olur.

### <a name="use-reservation-recommendations"></a>Rezervasyon önerilerini kullanma

Satın almanız gereken rezervasyonları belirlemeye yardımcı olmak için rezervasyon önerilerini kullanabilirsiniz.

- Azure portalında VM rezerve edilmiş bir örnek satın aldığınızda satın alma önerileri ve önerilen miktar gösterilmektedir.
- Azure Danışmanı, tek tek abonelikler için satın alma önerileri sağlar.  
- API'leri hem paylaşılan kapsam hem de tek abonelik kapsamı için satın alma önerileri almak için kullanabilirsiniz. Daha fazla bilgi için, [kurumsal müşteriler için Ayrılmış örnek satın alma önerisi API'leri'ne](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation)bakın.
- Kurumsal Sözleşme (EA) ve Microsoft Müşteri Sözleşmesi (MCA) müşterileri için, Azure [Tüketim Öngörüleri Power BI içerik paketiyle](/power-bi/service-connect-to-azure-consumption-insights)paylaşılan ve tek abonelik kapsamları için satın alma önerileri mevcuttur.

### <a name="services-that-get-vm-reservation-discounts"></a>VM rezervasyon indirimlerini alan hizmetler

VM rezervasyonlarınız yalnızca VM dağıtımlarınıza değil çeşitli hizmetlerden çıkan VM kullanımına da uygulanabilir. Rezervasyon indirimi alan kaynaklar örnek boyutu esnekliği ayarına bağlı olarak değişir.

#### <a name="instance-size-flexibility-setting"></a>Örnek boyut esnekliği ayarı

Örnek boyutu esnekliği ayarı, ayrılmış örnek indirimlerini hangi hizmetlerin alacağını belirler.

*ConsumedService*`Microsoft.Compute` olduğunda, ayarın açık veya kapalı olmasına bakılmaksızın eşleşen her VM kullanımına rezervasyon indirimleri uygulanır. Dolayısıyla kullanım verilerinizde *ConsumedService* değerini gözden geçirin. Bazı örnekler:

- Sanal makineler
- Sanal makine ölçek kümeleri
- Kapsayıcı hizmeti
- Azure Batch dağıtımları (kullanıcı abonelikleri modunda)
- Azure Kubernetes Hizmeti (AKS)
- Service Fabric

Ayar açık olduğunda, eşleşen her VM kullanımına rezervasyon indirimlerinin uygulanması için *ConsumedService* aşağıdaki öğelerden biri olmalıdır:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft.MachineLearningServices
- Microsoft.Kusto

Kullanımın rezervasyon indirimlerine uygun olup olmadığını saptamak için kullanım verilerinizde *ConsumedService* değerini gözden geçirin.

Örnek boyutu esnekliği hakkında daha fazla bilgi için bkz. [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](../articles/virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

### <a name="analyze-your-usage-information"></a>Kullanım bilgilerinizi analiz edin

Hangi rezervasyonları satın almanız gerektiğini belirlemeye yardımcı olmak için kullanım bilgilerinizi analiz edin. Kullanım verileri kullanım dosyasında ve API'lerde mevcuttur. Hangi rezervasyonu satın alarak bunları birlikte kullanın. Satın alınacak rezervasyon miktarını belirlemek için günlük olarak yüksek kullanıma sahip VM örneklerini denetleyin. Kullanım `Meter` verilerindeki `Product` alt kategori ve alanlardan kaçının. Bunlar, birinci sınıf depolama alanı kullanan VM boyutları arasında ayrım yapmaz. Rezervasyon satın almak için VM boyutunu belirlemek için bu alanları kullanırsanız, yanlış boyutu satın alabilirsiniz. O zaman beklediğiniz rezervasyon indirimini alamazsınız. Bunun yerine, `AdditionalInfo` doğru VM boyutunu belirlemek için kullanım dosyanızdaki alana veya kullanım API'sine bakın.

Kullanım dosyanız, fatura dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Kullanım dosyanızı indirme hakkında daha fazla bilgi için [Azure kullanımınızı ve ücretlerinizi görüntüle ve indirin.](../articles/cost-management-billing/understand/download-azure-daily-usage.md) Daha sonra, kullanım dosyası bilgilerini kullanarak, [hangi rezervasyonsatın alınabileceğini belirleyebilirsiniz.](../articles/cost-management-billing/reservations/determine-reservation-purchase.md)

### <a name="purchase-restriction-considerations"></a>Satın alma kısıtlaması hususları

Ayrılmış VM Örnekleri, bazı istisnalar dışında çoğu VM boyutu için kullanılabilir. Rezervasyon indirimleri aşağıdaki VM'ler için geçerli değildir:

- **VM serisi** - A serisi, Av2 serisi veya G serisi.

- **Önizleme veya Promo VM'ler** - Önizlemede olan veya promosyon ölçer kullanan herhangi bir VM serisi veya boyutu.

- **Bulutlar** - Rezervasyonlar Almanya veya Çin bölgelerinde satın alınamaz.

- **Yetersiz kota** - Tek bir aboneliği kapsamaalan bir rezervasyon, yeni RI aboneliğinde vCPU kotası olmalıdır. Örneğin, hedef aboneliğin D-Serisi için 10 vCPUs kota sınırı varsa, 11 Standard_D1 örneği için rezervasyon satın alamazsınız. Rezervasyonlar için kota denetimi, abonelikte zaten dağıtılan VM'leri içerir. Örneğin, aboneliğin D Serisi için 10 vCPUs kotası varsa ve dağıtılan iki standard_D1 örneği varsa, bu abonelikteki 10 standard_D1 örneği için rezervasyon satın alabilirsiniz. Bu sorunu gidermek için [teklif artış isteği oluşturabilirsiniz.](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md)

- **Kapasite kısıtlamaları** - Nadir durumlarda Azure, bir bölgedeki düşük kapasite nedeniyle VM boyutları alt kümesi için yeni rezervasyonsatın almayı sınırlar.

## <a name="buy-a-reserved-vm-instance"></a>Ayrılmış VM Örneği Satın Alma

[Azure portalından](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D)ayrılmış bir VM örneği satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](../articles/cost-management-billing/reservations/monthly-payments-reservations.md) rezervasyon ödemesi yapın.
Bu gereksinimler ayrılmış bir VM örneğini satın almak için geçerlidir:

- En az bir EA aboneliği veya istediğiniz kadar öde fiyatına sahip bir abonelik için Sahip rolünde olmalısınız.
- EA abonelikleri için **Ayrılmış Örnekler Ekle** seçeneği, [EA portalında](https://ea.azure.com/)etkinleştirilmelidir. Veya, bu ayar devre dışı bırakıldıysa, aboneliğin EA Yöneticisi olmanız gerekir.
- Bulut Çözüm Sağlayıcısı (CSP) programı için rezervasyonları yalnızca yönetici aracılar veya satış temsilcileri satın alabilir.

Bir örneği satın almak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Tüm hizmetler** > **rezervasyonlarını**seçin.
1. Yeni bir rezervasyon satın almak için **Ekle'yi** seçin ve ardından **Sanal makine'yi**tıklatın.
1. Gerekli alanları doldurun. Seçtiğiniz özniteliklerle eşleşen VM örneklerinin çalıştırılması, rezervasyon indirimi almaya uygundur. İndirim alan VM örneklerinizin gerçek sayısı, seçilen kapsama ve miktara bağlıdır.

Bir EA anlaşmanız varsa, hızlı bir şekilde ek örnekler eklemek için **daha fazla ekle seçeneğini** kullanabilirsiniz. Bu seçenek diğer abonelik türleri için kullanılamaz.


| Alan      | Açıklama|
|------------|--------------|
|Abonelik|Abonelik rezervasyon için ödeme yapmak için kullanılır. Abonelikteki ödeme yöntemi, rezervasyon masraflarından tahsil edilir. Abonelik türü bir kurumsal sözleşme olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Microsoft Müşteri Sözleşmesi veya kullandıkça öde oranlarına sahip bireysel bir abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Ücretler, varsa parasal taahhüt bakiyesinden düşülür veya fazlalık olarak tahsil edilir. Kullandıkça öde oranlarına sahip bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.|    
|Kapsam       |Rezervasyonun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsam) kapsayabilir. Seçerseniz: <ul><li>**Tek kaynak grubu kapsamı**: Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.</li><li>**Tek abonelik kapsamı**: Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.</li><li>**Paylaşılan kapsam**: Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. EA müşterileri için faturalandırma bağlamı kayıttır. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.</li></ul>|
|Bölge    |Rezervasyon kapsamındaki Azure bölgesi.|    
|VM Boyutu     |VM örneklerinin boyutu.|
|Optimize etmek için     |Varsayılan olarak VM örnek boyutu esnekliği seçilir. Aynı [VM boyutu grubundaki](../articles/virtual-machines/windows/reserved-vm-instance-size-flexibility.md)diğer VM'lere rezervasyon indirimi uygulamak için örnek boyutu esneklik değerini değiştirmek için **Gelişmiş ayarları** tıklatın. Kapasite önceliği dağıtımlarınızda veri merkezi kapasitesine öncelik verir. İhtiyacınız olduğunda VM örneklerini başlatma yeteneğinize ek güven sağlar. Kapasite önceliği yalnızca rezervasyon kapsamı tek abonelik olduğunda kullanılabilir. |
|Sözleşme Dönemi        |Bir ya da üç yıl.|
|Miktar    |Rezervasyon içinde satın alınan örnek sayısı. Miktar, faturalandırma iskontosu alabilen çalışan VM örneklerinin sayısıdır. Örneğin, Doğu ABD'de 10 Standard_D2 VM çalıştırıyorsanız, çalışan tüm VM'lerin yararını en üst düzeye çıkarmak için miktarı 10 olarak belirtirsiniz. |

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2PjmT]

## <a name="usage-data-and-reservation-utilization"></a>Kullanım verileri ve rezervasyon kullanımı

Kullanım verileriniz, kullanım için sıfır geçerli fiyata sahip olup rezervasyon indirimi alır. Her rezervasyon için hangi VM örneğinin rezervasyon indirimialdığını görebilirsiniz.

Kullanım verilerinde rezervasyon indirimlerinin nasıl göründüğü hakkında daha fazla bilgi için, EA müşterisiyseniz [Kurumsal kaydınız için Azure rezervasyon kullanımını anlayın'](../articles/cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) a bakın. Tek bir aboneliğiniz varsa, [Kullan-Öde aboneliğiniz için Azure rezervasyon kullanımını anlayın'](../articles/cost-management-billing/reservations/understand-reserved-instance-usage.md)a bakın.

## <a name="change-a-reservation-after-purchase"></a>Satın alma işleminden sonra rezervasyonu değiştirme

Satın alma işleminden sonra bir rezervasyon üzerinde aşağıdaki değişiklikleri yapabilirsiniz:

- Rezervasyon kapsamını güncelleştirme
- Örnek boyutu esnekliği (varsa)
- Sahiplik

Ayrıca, bir rezervasyonu daha küçük parçalara bölebilir ve zaten bölünmüş rezervasyonları birleştirebilirsiniz. Değişikliklerin hiçbiri yeni bir ticari işlem yapılmasına veya rezervasyonun bitiş tarihini değiştirmeme neden olmaz.

Satın alma işleminden sonra aşağıdaki değişiklik türlerini doğrudan yapamazsınız:

- Mevcut bir rezervasyonbölgesi
- SKU
- Miktar
- Süre

Ancak, değişiklik yapmak isterseniz rezervasyon *değiştirebilirsiniz.*

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](../articles/cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonu yönetme hakkında bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri’ni Yönetme](../articles/cost-management-billing/reservations/manage-reserved-vm-instance.md).
- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure Rezervasyonları nedir?](../articles/cost-management-billing/reservations/save-compute-costs-reservations.md)
    - [Azure’da Rezervasyonları Yönetme](../articles/cost-management-billing/reservations/manage-reserved-vm-instance.md)
    - [Rezervasyon indiriminin nasıl uygulandığını anlama](../articles/cost-management-billing/manage/understand-vm-reservation-charges.md)
    - [Kullandıkça öde fiyatlarına tabi bir abonelik için rezervasyon kullanımını anlama](../articles/cost-management-billing/reservations/understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlama](../articles/cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
    - [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](../articles/cost-management-billing/reservations/reserved-instance-windows-software-costs.md)
    - [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)
