---
title: Ayrıcalıklı Kimlik Yönetiminde Yönetemeyeceğiniz Roller - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) yönetemeyeceğiniz rolleri açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 03/31/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6eaa50f57dd8037ef0ad96b69284f565bd3558f
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80607534"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminde Yönetilemediğiniz Roller

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), tüm [Azure AD rollerini](../users-groups-roles/directory-assign-admin-roles.md) ve tüm Azure [rollerini](../../role-based-access-control/built-in-roles.md)yönetmenize olanak tanır. Azure rolleri, yönetim gruplarınıza, aboneliklerinize, kaynak gruplarınıza ve kaynaklarınıza bağlı özel rollerinizi de içerebilir. Ancak, yönetemeyeceğiniz birkaç rol vardır. Bu makalede, Ayrıcalıklı Kimlik Yönetimi'nde yönetilemediğiniz roller açıklanmaktadır.

## <a name="classic-subscription-administrator-roles"></a>Klasik abonelik yönetici rolleri

Ayrıcalıklı Kimlik Yönetimi'nde aşağıdaki klasik abonelik yöneticisi rollerini yönetemezsiniz:

- Hesap Yöneticisi
- Hizmet Yöneticisi
- Ortak Yönetici

Klasik abonelik yöneticisi rolleri hakkında daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yöneticisi rolleri.](../../role-based-access-control/rbac-and-directory-admin-roles.md)

## <a name="what-about-office-365-admin-roles"></a>Peki ya Office 365 yönetici rolleri?

Exchange Administrator ve SharePoint Administrator gibi Azure AD Rolleri ve Yöneticiler portalı deneyimindeki tüm Office365 rollerini destekliyoruz, ancak Exchange RBAC veya SharePoint RBAC içindeki belirli rolleri desteklemeyiz. Bu Office 365 hizmetleri hakkında daha fazla bilgi için [Bkz. Office 365 yönetici rolleri.](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles)

> [!NOTE]
> SharePoint Administrator rolü için uygun kullanıcılar ve Microsoft Güvenlik ve Uyumluluk Merkezi'ne erişmeye çalışan roller, rollerini etkinleştirdikten sonra birkaç saate kadar gecikmeler yaşayabilir. Sorunları çözmek için bu ekiplerle birlikte çalışıyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)
