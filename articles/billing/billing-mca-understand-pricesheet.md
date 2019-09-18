---
title: Microsoft Müşteri Sözleşmesi için fiyat listenizdeki koşulları anlama - Azure
description: Microsoft Müşteri Sözleşmesinin kullanımını ve faturasını okuma ve anlama hakkında bilgi edinin.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 4d83228fbec395d604e5ce3f988d2a6157f21eed
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490659"
---
# <a name="terms-in-your-microsoft-customer-agreement-price-sheet"></a>Microsoft Müşteri Sözleşmesi fiyat listenizdeki koşullar

Bu makale, Microsoft Müşteri Sözleşmesi’ne ilişkin Azure ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

Faturalama profili Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura Yöneticisi iseniz kuruluşunuzun fiyat listesini Azure portalından indirebilirsiniz. Bkz. [Kuruluşunuzun fiyatlandırmasını görüntüleme ve indirme](billing-ea-pricing.md).

## <a name="terms-and-descriptions-in-your-price-sheet"></a>Fiyat listenizdeki terimler ve açıklamalar

Aşağıdaki bölümde, Microsoft Müşteri Sözleşmesi fiyat listenizde gösterilen önemli terimler açıklanmaktadır.

| **Alan Adı**   | **Açıklama**   |
| --- | --- |
| billingAccountId  | Ödeme hesabının benzersiz tanımlayıcısı.   |
| billingAccountName  | Ödeme hesabının adı.  |
| billingProfileId  | Faturalama profilinin benzersiz tanımlayıcısı.   |
| billingProfileName  | Faturaları alması için ayarlanan faturalama profilinin adı. Fiyat listesindeki fiyatlar, bu faturalama profiliyle ilişkilidir. |
| productOrderName  | Satın alınan ürün planının adı. |
| serviceFamily  | Azure hizmetinin türü. Ör: İşlem, Analiz, Güvenlik |
| Ürün  | Ücret tahakkuk eden ürünün adı. Ör: Temel SQL DB ve Standart SQL DB  |
| productId  | Ölçümü kullanılan ürünün benzersiz tanımlayıcısı. |
| unitOfMeasure  | Hizmet faturalaması için ölçü birimlerini tanımlar. Örneğin, işlem hizmetleri saat bazında faturalandırılır. |
| meterId  | Ölçümün benzersiz tanımlayıcısı. |
| meterName  | Ölçümün adı. Ölçüm, bir Azure hizmetinin dağıtılabilir kaynağını temsil eder. |
| meterCategory  | Ölçüm için sınıflandırma kategorisinin adı. Örneğin, _Bulut hizmetleri_, _Ağ iletişimi_ vb. |
| meterType  |  Ölçüm türünün adı. |
| meterSubCategory  | Ölçüm alt sınıflandırma kategorisinin adı.  |
| meterRegion  | Hizmet ölçümünün kullanılabilir olduğu bölgenin adı. Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.    |
| tierId  | Uygun olduğunda fiyatlandırma katmanını tanımlar. Kullanılan birim sayısına göre fiyatlar farklılık gösterdiğinde bu, katmanlı fiyatlar ayarlamak için tierMinimumUnits ile birlikte çalışır.    |
| tierMinimumUnits  | Fiyatları tanımlanan katman aralığının alt sınırını tanımlar. Örneğin, aralık 0 ila 100 arasıysa tierMinimumUnits 0 olur.  |
| effectiveStartDate  | Fiyatın geçerli olduğu başlangıç tarihi. |
| effectiveEndDate  | Geçerli fiyatın bitiş tarihi. |
| unitPrice  | Bir ölçüme ve ürün siparişi adına özgü olarak faturalama sırasındaki birim başına fiyat (geçerli karma fiyat değildir).  Not: Birim fiyat, katmanlar arasında farklı fiyatları bulunan hizmetler olması durumunda kullanım ayrıntıları indirmelerindeki geçerli fiyatla aynı değildir.  Çok katmanlı fiyatlandırması bulunan hizmetler olması durumunda geçerli fiyat, katmanlar genelindeki karma bir fiyattır ve katmana özgü birim fiyatı göstermez. Karma fiyat veya geçerli fiyat, birden çok katman genelinde yayılan (her bir katmanın belirli bir birim fiyatının olduğu), kullanılan miktarın net fiyatıdır. |
| basePrice  | Müşterinin oturum açtığı andaki pazar fiyatı veya oturum açıldıktan sonra hizmet ölçümünün başlatıldığı andaki pazar fiyatı.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Kuruluşunuzun fiyatlandırmasını görüntüleme ve indirme](billing-ea-pricing.md)
