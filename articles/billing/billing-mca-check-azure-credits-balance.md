---
title: Microsoft Müşteri Sözleşmesi için Azure kredi bakiyesini izleme
description: Microsoft Müşteri Sözleşmesi için Azure kredi bakiyesinin nasıl denetleneceğini öğrenin.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: ea3fc21891f1e4d4e744449032a4b2cfcdfbb2f0
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177527"
---
# <a name="track-microsoft-customer-agreement-azure-credit-balance"></a>Microsoft Müşteri Sözleşmesi Azure kredi bakiyesini izleme

Azure portalında Microsoft Müşteri Sözleşmesi için Azure kredi bakiyesini denetleyebilirsiniz. Krediler kapsamında yer alan ücretleri ödemek için kredileri kullanırsınız.

Krediler kapsamında olmayan ürünleri kullandığınızda veya kullanımınız kredi bakiyenizi aştığında ücretlendirilirsiniz. Daha fazla bilgi için bkz. [Azure kredileri kapsamında olmayan ürünler](#products-that-arent-covered-by-azure-credits).

Bu makale, Microsoft Müşteri Sözleşmesi’ne ilişkin ödeme hesabı için geçerlidir. [Microsoft Müşteri Sözleşmesi’ne erişiminiz olup olmadığını denetleyin](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-your-credit-balance"></a>Kredi bakiyenizi denetleme

1. [Azure Portal]( https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3.  Soldan **Azure kredileri**’ni seçin. Erişiminize bağlı olarak, bir faturalama hesabı veya faturalama profili seçmeniz ve sonra **Azure kredileri**’ni seçmeniz gerekebilir.

4. Azure kredileri sayfasında aşağıdaki bilgiler görüntülenir:

   ![Bir Faturalama profili için kredi bakiyesinin ve işlemlerin ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Sözleşme Dönemi               | Tanım                           |
   |--------------------|--------------------------------------------------------|
   | Tahmini bakiye  | Tüm faturalanan ve beklemedeki işlemler dikkate alındıktan sonra sahip olduğunuz tahmini kredi tutarı |
   | Geçerli bakiye    | Son faturanızdan itibaren kredi tutarı. Beklemedeki işlemleri içermez |
   | İşlemler       | Azure kredi bakiyenizi etkileyen tüm faturalama işlemleri |

   Tahmini bakiyeniz 0’a indiğinde, kredileriniz kapsamındaki ürünler de dahil, tüm kullanımınız için ücretlendirilirsiniz.

6. Faturalama profilinize ilişkin kredilerin listesini görüntülemek için **Kredi listesi**’ni seçin. Kredi listesi şu bilgileri sağlar:

   ![Faturalama profili için kredi listelerinin ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Sözleşme Dönemi | Tanım |
   |---|---|
   | Tahmini bakiye | Faturalanmamış krediye uygun ücretler geçerli bakiyenizden düşüldükten sonra elinizde kalan Azure kredisi tutarı|
   | Geçerli bakiye | Faturalanmamış krediye uygun ücretler dikkate alınmadan önce sahip olduğunuz Azure kredisi tutarı. Aldığınız yeni Azure kredileri, son fatura zamanınızdaki kredi bakiyesine eklenerek hesaplanır|
   | Kaynak | Kredinin alım kaynağı |
   | Başlangıç tarihi | Krediyi aldığınız tarih |
   | Son kullanma tarihi | Kredi süresinin dolacağı tarih |
   | Bakiye | Son faturanızdaki bakiye |
   | Orijinal tutar | Orijinal kredi tutarı |
   | Durum | Kredinin geçerli durumu. Durum etkin, kullanıldı, süresi doldu veya süresi doluyor olabilir |

## <a name="how-credits-are-used"></a>Kredilerin kullanılma şekli

Bir Microsoft müşteri sözleşmesi için ödeme hesabında, faturalarınızı ve ödeme yöntemlerinizi yönetmek için faturalama profillerini kullanırsınız. Her faturalama profili için aylık fatura oluşturulur ve faturayı ödemek için ödeme yöntemlerini kullanırsınız.

Azure kredileri, ödeme yöntemlerinden biridir. Microsoft’tan promosyon kredisi ve hizmet düzeyi kredisi gibi krediler alırsınız. Bu krediler bir faturalama profiline atanır. Faturalama profili için bir fatura oluşturulduğunda krediler, ödemeniz gereken tutarı hesaplamak için toplam faturalanan tutara otomatik olarak eklenir. Kalan tutarı, çek veya havale gibi başka bir ödeme yöntemiyle ödersiniz.

## <a name="products-that-arent-covered-by-azure-credits"></a>Azure kredileri kapsamında olmayan ürünler

 Aşağıdaki ürünler, Azure kredileriniz kapsamında değildir. Kredi bakiyenize bakılmaksızın bu ürünlerin kullanımı için ücretlendirilirsiniz:

- Canonical
- Citrix XenApp Essentials
- Citrix XenDesktop
- Kayıtlı Kullanıcı
- Openlogic
- Uzaktan Erişim Hakları XenApp Essentials Kayıtlı Kullanıcı
- Ubuntu Advantage
- Visual Studio Enterprise (Aylık)
- Visual Studio Enterprise (Yıllık)
- Visual Studio Professional (Aylık)
- Visual Studio Professional (Yıllık)
- Azure Market ürünleri
- Azure destek planları

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Müşteri Sözleşmesi için ödeme hesabını anlama](billing-mca-overview.md)
- [Microsoft Müşteri Sözleşmesi faturanızdaki terimleri anlama](billing-mca-understand-your-invoice.md)
