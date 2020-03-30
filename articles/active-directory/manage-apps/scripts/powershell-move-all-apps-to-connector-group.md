---
title: PowerShell örneği - Uygulama Proxy uygulamalarını başka bir gruba taşıyın
description: Azure Active Directory (Azure AD) Application Proxy PowerShell örneği, şu anda bir bağlayıcı grubuna atanan tüm uygulamaları farklı bir bağlayıcı grubuna taşımak için kullanılır.
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
ms.openlocfilehash: f6dccdaa96dadb061b168bbdf6148ed4d759776c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75481990"
---
# <a name="move-all-apps-assigned-to-a-connector-group-to-another-connector-group"></a>Bağlayıcı grubuna atanan tüm uygulamaları başka bir bağlayıcı grubuna taşıma

Bu PowerShell komut dosyası örneği, şu anda bir bağlayıcı grubuna atanan tüm Azure Active Directory (Azure AD) Uygulama Proxy uygulamalarını farklı bir bağlayıcı grubuna taşır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek [için AzureAD V2 PowerShell grafik modülü modülü](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) veya [Graph modülü önizleme sürümü için AzureAD V2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/move-all-apps-to-a-connector-group.ps1 "Move all apps assigned to a connector group to another connector group")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Bir servis müdürü alır. |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Azure AD uygulaması alır. |
| [AzureADApplicationProxyConnectorGroup](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | Tüm bağlayıcı gruplarının veya belirtilen bağlayıcı grubunun ayrıntılarının listesini alır. |
| [Ayar-AzureADApplicationProxyConnectorGroup](https://docs.microsoft.com/powershell/module/azuread/set-azureadapplicationproxyapplicationconnectorgroup?view=azureadps-2.0) | Verilen bağlayıcı grubunu belirtilen bir uygulamaya atar.|

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için [Azure AD PowerShell modülüne genel bakış](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)bilgisine bakın.

Uygulama Proxy'si için diğer PowerShell örnekleri için Azure [AD Application Proxy için Azure AD PowerShell örneklerine](../application-proxy-powershell-samples.md)bakın.