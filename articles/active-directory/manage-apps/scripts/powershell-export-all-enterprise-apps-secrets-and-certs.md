---
title: PowerShell örneği-Azure Active Directory kiracısındaki kurumsal uygulamalar için gizli dizileri ve sertifikaları dışarı aktarın.
description: Azure Active Directory kiracınızda belirtilen kurumsal uygulamalar için tüm gizli dizileri ve sertifikaları dışarı aktaran PowerShell örneği.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: kenwith
ms.reviewer: mifarca
ms.openlocfilehash: 20caefe74a7c047fb8690bb1d9e6f4eb9da7e9b7
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102635204"
---
# <a name="export-secrets-and-certificates-for-enterprise-apps"></a>Kurumsal uygulamalar için gizli dizileri ve sertifikaları dışa aktarma
Bu PowerShell betiği örneği, belirtilen kurumsal uygulamalara ait tüm gizli dizileri, sertifikaları ve sahipleri dizininizden bir CSV dosyasına dışarı aktarır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-all-enterprise-apps-secrets-and-certs.ps1 "Exports all secrets and certificates for the specified enterprise apps in your directory.")]

## <a name="script-explanation"></a>Betik açıklaması

"Add-Member" komutu CSV dosyasındaki sütunları oluşturmaktan sorumludur.
"$Path" değişkenini doğrudan PowerShell 'de bir CSV dosyası yoluyla değiştirebilirsiniz, böylece dışa aktarma işlemini etkileşimli olmayan şekilde tercih edebilirsiniz.

| Komut | Notlar |
|---|---|
| [Get-AzureADServicePrincipal](/powershell/module/azuread/Get-azureADServicePrincipal?view=azureadps-2.0&preserve-view=true) | Dizininizden bir kurumsal uygulama alır. |
| [Get-AzureADServicePrincipalOwner](/powershell/module/azuread/Get-AzureADServicePrincipalOwner?view=azureadps-2.0&preserve-view=true) | Bir kuruluş uygulamasının sahiplerini dizininizden alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama yönetimi için diğer PowerShell örnekleri için bkz. [uygulama yönetimi Için Azure AD PowerShell örnekleri](../app-management-powershell-samples.md).
