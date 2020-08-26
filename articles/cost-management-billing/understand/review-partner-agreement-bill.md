---
title: Microsoft İş Ortağı Sözleşmesi faturanızı gözden geçirme - Azure
description: Microsoft İş Ortağı Sözleşmesi faturanızı gözden geçirerek kaynak kullanımınızı incelemeyi ve ücretleri doğrulamayı öğrenin.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: tutorial
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 9e5ecc67fe86afa15c66d175f0705818154588bf
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684363"
---
# <a name="tutorial-review-your-microsoft-partner-agreement-invoice"></a>Öğretici: Microsoft İş Ortağı Sözleşmesi faturanızı gözden geçirme

 Microsoft İş Ortağı Sözleşmesi'nin ödeme hesabında, her faturalama profili için ayda bir fatura oluşturulur. Fatura önceki aya ait tüm müşteri ücretlerini içerir. Azure portalında tek tek işlemleri analiz ederek faturanızdaki ücretleri anlayabilirsiniz. Faturalarınızı Azure portalında da görüntüleyebilir ve ücretleri kullanım ayrıntıları dosyasıyla karşılaştırabilirsiniz.

Daha fazla bilgi için bkz. [Azure portalından faturaları indirme](download-azure-invoice.md).

Bu öğretici yalnızca Microsoft İş Ortağı Sözleşmesi olan Azure iş ortakları için geçerlidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında faturalanmış işlemleri gözden geçirme
> * Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme
> * Azure kullanım ücretlerinizi analiz etme

## <a name="prerequisites"></a>Ön koşullar

Microsoft İş Ortağı Sözleşmesi’ne ait bir ödeme hesabına erişiminizin olması gerekir.

Azure'a abone olmanızın üzerinden en az 30 gün geçmiş olmalıdır. Azure sizi fatura döneminizin sonunda faturalar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme

Bir Microsoft İş Ortağı Sözleşmesi için ödeme hesabına erişiminiz olup olmadığını belirlemek için sözleşme türünü denetleyin.

Azure portalındaki arama kutusuna *maliyet yönetimi + faturalandırma* yazın ve **Maliyet Yönetimi + Faturalandırma**'yı seçin.

![Azure portalı aramasını gösteren ekran görüntüsü](./media/review-partner-agreement-bill/billing-search-cost-management-billing.png)

Yalnızca bir faturalama kapsamına erişiminiz varsa soldan **Özellikler**’i seçin. Ödeme hesabı türü **Microsoft İş Ortağı Sözleşmesi** olursa, bir Microsoft İş Ortağı Sözleşmesi için ödeme hesabına erişiminiz olur.

![Özellikler sayfasında Microsoft İş Ortağı Sözleşmesi'ni gösteren ekran görüntüsü](./media/review-partner-agreement-bill/billing-account-properties-partner-agreement.png)

Birden çok faturalama kapsamına erişiminiz varsa, ödeme hesabı sütunundaki türü denetleyin. Herhangi bir kapsamın ödeme hesabı türü **Microsoft İş Ortağı Sözleşmesi** ise bir Microsoft İş Ortağı Sözleşmesi için ödeme hesabına erişiminiz olur.

![Ödeme hesabı liste sayfasında Microsoft İş Ortağı Sözleşmesi'ni gösteren ekran görüntüsü](./media/review-partner-agreement-bill/mpa-in-the-list.png)

## <a name="review-invoiced-transactions-in-the-azure-portal"></a>Azure portalında faturalanmış işlemleri gözden geçirme

Maliyet Yönetimi + Faturalandırma sayfasının sol tarafında **Tüm işlemler**'i seçin. Erişiminize bağlı olarak, bir ödeme hesabı, faturalama profili veya müşteri seçmeniz ve sonra da **Tüm işlemler** seçeneğini belirlemeniz gerekebilir.

Tüm işlemler sayfasında aşağıdaki bilgiler görüntülenir:

![Faturalanan işlemlerin listesini gösteren ekran görüntüsü](./media/review-partner-agreement-bill/all-transactions.png)

|Sütun  |Tanım  |
|---------|---------|
|Tarih     | İşlem tarihi  |
|Fatura kimliği     | İşlemin faturalandığı faturanın tanımlayıcısı. Destek isteği gönderiyorsanız, destek isteğinizi hızlandırmak için Azure desteğiyle bu kimliği paylaşın |
|İşlem türü     |  Satın alma, iptal ve kullanım ücretleri gibi işlemin türü  |
|Ürün ailesi     | Sanal makine için işlem veya Azure SQL Veritabanı için veritabanı gibi ürünün kategorisi|
|Ürün SDKU'su     | Ürününüzün örneğini tanımlayan benzersiz kod |
|Miktar     |  İşlem miktarı      |
|Faturalama profili     | İşlem bu faturalama profili faturasında gösterilir |

Faturanın işlemlerini filtrelemek için fatura kimliği araması yapın.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme

Ücretler tahmin edilir ve fatura edilene kadar beklemede olarak kabul edilir. Bir sonraki faturanızı tahmin etmek için Azure portalında Microsoft İş Ortağı Sözleşmesi faturalama profilinizin beklemedeki ücretlerini görüntüleyebilirsiniz. Bekleyen ücretler tahminidir ve vergileri içermez. Sonraki faturanızın gerçek ücretleri bekleyen ücretlerden farklı olacaktır.

### <a name="view-pending-transactions"></a>Bekleyen işlemleri görüntüleme

Bekleyen ücretleri belirlediğinizde, ücretlere katkıda bulunan tek tek işlemleri analiz ederek ücretleri anlayabilirsiniz. Bu noktada, Tüm işlemler sayfasında bekleyen kullanım ücretleri görüntülenmez. Bekleyen kullanım ücretlerini Azure abonelikler sayfasında görüntüleyebilirsiniz.

Maliyet Yönetimi + Faturalandırma sayfasında bir faturalama profili seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Sayfanın sol tarafında **Tüm işlemler**'i seçin.

*Bekleyen* terimi için arama yapın. Geçerli ayın veya geçen ayın bekleyen ücretlerini görüntülemek için **Zaman Aralığı** filtresini kullanın.

<!-- ![Screenshot that shows the pending transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-pending-transactions.png) -->

### <a name="view-pending-charges-by-customer"></a>Müşteriye göre beklemedeki ücretleri görüntüleme

Maliyet Yönetimi + Faturalandırma sayfasında bir faturalama profili seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Sayfanın sol tarafından **Müşteriler**’i seçin.

<!-- ![screenshot of billing profile customers list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-customers.png) -->

Müşteriler sayfasında, faturalama profili ile ilişkili her müşteri için geçerli ayın ve önceki ayın ücretleri görüntülenir. Ayın başından bugüne kadarki ücretler geçerli ay için bekleyen ücretlerdir ve aya ait fatura oluşturulduğunda faturalandırılır. Geçen ayın faturası henüz oluşturulmadıysa geçen ayın ücretleri de beklemededir.

### <a name="view-pending-usage-charges"></a>Bekleyen kullanım ücretlerini görüntüleme

Maliyet Yönetimi + Faturalandırma sayfasında bir faturalama profili seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Sayfanın sol tarafından **Azure abonelikleri**'ni seçin. Azure abonelikleri sayfasında, faturalama profilindeki her abonelik için geçerli ayın ve geçen ayın ücretleri görüntülenir. Ayın başından bugüne kadarki ücretler geçerli ay için bekleyen ücretlerdir ve aya ait fatura oluşturulduğunda faturalandırılır. Geçen ayın faturası henüz oluşturulmadıysa geçen ayın ücretleri de beklemededir.

<!--     ![Screenshot that shows the Azure subscriptions list for MPA billing profile](./media/billing-mpa-understand-your-bill/mpa-billing-profile-subscriptions-list.png) -->

## <a name="analyze-your-azure-usage-charges"></a>Azure kullanım ücretlerinizi analiz etme

Kullanıma dayalı ücretlerinizi analiz etmek için Azure kullanımı ve ücretleri CSV dosyasını kullanın. Fatura PDF dosyasında listelenen her bir ürünün kullanım ücretlerinde mutabakat sağlamak için Azure kullanımı ve ücretleri dosyasına filtre uygulayabilirsiniz. Belirli bir ürünün ayrıntılı kullanım ücretlerini görüntülemek için, Azure kullanımı ve ücretleri CSV dosyasındaki **product** sütununa yalnızca söz konusu ürünün adını içerecek şekilde filtre uygulayın.

Ayrıca, Azure kullanımı ve ücretleri CSV dosyasındaki **customerName** sütununa filtre uygulayarak her müşterinizin günlük kullanım ücretlerini görüntüleyebilirsiniz. Günlük kullanım ücretlerini Azure aboneliğine göre görüntülemek istiyorsanız **subscriptionName** sütununu filtreleyin.

## <a name="pay-your-bill"></a>Faturanızı ödeme

Faturanızı ödeme yönergeleri faturanın alt kısmında gösterilir. Fatura tarihinizden sonraki 60 gün içinde banka havalesi veya çekle ödeme yapabilirsiniz.

Faturanızı zaten ödediyseniz Azure portalındaki Faturalar sayfasında ödeme durumunu denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında faturalanmış işlemleri gözden geçirme
> * Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme
> * Azure kullanım ücretlerinizi analiz etme

İş ortakları için Azure Maliyet Yönetimi'ni kullanma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [İş ortakları için Azure Maliyet Yönetimi’ni kullanmaya başlama](../costs/get-started-partners.md)
