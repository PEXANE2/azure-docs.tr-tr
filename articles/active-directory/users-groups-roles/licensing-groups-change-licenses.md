---
title: Kullanıcılar ve gruplar için lisans planlarını değiştirme - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'de grup lisanslamayı kullanarak grup içindeki kullanıcıları farklı hizmet planlarına geçirme
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf2f04e1728f94c89bddcc31c287cc017a79020f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025899"
---
# <a name="change-license-assignments-for-a-user-or-group-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki bir kullanıcı veya grup için lisans atamalarını değiştirme

Bu makalede, Azure Etkin Dizini'nde (Azure AD) kullanıcıların ve grupların hizmet lisans planları arasında nasıl taşınış edilebilen açıklanmaktadır. Azure AD'nin yaklaşımının amacı, lisans değişikliği sırasında hizmet veya veri kaybı olmamasını sağlamaktır. Kullanıcılar hizmetler arasında sorunsuz bir şekilde geçiş yapmalı. Bu makaledeki lisans planı atama adımları, Office 365 E1'deki bir kullanıcıveya grubu Office 365 E3'e değiştirmeyi açıklar, ancak adımlar tüm lisans planları için geçerlidir. Bir kullanıcı veya grup için lisans atamalarını güncellediğinizde, kullanıcıların lisans değişiklikleri sırasında hizmetlerine erişimlerini kaybetmemesi veya planlar arasındaki lisans çakışmalarını görmemesi için lisans ataması kaldırmaları ve yeni atamalar aynı anda yapılır.

## <a name="before-you-begin"></a>Başlamadan önce

Lisans atamalarını güncelleştirmeden önce, belirli varsayımların tüm kullanıcılar veya grupların güncelleştirilmesi için geçerli olduğunu doğrulamak önemlidir. Varsayımlar bir gruptaki tüm kullanıcılar için doğru değilse, geçiş bazıları için başarısız olabilir. Sonuç olarak, bazı kullanıcılar hizmetlere veya verilere erişimi kaybedebilir. Sağlamak:

- Kullanıcılar, bir gruba atanan ve kullanıcı tarafından devralınan ve doğrudan atanmayan geçerli lisans planına (bu durumda, Office 365 E1) sahiptir.

- Atadığınız lisans planı için yeterli lisansa sahipsiniz. Yeterli lisansınyoksa, bazı kullanıcılara yeni lisans planı atanmayabilir. Kullanılabilir lisans ların sayısını kontrol edebilirsiniz.

- Kullanıcıların, istenen lisansla çakışabilecek veya geçerli lisansın kaldırılmasını engelleyebilen başka atanmış hizmet lisansları yoktur. Örneğin, Diğer hizmetlere bağımlı olan İşyeri Analizi veya Project Online gibi bir hizmetten alınan lisans.

- Grupları şirket içinde yönetiyor ve Azure AD Connect ile Azure AD ile senkronize ediyorsanız, şirket içi sisteminizi kullanarak kullanıcıları ekler veya kaldırırsınız. Değişikliklerin Azure AD ile eşitleminin grup lisanslama tarafından alınması biraz zaman alabilir.

- Azure AD dinamik grup üyeliklerini kullanıyorsanız, kullanıcıların özniteliklerini değiştirerek ekler veya kaldırırsınız, ancak lisans atamaları için güncelleştirme işlemi aynı kalır.

## <a name="change-user-license-assignments"></a>Kullanıcı lisansı atamalarını değiştirme

Lisans **atamalarını güncelleştir** sayfasında, bazı onay kutularının kullanılolmadığını görürseniz, grup lisansından devralındıkları için değiştirilemeyen hizmetleri gösterir.

1. Azure REKLAM kuruluşunuzdaki Bir Lisans yöneticisi hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
1. **Azure Etkin Dizin** > **Kullanıcıları'nı**seçin ve ardından bir kullanıcı için **Profil** sayfasını açın.
1. **Lisansları**seçin.
1. Kullanıcı veya grup için lisans atamasını yeniden sağlamak için **Atamalar'ı** seçin. **Atamalar** sayfası, lisans atama çakışmalarını çözebileceğiniz yerdir.
1. Office 366 E3 için onay kutusunu seçin ve en azından kullanıcıya atanan tüm E1 hizmetlerinin seçildiğinden emin olun.
1. Office 365 E1 için onay kutusunu temizleyin.

    ![Office 365 E1'i gösteren bir kullanıcı için lisans atamaları sayfası temizlendi ve Office 365 E3 seçildi](media/licensing-groups-change-licenses/update-user-license-assignments.png)

1. **Kaydet'i**seçin.

Azure AD, hizmet sürekliliği sağlamak için yeni lisansları uygular ve eski lisansları aynı anda kaldırır.

## <a name="change-group-license-assignments"></a>Grup lisans atamalarını değiştirme

1. Azure REKLAM kuruluşunuzdaki Bir Lisans yöneticisi hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
1. **Azure Etkin Dizin** > **Grupları'nı**seçin ve ardından bir grup için **Genel Bakış** sayfasını açın.
1. **Lisansları**seçin.
1. Kullanıcı veya grup için lisans atamasını yeniden sağlamak için **Atamalar** komutunu seçin.
1. Office 366 E3 için onay kutusunu seçin. Hizmetin sürekliliğini korumak için, kullanıcıya zaten atanmış olan Tüm E1 hizmetlerini seçtiğinizden emin olun.
1. Office 365 E1 için onay kutusunu temizleyin.

    ![Bir kullanıcı veya grup Lisansları sayfasındaki Atamalar komutunu seçin](media/licensing-groups-change-licenses/update-group-license-assignments.png)

1. **Kaydet'i**seçin.

Azure AD, hizmet sürekliliği sağlamak için yeni lisansları uygular ve gruptaki tüm kullanıcılar için eski lisansları aynı anda kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde gruplar aracılığıyla lisans yönetimi için diğer senaryolar hakkında bilgi edinin:

- [Azure Active Directory'de gruba lisans atama](../users-groups-roles/licensing-groups-assign.md)
- [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)
- [Azure Active Directory'de tek tek lisanslı kullanıcıların grup lisanslanmasına nasıl geçirilir?](../users-groups-roles/licensing-groups-migrate-users.md)
- [Azure Active Directory grubu ek senaryoları lisanslama](../users-groups-roles/licensing-group-advanced.md)
- [Azure Active Directory'de grup lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
