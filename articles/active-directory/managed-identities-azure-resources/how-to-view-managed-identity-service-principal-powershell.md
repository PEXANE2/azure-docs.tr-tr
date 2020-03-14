---
title: PowerShell kullanarak yönetilen kimliğin hizmet sorumlusunu görüntüleme-Azure AD
description: PowerShell kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemek için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33fc6fbfd7c47b5809e8aff8ee9806c9eeac1162
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79298691"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>PowerShell kullanarak yönetilen kimliğin hizmet sorumlusunu görüntüleme

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemeyi öğreneceksiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın.
- Henüz bir Azure hesabınız yoksa [ücretsiz hesap için kaydolun](https://azure.microsoft.com/free/).
- [Bir sanal makine veya uygulama üzerinde sistem tarafından atanan kimliği](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) etkinleştirin. [application](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)
- [Azure PowerShell](/powershell/azure/install-az-ps) en son sürümünü yükler

## <a name="view-the-service-principal"></a>Hizmet sorumlusunu görüntüleme

Aşağıdaki komut, sistem tarafından atanan kimlik etkin bir VM veya uygulamanın hizmet sorumlusunu görüntülemeyi gösterir. `<VM or application name>` kendi değerlerinizle değiştirin.

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak Azure AD hizmet sorumlularını görüntüleme hakkında daha fazla bilgi için bkz. [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).


