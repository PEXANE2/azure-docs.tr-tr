---
title: B2B işbirliği sınırlamaları-Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: b45277c89193c51f70836bcef8a21636fc9c7973
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77196142"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliğinin sınırlamaları
Azure Active Directory (Azure AD) B2B işbirliği Şu anda bu makalede açıklanan sınırlamalara tabidir.

## <a name="possible-double-multi-factor-authentication"></a>Olası çift Multi-Factor Authentication
Azure AD B2B ile, çok faktörlü kimlik doğrulamasını kaynak kuruluşta (kuruluşun davet eden) zorunlu kılabilirsiniz. Bu yaklaşımın nedenleri, [B2B işbirliği kullanıcıları Için koşullu erişim](conditional-access.md)bölümünde ayrıntılı olarak açıklanmıştır. Bir iş ortağının zaten çok faktörlü kimlik doğrulaması ayarlandıysa ve zorlanmışsa, kullanıcıların kimlik doğrulamasını ana kuruluşlarındaki bir kez ve daha sonra bir kez gerçekleştirmesi gerekebilir.

## <a name="instant-on"></a>Anında
B2B işbirliği akışlarında, kullanıcıları dizine ekler ve davet satın alma, uygulama ataması vb. sırasında bunları dinamik olarak güncelleştirir. Güncelleştirmeler ve yazma işlemleri normalde tek bir dizin örneğinde gerçekleşir ve tüm örneklerde çoğaltılmalıdır. Tüm örnekler güncelleştirildikten sonra çoğaltma tamamlanır. Bazen nesne bir örnekte yazıldığında veya güncelleştirilirse ve bu nesneyi almak için yapılan çağrı başka bir örneğe ise çoğaltma gecikmeleri oluşabilir. Bu durumda, yenileyin veya yardım için yeniden deneyin. API 'imizi kullanarak bir uygulama yazıyorsanız, bu sorunu giderebilmeniz için bazı geri denemelerle yeniden denemeler yapmak iyi bir savunma yöntemidir.

## <a name="azure-ad-directories"></a>Azure AD dizinleri
Azure AD B2B, Azure AD hizmeti dizin sınırlarına tabidir. Bir kullanıcının oluşturabileceğiniz dizin sayısı ve bir kullanıcının veya Konuk kullanıcının ait olduğu dizin sayısı hakkında daha fazla bilgi için bkz. [Azure AD hizmet limitleri ve kısıtlamaları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="national-clouds"></a>Ulusal bulutlar
[Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) , Azure 'un fiziksel olarak yalıtılmış örnekleridir. B2B işbirliği Ulusal bulut sınırları genelinde desteklenmez. Örneğin, Azure kiracınız genel, genel bulutda ise, hesabı ulusal bir bulutta olan bir kullanıcıyı davet edebilirsiniz. Kullanıcıyla işbirliği yapmak için, başka bir e-posta adresi isteyin veya dizininizdeki kullanıcılar için bir üye kullanıcı hesabı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davetlerini devretmek](delegate-invitations.md)

