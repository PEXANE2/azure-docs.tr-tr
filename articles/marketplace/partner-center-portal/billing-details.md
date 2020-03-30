---
title: Ticari pazar fatura | Azure Marketi
description: Bu makale, ticari pazar için ticaretle ilgili konuları kapsamaktadır.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/12/2019
ms.openlocfilehash: a0912e1dca63fabfe6bc546305824a1c582b9a2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279751"
---
# <a name="commercial-marketplace-billing"></a>Ticari pazar yeri faturalandırma

Bu makalede, ticari pazar için ticaretle ilgili konular ele ait:

- [Pazar yeri yayımlama seçenekleri](#marketplace-publishing-options)
- [İşlemgenel genel bakış](#transact-general-overview)
- [Transact fatura modelleri](#transact-billing-models)

## <a name="marketplace-publishing-options"></a>Pazar yeri yayımlama seçenekleri

Ticari pazar yayıncılar için çeşitli yayımlama seçenekleri sunar.

### <a name="list-and-trial-publishing-options"></a>Liste ve deneme yayımlama seçenekleri

Yayıncılar, tanıtım ve kullanıcı edinme amacıyla listeyi kullanabilir, deneme yapabilir ve kendi lisans (BYOL) yayımlama seçeneklerinizi getirebilir. Bu seçeneklerle Microsoft, yayımcının yazılım lisans işlemlerine doğrudan katılmaz ve ilişkili işlem ücreti yoktur. Yayıncılar, sipariş, karşılama, ölçüm, faturalama, faturalama, ödeme ve tahsilat dahil ancak bunlarla sınırlı olmamak üzere yazılım lisansı işleminin tüm yönlerini desteklemekten sorumludur. Liste ve deneme yayımlama seçenekleriyle yayıncılar, müşteriden toplanan yayıncı yazılım lisans lama ücretlerinin %100'ünü saklar.

### <a name="transact-publishing-option"></a>İşleme yayımlama seçeneği

Liste ve deneme yayımlama seçeneklerine ek olarak, işlem yayımlama seçeneği yayıncılar tarafından kullanılabilir. Bu seçenek, Microsoft'un genel olarak kullanılabilen ticaret özelliklerinden yararlanır ve Microsoft'un bulut pazarı işlemlerini yayımcı adına barındırmasına olanak tanır.

## <a name="transact-general-overview"></a>İşlemgenel genel bakış

Microsoft, işlem yayımlama seçeneğini kullanırken, üçüncü taraf yazılımlarının satılmasını ve bazı teklif türlerinin müşterinin Azure aboneliğine dağıtılmasını sağlar. Yayıncı, bir faturalandırma modeli ve teklif türü seçerken altyapı ücretlerinin faturalandırmasını ve yayıncının kendi yazılım lisanslama ücretlerini göz önünde bulundurmalıdır.

Transact yayımlama seçeneği şu anda aşağıdaki teklif türleri için desteklenir: sanal makineler, Azure uygulamaları ve SaaS uygulamaları.

![Azure Marketinde İşleme](./media/transact-amp.png)

### <a name="billing-infrastructure-costs"></a>Altyapı maliyetlerini faturalandırma

#### <a name="for-virtual-machines-and-azure-applications"></a>Sanal makineler ve Azure uygulamaları için

Sanal makineler ve Azure uygulamaları için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğine fatura edilir. Altyapı kullanım ücretleri fiyatlandırılır ve müşterinin faturasındaki yazılım sağlayıcısının lisans ücretlerinden ayrı olarak sunulur.

#### <a name="for-saas-apps"></a>SaaS uygulamaları için

SaaS Apps için yayımcının Azure altyapı kullanım ücretlerini ve yazılım lisanslama ücretlerini tek bir maliyet öğesi olarak hesaba katması gerekir. Bu müşteri için sabit bir ücret olarak temsil edilir. Azure altyapı kullanımı yönetilir ve doğrudan iş ortağına faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayıncılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmalarına dahil etmeyi tercih eder. Yazılım lisans ücretleri ölçülü veya tüketime dayalı değildir.

## <a name="transact-billing-models"></a>Transact fatura modelleri

Kullanılan işlem seçeneğine bağlı olarak, yayıncının yazılım lisans ücretleri aşağıdaki gibi sunulabilir:

- *Ücretsiz*: Yazılım lisansları için ücretsizdir.
- *Kendi lisansınızı getirin (BYOL)*: Yazılım lisansları için geçerli olan tüm ücretler doğrudan yayıncı ve müşteri arasında yönetilir. Microsoft yalnızca Azure altyapı kullanım ücretlerinden geçer. (Yalnızca Sanal Makineler ve Azure Uygulamaları.)
- *Kullandıkça Öde*: Yazılım lisans ücretleri, kullanılan Azure altyapısına dayalı olarak saat başına, çekirdek başına (vCPU) fiyatlandırma oranı olarak sunulur. Bu model yalnızca sanal makineler ve Azure uygulamaları için geçerlidir.
- *Abonelik fiyatlandırması*: Yazılım lisans ücretleri aylık veya yıllık olarak sunulur, yinelenen ücret sabit oranlı veya koltuk başına faturalandırılır. Bu model yalnızca SaaS Uygulamaları ve Azure Uygulamaları - Yönetilen Uygulamalar için geçerlidir.
- *Ücretsiz yazılım deneme*: 30 gün veya 90 gün boyunca yazılım lisansları için ücretsizdir.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Ücretsiz ve getir-your-own-lisans (BYOL) fiyatlandırma

Ücretsiz veya kendi lisansını getir işlem teklifi yayımlarken, Microsoft yazılım lisans ücretleriniz için satış işlemini kolaylaştırmada rol oynamaz. Liste ve deneme yayımlama seçenekleri gibi, yayıncı yazılım lisans ücretlerinin %100'ünü tutar.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Gidilen ödeme ve abonelik (site tabanlı) fiyatlandırması

Microsoft, istediğiniz kadar öde veya abonelik işlemi teklifi yayımlarken, yazılım lisansı satın alma işlemlerini, iadeleri ve geri ödemeleri işlemek için gereken teknolojiyi ve hizmetleri sağlar. Bu senaryoda, yayımcı Microsoft'a bu amaçlar için aracı olarak hareket etmesi için yetki verebilmiştir. Yayımcı, Microsoft'un yazılım lisanslama işlemini kolaylaştırmasına izin verirken, satıcı, sağlayıcı, dağıtıcı ve lisansveren olarak atanmasını sağlar.

Microsoft, hem Microsoft'un ticari Pazarı'nın hem de yayıncının son kullanıcı lisans sözleşmesinin hüküm ve koşullarına tabi olarak müşterilerin yayımcı yazılımlarını sipariş etmesini, lisanslanmasını ve kullanmasını sağlar. Yayıncılar, son kullanıcı lisans sözleşmelerini sağlamalı veya teklifi oluştururken [Standart Sözleşme'yi](https://docs.microsoft.com/azure/marketplace/standard-contract) seçmelidir.

### <a name="free-software-trials"></a>Özgür yazılım denemeleri

Yayımlama senaryolarını işleyiş için yayımcı, bir yazılım lisansını 30 gün veya 90 gün boyunca ücretsiz olarak kullanılabilir hale getirebilir. Bu indirim yeteneği, iş ortağı çözümünün kullanımıyla yönlendirilen Azure altyapı kullanım maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Yayıncılar, teklif türlerinden ve faturalandırma modellerinden para kazanmak için kullanmanın yanı sıra, anlaşmalı ve anlaşmaya özgü fiyatlandırma veya özel yapılandırmalarla birlikte özel bir teklif sunabilir. Özel teklifler, 3 işlem yayımlama seçeneğinin tümü tarafından desteklenir.

Bu seçenek, herkese açık tekliften daha yüksek veya daha düşük fiyatlandırma sağlar. Özel teklifler indirim yapmak veya bir teklif için prim eklemek için kullanılabilir. Azure aboneliklerini teklif düzeyinde beyaz listeleyerek bir veya daha fazla müşterinin özel teklifleri kullanılabilir.

### <a name="examples"></a>Örnekler

#### <a name="pay-as-you-go"></a>Kullandıkça öde

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

### <a name="bring-your-own-license-byol"></a>Kendi Lisansını Getir (BYOL)

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

### <a name="saas-app-subscription"></a>SaaS Uygulama aboneliği

Bu seçenek Microsoft üzerinden satış yapacak şekilde yapılandırılmalıdır ve aylık veya yıllık bazda sabit bir oranda veya kullanıcı başına fiyatlandırılabilir.

• SaaS teklifi için Microsoft üzerinden Sat seçeneğini etkinleştiriseniz, aşağıdaki maliyet yapısına sahip olursunuz.

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

**İndirimli Pazar Yeri Hizmet Ücreti:** Ticari Pazarımızda yayınladığınız belirli SaaS Ürünleri için Microsoft, Pazar Yeri Hizmet Ücretini %20'den (Microsoft Publisher Anlaşması'nda açıklandığı gibi) %10'a düşürecektir.  Ürününüzün hak kazanabilmesi için, ürünlerinizden en az birinin Microsoft tarafından IP ortak satış hazır veya IP ortak satışı öncelikli olarak belirtilmesi gerekir. Bu indirimli Market Hizmet Ücretini aya alabilmek için, uygunluğun bir önceki takvim ayının bitiminden en az beş (5) iş günü önce karşılanması gerekir. İndirimli Pazar Yeri Hizmeti ücreti, Ticari Pazar Yeri miz aracılığıyla sunulan VM'ler, Yönetilen Uygulamalar veya diğer ürünler için geçerli olmayacaktır.  Bu İndirimli Pazar Yeri Hizmet Ücreti, Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 tarihleri arasında tahsil edilen lisans ücretleriyle nitelikli tekliflere sunulacaktır.  Bu süre sonunda, Market Servis Ücreti normal tutarına geri dönecektir.
