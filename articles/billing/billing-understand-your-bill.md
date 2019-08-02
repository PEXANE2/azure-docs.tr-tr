---
title: Azure faturanızı anlama
description: Azure aboneliğiniz için kullanımınızı ve faturanızı nasıl okuyup anlayacağınızı öğrenin.
author: bandersmsft
manager: dougeby
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: banders
ms.openlocfilehash: 51143644a62a77a61c4540d9f2ad3dce401c496b
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610124"
---
# <a name="understand-your-microsoft-azure-bill"></a>Microsoft Azure faturanızı anlayın
Azure faturanızı anlamak için faturanızı ayrıntılı günlük kullanım dosyası ile ve Azure portal maliyet yönetimi raporlarıyla karşılaştırırsınız.

Bu makale aşağıdaki müşterilere uygulanmaz:
- Kurumsal Anlaşma (EA müşterileri) olan Azure müşterileri. Bir EA müşterisiyseniz, bkz. [Azure müşterilerinin faturanızı bir kurumsal anlaşma anlayın](billing-understand-your-bill-ea.md).
- [Microsoft Müşteri anlaşmasıyla](#check-access-to-a-microsoft-customer-agreement)Azure müşterileri. Microsoft Müşteri sözleşmeniz varsa bkz. [Microsoft müşteri anlaşması faturanızda Azure ücretlerini anlama](billing-mca-understand-your-bill.md).

Faturalandırma, ödeme çevrimi, fiyatlandırma ve kullanım dahil olmak üzere Azure bulut çözümü sağlayıcısı (Azure CSP) programında nasıl çalıştığına ilişkin bir açıklama için bkz. [Azure CSP faturalandırma genel bakış](/azure/cloud-solution-provider/billing/azure-csp-billing-overview/).

## <a name="charges"></a>Ücretlerinizi gözden geçirin

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Faturanızda hakkında daha fazla bilgi edinmek istediğiniz bir ücretlendirme varsa, kullanım ve maliyetleri kullanım dosyası ile veya Azure portal karşılaştırabilirsiniz.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>Seçenek 1: Kullanım dosyası ile kullanımı ve maliyetleri karşılaştırın

Ayrıntılı kullanım CSV dosyası, faturalandırma dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Dosyayı indirmek veya görüntülemek için bkz. [Azure Faturalandırma faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md).

Kullanım ücretlerinizi ölçüm düzeyinde görüntülenir. Aşağıdaki koşullar, hem faturada hem de ayrıntılı kullanım dosyasında aynı şeyi ifade ederler. Örneğin, faturadaki fatura dönemi, ayrıntılı kullanım dosyasında gösterilen fatura dönemiyle aynıdır.

 | Fatura (PDF) | Ayrıntılı kullanım (CSV)|
 | --- | --- |
|Fatura döngüsü | Fatura Dönemi |
 |Ad |Sayaç Kategorisi |
 |Type |Ölçüm alt kategorisi |
 |Resource |Sayaç Adı |
 |Bölge |Ölçüm konumu |
 |Tüketilen |Tüketilen Miktar |
 |Dahil |Dahili Miktar |
 |Faturalanabilir |Fazla Kullanım Miktarı |

Faturanızda **kullanım ücretleri** bölümü, fatura döneminize göre tüketilen her bir ölçüm için toplam değeri gösterir. Örneğin, aşağıdaki görüntüde Azure Zamanlayıcı hizmeti için kullanım ücreti gösterilmektedir.

![Fatura kullanım ücretleri](./media/billing-understand-your-bill/1.png)

Ayrıntılı kullanım CSV 'nizin **Ekstre** bölümü aynı ücreti gösterir. *Tüketilen* miktar ve *değer* faturayla eşleşir.

![CSV kullanım ücretleri](./media/billing-understand-your-bill/2.png)

Ücrette günlük bir dökümü görmek için CSV dosyasının **günlük kullanım** bölümüne gidin. *Ölçer kategorisi*altında *Scheduler* için filtre. Ölçerin hangi günlerde kullanıldığını ve ne kadarının tüketildiğini görebilirsiniz. *Kaynak* ve *kaynak grubu* bilgileri de karşılaştırma için gösterilir. *Tüketilen* değerler, faturada gösterilenlerin üzerine eklenmelidir ve eşleşmelidir.

![CSV 'de günlük kullanım bölümü](./media/billing-understand-your-bill/3.png)

Gün başına maliyeti almak için, *kullanılan* miktarları **Ekstre** bölümündeki *oran* değeri ile çarpın.

Daha fazla bilgi için bkz:

- [Azure faturanızı anlayın](billing-understand-your-invoice.md)
- [Azure ayrıntılı kullanımınızı anlayın](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-in-the-azure-portal"></a>Seçenek 2: Azure portal kullanımı ve maliyetleri karşılaştırın

Azure portal, ücretlerinizi doğrulamanıza da yardımcı olabilir. Faturalanmış kullanımınız ve ücretlerinize hızlı bir genel bakış almak için, maliyet yönetimi grafiklerini görüntüleyin.

1. Azure portal [abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)' e gidin.
1. Abonelik > **Maliyet analizinizi**seçin.
1. **TimeSpan**'a göre filtreleyin.
1. Önceki örneğe devam etmek için, Azure Zamanlayıcı hizmeti için bir kullanım ücreti görürsünüz.

   ![Azure portal maliyet analizi görünümü](./media/billing-understand-your-bill/4.png)

1. Günlük maliyet dökümünü görmek için ücreti gösteren satırı seçin.

   ![Azure portal maliyet geçmişi görünümü](./media/billing-understand-your-bill/5.png)

Daha fazla bilgi edinmek için bkz. [Azure Faturalandırma ve maliyet yönetimiyle ilgili beklenmedik maliyetleri engelleme](billing-getting-started.md#costs).

## <a name="external"></a>Dış hizmetler ayrı olarak faturalandırılır

Dış hizmetler veya Market ücretleri, üçüncü taraf yazılım satıcıları tarafından oluşturulan kaynaklara yöneliktir. Bu kaynaklar Azure Marketi 'nde kullanılabilir. Örneğin, bir Barırcuda güvenlik duvarı, üçüncü taraf tarafından sunulan bir Azure Marketi kaynağıdır. Güvenlik Duvarı ve karşılık gelen ölçümler için tüm ücretler, dış hizmet ücretleri olarak görünür.

Dış hizmet ücretleri ayrı olarak faturalandırılır. Ücretler, Azure faturanızda gösterilmez. Daha fazla bilgi edinmek için bkz. [Azure dış hizmet ücretlerinizi anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="resources-billed-by-usage-meters"></a>Kullanım ölçümlerine göre faturalandırılan kaynaklar

Azure, kaynak maliyetine göre doğrudan fatura yapmaz. Bir kaynak için ücretler bir veya daha fazla ölçüm kullanılarak hesaplanır. Ölçümler, kaynağın yaşam süresi boyunca kullanımını izlemek için kullanılır. Bu ölçümler daha sonra faturayı hesaplamak için kullanılır.

Örneğin, bir sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda, bir veya daha fazla ölçüm örneği oluşturulur. Ölçümler, kaynağın zaman içinde kullanımını izlemek için kullanılır. Her ölçüm, Azure tarafından faturayı hesaplamak için kullanılan kullanım kayıtlarını yayar.

Örneğin, Azure 'da oluşturulan tek bir sanal makine (VM), kullanımını izlemek için aşağıdaki ölçümlere sahip olabilir:

- İşlem Saatleri
- IP Adresi Saatleri
- Veri Aktarımı Girişi
- Veri Aktarımı Çıkışı
- Standart yönetilen disk
- Standart Yönetilen Disk İşlemleri
- Standart GÇ-disk
- Standart GÇ-Blok Blobu okuma
- Standart GÇ-Blok Blobu yazma
- Standart GÇ-Blok Blobu silme

VM oluşturulduğunda, her ölçüm kullanım kayıtlarını yaymaya başlar. Bu kullanım ve ölçüm fiyatı Azure ölçüm sisteminde izlenir.

## <a name="payment"></a>Faturanızı ödeyin

Ödeme yönteminiz olarak bir kredi kartı kartı ayarlarsanız, ödeme dönemi sona erdikten sonra ödeme 10 gün içinde otomatik olarak ücretlendirilir. Kredi kartı deyiminizde, satır öğesi **MSFT Azure**olduğunu söylüyor.

Ücretlendirilen kredi kartını değiştirmek için bkz. [Azure için kredi kartı ekleme, güncelleştirme veya kaldırma](billing-how-to-change-credit-card.md).

[Fatura ile ödeme](billing-how-to-pay-by-invoice.md)yaparsanız, ödemenizi faturanızda listelenen konuma gönderin.

Ödemenizin durumunu denetlemek için [bir destek bileti oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Maliyet yönetimi ipuçları

- Kullanarak maliyetleri tahmin edin:
  - [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
  - [Toplam sahiplik Hesaplayıcı maliyeti](https://aka.ms/azure-tco-calculator)
  - [Her hizmet için ayrıntılı fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/)
- [Azure portal kullanımı ve maliyetlerinizi düzenli olarak gözden geçirin](billing-getting-started.md#costs)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesine erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure Faturalandırma faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md)
- [Microsoft Azure faturanızda koşulları anlayın](billing-understand-your-invoice.md)
- [Microsoft Azure ayrıntılı kullanımınızın koşullarını anlayın](billing-understand-your-usage.md)
- [Azure portal maliyet yönetimi](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Azure Faturalandırma ve maliyet yönetimiyle ilgili beklenmedik maliyetleri engelleyin](billing-getting-started.md#costs)
