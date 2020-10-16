---
title: Privileged Identity Management eski Güvenlik Sihirbazı 'nda Azure AD rolleri bulma ve öngörüleri (Önizleme)-Azure Active Directory
description: Bulma ve Öngörüler (eski adıyla Güvenlik Sihirbazı), kalıcı Azure AD rol atamalarını Privileged Identity Management tam zamanında atamalara dönüştürmenize yardımcı olur.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 09/01/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff5d15ef66b597fdf56fefe90f35cbf122bb093f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91534446"
---
# <a name="discovery-and-insights-preview-for-azure-ad-roles-formerly-security-wizard"></a>Azure AD rolleri için bulma ve Öngörüler (Önizleme) (eski adıyla Güvenlik Sihirbazı)

Azure Active Directory (Azure AD) kuruluşunuzda Privileged Identity Management (PıM) kullanıyorsanız, kullanmaya başlamak için **bulma ve Öngörüler (Önizleme)** sayfasını kullanabilirsiniz. Bu özellik, kuruluşunuzdaki ayrıcalıklı rollere kimlerin atandığını ve kalıcı rol atamalarını tam zamanında atamalara hızlıca değiştirmek için PıM 'yi nasıl kullanacağınızı gösterir. **Bulma ve Öngörüler (Önizleme)** içinde kalıcı ayrıcalıklı rol atamalarınızı görüntüleyebilir veya değişiklikler yapabilirsiniz. Bu bir çözümleme aracıdır ve bir eylem aracıdır.

## <a name="discovery-and-insights-preview"></a>Bulma ve Öngörüler (Önizleme)

Kuruluşunuz Privileged Identity Management kullanmaya başlamadan önce, tüm rol atamaları kalıcıdır. Kullanıcılar, ayrıcalıklarına gerek duymadığı durumlarda bile her zaman kendilerine atanan rollerdir. Önceki Güvenlik Sihirbazı 'Nın yerini alan bulma ve Öngörüler (Önizleme), ayrıcalıklı rollerin bir listesini ve şu anda bu rollerdeki Kullanıcı sayısını gösterir. Bir veya daha fazla bilginiz yoksa atanan kullanıcılar hakkında daha fazla bilgi edinmek için, bir rolün atamalarını listeleyebilirsiniz.

: heavy_check_mark: **Microsoft** , genel yönetici rolüne kalıcı olarak atanmış iki kesme camı hesabını tutmanızı önerir. Bu hesapların, [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)bölümünde açıklandığı gibi, oturum açmak için normal yönetim hesaplarınızla aynı Multi-Factor Authentication mekanizmasına gerek olmadığından emin olun.

Ayrıca, bir Kullanıcı bir Microsoft hesabı (başka bir deyişle, Skype veya Outlook.com gibi Microsoft hizmetlerinde oturum açmak için kullandıkları bir hesap) varsa rol atamalarını kalıcı tutun. Rol atamasını etkinleştirmek üzere Microsoft hesabı olan bir kullanıcı için Multi-Factor Authentication gerekirse, Kullanıcı kilitlenir.

## <a name="open-discovery-and-insights-preview"></a>Keşif ve Öngörüler 'i açma (Önizleme)

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri** ' ni seçin ve ardından **bulma ve Öngörüler ' i (Önizleme)** seçin. Sayfayı açmak ilgili rol atamalarını bulmak için bulma işlemini başlatır.

    ![Azure AD rolleri-3 seçenekleri gösteren bulma ve Öngörüler sayfası](./media/pim-security-wizard/new-preview-link.png)

1. **Küresel yöneticileri azalt**' ı seçin.

    !["Bulma ve Öngörüler (Önizleme)" öğesini "genel yöneticileri azalt" eylemi seçili olarak gösteren ekran görüntüsü.](./media/pim-security-wizard/new-preview-page.png)

1. Genel yönetici rolü atamalarının listesini gözden geçirin.

    ![Tüm genel yöneticileri gösteren genel Yöneticiler-roller bölmesini azaltma](./media/pim-security-wizard/new-global-administrator-list.png)

1. **İleri** ' yi seçerek uygun hale getirmek istediğiniz kullanıcıları veya grupları seçin ve ardından **uygun yap** veya **Atamayı Kaldır**' ı seçin.

    ![Roller için uygun hale getirmek istediğiniz üyeleri seçmek üzere seçenekleri içeren uygun sayfaya Üyeler dönüştürün](./media/pim-security-wizard/new-global-administrator-buttons.png)

1. Ayrıca, tüm genel yöneticilerin kendi erişimini gözden geçirmesine gerek vardır.

    ![Erişim İncelemeleri bölümünü gösteren genel Yöneticiler sayfası](./media/pim-security-wizard/new-global-administrator-access-review.png)

1. Bu değişikliklerden herhangi birini seçtikten sonra bir Azure bildirimi görürsünüz.

1. Daha sonra, diğer ayrıcalıklı rollerde ve hizmet sorumlusu rolü atamalarında yukarıdaki adımları yinelemek için, **erişimi ortadan kaldırma** veya **hizmet sorumlularını gözden geçirme** ' yi seçebilirsiniz. Hizmet sorumlusu rol atamaları için yalnızca rol atamalarını kaldırabilirsiniz.

    ![Erişimi ortadan kaldırmaya ve hizmet sorumlularını incelemeye yönelik ek Öngörüler seçenekleri ](./media/pim-security-wizard/new-preview-page-service-principals.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [Privileged Identity Management yönetmek için diğer yöneticilere erişim izni verin](pim-how-to-give-access-to-pim.md)
