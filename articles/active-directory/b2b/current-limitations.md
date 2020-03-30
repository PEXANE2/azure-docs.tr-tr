---
title: B2B işbirliğinin sınırlamaları - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği için geçerli sınırlamalar
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: ffee01488ecf658ce02a20a41252aca19288667c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263367"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliğinin sınırlamaları
Azure Etkin Dizin (Azure AD) B2B işbirliği şu anda bu makalede açıklanan sınırlamalara tabidir.

## <a name="possible-double-multi-factor-authentication"></a>Olası çift çok faktörlü kimlik doğrulama
Azure AD B2B ile kaynak kuruluşta (davet eden kuruluş) çok faktörlü kimlik doğrulaması uygulayabilirsiniz. Bu yaklaşımın nedenleri [B2B işbirliği kullanıcıları için Koşullu Erişim](conditional-access.md)ayrıntılı olarak açıklanmaktadır. Bir iş ortağızaten çok faktörlü kimlik doğrulaması ayarlanmış ve zorlanmışsa, kullanıcılarının kimlik doğrulamasını kendi ev kuruluşunda bir kez ve daha sonra tekrar sizin evinizde gerçekleştirmesi gerekebilir.

## <a name="instant-on"></a>Anında Açma
B2B işbirliği akışlarında, kullanıcıları dizine ekler ve davet kullanımı, uygulama ataması ve benzeri durumlarda bunları dinamik olarak güncelleriz. Güncelleştirmeler ve yazmalar normalde tek bir dizin örneğinde gerçekleşir ve tüm örneklerde çoğaltılması gerekir. Tüm örnekler güncelleştirildikten sonra çoğaltma tamamlanır. Bazen nesne bir örnekte yazıldığında veya güncelleştirildiğinde ve bu nesneyi alma çağrısı başka bir örneğe geldiğinde, çoğaltma gecikmeleri oluşabilir. Bu durumda, yenilemek veya yardım etmeye yeniden çalışın. Bizim API kullanarak bir uygulama yazıyorsanız, o zaman bazı back-off ile yeniden bu sorunu hafifletmek için iyi, savunma uygulamadır.

## <a name="azure-ad-directories"></a>Azure REKLAM dizinleri
Azure AD B2B, Azure AD hizmet dizini sınırlarına tabidir. Bir kullanıcının oluşturabileceği dizin sayısı ve bir kullanıcının veya konuk kullanıcının ait olabileceği dizin sayısı hakkında ayrıntılı bilgi için [Azure REKLAM hizmet sınırları ve kısıtlamalarına](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions)bakın.

## <a name="national-clouds"></a>Ulusal bulutlar
[Ulusal bulutlar,](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) fiziksel olarak yalıtılmış Azure örnekleridir. B2B işbirliği ulusal bulut sınırları içinde desteklenmez. Örneğin, Azure kiracınız genel bulutta, genel buluttaysa, hesabı ulusal bulutta olan bir kullanıcıyı davet edemezsiniz. Kullanıcıyla işbirliği yapmak için, onlardan başka bir e-posta adresi isteyin veya dizininizde onlar için bir üye kullanıcı hesabı oluşturun.

## <a name="azure-us-government-clouds"></a>Azure ABD Devlet bulutları
Azure ABD Hükümeti bulutu içinde, B2B işbirliği şu anda yalnızca hem Azure ABD Hükümeti bulutu içinde olan hem de B2B işbirliğini destekleyen kiracılar arasında desteklenir. Bir kullanıcıyı Azure ABD Kamu bulutunun bir parçası olmayan veya henüz B2B işbirliğini desteklemeyen bir kiracıya davet ederseniz, davet başarısız olur veya kullanıcı daveti kullanamaz. Diğer sınırlamalar hakkında ayrıntılı bilgi için [Azure Active Directory Premium P1 ve P2 Varyasyonları'na](https://docs.microsoft.com/azure/azure-government/documentation-government-services-securityandidentity#azure-active-directory-premium-p1-and-p2)bakın.

### <a name="how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant"></a>B2B işbirliğinin Azure ABD Devlet kiracımda kullanılabilir olup olmadığını nasıl anlayabilirim?
Azure ABD Kamu bulut kiracınızın B2B işbirliğini destekleyip desteklemeyin bunu öğrenmek için aşağıdakileri yapın:

1. Bir tarayıcıda, * &lt;kiracı adınızı&gt;* n için aşağıdaki URL'ye gidin:

   `https://login.microsoftonline.com/<tenantname>/v2.0/.well-known/openid-configuration`

2. JSON yanıtı nda bulun: `"tenant_region_scope"`

   - `"tenant_region_scope":"USGOV”` Görünürse, B2B desteklenir.
   - `"tenant_region_scope":"USG"` Görünürse, B2B desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Temsilci B2B işbirliği davetiyeleri](delegate-invitations.md)

