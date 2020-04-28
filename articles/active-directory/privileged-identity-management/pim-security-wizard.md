---
title: Azure AD, PIM'de güvenlik sihirbazı rol aldı - Azure Active Directory | Microsoft Dokümanlar
description: Kalıcı ayrıcalıklı Azure AD rol atamalarını Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) kullanarak uygun kullanıma dönüştürmek için kullanabileceğiniz güvenlik sihirbazını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/21/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9f12b2b31da4c7fe67eef9674d96b517d4e2bfa
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867746"
---
# <a name="azure-ad-roles-security-wizard-in-privileged-identity-management"></a>Azure AD, Ayrıcalıklı Kimlik Yönetimi'nde güvenlik sihirbazı rollerini üstlendi

Azure Etkin Dizin (Azure AD) kuruluşunuzda Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanan ilk kişiyseniz, başlamak için size bir sihirbaz sunulur. Sihirbaz, ayrıcalıklı kimliklerin güvenlik risklerini ve bu riskleri azaltmak için Ayrıcalıklı Kimlik Yönetimi'ni nasıl kullanacağınızı anlamanıza yardımcı olur. Daha sonra yapmayı tercih ederseniz, sihirbazdaki varolan rol atamalarında herhangi bir değişiklik yapmanız gerekmez.

> [!Important]
> Güvenlik sihirbazı geçici olarak kullanılamıyor. Sabrınız için teşekkürler.

## <a name="wizard-overview"></a>Sihirbaza genel bakış

Kuruluşunuz Ayrıcalıklı Kimlik Yönetimi'ni kullanmaya başlamadan önce, tüm rol atamaları kalıcıdır: kullanıcılar şu anda ayrıcalıklarına ihtiyaç duymasalar bile her zaman bu rollerdedir. Sihirbazın ilk adımı, yüksek ayrıcalıklı rollerin ve bu rollerde şu anda kaç kullanıcının bulunduğuna ait bir liste gösterir. Kullanıcılardan biri veya daha fazlası yabancıysa, kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir role sondaj yapabilirsiniz.

Sihirbazın ikinci adımı, yöneticinin rol atamalarını değiştirme fırsatı verir.  

> [!WARNING]
> En az bir Global yöneticiniz ve bir iş veya okul hesabı olan birden fazla Ayrıcalıklı rol yöneticiniz (Microsoft hesabı değil) olması önemlidir. Yalnızca bir Ayrıcalıklı rol yöneticisi varsa, bu hesap silinirse kuruluş Ayrıcalıklı Kimlik Yönetimi'ni yönetemez.
> Ayrıca, bir kullanıcının Microsoft hesabı varsa (diğer bir deyişle, Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açmada kullandıkları bir hesap) rol atamalarıkalıcı tutun. Bu rol için etkinleştirme için çok faktörlü kimlik doğrulaması gerektirmeyi planlıyorsanız, bu kullanıcı kilitlenir.

## <a name="run-the-wizard"></a>Sihirbazı çalıştırma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini** seçin ve ardından **Sihirbaz'ı**seçin.

    ![Azure AD rolleri - Sihirbazı çalıştırmak için 3 adımı gösteren sihirbaz sayfası](./media/pim-security-wizard/wizard-start.png)

1. **1 Ayrıcalıklı rolleri keşfedin' i**seçin.

1. Hangi kullanıcıların kalıcı veya uygun olduğunu görmek için ayrıcalıklı rollerin listesini gözden geçirin.

    ![Ayrıcalıklı rolleri keşfedin - Kalıcı ve uygun üyeleri gösteren rol bölmesi](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Uygun hale getirmek istediğiniz kullanıcıları veya grupları seçmek için **İleri'yi** seçin.

    ![Roller için uygun hale getirmek istediğiniz üyeleri seçmek için seçenekleri olan uygun sayfaya üyeleri dönüştürme](./media/pim-security-wizard/convert-members-eligible.png)

1. Kullanıcıları veya grupları seçtikten sonra **İleri'yi**seçin.

    ![Dönüştürülecek kalıcı rol atamaları olan üyeleri gösteren değişiklikleri gözden geçirme sayfası](./media/pim-security-wizard/review-changes.png)

1. Kalıcı atamaları uygun alabilmek için **Tamam'ı** seçin.

    Dönüşüm tamamlandığında, bir bildirim görürsünüz.

    ![Dönüşüm durumunu gösteren bildirim](./media/pim-security-wizard/notification-completion.png)

Diğer ayrıcalıklı rol atamalarını uygun alete dönüştürmeniz gerekiyorsa, sihirbazı yeniden çalıştırabilirsiniz. Sihirbaz yerine Ayrıcalıklı Kimlik Yönetimi arabirimini kullanmak istiyorsanız, [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rollerini atay'a](pim-how-to-add-role-to-user.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
- [Ayrıcalıklı Kimlik Yönetimini yönetmek için diğer yöneticilere erişim izni verme](pim-how-to-give-access-to-pim.md)
