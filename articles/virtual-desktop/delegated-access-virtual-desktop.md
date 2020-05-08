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
ms.openlocfilehash: 16b4fca475f91a8cb5b7f9a20ea5aa74b6b674a3
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612869"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne temsilci erişimi

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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
    * Çalışma Alanları

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
