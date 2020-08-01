---
title: Azure Active Directory bir gruba atanmış rolleri görüntüle | Microsoft Docs
description: Kimlik yönetimi temsilcisi seçme için özel Azure AD rollerini önizleyin. Azure portal, PowerShell veya Graph API Azure rollerini yönetin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7613fb70299a70e4389b97c2647a26cb7c3374
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87476214"
---
# <a name="view-roles-assigned-to-a-group-in-azure-active-directory"></a>Azure Active Directory bir gruba atanan rolleri görüntüleme

Bu bölümde, bir gruba atanan rollerin Azure AD Yönetim Merkezi kullanılarak nasıl görüntülenebileceğini açıklar. Grupları ve atanan rolleri görüntüleme varsayılan kullanıcı izinleridir.

1. Yönetici olmayan veya yönetici kimlik bilgileriyle [Azure AD Yönetim merkezinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oturum açın.

1. İlgilendiğiniz grubu seçin.

1. **Atanan roller**' i seçin. Artık bu gruba atanmış tüm Azure AD rollerini görebilirsiniz.

   ![Seçili bir gruba atanan tüm rolleri görüntüle](./media/roles-groups-view-assignments/view-assignments.png)

## <a name="using-powershell"></a>PowerShell’i kullanma

### <a name="get-object-id-of-the-group"></a>Grubun nesne KIMLIĞINI al

```powershell
Get-AzureADMSGroup -SearchString “Contoso_Helpdesk_Administrators”
```

### <a name="view-role-assignment-to-a-group"></a>Rol atamasını bir gruba görüntüle

```powershell
Get-AzureADMSRoleAssignment -Filter "principalId eq '<object id of group>" 
```

## <a name="using-microsoft-graph-api"></a>Microsoft Graph API 'sini kullanma

### <a name="get-object-id-of-the-group"></a>Grubun nesne KIMLIĞINI al

```powershell
GET https://graph.microsoft.com/beta/groups?$filter displayName eq ‘Contoso_Helpdesk_Administrator’ 
```

### <a name="get-role-assignments-to-a-group"></a>Gruba rol atamaları al

```powershell
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments?$filter=principalId eq
```

## <a name="next-steps"></a>Sonraki adımlar

- [Rol atamalarını yönetmek için bulut gruplarını kullanma](roles-groups-concept.md)
- [Bulut gruplarına atanan rol sorunlarını giderme](roles-groups-faq-troubleshooting.md)
