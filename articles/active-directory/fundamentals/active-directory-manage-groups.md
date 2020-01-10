---
title: Grupları kullanarak uygulama & kaynak erişimini yönetme-Azure AD
description: Kuruluşunuzun bulut tabanlı uygulamalar, şirket içi uygulamalar ve Azure Active Directory grupları kullanarak kaynaklara erişimi yönetme hakkında bilgi edinin.
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
ms.openlocfilehash: 961444e15ae1c45db1fc7423a6ac3cc96cc7b3fb
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75768019"
---
# <a name="manage-app-and-resource-access-using-azure-active-directory-groups"></a>Azure Active Directory grupları'nı kullanarak uygulama ve kaynak erişimini yönetme
Azure Active Directory (Azure AD), bulut tabanlı uygulamalarınıza, şirket içi uygulamalarınıza ve kaynaklarınıza erişimi yönetmek için grupları kullanmanıza olanak sağlar. Kaynaklarınız, Azure AD 'deki roller aracılığıyla nesneleri yönetme izinleri ya da hizmet olarak yazılım (SaaS) uygulamaları, Azure Hizmetleri, SharePoint siteleri ve şirket içi kaynaklar gibi kuruluş dışında, Azure AD kuruluşunun bir parçası olabilir.

>[!NOTE]
>Azure Active Directory'yi kullanmak için bir Azure hesabına ihtiyacınız vardır. Hesabınız yoksa [ücretsiz Azure hesabı için kaydolabilirsiniz](https://azure.microsoft.com/free/).
>
> Azure portal, üyelik ve Grup ayrıntılarını portalda yönetemeyeceğinizdeki bazı grupları görebilirsiniz:
>
> - Şirket içi Active Directory eşitlenen gruplar, yalnızca şirket içi Active Directory yönetilebilir.
> - Dağıtım listeleri ve posta etkin güvenlik grupları gibi diğer Grup türleri yalnızca Exchange Yönetim Merkezi veya Microsoft 365 Yönetim Merkezi 'nde yönetilir. Bu grupları yönetmek için Exchange Yönetim Merkezi veya Microsoft 365 Yönetim Merkezi ' nde oturum açmanız gerekir.

## <a name="how-access-management-in-azure-ad-works"></a>Azure AD 'de erişim yönetimi nasıl işe yarar?

Tek bir kullanıcı için erişim haklarını sağlayarak kuruluşunuzun kaynaklarına erişmesini veya grup için tüm Azure AD'yi azure AD yardımcı olur. Grupları kullanarak, kaynak sahibi (veya Azure AD directory sahibi), tek tek hakları sağlamak zorunda kalmak yerine bu grubun tüm üyeleri için erişim izinleri kümesi atama olanak tanır. Kaynak veya dizin sahibi de yönetim hakları üye listesi için bir bölüm Yöneticisi'ni veya bir Yardım Masası Yöneticisi, bu kişinin, ekleme ve gerektiğinde üyeleri kaldırma gibi başka bir kişiye verebilirsiniz. Grup sahiplerini yönetme hakkında daha fazla bilgi için bkz. [Grup sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md)

![Azure Active Directory erişim yönetimi diyagramı](./media/active-directory-manage-groups/active-directory-access-management-works.png)

## <a name="ways-to-assign-access-rights"></a>Erişim hakları atama yolları

Kullanıcıların erişim haklarını kaynak atamak için izleyebileceğiniz dört yol vardır:

- **Doğrudan atamayı.** Kaynak sahibi kullanıcı kaynağa doğrudan atar.

- **Grup ataması.** Kaynak sahibi Azure AD grubu için kaynak sağlayan otomatik olarak, tüm Grup üyeleri erişimi kaynağa atar. Grup üyeliği, Grup sahibi ve üye ekleme veya gruptan kaldırma ya da sahibi izin vererek kaynak sahibi tarafından yönetilir. Ekleme veya grup üyeliği kaldırma hakkında daha fazla bilgi için bkz. [nasıl yapılır: ekleme veya bir grup Azure Active Directory portalı kullanarak başka bir gruptan kaldırma](active-directory-groups-membership-azure-portal.md). 

- **Kural tabanlı atama.** Kaynak sahibi bir grup oluşturur ve hangi kullanıcıların belirli bir kaynağa atanmış tanımlamak için bir kural kullanır. Kural bireysel kullanıcılara atanan ve öznitelikleri temel alır. Kaynak sahibi, hangi öznitelikleri ve değerleri kaynak erişime izin vermek için gerekli olan belirleme kural yönetir. Daha fazla bilgi için [dinamik bir grup oluşturun ve durumunu denetlemek](../users-groups-roles/groups-create-rule.md).

    Oluşturma ve dinamik grupları kullanma hakkında hızlı bir açıklama bu kısa videoyu da izleyebilirsiniz:

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]

- **Dış yetkilisi atama.** Bir şirket içi dizin veya bir SaaS uygulaması gibi bir dış kaynaktan erişim gelir. Bu durumda, kaynağa erişim sağlamak için bir grup kaynak sahibi atar ve ardından Grup üyelerinin dış kaynağı yönetir.

   ![Erişim yönetimine genel bakış diyagramı](./media/active-directory-manage-groups/access-management-overview.png)

## <a name="can-users-join-groups-without-being-assigned"></a>Kullanıcılar, gruplar atanmadan birleştirebilir miyim?
Grup sahibi katılma, atamak yerine kendi grupları bulmak izin verebilirsiniz. Katılan tüm kullanıcılar otomatik olarak kabul etme veya onay gerektirmesini sahibi grubu de ayarlayabilirsiniz.

Bir kullanıcı bir gruba katılma isteklerini sonra isteği Grup sahibine iletilir. Gerekliyse, sahibi, isteği onaylayabilir ve kullanıcının grup üyeliğini bildirilir. Ancak, birden fazla sahibe varsa ve bunlardan birinin disapproves kullanıcı bildirilir ancak grubuna eklenmez. Daha fazla bilgi ve kullanıcılarınızın bu gruplara katılmak için istek izin vermek hakkında yönergeler için bkz. [kullanıcılar bu gruplara katılmak için isteyebilir için Azure AD'yi ayarlarken ayarlayın](../users-groups-roles/groups-self-service-management.md)

## <a name="next-steps"></a>Sonraki adımlar
Biraz erişim yönetim gruplarını kullanarak bir giriş olduğuna göre kaynakları ve uygulamaları yönetmek başlatın.

- [Azure Active Directory kullanarak yeni bir grup oluşturmak](active-directory-groups-create-azure-portal.md) veya [oluşturma ve PowerShell cmdlet'lerini kullanarak yeni bir grubu yönetme](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Tümleşik bir SaaS uygulamasına erişim atamak için grupları kullanma](../users-groups-roles/groups-saasapps.md)

- [Azure AD Connect kullanarak Azure için bir şirket içi Grup eşitleme](../hybrid/whatis-hybrid-identity.md)
