---
title: Microsoft ticari Market Transact özellikleri
description: Bu makalede ticari Market Transact seçeneğine yönelik fiyatlandırma, faturalandırma, faturalandırma ve ödeme konuları açıklanmaktadır.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: 653c55fa7476fa5fed077002db226297a33dfef6
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119438"
---
# <a name="commercial-marketplace-transact-capabilities"></a>Ticari Market Transact özellikleri

## <a name="transactions-by-publishing-option"></a>Yayımlama seçeneğine göre işlemler

Yayımcı veya Microsoft, ticari Market 'teki teklifler için yazılım lisansı işlemlerini yönetmekten sorumludur. Teklifiniz için seçtiğiniz yayımlama seçeneği, işlemi kimin yönettiğini belirleyeceğini tespit eder. Her yayımlama seçeneğinin kullanılabilirliği ve açıklamaları için [Yayımlama seçeneğinizi belirleme](./determine-your-listing-type.md#choose-a-publishing-option) konusuna bakın.

### <a name="list-trial-and-byol-publishing-options"></a>List, deneme ve KLG yayımlama seçenekleri

Mevcut ticaret özelliklerine sahip yayımcılar, promosyon ve Kullanıcı alma amaçları için liste, deneme ve kendi lisansını getir (KLG) yayımlama seçeneklerini seçebilirler. Bu seçeneklerle, Microsoft doğrudan yayımcının yazılım lisansı işlemlerine katılmaz ve ilişkili işlem ücreti yoktur. Yayımcılar, sipariş, karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon dahil olmak üzere, yazılım lisans işleminin tüm yönlerini desteklemekten sorumludur. Liste ve deneme yayımlama seçenekleriyle, yayımcılar, müşteriden toplanan yayımcı yazılım lisansı ücretleri %100 ' i tutar.

### <a name="transact-publishing-option"></a>Transact yayımlama seçeneği

Transact yayımlama seçeneği Microsoft ticaret olanaklarından yararlanır ve bulma ve değerlendirme ile satın alma ve uygulamaya kadar uçtan uca bir deneyim sunar. Transact teklifleri mevcut bir Microsoft aboneliğine veya kredi kartına göre faturalandırılır ve Microsoft 'un yayımcı adına bulut marketi işlemlerini barındırmalarına olanak tanır.

Iş Ortağı Merkezi 'nde yeni bir teklif oluşturduğunuzda Transact seçeneğini belirleyin. **Teklif kurulumu** sayfasında, **Kurulum Ayrıntıları**' nın altında, "Evet, Microsoft üzerinden satış yapmak istiyorum ve Microsoft ana bilgisayar işlemlerine izin veriyorum" seçeneğini belirleyin. Bu seçenek yalnızca, teklif türü için Transact kullanılabiliyorsa gösterilir.

## <a name="transact-overview"></a>Transact genel bakış

Transact Publishing seçeneğini kullanırken, Microsoft üçüncü taraf yazılımlarının ve bazı teklif türlerinin müşterinin Azure aboneliğine dağıtılması için satışı sağlar. Yayımcı, bir faturalandırma modeli ve teklif türü seçerken, altyapı ücretleri ve kendi yazılım lisanslama ücretlerine ilişkin faturalandırmayı göz önünde bulundurmalıdır.

Şu anda aşağıdaki teklif türleri için Transact yayımlama seçeneği desteklenmektedir:

- Sanal makineler
- Azure uygulamaları
- SaaS uygulamaları

### <a name="billing-infrastructure-costs"></a>Faturalandırma altyapısı maliyetleri

**Sanal makineler** ve **Azure uygulamaları**için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğine faturalandırılır. Altyapı kullanım ücretleri fiyatlandırılır ve müşterinin faturasında yazılım sağlayıcısının lisanslama ücretlerinden ayrı olarak sunulur.

**SaaS uygulamaları**için, yayımcının tek bir maliyet öğesi olarak Azure altyapı kullanım ücretlerini ve yazılım lisanslama ücretlerini hesaba göre hesabı olması gerekir.  Müşteriye yönelik bir sabit ücret olarak gösterilir. Azure altyapı kullanımı, doğrudan iş ortağı tarafından yönetilir ve faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlendirmeyi tercih edebilir. Yazılım Lisanslama ücretleri tarifeli veya tüketim temelli değildir.

## <a name="transact-billing-models"></a>Transact faturalandırma modelleri

Kullanılan işlem seçeneğine bağlı olarak, yazılım lisansı ücretleri aşağıdaki gibidir:

- **Ücretsiz** – yazılım lisansları için ücret alınmaz.
- **Kendi lisansınızı getirin** (KLG) – yazılım lisansları için geçerli ücretler, doğrudan yayımcı ile müşteri arasında yönetilir. Microsoft yalnızca Azure altyapı kullanım ücretleri üzerinden geçer. Bu yalnızca sanal makineler ve Azure uygulamaları için geçerlidir.
- **Kullandıkça Öde** – yazılım lisans ücretleri, kullanılan Azure altyapısına göre saat başına, çekirdek başına (vCPU) fiyatlandırma ücreti olarak sunulur. Bu yalnızca sanal makineler ve Azure uygulamaları için geçerlidir.
- **Abonelik fiyatlandırması** – yazılım lisans ücretleri aylık veya yıllık olarak, sabit bir fiyat veya bilgisayar başına faturalandırılan yinelenen ücret olarak sunulur. Bu, SaaS uygulamaları (aylık veya yıllık) ve Azure uygulamaları tarafından yönetilen uygulamalar (aylık) için geçerlidir.
- **Ücretsiz yazılım denemesi** : 30 veya 90 gün için yazılım lisansları ücreti alınmaz.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Ücretsiz ve kendi lisansını getir (KLG) fiyatlandırması

Ücretsiz veya kendi lisans işlem teklifini yayımlarken, Microsoft, yazılım lisans ücretlerinizin satış işlemini kolaylaştırmaya yönelik bir rol oynamaz. Yayımcı, liste ve deneme yayımlama seçenekleri gibi yazılım lisansı ücretlerine %100 oranında devam eder.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Kullandıkça öde ve abonelik (site tabanlı) fiyatlandırması

Kullandıkça Öde veya abonelik işlemi teklifi yayımlandığında, Microsoft yazılım lisansı satın alma, iade ve geri ödeme işlemlerini işleyecek teknoloji ve hizmetler sağlar. Bu senaryoda, yayımcı Microsoft 'un bu amaçlar için bir aracı görevi görmesini yetkilendirir. Yayımcı, Microsoft 'un yazılım lisanslama işlemini kolaylaştırmasına olanak sağlarken satıcı, sağlayıcı, dağıtıcı ve lisans verme özelliklerini korur.

Microsoft, müşterilerin, Microsoft 'un ticari marketi ve Son Kullanıcı lisanslama sözleşmenizin hüküm ve koşullarına tabi olmak üzere yazılımınızı sipariş, lisans ve kullanma imkanı sağlar. Kendi Son Kullanıcı lisanslama sözleşmenizi sağlamanız veya teklifi oluştururken [Standart sözleşmeyi](./standard-contract.md) seçmeniz gerekir.

### <a name="free-software-trials"></a>Ücretsiz yazılım denemeleri

Transact yayımlama senaryolarında, yazılım lisansını 30 veya 90 gün boyunca ücretsiz olarak kullanılabilir hale getirebilirsiniz. Bu ayırma özelliği, iş ortağı çözümü kullanılarak yönetilen Azure altyapı kullanımının maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Teklif türlerini ve faturalandırma modellerini bir teklifi ayırmak için kullanmanın yanı sıra, anlaşmalı, anlaşmaya özgü fiyatlandırma veya özel yapılandırmalarda özel bir teklif elde edebilirsiniz. Özel teklifler, her üç Transact yayımlama seçeneği tarafından desteklenir.

Bu seçenek, genel kullanıma sunulan teklife göre daha yüksek veya daha düşük fiyatlandırmayı sağlar. Özel teklifler, bir teklifle bir Premium için indirim veya bir Premium eklemek için kullanılabilir. Özel teklifler, bir veya daha fazla müşteriye, Azure aboneliklerini teklif düzeyinde listeleyerek beyaz listeye alınabilir.

### <a name="examples"></a>Örnekler

**Kullandıkça Öde** 

Kullandıkça Öde aşağıdaki maliyet yapısına sahiptir:

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

**Kendi lisansınızı getirin (KLG)**

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

Azaltılmış Market hizmet ücreti için uygun olan iş ortakları, 2019 2020 Haziran 'a kadar Mayıs 'a kadar olan SaaS teklifleriyle daha düşük bir işlem ücreti görür.

Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $90,00 ' ye ödeme yapar:

|Microsoft faturaları  | $100,00/ay  |
|---------|---------|
|Microsoft, lisans maliyetinizi %80 oranında ödetir <br> \*Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %90 oranında ödetir   |   $80,00/ay <br> \*$90,00/ay    |
|Microsoft, lisans maliyetlerinizin %20 ' sini tutar <br> \*Microsoft, tüm nitelikli SaaS uygulamaları için lisans maliyetinizi %10 oranında tutar.  |  $20,00/ay <br> \*$10,00     |

Ticari Market 'te yayımladığınız belirli SaaS ürünleri için Microsoft, **Market hizmet ücretini** %20 ' den (Microsoft Publisher anlaşmasında açıklandığı gibi) %10 oranında azaltacak. Teklifinizin uygun olması için, tekliflerinizin en az birinin, IP ortak satışı veya IP ortak satışı kapsamında Microsoft tarafından belirlenmiş olması gerekir. Bu ay için daha düşük olan bu Market hizmeti ücretini almak için, uygunluk önceki takvim ayının sonundan önce en az beş iş günü önce sağlanmalıdır. Daha az Market hizmeti ücreti, sanal makineler, yönetilen uygulamalar veya ticari Market aracılığıyla sunulan diğer ürünlerden hiçbiri için de geçerlidir. Bu azaltılan ücret, Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 arasında toplanan lisans ücretleri sayesinde nitelikli tekliflerle kullanılabilir. Bu süreden sonra, ücret normal miktarına geri döner.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Müşteri Faturalama, ödeme, faturalandırma ve koleksiyonlar

**Faturalama ve ödeme** – abonelik veya PAYGO yazılım lisansı ücretleri sunmak için müşterinin tercih edilen faturalandırma yöntemini kullanabilirsiniz.

**Kurumsal Anlaşma** – müşterinin tercih edilen faturalandırma yöntemi Microsoft kurumsal anlaşma ise, yazılım lisans ücretleriniz, Azure 'a özgü kullanım maliyetlerinden ayrı olarak, bu faturalandırma yöntemi kullanılarak, bir maliyet dökümü olarak faturalandırılacaktır.

**Kredi kartları ve aylık fatura** – müşteriler, kredi kartı ve aylık fatura kullanarak da ödeme yapabilir. Bu durumda, yazılım lisans ücretleriniz, bir Azure 'a özgü kullanım maliyetlerinden ayrı olarak, Kurumsal Anlaşma senaryosu gibi, yeni bir maliyet olarak faturalandırılacaktır.

**Ücretsiz krediler ve parasal taahhüt** : bazı müşteriler azure 'u kurumsal anlaşma bir parasal taahhütte ön ödeme yapmayı tercih ediyor veya Azure ile kullanım için ücretsiz krediler sağladı. Bu krediler Azure kullanımı için ödeme yapmak üzere kullanılabilse de, yayımcı yazılımı lisans ücretleri için ödeme yapmak üzere kullanılamaz.

**Faturalandırma ve koleksiyonlar** – yayımcı yazılım lisansı faturalandırması müşteri tarafından seçilen faturalandırma yöntemi kullanılarak sunulur ve faturalama zaman çizelgesine uyar. Yerinde Kurumsal Anlaşma olmayan müşteriler Market yazılım lisansları için aylık olarak faturalandırılır. Kurumsal Anlaşma olan müşteriler aylık olarak sunulan bir fatura aracılığıyla aylık olarak faturalandırılır.

Abonelik veya kullandıkça öde fiyatlandırma modelleri seçildiğinde, Microsoft, yayımcının aracı olarak davranır ve faturalandırma, ödeme ve koleksiyonun tüm yönleriyle sorumludur.

### <a name="publisher-payout-and-reporting"></a>Yayımcı ödeme ve raporlama

Aracı olarak Microsoft tarafından toplanan tüm yazılım lisanslama ücretleri, aksi belirtilmediği ve yayımcı ödeme sırasında kesilmedikleri sürece %20 işlem ücretine tabidir.

Müşteriler genellikle Kurumsal Anlaşma veya kredi kartı etkin bir Kullandıkça Öde sözleşmesini kullanarak satın alabilir. Anlaşma türü faturalandırma, faturalandırma, toplama ve ödeme zamanlamasını belirler.

>[!NOTE]
>Transact yayımlama seçeneğine yönelik tüm raporlama ve Öngörüler, Iş Ortağı Merkezi 'nin analiz bölümü aracılığıyla kullanılabilir.

#### <a name="billing-questions-and-support"></a>Faturalandırma soruları ve destek

Daha fazla bilgi ve yasal ilkeler için bkz. [Yayımcı Sözleşmesi](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt) (Iş Ortağı Merkezi 'nde kullanılabilir).

Faturalandırma soruları hakkında yardım almak için [ticari Market yayımcı desteği](https://aka.ms/marketplacepublishersupport)'ne başvurun.

## <a name="transact-requirements"></a>Transact gereksinimleri

Bu bölümde, farklı teklif türleri için Transact gereksinimleri ele alınmaktadır.

### <a name="requirements-for-all-offer-types"></a>Tüm teklif türleri için gereksinimler

- Teklifin fiyatlandırma modelinden bağımsız olarak, Transact yayımlama seçeneği için Microsoft hesabı ve finansal bilgiler gereklidir.
- Zorunlu mali bilgiler, ödeme hesabı ve vergi profilini içerir.

Bu hesapları ayarlama hakkında daha fazla bilgi için bkz. [Iş Ortağı Merkezi 'nde ticari Market hesabınızı yönetme](partner-center-portal/manage-account.md).

### <a name="requirements-for-specific-offer-types"></a>Belirli teklif türleri için gereksinimler

Transact yayımlama seçeneği yalnızca aşağıdaki Market teklif türleriyle kullanılabilir:

- **Sanal makine** : ücretsiz, kendi lisansını getir veya kullandıkça öde fiyatlandırma modelleri arasından seçim yapın ve teklif düzeyinde tanımlanan SKU 'lar olarak sunun. Microsoft, müşterinin Azure faturasında, yayımcı yazılımı lisans ücretlerini temel alınan Azure altyapı ücretlerinden ayrı olarak sunar. Azure altyapı ücretleri, yayımcı yazılımı kullanılarak dağıtılır.

- **Azure uygulaması: çözüm şablonu veya yönetilen uygulama** – bir veya daha fazla sanal makine sağlamalıdır ve sanal makine fiyatlandırmasının toplamını almalıdır. Tek bir planda yönetilen uygulamalar için, sanal makine fiyatlandırması yerine fiyatlandırma modeli olarak bir sabit hızlı aylık abonelik seçilebilir. Bazı durumlarda, Azure altyapı kullanım ücretleri müşteriye yazılım lisans ücretlerinden ayrı olarak geçirilir, ancak aynı faturalandırma beyanının üzerinden yapılır. Ancak, ISV altyapı ücretleri için yönetilen bir uygulama teklifi yapılandırırsanız Azure kaynakları yayımcıya faturalandırılır ve müşteri altyapı, yazılım lisansı ve yönetim hizmetleri maliyetini içeren bir sabit ücret alır.

- **SaaS uygulaması** -çok kiracılı bir çözüm olmalıdır, kimlik doğrulaması için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kullanın ve [SaaS karşılama API 'leriyle](partner-center-portal/pc-saas-fulfillment-api-v2.md)tümleştirin. Azure altyapı kullanımı, doğrudan sizin kullanımınıza (iş ortağı) göre yönetilir ve faturalandırılır. bu nedenle, Azure altyapı kullanım ücretleri ve yazılım lisanslama ücretlerini tek bir maliyet öğesi olarak hesaba almalısınız. Ayrıntılı kılavuz için bkz. [ticari Market 'te yeni SaaS teklifi oluşturma](partner-center-portal/create-new-saas-offer.md).

## <a name="next-steps"></a>Sonraki adımlar

- Teklifinizin seçimini ve yapılandırmasını tamamlamak için teklif türüne göre yayımlama seçeneklerinde uygunluk gereksinimlerini gözden geçirin.
- Çözümünüzün bir teklif türü ve yapılandırmaya nasıl eşlendiğini gösteren örnekler için storefront göre yayımlama düzenlerini gözden geçirin.
