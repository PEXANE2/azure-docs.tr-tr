---
title: Azure hesabı ile Office 365 için kaydolma | Microsoft Docs
description: Azure hesabı kullanarak nasıl Office 365 aboneliği oluşturulacağını öğrenin
author: JiangChen79
ms.reviewer: adwise
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: banders
ms.openlocfilehash: b38800a990056d7de0b6978573ddc19c80503a1d
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199934"
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Azure hesabınızla Office 365 aboneliğine kaydolma
Azure abonesiyseniz, Office 365 aboneliğine kaydolmak için Azure hesabınızı kullanabilirsiniz. Azure aboneliği olan bir kuruluşun parçasıysanız, mevcut Azure Active Directory’nizde (Azure AD) kullanıcılar için Office 365 abonelikleri oluşturabilirsiniz. Azure Active Directory kiracınızda Genel Yönetici veya Faturalama Yöneticisi izinlerine sahip olan bir hesap kullanarak Office 365’e kaydolun. Daha fazla bilgi için bkz. [Azure AD’de hesap izinlerimi denetleme](#RoleInAzureAD) ve [Azure Active Directory’de yönetici rolleri atama](../../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Zaten bir Office 365 hesabınız ve Azure aboneliğiniz varsa, [bir Office 365 kiracısını Azure aboneliği ile ilişkilendirebilirsiniz](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Azure hesabınızı kullanarak Office 365 aboneliği alma

1. [Office 365 ürün sayfasına](https://products.office.com/business) gidin ve bir plan seçin.
2. Sayfanın sağ üst köşesinde **Oturum aç**’a tıklayın.

    ![Office 365 deneme sayfasının ekran görüntüsü](./media/azure-account-for-office-365-subscription/12-office-365-trial-page.png)
3. Azure hesabı kimlik bilgilerinizle oturum açın. Kuruluşunuz için bir abonelik oluşturuyorsanız, Azure Active Directory kiracınızda Genel Yönetici veya Faturalama Yöneticisi dizin rolünün bir üyesi olan bir Azure hesabı kullanın.

    ![Office 365 oturum açma ekran görüntüsü](./media/azure-account-for-office-365-subscription/13-office-365-sign-in.png)
4. **Şimdi deneyin** öğesine tıklayın.

    ![Office 365 için siparişinizi onaylayan ekran görüntüsü.](./media/azure-account-for-office-365-subscription/14-office-365-confirm-your-order.png)
5. Sipariş alındı belgesi sayfasında **Devam** öğesine tıklayın.

    ![Office 365 sipariş alındı belgesinin ekran görüntüsü](./media/azure-account-for-office-365-subscription/15-office-365-order-receipt.png)

Şimdi işiniz tamamlanmıştır.
Kuruluşunuz için Office 365 aboneliği oluşturduysanız, Azure AD kullanıcılarınızın şimdi Office 365’te olup olmadığını denetlemek için aşağıdaki adımları kullanın.

1. Microsoft 365 yönetim merkezini açın.
2. **KULLANICILAR**’ı genişletin ve **Etkin Kullanıcılar**’a tıklayın.

    ![Microsoft 365 yönetim merkezi kullanıcılarının ekran görüntüsü](./media/azure-account-for-office-365-subscription/16-microsoft-365-admin-center-users.png)

Kaydolmanızın ardından Office 365 aboneliği, Azure aboneliğinizin ait olduğu aynı Azure Active Directory örneğine eklenir. Daha fazla bilgi için bkz. [Azure ve Office 365 abonelikleri hakkında daha fazla bilgi](office-365-account-for-azure-subscription.md#more-about-subs) ve [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Azure AD’de hesap izinlerimi denetleme
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmetler**’e tıklayın ve sonra **Active Directory** araması yapın.

    ![Azure portalında Active Directory ekran görüntüsü](./media/azure-account-for-office-365-subscription/billing-more-services-active-directory.png)
3. **Kullanıcılar ve gruplar** > **Tüm kullanıcılar** seçeneklerine tıklayın.
4. Kullanıcı adını seçin.

    ![Azure Active Directory kullanıcılarını gösteren ekran görüntüsü](./media/azure-account-for-office-365-subscription/billing-users-groups.png)

5. **Dizin rolü**’ne tıklayın.

    ![Azure portalı dizin rolünü gösteren ekran görüntüsü](./media/azure-account-for-office-365-subscription/billing-user-directory-role.png)
6.  Mevcut Azure Active Directory’nizdeki kullanıcılara ilişkin bir Office 365 aboneliği oluşturmak için **Genel yönetici** veya **Sınırlı yönetici** > **Faturalama yöneticisi** rolü gereklidir.

    ![Faturalama Yöneticisi Azure portalı dizin rolünü gösteren ekran görüntüsü](./media/azure-account-for-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
