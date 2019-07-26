---
title: Azure Faturalandırma ve maliyet yönetimi için otomasyon senaryoları | Microsoft Docs
description: Genel faturalandırma ve maliyet yönetimi senaryolarının farklı API 'Lerle nasıl eşleştirildiğini öğrenin.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/13/2018
ms.author: banders
ms.openlocfilehash: 3d06df9b9a90f26b39afc17b8fcd02c85da567a1
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443245"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Faturalandırma ve maliyet yönetimi için otomasyon senaryoları

Bu makalede, Azure Faturalandırma ve maliyet yönetimi için genel senaryolar listelenmektedir. Bu senaryoları, kullanabileceğiniz API 'Ler ile eşler. Her senaryo-API eşlemesi altında, API 'lerin ve sundukları işlevselliğin bir özetini bulabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Ücret ile ilgili ve kullanımla ilgili soruları yanıtlamak için faturalandırma ve maliyet yönetimi API 'Lerini çeşitli senaryolarda kullanabilirsiniz. Yaygın senaryolardan oluşan bir ana hat aşağıda verilmiştir:

- **Fatura mutabakatı**: Microsoft doğru miktarı mi ücretlendirdim?  Faturam nedir ve kendi kendinize hesaplayabilir miyim?

- **Çapraz ücretler**: Ne kadar ücretlendirildiğimize göre, Kuruluşumun ne kadar ücret ödediğine biliyorum mi?

- **Maliyet iyileştirmesi**: Ne kadar ücretlendirildiğimizi biliyorum. Azure 'da harcadığım paradan nasıl daha fazla bilgi alabilirim?

- **Maliyet izleme**: Zaman içinde Azure 'u ne kadar harcadığımı ve kullanmakta olduğumu görmek istiyorum. Eğilimleri nelerdir? Nasıl daha iyi yapabilirim?

- **Aylık Azure harcama**: Geçerli aydaki harcama tarihi ne olur? Azure 'un harcama ve/veya kullanımım durumunda herhangi bir değişiklik yapmam gerekiyor mu? Aylık olarak Azure tüketdim.

- **Uyarılar**: Kaynak tabanlı tüketimi veya parasal tabanlı uyarıları nasıl ayarlayabilirim?

## <a name="scenario-to-api-mapping"></a>Senaryo-API eşleme

|         API        | Fatura mutabakatı    | Çapraz ücretler    | Maliyet iyileştirmesi    | Maliyet izleme    | Orta ay harcama    | Uyarılar    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Bütçeler                     |                           |                  |           X          |                  |                    |     X     |
| Market Ücretleri                |             X             |         X        |           X          |         X        |          X         |     X     |
| Fiyat Listesi                 |             X             |         X        |           X          |         X        |          X         |           |
| Rezervasyon Önerileri |                           |                  |           X          |                  |                    |           |
| Rezervasyon Ayrıntıları         |                           |                  |           X          |         X        |                    |           |
| Rezervasyon Özetleri       |                           |                  |           X          |         X        |                    |           |
| Kullanım Ayrıntıları               |             X             |         X        |           X          |         X        |          X         |     X     |
| Fatura dönemleri             |             X             |         X        |           X          |         X        |                    |           |
| Faturalar                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Derecelendirilmemiş kullanım               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Senaryo-API eşleme, kurumsal tüketim API 'Lerini içermez. Mümkün olduğunda, yeni geliştirme senaryoları için genel tüketim API 'Lerini kullanın.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
Web Direct ve Enterprise müşterileri, belirtilen durumlar dışında aşağıdaki API 'Leri kullanabilir:

-   [Bütçe API 'si](https://docs.microsoft.com/rest/api/consumption/budgets) (*Yalnızca kurumsal müşteriler*): Kaynaklar, kaynak grupları ya da faturalandırma ölçümleri için maliyet veya kullanım bütçeleri oluşturun. Bütçeleri oluştururken, tanımlı bütçe eşiklerini aşdığınızda size bildirimde bulunan uyarıları yapılandırabilirsiniz. Ayrıca, bütçe miktarlarından erişildiğinde gerçekleştirilecek eylemleri de yapılandırabilirsiniz.

-   [Market ücretleri API 'si](https://docs.microsoft.com/rest/api/consumption/marketplaces): Tüm Azure Marketi kaynaklarında (Azure iş ortağı teklifleri) ücret ve kullanım verilerini alın. Bu verileri, tüm Market kaynakları genelinde maliyet eklemek veya belirli kaynaklardaki maliyetleri/kullanımı araştırmak için kullanabilirsiniz.

-   [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) (*Yalnızca kurumsal müşteriler*): Tüm ölçümler için özel fiyatlandırma alın. Kuruluşlar, kullanım ve Market verilerini kullanarak maliyetleri hesaplamak için kullanım ayrıntıları ve Market kullanım bilgileriyle birlikte bu verileri kullanabilir. 

-   [Ayırma önerileri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations): Ayrılmış VM örnekleri satın alma önerilerini alın. Öneriler, beklenen maliyet tasarrufunu ve satın alma tutarlarını çözümlemenize yardımcı olur. Daha fazla bilgi için bkz. [Azure ayırma Otomasyonu API 'leri](billing-reservation-apis.md).

-   [Ayırma ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/reservationsdetails): Ne kadar tüketim ayrılacağını ve ne kadarının kullanıldığına ilişkin daha önce satın alınan VM ayırmaları hakkında bilgi görüntüleyin. Verileri VM düzeyinde ayrıntılı olarak görebilirsiniz. Daha fazla bilgi için bkz. [Azure ayırma Otomasyonu API 'leri](billing-reservation-apis.md).

-   [Ayırma özetleri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationssummaries): Toplamada ne kadar tüketim ayrılacağını ve toplamanın ne kadarının kullanıldığını gösteren VM ayırmaları hakkında toplu bilgileri görün. Daha fazla bilgi için bkz. [Azure ayırma Otomasyonu API 'leri](billing-reservation-apis.md).

-   [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/usagedetails): Microsoft 'un tüm Azure kaynakları hakkında ücret ve kullanım bilgileri alın. Bilgiler, şu anda günde bir kez bir kez yayılan kullanım ayrıntıları kayıtları biçimindedir. Tüm kaynakların maliyetlerini eklemek veya belirli kaynaklardaki maliyetleri/kullanımı araştırmak için bu bilgileri kullanabilirsiniz.

-   [Ratecard API 'si](/previous-versions/azure/reference/mt219005(v=azure.100)): Web doğrudan müşterisiyseniz ölçüm ücretleri alın. Ardından, beklenen faturanızı el ile hesaplamak için kaynak kullanım bilgileriniz ile döndürülen bilgileri kullanabilirsiniz. 

-   [Derecelendirilmemiş kullanım API 'si](/previous-versions/azure/reference/mt219003(v=azure.100)): Azure 'un herhangi bir ölçümü/ücretlendirme yapmadan önce ham kullanım bilgilerini alın.

### <a name="billing"></a>Faturalandırma
-   [Faturalandırma dönemleri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Analiz edilecek bir fatura dönemini ve bu döneme ait fatura kimliklerini belirleme. Fatura kimliklerini faturalar API 'siyle birlikte kullanabilirsiniz.

-   [Faturalar API 'si](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices): PDF biçiminde faturalandırma dönemi için bir faturaya ait indirme URL 'sini alın.

### <a name="enterprise-consumption"></a>Kurumsal tüketim
Aşağıdaki API 'Ler yalnızca kuruluş içindir:

-   [Bakiye Özeti API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): Bakiyeler, yeni satın alma işlemleri, Azure Marketi hizmeti ücretleri, ayarlamalar ve fazla kullanım ücretleri hakkındaki bilgilere yönelik aylık bir Özet alın. Bu bilgileri geçerli fatura dönemi veya geçmişteki herhangi bir dönem için alabilirsiniz. Kuruluşlar, bu verileri el ile hesaplanan Özet ücretleri ile karşılaştırmak için kullanabilir. Bu API kaynağa özgü bilgiler veya maliyetlerin toplam bir görünümünü sağlamaz.

-   [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): Geçerli ay, belirli bir fatura dönemi veya özel bir tarih dönemi için Azure kullanımı (Microsoft tekliflerinden) hakkında bilgi alın. Kuruluşlar, bu verileri fiyat ve tüketim temelinde faturaları el ile hesaplamak için kullanabilir. Kuruluşlar, kuruluş genelinde maliyet sağlamak için departman/kuruluş bilgilerini de kullanabilir. Veriler, kullanım/maliyet kaynağına özgü bir görünüm sağlar.

-   [Market mağaza ücreti API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): Geçerli aya ait Azure kullanımı (iş ortağı teklifleri), belirli bir fatura dönemi veya özel bir tarih dönemi hakkında bilgi alın. Kuruluşlar, bu verileri fiyat ve tüketim temelinde faturaları el ile hesaplamak için kullanabilir. Kuruluşlar, kuruluş genelinde maliyet sağlamak için departman/kuruluş bilgilerini de kullanabilir. Bu API, kullanım/maliyet kaynağına özgü bir görünüm sağlar.

-   [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): Verilen kayıt ve fatura dönemi için her bir ölçüm için uygun ücreti alın. Beklenen faturanızı el ile hesaplamak için bu ücret bilgilerini kullanım ayrıntıları ve Market kullanım bilgileriyle birlikte kullanabilirsiniz.

-   [Faturalandırma dönemleri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Fatura dönemlerinin bir listesini alın. API Ayrıca, faturalandırma dönemine ait olan dört kurumsal API verisi kümesi için API yolunu işaret eden bir özellik sağlar: BalanceSummary, UsageDetails, Market ücretleri ve fiyat listesi.

-   [Ayrılmış örnek önerileri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): En fazla 7 gün, 30 gün, sanal makine kullanımı 60 gün ve tek ve paylaşılan satın alma önerileri alın. Beklenen maliyet tasarrufunu ve önerilen satın alma tutarlarını çözümlemek için bu API 'yi kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırma Otomasyonu API 'leri](billing-reservation-apis.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Kurumsal Raporlama API 'leri ile tüketim API 'Leri arasındaki fark nedir? Ne zaman kullanmalıyım?
Bu API 'Ler benzer bir işlevsellik kümesine sahiptir ve faturalandırma ve maliyet yönetimi alanında aynı geniş soruları yanıtlayabilir. Ancak farklı izleyicileri hedefler: 

- Kurumsal Raporlama API 'Leri, Microsoft ile Kurumsal Anlaşma imzalayan ve bu kullanıcılara, anlaşmalı parasal taahhütler ve özel fiyatlandırma erişimi veren müşteriler tarafından kullanılabilir. API 'Ler [Enterprise Portal](https://ea.azure.com)alabileceğiniz bir anahtar gerektirir. Bu API 'lerin açıklaması için bkz. [Kurumsal müşterilere yönelik Raporlama API 'Lerine genel bakış](billing-enterprise-api.md).

- Tüketim API 'Leri, birkaç özel durum dışında tüm müşteriler tarafından kullanılabilir. Daha fazla bilgi için bkz. [Azure tüketim API 'sine genel bakış](billing-consumption-api-overview.md) ve [Azure tüketim API başvurusu](https://docs.microsoft.com/rest/api/consumption/). En son geliştirme senaryolarına yönelik çözüm olarak, sunulan API 'Leri öneririz. 

### <a name="whats-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Kullanım ayrıntıları API 'SI ile kullanım API 'SI arasındaki fark nedir?
Bu API 'Ler temelde farklı veriler sağlar:

- [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) , ölçüm örneği başına Azure kullanımı ve maliyet bilgilerini sağlar. Verilen veriler Azure 'daki maliyet ölçüm sistemi üzerinden zaten geçmiştir ve diğer olası değişikliklerle birlikte buna uygulanan maliyetli bir maliyettir:

   - Ön ödemeli parasal taahhütlerin kullanımı için hesapta yapılan değişiklikler
   - Azure tarafından keşfedilen kullanım tutarsızlıkları için hesapta yapılan değişiklikler

- [Kullanım API 'si](/previous-versions/azure/reference/mt219003(v=azure.100)) , Azure 'daki maliyet ölçümü sisteminin geçirmeden önce ham Azure kullanım bilgilerini sağlar. Bu veriler, Azure ücret ölçüm sisteminden sonra görülen kullanım veya ücret miktarıyla bağıntılı olabilir.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Fatura API 'si ile kullanım ayrıntıları API 'SI arasındaki fark nedir?
Bu API 'Ler aynı verilerin farklı bir görünümünü sağlar:

- [Fatura API 'si](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices) yalnızca Web Direct müşterilerine yöneliktir. Her ölçüm türünün toplam ücretlerine göre faturanızda aylık bir toplu toplama sağlar. 

- [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) her bir güne ait kullanım/maliyet kayıtlarının ayrıntılı bir görünümünü sağlar. Hem kurumsal hem de Web Direct müşterileri bu uygulamayı kullanabilir.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Fiyat listesi API 'si ve RateCard API 'SI arasındaki fark nedir?
Bu API 'Ler benzer veri kümeleri sağlar ancak farklı izleyicileri vardır:

- [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) , bir kurumsal müşteri için anlaşmakta olan özel fiyatlandırmayı sağlar.

- [Ratecard API 'si](/previous-versions/azure/reference/mt219005(v=azure.100)) , Web Direct müşterileri için geçerli olan herkese açık fiyatlandırma sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure kullanımı hakkında bilgi edinmek için Azure API 'Leri kullanma hakkında daha fazla bilgi için bkz. [Azure tüketim API 'sine genel bakış](billing-consumption-api-overview.md) ve [Azure Faturalandırma API 'sine genel bakış](billing-usage-rate-card-overview.md).

- Faturanızı ayrıntılı günlük kullanım dosyası ve Azure portal maliyet yönetimi raporlarıyla karşılaştırmak için, bkz. [Microsoft Azure faturanızı anlayın](billing-understand-your-bill.md).

- Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
