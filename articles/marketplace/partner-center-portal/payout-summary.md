---
title: Ticari Market ödeme Özeti | Azure Marketi
description: Ödeme Özeti, teklifiniz ile kazanabileceğiniz paradan ilgili ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınız ve ne kadar ücret ödetireceğiz size bilgi verir.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: guide
ms.date: 12/10/2019
ms.openlocfilehash: 3836e815d296456ff95f87f0592e3277503420fa
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427153"
---
# <a name="payout-reporting"></a>Ödeme raporlaması

[**Ödeme Özeti**](https://docs.microsoft.com/windows/uwp/publish/payout-summary) , Microsoft ile kazandığınızı gösteren ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınız ve ne kadar ücret ödetireceğiz size bilgi verir.

Azure Market 'te teklifler satmanız durumunda, **ödeme özetinde başarılı ödemeler**hakkındaki bilgileri de görürsünüz. Azure Marketi ödemesiyle ilgili daha fazla bilgi için [Microsoft Azure Market katılım ilkelerine](https://go.microsoft.com/fwlink/p/?LinkId=722436) ve [Microsoft Azure Market yayımcı anlaşmasına](https://go.microsoft.com/fwlink/p/?LinkID=699560)bakın.

> [!NOTE]
> Ödemenizin uygun olması için, devam eden $50 [ödeme eşiğine](payment-thresholds-methods-timeframes.md) ulaşmalıdır. Ödeme eşiği hakkında daha fazla bilgi için bu sayfaya bakın ve [Microsoft Azure Market yayımcı sözleşmesini](https://go.microsoft.com/fwlink/p/?LinkID=699560)gözden geçirin.

- [Ödeme raporuna erişim için roller ve izinler](#roles-and-permission-to-access-the-payout-report)
- [Ödeme raporu: Bulut İş Ortağı Portalı ve Iş ortağı merkezi arasındaki fark](#payout-report-difference-between-cloud-partner-portal-and-partner-center)
- [Müşteri türleri](#customer-types)
- [Ödeme ve kullanım arasındaki ortak ilişki](#corelation-between-payout-and-usage)
- [İşlem geçmişi indirmesi](#transaction-history-download-export)
- [Faturalandırma soruları ve destek](#billing-questions-and-support)

## <a name="roles-and-permission-to-access-the-payout-report"></a>Ödeme raporuna erişim için roller ve izinler

| Raporlar/sayfalar    | Hesap sahibi    | Yöneticisi  | Geliştirici | İş Katılımcısı |  Finans Katılımcısı | Pazarlamacısıdır |
|------------------|------------------|----------|-----------|----|----|-----|
| Alım raporu (neredeyse gerçek zaman verileri dahil) | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok |
| Geri bildirim raporu/yanıtları | Geri bildirim görüntüleyebilir ve gönderebilir | Geri bildirim görüntüleyebilir ve gönderebilir | Geri bildirim görüntüleyebilir ve gönderebilir | Erişim yok | Erişim yok | Geri bildirim görüntüleyebilir ve gönderebilir |
| Sistem durumu raporu (neredeyse gerçek zaman verileri dahil) | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Kullanım raporu | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Ödeme hesabı | Güncelleştirebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleştirebilir | Erişim yok |
| Vergi profili | Güncelleştirebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleştirebilir | Erişim yok |
| Ödeme özeti | Görüntüleyebilir | Erişim yok | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok |

## <a name="payout-report-difference-between-cloud-partner-portal-and-partner-center"></a>Ödeme raporu: Bulut İş Ortağı Portalı ve Iş ortağı merkezi arasındaki fark

| | Bulut İş Ortağı Portalı | İş Ortağı Merkezi |
|---------|---------|---------|
| Bağlantılar | https://cloudpartner.azure.com/ | https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory ve https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| Gezinme | Öngörüler ödeme için sunulan ödeme raporlaması | Iş Ortağı Merkezi – ödeme simgesinde sunulan ödeme raporlaması |
| Kapsam | <ul> <li>Her satır öğesi için işlem görünür, koleksiyon devam ediyor, toplandı ve ücretli olarak </li> <li>Raporlama – bir satınalma siparişi oluşturulduktan sonra, devam eden koleksiyon ve devam eden faturalama dahil olmak üzere tüm satır öğelerini ve henüz ödenmeyen koleksiyon durumu ve satır öğelerini gösterir. </li> </ul> | <ul> <li>Uygun kazanç olarak kabul edildikten sonra satır öğelerini gösterir.</li> <li>Müşteriler ilk olarak Microsoft 'a ödeme yapar ve ardından ISV 'Ler ödeme raporunu görebilir.</li> <li>Ödeme raporu, devam eden ve faturalandırma aşamasında koleksiyonu göstermez.  </li> </ul>  |
| İşlem ödeme için hazır değil | Faturalama devam ediyor | Sonraki tahmini ödeme: ödeme durumu işlenmemiş durumundadır.  |
| Ödeme durumu |  | İşlenmemiş <br> Kazanç ödeme için uygun. Bu durum, bir soğutma dönemi için, ister program kılavuzunda, ister program kılavuzumuza göre tanımlanan bu durumda kalır. <br> <br> İlerideki <br> Ödeme siparişi tarafından oluşturulan, ödeme işlenmeden önce bekleyen dahili incelemeler. <br> <br> Gönderilip <br> Ödeme, bankanızla gönderilmiştir. |

## <a name="customer-types"></a>Müşteri türleri 

### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

Yerinde Kurumsal Anlaşma olmayan müşteriler Market yazılım lisansları için aylık olarak faturalandırılır. Kurumsal Anlaşma olan müşteriler aylık olarak sunulan bir fatura aracılığıyla aylık olarak faturalandırılır.

### <a name="credit-cards-and-monthly-invoice"></a>Kredi kartları ve aylık fatura

Müşteriler, kredi kartı ve aylık fatura kullanarak da ödeme yapabilir. Bu durumda, yazılım lisans ücretleri aylık olarak faturalandırılır.

### <a name="csp-and-direct-pay-users"></a>CSP ve doğrudan ödeme kullanıcıları

Örneğin, müşteri kredi kartı kullanarak satın alıyorsa.

## <a name="corelation-between-payout-and-usage"></a>Ödeme ve kullanım arasındaki ortak ilişki 

|Açıklama    |    Tarih  | Siparişler/kullanım  | Ödeme |
|----------|----------|-----------|-------------|
|Sipariş dönemi   | 15 Ağu, 2019-30 Ağu, 2019 | **Bağıntı öznitelikleri siparişleri** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Kullanım** <br> <ul> <li>CustomerId </li> <li>Müşteri adı</li> <li>(UsageReference) PurchaseRecordId/Lineıtemıd</li> <li> Tahmini genişletilmiş ücret <br> Tahmini ödeme (PC) </li> </ul> |  |
|Dönem sonu (ay)   | 30 Ağu, 2019 | | |
|Faturalama tarihi | 1 Eylül 2019 | | |
|Müşteri ödeme tarihi | 1 Eylül 2019 | | |
|Emanet dönemi (yalnızca kredi kartları, 30 gün) | 1 Eylül 2019-Eyl 30, 2019 | | **Bağıntı öznitelikleri siparişleri:** <br> <ul><li>AssetId</li> <li>Müşteri Kimliği</li> <li> Müşteri adı</li> </ul> <br> **Kullanım** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Müşteri adı</li> <li>OrderId</li> <li>Lineıtemıd</li> <li>Işlem miktarı</li> <li>Earningamountınlastpaymentcurrency</li> </ul> <br> **Ödeme durumu:** İşlenmemiş |
|Toplama dönemi başlangıcı | 1 Eylül 2019 | | |
|Toplama dönemi sonu (maksimum, 30 gün) | 30 Eylül 2019 | | |
|Ödeme hesaplama tarihi (aylık 15 ' te) | 1 Eki, 2019 | | **Bağıntı öznitelikleri** <br> <ul><li>AssetId</li> <li>Müşteri Kimliği</li> <li>Müşteri adı</li> </ul> <br> **Kullanım** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Müşteri adı</li> <li>OrderId</li> <li>Lineıtemıd</li> <li>Işlem miktarı</li> <li>Earningamountınlastpaymentcurrency</li> </ul> <br> **Ödeme durumu:** Ilerideki |
|Ödeme tarihi | 15 Ekim 2019 | | **Bağıntı öznitelikleri** <br> <ul><li>AssetId</li> <li>Müşteri Kimliği</li> <li> Müşteri adı</li> </ul> <br> **Kullanım** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Müşteri adı</li> <li>OrderId</li> <li>Lineıtemıd</li> <li>Işlem miktarı</li> <li>Earningamountınlastpaymentcurrency</li> </ul> <br> **Ödeme durumu:** Ödeme gönderildi |

### <a name="enterprise-agreement-quarterlymonthly-customers"></a>Kurumsal Anlaşma (üç aylık/aylık müşteriler)

| Açıklama |    Tarih  | Kullanım | Ödeme |
|----------|----------|---------|-----------|
|Sipariş dönemi | 15 Ağu, 2019-30 Ağu, 2019 | **Bağıntı öznitelikleri siparişleri** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Kullanım raporu** <br> <ul> <li>CustomerId </li> <li>Müşteri adı</li> <li>(UsageReference) PurchaseRecordId/Lineıtemıd</li> <li> Tahmini genişletilmiş ücret <br> Tahmini ödeme (PC) </li> </ul> | |
|Dönem sonu (çeyrek) | 30 Eylül 2019 | | |
|Faturalama tarihi | 15 Ekim 2019 | | |
|Emanet dönemi (yalnızca kredi kartları, 30 gün) | Yok | | |
|Toplama dönemi başlangıcı | 15 Ekim 2019 | | |
|Yalnızca kredi kartları, 30 gün | 1 Kasım 2019-kas 30, 2019 | | |
|Toplama dönemi sonu (maksimum, 90 gün) | 15 Ocak 2020 | | |
|Müşteri ödeme tarihi | 30 Aralık 2019 | | |
|Ödeme hesaplaması | 15 Ocak 2020 | | |
|Ödeme tarihi | 15 Şubat 2020 | | **Üç aylık tabanlı müşteriler için** <br> <br> **Sipariş raporu** <br> <ul><li>AssetId</li> <li>Müşteri Kimliği</li> <li> Müşteri adı</li> </ul> <br> **Kullanım** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Müşteri adı</li> <li>OrderId</li> <li>Lineıtemıd</li> <li>Işlem miktarı</li> <li>Earningamountınlastpaymentcurrency</li> </ul> <br> **Ödeme durumu:** gönderildi |

## <a name="transaction-history-download-export"></a>İşlem geçmişi indirme dışarı aktarma

Bu seçenek, teşvikleri programı için geçerli olan Işlem geçmişi sayfasında gördüğünüz her bir atma satırı öğesinin bir indirisini, tür, tarih, ilişkili işlem miktarı, müşteri, ürün ve diğer işlem ayrıntılarını sağlar. 

| Sütun adı     | Açıklama    | 
|-------------|-------------------------------|
| Eardokgıd                      | Her bir atma için benzersiz tanımlayıcı                                                                                                       |
| participantId                  | İş ortağının birincil kimliği program altında                                                                            | 
| participantIdType              | Programları ve satıcı, mağaza programları ve Azure Marketi için genellikle program KIMLIĞI                                          | 
| participantName                | Kazanç ortağının adı                                                                                                              | 
| partnerCountryCode             | Kazanç ortağının konumu/ülkesi                                                                                                  |
| Programadı                    | Program adını teşvik edin/depolayın                                                                                                             | 
| transactionId                  | İşlem için benzersiz tanımlayıcı                                                                                                    | 
| Işlem para birimi            | Orijinal müşteri işleminin gerçekleştiği para birimi (iş ortağı konum para birimi değil)                                     | 
| transactionDate                | İşlemin tarihi. Birçok işlemin tek bir işleme katkıda bulunduğu programlar için yararlıdır                                           | 
| transactionExchangeRate        | Karşılık gelen işlem USD tutarını göstermek için kullanılan döviz kuru                                                                 | 
| Işlem miktarı              | Orijinal işlem para biriminde, kazanıştan oluşan işleme göre işlem miktarı                                              | 
| transactionAmountUSD           | USD cinsinden işlem miktarı                                                                                                                | 
| düzeyi                          | Kazanç için iş kuralını gösterir                                                                                                  | 
| Eardokgrate                    | Bir kazanç oluşturmak için işlem tutarına uygulanan teşvik oranı                                                                      | 
| miktar                       | Programa göre farklılık gösterir. İşlem programlarının faturalandırılan miktarını gösterir                                                            | 
| quantityType                   | Miktarın türünü gösterir, örneğin: faturalandırılan miktar, MAU                                                                                     |
| earningType                    | Ücret ödemesinin, indirimin, Coop, satış vb. olduğunu gösterir.                                                                                          | 
| earningAmount                  | Orijinal işlem para birimindeki miktarı atma                                                                                      |
| earningAmountUSD               | USD cinsinden miktar atma                                                                                                                    |
| earningDate                    | Kazanlama tarihi                                                                                                                      |
| calculationDate                | Sistemin sistemde hesaplandığı Tarih                                                                                            |
| earningExchangeRate            | Karşılık gelen USD tutarını göstermek için kullanılan döviz kuru                                                                                  |
| exchangeRateDate               | EarningAmount USD 'yi hesaplamak için kullanılan döviz kuru tarihi                                                                                   | 
| paymentAmountWOTax             | Yalnızca "gönderilen" ödemeler için para birimi ödeyerek miktar (vergi olmadan)                                                                 |
| paymentCurrency                | Ödeme profilinde iş ortağı tarafından seçilen para birimine öde. Yalnızca gönderilen ödemeler için gösteriliyor                                                   |
| paymentExchangeRate            | ExchangeRateDate kullanarak ödeme para birimindeki paymentAmountWOTax hesaplamak için kullanılan döviz kuru                                            |
| Paymentıd            | Ödeme için benzersiz tanımlayıcı. Bu sayı, banka ekstreninizde görülebilir                                            |
| paymentStatus            | Ödeme durumu                                            |
| paymentStatusDescription            | Ödeme durumunun kolay açıklaması                                            |
| customerId                     | Her zaman boş kalır                                                                                                                     |
| customerName                   | Her zaman boş kalır                                                                                                                     |
| partNumber                     | Her zaman boş kalır                                                                                                                     |
| productName                    | İşlemle bağlantılı ürün adı                                                                                                       |
| productId                      | Benzersiz ürün tanımlayıcısı                                                                                                                |
| Parentproductıd                | Benzersiz üst ürün tanımlayıcısı. Note: işlem için bir üst ürün yoksa, üst ürün KIMLIĞI = ürün KIMLIĞI. |
| parentProductName              | Ana ürünün adı. Note: işlem için bir üst ürün yoksa, üst ürün adı = ürün adı.   |
| productType                    | Ürün türü (uygulama, eklenti, oyun vb.)                                                                                        |
| Faturanumarası                  | Fatura numarası (yalnızca EA için geçerlidir)                                                                                                  |
| ResellerID                     | Satıcı tanımlayıcısı                                                                                                                      |
| Reselleradı                   | Satıcı adı                                                                                                                            |
| Işlem türü                | İşlem türü (satın alma, geri ödeme, ters çevirme, geri ödeme vb.)                                                               |
| Localprovidersatıcı            | Yerel sağlayıcı/kayıt satıcı                                                                                                          |
| Vergilenhavale edilmiş                    | Havale edilen vergi miktarı (satış, kullanım veya KDV/GST vergileri).                                                                                   |
| Taxhavalemodeli                  | Vergi vergisinden sorumlu olan parti (satış, kullanım veya KDV/GST vergileri).                                                                    |
| storeFee                       | Uygulamanın veya eklentinin mağazada kullanılabilir hale getirilmesi için Microsoft tarafından bir ücret karşılığında korunan miktar.                                            |
| transactionPaymentMethod       | İşlem için kullanılan müşteri ödeme gereci (kart, mobil taşıyıcı faturalandırma, PayPal vb.)                                |
| tpan                           | Üçüncü taraf ad ağını belirtir                                                                                                     |
| customerCountry                | Müşteri ülkesi                                                                                                                         |
| customerCity                   | Müşteri şehri                                                                                                                            |
| customerState                  | Müşteri durumu                                                                                                                           |
| Müşterzıp                    | Müşteri posta kodu                                                                                                                 |
| Değerine                       |                                                                                                                                          |
| Externalreferenceıd            | Program için benzersiz tanımlayıcı                                                                                                        |
| Externalreferenceıdlabel       | Benzersiz tanımlayıcı etiketi                                                                                                                  |
| transactionCountryCode       | İşlemin gerçekleştiği ülke kodu                                                                                                                  |
| taxCountry       | Müşterinin ülkesinde satılan                                                                                                                  |
| taxState       | Müşteri durumuna satıldı                                                                                                                  |
| Vergilencity       | Müşteri şehrine satışı                                                                                                                  |
| taxZipCode       | Müşteri posta kodu 'Na satılan                                                                                                                  |
| LicensingProgramName       |                                                                                                                   |
| Program kodu       | Program adıyla Eşlenecek dize                                                                                                                   |
| Earningamountınlastpaymentcurrency       | Son ödeme para biriminde (önceki ödemeler ödeniyorsa, alan boş olur) miktarı kazanlama                                                                                                                   |
| lastPaymentCurrency       | Son ödeme para birimi (önceki ödeme ödeniyorsa, alan boş olacaktır)                                                                                                                   |
| AssetId       | Market hizmetiniz için müşteri siparişlerinin benzersiz tanımlayıcısı.  Bu işlem, işlenen satın alma satırı öğelerini temsil eder. Birden çok varlık olabilir.                                                                                                                   |
| OrderId       | müşterinin faturasıyla ilişkili                                                                                                                   |
| Lineıtemıd       | müşterinin faturasında tek satırlık                                                                                                                   |
| Müşteri ülkesi       | Müşterinin sunduğu ülke adı.  Bu, müşterinin Azure aboneliğindeki ülkeden farklı olabilir.                                                                                                                   |
| Müşteri EMAA adresi       | Son müşteri tarafından sunulan e-posta adresi.  Bu, müşterinin Azure aboneliğindeki e-posta adresinden farklı olabilir.                                                                                                                   |
| SkuId       | Yayımlama sırasında tanımlanan SKU KIMLIĞI. Bir teklifin birçok SKU 'su olabilir, ancak bir SKU yalnızca tek bir teklifle ilişkilendirilebilir.                                                                                                                   |

>[!Note]
>Transact yayımlama seçeneğine yönelik tüm raporlama ve içgörüler, Iş Ortağı Merkezi 'nin Bulut İş Ortağı Portalı veya Analiz bölümünün Öngörüler bölümü aracılığıyla bulunabilir.

## <a name="billing-questions-and-support"></a>Faturalandırma soruları ve destek

Faturalandırma soruları hakkında yardım almak için lütfen [ticari Market yayımcı desteği](https://aka.ms/marketplacepublishersupport)'ne başvurun.
