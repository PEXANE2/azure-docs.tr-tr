---
title: PowerShell örneği - Tüm Uygulama Proxy uygulamalarını bir ilkeyle listele
description: Dizininizdeki ömür boyu belirteç ilkesiolan tüm Azure Active Directory (Azure AD) Uygulama Proxy uygulamalarını listeleyen PowerShell örneği.
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
ms.openlocfilehash: c2ad7b769dc890917e5364fe57582acdfe16acac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482133"
---
# <a name="get-all-application-proxy-apps-with-a-token-lifetime-policy"></a>Bir belirteç ömür boyu ilkesi ile tüm Uygulama Proxy uygulamaları alın

Bu PowerShell komut dosyası örneği, dizininizdeki ömür boyu ilke olan tüm Azure Active Directory (Azure AD) Uygulama Proxy uygulamalarını listeler ve ilke yle ilgili ayrıntıları listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu [örnek, Grafik modülü önizleme sürümü için AzureAD V2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-with-policy.ps1 "Get all Application Proxy apps with a token lifetime policy")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Bir servis müdürü alır. |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Azure AD uygulaması alır. |
|[AzureAD İlkesi Al](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) | Azure AD'de bir ilke alır. |
|[AzureADServicePrincipalPolicy Al](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview) | Azure AD'de bir hizmet sorumlusunun ilkesini alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için [Azure AD PowerShell modülüne genel bakış](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)bilgisine bakın.

Uygulama Proxy'si için diğer PowerShell örnekleri için Azure [AD Application Proxy için Azure AD PowerShell örneklerine](../application-proxy-powershell-samples.md)bakın.