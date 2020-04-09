---
title: Birden çok kiracı etkileşiminin özellikleri - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory kiracılarınızı tamamen bağımsız kuruluşlar olarak anlama
services: active-tenant
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 04/07/2020
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: 175d9ce7db1657e0e654f46adaf8a8d8ef28c25e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878128"
---
# <a name="understand-how-multiple-azure-active-directory-organizations-interact"></a>Birden çok Azure Etkin Dizin kuruluşunun nasıl etkileşimde olduğunu anlama

Azure Etkin Dizin (Azure AD)'nde her kiracı tamamen bağımsız bir kuruluştur: yönettiğiniz diğer Azure AD kuruluşlarından mantıksal olarak bağımsız bir eş. Kuruluşlar arasındaki bu bağımsızlık kaynak bağımsızlığını, idari bağımsızlığı ve senkronizasyon bağımsızlığını içerir. Kuruluşlar arasında üst-alt ilişkisi yoktur.

## <a name="resource-independence"></a>Kaynak bağımsızlığı

* Bir kuruluşta bir Azure REKLAM kaynağı oluşturur veya silerseniz, dış kullanıcılar dışında kısmi istisna dışında başka bir kuruluştaki hiçbir kaynak üzerinde hiçbir etkisi yoktur.
* Alan adlarından birini bir kuruluşa kaydettirecekolursa, başka bir kuruluş tarafından kullanılamaz.

## <a name="administrative-independence"></a>İdari bağımsızlık

'Contoso' kuruluşunun yönetim dışı bir kullanıcısı 'Test' bir test organizasyonu oluşturursa, o zaman:

* Varsayılan olarak, bir kuruluş oluşturan kullanıcı bu yeni kuruluşa dış kullanıcı olarak eklenir ve bu kuruluşta genel yönetici rolü atanır.
* 'Contoso' kuruluşunun yöneticilerinin, 'Test' yöneticisi onlara bu ayrıcalıkları özel olarak vermedikçe ,'Test' kuruluşuna doğrudan yönetim ayrıcalıkları yoktur. Ancak, 'Contoso' yöneticileri, 'Test' oluşturulan kullanıcı hesabını denetlerse organizasyon 'Test'e erişimi denetleyebilir.
* Bir kuruluştaki bir kullanıcı için bir Azure REKLAM rolü ekler veya kaldırırsanız, değişiklik kullanıcının başka bir Azure REKLAM kuruluşunda atandığı rolleri etkilemez.

## <a name="synchronization-independence"></a>Senkronizasyon bağımsızlığı

Her Azure REKLAM kuruluşunu, verilerin tek bir örneğinden eşitlenmesini sağlamak için bağımsız olarak yapılandırabilirsiniz:

* Verileri tek bir AD ormanıyla eşitlemek için Azure AD Connect aracı.
* Verileri bir veya daha fazla şirket içi ormanla ve/veya Azure'a ait olmayan REKLAM veri kaynaklarıyla senkronize etmek için Ön Ön Kimlik Yöneticisi için Azure Etkin Dizin Bağlayıcısı.

## <a name="add-an-azure-ad-organization"></a>Azure AD kuruluşu ekleme

Azure portalına bir Azure REKLAM kuruluşu eklemek için, Azure AD global yöneticisi olan bir hesapla [Azure portalında](https://portal.azure.com) oturum açın ve **Yeni'yi**seçin.

> [!NOTE]
> Diğer Azure kaynaklarının aksine, Azure REKLAM kuruluşlarınız Bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğiniz iptal edildiyse veya süresi dolduysa, Azure PowerShell, Microsoft Graph API veya Microsoft 365 yönetici merkezini kullanarak Azure REKLAM kuruluşunuzun verilerine erişebilir. [Ayrıca, başka bir aboneliği kuruluşla ilişkilendirebilirsiniz.](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
>

## <a name="next-steps"></a>Sonraki adımlar

Azure AD lisanslama hususları ve en iyi uygulamalar için [bkz.](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
