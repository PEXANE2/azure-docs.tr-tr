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
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fac7074cf85a585c93ece60be9eea8ffb9a6345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72895198"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminde Yönetilemediğiniz Roller

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), tüm [Azure AD rollerini](../users-groups-roles/directory-assign-admin-roles.md) ve tüm Azure kaynak [rollerini](../../role-based-access-control/built-in-roles.md)yönetmenize olanak tanır. Bu roller, yönetim gruplarınıza, aboneliklerinize, kaynak gruplarınıza ve kaynaklarınıza bağlı özel rollerinizi de içerir. Ancak, yönetemeyeceğiniz birkaç rol vardır. Bu makalede, Ayrıcalıklı Kimlik Yönetimi'nde yönetemeyeceğiniz roller açıklanmaktadır.

## <a name="classic-subscription-administrator-roles"></a>Klasik abonelik yönetici rolleri

Ayrıcalıklı Kimlik Yönetimi'nde aşağıdaki klasik abonelik yöneticisi rollerini yönetemezsiniz:

- Hesap Yöneticisi
- Hizmet Yöneticisi
- Ortak Yönetici

Klasik abonelik yöneticisi rolleri hakkında daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yöneticisi rolleri.](../../role-based-access-control/rbac-and-directory-admin-roles.md)

## <a name="what-about-office-365-admin-roles"></a>Peki ya Office 365 yönetici rolleri?

Exchange Administrator ve SharePoint Administrator dışındaki Exchange Online veya SharePoint Online'daki roller Azure AD'de temsil edilmez ve bu nedenle Ayrıcalıklı Kimlik Yönetimi'nde yönetilemez. Bu Office 365 hizmetleri hakkında daha fazla bilgi için [Bkz. Office 365 yönetici rolleri.](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles)

> [!NOTE]
> SharePoint Administrator, SharePoint Online yönetici merkezi aracılığıyla SharePoint Online'a yönetim erişimine sahiptir ve SharePoint Online'da hemen hemen her görevi gerçekleştirebilir. Uygun kullanıcılar, Ayrıcalıklı Kimlik Yönetimi'ni etkinleştirdikten sonra SharePoint içinde bu rolü kullanırken gecikmeler yaşayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)
