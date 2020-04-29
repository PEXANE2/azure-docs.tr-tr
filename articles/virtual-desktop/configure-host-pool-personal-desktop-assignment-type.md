---
title: Windows sanal masaüstü kişisel masaüstü atama türü-Azure
description: Windows sanal masaüstü kişisel masaüstü konak havuzu için atama türünü yapılandırma.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 41b24a94d36b21fe5d5f539e056abb535bda433a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79128295"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Kişisel masaüstü konak havuzu atama türünü yapılandırma

Windows sanal masaüstü ortamınızı gereksinimlerinize daha uygun olacak şekilde ayarlamak için kişisel masaüstü ana bilgisayar havuzunuzun atama türünü yapılandırabilirsiniz. Bu konu başlığında, kullanıcılarınız için otomatik veya doğrudan atamayı nasıl yapılandıracağınızı göstereceğiz.

>[!NOTE]
> Havuza alınmış konak havuzlarındaki kullanıcılar belirli oturum konaklarına atanmadığından, bu makaledeki yönergeler yalnızca kişisel masaüstü ana bilgisayar havuzları için geçerlidir.

## <a name="configure-automatic-assignment"></a>Otomatik atamayı yapılandırma

Otomatik atama, Windows sanal masaüstü ortamınızda oluşturulan yeni kişisel masaüstü konak havuzları için varsayılan atama türüdür. Kullanıcıları otomatik olarak atamak, belirli bir oturum ana bilgisayarı gerektirmez.

Kullanıcıları otomatik olarak atamak için, önce bunları kendi akışlarından görebilmesi için kişisel masaüstü ana bilgisayar havuzuna atayın. Atanan bir Kullanıcı, akışını akışlarda başlattığında, bu kullanıcılar, atama işlemini tamamlayan, zaten konak havuzuna bağlanmadıysanız kullanılabilir bir oturum ana bilgisayarı talep eder.

Başlamadan önce, henüz yapmadıysanız [Windows sanal masaüstü PowerShell modülünü indirip içeri aktarın](/powershell/windows-virtual-desktop/overview/) . 

> [!NOTE]
> Bu yönergeleri izleyerek önce Windows sanal masaüstü PowerShell modülü sürüm 1.0.1534.2001 veya üstünü yüklediğinizden emin olun.

Bundan sonra hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bir konak havuzunu kullanıcıları sanal makinelere otomatik olarak atanacak şekilde yapılandırmak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Bir kullanıcıyı kişisel masaüstü konak havuzuna atamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Doğrudan atamayı yapılandırma

Otomatik atamanın aksine, doğrudan atama kullandığınızda, kullanıcıyı kişisel masaüstlerine bağlanabilmek için hem kişisel masaüstü ana bilgisayar havuzuna hem de belirli bir oturum konağına atamanız gerekir. Kullanıcı yalnızca bir oturum ana bilgisayar ataması olmadan bir konak havuzuna atanırsa, kaynaklara erişemez.

Bir konak havuzunu kullanıcıların oturum ana bilgisayarlarına doğrudan atanmasını gerektirecek şekilde yapılandırmak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Bir kullanıcıyı kişisel masaüstü konak havuzuna atamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Bir kullanıcıyı belirli bir oturum konağına atamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>Sonraki adımlar

Kişisel masaüstü atama türünü yapılandırdığınıza göre, bir Windows sanal masaüstü istemcisinde oturum açarak Kullanıcı oturumunun bir parçası olarak test edebilirsiniz. Bu sonraki iki nasıl-TOS, tercih ettiğiniz istemciyi kullanarak bir oturuma nasıl bağlanacağınızı bildirir:

- [Windows Masaüstü istemcisine bağlanma](connect-windows-7-and-10.md)
- [Web istemcisiyle bağlanma](connect-web.md)
