---
title: Azure kaynakları için rezervasyonları görüntüleyin
description: Azure portalında Azure rezervasyonlarının nasıl görüntüleneceğini öğrenin. API'ler, PowerShell, CLI ve Power BI kullanarak rezervasyonları ve kullanımı görüntüleyin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 02/24/2021
ms.author: banders
ms.openlocfilehash: 477dff9db28672f8231710af34786ac387f43b71
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050350"
---
# <a name="view-azure-reservations"></a>Azure rezervasyonlarını görüntüleme

Bu makale, Azure rezervasyonlarının Azure portalda nasıl görüntüleneceği açıklamaktadır. Satın alınan bir rezervasyonu Azure portalda görüntüleyip yönetebilirsiniz.

## <a name="who-can-manage-a-reservation-by-default"></a>Bir rezervasyonu varsayılan olarak kim yönetebilir?

Varsayılan olarak, aşağıdaki kullanıcılar rezervasyonları görüntüleyebilir ve yönetebilir:

- Rezervasyonu satın alan kişi ve rezervasyonu satın almak için kullanılan faturalama aboneliğinin hesap yöneticisi rezervasyon siparişine eklenir.
- Kurumsal Anlaşma ve Microsoft Müşteri Anlaşması faturalandırma yöneticileri.

Diğer kişilerin rezervasyonları yönetmesine izin vermek için iki seçeneğiniz vardır:

- Bireysel bir rezervasyon siparişi için erişim yönetimi temsilcisi atama:
    1. [Azure Portal](https://portal.azure.com) oturum açın.
    1. **Tüm Hizmetler** > **Rezervasyon**'u seçerek erişim sahibi olduğunuz rezervasyonları listeleyin.
    1. Diğer kullanıcılara erişim vermek istediğiniz rezervasyonu seçin.
    1. Rezervasyon ayrıntılarından rezervasyon siparişini seçin.
    1. **Erişim denetimi (IAM)** öğesini seçin.
    1. **Rol ataması ekle** > **Rol** > **Sahip**'i seçin. Sınırlı erişim vermek istiyorsanız farklı bir rol seçin.
    1. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın.
    1. Kullanıcıyı ve ardından **Kaydet**'i seçin.

- Bir Kullanıcıyı bir Kurumsal Anlaşma veya bir Microsoft Müşteri Anlaşmasına faturalandırma yöneticisi olarak ekleme:
    - Kurumsal Anlaşma için, Kurumsal Anlaşma uygulanan tüm rezervasyon siparişlerini görüntülemek ve yönetmek üzere _Kuruluş Yöneticisi_  rolüne sahip kullanıcıları ekleyin. _Kuruluş Yöneticisi (salt okuma) rolü olan kullanıcılar_  yalnızca rezervasyonu görüntüleyebilir. Bölüm yöneticileri ve hesap sahipleri, erişim denetimi (IAM) kullanılarak bunlara açıkça eklenmedikleri _takdirde ayırmaları_ görüntüleyemez. Daha fazla bilgi için bkz. [Azure Enterprise rollerini yönetme](../manage/understand-ea-roles.md).

        _Kurumsal Yöneticiler, bir rezervasyon siparişinin sahipliğini alabilir ve erişim denetimi (IAM) kullanarak bir rezervasyona başka kullanıcılar ekleyebilirler._
    - Bir Microsoft Müşteri Anlaşması için, faturalandırma profili sahibi veya faturalandırma profili katılımcısı rolüne sahip kullanıcılar, faturalandırma profilini kullanarak yapılan tüm rezervasyon satın almalarını yönetebilir. Faturalandırma profili okuyucuları ve fatura yöneticileri, faturalandırma profiliyle ilgili ödenen tüm rezervasyonları görüntüleyebilir. Ancak, rezervasyonlar üzerinde değişiklik yapamaz.
    Daha fazla bilgi için bkz. [Faturalandırma profili rolleri ve görevleri](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="how-billing-administrators-view-or-manage-reservations"></a>Faturalandırma yöneticileri rezervasyonları nasıl görüntüler veya yönetir

Azure RBAC erişimi ile rezervasyonlar veya rezervasyon siparişlerine erişiminiz varsa, rezervasyon işlemlerine gittiğinizde yalnızca bir ayırma işlemleri veya hiçbiri için bir alt küme görebilirsiniz. Tüm ayırmaları ve rezervasyon işlemlerini görüntülemek ve yönetmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com) oturum açın ve **maliyet yönetimi + faturalandırma**' a gidin.
    - EA yöneticisiyseniz, sol taraftaki menüden **faturalandırma kapsamları** ' nı seçin ve ardından Faturalandırma kapsamları listesinden bir tane seçin.
    - Microsoft Müşteri Sözleşmesi faturalama profili sahibiyseniz, sol taraftaki menüden **faturalandırma profilleri**' ni seçin. Faturalandırma profilleri listesinde bir tane seçin.
1. Sol menüde, **ayırma işlemleri**' ni seçin. Rezervasyon işlemlerinin listesi gösterilir.
1. Sayfanın üst kısmındaki bir başlık *artık faturalama yöneticileri rezervasyonları yönetebilir. Rezervasyonları yönetmek için buraya tıklayın.* Başlığı seçin.
1. EA kaydınız veya faturalandırma profiliniz için rezervasyonların tüm listesi gösterilir.
1. Bir ayırmanın sahipliğini almak istiyorsanız bunu seçin. Ardından izinleri ayarla sayfasında, **erişim ver**' i seçin. Rezervasyon ve rezervasyon siparişi için sahip erişimi verildi.

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Azure portalında rezervasyonu ve kullanımı görüntüleme

Bir rezervasyonu Sahip veya Okuyucu olarak görüntülemek için

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. [Rezervasyonlar](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)’a gidin.
3. Listede, Sahip veya Okuyucu rolüne sahip olduğunuz tüm rezervasyonlar gösterilir. Her rezervasyon, bilinen son kullanım yüzdesini gösterir.
4. Kullanım geçmişini ve ayrıntılarını görmek için kullanım yüzdesini seçin. Aşağıdaki videoda bulunan ayrıntılara bakın.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-and-cli"></a>API’ler, PowerShell ve CLI’yı kullanarak rezervasyonlar ve kullanım alın

Aşağıdaki kaynakları kullanarak tüm rezervasyonların listesini alın:

- [API: Rezervasyon Siparişi - Liste](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: Rezervasyon Siparişi - Liste](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [CLI: Rezervasyon Siparişi - Liste](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

Ayrılmış Örnek kullanımı API’sini kullanarak da [rezervasyon kullanımını](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) elde edebilirsiniz. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Power BI’da rezervasyonları ve kullanımı görme

Power BI kullanıcılarına yönelik iki seçenek vardır
- İçerik Paketi: Rezervasyon satın alma tarihi ve kullanım verileri [Tüketim İçgörüleri Power BI içerik paketinde](/power-bi/desktop-connect-azure-cost-management) mevcuttur. İçerik paketini kullanarak istediğiniz raporları oluşturun. 
- Maliyet yönetimi uygulaması: Daha fazla özelleştirebileceğiniz önceden oluşturulmuş raporlar için [Maliyet Yönetimi Uygulaması](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp)’nı kullanın.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Rezervasyonları’nı yönetin](manage-reserved-vm-instance.md).
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md).
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md).
- [CSP abonelikleri için rezervasyon kullanımını anlama](/partner-center/azure-reservations).

