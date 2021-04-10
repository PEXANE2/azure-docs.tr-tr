---
title: PowerShell örneği-ilke ile tüm uygulama proxy uygulamalarını listeleme
description: Bir yaşam süresi belirteç ilkesine sahip dizininizdeki tüm Azure Active Directory (Azure AD) uygulama proxy uygulamalarını listeleyen PowerShell örneği.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: c2da1885d975ea6b4daf26714e430143dd89da42
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105709778"
---
# <a name="get-all-application-proxy-apps-with-a-token-lifetime-policy"></a>Belirteç ömür ilkesi ile tüm uygulama proxy uygulamalarını edinme

Bu PowerShell betiği örneği, dizininizde belirteç ömür ilkesi bulunan ve ilkeyle ilgili ayrıntıları listeleyen tüm Azure Active Directory (Azure AD) uygulama proxy uygulamalarını listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-with-policy.ps1 "Get all Application Proxy apps with a token lifetime policy")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Hizmet sorumlusu alır. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Bir Azure AD uygulaması alır. |
|[Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Azure AD 'de bir ilke alır. |
|[Get-Azureadservicesprincipalpolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Azure AD 'de bir hizmet sorumlusu ilkesini alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).
