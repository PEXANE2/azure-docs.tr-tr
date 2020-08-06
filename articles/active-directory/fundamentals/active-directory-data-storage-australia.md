---
title: Avustralya ve Yeni Zelanda müşterileri için kimlik verileri depolama-Azure AD
description: Azure Active Directory, Avustralya müşterileri için kimlikle ilgili verileri nerede depoladığını öğrenin.
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
ms.openlocfilehash: 97d651a1a6ce8ef412c148f16d0aa4412df94cb0
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87799557"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Azure Active Directory 'de Avustralya ve Yeni Zelanda müşterileri için kimlik verileri depolama

Kimlik verileri, Azure AD tarafından, Office 365 ve Azure gibi bir Microsoft Online hizmetine abone olurken kuruluşunuzun verdiği adrese bağlı olarak coğrafi bir konumda depolanır. Kimlik müşteri verilerinizin depolandığı yer hakkında bilgi için, Microsoft Güven Merkezi ' nin [veri konumunu nerede bulabilirsiniz?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünde bulabilirsiniz.

> [!NOTE]
> Azure AD ile tümleştirilen hizmetler ve uygulamalar, kimlik müşteri verilerine erişebilir. Kimlik müşteri verilerinin, belirli bir hizmet ve uygulama tarafından nasıl işlendiğini ve şirketinizin veri depolama gereksinimlerini karşılayıp karşılamadığını öğrenmek için kullandığınız her hizmeti ve uygulamayı değerlendirin. Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin Verileriniz nerede bulunur? bölümünü inceleyebilirsiniz.

Avustralya veya Yeni Zelanda 'da adres sağlayan müşteriler için Azure AD, Avustralya veri merkezleri içindeki bu hizmetler için kimlik verilerini tutar: 
- Azure AD dizin yönetimi 
- Kimlik Doğrulaması

Diğer tüm Azure AD Hizmetleri, müşteri verilerini küresel veri merkezlerinde depolar. Bir hizmetin veri merkezini bulmak için bkz. [Azure Active Directory – verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located)

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)

MFA, kimlik müşteri verilerini küresel veri merkezlerinde depolar. Bulut tabanlı Azure MFA ve Azure MFA sunucusu tarafından toplanan ve depolanan kullanıcı bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Multi-Factor Authentication Kullanıcı verileri koleksiyonu](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency).

## <a name="next-steps"></a>Sonraki adımlar
Yukarıda açıklanan özellik ve işlevlerden herhangi biri hakkında daha fazla bilgi için şu makalelere bakın:
- [Multi-Factor Authentication nedir?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
