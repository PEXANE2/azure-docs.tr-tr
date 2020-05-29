---
title: Azure Rezervasyon maliyetlerini geri ödeme
description: Geri ödeme için Azure Rezervasyon maliyetlerini görüntülemeyi öğrenin.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 05/14/2020
ms.author: banders
ms.openlocfilehash: 22e2ee897236a2e079f0ce6e230502d5242a02e8
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714364"
---
# <a name="charge-back-azure-reservation-costs"></a>Azure Rezervasyon maliyetlerini geri ödeme

Kurumsal Anlaşma ve Microsoft Müşteri Sözleşmesi okuyucuları, rezervasyonlar için itfa edilen maliyet verilerini görüntüleyebilir. Bu kişiler maliyet verilerini bir abonelik, kaynak grubu, kaynak veya iş ortaklarına yönelik bir etiketin parasal değerini geri ödemek üzere kullanabilirler. İtfa edilmiş verilerde geçerli fiyat, eşit olarak bölünmüş saatlik rezervasyon maliyetidir. Maliyet, ilgili günde kaynağa göre rezervasyon kullanımının toplam maliyetidir.

Tek aboneliğe sahip kullanıcılar, itfa edilmiş maliyet verilerini kullanım dosyalarından alabilir. Bir kaynağa rezervasyon indirimi yapıldığında, kullanım dosyasındaki *AdditionalInfo* bölümünde rezervasyon ayrıntıları yer alır. Daha fazla bilgi için bkz. [Azure portalından kullanımı indirme](https://docs.microsoft.com/azure/cost-management-billing/understand/download-azure-daily-usage#download-usage-from-the-azure-portal-csv).

## <a name="get-reservation-charge-back-data-for-chargeback"></a>Geri ödeme için rezervasyon geri ödeme verilerini alma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama**’ya gidin.
1. **Gerçek Maliyet** bölümünde **İtfa Edilen Maliyet** ölçümünü seçin.
1. Rezervasyon tarafından kullanılan kaynakları görmek için **Rezervasyon**’a bir filtre uygulayın ve sonra rezervasyonları seçin.
1. **Ayrıntı düzeyi**'ni **Aylık** veya **Günlük** olarak ayarlayın.
1. Grafik türünü **Tablo** olarak ayarlayın.
1. **Gruplandırma ölçütü** seçeneğini **Kaynak** olarak ayarlayın.

[![Geri ödeme için kullanabileceğiniz rezervasyon kaynağı maliyetlerini gösteren örnek](./media/charge-back-usage/amortized-reservation-costs.png)](./media/charge-back-usage/amortized-reservation-costs.png#lightbox)

Azure portalında rezervasyon kullanım maliyetlerini nasıl görüntüleyeceğinizi gösteren bir videoyu aşağıda bulabilirsiniz:

 > [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4sQOw] 

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonu yönetme hakkında bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri’ni Yönetme](manage-reserved-vm-instance.md).
- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure’da Rezervasyonları Yönetme](manage-reserved-vm-instance.md)
