---
title: PowerShell örneği-uygulama proxy 'Si uygulaması için kullanıcıları & grupları listeleme
description: Belirli bir Azure Active Directory (Azure AD) uygulama proxy 'Si uygulamasına atanan tüm kullanıcıları ve grupları listeleyen PowerShell örneği.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: aab5ac5d4712dd9e72fc0b9665d740859b5a6775
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75483394"
---
# <a name="display-users-and-groups-assigned-to-an-application-proxy-application"></a>Uygulama proxy 'Si uygulamasına atanan kullanıcıları ve grupları görüntüleme

Bu PowerShell betiği örneği, belirli bir Azure Active Directory (Azure AD) uygulama proxy 'Si uygulamasına atanan kullanıcıları ve grupları listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/display-users-group-of-an-app.ps1 "Display users and groups assigned to an Application Proxy application")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
| [Get-AzureADUser](https://docs.microsoft.com/powershell/module/AzureAD/get-azureaduser?view=azureadps-2.0)| Bir kullanıcıyı alır. |
| [Get-AzureADGroup](https://docs.microsoft.com/powershell/module/AzureAD/get-azureadgroup?view=azureadps-2.0)| Bir grup alır. |
| [Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Hizmet sorumlusu alır. |
| [Get-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/AzureAD/get-azureaduserapproleassignment?view=azureadps-2.0) | Kullanıcı uygulama rolü ataması alın. |
| [Get-AzureADGroupAppRoleAssignment](https://docs.microsoft.com/powershell/module/AzureAD/get-azureadgroupapproleassignment?view=azureadps-2.0) | Grup uygulama rolü ataması alın. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).