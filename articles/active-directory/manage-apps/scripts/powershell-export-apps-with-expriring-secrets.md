---
title: PowerShell örneği-Azure Active Directory kiracısındaki süresi dolan gizli dizileri ve sertifikaları dışarı aktarın.
description: Azure Active Directory kiracınızda belirtilen uygulamalar için süresi dolan gizli dizileri ve sertifikaları olan tüm uygulamaları dışa aktaran PowerShell örneği.
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
ms.openlocfilehash: c8a37b08db5cdb35bc9328cbc29caa04a5cdf412
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103010846"
---
# <a name="export-apps-with-expiring-secrets-and-certificates"></a>Süresi dolan gizlilikler ve sertifikalarla uygulamaları dışa aktarma

Bu PowerShell betiği örneği, bir CSV dosyasındaki dizininizden belirtilen uygulamalar için süresi dolan gizli dizileri, sertifikaları ve onların sahiplerini içeren tüm uygulama kayıtlarını dışa aktarır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Bu örnek, Graf modülü (azuread) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2) 'ı veya Graf modülü önizleme sürümü (azureadpreview) [Için Azuread v2 PowerShell](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) 'i gerektirir.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-expiring-secrets.ps1 "Exports all apps with expiring secrets and certificates for the specified apps in your directory.")]

## <a name="script-explanation"></a>Betik açıklaması

"Add-Member" komutu CSV dosyasındaki sütunları oluşturmaktan sorumludur.
"New-Object" komutu CSV dosyası dışarı aktarma içindeki sütunlarda kullanılacak bir nesne oluşturur.
"$Path" değişkenini doğrudan PowerShell 'de bir CSV dosyası yoluyla değiştirebilirsiniz, böylece dışa aktarma işlemini etkileşimli olmayan şekilde tercih edebilirsiniz.

| Komut | Notlar |
|---|---|
| [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0&preserve-view=true) | Dizininizden bir uygulama alır. |
| [Get-AzureADApplicationOwner](/powershell/module/azuread/Get-AzureADApplicationOwner?view=azureadps-2.0&preserve-view=true) | Bir uygulamanın sahiplerini dizininizden alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama yönetimi için diğer PowerShell örnekleri için bkz. [uygulama yönetimi Için Azure AD PowerShell örnekleri](../app-management-powershell-samples.md).
