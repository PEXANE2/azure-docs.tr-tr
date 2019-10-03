---
title: Azure kullanımını ve ücretlerini Görüntüleme ve İndirme
description: Günlük Azure kullanımınızın ve ücretlerinizin nasıl indirileceğini veya görüntüleneceğini açıklar.
keywords: faturalama kullanımı, kullanım ücretleri, kullanım indirmesi, kullanımı görüntüleme, azure faturası, azure kullanımı
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 23cd7c3765fc99eb5907aa853d7431d5e247aea6
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709720"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme

Kurumsal Anlaşma müşterisiyseniz veya [Microsoft Müşteri Sözleşmeniz](#check-your-access-to-a-microsoft-customer-agreement) varsa, [Azure portalından](https://portal.azure.com/) Azure kullanımını ve ücretlerini indirebilirsiniz. Diğer abonelikler için [Azure Hesap Merkezi](https://account.azure.com/Subscriptions)’ne giderek kullanım bilgilerini indirin.

Yalnızca Hesap Yöneticisi veya Kuruluş Yöneticisi gibi belirli rollerin Azure kullanım bilgilerini alma izni vardır. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

[Microsoft Müşteri Sözleşmeniz](#check-your-access-to-a-microsoft-customer-agreement) varsa, Azure kullanımınızı ve ücretlerinizi görüntülemek için faturalama profili Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir. Microsoft Müşteri Sözleşmelerine ilişkin faturalama rolleri hakkında daha fazla bilgi edinmek için bkz. [Faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="download-usage-from-the-account-center-csv"></a>Kullanım bilgilerinizi Hesap Merkezinden (.csv) indirme

1. [Azure Hesap Merkezi](https://account.windowsazure.com/subscriptions)’nde Hesap Yöneticisi olarak oturum açın.

2. Fatura ve kullanım bilgilerini görmek istediğiniz aboneliği seçin.

3. **FATURA GEÇMİŞİ**’ni seçin.

    ![Fatura geçmişi seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Son altı faturalama dönemine ait ve güncel faturalanmamış döneme ait bildirimlerinizi görebilirsiniz.

    ![Faturalama dönemlerini, fatura ve günlük kullanımı indirme seçeneklerini ve her faturalama dönemine ait toplam ücretleri gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Tahminin oluşturulduğu zaman itibarıyla yapılacak bir ücret tahminini görmek için **Geçerli Bildirimi Görüntüle**'yi seçin. Bu bilgiler yalnızca günlük olarak güncelleştirilir ve tüm kullanımınızı içermeyebilir. Aylık faturanız bu tahminden farklı olabilir.

    ![Geçerli Bildirimi Görüntüle seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Geçerli ücretlerin tahminini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Günlük kullanım verilerini CSV dosyası olarak indirmek için **Kullanımı İndir**'i seçin. İki sürüm varsa 2. sürümü indirin.

    ![Kullanımı İndir seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Yalnızca Hesap Yöneticisi Azure Hesap Merkezi'ne erişebilir. Sahip gibi diğer fatura yöneticileri, [Faturalama API’lerini](billing-usage-rate-card-overview.md) kullanarak kullanım bilgilerini alabilir.

Günlük kullanımınız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md). Maliyetlerinizin yönetimi konusunda yardım için bkz. [Azure fatura ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

## <a name="download-usage-for-ea-customers"></a>Kurumsal Anlaşma müşterileri için kullanımı indirme

Kurumsal Anlaşma müşterisi olarak kullanım verilerinizi görüntülemek veya indirmek için Kuruluş Yöneticisi, Hesap Sahibi veya ücretleri görüntüleme ilkesi etkinleştirilmiş Bölüm Yöneticisi olmanız gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.

    ![Azure portalı aramasını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. **Kullanım + ücretler**’i seçin.
1. İndirmek istediğiniz ay için **İndir**’i seçin.

## <a name="download-usage-for-your-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmeniz için kullanımı indirme

Microsoft Müşteri Sözleşmeniz varsa, faturalama profiliniz için Azure kullanımınızı ve ücretlerinizi indirebilirsiniz. Azure kullanım bilgilerini ve ücretlerini içeren CSV dosyasını indirmek için faturalama profilinin Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir.

### <a name="download-usage-for-billed-charges"></a>Faturalanmış ücretler için kullanım bilgilerini indirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. *Maliyet Yönetimi + Faturalama* araması yapın.
3. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
4. **Faturalar**'ı seçin.
5. Fatura kılavuzunda, indirmek istediğiniz kullanıma karşılık gelen fatura satırını bulun.
6. Satırın sonundaki üç noktaya (`...`) tıklayın.

    ![Satırın sonundaki üç noktayı gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)

7. İndirme bağlam menüsünde **Azure kullanımı ve ücretleri**‘ni seçin.

     ![Azure kullanımı ve ücretlerini seçili şekilde gösteren ekran görüntüsü](./media/billing-download-azure-usage/contextmenu-usage.png)

### <a name="download-usage-for-pending-charges"></a>Beklemedeki ücretler için kullanım bilgilerini indirme

Ayrıca, geçerli faturalama dönemi için ay başından bugüne kadarki kullanım bilgilerini de indirebilirsiniz. Henüz faturalanmamış kullanım ücretleri.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. *Maliyet Yönetimi + Faturalama* araması yapın.
3. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
4. **Genel Bakış** alanında, ay başından bugüne kadarki ücretlerin altında indirme bağlantılarını bulun.
5. **Azure kullanımı ve ücretleri**’ni seçin.

    ![Genel Bakış’tan indirme seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişiminizi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturanız ve kullanım ücretleri hakkında daha fazla bilgi edinmek için bkz:

- [Ayrıntılı Microsoft Azure kullanım bilgilerinizdeki terimleri anlama](billing-understand-your-usage.md)
- [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](billing-download-azure-invoice.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme](billing-ea-pricing.md)

Microsoft Müşteri Sözleşmeniz varsa bkz:

- [Microsoft Müşteri Sözleşmesi ayrıntılı Azure kullanım bilgilerinizdeki terimleri anlama](billing-mca-understand-your-usage.md)
- [Microsoft Müşteri Sözleşmesi faturanızdaki ücretleri anlama](billing-mca-understand-your-bill.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](billing-download-azure-invoice.md)
- [Microsoft Müşteri Sözleşmeniz için vergi belgelerini görüntüleme ve indirme](billing-mca-download-tax-document.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme](billing-ea-pricing.md)
