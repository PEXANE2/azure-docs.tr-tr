---
title: Pazar Yeri Ticari İşlem Yetenekleri ve Dikkat Edilmesi Gerekenler | Azure
description: Bu makalede, bir teklif türü için Transact fiyatlandırma, faturalama, faturalama ve ödeme hususlaraçıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: dsindona
ms.openlocfilehash: de93a3c48e0b0aceb447e54e11190f487d1aa2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279836"
---
# <a name="commercial-marketplace-transaction-capabilities-and-considerations"></a>Ticari market işlem özellikleri ve dikkat edilmesi gereken noktalar

Bu makale, ticari pazar için aşağıdaki ticaret le ilgili konuları kapsamaktadır

* Pazar yeri yayımlama seçenekleri
* İşlemgenel genel bakış
* Transact fatura modelleri
* İşlem etüt gereksinimleri

## <a name="marketplace-publishing-options"></a>Pazar yeri yayımlama seçenekleri

Aşağıdaki yayımlama seçenekleri ticari pazar yayıncıları tarafından kullanılabilir.

### <a name="list--trial-publishing-options"></a>Deneme yayımlama seçeneklerini & listele

Yayıncılar, tanıtım ve kullanıcı edinme amacıyla liste, deneme sürümü ve BYOL yayımlama seçeneklerinden yararlanabilir. Bu seçeneklerle Microsoft, yayımcının yazılım lisans işlemlerine doğrudan katılmaz ve ilişkili işlem ücreti yoktur. Yayıncılar, sipariş, karşılama, ölçüm, faturalama, faturalama, ödeme ve tahsilat dahil ancak bunlarla sınırlı olmamak üzere yazılım lisansı işleminin tüm yönlerini desteklemekten sorumludur. Liste ve deneme yayımlama seçenekleriyle yayıncılar, müşteriden toplanan yayıncı yazılım lisans lama ücretlerinin %100'ünü saklar. 

### <a name="transact-publishing-option"></a>İşleme yayımlama seçeneği

Liste ve deneme yayımlama seçeneklerine ek olarak, işlem yayımlama seçeneği yayıncılar tarafından kullanılabilir. Bu, Microsoft'un genel olarak kullanılabilen ticaret özelliklerinden yararlanır ve Microsoft'un bulut pazarı işlemlerini yayımcı adına barındırmasına olanak tanır.

## <a name="transact-general-overview"></a>İşlemgenel genel bakış

Microsoft, işlem yayımlama seçeneğini kullanırken, üçüncü taraf yazılımlarının satılmasını ve bazı teklif türlerinin müşterinin Azure aboneliğine dağıtılmasını sağlar. Yayıncı, bir faturalandırma modeli ve teklif türü seçerken altyapı ücretlerinin faturalandırmasını ve yayıncının kendi yazılım lisanslama ücretlerini göz önünde bulundurmalıdır.

Transact yayımlama seçeneği şu anda aşağıdaki teklif türleri için desteklenir: Sanal Makineler, Azure Uygulamaları ve SaaS Uygulamaları.


![[Azure Marketinde Kurumsal Fırsatları Niçin Aktaran]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Altyapı maliyetlerini faturalandırma

**Sanal Makineler ve Azure uygulamaları için**

Sanal Makineler ve Azure Uygulamaları için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğine fatura edilir.  Altyapı kullanım ücretleri fiyatlandırılır ve müşterinin faturasındaki yazılım sağlayıcısının lisans ücretlerinden ayrı olarak sunulur.

**SaaS uygulamaları için**

SaaS Apps için yayımcının Azure altyapı kullanım ücretlerini ve yazılım lisanslama ücretlerini tek bir maliyet öğesi olarak hesaba katması gerekir.  Bu müşteri için sabit bir ücret olarak temsil edilir. Azure altyapı kullanımı yönetilir ve doğrudan iş ortağına faturalandırılır.  Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez.  Yayıncılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmalarına dahil etmeyi tercih eder.  Yazılım lisans ücretleri ölçülü veya tüketime dayalı değildir.

## <a name="transact-billing-models"></a>Transact fatura modelleri

Kullanılan işlem seçeneğine bağlı olarak, yayıncının yazılım lisans ücretleri aşağıdaki gibi sunulabilir:  

* Ücretsiz: Yazılım lisansları için ücretsizdir. 

* Kendi lisansınızı getirin (BYOL): Yazılım lisansları için geçerli olan tüm ücretler doğrudan yayıncı ve müşteri arasında yönetilir. Microsoft yalnızca Azure altyapı kullanım ücretlerinden geçer. (Yalnızca Sanal Makineler ve Azure Uygulamaları.)

* Kullandıkça Öde: Yazılım lisans ücretleri, kullanılan Azure altyapısına dayalı olarak saat başına, çekirdek başına (vCPU) fiyatlandırma oranı olarak sunulur. Bu yalnızca Sanal Makineler ve Azure Uygulamaları için geçerlidir.

* Abonelik fiyatlandırması: Yazılım lisans ücretleri aylık veya yıllık, yinelenen ücret olarak sabit oranlı veya koltuk başına faturalandırılır. Bu yalnızca SaaS Uygulamaları ve Azure Uygulamaları - Yönetilen Uygulamalar için geçerlidir.

* Ücretsiz yazılım deneme: 30 gün veya 90 gün boyunca yazılım lisansları için ücretsizdir.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Ücretsiz ve getir-your-own-lisans (BYOL) fiyatlandırma

Ücretsiz veya kendi lisansını getir işlem teklifi yayımlarken, Microsoft yazılım lisans ücretleriniz için satış işlemini kolaylaştırmada rol oynamaz. Liste ve deneme yayımlama seçenekleri gibi, yayıncı yazılım lisans ücretlerinin %100'ünü tutar. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Gidilen ödeme ve abonelik (site tabanlı) fiyatlandırması

Microsoft, istediğiniz kadar öde veya abonelik işlemi teklifi yayımlarken, yazılım lisansı satın alma işlemlerini, iadeleri ve geri iadeleri işlemek için teknoloji ve hizmetler sağlar. Bu senaryoda, yayımcı Microsoft'a bu amaçlar için aracı olarak hareket etmesi için yetki verebilmiştir. Yayımcı, Microsoft'un yazılım lisanslama işlemini kolaylaştırmasına izin verirken, satıcı, sağlayıcı, dağıtıcı ve lisansveren olarak atanmasını sağlar.

Microsoft, hem Microsoft'un ticari Pazarı'nın hem de yayıncının son kullanıcı lisans sözleşmesinin hüküm ve koşullarına tabi olarak müşterilerin yayımcı yazılımlarını sipariş etmesini, lisanslanmasını ve kullanmasını sağlar. Yayıncılar, son kullanıcı lisans sözleşmelerini sağlamalı veya teklifi oluştururken [Standart Sözleşme'yi](https://docs.microsoft.com/azure/marketplace/standard-contract) seçmelidir.


### <a name="free-software-trials"></a>Özgür yazılım denemeleri

Yayımlama senaryolarını işleyiş için yayımcı, bir yazılım lisansını 30 gün veya 90 gün boyunca ücretsiz olarak kullanılabilir hale getirebilir. Bu indirim yeteneği, iş ortağı çözümünün kullanımıyla yönlendirilen Azure altyapı kullanım maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Yayıncılar, teklif türlerinden ve faturalandırma modellerinden para kazanmak için kullanmanın yanı sıra, anlaşmalı, anlaşmaya özgü fiyatlandırma veya özel yapılandırmalarla birlikte özel bir teklif sunabilir. Özel teklifler, 3 işlem yayımlama seçeneğinin tümü tarafından desteklenir.

Bu seçenek, herkese açık tekliften daha yüksek veya daha düşük fiyatlandırma sağlar. Özel teklifler indirim yapmak veya bir teklif için prim eklemek için kullanılabilir. Azure aboneliklerini teklif düzeyinde beyaz listeleyerek bir veya daha fazla müşterinin özel teklifleri kullanılabilir.


### <a name="examples"></a>Örnekler

**Kullandıkça Öde** 

* Çünkü'e Git seçeneğini etkinleştiriseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz  | $1.00 /saat  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-Core)    |   $0.14 /saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$1.14 /saat*       |

* Bu senaryoda, Microsoft yayınlanan VM resminizin kullanımı için saat başına 1,14 TL faturalar.

|Microsoft faturaları  | $1.14 /saat  |
|---------|---------|
|Microsoft, lisans maliyetinizin %80'ini size öder|   $0.80 /saat     |
|Microsoft, lisans maliyetinizin %20'sini tutar  |  $0.20 /saat       |
|Microsoft, Azure kullanım maliyetinin %100'ünü tutar | $0.14 /saat |

**Kendi Lisansını Getir (BYOL)**

* BYOL seçeneğini etkinleştirirseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz  | Lisans ücreti sizin yerinizle görüşülüp faturalandırılır  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-Core)    |   $0.14 /saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$0.14 /saat*       |

* Bu senaryoda, Microsoft yayınlanan VM resminizin kullanımı için saat başına 0,14 TL faturalar.

|Microsoft faturaları  | $0.14 /saat  |
|---------|---------|
|Microsoft Azure kullanım maliyetini korur    |   $0.14 /saat     |
|Microsoft lisans maliyetinin %0'ını tutar   |  $0.00 /saat       |

**SaaS Uygulama aboneliği**

Bu seçenek Microsoft üzerinden satış yapacak şekilde yapılandırılmalıdır ve aylık veya yıllık bazda sabit bir oranda veya kullanıcı başına fiyatlandırılabilir.
*    SaaS teklifi için Microsoft üzerinden Sat seçeneğini etkinleştiriseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz       | $100.00 /ay  |
|--------------|---------|
|Azure kullanım maliyeti (D1/1-Core)    | Müşteriye değil, doğrudan yayımcıya faturalandırılır |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *Ayda $100.00 (not: yayıncı lisans ücretinde tahakkuk eden veya geçiş yapan altyapı maliyetlerini hesaba katmalıdır)*  |

* Bu senaryoda, Microsoft yazılım lisansınız için 100,00 TL fatura eder ve yayımcıya 80,00 TL öder.
* İndirimli Pazar Yeri Hizmet Ücreti'ne hak kazanan iş ortakları, Mayıs 2019'dan Haziran 2020'ye kadar SaaS tekliflerinde indirimli işlem ücreti görebilecektir. Bu senaryoda, Microsoft yazılım lisansınız için 100,00 TL fatura eder ve yayımcıya 90,00 TL öder.

|Microsoft faturaları  | $100.00 /ay  |
|---------|---------|
|Microsoft, lisans maliyetinizin %80'ini size öder <br> \*Microsoft, herhangi bir nitelikli SaaS uygulaması için lisans maliyetinizin %90'ını öder   |   $80.00 /ay <br> \*$90.00 /ay    |
|Microsoft, lisans maliyetinizin %20'sini tutar <br> \*Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinin %10'unu tutar.  |  $20.00 /ay <br> \*10,00 $     |

* **İndirimli Pazar Yeri Hizmet Ücreti:** Ticari Pazarımızda yayınladığınız belirli SaaS Ürünleri için Microsoft, Pazar Yeri Hizmet Ücretini %20'den (Microsoft Publisher Anlaşması'nda açıklandığı gibi) %10'a düşürecektir.  Ürününüzün hak kazanabilmesi için, ürünlerinizden en az birinin Microsoft tarafından IP ortak satış hazır veya IP ortak satışı öncelikli olarak belirtilmesi gerekir. Bu indirimli Market Hizmet Ücretini aya alabilmek için, uygunluğun bir önceki takvim ayının bitiminden en az beş (5) iş günü önce karşılanması gerekir. İndirimli Pazar Yeri Hizmeti ücreti, Ticari Pazar Yeri miz aracılığıyla sunulan VM'ler, Yönetilen Uygulamalar veya diğer ürünler için geçerli olmayacaktır.  Bu İndirimli Pazar Yeri Hizmet Ücreti, Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 tarihleri arasında tahsil edilen lisans ücretleriyle nitelikli tekliflere sunulacaktır.  Bu süre sonunda, Market Servis Ücreti normal tutarına geri dönecektir.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Müşteri faturalama, ödeme, faturalandırma ve tahsilatlar

**Faturalama ve ödeme**

Publisher, abonelik veya PAYGO yazılım lisans ücretleri sunmak için müşterinin tercih ettiği faturalama yöntemini kullanabilir.

**Kurumsal anlaşma** 

Müşterinin tercih ettiği faturalandırma yöntemi Microsoft Kurumsal Sözleşmesi ise, yazılım lisans ücretleriniz bu faturalama yöntemi kullanılarak Azure'a özgü kullanım maliyetlerinden ayrı olarak ayrıntılı bir maliyet olarak faturalandırılır.

**Kredi kartları ve aylık fatura** 

Müşteriler kredi kartı ve aylık fatura kullanarak da ödeme yapabilir. Bu durumda, yazılım lisans ücretleriniz, Azure'a özel tüm kullanım maliyetlerinden ayrı olarak, ayrıntılı bir maliyet olarak Kurumsal Sözleşme senaryosunda olduğu gibi faturalandırılır.

Örneğin, müşteri kredi kartı kullanarak alışveriş yapıyorsa:

|Açıklama    |    Tarih  |
|----------|----------|
|Sipariş Dönemi   | 15 Ağustos 2018 - 30 Ağustos 2018 |
|Dönem Sonu (ay)   | Ağu 30, 2018 |
|Fatura Tarihi | 1 Eylül 2018 |
|Müşteri Ödeme Tarihi | 1 Eylül 2018 |
|Emanet Dönemi (sadece kredi kartları, 30 gün) | 1 Eylül 2018 - 30 Eylül 2018 |
|Tahsilat Dönemi Başlangıcı | 1 Eylül 2018 |
|Tahsilat Dönemi Sonu (en fazla 30 gün) | 30 Eylül 2018 |
|Ödeme Hesaplama Tarihi (ayın 15'inde aylık) | Eki 1, 2018 |
|Ödeme Tarihi | 15 Ekim 2018 |

Müşteri Kurumsal Sözleşme kullanarak alışveriş yaparsa:

| Açıklama |    Tarih  |
|----------|----------|
|Sipariş Dönemi | 15 Ağustos 2018 - 30 Ağustos 2018 |
|Dönem Bitişi (üç aylık dönem) | 30 Eylül 2018 |
|Fatura Tarihi | 15 Ekim 2018 |
|Emanet Dönemi (sadece kredi kartları, 30 gün) | yok |
|Tahsilat Dönemi Başlangıcı | 15 Ekim 2018 |
|Tahsilat Dönemi Sonu (maksimum, 90 gün) | Ocak 15, 2019 |
|Müşteri Ödeme Tarihi | 30 Aralık 2018 |
|Ödeme Hesaplama Tarihi (ayın 15'inde aylık) | Ocak 15, 2019 |
|Ödeme Tarihi | Şub 15, 2019 |

**Ücretsiz krediler ve parasal taahhüt** 

Bazı müşteriler Kurumsal Sözleşme'de parasal bir taahhütle Azure'a ön ödeme yapmayı seçer veya Azure ile kullanılmak üzere ücretsiz kredi sağlanmıştır. Bu krediler Azure kullanımı için kullanılabilir, ancak yayımcı yazılım lisans ücretlerini ödemek için kullanılamaz.

**Faturalama ve tahsilatlar** 

Publisher yazılım lisansı faturalandırması, müşteri tarafından seçilen faturalama yöntemi kullanılarak sunulur ve faturalama zaman çizelgesini izler. Kurumsal Sözleşmesi olan müşteriler, pazar yeri yazılım lisansları için aylık olarak faturalandırılır. Kurumsal Sözleşmesi olan müşteriler, üç ayda bir sunulan bir fatura yla aylık olarak faturalandırılır.

Abonelik veya You-Go Olarak Öde fiyatlandırma modelleri seçildiğinde, Microsoft yayımcının aracısı olarak hareket eder ve faturalama, ödeme ve tahsilatın tüm yönlerinden sorumludur.

### <a name="publisher-payout-and-reporting"></a>Yayıncı ödeme ve raporlama

* Microsoft tarafından aracı olarak toplanan yazılım lisans ücretleri, aksi belirtilmedikçe ve yayımcı ödemesi sırasında düşülmedikçe %20 işlem ücretine tabidir.

* Müşteriler genellikle Kurumsal Sözleşme'yi veya kredi kartı etkinleştirilmiş kullandıkça öde sözleşmesini kullanarak satın satın almazlar. Anlaşma türü faturalama, faturalama, tahsilat ve ödeme zamanlamasını belirler.

>[!NOTE] 
>İşleme yayıncılık seçeneğine ilişkin tüm raporlama ve öngörülere, İş Ortağı Merkezi'nin Bulut İş Ortağı Portalı veya Analytics bölümünün Öngörüler bölümünden ulaşabilirsiniz.

#### <a name="billing-questions-and-support"></a>Faturalama soruları ve desteği

Daha fazla bilgi ve yasal ilkeler için [Publisher Sözleşmesi'ne](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (Bulut İş Ortağı Portalı'nda mevcuttur) bakın.

Faturalandırma sorularında yardım almak için lütfen [ticari pazar yeri yayıncı desteğine](https://aka.ms/marketplacepublishersupport)başvurun.

## <a name="transact-requirements"></a>İşlem etüt gereksinimleri

Farklı teklif türleri için işlem gereksinimleri bu bölümde ele alınmıştır.

### <a name="requirements-for-all-offer-types"></a>Tüm teklif türleri için gereksinimler

- Teklifin fiyatlandırma modeline bakılmaksızın, işlem yayımlama seçeneği için bir Microsoft hesabı ve mali bilgiler gereklidir.
- Zorunlu finansal bilgiler ödeme hesabı ve vergi profilini içerir.

Bu hesapların ayarlanması hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/manage-account)


### <a name="requirements-for-specific-offer-types"></a>Belirli teklif türleri için gereksinimler

İşleme yayımlama seçeneği yalnızca aşağıdaki pazar teklifi türleri ile kullanılabilir: 

**Sanal Makine** 

Ücretsiz, kendi lisansını getir veya öde-as-you-go-fiyatlandırma modelleri arasından seçim yapın ve teklif düzeyinde tanımlanan STU'lar gibi mevcut olun. Müşterinin Azure faturasında Microsoft, yayımcı yazılım lisans ücretlerini temel Azure altyapı ücretlerinden ayrı olarak sunar. Azure altyapı ücretleri, yayımcı yazılımının kullanımıyla belirlenir.

**Azure Uygulamaları: Çözüm Şablonu veya Yönetilen Uygulama** 

Bir veya daha fazla sanal makine sağlamalı ve sanal makine fiyatlandırma toplamı ile çeker. Tek bir planüzerinde yönetilen uygulamalar için, sanal makine fiyatlandırması yerine fiyatlandırma modeli olarak sabit oranlı aylık abonelik seçilebilir. Bazı durumlarda, Azure altyapı kullanım ücretleri müşteriye yazılım lisans ücretlerinden ayrı olarak, ancak aynı fatura bildiriminde aktarılır. Ancak, ISV altyapı ücretleri için yönetilen bir Uygulama teklifini yapılandırırsanız, Azure kaynakları yayımcıya faturalandırılır ve müşteri altyapı, yazılım lisansları ve yönetim hizmetlerinin maliyetini içeren sabit bir ücret alır.

## <a name="next-steps"></a>Sonraki adımlar

* Teklifinizin seçimini ve yapılandırmasını sonuçlandırmak için teklif türü bölümüne göre yayımlama seçeneklerindeki uygunluk gereksinimlerini gözden geçirin.
* Çözümünüzün teklif türü ve yapılandırmasına nasıl eşlerini oluşturduğuna ilişkin örnekler için yayın desenlerini vitrine göre inceleyin.
