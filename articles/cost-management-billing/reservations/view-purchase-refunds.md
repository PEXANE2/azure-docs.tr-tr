---
title: Azure Rezervasyon satın alma ve iade işlemlerini görüntüleme
description: Azure Rezervasyon satın alma ve iade işlemlerini görüntülemeyi öğrenin.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 05/14/2020
ms.author: banders
ms.openlocfilehash: 0767ec7b4815fa7aadfef2bd6204627c30173993
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715384"
---
# <a name="view-reservation-purchase-and-refund-transactions"></a>Rezervasyon satın alma ve iade işlemlerini görüntüleme

Rezervasyon satın alma ve iade işlemlerini görüntülemenin birkaç farklı yolu vardır. Azure portalı, Power BI veya REST API’lerini kullanabilirsiniz.

## <a name="view-reservation-transactions-in-the-azure-portal"></a>Rezervasyon işlemlerini Azure portalında görüntüleme

Kurumsal kayıt veya Microsoft Müşteri Sözleşmesi faturalama yöneticisi, rezervasyon işlemlerini Maliyet Yönetimi ve Faturalama’da görüntüleyebilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. **Rezervasyon işlemleri**’ni seçin.
1. Sonuçları filtrelemek için **Zaman aralığı**, **Tür** veya **Açıklama** seçeneğini belirleyin.
1. **Uygula**’yı seçin.

[![Rezervasyon işlemlerini Azure portalında gösteren ekran görüntüsü](./media/view-purchase-refunds/azure-portal-reservation-transactions.png)](./media/view-purchase-refunds/azure-portal-reservation-transactions.png#lightbox)

## <a name="view-reservation-transactions-in-power-bi"></a>Rezervasyon işlemlerini Power BI’da görüntüleme

Kurumsal kayıt veya Microsoft Müşteri Sözleşmesi faturalama yöneticileri, rezervasyon işlemlerini Maliyet Yönetimi Power BI uygulamasıyla görüntüleyebilir.

1. [Maliyet Yönetimi Power BI uygulamasını](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) edinin.
1. RI Satın Almaları raporuna gidin.

[![Rezervasyon işlemlerini gösteren örnek](./media/view-purchase-refunds/power-bi-reservation-transactions.png)](./media/view-purchase-refunds/power-bi-reservation-transactions.png#lightbox)

Daha fazla bilgi için bkz. [Kurumsal Anlaşmalar için Azure Maliyet Yönetimi Power BI Uygulaması](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).

## <a name="use-apis-to-get-reservation-transactions"></a>Rezervasyon işlemlerini almak için API’leri kullanma

Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi kullanıcıları, [Rezervasyon İşlemleri - Liste API](https://docs.microsoft.com/rest/api/consumption/reservationtransactions/list)’sini kullanarak rezervasyon işlemleri verilerini alabilir.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonu yönetme hakkında bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri’ni Yönetme](manage-reserved-vm-instance.md).
- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure’da Rezervasyonları Yönetme](manage-reserved-vm-instance.md)
