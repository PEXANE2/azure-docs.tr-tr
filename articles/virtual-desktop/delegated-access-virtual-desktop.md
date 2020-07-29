---
title: Windows sanal masaüstü 'nde temsilci erişimi-Azure
description: Örnekler de dahil olmak üzere Windows sanal masaüstü dağıtımında yönetim özellikleri atama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2dc96f587a9e5db9d9810a4d1ab7d32c4ff49f7d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289870"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne temsilci erişimi

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md)bakın.

Windows sanal masaüstü, belirli bir kullanıcının bir rol atayarak izin verilen erişim miktarını tanımlamanızı sağlayan bir temsilci erişim modeli sağlar. Rol atamasının üç bileşeni vardır: güvenlik sorumlusu, rol tanımı ve kapsam. Windows sanal masaüstü temsilcisi erişim modeli, Azure RBAC modelini temel alır. Belirli rol atamaları ve bileşenleri hakkında daha fazla bilgi edinmek için bkz. [Azure rol tabanlı erişim denetimine genel bakış](../role-based-access-control/built-in-roles.md).

Windows sanal masaüstü temsilcisi erişimi, rol atamasının her bir öğesi için aşağıdaki değerleri destekler:

* Güvenlik sorumlusu
    * Kullanıcılar
    * Kullanıcı grupları
    * Hizmet sorumluları
* Rol tanımı
    * Yerleşik roller
    * Özel roller
* Kapsam
    * Konak havuzları
    * Uygulama grupları
    * Çalışma alanları

## <a name="powershell-cmdlets-for-role-assignments"></a>Rol atamaları için PowerShell cmdlet 'leri

Başlamadan önce, henüz yapmadıysanız Windows sanal masaüstü PowerShell modülünü ayarlamak için [PowerShell modülünü ayarlama](powershell-module.md) bölümündeki yönergeleri izlediğinizden emin olun.

Windows sanal masaüstü, uygulama gruplarını kullanıcılara veya Kullanıcı gruplarına yayımlarken Azure rol tabanlı erişim denetimi 'ni (RBAC) kullanır. Masaüstü Sanallaştırma Kullanıcı rolü kullanıcı veya kullanıcı grubuna atanır ve kapsam uygulama grubudur. Bu rol, kullanıcıya uygulama grubunda özel veri erişimi sağlar.  

Bir uygulama grubuna Azure Active Directory kullanıcıları eklemek için aşağıdaki cmdlet 'i çalıştırın:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'  
```

Azure Active Directory kullanıcı grubunu bir uygulama grubuna eklemek için aşağıdaki cmdlet 'i çalıştırın:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

## <a name="next-steps"></a>Sonraki adımlar

Her rolün kullanabileceği PowerShell cmdlet 'lerinin daha kapsamlı bir listesi için bkz. [PowerShell Başvurusu](/powershell/windows-virtual-desktop/overview).

Azure RBAC 'de desteklenen rollerin tam listesi için bkz. [Azure yerleşik rolleri](../role-based-access-control/built-in-roles.md).

Windows sanal masaüstü ortamı ayarlama yönergeleri için bkz. [Windows sanal masaüstü ortamı](environment-setup.md).
