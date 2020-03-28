---
title: Windows Sanal Masaüstü için uygulama gruplarını yönetme - Azure
description: Azure Active Directory'de Windows Sanal Masaüstü kiracılarının nasıl ayarlanır olduğunu açıklar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9a9d92ea525c6b5a64fdf7cc74babdce6a97f923
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79127819"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop"></a>Öğretici: Windows Sanal Masaüstü için uygulama gruplarını yönetme

Yeni bir Windows Sanal Masaüstü ana bilgisayar havuzu için oluşturulan varsayılan uygulama grubu da tam masaüstü yayımlar. Ayrıca, ana bilgisayar havuzu için bir veya daha fazla RemoteApp uygulama grubu oluşturabilirsiniz. Bir RemoteApp uygulama grubu oluşturmak ve tek tek **Başlat** menüsü uygulamalarını yayınlamak için bu öğreticiyi izleyin.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir RemoteApp grubu oluşturun.
> * RemoteApp programlarına erişim izni tanıyın.

Başlamadan önce, powershell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="create-a-remoteapp-group"></a>RemoteApp grubu oluşturma

1. Yeni bir boş RemoteApp uygulama grubu oluşturmak için aşağıdaki PowerShell cmdlet'i çalıştırın.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (İsteğe bağlı) Uygulama grubunun oluşturulduğunu doğrulamak için, ana bilgisayar havuzu için tüm uygulama gruplarının listesini görmek için aşağıdaki cmdlet'i çalıştırabilirsiniz.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Ana bilgisayar havuzunun sanal makine görüntüsünde **Başlat** menüsü uygulamalarının listesini almak için aşağıdaki cmdlet'i çalıştırın. **FilePath, IconPath,** **IconPath** **IconIndex**ve yayımlamak istediğiniz uygulama için diğer önemli bilgileri için değerleri yazın.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Uygulamayı temel alan yüklemek için aşağıdaki `AppAlias`cmdlet'i çalıştırın. `AppAlias`adım 3 çıktı çalıştırdığınızda görünür hale gelir.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. (İsteğe bağlı) Adım 1'de oluşturulan uygulama grubuna yeni bir RemoteApp programı yayınlamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Uygulamanın yayınlandığını doğrulamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Bu uygulama grubu için yayınlamak istediğiniz her uygulama için 1-5 adımlarını yineleyin.
8. Kullanıcılara uygulama grubundaki RemoteApp programlarına erişim izni vermek için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir uygulama grubu oluşturmayı, RemoteApp programlarıyla doldurmayı ve kullanıcıları uygulama grubuna atamayı öğrendiniz. Doğrulama ana bilgisayar havuzunu nasıl oluşturacağımıöğrenmek için aşağıdaki öğreticiye bakın. Üretim ortamınıza dağıtmadan önce hizmet güncelleştirmelerini izlemek için bir doğrulama ana bilgisayar havuzu kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Hizmet güncelleştirmelerini doğrulamak için konak havuzu oluşturma](./create-validation-host-pool.md)
