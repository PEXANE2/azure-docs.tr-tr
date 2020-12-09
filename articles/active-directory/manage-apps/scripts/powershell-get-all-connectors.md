---
title: PowerShell örneği-tüm uygulama proxy Bağlayıcısı gruplarını Listele
description: Dizininizdeki tüm Azure Active Directory (Azure AD) uygulama proxy Bağlayıcısı gruplarını ve bağlayıcılarını listeleyen PowerShell örneği.
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
ms.openlocfilehash: 544bdf1c4f9e000c79241418f5e6adefbc1b6121
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96858646"
---
# <a name="get-all-connector-groups-and-connectors-in-the-directory"></a>Dizindeki tüm bağlayıcı gruplarını ve bağlayıcıları al

Bu PowerShell betiği örneği, dizininizdeki tüm Azure Active Directory (Azure AD) uygulama proxy Bağlayıcısı gruplarını ve bağlayıcılarını listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-connectors.ps1 "Get all connector groups and connectors in the directory")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
| [Get-AzureADApplicationProxyConnectorGroup](/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup) | Tüm bağlayıcı gruplarının bir listesini veya belirtilmişse, belirtilen bağlayıcı grubunun ayrıntılarını alır. |
| [Get-AzureADApplicationProxyConnectorGroupMembers](/powershell/module/azuread/get-azureadapplicationproxyconnectorgroupmembers) | Her bağlayıcı grubuyla ilişkili tüm uygulama proxy bağlayıcılarını alır.|

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).
