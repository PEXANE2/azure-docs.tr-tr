---
title: Web API uygulaması ekleme - Azure Active Directory B2C | Microsoft Dokümanlar
description: Active Directory B2C kiracınıza nasıl bir web API uygulaması ekleyeceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: e6dbf3d6fd5a43ab2d075c193c5bc589dc3566a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190186"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracınıza bir web API uygulaması ekleme

 Web API kaynaklarını kiracınıza kaydedin, böylece erişim jetonu sunan istemci uygulamalarının isteklerini kabul edebilir ve yanıtlayabilir. Bu makalede, Azure Etkin Dizin B2C 'de (Azure AD B2C) bir web API'sini nasıl kaydedebilirsiniz.

Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web app/ web API ekle** ve **örtük akışa izin vermek**için **Evet'i**seçin.
7. **YanıtURL'si için,** Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Üretim uygulamanızda, yanıt URL'sini `https://localhost:44332`. Sınama amacıyla yanıt URL'sini `https://jwt.ms`.
8. **App ID URI**için, web API'nız için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur'u**tıklatın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama kimliğini kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *webapi1*.
1. **Uri'yi Yeniden Yönlendirme**altında **Web'i**seçin ve ardından Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Bir üretim uygulamasında, uri'yi yeniden yönlendirmeyi `https://localhost:5000`bir bitiş noktası olarak ayarlayabilirsiniz. Geliştirme veya sınama sırasında, `https://jwt.ms`bir belirteci (belirteç içeriği tarayıcınızdan asla çıkmadan) deşifre edilmiş içeriğini görüntüleyen Microsoft'a ait bir web uygulaması olarak ayarlayabilirsiniz. Kayıtlı uygulamalarınızda istediğiniz zaman yönlendirme URI'leri ekleyebilir ve değiştirebilirsiniz.
1. **Kaydol**’u seçin.
1. Web API'nizin kodunda kullanılmak üzere **Uygulama (istemci) kimliğini** kaydedin.

Dolaylı hibe akışını (örneğin JavaScript tabanlı tek sayfalı bir uygulama (SPA) uygulayan bir uygulamanız varsa, aşağıdaki adımları izleyerek akışı etkinleştirebilirsiniz:

1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Örtülü hibe**altında, hem **Erişim belirteçleri** hem de **kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet'i**seçin.

* * *

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için, UYGULAMA İzNİ'ni API'ye vermeniz gerekir. Örneğin, [Öğretici:Azure Active Directory B2C'ye bir uygulama kaydedin,](tutorial-register-applications.md) *webapp1* adlı bir web uygulaması Azure AD B2C'de kayıtlıdır. Bu uygulamayı web API'sini aramak için kullanabilirsiniz.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Uygulamanız korumalı web API'yi aramak için kayıtlıdır. Bir kullanıcı uygulamayı kullanmak için Azure AD B2C ile kimlik doğrulaması yaptı. Uygulama, korumalı web API'sine erişmek için Azure AD B2C'den yetkilendirme hibesi alır.
