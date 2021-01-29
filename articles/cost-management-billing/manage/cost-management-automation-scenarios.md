---
title: Azure faturalama ve maliyet yönetimi için otomasyon senaryoları
description: Yaygın faturalama ve maliyet yönetimi senaryolarının farklı API'lere nasıl eşlendiğini öğrenin.
author: bandersmsft
ms.reviewer: adwise
tags: billing
ms.service: cost-management-billing
ms.subservice: common
ms.topic: reference
ms.date: 01/26/2021
ms.author: banders
ms.openlocfilehash: 12c13b8a65296fb0ee74e0ee0449b604facf2f48
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051270"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Faturalama ve maliyet yönetimi için otomasyon senaryoları

Bu makalede Azure faturalama ve maliyet yönetimi için yaygın senaryolar listelenir. Bu senaryolar kullanabileceğiniz API'lerle eşlenir. Her senaryo-API eşlemesinde API'lerin özetini ve sağladıkları işlevleri bulabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Maliyetle ilgili ve kullanımla ilgili soru ları yanıtlamak için faturalama ve maliyet yönetimi API'lerini çeşitli senaryolarda kullanabilirsiniz. Yaygın senaryolar genel hatlarıyla şöyledir:

- **Fatura mutabakatı**: Microsoft beni doğru miktarda mı ücretlendirdi?  Faturam ne kadardır ve bunu kendim hesaplayabilir miyim?

- **Çapraz ücretler**: Artık ne kadar ücretlendirildiğimi biliyorum, kuruluşumda kimin ödemesi gerekiyor?

- **Maliyet iyileştirmesi**: Ne kadar ücretlendirildiğimi biliyorum. Azure'a yaptığım harcamadan nasıl daha iyi yararlanabilirim?

- **Maliyet izleme**: Zaman içinde Azure'a ne kadar harcama yaptığımı ve Azure'ı ne kadar kullandığımı görmek istiyorum. Eğilimler nelerdir? Daha iyisini nasıl yapabilirim?

- **Aylık Azure harcama**: geçerli aydan ne kadar harcama tarihine kadar? Azure harcamamda ve/veya kullanımında herhangi bir değişiklik yapmak gerekiyor mu? Ay boyunca ne zaman Azure tüketimim en yüksek düzeye çıkıyor?

- **Uyarılar**: Kaynak tabanlı tüketimi veya para tabanlı uyarıları nasıl ayarlayabilirim?

## <a name="scenario-to-api-mapping"></a>Senaryo-API eşlemesi

|         API        | Fatura mutabakatı    | Çapraz ücretler    | Maliyet iyileştirmesi    | Maliyet izleme    | Ay ortası harcaması    | Uyarılar    |
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
| Azure perakende fiyatları                    |             X             |                  |           X          |         X        |                    |           |


> [!NOTE]
> Senaryo-API eşlemesi Kurumsal Tüketim API'leri içermez. Mümkün olduğunca yeni geliştirme senaryoları için genel Tüketim API'lerini kullanın.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
Web Direct ve Enterprise müşterileri tersi belirtilmediği sürece aşağıdaki API'lerin tümünü kullanabilir:

-    [Bütçe API'si](/rest/api/consumption/budgets) (*Yalnızca Enterprise müşterileri*): Kaynaklar, kaynak grupları veya faturalama ölçümleri için maliyet veya kullanım bütçeleri oluşturun. Bütçeleri oluştururken, tanımlı bütçe eşiklerini aştığınızda size bildirilmesi için uyarıları yapılandırabilirsiniz. Ayrıca bütçe tutarlarına ulaştığınızda bazı eylemlerin gerçekleştirilmesini de yapılandırabilirsiniz.

-    [Market Ücretleri API'si](/rest/api/consumption/marketplaces): Tüm Azure Market kaynakları (Azure iş ortağı teklifleri) ile ilgili ücret ve kullanım verileri alın. Bu verileri kullanarak tüm Market kaynaklarının maliyetlerini toplayabilir veya belirli kaynakların maliyet/kullanım durumunu araştırabilirsiniz.

-    [Fiyat Listesi API'si](/rest/api/consumption/pricesheet) (*Yalnızca Enterprise müşterileri*): Tüm ölçümler için özel fiyatlandırmayı alın. Kuruluşlar bu verileri kullanım ayrıntılarıyla ve market kullanım bilgileriyle birlikte kullanarak, kullanım ve market verileri aracılığıyla maliyetleri hesaplayabilir.

-    [Rezervasyon Önerileri API'si](/rest/api/consumption/reservationrecommendations): Ayrılmış VM Örnekleri satın alma önerileri alın. Öneriler beklenen maliyet tasarruflarını ve satın alma tutarlarını analiz etmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-    [Rezervasyon Ayrıntıları API'si](/rest/api/consumption/reservationsdetails): Daha önce satın alınmış VM rezervasyonları hakkında, ne kadar tüketimin ayrıldığı ve ne kadarının kullanıldığı gibi bilgileri görün. Verileri VM ayrıntı düzeyinde görebilirsiniz. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-    [Rezervasyon Özetleri API'si](/rest/api/consumption/reservationssummaries): Kuruluşunuzun satın aldığı VM rezervasyonları hakkında, ne kadar tüketimin ayrıldığı ve toplamda ne kadarının kullanıldığı gibi toplama bilgilerini görün. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

-    [Kullanım Ayrıntıları API'si](/rest/api/consumption/usagedetails): Microsoft'un sağladığı tüm Azure kaynaklarıyla ilgili ücret ve kullanım bilgilerini alın. Bilgiler şu anda ölçüm başına günde bir kez gösterilen kullanım ayrıntısı kayıtları biçimindedir. Bilgileri kullanarak tüm kaynakların maliyetlerini toplayabilir veya belirli kaynakların maliyet/kullanım durumunu araştırabilirsiniz.

-    [Azure perakende fiyatları](/rest/api/cost-management/retail-prices/azure-retail-prices): Kullandıkça Öde fiyatlandırmasıyla ölçüm ücretleri alın. Ardından döndürülen bilgileri kaynak kullanım bilgilerinizle birlikte kullanarak beklenen faturayı el ile hesaplayabilirsiniz.

### <a name="billing"></a>Faturalandırma
-    [Fatura Dönemleri API'si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Analiz edilecek faturalama dönemini ve bu döneme ait fatura kimliklerini belirleyin. Fatura kimliklerini Faturalar API'siyle kullanabilirsiniz.

-    [Faturalar API'si](/rest/api/billing/2019-10-01-preview/invoices): Faturalama dönemine ait PDF biçiminde bir fatura için indirme URL'sini alın.

### <a name="enterprise-consumption"></a>Kuruluş tüketimi
Aşağıdaki API'ler yalnızca Kuruluş içindir:

-    [Bakiye Özeti API'si](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): Bakiyeler, yeni satın almalar, Azure Market hizmeti ücretleri, düzeltmeler ve fazla kullanım ücretleri hakkındaki bilgilerin aylık özetini alın. Bu bilgileri geçerli faturalama dönemi veya geçmişe ait bir dönem için alabilirsiniz. Kuruluşlar el ile hesaplanan özet ücretleriyle karşılaştırmak için bu verileri kullanabilir. Bu API kaynağa özgü bilgiler veya maliyetlerin toplam görünümünü sağlamaz.

-    [Kullanım Ayrıntıları API'si](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): Geçerli ay, belirli bir faturalama dönemi veya özel bir tarih aralığı için Azure kullanımı (Microsoft tekliflerinin kullanımı) hakkındaki bilgileri alabilirsiniz. Kuruluşlar bu verileri kullanarak fiyat ve tüketim temelinde aylık faturaları el ile hesaplayabilir. Kuruluşlar, organizasyonlar genelindeki maliyetlere atıfta bulunmak için bölüm/kuruluş bilgilerini de kullanabilir. Veriler kaynağa özgü kullanım/maliyet görünümü sağlar.

-    [Market Mağazası Ücreti API'si](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): Geçerli ay, belirli bir faturalama dönemi veya özel bir tarih aralığı için Azure kullanımı (iş ortağı tekliflerinin kullanımı) hakkındaki bilgileri alabilirsiniz. Kuruluşlar bu verileri kullanarak fiyat ve tüketim temelinde aylık faturaları el ile hesaplayabilir. Kuruluşlar, organizasyonlar genelindeki maliyetlere atıfta bulunmak için bölüm/kuruluş bilgilerini de kullanabilir. API kaynağa özgü kullanım/maliyet görünümü sağlar.

-    [Fiyat Listesi API'si](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): Belirtilen kayıt ve faturalama dönemi için her ölçümün geçerli fiyatını alın. Bu fiyat bilgilerini kullanım ayrıntıları ve market kullanım bilgileriyle birlikte kullanarak beklenen faturayı el ile hesaplayabilirsiniz.

-    [Fatura Dönemleri API'si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Faturalama dönemlerinin listesini alın. API size faturalama dönemine ait dört Kurumsal API veri kümesi için API yoluna işaret eden bir de özellik verir: BalanceSummary, UsageDetails, Marketplace Charges ve PriceSheet.

-    [Ayrılmış Örnek Önerileri API'si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): 7 günlük, 30 günlük veya 60 günlük sanal makine kullanımına bakın ve Tek veya Paylaşılan Satın Alma önerileri alın. Bu API'yi kullanarak beklenen maliyet tasarruflarını ve önerilen satın alma tutarlarını analiz edebilirsiniz. Daha fazla bilgi için bkz. [Azure rezervasyon otomasyonu API’leri](../reservations/reservation-apis.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Kurumsal Raporlama API'leriyle Tüketim API'leri arasında ne fark vardır? Bunların her birini hangi durumlarda kullanmalıyım?
Bu API'lerin benzer bir işlev kümesi vardır ve faturalama ve maliyet yönetimi alanında aynı kapsamlı soruları yanıtlayabilirler. Ama hedef kitleleri farklıdır:

- Kurumsal Raporlama API'leri, Microsoft'la kendilerine anlaşmalı Azure Ön Ödemesi (eski adıyla parasal taahhüt) ve özel fiyatlandırma getiren bir Kurumsal Anlaşma imzalamış müşterilere yöneliktir. API'ler için [Enterprise Portal](https://ea.azure.com)'dan alabileceğiniz bir anahtar gerekir. Bu API'lerin açıklaması için bkz. [Enterprise müşterileri için Raporlama API'lerine genel bakış](enterprise-api.md).

- Tüketim API'leri birkaç özel durum dışında tüm müşteriler tarafından kullanılabilir. Daha fazla bilgi için bkz. [Azure tüketim API'sine genel bakış](consumption-api-overview.md) ve [Azure Tüketim API'si başvurusu](/rest/api/consumption/). En son geliştirme senaryolarına çözüm olarak sağlanan API'lerin kullanılmasını öneririz.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Fatura API'siyle Kullanım Ayrıntıları API'si arasında ne fark vardır?
Bu API'ler aynı verilerin farklı görünümünü sağlar:

- [Fatura API'si](/rest/api/billing/2019-10-01-preview/invoices) yalnızca Web Direct müşterilerine yöneliktir. Her ölçüm türü için toplanan ücretlere dayanarak faturanızın aylık toplamını sağlar.

- [Kullanım Ayrıntıları API'si](/rest/api/consumption/usagedetails) her gün için kullanım/maliyet kayıtlarının ayrıntılı görünümünü sağlar. Hem Enterprise hem de Web Direct müşterileri bunu kullanabilir.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Fiyat Listesi API'siyle RateCard API'si arasında ne fark vardır?
Bu API'ler benzer veri kümeleri sağlar ama hedef kitleleri farklıdır:

- [Fiyat Listesi API'si](/rest/api/consumption/pricesheet) Enterprise müşterisi için üzerinde anlaşmaya varılmış özel fiyatlandırmayı sağlar.

- [Azure perakende fiyatları API 'si](/rest/api/cost-management/retail-prices/azure-retail-prices) , Web Direct müşterileri için geçerli olan, herkese açık Kullandıkça Öde fiyatlandırması sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- REST API 'Leri kullanma hakkında daha fazla bilgi için bkz. [Azure perakende fiyatlara genel bakış](/rest/api/cost-management/retail-prices/azure-retail-prices).

- Azure portalında faturanızı ayrıntılı günlük kullanım dosyası ve maliyet yönetimi raporlarıyla karşılaştırmak için bkz. [Microsoft Azure faturanızı anlama](../understand/review-individual-bill.md).

- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).