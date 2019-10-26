---
title: Privileged Identity Management Azure AD rolleri Güvenlik Sihirbazı Azure Active Directory | Microsoft Docs
description: Kalıcı ayrıcalıklı Azure AD rol atamalarını Azure AD Privileged Identity Management (PıM) kullanarak uygun hale dönüştürmek için kullanabileceğiniz güvenlik sihirbazını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d1c9905047cb4b234bf80ba82f2395f72f67f1
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895213"
---
# <a name="azure-ad-roles-security-wizard-in-privileged-identity-management"></a>Privileged Identity Management 'de Azure AD rolleri Güvenlik Sihirbazı

Azure Active Directory (Azure AD) kuruluşunuzda Privileged Identity Management (PıM) kullanmaya yönelik ilk kişiyseniz, kullanmaya başlamak için bir sihirbaz sunulur. Sihirbaz, ayrıcalıklı kimliklerin güvenlik risklerini ve Privileged Identity Management bu riskleri azaltmak için nasıl kullanacağınızı anlamanıza yardımcı olur. Daha sonra yapmayı tercih ediyorsanız, sihirbazda mevcut rol atamalarında herhangi bir değişiklik yapmanız gerekmez.

## <a name="wizard-overview"></a>Sihirbaza genel bakış

Kuruluşunuz Privileged Identity Management kullanmaya başlamadan önce, tüm rol atamaları kalıcıdır: kullanıcılar, ayrıcalıklarına gerek duymasa bile her zaman bu rollerdir. Sihirbazın ilk adımı, yüksek ayrıcalıklı rollerin bir listesini ve şu anda bu rollerdeki Kullanıcı sayısını gösterir. Bir veya daha fazla bilginiz yoksa kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir rol detayına gidebilirsiniz.

Sihirbazın ikinci adımı, size yöneticinin rol atamalarını değiştirme olanağı sağlar.  

> [!WARNING]
> En az bir genel yöneticiniz ve bir kuruluş hesabıyla (Microsoft hesabı değil) birden fazla ayrıcalıklı rol yöneticisi olması önemlidir. Yalnızca bir ayrıcalıklı rol yöneticisi varsa, bu hesap silinirse kuruluş Privileged Identity Management yönetemez.
> Ayrıca, bir Kullanıcı bir Microsoft hesabı (başka bir deyişle, Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açmak için kullandıkları bir hesap) varsa rol atamalarını kalıcı tutun. Bu rol için etkinleştirme için çok faktörlü kimlik doğrulaması yapılmasını düşünüyorsanız, bu kullanıcı kilitlenir.

## <a name="run-the-wizard"></a>Sihirbazı çalıştırma

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri** ' ni seçin ve ardından **sihirbaz**' ı seçin.

    ![Azure AD rolleri-Sihirbazı çalıştırmak için 3 adım gösteren sihirbaz sayfası](./media/pim-security-wizard/wizard-start.png)

1. **1 ayrıcalıklı rol bul**' u seçin.

1. Hangi kullanıcıların kalıcı veya uygun olduğunu görmek için ayrıcalıklı rollerin listesini gözden geçirin.

    ![Ayrıcalıklı rolleri bul-kalıcı ve uygun üyeleri gösteren rol bölmesi](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Uygun hale getirmek istediğiniz kullanıcıları veya grupları seçmek için **İleri ' yi** seçin.

    ![Roller için uygun hale getirmek istediğiniz üyeleri seçmek üzere seçenekleri içeren uygun sayfaya Üyeler dönüştürün](./media/pim-security-wizard/convert-members-eligible.png)

1. Kullanıcıları veya grupları seçtikten sonra **İleri**' yi seçin.

    ![Dönüştürülecek kalıcı rol atamaları olan üyeleri gösteren değişiklikleri gözden geçirme sayfası](./media/pim-security-wizard/review-changes.png)

1. Kalıcı atamaları uygun öğesine dönüştürmek için **Tamam ' ı** seçin.

    Dönüştürme tamamlandığında bir bildirim görürsünüz.

    ![Dönüştürmenin durumunu gösteren bildirim](./media/pim-security-wizard/notification-completion.png)

Diğer ayrıcalıklı rol atamalarını uygun olarak dönüştürmeniz gerekiyorsa, Sihirbazı yeniden çalıştırabilirsiniz. Sihirbazı yerine Privileged Identity Management arabirimini kullanmak istiyorsanız, bkz. [Azure AD rollerini Privileged Identity Management atama](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [Privileged Identity Management yönetmek için diğer yöneticilere erişim izni verin](pim-how-to-give-access-to-pim.md)
