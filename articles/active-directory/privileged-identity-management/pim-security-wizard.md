---
title: PıM-Azure Active Directory 'de Azure AD rolleri Güvenlik Sihirbazı | Microsoft Docs
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
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d6d94df29ba16ecee06d70f5edac15a96a4299d
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804022"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>PıM 'de Azure AD rolleri Güvenlik Sihirbazı

Kuruluşunuz için Azure Active Directory (Azure AD) Privileged Identity Management (PıM) ilk kişiyseniz, bir sihirbaz görüntülenir. Sihirbaz, ayrıcalıklı kimliklerin güvenlik risklerini ve bu riskleri azaltmak için PıM 'yi nasıl kullanacağınızı anlamanıza yardımcı olur. Daha sonra yapmayı tercih ediyorsanız, sihirbazda mevcut rol atamalarında herhangi bir değişiklik yapmanız gerekmez.

## <a name="wizard-overview"></a>Sihirbaza genel bakış

Kuruluşunuz PıM 'yi kullanmaya başlamadan önce, tüm rol atamaları kalıcıdır: kullanıcılar, ayrıcalıklarına gerek duymasa bile her zaman bu rollerdir. Sihirbazın ilk adımı, yüksek ayrıcalıklı rollerin bir listesini ve şu anda bu rollerdeki Kullanıcı sayısını gösterir. Bir veya daha fazla bilginiz yoksa kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir rol detayına gidebilirsiniz.

Sihirbazın ikinci adımı, size yöneticinin rol atamalarını değiştirme olanağı sağlar.  

> [!WARNING]
> En az bir genel yöneticiniz ve bir kuruluş hesabıyla (Microsoft hesabı değil) birden fazla ayrıcalıklı rol yöneticisi olması önemlidir. Yalnızca bir ayrıcalıklı rol yöneticisi varsa, bu hesap silinirse kuruluş PıM 'yi yönetemez.
> Ayrıca, bir Kullanıcı bir Microsoft hesabı (Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açmak için kullandıkları bir hesap) varsa rol atamalarını kalıcı tutun. Bu rolün etkinleştirilmesi için MFA gerektirmeyi düşünüyorsanız, bu kullanıcı kilitlenir.

## <a name="run-the-wizard"></a>Sihirbazı çalıştırma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri** ' ne tıklayın ve ardından **sihirbaz**' a tıklayın.

    ![Azure AD rolleri-Sihirbazı çalıştırmak için 3 adım gösteren sihirbaz sayfası](./media/pim-security-wizard/wizard-start.png)

1. **1. ayrıcalıklı rolleri bul**' a tıklayın.

1. Hangi kullanıcıların kalıcı veya uygun olduğunu görmek için ayrıcalıklı rollerin listesini gözden geçirin.

    ![Ayrıcalıklı rolleri bul-kalıcı ve uygun üyeleri gösteren rol bölmesi](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. **İleri** ' ye tıklayarak uygun hale getirmek istediğiniz üyeleri seçin.

    ![Roller için uygun hale getirmek istediğiniz üyeleri seçmek üzere seçenekleri içeren uygun sayfaya Üyeler dönüştürün](./media/pim-security-wizard/convert-members-eligible.png)

1. Üyeleri seçtikten sonra **İleri**' ye tıklayın.

    ![Dönüştürülecek kalıcı rol atamaları olan üyeleri gösteren değişiklikleri gözden geçirme sayfası](./media/pim-security-wizard/review-changes.png)

1. Kalıcı atamaları uygun hale dönüştürmek için **Tamam** ' ı tıklatın.

    Dönüştürme tamamlandığında bir bildirim görürsünüz.

    ![Dönüştürmenin durumunu gösteren bildirim](./media/pim-security-wizard/notification-completion.png)

Diğer ayrıcalıklı rol atamalarını uygun olarak dönüştürmeniz gerekiyorsa, Sihirbazı yeniden çalıştırabilirsiniz. Sihirbaz yerine PıM arabirimini kullanmak istiyorsanız, bkz. [PIM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [PıM 'yi yönetmek için diğer yöneticilere erişim izni verme](pim-how-to-give-access-to-pim.md)
