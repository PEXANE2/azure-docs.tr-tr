---
title: Birden çok kiracı etkileşiminin özellikleri - Azure AD | Microsoft Dokümanlar
description: Azure Etkin kiracılarınızı, kiracılarınızı tamamen bağımsız kaynaklar olarak anlayarak yönetin
services: active-tenant
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 11/08/2019
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4eb09ab7fa31af5edf14b113a6a88e08df2d115
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562267"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Birden çok Azure Etkin Dizin kiracılarının nasıl etkileşimde olduğunu anlama

Azure Etkin Dizini'nde (Azure AD) her kiracı tamamen bağımsız bir kaynaktır: yönettiğiniz diğer kiracılardan mantıksal olarak bağımsız bir eş. Kiracılar arasında ebeveyn-alt ilişkisi yoktur. Kiracılar arasındaki bu bağımsızlık kaynak bağımsızlığını, idari bağımsızlığı ve senkronizasyon bağımsızlığını içerir.

## <a name="resource-independence"></a>Kaynak bağımsızlığı
* Bir kiracıda bir kaynak oluşturur veya silerseniz, dış kullanıcılar dışında kısmi istisna dışında, başka bir kiracıdaki herhangi bir kaynak üzerinde hiçbir etkisi yoktur. 
* Alan adlarından birini bir kiracıyla kullanıyorsanız, başka bir kiracıyla kullanılamaz.

## <a name="administrative-independence"></a>İdari bağımsızlık
Kiracının 'Contoso' yönetim dışı kullanıcısı bir test kiracısı 'Test' oluşturursa, o zaman:

* Varsayılan olarak, kiracı oluşturan kullanıcı bu yeni kiracıya dış kullanıcı olarak eklenir ve bu kiracıda genel yönetici rolü atanır.
* Kiracı 'Contoso' yöneticileri, 'Test' yöneticisi onlara bu ayrıcalıkları özel olarak vermedikçe, kiracı 'Test'e doğrudan yönetim ayrıcalıkları yoktur. Ancak, 'Contoso' yöneticileri, 'Test'i oluşturan kullanıcı hesabını denetlerse kiracı 'Test'e erişimi denetleyebilir.
* Bir kiracıda bir kullanıcı için yönetici rolü ekler/kaldırırsanız, değişiklik kullanıcının başka bir kiracıda sahip olduğu yönetici rollerini etkilemez.

## <a name="synchronization-independence"></a>Senkronizasyon bağımsızlığı
Aşağıdakilerden birinin tek bir örneğinden veri eşitlemek için her Azure AD kiracısını bağımsız olarak yapılandırabilirsiniz:

* Verileri tek bir AD ormanıyla eşitlemek için Azure AD Connect aracı.
* Verileri bir veya daha fazla şirket içi ormanla ve/veya Azure'a ait olmayan REKLAM veri kaynaklarıyla senkronize etmek için Ön Ön Plan Kimlik Yöneticisi için Azure Etkin kiracı Bağlayıcısı.

## <a name="add-an-azure-ad-tenant"></a>Azure AD kiracısı ekleme
Azure portalına bir Azure AD kiracısı eklemek için, Azure AD global yöneticisi olan bir hesapla [Azure portalında](https://portal.azure.com) oturum açın ve solda **Yeni'yi**seçin.

> [!NOTE]
> Diğer Azure kaynaklarının aksine, kiracılarınız Bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğiniz iptal edildiyse veya süresi dolduysa, Azure PowerShell, Microsoft Graph API veya Microsoft 365 yönetici merkezini kullanarak kiracı verilerinize erişebilirsiniz. [Ayrıca kiracı ile başka bir abonelik ilişkilendirebilirsiniz.](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
>

## <a name="next-steps"></a>Sonraki adımlar
Azure AD lisanslama sorunlarına ve en iyi uygulamalara genel bir bakış için [bkz.](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
