---
title: PowerShell kullanarak yönetilen kimliğin hizmet sorumlusunu görüntüleme-Azure AD
description: PowerShell kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemek için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2f9ac119a3365eff39fe1a65ff8b553d3900b117
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90969327"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>PowerShell kullanarak yönetilen kimliğin hizmet sorumlusunu görüntüleme

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemeyi öğreneceksiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın.
- Henüz bir Azure hesabınız yoksa [ücretsiz hesap için kaydolun](https://azure.microsoft.com/free/).
- [Bir sanal makine veya uygulama üzerinde sistem tarafından atanan kimliği](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) etkinleştirin. [application](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity)
- Örnek betikleri çalıştırmak için iki seçeneğiniz vardır:
    - Kod bloklarının sağ üst köşesindeki **It TRY** düğmesini kullanarak açabileceğiniz [Azure Cloud Shell](../../cloud-shell/overview.md)kullanın.
    - [Azure PowerShell](/powershell/azure/install-az-ps)en son sürümünü yükleyerek betikleri yerel olarak çalıştırın ve ardından kullanarak Azure 'da oturum açın `Connect-AzAccount` .

## <a name="view-the-service-principal"></a>Hizmet sorumlusunu görüntüleme

Aşağıdaki komut, sistem tarafından atanan kimlik etkin bir VM veya uygulamanın hizmet sorumlusunu görüntülemeyi gösterir. `<VM or application name>`Kendi değerlerinizle değiştirin.

```azurepowershell-interactive
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak Azure AD hizmet sorumlularını görüntüleme hakkında daha fazla bilgi için bkz. [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).
