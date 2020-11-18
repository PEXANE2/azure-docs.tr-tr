---
title: PowerShell örneği-uygulama proxy 'Si uygulamaları için genişletilmiş bilgileri listeleyin
description: Tüm Azure Active Directory (Azure AD) uygulama proxy uygulamalarını, uygulama KIMLIĞI (AppID), adı (DisplayName), dış URL (ExternalUrl), iç URL (InternalUrl) ve kimlik doğrulama türü (ExternalAuthenticationType) ile birlikte listeleyen PowerShell örneği.
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
ms.openlocfilehash: cd1120874ca9ee0d3ef65417d6a200ffd0a40fdb
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659123"
---
# <a name="get-all-application-proxy-apps-and-list-extended-information"></a>Tüm uygulama proxy 'Si uygulamalarını al ve genişletilmiş bilgileri Listele

Bu PowerShell betiği örneği, uygulama KIMLIĞI (AppID), adı (DisplayName), dış URL (ExternalUrl), iç URL (InternalUrl) ve kimlik doğrulama türü (ExternalAuthenticationType) dahil olmak üzere tüm Azure Active Directory (Azure AD) uygulama proxy uygulamaları hakkındaki bilgileri listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-extended.ps1 "Get all Application Proxy apps")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Hizmet sorumlusu alır. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Bir Azure AD uygulaması alır. |
|[Get-Azureadapplicationproxun](/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Azure AD 'de uygulama proxy 'Si için yapılandırılmış bir uygulamayı alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview?view=azureadps-2.0).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).