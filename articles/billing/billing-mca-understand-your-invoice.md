---
title: Azure 'da Microsoft müşteri anlaşması faturanızı anlayın
description: Azure 'da Microsoft müşteri anlaşması faturanızı okumayı ve anlamayı öğrenin
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: a5f77120c1d4e8a6721f3bc207132bee19a7772f
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383543"
---
# <a name="terms-in-your-microsoft-customer-agreement-invoice"></a>Microsoft müşteri anlaşması faturanızda şartlar

Bu makale, bir Microsoft Müşteri Sözleşmesi için Azure Faturalandırma hesabına yöneliktir. [Bir Microsoft Müşteri sözleşmesine erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

Faturanızda, ücretlerinizin ve ödeme talimatlarınızın bir özeti sağlanır. [Azure Portal](https://portal.azure.com/) taşınabilir belge biçimi 'nde (. PDF) indirilebilir veya e-posta yoluyla gönderilebilir. Daha fazla bilgi için, bkz. [Microsoft Azure faturanızı görüntüleme ve indirme](billing-download-azure-invoice.md).

## <a name="billing-period"></a>Faturalama dönemi

Aylık olarak faturalandırılır. [Azure Portal](https://portal.azure.com/)faturalama profili özellikleri altında *Fatura tarihi* ' ni denetleyerek, hangi ayın hangi gününe de fatura alacağını öğrenebilirsiniz. Fatura döneminin bitişi ile fatura tarihi arasında gerçekleşen ücretler bir sonraki fatura döneminde olduklarından, sonraki ay faturasına dahil edilir. Her bir faturaya ait fatura dönemi başlangıç ve bitiş tarihleri, **faturalandırma özetinin**ÜZERINDEKI fatura PDF dosyasında listelenir.

## <a name="invoice-terms-and-descriptions"></a>Fatura hüküm ve açıklamaları

Aşağıdaki bölümlerde, faturanızda gördüğünüz önemli terimler ve her dönem için açıklama sağlanmaktadır.

### <a name="invoice-summary"></a>Fatura Özeti

**Fatura Özeti** ilk sayfanın en üstünde bulunur ve faturalandırma profiliniz hakkındaki bilgileri ve ödeme yapma şeklini gösterir.

![Fatura Özeti bölümü](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Terim | Açıklama |
| --- | --- |
| Kime Satıldı |Faturalandırma hesabı özelliklerinde bulunan yasal varlığınızın adresi|
| Fatura adresi |Faturalandırma profili özelliklerinde bulunan, faturayı alan faturalandırma profilinin fatura adresi|
| Faturalama Profili |Faturayı alan Faturalandırma profili adı |
| P.O. numarası |İzleme için size atanan isteğe bağlı bir satınalma siparişi numarası |
| Fatura numarası |İzleme amacıyla kullanılan benzersiz, Microsoft tarafından üretilmiş bir fatura numarası |
| Fatura tarihi |Faturanın oluşturulduğu tarih, genellikle fatura döngüsünün sonundan sonra beş ile 12 gün sonra. Fatura tarihi ' ni Faturalandırma profili özellikleri ' nde kontrol edebilirsiniz.|
| Ödeme koşulları |Microsoft faturanız için ödeme yapma. *Net 30 gün* , fatura tarihinin 30 gün içinde ödeme yaptığınız anlamına gelir. |

### <a name="billing-summary"></a>Faturalandırma Özeti

**Fatura Özeti** , önceki faturalama döneminden bu yana, uygulanan tüm krediler, vergi ve toplam tutarının ödenmesi için faturalandırma profiline karşı ücretleri gösterir.

![Faturalandırma Özeti bölümü](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Terim | Açıklama |
| --- | --- |
| Ücretler|Bu Faturalandırma profili için son faturalandırma döneminden bu yana olan toplam Microsoft ücreti sayısı |
| Jenerik |Dönüşlerden aldığınız krediler |
| Azure kredileri uygulandı | Her faturalandırma döneminde Azure ücretlerine otomatik olarak uygulanan Azure kredileri |
| Alt toplam |Ödenecek ön vergi tutarı |
| Vergi |Faturalandırma profilinizin ülkesine/bölgesine bağlı olarak, ödeyolduğunuz verginin türü ve miktarı. Vergiyi ödemeniz gerekmiyorsa faturanızda vergiyi görmezsiniz. |
| Tahmini toplam tasarruf |Etkin indirimlerden kaydettiğiniz tahmini toplam tutar. Geçerliyse, etkin indirimli fiyatlar, fatura tarafından Ayrıntılar bölümündeki satın alma satırı öğeleri altında listelenir. |

### <a name="invoice-sections"></a>Fatura bölümleri

Fatura profilinizin altındaki her fatura bölümü için ücretleri, uygulanan Azure kredilerinin miktarını, vergiyi ve toplam tutarı görürsünüz.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Fatura bölümüne göre Ayrıntılar

Ayrıntılar, her fatura için ürün sırasına göre ayrılmış maliyeti gösterir. Her ürün sırasında maliyet, hizmet türüne göre bölünür. Ürün ve hizmetlerinize yönelik günlük ücretleri, Azure portal ve Azure kullanımı ile CSV ve ücretler CSV ' de bulabilirsiniz. Daha fazla bilgi edinmek için bkz. [Microsoft Müşteri Sözleşmesi için faturanızda ücretleri anlayın](billing-mca-understand-your-bill.md).

Her bir hizmet ailesi için ödenecek toplam tutar *krediler/ücretler* üzerinden *Azure kredileri* çıkarılarak ve *vergi*eklenerek hesaplanır:


![Fatura bölümüne göre Ayrıntılar](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Terim |Açıklama |
| --- | --- |
| Birim fiyatı | Hizmetin kullanım oranını derecelendirmek için kullanılan geçerli birim fiyatı (fiyatlandırma para birimi cinsinden). Bu bir ürün, hizmet ailesi, ölçüm ve teklif için benzersizdir. |
| Adet | Fatura dönemi boyunca satın alınan veya tüketilen miktar |
| Ücretler/krediler | Jenerik/para iadesi uygulandıktan sonra net ücret miktarı |
| Azure Kredisi | Ücretlere/kredilerine uygulanan Azure kredilerinin miktarı|
| Vergi oranı | Ülke/bölgeye bağlı olarak vergi oranı |
| Vergi tutarı | Vergi oranına göre satın alma için uygulanan vergi miktarı |
| Toplam | Satınalmada kaynaklanan toplam tutar |

### <a name="how-to-pay"></a>Ödeme yapma

Faturanın en altında faturanızı ödeyerek ilgili yönergeler vardır. Çek, hat veya çevrimiçi ile ödeme yapabilirsiniz. Çevrimiçi ödeme yaparsanız, varsa kredi kartı veya Azure kredilerini kullanabilirsiniz.

### <a name="publisher-information"></a>Yayımcı bilgileri

Faturanızda üçüncü taraf hizmetleriniz varsa, her yayımcının adı ve adresi faturanızda en altında listelenir.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesine erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Faturalandırma profilinizin faturasındaki ücretleri anlayın](billing-mca-understand-your-bill.md)
- [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](billing-ea-pricing.md)
- [Faturalandırma profiliniz için vergi belgelerini görüntüleyin](billing-mca-download-tax-document.md)
