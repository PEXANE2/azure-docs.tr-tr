---
title: PowerShell-Azure kullanarak Windows sanal masaüstü önizleme hizmet sorumluları ve rol atamaları oluşturma
description: Windows sanal masaüstü önizlemesinde PowerShell kullanarak hizmet sorumluları oluşturma ve roller atama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: 3e9ee3f5dd04ef838f78b9731885b7ea48e6c99d
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70811330"
---
# <a name="tutorial-create-service-principals-and-role-assignments-by-using-powershell"></a>Öğretici: PowerShell kullanarak hizmet sorumluları ve rol atamaları oluşturma

Hizmet sorumluları, belirli bir amaçla roller ve izinler atamak için Azure Active Directory oluşturabileceğiniz kimliklerdir. Windows sanal masaüstü önizlemesinde, bir hizmet sorumlusu oluşturabilirsiniz:

- Belirli Windows sanal masaüstü yönetim görevlerini otomatikleştirin.
- Windows sanal masaüstü için herhangi bir Azure Resource Manager şablonu çalıştırırken, MFA 'ya gerekli kullanıcıların yerine kimlik bilgilerini kullanın.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Azure Active Directory bir hizmet sorumlusu oluşturun.
> * Windows sanal masaüstü 'nde bir rol ataması oluşturun.
> * Hizmet sorumlusunu kullanarak Windows sanal masaüstü 'nde oturum açın.

## <a name="prerequisites"></a>Önkoşullar

Hizmet sorumluları ve rol atamaları oluşturabilmeniz için üç şey yapmanız gerekir:

1. AzureAD modülünü yükler. Modülünü yüklemek için PowerShell 'i yönetici olarak çalıştırın ve aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    Install-Module AzureAD
    ```

2. [Windows sanal masaüstü PowerShell modülünü indirme ve içeri aktarma](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview)

3. Bu makaledeki tüm yönergeleri aynı PowerShell oturumunda izleyin. Pencereyi kapatıp daha sonra geri dönerseniz, bu çalışmayabilir.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Azure Active Directory'de hizmet sorumlusu oluşturma

PowerShell oturumunuzda önkoşulları karşıladıktan sonra, Azure 'da çok kiracılı hizmet sorumlusu oluşturmak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="view-your-credentials-in-powershell"></a>PowerShell 'de kimlik bilgilerinizi görüntüleme

PowerShell oturumunuzu sonlandırmadan önce, kimlik bilgilerinizi görüntüleyin ve daha sonra başvurmak üzere bunları aşağı yazın. Bu PowerShell oturumunu kapattıktan sonra bu parola özellikle önemlidir.

Aşağıda, yazmanız gereken üç kimlik bilgileri ve bunları almak için çalıştırmanız gereken cmdlet 'ler verilmiştir:

- Parolayı

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Kiracı KIMLIĞI:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Uygulama KIMLIĞI:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Windows sanal masaüstü önizlemesinde rol ataması oluşturma

Daha sonra, hizmet sorumlusu için Windows sanal masaüstünde bir RDS rol ataması oluşturacaksınız ve bu, hizmet sorumlusunun Windows sanal masaüstünde oturum açmasını sağlar. RDS rol atamaları oluşturmak için izinlere sahip bir hesap kullandığınızdan emin olun.

Windows sanal masaüstüne bağlanmak ve RDS Kiracılarınızı göstermek için aşağıdaki PowerShell cmdlet 'lerini çalıştırın.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Get-RdsTenant | FL
```

Doğru kiracı için TenantName ' i kullanın ve belirtilen Kiracıdaki hizmet sorumlusu için bir rol ataması oluşturmak üzere aşağıdaki PowerShell cmdlet 'lerini çalıştırın.

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantName "<my-rds-tenantname>"
```

## <a name="sign-in-with-the-service-principal"></a>Hizmet sorumlusu ile oturum açma

Hizmet sorumlusu için bir rol ataması oluşturduktan sonra, hizmet sorumlusunun aşağıdaki cmdlet 'i çalıştırarak Windows sanal masaüstü 'nde oturum açmasını sağlayın:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Oturum açtıktan sonra, hizmet sorumlusu ile birkaç Windows sanal masaüstü PowerShell cmdlet 'ini test ederek her şeyin çalıştığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Hizmet sorumlusunu oluşturup Windows sanal masaüstü kiracınızda bir rol atadıktan sonra, bir konak havuzu oluşturmak için bu hizmeti kullanabilirsiniz. Konak havuzları hakkında daha fazla bilgi edinmek için Windows sanal masaüstü 'nde bir konak havuzu oluşturma öğreticisine geçin.

 > [!div class="nextstepaction"]
 > [Windows sanal masaüstü ana bilgisayar havuzu öğreticisi](./create-host-pools-azure-marketplace.md)
