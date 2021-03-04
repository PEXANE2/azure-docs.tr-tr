---
title: Kiracılar arasında Azure ayırma aktarımlarının sorunlarını giderme
description: Bu makale, rezervasyon sahiplerinin bir Azure Active Directory kiracısından (Dizin) bir rezervasyon siparişini diğerine aktarmasına yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 02/24/2021
ms.openlocfilehash: 79473d57cc7504e7e6ef4ef68ba0cee74203f62b
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055844"
---
# <a name="troubleshoot-reservation-transfers-between-tenants"></a>Kiracılar arasında rezervasyon aktarımlarının sorunlarını giderme

Bu makale, rezervasyon sahiplerinin bir Azure Active Directory kiracısından (Dizin) bir rezervasyon siparişini diğerine aktarmasına yardımcı olur. Bir rezervasyon sırasının dizinini değiştirdiğinizde, tüm Azure RBAC erişimini rezervasyon siparişi ve bağımlı rezervasyonlar olarak kaldırır. Yalnızca siz değişiklikten sonra erişiminiz olur. Dizinin değiştirilmesi, rezervasyon siparişi için faturalandırma sahipliğini değiştirmez. Dizin, üst ayırma siparişi ve bağımlı rezervasyonlar için değiştirilir.

Kiracılar arasında aktarım yapmak için bir ayırma değişimi ve iptali gerekli değildir.

Bir ayırmayı başka bir kiracıya aktardıktan sonra, ayırmaya ek sahipler de eklemek isteyebilirsiniz. Daha fazla bilgi için bkz. [bir ayırmayı varsayılan olarak kimin yönetebileceğini](view-reservations.md#who-can-manage-a-reservation-by-default)öğrenin.

Bir rezervasyon siparişi aktardığınızda, siparişin altındaki tüm rezervasyonlar onunla birlikte aktarılır.

## <a name="transfer-a-reservation"></a>Bir ayırmayı aktarma

Bir rezervasyon siparişini ve bu bağımlı rezervasyonları başka bir kiracıya aktarmak için aşağıdaki adımları kullanın.

1. [Azure portal](https://portal.azure.com) oturum açın.
1. Faturalandırma yöneticisiyseniz ancak bir ayırma sahibiyseniz, **rezervasyonlar** ' a gidip 6. adıma atlayın.
1. **Maliyet Yönetimi ve Faturalama** bölümüne gidin.
    - EA yöneticisiyseniz, sol taraftaki menüden **faturalandırma kapsamları** ' nı seçin ve ardından Faturalandırma kapsamları listesinden bir tane seçin.
    - Microsoft Müşteri Sözleşmesi faturalama profili sahibiyseniz, sol taraftaki menüden **faturalandırma profilleri**' ni seçin. Faturalandırma profilleri listesinde bir tane seçin.
1. Sol menüde, **ayırma işlemleri**' ni seçin. Rezervasyon işlemlerinin listesi gösterilir.
1. Sayfanın üst kısmındaki bir başlık *artık faturalama yöneticileri rezervasyonları yönetebilir. Rezervasyonları yönetmek için buraya tıklayın.* Başlığı seçin.
1. EA kaydınız veya faturalandırma profiliniz için rezervasyonların tüm listesi gösterilir.
1. Aktarmak istediğiniz ayırmayı seçin.
1. Rezervasyon ayrıntıları ' nda rezervasyon siparişi KIMLIĞINI seçin.
1. Rezervasyon emrinde, **dizini Değiştir**' i seçin.
1. Dizini Değiştir bölmesinde, ayırmayı aktarmak istediğiniz Azure AD dizinini seçin ve ardından **Onayla**' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonlar hakkında daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)