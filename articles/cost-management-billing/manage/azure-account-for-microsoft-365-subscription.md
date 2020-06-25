---
title: Azure hesabıyla Microsoft 365’e kaydolma
description: Azure hesabı kullanarak nasıl Microsoft 365 aboneliği oluşturulacağını öğrenin
author: JiangChen79
ms.reviewer: adwise
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: banders
ms.openlocfilehash: 006f8edc81ec19647ff6173a4c786c6d7e93fadc
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84810389"
---
# <a name="sign-up-for-a-microsoft--365-subscription-with-your-azure-account"></a>Azure hesabınızla Microsoft 365 aboneliğine kaydolma

Azure abonesiyseniz, Microsoft 365 aboneliğine kaydolmak için Azure hesabınızı kullanabilirsiniz. Azure aboneliği olan bir kuruluşun parçasıysanız, mevcut Azure Active Directory’nizde (Azure AD) kullanıcılar için Microsoft 365 abonelikleri oluşturabilirsiniz. Azure Active Directory kiracınızda Genel Yönetici veya Faturalama Yöneticisi izinlerine sahip olan bir hesap kullanarak Microsoft 365’e kaydolun. Daha fazla bilgi için bkz. [Azure AD’de hesap izinlerimi denetleme](#RoleInAzureAD) ve [Azure Active Directory’de yönetici rolleri atama](../../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Zaten bir Microsoft 365 hesabınız ve Azure aboneliğiniz varsa [Microsoft 365 kiracısını Azure aboneliği ile ilişkilendirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) işlemini yapabilirsiniz.

## <a name="get-a-microsoft-365-subscription-by-using-your-azure-account"></a>Azure hesabınızı kullanarak Microsoft 365 aboneliği alma

1. [Microsoft 365 ürün sayfasına](https://www.microsoft.com/microsoft-365/business/all-business) gidin ve bir plan seçin.
2. Sayfanın sağ üst köşesinde **Oturum aç**’a tıklayın.

    ![Microsoft 365 deneme sayfasının ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/12-office-365-trial-page.png)
3. Azure hesabı kimlik bilgilerinizle oturum açın. Kuruluşunuz için bir abonelik oluşturuyorsanız, Azure Active Directory kiracınızda Genel Yönetici veya Faturalama Yöneticisi dizin rolünün bir üyesi olan bir Azure hesabı kullanın.

    ![Microsoft oturum açma işleminin ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/13-office-365-sign-in.png)
4. **Şimdi deneyin** öğesine tıklayın.

    ![Microsoft 365 için siparişinizi onaylayan ekran görüntüsü.](./media/azure-account-for-microsoft-365-subscription/14-office-365-confirm-your-order.png)
5. Sipariş alındı belgesi sayfasında **Devam** öğesine tıklayın.

    ![Microsoft 365 sipariş alındı belgesinin ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/15-office-365-order-receipt.png)

Şimdi işiniz tamamlanmıştır. Kuruluşunuz için Microsoft 365 aboneliği oluşturduysanız, Azure AD kullanıcılarınızın şimdi Microsoft 365’te olup olmadığını denetlemek için aşağıdaki adımları kullanın.

1. Microsoft 365 yönetim merkezini açın.
2. **KULLANICILAR**’ı genişletin ve **Etkin Kullanıcılar**’a tıklayın.

    ![Microsoft 365 yönetim merkezi kullanıcılarının ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/16-microsoft-365-admin-center-users.png)

Kaydolmanızın ardından Microsoft 365 aboneliği, Azure aboneliğinizin ait olduğu aynı Azure Active Directory örneğine eklenir. Daha fazla bilgi için bkz. [Azure ve Microsoft 365 abonelikleri hakkında daha fazla bilgi](microsoft-365-account-for-azure-subscription.md#more-about-subs) ve [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="check-my-account-permissions-in-azure-ad"></a><a id="RoleInAzureAD"></a>Azure AD’de hesap izinlerimi denetleme
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmetler**’e tıklayın ve sonra **Active Directory** araması yapın.

    ![Azure portalında Active Directory ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/billing-more-services-active-directory.png)
3. **Kullanıcılar ve gruplar** > **Tüm kullanıcılar** seçeneklerine tıklayın.
4. Kullanıcı adını seçin.

    ![Azure Active Directory kullanıcılarını gösteren ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/billing-users-groups.png)

5. **Dizin rolü**’ne tıklayın.

    ![Azure portalı dizin rolünü gösteren ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/billing-user-directory-role.png)
6.  Mevcut Azure Active Directory’nizdeki kullanıcılara ilişkin bir Microsoft 365 aboneliği oluşturmak için **Genel yönetici** veya **Sınırlı yönetici** > **Faturalama yöneticisi** rolü gereklidir.

    ![Faturalama Yöneticisi Azure portalı dizin rolünü gösteren ekran görüntüsü](./media/azure-account-for-microsoft-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft 365 kiracısını Azure aboneliğiyle ilişkilendirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
