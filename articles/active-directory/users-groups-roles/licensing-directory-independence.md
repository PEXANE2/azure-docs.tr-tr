---
title: Birden çok kiracı etkileşiminin özellikleri-Azure AD | Microsoft Docs
description: Kiracılarınızı tamamen bağımsız kaynaklar olarak tanımlayarak Azure etkin kiracı Kiracılarınızı yönetin
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
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562267"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Birden çok Azure Active Directory kiracının nasıl etkileşime gireceğini anlayın

Azure Active Directory (Azure AD) ' de, her kiracı tam olarak bağımsız bir kaynaktır: yönettiğiniz diğer kiracılardan mantıksal olarak bağımsız bir eş. Kiracılar arasında üst-alt ilişkisi yoktur. Kiracılar arasındaki bu bağımsızlık kaynak bağımsızlık, yönetim bağımsızlık ve eşitleme bağımsızlığı bulunur.

## <a name="resource-independence"></a>Kaynak bağımsızlık
* Bir kiracıda bir kaynak oluşturur veya silerseniz, dış kullanıcıların kısmi özel durumu ile başka bir Kiracıdaki herhangi bir kaynak üzerinde hiçbir etkisi olmaz. 
* Etki alanı adlarınızın birini tek bir kiracı ile kullanırsanız, başka bir kiracıyla kullanılamaz.

## <a name="administrative-independence"></a>Yönetim bağımsızlık
' Contoso ' kiracının yönetici olmayan kullanıcısı bir test kiracısı ' test ' i oluşturursa:

* Varsayılan olarak, bir kiracı oluşturan kullanıcı, bu yeni kiracıya bir dış Kullanıcı olarak eklenir ve bu kiracıya genel yönetici rolü atanır.
* ' Test ' Yöneticisi özel olarak onlara bu ayrıcalıkları vermediği takdirde, ' contoso ' kiracının yöneticilerinin ' test ' kiracısında doğrudan yönetim ayrıcalıkları yoktur. Ancak ' contoso ' yöneticileri, ' test ' ' in oluşturduğu kullanıcı hesabını denetlerse kiracıya erişimi denetleyebilir.
* Bir Kiracıdaki bir kullanıcı için yönetici rolü ekler/kaldırırsanız, değişiklik kullanıcının başka bir kiracıda sahip olduğu yönetici rollerini etkilemez.

## <a name="synchronization-independence"></a>Eşitleme bağımsızlığı
Her bir Azure AD kiracısını, her birinin tek bir örneğinden eşitlenmiş verileri almak için bağımsız olarak yapılandırabilirsiniz:

* Azure AD Connect Aracı, verileri tek bir AD ormanıyla eşitler.
* Forefront Identity Manager için Azure Active kiracı Bağlayıcısı, verileri bir veya daha fazla şirket içi ormanla ve/veya Azure dışı AD veri kaynaklarıyla eşitlemeye yöneliktir.

## <a name="add-an-azure-ad-tenant"></a>Azure AD kiracısı ekleme
Azure portal bir Azure AD kiracısı eklemek için Azure AD Genel Yöneticisi olan bir hesapla [Azure Portal](https://portal.azure.com) oturum açın ve sol tarafta **Yeni**' yi seçin.

> [!NOTE]
> Diğer Azure kaynaklarından farklı olarak, kiracılarınız bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğiniz iptal edildiğinde veya dolmuşsa, kiracı verilerinize Azure PowerShell, Microsoft Graph API 'sini veya Microsoft 365 Yönetim merkezini kullanarak erişmeye devam edebilirsiniz. Ayrıca [, başka bir aboneliği kiracı ile ilişkilendirebilirsiniz](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Sonraki adımlar
Azure AD lisanslama sorunlarına ve en iyi yöntemlere ilişkin geniş bir genel bakış için bkz. [Azure etkin kiracı lisanslama nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
