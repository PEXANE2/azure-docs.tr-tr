---
title: Azure faturalamaya erişimi yönetme
description: Ekibinizin üyelerine Azure faturalama bilgilerinize nasıl erişim yetkisi verileceğini öğrenin.
author: vikramdesai01
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 08/20/2020
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: f42c185046e6ce78edd234923da1a3390ed08608
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690859"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Azure için faturalama bilgilerine erişimi yönetme

Diğer kullanıcılara Azure portalında hesabınız için faturalama bilgilerine erişme yetkisi sağlayabilirsiniz. Faturalama bilgilerine erişim sağlama yönergeleri ve faturalama rollerinin türü, ödeme hesabınızın türüne göre değişiklik gösterir. Ödeme hesabınızın türünü belirlemek için bkz. [Ödeme hesabınızın türünü denetleme](#check-the-type-of-your-billing-account).

Bu makale, Microsoft Online Service program hesapları olan müşteriler için geçerlidir. Kurumsal Anlaşması (EA) olan bir Azure müşterisi ve Kuruluş Yöneticisiyseniz, Enterprise Portal’da Bölüm Yöneticilerine ve Hesap Sahiplerine izin verebilirsiniz. Daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](understand-ea-roles.md). Microsoft Müşteri Sözleşmesi müşterisiyseniz bkz. [Azure’daki Microsoft Müşteri Sözleşmesi yönetici rollerini anlama](understand-mca-roles.md).

## <a name="account-administrators-for-microsoft-online-service-program-accounts"></a>Microsoft Online Service program hesapları için hesap yöneticileri

Hesap Yöneticisi, Microsoft Online Service Programı ödeme hesabının tek sahibidir. Rol, Azure’a kaydolan bir kişiye atanır. Hesap Yöneticileri, abonelik oluşturma, faturaları görüntüleme veya aboneliğin faturalamasını değiştirme gibi çeşitli faturalama görevlerini gerçekleştirme yetkisine sahiptir.

## <a name="give-others-access-to-view-billing-information"></a>Diğer kullanıcılara faturalama bilgilerini görüntüleme erişimi izni verme

Hesap yöneticisi, hesabındaki bir abonelikte aşağıdaki rollerden birini atayarak Azure faturalama bilgilerine diğer kullanıcıların erişmesine izin verebilir.

- Hizmet Yöneticisi
- Ortak Yönetici
- Sahip
- Katılımcı
- Okuyucu
- Faturalama okuyucusu

Bu rollerin [Azure portalındaki](https://portal.azure.com/) faturalama bilgilerine erişimi vardır. Bu rollere atanan kişiler, program aracılığıyla faturaları ve kullanım ayrıntıları almak için [Faturalama API'lerini](usage-rate-card-overview.md) de kullanabilir.

Roller atamak için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

** Kurumsal Anlaşma müşterisiyseniz, Hesap Sahibi takımın diğer kullanıcılarına yukarıdaki rolü atayabilir. Ancak bu kullanıcıların faturalama bilgilerini görüntülemesi için Kuruluş Yöneticisi, Enterprise portal’da AO ücretleri görüntüleme özelliğini etkinleştirmelidir.


### <a name="allow-users-to-download-invoices"></a><a name="opt-in"></a> Kullanıcıların faturaları indirmesine izin verme

Bir Hesap Yöneticisi, diğer kullanıcılara uygun rolleri atadıktan sonra Azure portalında faturaları indirme erişimini etkinleştirmelidir. Aralık 2016’dan eski faturalar yalnızca Hesap Yöneticisi tarafından kullanılabilir.

1. [Azure portalında](https://portal.azure.com/) Hesap Yöneticisi olarak oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Azure portalı aramasını gösteren ekran görüntüsü](./media/manage-billing-access/billing-search-cost-management-billing.png)

1. Sol bölmeden **Abonelikler**’i seçin. Erişiminize bağlı olarak, bir faturalama kapsamı ve ardından **Abonelikler**’i seçmeniz gerekebilir.

    ![Aboneliklerin seçilmesini gösteren ekran görüntüsü](./media/manage-billing-access/billing-select-subscriptions.png)

1. **Faturalar**’ı ve ardından **Faturalara erişim**’i seçin.

    ![Ekran görüntüsü, faturalara erişim yetkisinin nasıl devredileceğini gösterir](./media/manage-billing-access/aa-optin01.png)

1. **Açık** seçeneğini belirleyip kaydedin.

    ![Ekran görüntüsü, faturaya erişim yetkisini devretme seçeneğinin açılmasını/kapatılmasını gösterir](./media/manage-billing-access/aa-optinallow01.png)

Hesap Yöneticisi ayrıca faturaların e-posta ile gönderilmesini de sağlayabilir. Daha fazla bilgi için bkz. [Faturanızı e-posta ile alma](download-azure-invoice-daily-usage-date.md).

## <a name="give-read-only-access-to-billing"></a>Faturalamaya salt okunur erişim yetkisi verme

Faturalama Okuyucusu rolünü, abonelik faturalama bilgilerine salt okunur erişim yetkisine ihtiyacı olan, ancak Azure hizmetlerini yönetme veya oluşturma yeteneğine ihtiyacı olmayan kişilere atayın. Bu rol, bir kuruluştaki Azure abonelikleri için finans ve maliyet yönetiminden sorumlu olan kullanıcılar için uygundur.

Faturalama Okuyucusu özelliği önizleme aşamasındadır ve henüz genel olmayan bulutları desteklememektedir.

1. [Azure portalında](https://portal.azure.com/) Hesap Yöneticisi olarak oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Azure portalı aramasını gösteren ekran görüntüsü](./media/manage-billing-access/billing-search-cost-management-billing.png)

1. Sol bölmeden **Abonelikler**’i seçin. Erişiminize bağlı olarak, bir faturalama kapsamı ve ardından **Abonelikler**’i seçmeniz gerekebilir.

    ![Aboneliklerin seçilmesini gösteren ekran görüntüsü](./media/manage-billing-access/billing-select-subscriptions.png)

1. **Erişim denetimi (IAM)** öğesini seçin.
1. Sayfanın üst kısmında **Ekle**’yi seçin.

    ![Rol ataması ekle seçeneğinin tıklanmasını gösteren ekran görüntüsü](./media/manage-billing-access/billing-click-add-role-assignment.png)

1. **Rol** açılan listesinde **Faturalama Okuyucusu**’nu seçin.
1. **Seç** metin kutusuna eklemek istediğiniz kullanıcının adını veya e-postasını yazın.
1. Kullanıcıyı seçin.
1. **Kaydet**’i seçin.
    ![Rol ataması ekle seçeneğinin tıklanmasını gösteren ekran görüntüsü](./media/manage-billing-access/billing-save-role-assignment.png)

1. Birkaç dakika sonra kullanıcıya abonelik için Faturalama Okuyucusu rolü atanmış olur.

** Kurumsal Anlaşma müşterisiyseniz bir Hesap Sahibi veya Bölüm Yöneticisi, takım üyelerine Faturalama Okuyucusu rolünü atayabilir. Ancak bu Faturalama Okuyucusunun bölüm veya hesaba ilişkin faturalama bilgilerini görüntülemesi için Kuruluş Yöneticisinin Enterprise portal’da **AO ücretleri görüntüleme** veya **DA ücretleri görüntüleme** ilkelerini etkinleştirmesi gerekir.

## <a name="check-the-type-of-your-billing-account"></a>Ödeme hesabınızın türünü denetleme
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Sahip veya Katkıda Bulunan gibi diğer rollerdeki kullanıcılar yalnızca faturalama bilgilerine değil, Azure hizmetlerine de erişebilir. Bu rolleri yönetmek için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).
- Roller hakkında daha fazla bilgi için bkz. [Azure yerleşik roller](../../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
