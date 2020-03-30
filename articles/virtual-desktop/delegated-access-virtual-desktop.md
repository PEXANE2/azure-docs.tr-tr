---
title: Windows Sanal Masaüstünde temsilci erişimi - Azure
description: Örnekler de dahil olmak üzere, Windows Sanal Masaüstü dağıtımında yönetim özelliklerini nasıl devraaktarabilirsiniz?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3e27550ecc5b42c2bf0d947690da09e13d88ea4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128041"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne temsilci erişimi

Windows Sanal Masaüstü, belirli bir kullanıcının onlara bir rol atayarak sahip olmasına izin verilen erişim miktarını tanımlamanıza olanak tanıyan bir yetkin erişim modeline sahiptir. Rol ataması üç bileşenden oluşur: güvenlik ilkesi, rol tanımı ve kapsam. Windows Sanal Masaüstü temsilci erişim modeli Azure RBAC modelini temel alır. Belirli rol atamaları ve bileşenleri hakkında daha fazla bilgi edinmek için [Azure rol tabanlı erişim denetimine genel bakış'a](../role-based-access-control/built-in-roles.md)bakın.

Windows Sanal Masaüstü temsilci erişimi, rol atamasının her öğesi için aşağıdaki değerleri destekler:

* Güvenlik sorumlusu
    * Kullanıcılar
    * Hizmet sorumluları
* Rol tanımı
    * Yerleşik roller
* Kapsam
    * Kiracı grupları
    * Kiracılar
    * Ana havuzlar
    * Uygulama grupları

## <a name="built-in-roles"></a>Yerleşik roller

Windows Sanal Masaüstü'nde temsilci erişimi, kullanıcılara ve hizmet ilkelerine atayabileceğiniz birkaç yerleşik rol tanımına sahiptir.

* Bir RDS Sahibi, kaynaklara erişim de dahil olmak üzere her şeyi yönetebilir.
* Bir RDS Katılımcısı kaynaklara erişim hariç her şeyi yönetebilir.
* RdS Reader her şeyi görüntüleyebilir, ancak değişiklik yapamaz.
* Bir RDS Operatörü tanılama etkinliklerini görüntüleyebilir.

## <a name="powershell-cmdlets-for-role-assignments"></a>Rol atamaları için PowerShell cmdlets

Rol atamaları oluşturmak, görüntülemek ve kaldırmak için aşağıdaki cmdlets çalıştırabilirsiniz:

* **Get-RdsRoleAssignment** rol atamalarının listesini görüntüler.
* **Yeni-RdsRoleAssignment** yeni bir rol ataması oluşturur.
* **Remove-RdsRoleAssignment** rol atamalarını siler.

### <a name="accepted-parameters"></a>Kabul edilen parametreler

Temel üç cmdlet'i aşağıdaki parametrelerle değiştirebilirsiniz:

* **AadTenantId**: hizmet yöneticisinin üyesi olduğu Azure Active Directory kiracı kimliğini belirtir.
* **AppGroupName**: Uzak Masaüstü uygulama grubunun adı.
* **Tanılama**: tanılama kapsamını gösterir. **(Altyapı** veya **Kiracı** parametreleri ile eşleştirilmiş olmalıdır.)
* **HostPoolName**: Uzak Masaüstü ana bilgisayar havuzunun adı.
* **Altyapı**: altyapı kapsamını gösterir.
* **RoleDefinitionName**: Kullanıcıya, gruba veya uygulamaya atanan Uzak Masaüstü Hizmetleri rol tabanlı erişim denetimi rolünün adı. (Örneğin, Uzak Masaüstü Hizmetleri Sahibi, Uzak Masaüstü Hizmetleri Okuyucu vb.)
* **ServerPrincipleName**: Azure Active Directory uygulamasının adı.
* **SignInName**: kullanıcının e-posta adresi veya kullanıcı nın ana adı.
* **TenantName**: Uzak Masaüstü kiracının adı.

## <a name="next-steps"></a>Sonraki adımlar

Her rolün kullanabileceği PowerShell cmdletlerinin daha eksiksiz bir listesi için [PowerShell referansına](/powershell/windows-virtual-desktop/overview)bakın.

Windows Sanal Masaüstü ortamının nasıl ayarlanda ayarlanabilenlere ilişkin yönergeler için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.
