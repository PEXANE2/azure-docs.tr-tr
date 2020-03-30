---
title: Ticari pazar ödeme özeti | Azure Marketi
description: Ödeme özeti, teklifinizle kazandığınız parayla ilgili ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınızı ve ne kadar ödeme alacağınızı da bildirir.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/10/2019
ms.openlocfilehash: 678dc8b058d0ae0694dafeb4222b2fc9f10ecda7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288708"
---
# <a name="payout-reporting"></a>Ödeme raporlama

[**Ödeme özeti,**](https://docs.microsoft.com/windows/uwp/publish/payout-summary) Microsoft ile kazandığınız parayla ilgili ayrıntıları gösterir. Ayrıca, ödemeleri ne zaman alacağınızı ve ne kadar ödeme alacağınızı da bildirir.

Azure Marketi'nde teklif satıyorsanız, **Ödeme özetinde**başarılı ödemeler hakkındaki bilgileri de görürsünüz. Azure Marketi ödemesi ile ilgili daha fazla bilgi için [Microsoft Azure Marketi Katılım İlkeleri](https://go.microsoft.com/fwlink/p/?LinkId=722436) ve [Microsoft Azure Marketi Yayımcı Sözleşmesi'ne](https://go.microsoft.com/fwlink/p/?LinkID=699560)bakın.

> [!NOTE]
> Ödeme için uygun olmak için, gelirleri 50 $ [ödeme eşiğine](payment-thresholds-methods-timeframes.md) ulaşması gerekir. Ödeme eşiği yle ilgili ayrıntılar için bu sayfaya bakın ve [Microsoft Azure Marketi Yayımcı Sözleşmesi'ni](https://go.microsoft.com/fwlink/p/?LinkID=699560)inceleyin.

- [Ödemeler ve ödeme raporuna erişim izni](#roles-and-permission-to-access-the-payout-report)
- [Ödeme raporu: Bulut İş Ortağı Portalı ile İş Ortağı Merkezi arasındaki fark](#payout-report-difference-between-cloud-partner-portal-and-partner-center)
- [Müşteri tipleri](#customer-types)
- [Ödeme ve kullanım arasındaki birlikte ilişki](#corelation-between-payout-and-usage)
- [İşlem geçmişi indir](#transaction-history-download-export)
- [Faturalama soruları ve desteği](#billing-questions-and-support)

## <a name="roles-and-permission-to-access-the-payout-report"></a>Ödemeler ve ödeme raporuna erişim izni

| Raporlar / Sayfalar    | Hesap sahibi    | Yönetici  | Geliştirici | İş katkıda bulunan |  Finans katılımcısı | Pazarlamacı |
|------------------|------------------|----------|-----------|----|----|-----|
| Satın alma raporu (Yakın Gerçek Zamanlı veriler dahil) | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok |
| Geri bildirim raporu/yanıtları | Görüşebilir ve geri bildirim gönderebilir | Görüşebilir ve geri bildirim gönderebilir | Görüşebilir ve geri bildirim gönderebilir | Erişim yok | Erişim yok | Görüşebilir ve geri bildirim gönderebilir |
| Sağlık raporu (neredeyse gerçek zamanlı veriler dahil) | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Kullanım raporu | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Görüntüleyebilir | Erişim yok | Erişim yok |
| Ödeme hesabı | Güncelleyebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleyebilir | Erişim yok |
| Vergi profili | Güncelleyebilir | Erişim yok | Erişim yok | Erişim yok | Güncelleyebilir | Erişim yok |
| Ödeme özeti | Görüntüleyebilir | Erişim yok | Erişim yok | Erişim yok | Görüntüleyebilir | Erişim yok |

## <a name="payout-report-difference-between-cloud-partner-portal-and-partner-center"></a>Ödeme raporu: Bulut İş Ortağı Portalı ile İş Ortağı Merkezi arasındaki fark

| | Bulut İş Ortağı Portalı | Ortak Merkezi |
|---------|---------|---------|
| Bağlantılar | [https://cloudpartner.azure.com/](https://cloudpartner.azure.com) | [https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory](https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory)Ve[https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments](https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments) |
| Gezinti | Insights Payout'ta sağlanan ödeme raporlaması | Ortak Merkezi'nde sağlanan ödeme raporlaması – Payout Icon |
| Kapsam | <ul> <li>Satır öğesi başına işlem, devam eden, toplanan ve ödenen tahsilat için görünür </li> <li>Raporlama – devam eden tahsilat ve devam eden faturalandırma ve henüz ödenmeye uygun olmayan tahsilat durumu ve satır öğeleri de dahil olmak üzere satınalma siparişi oluşturulduktan sonra tüm satır öğelerini gösterir. </li> </ul> | <ul> <li>Uygun kazanç olarak kabul edildikten sonra satır öğelerini gösterir.</li> <li>Müşteriler önce Microsoft'a ödeme yaparsa, ardından ISV'ler ödeme raporunun başladığını görebilir.</li> <li>Ödeme raporu, devam eden tahsilat ve devam eden faturalandırmayı göstermez.  </li> </ul>  |
| Ödeme için hazır olmayan işlem | Devam Eden Faturalandırma | Sonraki tahmini ödeme: Ödeme durumu işlenmemiş durumdadır.  |
| Ödeme durumu |  | Işlenme -miş: <br> Kazanç ödeme için uygundur. Teşvik programı için program kılavuzunda tanımlandığı gibi bir soğutma dönemi için bu durumda kalır. <br> <br> Yaklaşan: <br> Ödeme işlenmeden önce ödeme emri tarafından oluşturulan bekleyen dahili incelemeler. <br> <br> Gönderilen: <br> Ödeme bankanıza gönderildi. |

## <a name="customer-types"></a>Müşteri tipleri

### <a name="enterprise-agreement"></a>Kurumsal anlaşma

Kurumsal Sözleşmesi olan müşteriler, pazar yeri yazılım lisansları için aylık olarak faturalandırılır. Kurumsal Sözleşmesi olan müşteriler, üç ayda bir sunulan bir fatura yla aylık olarak faturalandırılır.

### <a name="credit-cards-and-monthly-invoice"></a>Kredi kartları ve aylık fatura

Müşteriler kredi kartı ve aylık fatura kullanarak da ödeme yapabilir. Bu durumda, yazılım lisans ücretleriniz aylık olarak faturalandırılır.

### <a name="csp-and-direct-pay-users"></a>CSP ve Doğrudan Ödeme Kullanıcıları

Örneğin, müşteri kredi kartı kullanarak alışveriş yapıyorsa.

## <a name="corelation-between-payout-and-usage"></a>Ödeme ve kullanım arasındaki birlikte ilişki

|Açıklama    |    Tarih  | Siparişler / Kullanım  | Ödeme |
|----------|----------|-----------|-------------|
|Sipariş Dönemi   | 15 Ağustos 2019 - 30 Ağustos 2019 | **Korelasyon Öznitelikleri Siparişler** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Kullanım** <br> <ul> <li>CustomerId </li> <li>Müşteri Adı</li> <li>(Kullanım Başvurusu) Satın AlmaRecordId/LineItemid</li> <li> Tahmini Genişletilmiş Şarj <br> Tahmini Ödeme (PC) </li> </ul> |  |
|Dönem Sonu (ay)   | 30 Ağustos 2019 | | |
|Fatura Tarihi | 1 Eylül 2019 | | |
|Müşteri Ödeme Tarihi | 1 Eylül 2019 | | |
|Emanet Dönemi (sadece kredi kartları, 30 gün) | 1 Eylül 2019 - 30 Eylül 2019 | | **Korelasyon Öznitelikleri Siparişler:** <br> <ul><li>Varlık Kimliği</li> <li>Müşteri Kimliği</li> <li> Müşteri Adı</li> </ul> <br> **Kullanım** <br> <ul> <li>Varlık Kimliği</li> <li>CustomerId</li> <li>Müşteri Adı</li> <li>OrderId</li> <li>LineItemId</li> <li>işlemTutar</li> <li>Kazanç TutarıInLast Ödeme Para Birimi</li> </ul> <br> **Ödeme Durumu:** Işlenme -miş |
|Tahsilat Dönemi Başlangıcı | 1 Eylül 2019 | | |
|Tahsilat Dönemi Sonu (en fazla 30 gün) | 30 Eylül 2019 | | |
|Ödeme Hesaplama Tarihi (ayın 15'inde aylık) | Eki 1, 2019 | | **Korelasyon Özellikleri** <br> <ul><li>Varlık Kimliği</li> <li>Müşteri Kimliği</li> <li>Müşteri Adı</li> </ul> <br> **Kullanım** <br> <ul> <li>Varlık Kimliği</li> <li>CustomerId</li> <li>Müşteri Adı</li> <li>OrderId</li> <li>LineItemId</li> <li>işlemTutar</li> <li>Kazanç TutarıInLast Ödeme Para Birimi</li> </ul> <br> **Ödeme Durumu:** Yaklaşan |
|Ödeme Tarihi | Eki 15, 2019 | | **Korelasyon Özellikleri** <br> <ul><li>Varlık Kimliği</li> <li>Müşteri Kimliği</li> <li> Müşteri Adı</li> </ul> <br> **Kullanım** <br> <ul> <li>Varlık Kimliği</li> <li>CustomerId</li> <li>Müşteri Adı</li> <li>OrderId</li> <li>LineItemId</li> <li>işlemTutar</li> <li>Kazanç TutarıInLast Ödeme Para Birimi</li> </ul> <br> **Ödeme Durumu:** Gönderilen ödeme |

### <a name="enterprise-agreement-quarterlymonthly-customers"></a>Kurumsal sözleşme (üç aylık/aylık müşteriler)

| Açıklama |    Tarih  | Kullanım | Ödeme |
|----------|----------|---------|-----------|
|Sipariş Dönemi | 15 Ağustos 2019 - 30 Ağustos 2019 | **Korelasyon öznitelikleri siparişleri** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Kullanım raporu** <br> <ul> <li>CustomerId </li> <li>Müşteri Adı</li> <li>(Kullanım Başvurusu) Satın AlmaRecordId/LineItemid</li> <li> Tahmini Genişletilmiş Şarj <br> Tahmini Ödeme (PC) </li> </ul> | |
|Dönem Bitişi (üç aylık dönem) | 30 Eylül 2019 | | |
|Fatura Tarihi | Eki 15, 2019 | | |
|Emanet Dönemi (sadece kredi kartları, 30 gün) | yok | | |
|Tahsilat Dönemi Başlangıcı | Eki 15, 2019 | | |
|Sadece kredi kartları, 30 gün | 1 Kasım 2019 - 30 Kasım 2019 | | |
|Tahsilat Dönemi Sonu (maksimum, 90 gün) | Ocak 15, 2020 | | |
|Müşteri Ödeme Tarihi | Aralık 30, 2019 | | |
|Ödeme Hesaplama | Ocak 15, 2020 | | |
|Ödeme Tarihi | Şub 15, 2020 | | **Üç aylık tabanlı müşteriler için** <br> <br> **Sipariş raporu** <br> <ul><li>Varlık Kimliği</li> <li>Müşteri Kimliği</li> <li> Müşteri Adı</li> </ul> <br> **Kullanım** <br> <ul> <li>Varlık Kimliği</li> <li>CustomerId</li> <li>Müşteri Adı</li> <li>OrderId</li> <li>LineItemId</li> <li>işlemTutar</li> <li>Kazanç TutarıInLast Ödeme Para Birimi</li> </ul> <br> **Ödeme Durumu:** gönderildi |

## <a name="transaction-history-download-export"></a>İşlem geçmişi indirme dışa aktarma

Bu seçenek, İşlem geçmişi sayfasında gördüğünüz her kazanç satır öğesinin karşıdan yüklenmesini, kazanç türünü, tarihi, ilişkili işlem tutarını, müşteriyi, ürünü ve Teşvikler programı için geçerli olan diğer işlem ayrıntılarını sağlar.

| Sütun adı     | Açıklama    |
|-------------|-------------------------------|
| kazançId                      | Her kazanç için benzersiz tanımlayıcı                                                                                                       |
| katılımcıKimliği                  | Program kapsamında kazanç elde eden ortağın birincil kimliği                                                                            |
| katılımcıIdType              | Mağaza programları ve Azure Marketi için Teşvik programları ve Satıcı IF için çoğunlukla program kimliği                                          |
| katılımcıAdı                | Kazanan ortağın adı                                                                                                              |
| partnerCountryCode             | Kazanç ortağının konumu/ülkesi                                                                                                  |
| programAdı                    | Teşvik/mağaza program adı                                                                                                             |
| Transactionıd                  | Hareket için benzersiz tanımlayıcı                                                                                                    |
| işlemPara Birimi            | Özgün müşteri hareketinin gerçekleştiği para birimi (ortak konum para birimi değildir)                                     |
| işlemTarihi                | İşlemtarihi. Birçok işlemin bir kazanç için katkıda bulunduğu programlar için yararlıdır                                           |
| işlemExchangeRate        | İlgili işlem USD tutarını göstermek için kullanılan döviz kuru                                                                 |
| işlemTutar              | Kazancın oluşturulduğu temele bağlı olarak orijinal hareket para birimindeki hareket tutarı                                              |
| işlemTutarUSD           | USD'deki işlem tutarı                                                                                                                |
| Kolu                          | Kazanç için iş kuralını gösterir                                                                                                  |
| kazançOranı                    | Kazanç elde etmek için işlem tutarına uygulanan teşvik oranı                                                                      |
| miktar                       | Programa göre değişir. Hareket programları için faturalanan miktarı gösterir                                                            |
| miktarTip                   | Miktar türünü gösterir, örneğin: Faturalı miktar, MAU                                                                                     |
| kazançTürü                    | Ücret, indirim, kümes, satış vb. olup olmadığını gösterir.                                                                                          |
| kazançTutar                  | Orijinal hareket para biriminde Kazanç Tutarı                                                                                      |
| kazançTutarUSD               | USD Kazanç Tutarı                                                                                                                    |
| kazançTarihi                    | Kazanç tarihi                                                                                                                      |
| hesaplamaTarihi                | Kazancın sistemde hesaplandığı tarih                                                                                            |
| kazançExchangeRate            | İlgili USD tutarını göstermek için kullanılan döviz kuru                                                                                  |
| exchangeRateDate               | Kazanç Tutarı USD'yi hesaplamak için kullanılan döviz kuru tarihi                                                                                   |
| ödemeAmountWOTax             | Yalnızca "Gönderilen" ödemeler için Ödeme Para Birimi'nde kazanç tutarı (vergi olmadan)                                                                 |
| ödemePara Birimi                | Ödeme profilinde ortak tarafından seçilen para birimine ödeme. Yalnızca gönderilen ödemeler için gösterilir                                                   |
| paymentExchangeRate            | ExchangeRateDate kullanarak ödeme para birimi cinsinden ödemeAmountWOTax hesaplamak için kullanılan döviz kuru                                            |
| paymentId            | Ödeme için benzersiz tanımlayıcı. Bu numara banka ekstrenizde görünür                                            |
| ödemeDurum            | Ödeme durumu                                            |
| ödemeStatusDescription            | Ödeme durumunun dostça açıklaması                                            |
| customerId                     | Her zaman boş olacak                                                                                                                     |
| Müşteriadı                   | Her zaman boş olacak                                                                                                                     |
| partNumber                     | Her zaman boş olacak                                                                                                                     |
| Productname                    | İşleme bağlı ürün adı                                                                                                       |
| productId                      | Benzersiz ürün tanımlayıcısı                                                                                                                |
| parentProductId                | Benzersiz ana ürün tanımlayıcısı. Not: Hareket için bir ana ürün yoksa, Ana Ürün Kimliği = Ürün Kimliği. |
| parentProductName              | Ana ürünün adı. Not: Hareket için bir ana ürün yoksa, Ana Ürün Adı = Ürün Adı.   |
| productType                    | Ürün türü (Uygulama, Eklenti, Oyun, vb.)                                                                                        |
| faturaNumarası                  | Fatura numarası (yalnızca EA için geçerlidir)                                                                                                  |
| bayiId                     | Bayi tanımlayıcısı                                                                                                                      |
| bayiAd                   | Satıcı adı                                                                                                                            |
| Transactiontype                | İşlem türü (satın alma, geri ödeme, geri alma, ters ibraz, ters ibraz, vb.)                                                               |
| localProviderSeller            | Yerel sağlayıcı/kayıt satıcısı                                                                                                          |
| taxRemitted                    | Havale edilen vergi tutarı (satış, kullanım veya KDV/GST vergileri).                                                                                   |
| taxRemitModel                  | Vergilerin (satış, kullanım veya KDV/GST vergileri) havale ile yükümlü olduğu taraf.                                                                    |
| mağazaÜcreti                       | Microsoft tarafından uygulama veya eklentiyi Mağaza'da kullanılabilir hale getirmek için bir ücret olarak tutulan tutar.                                            |
| işlemPaymentMethod       | İşlem için kullanılan müşteri ödeme aracı (Kart, Mobil Operatör Faturası, PayPal, vb.)                                |
| tpan                           | Üçüncü taraf reklam ağını gösterir                                                                                                     |
| customerCountry                | Müşteri ülkesi                                                                                                                         |
| customerCity                   | Müşteri şehri                                                                                                                            |
| customerState                  | Müşteri durumu                                                                                                                           |
| müşteriZip                    | Müşteri posta/posta kodu                                                                                                                 |
| Kiracı Kimliği                       |                                                                                                                                          |
| externalReferenceId            | Program için benzersiz tanımlayıcı                                                                                                        |
| externalReferenceIdLabel       | Benzersiz tanımlayıcı etiketi                                                                                                                  |
| işlemCountryCode       | İşlemin gerçekleştiği Ülke Kodu                                                                                                                  |
| taxCountry       | Müşteri Ülkesine Satıldı                                                                                                                  |
| taxState       | Müşteri Durumuna Satıldı                                                                                                                  |
| taxŞehir       | Müşteri Şehrine Satıldı                                                                                                                  |
| vergiZipCode       | Müşteri Zip'e Satıldı                                                                                                                  |
| LisansLamaProgramName       |                                                                                                                   |
| Program Kodu       | Program adı ile eşlenecek dize                                                                                                                   |
| kazançAmountInLastPaymentCurrency       | Son ödeme para biriminde kazanç tutarı (ön ödeme ödenmemişse alan boş olacaktır)                                                                                                                   |
| lastPaymentCurrency       | Son ödeme para birimi (ön ödeme ödenmemişse alan boş olacaktır)                                                                                                                   |
| Varlık Kimliği       | Pazar yeri hizmetiniz için müşteri siparişleri için benzersiz tanımlayıcı.  İşlenen satınalma satırı öğelerini temsil eder. Birden fazla varlık olabilir.                                                                                                                   |
| OrderId       | müşterinin faturasıyla ilgilidir                                                                                                                   |
| LineItemId       | müşterinin faturasında tek tek satır                                                                                                                   |
| Müşteri Ülkesi       | Müşteri tarafından sağlanan ülke adı.  Bu, bir müşterinin Azure Aboneliği'ndeki ülkeden farklı olabilir.                                                                                                                   |
| Müşteri E-posta Adresi       | Son müşteri tarafından sağlanan e-posta adresi.  Bu, müşterinin Azure Aboneliği'ndeki e-posta adresinden farklı olabilir.                                                                                                                   |
| SkuId       | Yayımlama sırasında tanımlandığı şekilde SKU Kimliği. Bir teklifin birçok SKU'su olabilir, ancak SKU yalnızca tek bir teklifle ilişkilendirilebilir.                                                                                                                   |

>[!Note]
>İşleme yayıncılık seçeneğine ilişkin tüm raporlama ve öngörülere, İş Ortağı Merkezi'nin Bulut İş Ortağı Portalı veya Analytics bölümünün Öngörüler bölümünden ulaşabilirsiniz.

## <a name="billing-questions-and-support"></a>Faturalama soruları ve desteği

Faturalandırma sorularında yardım almak için lütfen [ticari pazar yeri yayıncı desteğine](https://aka.ms/marketplacepublishersupport)başvurun.
