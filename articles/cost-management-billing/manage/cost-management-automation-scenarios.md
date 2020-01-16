---
title: Azure faturalama ve maliyet yönetimi için otomasyon senaryoları | Microsoft Docs
description: Yaygın faturalama ve maliyet yönetimi senaryolarının farklı API'lere nasıl eşlendiğini öğrenin.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: c9de7d5f7661e4083d3a2f5b53368616d0e6655a
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75992822"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Faturalama ve maliyet yönetimi için otomasyon senaryoları

Bu makalede Azure faturalama ve maliyet yönetimi için yaygın senaryolar listelenir. Bu senaryolar kullanabileceğiniz API'lerle eşlenir. Her senaryo-API eşlemesinde API'lerin özetini ve sağladıkları işlevleri bulabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Maliyetle ilgili ve kullanımla ilgili soru ları yanıtlamak için faturalama ve maliyet yönetimi API'lerini çeşitli senaryolarda kullanabilirsiniz. Yaygın senaryolar genel hatlarıyla şöyledir:

- **Fatura mutabakatı**: Microsoft doğru miktarı mi ücretlendirdim?  Faturam ne kadardır ve bunu kendim hesaplayabilir miyim?

- **Çapraz ücretler**: artık ne kadar ücretlendirildiğimize göre, Kuruluşumun ne kadar ücret ödediğine biliyorum mi?

- **Maliyet iyileştirmesi**: ne kadar ücretlendirildiğimizi biliyorum. Azure'a yaptığım harcamadan nasıl daha iyi yararlanabilirim?

- **Maliyet izleme**: zaman içinde Azure 'u ne kadar harcadığımı ve kullandığınızı görmek istiyorum. Eğilimler nelerdir? Daha iyisini nasıl yapabilirim?

- **Aylık Azure harcama**: geçerli aydan ne kadar harcama tarihine kadar? Azure harcamamda ve/veya kullanımında herhangi bir değişiklik yapmak gerekiyor mu? Ay boyunca ne zaman Azure tüketimim en yüksek düzeye çıkıyor?

- **Uyarılar**: kaynak tabanlı tüketimi veya parasal tabanlı uyarıları nasıl ayarlayabilirim?

## <a name="scenario-to-api-mapping"></a>Senaryo-API eşlemesi

|         eklentisi        | Fatura mutabakatı    | Çapraz ücretler    | Maliyet iyileştirmesi    | Maliyet izleme    | Ay ortası harcaması    | Uyarılar    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Bütçeler                     |                           |                  |           X          |                  |                    |     X     |
| Market Ücretleri                |             X             |         X        |           X          |         X        |          X         |     X     |
| Fiyat Listesi                 |             X             |         X        |           X          |         X        |          X         |           |
| Rezervasyon Önerileri |                           |                  |           X          |                  |                    |           |
| Rezervasyon Ayrıntıları         |                           |                  |           X          |         X        |                    |           |
| Rezervasyon Özetleri       |                           |                  |           X          |         X        |                    |           |
| Kullanım Ayrıntıları               |             X             |         X        |           X          |         X        |          X         |     X     |
| Fatura Dönemleri             |             X             |         X        |           X          |         X        |                    |           |
| Faturalar                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Derecelendirilmemiş Kullanım               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Senaryo-API eşlemesi Kurumsal Tüketim API'leri içermez. Mümkün olduğunca yeni geliştirme senaryoları için genel Tüketim API'lerini kullanın.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
Web Direct ve Enterprise müşterileri tersi belirtilmediği sürece aşağıdaki API'lerin tümünü kullanabilir:

-   [Bütçe API 'si](https://docs.microsoft.com/rest/api/consumption/budgets) (*yalnızca kurumsal müşteriler*): kaynaklar, kaynak grupları veya faturalandırma ölçümleri için maliyet veya kullanım bütçeleri oluşturun. Bütçeleri oluştururken, tanımlı bütçe eşiklerini aştığınızda size bildirilmesi için uyarıları yapılandırabilirsiniz. Ayrıca bütçe tutarlarına ulaştığınızda bazı eylemlerin gerçekleştirilmesini de yapılandırabilirsiniz.

-   [Market ücretleri API 'si](https://docs.microsoft.com/rest/api/consumption/marketplaces): tüm Azure Marketi kaynaklarında (Azure iş ortağı teklifleri) ücret ve kullanım verilerini alın. Bu verileri kullanarak tüm Market kaynaklarının maliyetlerini toplayabilir veya belirli kaynakların maliyet/kullanım durumunu araştırabilirsiniz.

-   [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) (*yalnızca kurumsal müşteriler*): tüm ölçümler için özel fiyatlandırma alın. Kuruluşlar bu verileri kullanım ayrıntılarıyla ve market kullanım bilgileriyle birlikte kullanarak, kullanım ve market verileri aracılığıyla maliyetleri hesaplayabilir.

-   [Ayırma önerileri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations): ayrılmış VM örnekleri satın alma önerilerini alın. Öneriler beklenen maliyet tasarruflarını ve satın alma tutarlarını analiz etmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-   [Ayırma ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/reservationsdetails): daha önce satın alınan VM rezervasyonları hakkında, ne kadar tüketim ayrılacağını ve ne kadar kullanıldığını öğrenin. Verileri VM ayrıntı düzeyinde görebilirsiniz. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-   [Ayırma özetleri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationssummaries): toplama için ne kadar tüketim ayrılacağını ve ne kadar harcandığını belirten, kuruluşunuzun satın aldığı VM ayırmaları hakkında toplanmış bilgilere bakın. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-   [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/usagedetails): Microsoft 'un tüm Azure kaynakları hakkında ücret ve kullanım bilgileri alın. Bilgiler şu anda ölçüm başına günde bir kez gösterilen kullanım ayrıntısı kayıtları biçimindedir. Bilgileri kullanarak tüm kaynakların maliyetlerini toplayabilir veya belirli kaynakların maliyet/kullanım durumunu araştırabilirsiniz.

-   [Ratecard API 'si](/previous-versions/azure/reference/mt219005(v=azure.100)): Web doğrudan müşterisiyseniz ölçüm ücretleri alın. Ardından döndürülen bilgileri kaynak kullanım bilgilerinizle birlikte kullanarak beklenen faturayı el ile hesaplayabilirsiniz.

-   [Derecelendirilmemiş kullanım API 'si](/previous-versions/azure/reference/mt219003(v=azure.100)): Azure 'dan önce herhangi bir ölçüm/ücretlendirme yapmadan önce ham kullanım bilgilerini alın.

### <a name="billing"></a>Faturalandırma
-   [Faturalandırma dönemleri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Bu döneme ait fatura kimlikleriyle birlikte analiz edilecek bir fatura dönemi belirlenir. Fatura kimliklerini Faturalar API'siyle kullanabilirsiniz.

-   [Faturalar API 'si](/rest/api/billing/2019-10-01-preview/invoices): fatura dönemı için PDF biçimindeki indirme URL 'sini alın.

### <a name="enterprise-consumption"></a>Kuruluş tüketimi
Aşağıdaki API'ler yalnızca Kuruluş içindir:

-   [Bakiye Özeti API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): bakiyeleri, yeni satın alımları, Azure Marketi hizmeti ücretleri, ayarlamaları ve fazla kullanım ücretleri hakkındaki bilgilerin aylık özetini alın. Bu bilgileri geçerli faturalama dönemi veya geçmişe ait bir dönem için alabilirsiniz. Kuruluşlar el ile hesaplanan özet ücretleriyle karşılaştırmak için bu verileri kullanabilir. Bu API kaynağa özgü bilgiler veya maliyetlerin toplam görünümünü sağlamaz.

-   [Kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): geçerli ay, belirli bir fatura dönemi veya özel bir tarih dönemi için Azure kullanımı (Microsoft tekliflerinden) hakkında bilgi alın. Kuruluşlar bu verileri kullanarak fiyat ve tüketim temelinde aylık faturaları el ile hesaplayabilir. Kuruluşlar, organizasyonlar genelindeki maliyetlere atıfta bulunmak için bölüm/kuruluş bilgilerini de kullanabilir. Veriler kaynağa özgü kullanım/maliyet görünümü sağlar.

-   [Market mağaza ücreti API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): geçerli ay, belirli bir fatura dönemi veya özel bir tarih dönemi için Azure kullanımı (iş ortağı teklifleri) hakkında bilgi alın. Kuruluşlar bu verileri kullanarak fiyat ve tüketim temelinde aylık faturaları el ile hesaplayabilir. Kuruluşlar, organizasyonlar genelindeki maliyetlere atıfta bulunmak için bölüm/kuruluş bilgilerini de kullanabilir. API kaynağa özgü kullanım/maliyet görünümü sağlar.

-   [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): belirtilen kayıt ve fatura dönemi için her bir ölçüm için uygun ücreti alın. Bu fiyat bilgilerini kullanım ayrıntıları ve market kullanım bilgileriyle birlikte kullanarak beklenen faturayı el ile hesaplayabilirsiniz.

-   [Faturalandırma dönemleri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Faturalandırma dönemlerinin bir listesini alın. API Ayrıca, faturalandırma dönemine ait olan dört kurumsal API verisi kümesi için API yolunu işaret eden bir özellik sağlar: BalanceSummary, UsageDetails, Market charges ve fiyat listesi.

-   [Ayrılmış örnek önerileri API 'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): 7 güne, 30 güne veya 60 güne sahip sanal makine kullanımına bakın ve tek ve paylaşılan satın alma önerileri alın. Bu API'yi kullanarak beklenen maliyet tasarruflarını ve önerilen satın alma tutarlarını analiz edebilirsiniz. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Kurumsal Raporlama API'leriyle Tüketim API'leri arasında ne fark vardır? Bunların her birini hangi durumlarda kullanmalıyım?
Bu API'lerin benzer bir işlev kümesi vardır ve faturalama ve maliyet yönetimi alanında aynı kapsamlı soruları yanıtlayabilirler. Ama hedef kitleleri farklıdır:

- Kurumsal Raporlama API'leri, Microsoft'la kendilerine anlaşmalı parasal taahhütler ve özel fiyatlandırma getiren bir Kurumsal Anlaşma imzalamış müşterilere yöneliktir. API'ler için [Enterprise Portal](https://ea.azure.com)'dan alabileceğiniz bir anahtar gerekir. Bu API'lerin açıklaması için bkz. [Enterprise müşterileri için Raporlama API'lerine genel bakış](enterprise-api.md).

- Tüketim API'leri birkaç özel durum dışında tüm müşteriler tarafından kullanılabilir. Daha fazla bilgi için bkz. [Azure tüketim API'sine genel bakış](consumption-api-overview.md) ve [Azure Tüketim API'si başvurusu](https://docs.microsoft.com/rest/api/consumption/). En son geliştirme senaryolarına çözüm olarak sağlanan API'lerin kullanılmasını öneririz.

### <a name="whats-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Kullanım Ayrıntıları API'siyle Kullanım API'si arasında ne fark vardır?
Bu API'ler temelde farklı veriler sağlar:

- [Kullanım Ayrıntıları API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) ölçüm örneği başına Azure kullanım ve maliyet bilgilerini sağlar. Sağlanan veriler Azure'da maliyet ölçüm sisteminden zaten geçirilmiştir ve bunlara diğer olası değişikliklerle birlikte maliyet de uygulanmıştır:

   - Ön ödemeli parasal taahhütler için hesapta yapılan değişiklikler
   - Azure tarafından bulunan kullanım tutarsızlıkları için hesapta yapılan değişiklikler

- [Kullanım API'si](/previous-versions/azure/reference/mt219003(v=azure.100)) Azure'da maliyet ölçüm sisteminden geçirilmeden önceki ham Azure kullanım bilgilerini sağlar. Bu verilerin, Azure ücret ölçüm sisteminden sonra görülen kullanım ve ücret miktarıyla hiçbir bağıntısı olmayabilir.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Fatura API'siyle Kullanım Ayrıntıları API'si arasında ne fark vardır?
Bu API'ler aynı verilerin farklı görünümünü sağlar:

- [Fatura API'si](/rest/api/billing/2019-10-01-preview/invoices) yalnızca Web Direct müşterilerine yöneliktir. Her ölçüm türü için toplanan ücretlere dayanarak faturanızın aylık toplamını sağlar.

- [Kullanım Ayrıntıları API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) her gün için kullanım/maliyet kayıtlarının ayrıntılı görünümünü sağlar. Hem Enterprise hem de Web Direct müşterileri bunu kullanabilir.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Fiyat Listesi API'siyle RateCard API'si arasında ne fark vardır?
Bu API'ler benzer veri kümeleri sağlar ama hedef kitleleri farklıdır:

- [Fiyat Listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) Enterprise müşterisi için üzerinde anlaşmaya varılmış özel fiyatlandırmayı sağlar.

- [RateCard API'si](/previous-versions/azure/reference/mt219005(v=azure.100)) Web Direct müşterilerine uygulanan herkese açık fiyatlandırmayı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Azure API'lerini kullanarak program aracılığıyla Azure kullanımınızla ilgili içgörü elde etme hakkında bilgi için bkz. [Azure Tüketim API'sine genel bakış](consumption-api-overview.md) ve [Azure Faturalama API'sine genel bakış](usage-rate-card-overview.md).

- Azure portalında faturanızı ayrıntılı günlük kullanım dosyası ve maliyet yönetimi raporlarıyla karşılaştırmak için bkz. [Microsoft Azure faturanızı anlama](../understand/review-individual-bill.md).

- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
