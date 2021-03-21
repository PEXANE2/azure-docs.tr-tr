---
title: Avustralya ve Yeni Zelanda müşterileri için kimlik verileri depolama-Azure AD
description: Azure Active Directory, Avustralya ve Yeni Zelanda müşterileri için kimlikle ilgili verileri nerede depoladığını öğrenin.
services: active-directory
author: ajburnle
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 498b75a6541da97e57e23465ae6eb23a6c854727
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94836963"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Azure Active Directory 'de Avustralya ve Yeni Zelanda müşterileri için kimlik verileri depolama

Kimlik verileri, Azure AD tarafından, Microsoft 365 ve Azure gibi bir Microsoft Online hizmetine abone olurken kuruluşunuzun verdiği adrese bağlı olarak coğrafi bir konumda depolanır. Kimlik müşteri verilerinizin depolandığı yer hakkında bilgi için, Microsoft Güven Merkezi ' nin [veri konumunu nerede bulabilirsiniz?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünde bulabilirsiniz.

> [!NOTE]
> Azure AD ile tümleştirilen hizmetler ve uygulamalar, kimlik müşteri verilerine erişebilir. Kimlik müşteri verilerinin, belirli bir hizmet ve uygulama tarafından nasıl işlendiğini ve şirketinizin veri depolama gereksinimlerini karşılayıp karşılamadığını öğrenmek için kullandığınız her hizmeti ve uygulamayı değerlendirin. Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin Verileriniz nerede bulunur? bölümünü inceleyebilirsiniz.

Avustralya veya Yeni Zelanda 'da adres sağlayan müşteriler için Azure AD, Avustralya veri merkezleri içindeki bu hizmetler için kimlik verilerini tutar: 
- Azure AD dizin yönetimi 
- Kimlik Doğrulaması

Diğer tüm Azure AD Hizmetleri, müşteri verilerini küresel veri merkezlerinde depolar. Bir hizmetin veri merkezini bulmak için bkz. [Azure Active Directory – verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located)

## <a name="microsoft-azure-ad-multi-factor-authentication-mfa"></a>Microsoft Azure AD Multi-Factor Authentication (MFA)

MFA, kimlik müşteri verilerini küresel veri merkezlerinde depolar. Bulut tabanlı Azure AD MFA ve Azure MFA sunucusu tarafından toplanan ve depolanan kullanıcı bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Multi-Factor Authentication Kullanıcı verileri koleksiyonu](../authentication/concept-mfa-data-residency.md).

## <a name="next-steps"></a>Sonraki adımlar
Yukarıda açıklanan özellik ve işlevlerden herhangi biri hakkında daha fazla bilgi için şu makalelere bakın:
- [Multi-Factor Authentication nedir?](../authentication/concept-mfa-howitworks.md)