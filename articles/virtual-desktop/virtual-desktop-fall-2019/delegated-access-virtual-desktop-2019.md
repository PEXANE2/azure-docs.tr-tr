---
title: Windows sanal masaüstü 'nde temsilci erişimi-Azure
description: Örnekler de dahil olmak üzere Windows sanal masaüstü dağıtımında yönetim özellikleri atama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 899e42e3b592ce1bc92d7d47578e10355b1c6de7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82614440"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne temsilci erişimi

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir. Spring 2020 güncelleştirmesinde tanıtılan Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../delegated-access-virtual-desktop.md)bakın.

Windows sanal masaüstü, belirli bir kullanıcının bir rol atayarak izin verilen erişim miktarını tanımlamanızı sağlayan bir temsilci erişim modeli sağlar. Rol atamasının üç bileşeni vardır: güvenlik sorumlusu, rol tanımı ve kapsam. Windows sanal masaüstü temsilcisi erişim modeli, Azure RBAC modelini temel alır. Belirli rol atamaları ve bileşenleri hakkında daha fazla bilgi edinmek için bkz. [Azure rol tabanlı erişim denetimine genel bakış](../../role-based-access-control/built-in-roles.md).

Windows sanal masaüstü temsilcisi erişimi, rol atamasının her bir öğesi için aşağıdaki değerleri destekler:

* Güvenlik sorumlusu
    * Kullanıcılar
    * Hizmet sorumluları
* Rol tanımı
    * Yerleşik roller
* Kapsam
    * Kiracı grupları
    * Kiracılar
    * Konak havuzları
    * Uygulama grupları

## <a name="built-in-roles"></a>Yerleşik roller

Windows sanal masaüstü 'nde temsilci erişimi, kullanıcılara ve hizmet sorumlularına atayabileceğiniz birkaç yerleşik rol tanımına sahiptir.

* Bir RDS sahibi, kaynaklara erişim dahil her şeyi yönetebilir.
* Bir RDS katılımcısı her şeyi yönetebilir, ancak kaynaklara erişemez.
* Bir RDS okuyucusu her şeyi görüntüleyebilir, ancak herhangi bir değişiklik yapamaz.
* Bir RDS Işleci, tanılama etkinliklerini görüntüleyebilir.

## <a name="powershell-cmdlets-for-role-assignments"></a>Rol atamaları için PowerShell cmdlet 'leri

Rol atamalarını oluşturmak, görüntülemek ve kaldırmak için aşağıdaki cmdlet 'leri çalıştırabilirsiniz:

* **Get-RdsRoleAssignment** , rol atamalarının bir listesini görüntüler.
* **New-RdsRoleAssignment** yeni bir rol ataması oluşturur.
* **Remove-RdsRoleAssignment** , rol atamalarını siler.

### <a name="accepted-parameters"></a>Kabul edilen parametreler

Temel üç cmdlet 'i aşağıdaki parametrelerle değiştirebilirsiniz:

* **Aadtenantıd**: hizmet sorumlusunun üye olduğu Azure ACTIVE DIRECTORY Kiracı kimliğini belirtir.
* **Appgroupname**: Uzak Masaüstü uygulama grubunun adı.
* **Tanılama**: Tanılama kapsamını gösterir. ( **Altyapıya** da **kiracı** parametreleriyle eşleştirilmelidir.)
* **Hostpoolname**: Uzak Masaüstü konak havuzunun adı.
* **Altyapı**: altyapı kapsamını belirtir.
* **Roledefinitionname**: Kullanıcı, Grup veya uygulamaya atanan Uzak Masaüstü Hizmetleri rol tabanlı erişim denetimi rolünün adı. (Örneğin, Uzak Masaüstü Hizmetleri sahibi, Uzak Masaüstü Hizmetleri okuyucu vb.)
* **Serverprentaname**: Azure Active Directory uygulamasının adı.
* **Signınname**: kullanıcının e-posta adresi veya Kullanıcı asıl adı.
* **TenantName**: Uzak Masaüstü kiracının adı.

## <a name="next-steps"></a>Sonraki adımlar

Her rolün kullanabileceği PowerShell cmdlet 'lerinin daha kapsamlı bir listesi için bkz. [PowerShell Başvurusu](/powershell/windows-virtual-desktop/overview).

Windows sanal masaüstü ortamı ayarlama yönergeleri için bkz. [Windows sanal masaüstü ortamı](environment-setup-2019.md).
