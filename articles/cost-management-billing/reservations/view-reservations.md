---
title: Azure kaynakları için rezervasyonları görüntüleme | Microsoft Docs
description: Azure portalında Azure rezervasyonlarının nasıl görüntüleneceğini öğrenin. API'ler, PowerShell, CLI ve Power BI kullanarak rezervasyonları ve kullanımı görüntüleyin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: ec91d0997717d536c3b47ce7b276f75b21d1baa1
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88681660"
---
# <a name="view-azure-reservations"></a>Azure rezervasyonlarını görüntüleme

Satın alınan rezervasyonu Azure portalından görüntüleyip yönetebilirsiniz.

## <a name="permissions-to-view-a-reservation"></a>Rezervasyonu görüntüleme izinleri

Rezervasyonu görüntüleyip yönetmek için bu rezervasyon üzerinde okuyucu veya sahip iznine ihtiyaç duyarsınız. Rezervasyon satın aldığınızda varsayılan olarak siz ve hesap yöneticisi, rezervasyon siparişinde ve rezervasyonda otomatik olarak Sahip rolünü alır. Diğer kişilerin rezervasyonu görüntülemesine izin vermek için bu kişileri, Rezervasyon siparişinde veya rezervasyonda **Sahip** veya **Okuyucu** olarak eklemeniz gerekir. Rezervasyonu faturalamak için sağlanan aboneliğe birini eklemek, bu kişiyi otomatik olarak rezervasyona eklemez. 

Daha fazla bilgi için bkz. [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Azure portalında rezervasyonu ve kullanımı görüntüleme

Bir rezervasyonu Sahip veya Okuyucu olarak görüntülemek için

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. [Rezervasyonlar](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)’a gidin.
3. Listede, Sahip veya Okuyucu rolüne sahip olduğunuz tüm rezervasyonlar gösterilir. Her rezervasyon, bilinen son kullanım yüzdesini gösterir.
4. Kullanım geçmişini ve ayrıntılarını görmek için kullanım yüzdesine tıklayın. Aşağıdaki videoda bulunan ayrıntılara bakın.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-cli"></a>API’ler, PowerShell ve CLI’yı kullanarak rezervasyonları ve kullanımı alma

Aşağıdaki kaynakları kullanarak tüm rezervasyonların yer aldığı listeyi edinin
- [API: Rezervasyon Siparişi - Liste](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: Rezervasyon Siparişi - Liste](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [CLI: Rezervasyon Siparişi - Liste](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

Ayrılmış Örnek kullanımı API’sini kullanarak da [rezervasyon kullanımını](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) elde edebilirsiniz. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Power BI’da rezervasyonları ve kullanımı görme

Power BI kullanıcılarına yönelik iki seçenek vardır
- İçerik Paketi: Rezervasyon satın alımları ve kullanım verileri [Tüketim İçgörüleri Power BI içerik paketinde](/power-bi/desktop-connect-azure-cost-management) bulunur. Bu içerik paketini kullanarak istediğiniz raporları oluşturun. 
- Maliyet yönetimi uygulaması: Daha fazla özelleştirebileceğiniz önceden oluşturulmuş raporlar için [Maliyet Yönetimi Uygulaması](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp)’nı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Rezervasyonları’nı yönetin](manage-reserved-vm-instance.md).
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için rezervasyon kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
