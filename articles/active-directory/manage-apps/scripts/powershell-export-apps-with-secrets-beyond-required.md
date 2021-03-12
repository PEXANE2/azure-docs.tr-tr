---
title: PowerShell örneği-Azure Active Directory kiracısındaki gerekli tarihin ötesinde gizli dizileri ve sertifikaları dışarı aktarma.
description: Azure Active Directory kiracınızdaki belirtilen uygulamalar için gerekli tarihin ötesinde gizli dizi ve sertifikalara sahip tüm uygulamaları dışa aktaran PowerShell örneği.
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
ms.openlocfilehash: 9c0e5508830343561833785fbce31f547a8a7428
ms.sourcegitcommit: 6776f0a27e2000fb1acb34a8dddc67af01ac14ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103149690"
---
# <a name="export-apps-with-secrets-and-certificates-expiring-beyond-the-required-date"></a>Uygulamaları gizli ve sertifikalara göre, gerekli tarihin ötesinde süresi dolacak şekilde dışa aktarma

Bu PowerShell betiği örneği, bir CSV dosyasındaki dizininizden etkileşimli olmayan bir şekilde, belirtilen uygulamalar için gerekli bir sürenin ötesine geçen tüm uygulama kaydı gizli dizilerini ve sertifikalarını dışa aktarır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-secrets-beyond-required.ps1 "Exports all apps with secrets and certificates expiring beyond the required date for the specified apps in your directory.")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik etkileşimli olmayan şekilde çalışıyor. Bunu kullanan yöneticinin, kendi uygulama KIMLIĞI, uygulama gizli anahtarı, kiracı adı, uygulama kimlik bilgilerinin süre sonu ve CSV 'nin aktarılacağı yol ile "DEĞIŞTIRILECEK #PARAMETERS" bölümündeki değerleri değiştirmesi gerekir.
Bu betik, "RefreshToken" işlevi, yönetici tarafından değiştirilen parametrelerin değerlerine göre erişim belirtecini oluşturacak [Client_Credential OAuth akışını](../../develop/v2-oauth2-client-creds-grant-flow.md) kullanır.

"Add-Member" komutu CSV dosyasındaki sütunları oluşturmaktan sorumludur.

| Komut | Notlar |
|---|---|
| [Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1) | Bir Web sayfasına veya Web hizmetine HTTP ve HTTPS istekleri gönderir. Yanıtı ayrıştırır ve bağlantılar, görüntüler ve diğer önemli HTML öğeleri için koleksiyonları döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure AD PowerShell modülüne genel bakış](/powershell/azure/active-directory/overview).

Uygulama yönetimi için diğer PowerShell örnekleri için bkz. [uygulama yönetimi Için Azure AD PowerShell örnekleri](../app-management-powershell-samples.md).
