---
title: Windows Sanal Masaüstü hizmeti temel rol atama - Azure
description: Windows Sanal Masaüstü'nde PowerShell'i kullanarak hizmet ilkeleri oluşturma ve roller atama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 61b5017609d99f2f0074c67d3838cf351ea38bea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79365432"
---
# <a name="tutorial-create-service-principals-and-role-assignments-by-using-powershell"></a>Öğretici: PowerShell'i kullanarak hizmet ilkeleri ve rol atamaları oluşturun

Hizmet ilkeleri, belirli bir amaç için roller ve izinler atamak için Azure Etkin Dizini'nde oluşturabileceğiniz kimliklerdir. Windows Sanal Masaüstü'nde, şu lar için bir hizmet sorumlusu oluşturabilirsiniz:

- Belirli Windows Sanal Masaüstü yönetim görevlerini otomatikleştirin.
- Windows Sanal Masaüstü için herhangi bir Azure Kaynak Yöneticisi şablonu çalıştırırken MFA'dan gerekli kullanıcıların yerine kimlik bilgileri olarak kullanın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Etkin Dizini'nde bir hizmet ilkesi oluşturun.
> * Windows Sanal Masaüstü'nde bir rol ataması oluşturun.
> * Hizmet ilkesini kullanarak Windows Sanal Masaüstü'nde oturum açın.

## <a name="prerequisites"></a>Ön koşullar

Hizmet ilkeleri ve rol atamaları oluşturmadan önce üç şey yapmanız gerekir:

1. AzureAD modüllerini yükleyin. Modülü yüklemek için PowerShell'i yönetici olarak çalıştırın ve aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Install-Module AzureAD
    ```

2. [Windows Virtual Desktop PowerShell modülünü indirin ve aktarın.](/powershell/windows-virtual-desktop/overview/)

3. Aynı PowerShell oturumunda bu makaledeki tüm talimatları izleyin. PowerShell oturumunuzu pencereyi kapatıp daha sonra yeniden açarak keserseniz işlem çalışmayabilir.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Azure Active Directory'de hizmet sorumlusu oluşturma

PowerShell oturumunuzdaki ön koşulları yerine getirdikten sonra, Azure'da çok kiracılı bir hizmet ilkesi oluşturmak için aşağıdaki PowerShell cmdlets'i çalıştırın.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```
## <a name="view-your-credentials-in-powershell"></a>PowerShell'de kimlik bilgilerinizi görüntüleyin

Hizmet yöneticiniz için rol atamasını oluşturmadan önce kimlik bilgilerinizi görüntüleyin ve ileride başvurmak üzere yazın. Bu PowerShell oturumunu kapattıktan sonra geri alamayacağınız için parola özellikle önemlidir.

Burada yazmanız gereken üç kimlik bilgileri ve bunları almak için çalıştırmak için gereken cmdlets şunlardır:

- Parola:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Kiracı Kimliği:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Uygulama Kimliği:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Windows Sanal Masaüstü Önizleme'de rol ataması oluşturma

Ardından, hizmet sorumlusunun Windows Sanal Masaüstü'nde oturum açabilmesi için bir rol ataması oluşturmanız gerekir. Rol atamaları oluşturma izinleri olan bir hesapla oturum açtırdığından emin olun.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/)

Windows Sanal Masaüstü'ne bağlanmak ve kiracılarınızı görüntülemek için aşağıdaki PowerShell cmdletlerini çalıştırın.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Get-RdsTenant
```

Rol ataması oluşturmak istediğiniz kiracının kiracı adını bulduğunuzda, aşağıdaki cmdlet'te bu adı kullanın:

```powershell
$myTenantName = "<Windows Virtual Desktop Tenant Name>"
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Servis müdürüyle oturum açın

Hizmet sorumlusu için bir rol ataması oluşturduktan sonra, servis müdürünün aşağıdaki cmdlet'i çalıştırarak Windows Sanal Masaüstü'nde oturum açabilmesini unutmayın:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Oturum açtıktan sonra, birkaç Windows Sanal Masaüstü PowerShell cmdlet'ini servis ilkesiyle test ederek her şeyin çalıştığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Hizmet ilkesini oluşturduktan ve Windows Sanal Masaüstü kiracınızda bir rol atadıktan sonra, bu hizmeti ana bilgisayar havuzu oluşturmak için kullanabilirsiniz. Ana bilgisayar havuzları hakkında daha fazla bilgi edinmek için, Windows Sanal Masaüstü'nde ana bilgisayar havuzu oluşturma eğitimine devam edin.

 > [!div class="nextstepaction"]
 > [Azure Market ile ana bilgisayar havuzu oluşturma](./create-host-pools-azure-marketplace.md)
