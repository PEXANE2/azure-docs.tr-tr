---
title: Microsoft Müşteri sözleşmenizin faturanızı gözden geçirin-Azure
description: Faturanızı ve kaynak kullanımınızı incelemeyi ve Microsoft müşteri anlaşması faturanız için ücretleri doğrulamanızı öğrenin.
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2019
ms.author: banders
ms.openlocfilehash: 3cbc4ab4f0e2ad18c7d1d430ed5501e23b5f5f4d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987071"
---
# <a name="tutorial-review-your-microsoft-customer-agreement-invoice"></a>Öğretici: Microsoft müşteri anlaşması faturanızı gözden geçirme

Tek tek işlemleri çözümleyerek faturanızda bulunan ücretleri gözden geçirebilirsiniz. Microsoft Müşteri Sözleşmesi'nin ödeme hesabında, her faturalama profili için ayda bir fatura oluşturulur. Fatura önceki aya ait tüm ücretleri içerir. Faturanız Azure portal görüntüleyebilir ve ücretleri kullanım ayrıntı dosyası ile karşılaştırabilirsiniz.

Bu öğretici yalnızca Microsoft Müşteri anlaşmasıyla Azure müşterileri için geçerlidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portal faturalanmış işlemleri gözden geçirin
> * Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme
> * Azure kullanım ücretlerinizi analiz etme

## <a name="prerequisites"></a>Ön koşullar

Microsoft Müşteri Sözleşmesi için bir faturalandırma hesabınız olmalıdır.

Bir Microsoft Müşteri sözleşmesine erişiminizin olması gerekir. Fatura ve kullanım bilgilerini görüntülemek için bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya Fatura Yöneticisi olmanız gerekir. Microsoft Müşteri Sözleşmelerine ilişkin faturalama rolleri hakkında daha fazla bilgi edinmek için bkz. [Faturalama profili rolleri ve görevleri](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

Azure 'a abone olduğunuz günden 30 günden daha uzun olmalıdır. Azure sizi fatura döneminizin sonunda faturalar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme

Bir Microsoft Müşteri Sözleşmesi için ödeme hesabına erişiminiz olup olmadığını belirlemek için sözleşme türünü denetleyin.

Azure portal, arama kutusuna *maliyet yönetimi + faturalandırma* yazın ve ardından **maliyet yönetimi + faturalandırma**' i seçin.

![Azure portalında maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/billing-search-cost-management-billing.png)

Yalnızca bir faturalama kapsamına erişiminiz varsa soldan **Özellikler**’i seçin. Ödeme hesabı türü **Microsoft Müşteri Sözleşmesi** olursa, bir Microsoft Müşteri Sözleşmesi için ödeme hesabına erişiminiz olur.

![Özellikler sayfasında Microsoft Müşteri anlaşmasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/billing-mca-property.png)

Birden çok faturalama kapsamına erişiminiz varsa, ödeme hesabı sütunundaki türü denetleyin. Herhangi bir kapsam için ödeme hesabı türü **Microsoft Müşteri Sözleşmesi** olursa, bir Microsoft Müşteri Sözleşmesi için ödeme hesabına erişiminiz olur.

![Faturalandırma hesabı listesi sayfasında Microsoft Müşteri anlaşmasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/billing-mca-in-the-list.png)

## <a name="review-invoiced-transactions-in-the-azure-portal"></a>Azure portal faturalanmış işlemleri gözden geçirin

Azure portal sayfanın sol tarafındaki **tüm işlemler** ' i seçin. Erişiminize bağlı olarak, bir ödeme hesabı, faturalama profili veya fatura bölümü seçmeniz ve sonra da **Tüm işlemler** seçeneğini belirtmeniz gerekebilir.

Tüm işlemler sayfasında aşağıdaki bilgiler görüntülenir:

![Faturalanan işlemlerin listesini gösteren ekran görüntüsü](./media/review-customer-agreement-bill/mca-billed-transactions-list.png)

|Sütun  |Tanım  |
|---------|---------|
|Tarih     | İşlem tarihi  |
|Fatura kimliği     | İşlemin faturalandığı faturanın tanımlayıcısı. Destek isteği gönderiyorsanız, destek isteğinizi hızlandırmak için Azure desteğiyle bu kimliği paylaşın |
|İşlem türü     |  Satın alma, iptal ve kullanım ücretleri gibi işlemin türü  |
|Ürün ailesi     | Sanal makine için işlem veya Azure SQL veritabanı için veritabanı gibi ürünün kategorisi|
|Ürün sku'su     | Ürününüzün örneğini tanımlayan benzersiz kod |
|Miktar     |  İşlem miktarı      |
|Fatura bölümü     | İşlem faturalama profili faturasının bu bölümünde gösterilir |
|Faturalama profili     | İşlem bu faturalama profili faturasında gösterilir |

Faturaya yönelik işlemleri filtrelemek için bir fatura KIMLIĞI arayın.

### <a name="view-transactions-by-invoice-sections"></a>Fatura bölümlerine göre işlemleri görüntüleme

Fatura bölümleri bir faturalama profili faturasında maliyetleri düzenlemenize yardımcı olur. Daha fazla bilgi için. Fatura oluşturulduğunda faturalama profilindeki tüm bölümlere ilişkin ücretler faturada gösterilir.

Aşağıdaki resimde örnek faturanın Accounting Dept fatura bölümüne ilişkin ücretler gösterilir.

![Fatura bölümü bilgilerine göre ayrıntıların gösterildiği örnek resim](./media/review-customer-agreement-bill/invoicesection-details.png)

Fatura bölümünün ücretlerini belirlediğinizde, ücretleri anlamak için Azure portalında işlemleri görüntüleyebilirsiniz.

Faturanın işlemlerini görüntülemek için Azure portalında Tüm işlemler sayfasına gidin.

İşlemleri görmek için fatura bölümü adına göre filtreleyin.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme

Bir Microsoft Müşteri Sözleşmesi faturalandırma hesabında, ücretler tahmin edilir ve faturalanana kadar bekliyor olarak kabul edilir. Bir sonraki faturanızı tahmin etmek için Azure portalında bekleyen ücretleri görüntüleyebilirsiniz. Bekleyen ücretler tahminidir ve vergileri içermez. Sonraki faturanızın gerçek ücretleri bekleyen ücretlerden farklı olacaktır.

### <a name="view-summary-of-pending-charges"></a>Bekleyen ücretlerin özetini görüntüleme

[Azure Portal](https://portal.azure.com)’ında oturum açın.

Faturalama profilini seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Ekranın üst kısmından **Özet** sekmesini seçin.

Ücretler bölümünde ay başından bugüne kadarki ücretlerle geçen ayın ücretleri görüntülenir.

![Azure portalında maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/mca-billing-profile-summary.png)

Ayın başından bugüne kadarki ücretler geçerli ay için bekleyen ücretlerdir ve aya ait fatura oluşturulduğunda faturalandırılır. Geçen ayın faturası henüz oluşturulmadıysa geçen ayın ücretleri de beklemededir ve sonraki faturada gösterilir.

### <a name="view-pending-transactions"></a>Bekleyen işlemleri görüntüleme

Bekleyen ücretleri belirlediğinizde, ücretlere katkıda bulunan tek tek işlemleri analiz ederek ücretleri anlayabilirsiniz. Bu noktada, Tüm işlemler sayfasında bekleyen kullanım ücretleri görüntülenmez. Bekleyen kullanım ücretlerini Azure abonelikler sayfasında görüntüleyebilirsiniz.

[Azure Portal](https://portal.azure.com)’ında oturum açın.

Azure portal, arama kutusuna *maliyet yönetimi + faturalandırma* yazın ve ardından **maliyet yönetimi + faturalandırma**' i seçin.

Faturalama profilini seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Sayfanın sol tarafında **Tüm işlemler**'i seçin.

*Bekleyen* terimi için arama yapın. Geçerli ayın veya geçen ayın bekleyen ücretlerini görüntülemek için **Zaman Aralığı** filtresini kullanın.

![Bekleyen işlemlerin listesini gösteren ekran görüntüsü](./media/review-customer-agreement-bill/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Bekleyen kullanım ücretlerini görüntüleme

[Azure Portal](https://portal.azure.com)’ında oturum açın.

Azure portal, arama kutusuna *maliyet yönetimi + faturalandırma* yazın ve ardından **maliyet yönetimi + faturalandırma**' i seçin.

Faturalama profilini seçin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Ödeme hesabında **Faturalama profilleri**'ni ve ardından bir faturalama profili seçin.

Sayfanın sol tarafında **Tüm abonelikler**'i seçin.

Azure abonelikleri sayfasında, faturalama profilindeki her abonelik için geçerli ayın ve geçen ayın ücretleri görüntülenir. Ayın başından bugüne kadarki ücretler geçerli ay için bekleyen ücretlerdir ve aya ait fatura oluşturulduğunda faturalandırılır. Geçen ayın faturası henüz oluşturulmadıysa geçen ayın ücretleri de beklemededir.

![Faturalama profili için Azure abonelikleri listesini gösteren ekran görüntüsü](./media/review-customer-agreement-bill/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Azure kullanım ücretlerinizi analiz etme

Kullanıma dayalı ücretlerinizi analiz etmek için Azure kullanımı ve ücretleri CSV dosyasını kullanın. Bu dosyayı bir fatura için veya bekleyen ücretler için indirebilirsiniz.

### <a name="download-your-invoice-and-usage-details"></a>Faturanızı ve kullanım ayrıntılarını indirin

Erişimlerinize bağlı olarak, maliyet yönetimi + Faturalandırma ' de bir faturalandırma hesabı veya faturalandırma profili seçmeniz gerekebilir. Soldaki menüde **Faturalama** altındaki **Faturalar**’ı seçin. Fatura kılavuzunda, indirmek istediğiniz fatura satırını bulun. Satırın sonundaki indir simgesine veya üç noktaya (...) tıklayın. **İndir** kutusunda kullanım ayrıntıları dosyasını ve faturayı indirin.

### <a name="view-detailed-usage-by-invoice-section"></a>Fatura bölümüne göre ayrıntılı kullanımı görüntüleme

Fatura bölümlerinizdeki kullanım ücretlerinde mutabakat sağlamak için Azure kullanımı ve ücretleri dosyasına filtre uygulayabilirsiniz.

Aşağıdaki bilgiler, muhasebe bölümü faturası bölümünün işlem ücretlerini mutabık kılma konusunda size kılavuzluk eder:

![Fatura bölümü bilgilerine göre ayrıntıların gösterildiği örnek resim](./media/review-customer-agreement-bill/invoicesection-details.png)

| Fatura PDF dosyası | Azure kullanımı ve ücretleri CSV dosyası |
| --- | --- |
|Accounting Dept |invoiceSectionName |
|Kullanım Ücretleri - Microsoft Azure Planı |productOrderName |
|Bilgi İşlem |serviceFamily |

CSV dosyasındaki **ınvoicesectionname** sütununu **muhasebe bölüm**' e filtreleyin. Sonra, CSV dosyasındaki **productOrderName** sütununu **Microsoft Azure plan**olarak filtreleyin. Ardından, CSV dosyasındaki **Servicefamily** sütununu **Microsoft. COMPUTE**olarak filtreleyin.

![Fatura bölümüne göre filtrelenmiş kullanım ve ücretler dosyasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/billing-usage-file-filtered-by-invoice-section.png)

### <a name="view-detailed-usage-by-subscription"></a>Aboneliğe göre ayrıntılı kullanımı görüntüleme

Aboneliklerinizde kullanım ücretlerinde mutabakat sağlamak için Azure kullanımı ve ücretleri CSV dosyasına filtre uygulayabilirsiniz. Abonelik için ücretleri belirledikten sonra, bu ücretleri analiz etmek için Azure kullanımı ve ücretleri CSV dosyasını kullanın.

Aşağıdaki resimde Azure portalında abonelik listesi gösterilir.

![Faturalama profili için Azure abonelikleri listesini gösteren ekran görüntüsü](./media/review-customer-agreement-bill/mca-billing-profile-subscriptions-list-highlighted.png)

WA_Subscription aboneliğinin ayrıntılı kullanım ücretlerini görüntülemek için Azure kullanımı ve ücretleri CSV dosyasının **subscriptionName** sütununa **WA_Subscription** filtresi uygulayın.

![Aboneliğe göre filtrelenmiş kullanım ve ücretler dosyasını gösteren ekran görüntüsü](./media/review-customer-agreement-bill/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Faturanızı ödeme

Faturanızı ödeme yönergeleri faturanın alt kısmında gösterilir. [Nasıl ödeneceğini öğrenin](mca-understand-your-invoice.md#how-to-pay).

Faturanızı zaten ödediyseniz Azure portalındaki Faturalar sayfasında ödeme durumunu denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portal faturalanmış işlemleri gözden geçirin
> * Sonraki faturanızı tahmin etmek için bekleyen ücretleri gözden geçirme
> * Azure kullanım ücretlerinizi analiz etme

Maliyet analizini kullanmaya başlamak için hızlı başlangıcı doldurun.

> [!div class="nextstepaction"]
> [Maliyet analizini başlatma](../costs/quick-acm-cost-analysis.md)
