---
title: Birden çok kiracı etkileşiminin özellikleri-Azure AD | Microsoft Docs
description: Azure Active Directory kuruluşlarınızın veri bağımsızlığını anlama
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 04/29/2020
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1db812fb9c1503771bec0b1795ee8c15bb70014a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84727713"
---
# <a name="understand-how-multiple-azure-active-directory-organizations-interact"></a>Çoklu Azure Active Directory kuruluşların nasıl etkileşime gireceğini anlayın

Azure Active Directory (Azure AD) ' de, her Azure AD kuruluşu tamamen bağımsızdır: yönettiğiniz diğer Azure AD kuruluşlarından mantıksal olarak bağımsız olan bir eş. Kuruluşlar arasındaki bu bağımsızlık, kaynak bağımsızlığı, yönetim bağımsızlık ve eşitleme bağımsızlığı bulunur. Kuruluşlar arasında üst-alt ilişkisi yoktur.

## <a name="resource-independence"></a>Kaynak bağımsızlık

* Bir kuruluşta bir Azure AD kaynağı oluşturur veya silerseniz, dış kullanıcıların kısmi özel durumu ile başka bir kuruluştaki herhangi bir kaynak üzerinde hiçbir etkisi olmaz.
* Etki alanı adlarınızın birini tek bir kuruluşa kaydettiğinizde, başka bir kuruluş tarafından kullanılamaz.

## <a name="administrative-independence"></a>Yönetim bağımsızlık

' Contoso ' kuruluşunun yönetici olmayan kullanıcısı bir test kuruluşu ' test ' öğesini oluşturursa:

* Varsayılan olarak, bir kuruluş oluşturan kullanıcı, bu yeni kuruluşa bir dış Kullanıcı olarak eklenir ve bu kuruluşta genel yönetici rolü atanır.
* ' Test ' Yöneticisi özel olarak onlara bu ayrıcalıkları vermediği takdirde, ' contoso ' kuruluşunun yöneticileri kuruluş ' test ' üzerinde doğrudan yönetim ayrıcalıklarına sahip değildir. Ancak ' contoso ' yöneticileri, ' test ' tarafından oluşturulan kullanıcı hesabını denetlerse ' test ' kuruluşuna erişimi denetleyebilir.
* Bir kuruluştaki bir kullanıcı için bir Azure AD rolü ekler veya kaldırırsanız, bu değişiklik kullanıcının başka bir Azure AD kuruluşunda atandığı rolleri etkilemez.

## <a name="synchronization-independence"></a>Eşitleme bağımsızlığı

Her bir Azure AD kuruluşunu, her birinin tek bir örneğinden eşitlenmiş verileri almak için bağımsız olarak yapılandırabilirsiniz:

* Azure AD Connect Aracı, verileri tek bir AD ormanıyla eşitler.
* Forefront Identity Manager için Azure Active Directory bağlayıcı, verileri bir veya daha fazla şirket içi ormanla ve/veya Azure dışı AD veri kaynakları ile eşitler.

## <a name="add-an-azure-ad-organization"></a>Azure AD organizasyonu ekleme

Azure portal bir Azure AD organizasyonu eklemek için Azure AD Genel Yöneticisi olan bir hesapla [Azure Portal](https://portal.azure.com) oturum açın ve **Yeni**' yi seçin.

> [!NOTE]
> Diğer Azure kaynaklarından farklı olarak, Azure AD kuruluşlarınız bir Azure aboneliğinin alt kaynakları değildir. Azure aboneliğiniz iptal edildiğinde veya dolmuşsa, Azure AD kuruluşunuzun verilerine Azure PowerShell, Microsoft Graph API 'sini veya Microsoft 365 Yönetim merkezini kullanarak erişmeye devam edebilirsiniz. Ayrıca, [başka bir aboneliği kuruluşla ilişkilendirebilirsiniz](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Sonraki adımlar

Azure AD lisanslama konuları ve en iyi uygulamalar için bkz. [Azure Active Directory lisanslama nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
