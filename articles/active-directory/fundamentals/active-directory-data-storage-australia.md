---
title: Avustralya ve Yeni Zelandamüşterileri için kimlik veri depolama - Azure AD
description: Azure Active Directory'nin Avustralyalı müşterileri için kimlikle ilgili verileri nerede depoladığı hakkında bilgi edinin.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 850298719d5636e964b0c338d7a2a4cc9bb8aece
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77370301"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Azure Active Directory'de Avustralya ve Yeni Zelanda müşterileri için kimlik veri depolama

Kimlik verileri, Azure AD tarafından kuruluşunuzun Office 365 ve Azure gibi bir Microsoft Çevrimiçi hizmetine abone olurken sağladığı adrese dayalı coğrafi bir konumda depolanır. Kimlik Müşteri Verilerinizin nerede depolandığı hakkında bilgi almak [için, verilerinizin nerede bulunduğu?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü microsoft güven merkezinin kullanabilirsiniz.

> [!NOTE]
> Azure AD ile tümleşen hizmetler ve uygulamalar Kimlik Müşteri Verilerine erişebilir. Kimlik Müşteri Verilerinin söz konusu hizmet ve uygulama tarafından nasıl işlenip işlenmediğini ve şirketinizin veri depolama gereksinimlerini karşılayıp karşılamadığını belirlemek için kullandığınız her hizmet ve uygulamayı değerlendirin. Microsoft hizmetlerinin verileri depoladığı konumlar hakkında daha fazla bilgi için Microsoft Güven Merkezi'nin Verileriniz nerede bulunur? bölümünü inceleyebilirsiniz.

Azure AD, Avustralya veya Yeni Zelanda'da adres sağlayan müşteriler için bu hizmetlerin kimlik verilerini Avustralya veri merkezlerinde saklar: 
- Azure AD Dizin Yönetimi 
- Kimlik doğrulaması

Diğer tüm Azure AD hizmetleri müşteri verilerini genel veri merkezlerinde depolar. Bir hizmetin veri merkezini bulmak için Azure [Etkin Dizini – Verileriniz nerede bulunur?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located)

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure çok faktörlü kimlik doğrulama (MFA)

MFA, Kimlik Müşteri Verilerini küresel veri merkezlerinde depolar. Bulut tabanlı Azure MFA ve Azure MFA Server tarafından toplanan ve depolanan kullanıcı bilgileri hakkında daha fazla bilgi edinmek için [Azure Çok Faktörlü Kimlik Doğrulama kullanıcı veri koleksiyonuna](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Yukarıda açıklanan özellikler ve işlevler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- [Multi-Factor Authentication nedir?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
