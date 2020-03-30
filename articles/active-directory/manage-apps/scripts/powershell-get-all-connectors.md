---
title: PowerShell örneği - Tüm Uygulama Proxy bağlayıcı gruplarını listele
description: Dizininizdeki tüm Azure Active Directory (Azure AD) Application Proxy bağlayıcı gruplarını ve bağlayıcılarını listeleyen PowerShell örneği.
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
ms.openlocfilehash: 53af7f3f9049edc23ed4226f43e7fe67398c29f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482120"
---
# <a name="get-all-connector-groups-and-connectors-in-the-directory"></a>Tüm bağlayıcı gruplarını ve bağlayıcıları dizine alma

Bu PowerShell komut dosyası örneği, dizininizdeki tüm Azure Active Directory (Azure AD) Application Proxy bağlayıcı gruplarını ve bağlayıcılarını listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek [için AzureAD V2 PowerShell grafik modülü modülü](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) veya [Graph modülü önizleme sürümü için AzureAD V2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-connectors.ps1 "Get all connector groups and connectors in the directory")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
| [AzureADApplicationProxyConnectorGroup](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | Tüm bağlayıcı gruplarının veya belirtilen bağlayıcı grubunun ayrıntılarının listesini alır. |
| [AzureADApplicationProxyConnectorGroupMembers](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroupmembers?view=azureadps-2.0) | Her bağlayıcı grubuyla ilişkili tüm Uygulama Proxy bağlayıcılarını alır.|

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için [Azure AD PowerShell modülüne genel bakış](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)bilgisine bakın.

Uygulama Proxy'si için diğer PowerShell örnekleri için Azure [AD Application Proxy için Azure AD PowerShell örneklerine](../application-proxy-powershell-samples.md)bakın.