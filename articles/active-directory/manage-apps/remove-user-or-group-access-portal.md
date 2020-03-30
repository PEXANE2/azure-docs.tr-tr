---
title: Azure AD'deki bir uygulamadan kullanıcı veya grup atamalarını kaldırma
description: Azure Active Directory'deki bir kurumsal uygulamadan bir kullanıcının veya grubun erişim ataması kaldırma
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: edf918b57212cf2adfbffb358a0257d9dbea85e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275872"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki bir kurumsal uygulamadan kullanıcı veya grup ataması kaldırma

Azure Etkin Dizini'ndeki (Azure AD) kurumsal uygulamalarınızdan birine atanmış erişimden bir kullanıcıyı veya grubu kaldırmak kolaydır. Kurumsal uygulamayı yönetmek için uygun izinlere ihtiyacınız vardır. Ve, dizin için küresel yönetici olmalıdır.

> [!NOTE]
> Microsoft Uygulamaları (Office 365 uygulamaları gibi) için, kullanıcıları kurumsal bir uygulamaya kaldırmak için PowerShell'i kullanın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Azure portalındaki bir kurumsal uygulamaya kullanıcı veya grup atamasını nasıl kaldırırım?

1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
1. **Tüm hizmetleri**seçin, metin kutusuna Azure **Etkin Dizini** girin ve sonra **Enter'u**seçin.
1. Azure **Etkin Dizini - *dizin adı* ** sayfasında (diğer bir deyişle, yöneticiliğiniz için Azure REKLAM sayfası), Kurumsal **uygulamaları**seçin.
1. Kurumsal **uygulamalarda - Tüm uygulamalar** sayfasında yönetebileceğiniz uygulamaların bir listesini görürsünüz. Bir uygulama seçin.
1. ***Appname*** genel bakış sayfasında (diğer bir deyişle, başlıkta seçilen uygulamanın adının yer alan sayfası), **Kullanıcı & Grupları'nı**seçin.
1. ***Eknamede*** **- Kullanıcı & Grup Atama** sayfasında, daha fazla kullanıcı veya gruptan birini seçin ve ardından **Kaldır** komutunu seçin. Kararınızı hemen onaylayın.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>PowerShell'i kullanarak bir kurumsal uygulamaya kullanıcı veya grup atamasını nasıl kaldırırım?

1. Yükseltilmiş bir Windows PowerShell komut istemini açın.

   > [!NOTE]
   > AzureAD modüllerini yüklemeniz gerekir (komutu `Install-Module -Name AzureAD`kullanın). Bir NuGet modülü veya yeni Azure Active Directory V2 PowerShell modülü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

1. Global `Connect-AzureAD` Admin kullanıcı hesabıyla çalıştırın ve oturum açın.
1. Bir kullanıcıyı ve rolü bir uygulamadan kaldırmak için aşağıdaki komut dosyasını kullanın:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Tüm gruplarımı görün](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Bir kurumsal uygulamaya kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
- [Kurumsal bir uygulama için kullanıcı oturum açma larını devre dışı](disable-user-sign-in-portal.md)
- [Kurumsal bir uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)
