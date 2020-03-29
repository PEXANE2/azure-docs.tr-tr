---
title: Azure Medya Hizmetleri API'sine erişmek için bir Azure AD uygulaması oluşturmak için PowerShell'i kullanın | Microsoft Dokümanlar
description: Bir Azure Etkin Dizin (Azure AD) uygulaması oluşturmak ve Azure Medya Hizmetleri API'sine erişmek için kurmak için PowerShell'i nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: ff7f8bc27d358c667b10c0bd3383e78b20494303
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64680126"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Azure Medya Hizmetleri API'sini kullanmak üzere bir Azure AD uygulaması oluşturmak için PowerShell'i kullanın

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Media Hizmetleri kaynaklarına erişmek için bir Azure Active Directory (Azure AD) uygulaması ve hizmet ilkesi oluşturmak için PowerShell komut dosyasını nasıl kullanacağınızı öğrenin.  

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Hesabınız yoksa, [Azure ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/)başlayın. 
- Bir Media Services hesabı. Daha fazla bilgi için [bkz.](media-services-portal-create-account.md)

- Azure PowerShell. Daha fazla bilgi için [Azure PowerShell'i nasıl kullanacağınız bilgisine](https://docs.microsoft.com/powershell/azure/overview)bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="create-an-azure-ad-app-by-using-powershell"></a>PowerShell'i kullanarak bir Azure AD uygulaması oluşturun  

```powershell
Connect-AzAccount
Import-Module Az.Resources
Set-AzContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Azure PowerShell'i kullanarak Rol Tabanlı Erişim Denetimini Yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Sertifikaları kullanarak daemon uygulamalarını el ile yapılandırma](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Sonraki adımlar

[Hesabınıza dosya yüklemeye](media-services-portal-upload-files.md)başlayın.
