---
title: Ayrıntılı kullanım ve ücretlerinizi anlayın | Microsoft Docs
description: Ayrıntılı kullanım ve ücretlerinizi nasıl okuyup anlayacağınızı öğrenin
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 66b54c027cde6341b23aef2c10b43fa21bf357da
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383456"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Azure kullanımı ve ücretler dosyanızdaki koşulları anlayın

Ayrıntılı kullanım ve ücretler dosyası, belirtilen dönem için anlaşmaya, satın alımlara (örneğin, Rezervasyonlar, Market ücretleri) ve para iadelerini temel alarak günlük olarak derecelendirilmiş kullanım içerir.
Ücretler krediler, vergiler veya diğer ücretleri ya da indirimleri içermez.
Aşağıdaki tablo her hesap türü için hangi ücretlerin dahil edileceğini kapsar.

Hesap türü | Azure kullanımı | Market kullanımı | Satın | İadelerini
--- | --- | --- | --- | ---
Kurumsal Anlaşma (EA) | Evet | Evet | Evet | Hayır
Microsoft Müşteri Sözleşmesi (MCA) | Evet | Evet | Evet | Evet
Kullandıkça Öde (PAYG) | Evet | Hayır | Hayır | Hayır

Market siparişleri hakkında daha fazla bilgi edinmek için (dış hizmetler olarak da bilinir) bkz. [Azure dış hizmet ücretlerinizi anlayın](billing-understand-your-azure-marketplace-charges.md).

İndirme yönergeleri için bkz. [Azure Faturalandırma faturanızı ve günlük kullanım verilerinizi alma](billing-download-azure-invoice-daily-usage-date.md).
Kullanım ve ücretler CSV dosyanızı Microsoft Excel veya başka bir elektronik tablo uygulamasında açabilirsiniz.

## <a name="list-of-terms-and-descriptions"></a>Hüküm ve açıklama listesi

Aşağıdaki tabloda, Azure kullanımı ve ücretler dosyasının en son sürümünde kullanılan önemli terimler açıklanmaktadır.
Liste, Kullandıkça Öde (PAYG), Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi (MCA) hesaplarını içerir.

Terim | Hesap türü | Açıklama
--- | --- | ---
Hesap adı | EA, PAYG | EA kayıt hesabının veya PAYG faturalandırma hesabının görünen adı.
Hesap sahibinin kimliği | EA, PAYG | EA kayıt hesabı veya PAYG faturalandırma hesabı için benzersiz tanımlayıcı.
Ek bilgi | Tümü | Hizmete özgü meta veriler. Örneğin, bir sanal makine için görüntü türü.
Billingaccountıd | Tümü | Kök faturalandırma hesabı için benzersiz tanımlayıcı.
BillingAccountName | Tümü | Faturalandırma hesabının adı.
BillingCurrency | Tümü | Faturalandırma hesabıyla ilişkili para birimi.
BillingPeriod | EA, PAYG | Ücretin fatura dönemi.
BillingPeriodEndDate | Tümü | Fatura döneminin bitiş tarihi.
BillingPeriodStartDate | Tümü | Fatura döneminin başlangıç tarihi.
Billingprofileıd | Tümü | EA kaydı, PAYG aboneliği, MCA Faturalandırma profili veya AWS birleştirilmiş hesabının benzersiz tanıtıcısı.
BillingProfileName | Tümü | EA kaydı, PAYG aboneliği, MCA Faturalandırma profili veya AWS birleştirilmiş hesabının adı.
ChargeType | Tümü | Ücretin kullanım (**kullanım**), satın alma (**satın alma**) veya para iadesi (**para iadesi**) temsil edip etmediğini gösterir.
Tüketilen hizmet | Tümü | Ücretle ilişkili hizmetin adı.
CostCenter | EA, MCA | İzleme maliyetleri için abonelik için tanımlanan maliyet merkezi (yalnızca MCA hesapları için açık faturalandırma dönemlerinde mevcuttur).
Maliyet | EA, PAYG | Bkz. Costınbillingcurrency.
Maliyetsiz para birimi | EXCEP | Kredi veya vergi öncesi faturalandırma para birimi cinsinden ücret ücreti.
Costınpricingcurrency | EXCEP | Kredi veya vergi öncesi fiyatlandırma para birimi cinsinden ücret ücreti.
Currency | EA, PAYG | Bkz. BillingCurrency.
Date | Tümü | Ücretin kullanım veya satın alma tarihi.
Efekt fiyatı | Tümü | Dönem için karıştırılan birim fiyatı. Karıştırılan fiyatlar, birim fiyatındaki dalgalanmaların ortalamasını artırır ve bu da süreyi, zaman içinde miktarı artar şekilde azaltır.
ExchangeRateDate | EXCEP | Döviz oranının kurulduğu tarih.
ExchangeRatePricingToBilling | EXCEP | Fiyatlandırma para birimindeki maliyeti faturalandırma para birimine dönüştürmek için kullanılan döviz kuru.
Sıklık | Tümü | Bir ücretin yinelenmesi beklenip yinelenmeyeceğini gösterir. Ücretler bir kez (**Onetime**), aylık veya yıllık olarak yinelenir (**yinelenen**) veya kullanımı (**usagebased**) temel alabilir.
Includedquantity | PAYG | Geçerli fatura döneminize ücretsiz olarak dahil edilen ölçüm miktarı.
Örnek kimliği | PAGY | Bkz. ResourceID.
InvoiceId | PAYG, MCA | Fatura PDF 'sinde listelenen benzersiz belge KIMLIĞI.
Invoicesection | EXCEP | Bkz. ınvoicesectionname.
Invoicesectionıd | EA, MCA | EA Departmanı veya MCA Invoice bölümü için benzersiz tanımlayıcı.
Invoicesectionname | EA, MCA | EA Departmanı veya MCA Invoice bölümünün adı.
IsAzureCreditEligible | Tümü | Azure kredilerinin kullanımı için ücretin ödeme için uygun olup olmadığını gösterir (değerler: True, false).
Location | EXCEP | Kaynağın çalıştığı veri merkezi konumu.
Ölçüm kategorisi | Tümü | Ölçüm için sınıflandırma kategorisinin adı. Örneğin, *bulut hizmetleri* ve *ağ*.
Ölçüm kimliği | Tümü | Ölçüm için benzersiz tanımlayıcı.
Ölçüm adı | Tümü | Ölçerin adı.
Ölçüm bölgesi | Tümü | Konuma göre fiyatlandırılır Hizmetleri için veri merkezi konumunun adı. Bkz. konum.
Ölçüm alt kategorisi | Tümü | Ölçüm alt sınıflandırma kategorisinin adı.
OfferId | Tümü | Satın alınan teklifin adı.
PartNumber | EA, PAYG | Belirli ölçüm fiyatlandırmasını almak için kullanılan tanımlayıcı.
PlanName | EA, PAYG | Market planı adı.
Previousıniceıd | EXCEP | Bu satır öğesi bir para iadesi ise orijinal faturaya başvuru.
PricingCurrency | EXCEP | Anlaşmalı fiyatlara göre derecelendirirken kullanılan para birimi.
Ürün | Tümü | Ürünün adı.
Ürün kimliği | EXCEP | Ürün için benzersiz tanımlayıcı.
ProductOrderId | Tümü | Ürün siparişi için benzersiz tanımlayıcı.
ProductOrderName | Tümü | Ürün siparişi için benzersiz ad.
PublisherName | Tümü | Market Hizmetleri için Yayımcı.
PublisherType | Tümü | Yayımcı türü (değerler: **Azure**, **AWS**, **Market**).
Miktar | Tümü | Satın alınan veya tüketilen birim sayısı.
Reservationıd | EA, MCA | Satın alınan ayırma örneği için benzersiz tanımlayıcı.
Rezervadı | EA, MCA | Satın alınan rezervasyon örneğinin adı.
ResourceGroup | Tümü | Kaynağın bulunduğu [kaynak grubunun](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) adı.
RESOURCEID | Tümü | [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) kaynağının benzersiz tanıtıcısı.
Kaynak konumu | Tümü | Kaynağın çalıştığı veri merkezi konumu. Bkz. konum.
ResourceName | EA, PAYG | Kaynağın adı.
KaynakTürü | EXCEP | Kaynak örneğinin türü.
ServiceFamily | EXCEP | Hizmetin ait olduğu hizmet ailesi.
Hizmet bilgisi 1 | Tümü | Hizmete özgü meta veriler.
Hizmet bilgisi 2 | Tümü | İsteğe bağlı hizmete özgü meta verileri olan eski alan.
ServicePeriodEndDate | EXCEP | Tüketilen veya satın alınan hizmete yönelik fiyatlandırma ve kilitleme döneminin bitiş tarihi.
ServicePeriodStartDate | EXCEP | Tüketilen veya satın alınan hizmet için tanımlanan ve kilitlenen fiyatlandırma döneminin başlangıç tarihi.
SubscriptionId | Tümü | Azure aboneliği için benzersiz tanımlayıcı.
Abonelik adı | Tümü | Azure aboneliğinin adı.
Tags | Tümü | Kaynağa atanan Etiketler. Kaynak grubu etiketlerini içermez. , İç geri ödeme için maliyetleri gruplamak veya dağıtmak için kullanılabilir. Daha fazla bilgi için bkz. [Azure kaynaklarınızı etiketlerle düzenleme](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Ölçü birimi | Tümü | Hizmetin faturalandırılması için ölçü birimi. Örneğin, işlem hizmetleri saat başına faturalandırılır.
Fiyatı | EA, PAYG | Ücret için birim başına fiyat.

Note bazı alanlar, hesap türleri arasında büyük küçük harf ve aralığa göre farklılık gösterebilir.
Kullandıkça Öde kullanım dosyalarının eski sürümlerinde, bildirim ve günlük kullanım için ayrı bölümler vardır.

### <a name="list-of-terms-from-older-apis"></a>Eski API 'lerden koşullar listesi
Aşağıdaki tabloda, eski API 'lerde kullanılan terimler yeni koşullara eşlenir. Bu açıklamalar için yukarıdaki tabloya bakın.

Eski dönem | Yeni terim
--- | ---
Tüketilen miktar | Miktar
Includedquantity | Yok
Örnek kimliği | RESOURCEID
Fiyat | Efekt fiyatı
Birim | Ölçü birimi
UsageDate | Date
UsageEnd | Date
UsageStart | Date


## <a name="ensure-charges-are-correct"></a>Ücretlerin doğru olduğundan emin olun

Ayrıntılı kullanım ve ücretler hakkında daha fazla bilgi edinmek için [Kullandıkça Öde](./billing-understand-your-bill.md) veya [Microsoft müşteri anlaşması](billing-mca-understand-your-bill.md) faturanızı nasıl anlayacağınızı okuyun.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Azure faturanızı görüntüleyin ve indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleyin ve indirin](billing-download-azure-daily-usage.md)
