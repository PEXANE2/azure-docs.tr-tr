---
title: Web uygulaması ekleme-Azure Active Directory B2C | Microsoft Docs
description: Active Directory B2C kiracınıza bir Web uygulaması eklemeyi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 50a4ead58cc70524ec464e52ce546b36f9685df5
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064545"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracınıza bir Web API uygulaması ekleme

 Web API kaynaklarını kiracınıza kaydederek erişim belirtecini sunan istemci uygulamalarına yönelik istekleri kabul edebilir ve yanıtlayabilir. Bu makalede, uygulamanın Azure Active Directory B2C (Azure AD B2C) nasıl kaydedileceği gösterilmektedir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
7. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Üretim uygulamanızda, yanıt URL 'sini gibi `https://localhost:44332`bir değere ayarlayabilirsiniz. Sınama amacıyla, yanıt URL 'sini olarak `https://jwt.ms`ayarlayın.
8. **Uygulama kimliği URI 'si**için, Web API 'niz için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin: `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**'a tıklayın.
10. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

1. **Uygulamalar**' ı seçin ve ardından *webapi1*' ı seçin.
2. **Yayımlanan kapsamları**seçin.
3. **Kapsam**için girin `Read`ve açıklama için girin. `Read access to the application`
4. **Kapsam**için girin `Write`ve açıklama için girin. `Write access to the application`
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar, Web API 'sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Örneğin, [öğreticide: Bir uygulamayı Azure Active Directory B2C](tutorial-register-applications.md)kaydetme, *WebApp1*adlı Azure AD B2C bir Web uygulaması oluşturulur. Web API 'sini çağırmak için bu uygulamayı kullanabilirsiniz.

1. **Uygulamalar**' ı seçin ve ardından Web uygulamanızı seçin.
2. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
3. **API Seç** açılan menüsünde *webapi1*' yi seçin.
4. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız **okuma** ve **yazma** kapsamlarını seçin.
5. **Tamam**'ı tıklatın.

Uygulamanız, korumalı Web API 'sini çağırmak için kaydedilir. Kullanıcı, uygulamayı kullanmak için Azure AD B2C kimliğini doğrular. Uygulama, korumalı Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.
