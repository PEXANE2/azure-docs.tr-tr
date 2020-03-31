---
title: Microsoft 365 hizmetlerinde yönetici rol dokümanları - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'de Microsoft 365 hizmetleri için yönetici rolleri için içerik ve API başvuruları bulma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 430b65217da33185cd3b5c235fb148f3350e1bfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74024449"
---
# <a name="administrator-roles-for-microsoft-365-services"></a>Microsoft 365 hizmetleri için yönetici rolleri

Microsoft 365'teki tüm ürünler Azure AD'deki yönetimsel rollerle yönetilebilir. Bazı ürünler de bu ürüne özgü ek roller sağlar. Her ürün tarafından desteklenen roller hakkında bilgi için aşağıdaki tabloya bakın. Delegasyon konularının genel tartışmaları [Azure Active Directory'deki Rol delegasyonu planlamasında](roles-concept-delegation.md)bulunabilir.

## <a name="where-to-find-content"></a>İçeriği nerede bulabilirsiniz?

Microsoft 365 hizmeti | Rol içeriği | API içeriği
---------------------- | ------------------ | -----------------
Office 365 ve Microsoft 365 iş planlarında yönetici rolleri | [Office 365 yönetici rolleri](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | Kullanılamaz
Azure Etkin Dizin (Azure AD) ve Azure AD Kimlik Koruması| [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Rol tabanlı erişim denetimini değiştirme](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help) |  [Değişim için PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamaları getir](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Azure AD yönetici rolleri](directory-assign-admin-roles.md)<br>Ayrıca [Office 365'teki SharePoint yönetici rolü hakkında](https://docs.microsoft.com/sharepoint/sharepoint-admin-role) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
İşletmeler için Takımlar/Skype | [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Güvenlik & Uyumluluk Merkezi (Office 365 Gelişmiş Tehdit Koruması, Değişim Çevrimiçi Koruma, Bilgi Koruma) | [Office 365 yönetici rolleri](https://docs.microsoft.com/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Rol atamaları getir](https://docs.microsoft.com/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Güvenlik Puanı | [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Uyumluluk Yöneticisi | [Uyumluluk Yöneticisi rolleri](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Kullanılamaz
Azure Information Protection | [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Rol tabanlı erişim denetimi](https://docs.microsoft.com/cloud-app-security/manage-admins) | [API başvurusu](https://docs.microsoft.com/cloud-app-security/api-tokens) 
Azure Gelişmiş Tehdit Koruması | [Azure ATP rol grupları](https://docs.microsoft.com/azure-advanced-threat-protection/atp-role-groups) | Kullanılamaz
Windows Defender Gelişmiş Tehdit Koruması | [Windows Defender ATP rol tabanlı erişim kontrolü](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Kullanılamaz
Privileged Identity Management | [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Rol tabanlı erişim denetimine uyum sağlar](https://docs.microsoft.com/intune/role-based-access-control) | [Grafik API](https://docs.microsoft.com/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Yönetilen Masaüstü | [Azure AD yönetici rolleri](directory-assign-admin-roles.md) | [Grafik API](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0)<br>[Rol atamaları getir](https://docs.microsoft.com/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD yöneticisi rollerini atama veya kaldırma](directory-manage-roles-portal.md)
* [Azure AD yöneticisi rolleri başvurusu](directory-assign-admin-roles.md)
