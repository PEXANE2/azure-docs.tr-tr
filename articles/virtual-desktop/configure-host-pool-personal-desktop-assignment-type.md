---
title: Windows Sanal Masaüstü kişisel masaüstü atama türü - Azure
description: Windows Virtual Desktop kişisel masaüstü ana bilgisayar havuzu için atama türünü yapılandırma.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 41b24a94d36b21fe5d5f539e056abb535bda433a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128295"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Kişisel masaüstü ana bilgisayar havuzu atama türünü yapılandırma

Kişisel masaüstü ana bilgisayar havuzunuzun atama türünü, Windows Sanal Masaüstü ortamınızı gereksinimlerinize daha iyi uyacak şekilde ayarlayabilirsiniz. Bu konuda, kullanıcılarınız için otomatik veya doğrudan atamayı nasıl yapılandıracağınız göstereceğiz.

>[!NOTE]
> Bu makaledeki yönergeler, havuzlu ana bilgisayar havuzlarındaki kullanıcılar belirli oturum ana bilgisayarlarına atanmdığından, yalnızca havuzlu ana bilgisayar havuzları için geçerlidir.

## <a name="configure-automatic-assignment"></a>Otomatik atamayı yapılandırma

Otomatik atama, Windows Sanal Masaüstü ortamınızda oluşturulan yeni kişisel masaüstü ana bilgisayar havuzları için varsayılan atama türüdür. Kullanıcıları otomatik olarak atamak için belirli bir oturum ana bilgisayarı gerekmez.

Kullanıcıları otomatik olarak atamak için, önce onları kişisel masaüstü ana bilgisayar havuzuna atayın, böylece akışlarında masaüstünü görebilsinler. Atanan bir kullanıcı masaüstünü akışlarında başlattığında, atama işlemini tamamlayan ana bilgisayar havuzuna zaten bağlı değillerse kullanılabilir bir oturum ana bilgisayarını talep eder.

Başlamadan önce, windows [sanal masaüstü powershell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) 

> [!NOTE]
> Bu yönergeleri takip etmeden önce Windows Virtual Desktop PowerShell modül sürümünü 1.0.1534.2001 veya daha sonra yüklediğinizden emin olun.

Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Kullanıcıları VM'lere otomatik olarak atayabilmek için ana bilgisayar havuzunu yapılandırmak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Bir kullanıcıyı kişisel masaüstü ana bilgisayar havuzuna atamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Doğrudan atamayı yapılandırma

Otomatik atamanın aksine, doğrudan atama kullandığınızda, kullanıcıyı kişisel masaüstü netosuna bağlanabilmeleri için hem kişisel masaüstü ana bilgisayar havuzuna hem de belirli bir oturum ana bilgisayarına atamanız gerekir. Kullanıcı yalnızca oturum ana bilgisayar ataması olmayan bir ana bilgisayar havuzuna atanmışsa, kaynaklara erişemez.

Bir ana bilgisayar havuzunu, kullanıcıların oturum ana bilgisayarlarına doğrudan atanmasını gerektirecek şekilde yapılandırmak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Bir kullanıcıyı kişisel masaüstü ana bilgisayar havuzuna atamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Bir kullanıcıyı belirli bir oturum ana bilgisayara atamak için aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>Sonraki adımlar

Artık kişisel masaüstü atama türünü yapılandırdığınıza göre, kullanıcı oturumunun bir parçası olarak sınamak için Windows Sanal Masaüstü istemcisinde oturum açabilirsiniz. Bu sonraki iki Nasıl Yapılacaklar, seçtiğiniz istemciyi kullanarak bir oturuma nasıl bağlanacağınızı size söyleyecektir:

- [Windows Masaüstü istemcisine bağlanma](connect-windows-7-and-10.md)
- [Web istemcisiyle bağlanma](connect-web.md)
