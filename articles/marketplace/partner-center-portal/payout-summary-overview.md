---
title: Ödeme özetine genel bakış-Azure Marketi
description: Ödeme Özeti, teklifiniz ile kazanabileceğiniz paradan ilgili ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınız ve ne kadar ücret ödetireceğiz size bilgi verir.
ms.subservice: partnercenter-marketplace-publisher
ms.service: marketplace
ms.topic: article
author: mingshen
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 087f7df8f8e3cc5e6e01ff023a88306d5fc4102d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535969"
---
# <a name="payout-summary-overview"></a>Ödeme özetine genel bakış

[Ödeme Özeti](./payout-summary.md) , Microsoft ile kazandığınızı gösteren ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınız ve ne kadar ücret ödetireceğiz size bilgi verir.

Azure Market 'te teklifler satmanız durumunda, ödeme özetinde başarılı ödemeler hakkındaki bilgileri de görürsünüz. Azure Marketi ödemesi hakkında daha fazla bilgi için bkz. [Azure Marketi Katılım İlkeleri](https://docs.microsoft.com/legal/marketplace/participation-policy) ve [Microsoft Azure Market Yayımcı Sözleşmesi](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Ödemenizin uygun olması için, devam eden $50 [ödeme eşiğine](./payment-thresholds-methods-timeframes.md) ulaşmalıdır. Ödeme eşiği hakkında daha fazla bilgi için [Microsoft Azure Market Yayımcı Sözleşmesi](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)' ne bakın.

Transact yayımlama seçeneğine yönelik tüm raporlama ve Öngörüler, portalın sağ üst köşesinde bu simge kullanılarak erişilen Iş Ortağı Merkezi 'nin analiz bölümünde mevcuttur:

![Iş Ortağı Merkezi portalının sağ üst köşesindeki ödeme simgesini gösterir.](./media/payout-overview.png)

## <a name="roles-and-permissions"></a>Roller ve izinler

Bunlar, ödeme raporuna erişmek için roller ve izinlerdir:

| Raporlar/sayfalar | Hesap sahibi | Yönetici | Geliştirici | İş Katılımcısı | Finans Katılımcısı | Pazarlamacısıdır |
| --- | --- | --- | --- | --- | --- | --- |
| Alım raporu (neredeyse gerçek zamanlı veriler dahil) | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok |
| Geri bildirim raporu/yanıtları | Geri bildirim görüntüleyebilir ve gönderebilir | Geri bildirim görüntüleyebilir ve gönderebilir | Geri bildirim görüntüleyebilir ve gönderebilir | Erişim yok | Erişim yok | Geri bildirim görüntüleyebilir ve gönderebilir |
| --- | --- | --- | --- | --- | --- | --- |
| Sistem durumu raporu (neredeyse gerçek zamanlı veriler dahil) | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Kullanım raporu | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Ödeme hesabı | Güncelleştirebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleştirebilir | Erişim yok |
| Vergi profili | Güncelleştirebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleştirebilir | Erişim yok |
| Ödeme özeti | Görüntüleyebilir | Erişim yok | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok  |
| | | | | | | |

## <a name="payment-schedules"></a>Ödeme zamanlamaları

Dönemleri, iş ortağı görünürlüğünü ve müşterinin bir kredi kartı veya fatura kullanması gibi ödeme zamanlamalarıyla ilgili bir tartışma için, ödeme **ayrıntıları** konusunun [ödeme zamanlamaları](./payout-policy-details.md#payment-schedules) bölümüne bakın.

## <a name="transaction-history-download-export"></a>İşlem geçmişi indirme dışarı aktarma

Bu seçenek, Işlem geçmişi sayfasında gördüğünüz her bir atma çizgisi öğesinin indirilmesini sağlar. Bu, teşvikleri programıyla ilişkili tür, tarih, ilişkili işlem miktarı, müşteri, ürün ve diğer işlem ayrıntılarını içerir.

| Sütun adı | Description |
| --- | --- |
| Eardokgıd | Her bir atma için benzersiz tanımlayıcı |
| participantId | İş ortağının birincil kimliği program altında |
| participantIdType | Program mağaza programları ve Azure Market için ise programları ve satıcı için program KIMLIĞI |
| participantName | Kazanç ortağının adı |
| partnerCountryCode | Kazanç ortağının konumu/ülkesi/bölgesi |
| Programadı | Program adını teşvik edin/depolayın |
| TransactionId | İşlem için benzersiz tanımlayıcı |
| Işlem para birimi | Orijinal müşteri işleminin gerçekleştiği para birimi (iş ortağı konum para birimi değil) |
| transactionDate | İşlemin tarihi. Birçok işlemin tek bir işleme katkıda bulunduğu programlar için yararlıdır |
| transactionExchangeRate | Karşılık gelen işlem USD tutarını göstermek için kullanılan döviz kuru |
| Işlem miktarı | Orijinal işlem para biriminde, kazanıştan oluşan işleme göre işlem miktarı |
| transactionAmountUSD | ABD Doları cinsinden işlem miktarı (USD) |
| düzeyi | Kazanç için iş kuralı |
| Eardokgrate | Bir kazanç oluşturmak için işlem tutarına uygulanan teşvik oranı |
| miktar | İşlem programlarının faturalandırılan miktarı. Programa göre farklılık gösterir |
| quantityType | Miktar türü, örneğin: faturalanan miktar, aylık etkin kullanıcı (MAU) |
| earningType | Ücret ödemesinin, indirimin, Coop, satış, vb. olduğunu belirtir |
| earningAmount | Orijinal işlem para birimindeki miktarı atma |
| earningAmountUSD | USD cinsinden miktar atma |
| earningDate | Kazanlama tarihi |
| calculationDate | Sistemin sistemde hesaplandığı Tarih |
| earningExchangeRate | Karşılık gelen USD tutarını göstermek için kullanılan döviz kuru |
| exchangeRateDate | EarningAmount USD 'yi hesaplamak için kullanılan döviz kuru tarihi |
| paymentAmountWOTax | &quot;Yalnızca gönderilen ödemeler için para birimi ödemelerinde miktar (vergi olmadan) &quot; |
| paymentCurrency | Ödeme profilinde iş ortağı tarafından seçilen para birimine öde. Yalnızca gönderilen ödemeler için gösteriliyor |
| paymentExchangeRate | ExchangeRateDate kullanarak ödeme para birimindeki paymentAmountWOTax hesaplamak için kullanılan döviz kuru |
| Paymentıd | Ödeme için benzersiz tanımlayıcı. Bu sayı, banka ekstreninizde görülebilir |
| paymentStatus | Ödeme durumu |
| paymentStatusDescription | Ödeme durumunun açıklaması |
| customerId | Her zaman boş kalır |
| customerName | Her zaman boş kalır |
| partNumber | Her zaman boş kalır |
| productName | İşlemle bağlantılı ürün adı |
| productId | Benzersiz ürün tanımlayıcısı |
| Parentproductıd | Benzersiz üst ürün tanımlayıcısı. İşlem için bir üst ürün yoksa, üst ürün KIMLIĞI = ürün KIMLIĞI. |
| parentProductName | Ana ürünün adı. İşlem için bir üst ürün yoksa, üst ürün adı = ürün adı. |
| productType | Ürün türü (uygulama, eklenti ve oyun gibi) |
| Faturanumarası | Fatura numarası (yalnızca kurumsal anlaşmalar için) |
| ResellerID | Satıcı tanımlayıcısı |
| Reselleradı | Satıcı adı |
| Işlem türü | İşlem türü (satın alma, geri ödeme, ters çevirme ve geri ödeme gibi) |
| Localprovidersatıcı | Yerel sağlayıcı/kayıt satıcı |
| Vergilenhavale edilmiş | Havale edilen vergi miktarı (satış, kullanım veya KDV/GST vergileri). |
| Taxhavalemodeli | Vergi vergisinden sorumlu olan parti (satış, kullanım veya KDV/GST vergileri). |
| storeFee | Microsoft tarafından, ticari Market 'te uygulamayı veya eklentiyi kullanılabilir hale getirmek için bir ücret ücreti elde eden miktar. |
| transactionPaymentMethod | İşlem için kullanılan müşteri ödeme gereci (kart, mobil taşıyıcı faturalandırma ve PayPal gibi) |
| tpan | Üçüncü taraf ad ağı |
| customerCountry | Müşteri ülkesi/bölgesi |
| customerCity | Müşteri şehri |
| customerState | Müşteri durumu |
| Müşterzıp | Müşteri posta kodu |
| Değerine | Kiracının KIMLIĞI |
| Externalreferenceıd | Program için benzersiz tanımlayıcı |
| Externalreferenceıdlabel | Benzersiz tanımlayıcı etiketi |
| transactionCountryCode | İşlemin gerçekleştiği ülke/bölge kodu |
| taxCountry | Müşterinin ülkesi/bölgesi |
| taxState | Müşterinin durumu |
| Vergilencity | Müşterinin şehri |
| taxZipCode | Müşterinin ZIP/posta kodu |
| LicensingProgramName | Lisanslama programının adı |
| Program kodu | Program adıyla Eşlenecek dize |
| Earningamountınlastpaymentcurrency | Son ödeme para birimindeki miktarı atma (önceki ödemeler ödeniyorsa alan boş olacaktır) |
| lastPaymentCurrency | Son ödeme para birimi (önceki ödeme ödenene kadar alan boştur) |
| AssetId | Market hizmetiniz için müşteri siparişlerinin benzersiz tanımlayıcısı. Bu, satın alma satırı öğelerini temsil eder. Birden çok varlık olabilir. |
| OrderId | Müşterinin faturasıyla ilişkili |
| Lineıtemıd | Müşterinin faturasında tek satırlık |
| Müşteri ülkesi/bölgesi | Müşterinin sunduğu ülke/bölge adı. Bu, müşterinin Azure aboneliğindeki ülke/bölgeden farklı olabilir. |
| Müşteri EMAA adresi | Müşterinin sunduğu e-posta adresi. Bu, müşterinin Azure aboneliğindeki e-posta adresinden farklı olabilir. |
| SkuId | Yayımlama sırasında tanımlanan SKU KIMLIĞI. Bir teklifin birçok SKU 'su olabilir, ancak bir SKU yalnızca tek bir teklifle ilişkilendirilebilir. |

> [!NOTE]
> Transact yayımlama seçeneğine yönelik tüm raporlama ve Öngörüler, Iş Ortağı Merkezi 'nin analiz bölümünde bulunabilir.

## <a name="billing-questions-and-support"></a>Faturalandırma soruları ve destek
Faturalandırma desteği için ticari Market [Yayımcı desteği](https://partner.microsoft.com/support/v2/?stage=1)'ne başvurun.

## <a name="next-step"></a>Sonraki adım

- [Ödeme özetleri](./payout-summary.md)
