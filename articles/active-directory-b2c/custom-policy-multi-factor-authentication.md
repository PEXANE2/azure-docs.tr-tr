---
title: Azure Active Directory B2C Multi-Factor Authentication | Microsoft Docs
description: Azure Active Directory B2C tarafından sağlanan tüketiciye yönelik uygulamalarda Multi-Factor Authentication etkinleştirme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c0ab4d367d2ca9fd59c26ce8d36b5f80dcb832c8
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94840636"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'de Multi-Factor Authentication 'ı etkinleştirme

Azure Active Directory B2C (Azure AD B2C), uygulamalarınızda kayıt ve oturum açma deneyimlerine ikinci bir güvenlik katmanı ekleyebilmeniz için doğrudan [Azure AD Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) ile tümleşir. Multi-Factor Authentication 'ı tek bir kod satırı yazmadan etkinleştirirsiniz. Zaten kaydolma ve oturum açma Kullanıcı akışları oluşturduysanız, çok faktörlü kimlik doğrulamasını yine de etkinleştirebilirsiniz.

Bu özellik, uygulamaların aşağıdaki gibi senaryoları işlemesine yardımcı olur:

- Bir uygulamaya erişmek için Multi-Factor Authentication gerekmez, ancak başka bir uygulamaya erişmesi gerekir. Örneğin, müşteri, sosyal veya yerel hesapla otomatik bir sigorta uygulamasında oturum açabilir, ancak aynı dizinde kayıtlı olan ev sigortası uygulamasına erişmeden önce telefon numarasını doğrulamalıdır.
- Genel olarak bir uygulamaya erişmek için Multi-Factor Authentication gerekmez, ancak bunun içindeki hassas bölümlere erişmesi gerekir. Örneğin, müşteri, sosyal veya yerel hesap içeren bir bankacılık uygulamasında oturum açabilir ve hesap bakiyesini denetleyebilir, ancak bir hat aktarımını denemeden önce telefon numarasını doğrulamanız gerekir.

## <a name="set-multi-factor-authentication"></a>Multi-Factor Authentication 'ı ayarlama

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Azure AD B2C kiracınızı içeren dizini seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C** seçin.
1. **Kullanıcı akışları ' nı** seçin.
1. MFA 'yı etkinleştirmek istediğiniz kullanıcı akışını seçin. Örneğin, *B2C_1_signinsignup*.
1. **Özellikler**’i seçin.
1. **Çok faktörlü kimlik doğrulaması** bölümünde, istenen **MFA yöntemini** seçin ve ardından **MFA zorlaması** altında **her zaman açık** veya **[koşullu](conditional-access-user-flow.md) (önerilen)** seçeneğini belirleyin. Koşullu [erişim ilkesi](conditional-access-identity-protection-setup.md) ilkesi oluşturun ve ilkenin uygulanmasını istediğiniz uygulamaları belirtin. 
1. Kaydet’i seçin. MFA artık bu kullanıcı akışı için etkinleştirilmiştir.

Deneyimi doğrulamak için **Kullanıcı akışını Çalıştır** ' i kullanabilirsiniz. Aşağıdaki senaryoyu onaylayın:

Multi-Factor Authentication adımı gerçekleşmeden önce kiracınızda bir müşteri hesabı oluşturulur. Adım sırasında müşterinin bir telefon numarası sağlaması ve doğrulanması istenir. Doğrulama başarılı olursa telefon numarası daha sonra kullanılmak üzere hesaba iliştirilir. Müşteri iptal ediyor veya devre dışı olsa bile, çok faktörlü kimlik doğrulaması etkin olan bir sonraki oturum açma sırasında müşterinin bir telefon numarasını doğrulaması istenebilir.



