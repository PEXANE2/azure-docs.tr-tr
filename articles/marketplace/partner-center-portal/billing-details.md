---
title: Azure Marketi 'nde ticari Market faturalandırması
description: Market Yayımlama seçenekleri ve ticari Market için Transact faturalandırma modelleri hakkında bilgi edinin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/12/2019
ms.openlocfilehash: deb4f3f1bb17bff0b09b2e4f79ceb967d4d7ff59
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744965"
---
# <a name="commercial-marketplace-billing-with-transact-billing-models"></a>Transact faturalandırma modelleriyle ticari Market faturalandırması

Bu makalede ticari Market 'e yönelik ticaret ile ilgili konular ele alınmaktadır:

- [Market Yayımlama seçenekleri](#marketplace-publishing-options)
- [Transact genel bakış](#transact-general-overview)
- [Transact faturalandırma modelleri](#transact-billing-models)

## <a name="marketplace-publishing-options"></a>Market Yayımlama seçenekleri

Ticari Market, yayımcılar için birkaç yayımlama seçeneği sunar.

### <a name="list-and-trial-publishing-options"></a>Liste ve deneme yayımlama seçenekleri

Yayımcılar, promosyon ve Kullanıcı alma amaçları için liste, deneme ve kendi lisansını getir (KLG) yayımlama seçeneklerinden yararlanabilir. Bu seçeneklerle, Microsoft doğrudan yayımcının yazılım lisansı işlemlerine katılmaz ve ilişkili işlem ücreti yoktur. Yayımcılar, sipariş, karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon dahil olmak üzere, yazılım lisans işleminin tüm yönlerini desteklemekten sorumludur. Liste ve deneme yayımlama seçenekleriyle, yayımcılar, müşteriden toplanan yayımcı yazılım lisansı ücretleri %100 ' i tutar.

### <a name="transact-publishing-option"></a>Transact yayımlama seçeneği

Liste ve deneme yayımlama seçeneklerine ek olarak, Transact yayımlama seçeneği yayımcılar tarafından kullanılabilir. Bu seçenek Microsoft 'un küresel olarak kullanılabilir ticari olanaklarından yararlanır ve Microsoft 'un bulut Market işlemlerini yayımcı adına barındırmasına imkan tanır.

## <a name="transact-general-overview"></a>Transact genel bakış

Transact yayımlama seçeneği kullanılırken, Microsoft üçüncü taraf yazılımlarının satışı ve bazı teklif türlerinin müşterinin Azure aboneliğine dağıtılması için bir satış sağlar. Yayımcı, bir faturalandırma modeli ve teklif türü seçerken, altyapı ücretlerinin ve yayımcının kendi yazılım lisanslama ücretlerine ilişkin faturalandırmayı göz önünde bulundurmalıdır.

Aşağıdaki teklif türleri için Transact Publishing seçeneği şu anda destekleniyor: sanal makineler, Azure uygulamaları ve SaaS uygulamaları.

![Azure Marketi 'nde Transact](./media/transact-amp.png)

### <a name="billing-infrastructure-costs"></a>Faturalandırma altyapısı maliyetleri

#### <a name="for-virtual-machines-and-azure-applications"></a>Sanal makineler ve Azure uygulamaları için

Sanal makineler ve Azure uygulamaları için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğine faturalandırılır. Altyapı kullanım ücretleri fiyatlandırılır ve müşterinin faturasında yazılım sağlayıcısının lisanslama ücretlerinden ayrı olarak sunulur.

#### <a name="for-saas-apps"></a>SaaS uygulamaları için

SaaS uygulamaları için yayımcı, Azure altyapı kullanım ücretlerini ve yazılım lisanslama ücretlerini tek bir maliyet öğesi olarak hesaba malıdır. Müşteriye yönelik bir sabit ücret olarak gösterilir. Azure altyapı kullanımı, doğrudan iş ortağı tarafından yönetilir ve faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlendirmeyi tercih edebilir. Yazılım Lisanslama ücretleri tarifeli veya tüketim temelli değildir.

## <a name="transact-billing-models"></a>Transact faturalandırma modelleri

Kullanılan işlem seçeneğine bağlı olarak, yayımcının yazılım lisansı ücretleri aşağıdaki gibi gösterilebilir:

- *Ücretsiz*: yazılım lisansları için ücret alınmaz.
- *Kendi lisansınızı getirin (KLG)*: yazılım lisansları için geçerli ücretler, doğrudan yayımcı ile müşteri arasında yönetilir. Microsoft yalnızca Azure altyapı kullanım ücretleri üzerinden geçer. (Yalnızca sanal makineler ve Azure uygulamaları.)
- *Kullandıkça Öde*: yazılım lisansı ücretleri, kullanılan Azure altyapısına bağlı olarak saat başına, çekirdek başına (vCPU) fiyatlandırma ücreti olarak sunulur. Bu model yalnızca sanal makineler ve Azure uygulamaları için geçerlidir.
- *Abonelik fiyatlandırması*: yazılım lisansı ücretleri aylık veya yıllık olarak, sabit bir fiyat veya bilgisayar başına faturalandırılan yinelenen ücret olarak sunulur. Bu model yalnızca SaaS uygulamaları ve Azure uygulamaları tarafından yönetilen uygulamalar için geçerlidir.
- *Ücretsiz yazılım deneme sürümü*: 30 günlük veya 90 gün için yazılım lisansları için ücret alınmaz.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Ücretsiz ve kendi lisansını getir (KLG) fiyatlandırması

Ücretsiz veya kendi lisans işlem teklifini yayımlarken, Microsoft, yazılım lisans ücretlerinizin satış işlemini kolaylaştırmaya yönelik bir rol oynamaz. Yayımcı, liste ve deneme yayımlama seçenekleri gibi yazılım lisansı ücretlerine %100 oranında devam eder.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Kullandıkça öde ve abonelik (site tabanlı) fiyatlandırması

Kullandıkça Öde veya abonelik işlemi teklifi yayımlandığında, Microsoft yazılım lisansı satın alma, iade ve geri ödeme işlemlerini işleyecek teknoloji ve hizmetler sağlar. Bu senaryoda, yayımcı Microsoft 'un bu amaçlar için bir aracı görevi görmesini yetkilendirir. Yayımcı, Microsoft 'un yazılım lisanslama işlemini kolaylaştırmasına olanak sağlarken satıcı, sağlayıcı, dağıtıcı ve lisans verme özelliklerini korur.

Microsoft, müşterilerin, Microsoft 'un ticari marketi ve yayımcısının Son Kullanıcı lisanslama sözleşmesinin hüküm ve koşullarına subjecting, yayımcı yazılımını sipariş etmesini, lisanslamasını ve kullanmasına olanak sağlar. Yayımcılar, Son Kullanıcı lisanslama anlaşmasını sağlamalıdır veya teklifi oluştururken [Standart sözleşmeyi](https://docs.microsoft.com/azure/marketplace/standard-contract) seçmelidir.

### <a name="free-software-trials"></a>Ücretsiz yazılım denemeleri

Transact yayımlama senaryolarında, Yayımcı, yazılım lisansını 30 gün veya 90 gün boyunca ücretsiz kullanılabilir hale getirir. Bu ayırma özelliği, iş ortağı çözümü kullanılarak yönlendirilen Azure altyapı kullanımının maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Bir teklifi ayırmak için teklif türlerini ve faturalandırma modellerini kullanmanın yanı sıra, yayımcılar özel bir teklif elde edebilir, anlaşmalı ve anlaşmaya özgü fiyatlandırmayla veya özel yapılandırmalara sahip olabilir. Özel teklifler, tüm 3 Transact yayımlama seçenekleri tarafından desteklenir.

Bu seçenek, genel kullanıma sunulan teklife göre daha yüksek veya daha düşük fiyatlandırmayı sağlar. Özel teklifler, bir teklif için bir Premium için indirim veya bir Premium eklemek üzere kullanılabilir. Özel teklifler, Azure aboneliklerini teklif düzeyinde listeleyerek beyaz bir veya daha fazla müşterinin kullanımına sunulabilir.

### <a name="examples"></a>Örnekler

#### <a name="pay-as-you-go"></a>Kullandıkça öde

* Kullandıkça Öde seçeneğini etkinleştirirseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz  | $1,00/saat  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   $0,14/saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$1,14/saat*       |

* Bu senaryoda, Microsoft, yayımlanmış VM Görüntünüzün kullanımı için saat başına $1,14.

|Microsoft faturaları  | $1,14/saat  |
|---------|---------|
|Microsoft, lisans maliyetinizi %80 oranında ödetir|   $0,80/saat     |
|Microsoft, lisans maliyetlerinizin %20 ' sini tutar  |  $0,20/saat       |
|Microsoft, Azure kullanım maliyetinin %100 ' ünü tutar | $0,14/saat |

### <a name="bring-your-own-license-byol"></a>Kendi lisansınızı getirin (KLG)

* KLG seçeneğini etkinleştirirseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz  | Lisans ücreti size anlaştı ve sizin tarafınızdan faturalandırılır  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   $0,14/saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$0,14/saat*       |

* Bu senaryoda, Microsoft, yayımlanmış VM Görüntünüzün kullanımı için saat başına $0,14.

|Microsoft faturaları  | $0,14/saat  |
|---------|---------|
|Microsoft, Azure kullanım maliyetini korur    |   $0,14/saat     |
|Microsoft, lisans maliyetinizi %0 oranında tutar   |  $0,00/saat       |

### <a name="saas-app-subscription"></a>SaaS uygulama aboneliği

Bu seçenek, Microsoft üzerinden satış için yapılandırılmış olmalıdır ve aylık veya yıllık olarak, sabit bir hızda veya Kullanıcı başına fiyatlandırılabilecek.

• SaaS teklifi için Microsoft 'tan satışı etkinleştir seçeneğini etkinleştirirseniz, aşağıdaki maliyet yapısına sahip olursunuz.

|Lisans maliyetiniz       | $100,00/ay  |
|--------------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    | Müşteriye değil doğrudan yayımcıya faturalandırılır |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *ayda $100,00 (Note: Yayımcı, lisans ücretindeki herhangi bir tahakkuk eden veya doğrudan altyapı maliyeti için hesaba başvurmalıdır)*  |

* Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $80,00 ' i ödeder.
* Azaltılmış Market hizmet ücreti için uygun olan iş ortakları, 2019 2020 Haziran 'a kadar Mayıs 'a kadar olan SaaS teklifleriyle daha düşük bir işlem ücreti görür. Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $90,00 ' i ödeder.

|Microsoft faturaları  | $100,00/ay  |
|---------|---------|
|Microsoft, lisans maliyetinizi %80 oranında ödetir <br> \*Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %90 oranında ödetir   |   $80,00/ay <br> \*$90,00/ay    |
|Microsoft, lisans maliyetlerinizin %20 ' sini tutar <br> \*Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %10 oranında tutar.  |  $20,00/ay <br> \*$10,00     |

**Daha az Market hizmeti ücreti:** Ticari marketi 'nde yayımladığınız belirli SaaS ürünleri için Microsoft, Market hizmet ücretini %20 ' den (Microsoft Publisher anlaşmasında açıklandığı gibi) %10 oranında azaltacak.  Ürününüzün uygun olabilmesi için, ürünlerinizin en az birinin, Microsoft tarafından IP ortak satışı için ayrılan veya IP ortak satışı için tasarlanmış olması gerekir. Bu ay için daha düşük olan bu Market hizmeti ücretini almak için, uygunluk önceki takvim ayının sonundan önce en az beş (5) iş gününe ulaşılmalıdır. Daha az Market hizmeti ücreti, sanal makineler, yönetilen uygulamalar veya ticari Market 'ten sunulan diğer ürünlerden hiçbiri için uygulanmaz.  Bu azaltılan Market hizmet ücreti, Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 arasında toplanan lisans ücretleri sayesinde nitelikli tekliflerle kullanılabilir.  Bu süreden sonra Market hizmeti ücreti normal miktarına geri döner.
