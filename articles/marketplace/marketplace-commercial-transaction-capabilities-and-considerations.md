---
title: Microsoft ticari Market Transact özellikleri
description: Bu makalede ticari Market Transact seçeneğine yönelik fiyatlandırma, faturalandırma, faturalandırma ve ödeme konuları açıklanmaktadır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: mingshen
author: mingshen-ms
ms.openlocfilehash: 04a984a2dfa473502fd9e534e52b60b33be52757
ms.sourcegitcommit: e0785ea4f2926f944ff4d65a96cee05b6dcdb792
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88704969"
---
# <a name="commercial-marketplace-transact-capabilities"></a>Ticari Market Transact özellikleri

Bu makalede, Microsoft ticari Market 'e yönelik fiyatlandırma, faturalandırma, faturalandırma ve ödeme konuları açıklanmaktadır. 

## <a name="transactions-by-listing-option"></a>Listeleme seçeneğine göre işlemler

Yayımcı veya Microsoft, ticari Market 'teki teklifler için yazılım lisansı işlemlerini yönetmekten sorumludur. Teklifiniz için seçtiğiniz listeleme seçeneği, işlemi kimin yönettiğini belirler. Her yayımlama seçeneğinin kullanılabilirliği ve açıklamaları için bkz. [liste seçme seçeneği](./determine-your-listing-type.md#choose-a-listing-option) .

### <a name="contact-me-free-trial-and-byol-options"></a>Benimle iletişim kurun, ücretsiz deneme ve KLG seçenekleri

Yayımcılar _benimle Iletişim kurun_ ve _ücretsiz deneme sürümü_, promosyon ve Kullanıcı alma amaçları için seçenekler arasından seçim yapabilir. Bazı teklif türlerinde, yayımcılar, müşterilerin doğrudan sizin satın aldıkları bir lisansı kullanarak teklifinizi bir abonelik satın almasını sağlamak için kendi lisansını getir (KLG) seçeneğini seçebilirler. Bu seçeneklerle, Microsoft doğrudan yayımcının yazılım lisansı işlemlerine katılmaz ve ilişkili işlem ücreti yoktur. 

Yayımcılar, yazılım lisans işleminin tüm yönlerini desteklemekten sorumludur. Bu, sipariş, karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon ile sınırlı değildir ancak bunlarla sınırlı değildir. Ilgili kişi listeleme seçeneğiyle, yayımcılar, müşteriden toplanan yayımcı yazılım lisansı ücretleri %100 ' ı tutar.

### <a name="transact-publishing-option"></a>Transact yayımlama seçeneği

Microsoft aracılığıyla satışı yapmak, Microsoft ticaret olanaklarından yararlanır ve bulma ve değerlendirme ile satın alma ve uygulamaya kadar uçtan uca bir deneyim sunar. Transactable bir teklif, Microsoft 'un yayımcı adına bir yazılım lisansı için para değişimini kolaylaştıran bir tekliftir. Transactable teklifler mevcut bir Microsoft aboneliğine veya kredi kartına göre faturalandırılır ve Microsoft 'un yayımcı adına bulut marketi işlemlerini barındırmalarına olanak tanır.

Iş Ortağı Merkezi 'nde yeni bir teklif oluşturduğunuzda Transact seçeneğini belirleyin. Bu seçenek yalnızca, teklif türü için Transact kullanılabiliyorsa gösterilir.

## <a name="transact-overview"></a>Transact genel bakış

Microsoft, Transact seçeneğini kullanırken, üçüncü taraf yazılımlarının ve bazı teklif türlerinin müşterinin Azure aboneliğine dağıtılması için satışı sağlar. Bir teklifin fiyatlandırma modelini seçerken, Yayımcı, altyapı ücretleri ve kendi yazılım lisans ücretlerinizi faturalandırmayı düşünmelidir.

Şu anda aşağıdaki teklif türleri için Transact yayımlama seçeneği desteklenmektedir:

- Sanal makineler
- Azure uygulamaları
- SaaS uygulamaları

### <a name="billing-infrastructure-costs"></a>Faturalandırma altyapısı maliyetleri

**Sanal makineler** ve **Azure uygulamaları**için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğine faturalandırılır. Altyapı kullanım ücretleri fiyatlandırılır ve müşterinin faturasında yazılım sağlayıcısının lisanslama ücretlerinden ayrı olarak sunulur.

**SaaS uygulamaları**için, yayımcının tek bir maliyet öğesi olarak Azure altyapı kullanım ücretlerini ve yazılım lisanslama ücretlerini hesaba göre hesabı olması gerekir.  Müşteriye yönelik bir sabit ücret olarak gösterilir. Azure altyapı kullanımı yönetilir ve doğrudan yayımcıya faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlendirmeyi tercih edebilir. Yazılım Lisanslama ücretleri tarifeli değildir veya Kullanıcı tüketimine göre hesaplanır.

## <a name="pricing-models"></a>Fiyatlandırma modelleri

Kullanılan işlem seçeneğine bağlı olarak, abonelik ücretleri aşağıdaki gibidir:

- Artık yazılım lisansları için ücret ödemezsiniz **(ücretsiz)** . Müşteriler ücretsiz bir teklif kullanmak için Azure Marketi ücretlerine ücretlendirilmez. Ücretsiz teklifler, ücretli bir teklifine dönüştürülemez. Müşterilerin ücretli bir teklif siparişi olması gerekir.
- **Kendi lisansınızı getirin** (KLG) – yazılım lisansları için geçerli ücretler, doğrudan yayımcı ile müşteri arasında yönetilir. Microsoft yalnızca Azure altyapı kullanım ücretleri üzerinden geçer. Ticari Market 'te bir teklif listeleniyorsa, teklifi ticari Market dışında bir şekilde alan veya teklif kullanan müşteriler, ticari Market ücretleri ücretlendirilmez.
- **Abonelik fiyatlandırması** – yazılım lisans ücretleri aylık veya yıllık olarak sunulur, yinelenen abonelik ücreti de sabit bir fiyat veya bilgisayar başına faturalandırılır. Yinelenen abonelik ücretleri, orta dönemli müşteri iptalleri veya kullanılmayan hizmetler için eşit olarak dağıtılır. Müşteri aboneliğini abonelik döneminin ortasında yükseltir veya daha eski sürüme düşürütirse, yinelenen abonelik ücretleri eşit olarak dağıtılır.
- **Kullanım tabanlı fiyatlandırma** – Azure sanal makine teklifleri için, müşteriler teklifin kullanımının kapsamına göre ücretlendirilir. Sanal makine görüntüleri için, müşteriler VM görüntülerinden dağıtılan sanal makinelerin kullanımı için yayımcılar tarafından belirlenen saatlik bir Azure Marketi ücretini ücretlendirirsiniz. Saatlik ücret, sanal makine boyutları arasında Tekdüzen veya değişken olabilir. Kısmi saatler dakikaya göre ücretlendirilir. Planlar aylık olarak faturalandırılır.
- **Ölçülen fiyatlandırma** : Azure Uygulama teklifleri ve SaaS teklifleri için yayımcılar, tercih ettikleri ölçüm boyutlarına göre tüketim için [Market ölçüm hizmetini](./partner-center-portal/marketplace-metering-service-apis.md) kullanabilir. Örneğin, bant genişliği, Bilet veya işlenen e-postalar. Yayımcılar, her plan için bir veya daha fazla ölçüm boyutu tanımlayabilir. Yayımcılar, her bir ölçümde teklifte tanımlanan her ölçüm ile müşterilerin kullanımını izlemekten sorumludur. Olayların bir saat içinde Microsoft 'a bildirilmesi gerekir. Microsoft, müşterileri, uygun fatura dönemi için yayımcılar tarafından bildirilen kullanım bilgilerine göre ücretlendirir.
- **Ücretsiz deneme** : teklif türüne bağlı olarak 30 günden fazla altı aya kadar olan yazılım lisansları için ücret ödemezsiniz. Yayımcılar aynı teklif dahilinde birden çok planda ücretsiz bir deneme sağladıysanız, müşteriler başka bir plandaki ücretsiz bir denemeye geçebilir, ancak deneme süresi yeniden başlatmaz. Sanal makine teklifleri için müşteriler, deneme süresi boyunca teklifin kullanımı için Azure altyapı maliyetlerinden ücretlendirilir. Deneme süresi sona erdiğinde, müşteriler deneme süresi sona ermeden önce iptal etmedikleri takdirde standart oranlar temelinde denedikleri son plana göre otomatik olarak ücretlendirilir.

> [!NOTE]
> Bir çözüm kullanıldıktan sonra tüketimine göre faturalandırılan teklifler, para iadesi için uygun değildir.

Bir teklifle ilişkili kullanım ücretlerini değiştirmek isteyen yayımcılar, öncelikle teklifi (veya teklif içindeki belirli bir planı) ticari marketten kaldırmalıdır. Kaldırma, [Microsoft Publisher sözleşmesinin](https://go.microsoft.com/fwlink/?LinkID=699560)gereksinimlerine uygun olarak yapılmalıdır. Ardından yayımcı yeni kullanım ücretlerini içeren yeni bir teklif (veya bir teklif içinde plan) yayımlayabilir. Teklif veya planı kaldırma hakkında bilgi için bkz. [bir teklif veya plan satma](./partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan).

### <a name="free-contact-me-and-bring-your-own-license-byol-pricing"></a>Ücretsiz, benimle Iletişim kurun ve kendi lisansını getir (KLG) fiyatlandırması

Şimdi al (ücretsiz), benimle Iletişim kurun veya KLG seçeneğiyle bir teklif yayımlarken, Microsoft, yazılım lisans ücretlerine yönelik satış işlemini kolaylaştırmaya yönelik bir rol oynamaz. Yayımcı, liste ve ücretsiz deneme yayımlama seçenekleri gibi yazılım lisansı ücretlerine %100 oranında devam eder.

### <a name="usage-based-and-subscription-pricing"></a>Kullanım tabanlı ve abonelik fiyatlandırması

Bir teklif, Kullanıcı tabanlı veya abonelik işlemi yayımlarken, Microsoft yazılım lisansı satın alma, iade ve geri ödeme işlemlerini işleyecek teknoloji ve hizmetler sağlar. Bu senaryoda, yayımcı Microsoft 'un bu amaçlar için bir aracı görevi görmesini yetkilendirir. Yayımcı, Microsoft 'un yazılım lisanslama işlemini kolaylaştırmasına olanak sağlarken satıcı, sağlayıcı, dağıtıcı ve lisans verme özelliklerini korur.

Microsoft, müşterilerin, Microsoft 'un ticari marketi ve Son Kullanıcı lisanslama sözleşmenizin hüküm ve koşullarına tabi olmak üzere yazılımınızı sipariş, lisans ve kullanma imkanı sağlar. Kendi Son Kullanıcı lisanslama sözleşmenizi sağlamanız veya teklifi oluştururken [Standart sözleşmeyi](./standard-contract.md) seçmeniz gerekir.

### <a name="free-software-trials"></a>Ücretsiz yazılım denemeleri

Transact yayımlama senaryolarında, aboneliğe bağlı olarak, yazılım lisansını 30 ila 120 gün boyunca ücretsiz kullanılabilir hale getirebilirsiniz. Bu ayırma özelliği, iş ortağı çözümü kullanılarak yönetilen Azure altyapı kullanımının maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Teklif türlerini ve faturalandırma modellerini bir teklifi ayırmak için kullanmanın yanı sıra, anlaşmalı, anlaşmaya özgü fiyatlandırma veya özel yapılandırmalarda özel bir teklif elde edebilirsiniz. Özel teklifler, her üç Transact yayımlama seçeneği tarafından desteklenir.

Bu seçenek, genel kullanıma sunulan teklife göre daha yüksek veya daha düşük fiyatlandırmayı sağlar. Özel teklifler, bir teklifle bir Premium için indirim veya bir Premium eklemek için kullanılabilir. Özel teklifler, bir veya daha fazla müşteriye, Azure aboneliklerini teklif düzeyinde listeleyerek beyaz listeye alınabilir.

### <a name="examples"></a>Örnekler

**Kullanım tabanlı** 

Kullanım tabanlı fiyatlandırma aşağıdaki maliyet yapısına sahiptir:

|Lisans maliyetiniz  | $1,00/saat   |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   $0,14/saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$1,14/saat*       |
||

Bu senaryoda, Microsoft, yayımlanmış VM Görüntünüzün kullanımı için saat başına $1,14.

|Microsoft faturaları  | $1,14/saat  |
|---------|---------|
|Microsoft, lisans maliyetinizi %80 oranında ödetir|   $0,80/saat     |
|Microsoft, lisans maliyetlerinizin %20 ' sini tutar  |  $0,20/saat       |
|Microsoft, Azure kullanım maliyetinin %100 ' ünü tutar | $0,14/saat |
||

**Kendi Lisansını Getir (KLG)**

KLG aşağıdaki maliyet yapısına sahiptir:

|Lisans maliyetiniz  | Lisans ücreti size anlaştı ve sizin tarafınızdan faturalandırılır  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   $0,14/saat     |
|*Müşteri Microsoft tarafından faturalandırılır*    |  *$0,14/saat*       |
||

Bu senaryoda, Microsoft, yayımlanmış VM Görüntünüzün kullanımı için saat başına $0,14.

|Microsoft faturaları  | $0,14/saat  |
|---------|---------|
|Microsoft, Azure kullanım maliyetini korur    |   $0,14/saat     |
|Microsoft, lisans maliyetinizi %0 oranında tutar   |  $0,00/saat       |
||

**SaaS uygulama aboneliği**

Bu seçenek, Microsoft üzerinden satış için yapılandırılmış olmalıdır ve aylık veya yıllık olarak, sabit bir hızda veya Kullanıcı başına fiyatlandırılabilecek. SaaS teklifi için **Microsoft 'Tan satışı** etkinleştir seçeneğini etkinleştirirseniz, aşağıdaki maliyet yapısına sahipsiniz:

| Lisans maliyetiniz       | $100,00/ay  |
|--------------|---------|
| Azure kullanım maliyeti (D1/1-çekirdek)    | Müşteriye değil doğrudan yayımcıya faturalandırılır |
| *Müşteri Microsoft tarafından faturalandırılır*    |  *aylık $100,00 (yayımcı, lisans ücretindeki herhangi bir tahakkuk eden veya geçiş altyapı maliyeti için hesap olmalıdır)*  |
||

Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $80,00 ' i ödeder.

Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $90,00 ' ye ödeme yapar:

|Microsoft faturaları  | $100,00/ay  |
|---------|---------|
|Microsoft, lisans maliyetinizi %80 oranında ödetir <br> \* Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %90 oranında ödetir   |   $80,00/ay <br> \* $90,00/ay    |
|Microsoft, lisans maliyetlerinizin %20 ' sini tutar <br> \* Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %10 oranında tutar.  |  $20,00/ay <br> \* $10,00     |

Microsoft, ticari Market 'te yayımladığınız bazı tekliflerle ilgili Market hizmet ücretini %20 ' den ( [Microsoft Publisher anlaşmasında](https://go.microsoft.com/fwlink/?LinkID=699560)açıklandığı gibi) %10 oranında azaltacak. Tekliflerinizin uygun olmasını sağlamak için teklifinizin Microsoft tarafından Azure IP ortak satışı incentivized olarak belirlenmiş olması gerekir. Uygunluk, ay için en az Market servis ücretini almak üzere her bir takvim ayının sonundan önce en az beş iş gününe ulaşılmalıdır. Daha düşük Market hizmet ücreti, Azure IP ortak satış incentivized SaaS, VM 'Ler, yönetilen uygulamalar ve ticari Market aracılığıyla sunulan diğer tüm nitelikli transactable IaaS teklifleri için geçerlidir.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Müşteri Faturalama, ödeme, faturalandırma ve koleksiyonlar

**Faturalama ve ödeme** – abonelik veya PAYGO yazılım lisansı ücretleri sunmak için müşterinin tercih edilen faturalandırma yöntemini kullanabilirsiniz.

**Kurumsal Anlaşma** – müşterinin tercih edilen faturalandırma yöntemi Microsoft kurumsal anlaşma ise, yazılım lisans ücretleriniz, Azure 'a özgü kullanım maliyetlerinden ayrı olarak, bu faturalandırma yöntemi kullanılarak, bir maliyet dökümü olarak faturalandırılacaktır.

**Kredi kartları ve aylık fatura** – müşteriler, kredi kartı ve aylık fatura kullanarak da ödeme yapabilir. Bu durumda, yazılım lisans ücretleriniz, bir Azure 'a özgü kullanım maliyetlerinden ayrı olarak, Kurumsal Anlaşma senaryosu gibi, yeni bir maliyet olarak faturalandırılacaktır.

**Ücretsiz krediler ve parasal taahhüt** : bazı müşteriler azure 'u kurumsal anlaşma bir parasal taahhütte ön ödeme yapmayı tercih ediyor veya Azure ile kullanım için ücretsiz krediler sağladı. Bu krediler Azure kullanımı için ödeme yapmak üzere kullanılabilse de, yayımcı yazılımı lisans ücretleri için ödeme yapmak üzere kullanılamaz.

**Faturalandırma ve koleksiyonlar** – yayımcı yazılım lisansı faturalandırması, faturalandırılmasına ve faturalama zaman çizelgesine göre belirtilen müşteri tarafından seçilen yöntemi kullanılarak sunulur. Yerinde Kurumsal Anlaşma olmayan müşteriler Market yazılım lisansları için aylık olarak faturalandırılır. Kurumsal Anlaşma olan müşteriler aylık olarak sunulan bir fatura aracılığıyla aylık olarak faturalandırılır.

Abonelik veya kullandıkça öde fiyatlandırma modelleri seçildiğinde, Microsoft, yayımcının aracı olarak davranır ve faturalandırma, ödeme ve koleksiyonun tüm yönleriyle sorumludur.

### <a name="publisher-payout-and-reporting"></a>Yayımcı ödeme ve raporlama

Aracı olarak Microsoft tarafından toplanan tüm yazılım lisanslama ücretleri, aksi belirtilmediği ve yayımcı ödeme sırasında kesilmedikleri sürece %20 işlem ücretine tabidir.

Müşteriler genellikle Kurumsal Anlaşma veya kredi kartı etkin bir Kullandıkça Öde sözleşmesini kullanarak satın alabilir. Anlaşma türü faturalandırma, faturalandırma, toplama ve ödeme zamanlamasını belirler.

>[!NOTE]
>Transact yayımlama seçeneğine yönelik tüm raporlama ve Öngörüler, Iş Ortağı Merkezi 'nin analiz bölümü aracılığıyla kullanılabilir.

#### <a name="billing-questions-and-support"></a>Faturalandırma soruları ve destek

Daha fazla bilgi ve yasal ilkeler için bkz. [Microsoft Publisher sözleşmesi](https://go.microsoft.com/fwlink/?LinkID=699560) (Iş Ortağı Merkezi 'nde kullanılabilir).

Faturalandırma soruları hakkında yardım almak için [ticari Market yayımcı desteği](https://aka.ms/marketplacepublishersupport)'ne başvurun.

## <a name="transact-requirements"></a>Transact gereksinimleri

Bu bölümde, farklı teklif türleri için Transact gereksinimleri ele alınmaktadır.

### <a name="requirements-for-all-offer-types"></a>Tüm teklif türleri için gereksinimler

- Teklifin fiyatlandırma modelinden bağımsız olarak, Transact yayımlama seçeneği için Microsoft hesabı ve finansal bilgiler gereklidir.
- Zorunlu mali bilgiler, ödeme hesabı ve vergi profilini içerir.

Bu hesapları ayarlama hakkında daha fazla bilgi için bkz. [Iş Ortağı Merkezi 'nde ticari Market hesabınızı yönetme](partner-center-portal/manage-account.md).

### <a name="requirements-for-specific-offer-types"></a>Belirli teklif türleri için gereksinimler

Transact yayımlama seçeneği yalnızca aşağıdaki Market teklif türleriyle kullanılabilir:

- **Azure sanal makinesi** : ücretsiz, kendi lisansını getir veya kullanım tabanlı fiyatlandırma modelleri arasından seçim yapın ve teklif düzeyinde tanımlanan planlar olarak sunun. Microsoft, müşterinin Azure faturasında, yayımcı yazılımı lisans ücretlerini temel alınan Azure altyapı ücretlerinden ayrı olarak sunar. Azure altyapı ücretleri, yayımcı yazılımı kullanılarak dağıtılır.

- **Azure uygulaması: çözüm şablonu veya yönetilen uygulama** – bir veya daha fazla sanal makine sağlamalıdır ve sanal makine fiyatlandırmasının toplamını almalıdır. Tek bir planda yönetilen uygulamalar için, sanal makine fiyatlandırması yerine fiyatlandırma modeli olarak bir sabit hızlı aylık abonelik seçilebilir. Bazı durumlarda, Azure altyapı kullanım ücretleri müşteriye yazılım lisans ücretlerinden ayrı olarak geçirilir, ancak aynı faturalandırma beyanının üzerinden yapılır. Ancak, ISV altyapı ücretleri için yönetilen bir uygulama teklifi yapılandırırsanız Azure kaynakları yayımcıya faturalandırılır ve müşteri altyapı, yazılım lisansı ve yönetim hizmetleri maliyetini içeren bir sabit ücret alır.

- **SaaS uygulaması** -çok kiracılı bir çözüm olmalıdır, kimlik doğrulaması için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kullanın ve [SaaS karşılama API 'leriyle](partner-center-portal/pc-saas-fulfillment-api-v2.md)tümleştirin. Azure altyapı kullanımı, doğrudan sizin kullanımınıza (iş ortağı) göre yönetilir ve faturalandırılır. bu nedenle, Azure altyapı kullanım ücretleri ve yazılım lisanslama ücretlerini tek bir maliyet öğesi olarak hesaba almalısınız. Ayrıntılı kılavuz için bkz. [ticari Market 'te yeni SaaS teklifi oluşturma](partner-center-portal/create-new-saas-offer.md).

## <a name="next-steps"></a>Sonraki adımlar

- Teklifinizin seçimini ve yapılandırmasını tamamlamak için teklif türüne göre yayımlama seçeneklerinde uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün bir teklif türü ve yapılandırmaya nasıl eşlendiğini gösteren örnekler için çevrimiçi mağazaya göre yayımlama düzenlerini gözden geçirin.
