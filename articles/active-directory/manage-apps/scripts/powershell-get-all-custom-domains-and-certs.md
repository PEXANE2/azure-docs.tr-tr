---
title: PowerShell örneği-özel etki alanları kullanan uygulama proxy uygulamaları
description: Özel etki alanları ve sertifika bilgilerini kullanan tüm Azure Active Directory (Azure AD) uygulama proxy uygulamalarını listeleyen PowerShell örneği.
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
ms.openlocfilehash: 9722178011c3c0411f2e0e888dd9ab5962b933b9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482055"
---
# <a name="get-all-application-proxy-apps-using-custom-domains-and-certificate-information"></a>Özel etki alanları ve sertifika bilgilerini kullanarak tüm uygulama proxy uygulamalarını alın

Bu PowerShell betiği örneği, özel etki alanları kullanan ve özel etki alanlarıyla ilişkili sertifika bilgilerini listeleyen tüm Azure Active Directory (Azure AD) uygulama proxy uygulamalarını listeler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-custom-domains-and-certs.ps1 "Get all Application Proxy apps using custom domains and certificate information")]

## <a name="script-explanation"></a>Betik açıklaması

| Komut | Notlar |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Hizmet sorumlusu alır. |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Bir Azure AD uygulaması alır. |
|[Get-Azureadapplicationproxun](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Azure AD 'de uygulama proxy 'Si için yapılandırılmış bir uygulamayı alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

Uygulama proxy 'Si için diğer PowerShell örnekleri için bkz. Azure [ad uygulama ara sunucusu Için Azure AD PowerShell örnekleri](../application-proxy-powershell-samples.md).