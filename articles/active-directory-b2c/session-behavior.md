---
title: Oturum davranışını Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C 'de oturum davranışını yapılandırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ed10a9258590f8f9037a574bd0322a82dd309a5b
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85385255"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'de oturum davranışını yapılandırma

Azure Active Directory B2C (Azure AD B2C) içindeki [Çoklu oturum açma (SSO) oturum](session-overview.md) yönetimi, kullanıcının kimliği doğrulandıktan sonra bir kullanıcının Kullanıcı ile etkileşimini denetlemesine olanak sağlar. Örneğin, yönetici, kimlik sağlayıcılarının seçiminin görüntülenip görüntülenmediğini veya hesap ayrıntılarının yeniden girilmesi gerekip gerekmediğini denetleyebilir. Bu makalede Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

## <a name="session-behavior-properties"></a>Oturum davranışı özellikleri

Web uygulaması oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumu ömrü (dakika)** -başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan Azure AD B2C's oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - En düşük (kapsamlı) = 15 dakika.
    - Maksimum (dahil) = 1440 dakika.
- **Web uygulaması oturumu zaman aşımı** - [oturum süre sonu türü](session-overview.md#session-expiry-type), *yuvarlama*veya *mutlak*. 
- **Çoklu oturum açma yapılandırması** -çoklu oturum açma (SSO) davranışının, Azure AD B2C kiracınızdaki birden çok uygulama ve Kullanıcı akışı üzerindeki [oturum kapsamı](session-overview.md#session-scope) . 


## <a name="configure-the-properties"></a>Özellikleri yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip Azure AD B2C kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikler**’i seçin.
7. **Web uygulaması oturumu yaşam süresi (dakika)**, **Web uygulaması oturumu zaman aşımı**, **Çoklu oturum açma yapılandırması**yapılandırın ve gerektiğinde **oturum kapatma isteklerinde kimlik belirteci gerektir** .

    ![Azure portal oturum davranışı özelliği ayarları](./media/session-behavior/session-behavior.png)

8. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C oturum](session-overview.md)hakkında daha fazla bilgi edinin.