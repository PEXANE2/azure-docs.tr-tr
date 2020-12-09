---
title: PowerShell örneği-bir uygulama proxy 'Si uygulamasına kullanıcı atama
description: Bir Azure Active Directory (Azure AD) uygulama proxy 'Si uygulamasına Kullanıcı atayan PowerShell örneği.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 020909464ca7c271c260dc9662c9e4a18d32f86f
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96859411"
---
# <a name="assign-a-user-to-a-specific-azure-ad-application-proxy-application"></a>Belirli bir Azure AD Uygulama Ara Sunucusu uygulamasına kullanıcı atama

Bu PowerShell betiği örneği, bir kullanıcıyı belirli bir Azure AD Uygulama Ara Sunucusu uygulamasına atamanıza olanak tanır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/assign-user-to-app.ps1 "Assign a user to an application")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
| [New-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/new-azureaduserapproleassignment) | Bir uygulama rolüne kullanıcı atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).
