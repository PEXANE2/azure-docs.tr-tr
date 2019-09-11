---
title: PıM-Azure Active Directory 'de yönetemezsiniz roller | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde yönetebileceğiniz rolleri açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 01/18/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: d66d433d9de537358777e54e3c7d5489c25c849b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804075"
---
# <a name="roles-you-cannot-manage-in-pim"></a>PıM 'de yöneteceğiniz roller

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), tüm [Azure AD rollerini](../users-groups-roles/directory-assign-admin-roles.md) ve tüm [Azure Kaynak rollerini](../../role-based-access-control/built-in-roles.md)yönetmenize olanak sağlar. Bu roller ayrıca yönetim gruplarınız, abonelikleriniz, kaynak grupları ve kaynaklarınıza iliştirilmiş özel rollerinizi de içerir. Ancak, yönetebilmeniz gereken birkaç rol vardır. Bu makalede, PıM 'de yöneteceğiniz roller açıklanmaktadır.

## <a name="classic-subscription-administrator-roles"></a>Klasik abonelik yöneticisi rolleri

PıM 'de aşağıdaki klasik abonelik yöneticisi rollerini yönetemezsiniz:

- Hesap Yöneticisi
- Hizmet Yöneticisi
- Ortak Yönetici

Klasik abonelik yöneticisi rolleri hakkında daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>Office 365 Yönetici rolleri hakkında ne olacak?

Exchange Yöneticisi ve SharePoint Yöneticisi dışındaki Exchange Online veya SharePoint Online içindeki roller, Azure AD 'de temsil edilmez ve bu nedenle PıM 'de yönetilemez. Bu Office 365 hizmetleri hakkında daha fazla bilgi için bkz. [office 365 Yönetici rolleri](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> SharePoint Yöneticisi SharePoint Online Yönetim Merkezi aracılığıyla SharePoint Online 'a yönetici erişimine sahiptir ve SharePoint Online 'da neredeyse tüm görevleri gerçekleştirebilir. Uygun kullanıcılar, PıM 'de etkinleştirdikten sonra SharePoint içindeki bu rolü kullanarak gecikmelere neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [PıM 'de Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)
