---
title: Grupları kullanarak uygulama & kaynak erişimini yönetme - Azure AD
description: Azure Active Directory gruplarını kullanarak kuruluşunuzun bulut tabanlı uygulamalarına, şirket içi uygulamalarına ve kaynaklarına erişimi nasıl yönetebilirsiniz hakkında bilgi edinin.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: ajburnle
ms.reviewer: piotrci
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89c4fa536994ecc8cecb50c907d43df2110be7df
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982608"
---
# <a name="manage-app-and-resource-access-using-azure-active-directory-groups"></a>Azure Active Directory gruplarını kullanarak uygulama ve kaynak erişimini yönetme
Azure Etkin Dizin (Azure AD), bulut tabanlı uygulamalarınıza, şirket içi uygulamalarınıza ve kaynaklarınıza erişimi yönetmek için grupları kullanmanıza olanak tanır. Kaynaklarınız, Azure AD'deki roller aracılığıyla veya Hizmet Olarak Yazılım (SaaS) uygulamaları, Azure hizmetleri, SharePoint siteleri ve şirket içi kaynaklar gibi kuruluş dışında nesneleri yönetme izinleri gibi Azure AD kuruluşunun bir parçası olabilir.

>[!NOTE]
> Azure portalında, üyelik ve grup bilgilerini portalda yönetemediğiniz bazı grupları görebilirsiniz:
>
> - Şirket içi Active Directory'den senkronize edilen gruplar yalnızca şirket içi Active Directory'de yönetilebilir.
> - Dağıtım listeleri ve posta yla etkin güvenlik grupları gibi diğer grup türleri yalnızca Exchange yönetici merkezinde veya Microsoft 365 yönetici merkezinde yönetilir. Bu grupları yönetmek için Exchange yönetici merkezi veya Microsoft 365 yönetici merkezinde oturum açmanız gerekir.

## <a name="how-access-management-in-azure-ad-works"></a>Azure AD'de erişim yönetimi nasıl çalışır?

Azure AD, tek bir kullanıcıya veya tüm Azure REKLAM grubuna erişim hakları sağlayarak kuruluşunuzun kaynaklarına erişim sağlamanıza yardımcı olur. Grupların kullanılması kaynak sahibinin (veya Azure AD dizin sahibinin) hakları tek tek sağlamak zorunda kalmadan grubun tüm üyelerine erişim izinleri atayabilmesini sağlar. Kaynak veya dizin sahibi, üye listesi için yönetim haklarını bölüm yöneticisi veya Yardım Masası yöneticisi gibi başka birine de verebilir ve gerektiğinde bu kişinin üye eklemesine ve kaldırmasına izin verebilir. Grup sahiplerini yönetme hakkında daha fazla bilgi için [bkz.](active-directory-accessmanagement-managing-group-owners.md)

![Azure Active Directory erişim yönetimi diyagramı](./media/active-directory-manage-groups/active-directory-access-management-works.png)

## <a name="ways-to-assign-access-rights"></a>Erişim hakları atama yolları

Kullanıcılarınıza kaynak erişim hakları atamanın dört yolu vardır:

- **Doğrudan atama.** Kaynak sahibi kullanıcıyı doğrudan kaynağa atar.

- **Grup ödevi.** Kaynak sahibi kaynağa, tüm grup üyelerinin kaynağa otomatik olarak erişmelerini sağlayan bir Azure REKLAM grubu atar. Grup üyeliği, hem grup sahibi hem de kaynak sahibi tarafından yönetilir ve sahipin in gruba üye eklemesini veya kaldırmasını izin verir. Grup üyeliği ekleme veya kaldırma hakkında daha fazla bilgi için [bkz: Azure Etkin Dizin portalını kullanarak başka bir gruptan grup ekleme veya kaldırma.](active-directory-groups-membership-azure-portal.md) 

- **Kural tabanlı atama.** Kaynak sahibi bir grup oluşturur ve belirli bir kaynağa hangi kullanıcıların atandığını tanımlamak için bir kural kullanır. Kural, tek tek kullanıcılara atanan özniteliklere dayanır. Kaynak sahibi, kaynağa erişime izin vermek için hangi özniteliklerin ve değerlerin gerekli olduğunu belirleyerek kuralı yönetir. Daha fazla bilgi için dinamik [bir grup oluştur ve durumu denetle'](../users-groups-roles/groups-create-rule.md)ye bakın.

    Dinamik gruplar oluşturma ve kullanma hakkında hızlı bir açıklama için bu kısa videoyu da izleyebilirsiniz:

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]

- **Dış otorite ataması.** Erişim, şirket içi dizini veya SaaS uygulaması gibi harici bir kaynaktan gelir. Bu durumda, kaynak sahibi kaynağa erişim sağlamak için bir grup atar ve sonra dış kaynak grup üyelerini yönetir.

   ![Erişim yönetimine genel bakış diyagramı](./media/active-directory-manage-groups/access-management-overview.png)

## <a name="can-users-join-groups-without-being-assigned"></a>Kullanıcılar atanmadan gruplara katılabilir mi?
Grup sahibi, kullanıcıların bunları atamak yerine katılmak için kendi gruplarını bulmasına izin verebilir. Sahibi, katılan tüm kullanıcıları otomatik olarak kabul etmek veya onay istemek için grubu da ayarlayabilir.

Bir kullanıcı gruba katılmak istedikten sonra, istek grup sahibine iletilir. Gerekirse, sahibi isteği onaylayabilir ve kullanıcı grup üyeliği bildirilir. Ancak, birden çok sahibiniz varsa ve bunlardan biri onaylamıyorsa, kullanıcı bildirilir, ancak gruba eklenmez. Kullanıcılarınızın gruplara katılmak için nasıl istekte bulunacağına ilişkin daha fazla bilgi ve yönergeler için, [kullanıcıların gruplara katılma isteğinde bulunabilmesi için Azure REKLAM'ı ayarlama'ya](../users-groups-roles/groups-self-service-management.md) bakın

## <a name="next-steps"></a>Sonraki adımlar
Artık grupları kullanarak yönetime erişim ekine biraz giriş yaptığınıza göre, kaynaklarınızı ve uygulamalarınızı yönetmeye başlarsınız.

- [Azure Etkin Dizin'i kullanarak yeni bir grup oluşturun](active-directory-groups-create-azure-portal.md) veya [PowerShell cmdlets kullanarak yeni bir grup oluşturun ve yeni bir grup oluşturun](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Tümleşik bir SaaS uygulamasına erişim atamak için grupları kullanma](../users-groups-roles/groups-saasapps.md)

- [Azure AD Connect'i kullanarak şirket içi grubu Azure ile senkronize etme](../hybrid/whatis-hybrid-identity.md)
