---
title: PowerShell - Azure AD'yi kullanarak yönetilen bir kimliğin hizmet ilkesini görüntüleyin
description: PowerShell kullanarak yönetilen bir kimliğin hizmet ilkesini görüntülemek için adım adım talimatlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298691"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>PowerShell'i kullanarak yönetilen bir kimliğin hizmet ilkesini görüntüleyin

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, PowerShell'i kullanarak yönetilen bir kimliğin hizmet ilkesini nasıl görüntüleyebilirsiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın.
- Zaten bir Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun.](https://azure.microsoft.com/free/)
- [Sanal bir makinede](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulamada](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)sistem atanan kimliği etkinleştirin.
- [Azure PowerShell'in](/powershell/azure/install-az-ps) en son sürümünü yükleme

## <a name="view-the-service-principal"></a>Hizmet ilkesini görüntüleme

Bu aşağıdaki komut, sistem atanmış kimlik etkin bir VM veya uygulamanın hizmet ilkesini nasıl görüntülenebildiğini gösterir. Kendi `<VM or application name>` değerlerinizle değiştirin.

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell'i kullanarak Azure REKLAM hizmet ilkelerini görüntüleme hakkında daha fazla bilgi için [Get-AzADServicePrincipal'a](/powershell/module/az.resources/get-azadserviceprincipal)bakın.


