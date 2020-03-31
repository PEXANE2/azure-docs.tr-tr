---
title: OpenID Connect ile kaydolma ve oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki herhangi bir OpenID Connect kimlik sağlayıcısıyla (IdP) kaydolma ve oturum açma'yı ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c72abf79f0a420309ebe229673be9439fd99b74c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188265"
---
# <a name="set-up-sign-up-and-sign-in-with-openid-connect-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak OpenID Connect ile kaydolma ve oturum açma

[OpenID Connect,](openid-connect.md) OAuth 2.0'ın üzerine inşa edilmiş ve güvenli kullanıcı oturum açmak için kullanılabilecek bir kimlik doğrulama protokolüdür. Bu protokolü kullanan kimlik sağlayıcılarının çoğu Azure AD B2C'de desteklenir. Bu makalede, kullanıcı akışlarınıza özel OpenID Connect kimlik sağlayıcıları nasıl ekleyebileceğiniz açıklanmaktadır.

## <a name="add-the-identity-provider"></a>Kimlik sağlayıcısını ekleme

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Üst menüdeki **Dizin + abonelik filtresini** tıklatarak ve kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. **Kimlik sağlayıcılarını**seçin ve ardından **Yeni OpenID Bağlantı sağlayıcısını**seçin.

## <a name="configure-the-identity-provider"></a>Kimlik sağlayıcısını yapılandırma

Her OpenID Connect kimlik sağlayıcısı, oturum açmak için gereken bilgilerin çoğunu içeren bir meta veri belgesini açıklar. Bu, kullanılacak URL'ler ve hizmetin genel imzalama anahtarlarının konumu gibi bilgileri içerir. OpenID Connect meta veri belgesi her zaman `.well-known\openid-configuration`. Eklemek istediğiniz OpenID Connect kimlik sağlayıcısı için meta veri URL'sini girin.

## <a name="client-id-and-secret"></a>İstemci kimliği ve gizli

Kullanıcıların oturum açmasına izin vermek için, kimlik sağlayıcısı geliştiricilerin bir uygulamayı kendi hizmetine kaydetmesini gerektirir. Bu uygulama istemci **kimliği** ve istemci **gizli**olarak adlandırılır bir kimlik vardır. Bu değerleri kimlik sağlayıcısından kopyalayın ve ilgili alanlara girin.

> [!NOTE]
> İstemci sırrı isteğe bağlıdır. Ancak, belirteç için kodu değiştirmek için gizli kullanan [yetkilendirme kodu akışını](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)kullanmak istiyorsanız, bir istemci gizli girmeniz gerekir.

## <a name="scope"></a>Kapsam

Kapsam, özel kimlik sağlayıcınızdan toplamak istediğiniz bilgileri ve izinleri tanımlar. OpenID Connect istekleri, `openid` kimlik sağlayıcısından kimlik belirteci almak için kapsam değerini içermelidir. Kimlik belirteci olmadan, kullanıcılar özel kimlik sağlayıcısını kullanarak Azure AD B2C'de oturum açamaz. Diğer kapsamlar boşluk la ayrılabilir. Başka kapsamların neler olabileceğini görmek için özel kimlik sağlayıcısının belgelerine bakın.

## <a name="response-type"></a>Yanıt türü

Yanıt türü, özel kimlik sağlayıcısının ilk çağrısında `authorization_endpoint` ne tür bilgilerin geri gönderildiğini açıklar. Aşağıdaki yanıt türleri kullanılabilir:

* `code`: Yetkilendirme [kodu akışına](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth)göre, bir kod Azure AD B2C'ye geri döndürülür. Azure AD B2C belirteç için kod değişimi aramak `token_endpoint` için devam eder.
* `id_token`: Bir kimlik belirteci, özel kimlik sağlayıcısından Azure AD B2C'ye geri döndürülür.

## <a name="response-mode"></a>Yanıt modu

Yanıt modu, verileri özel kimlik sağlayıcısından Azure AD B2C'ye geri göndermek için kullanılması gereken yöntemi tanımlar. Aşağıdaki yanıt modları kullanılabilir:

* `form_post`: Bu yanıt modu en iyi güvenlik için önerilir. Yanıt, `POST` `application/x-www-form-urlencoded` http yöntemi ile, kod veya belirteç biçimi kullanılarak gövdede kodlanmış olan iletilir.
* `query`: Kod veya belirteç sorgu parametresi olarak döndürülür.

## <a name="domain-hint"></a>Etki alanı ipucu

Etki alanı ipucu, kullanıcının kullanılabilir kimlik sağlayıcıları listesi arasında seçim yapması yerine, doğrudan belirtilen kimlik sağlayıcısının oturum açma sayfasına atlamak için kullanılabilir. Bu tür davranışlara izin vermek için etki alanı ipucu için bir değer girin. Özel kimlik sağlayıcısına atlamak için, oturum `domain_hint=<domain hint value>` açma için Azure AD B2C'yi ararken parametreyi isteğinizin sonuna kadar ekleştirin.

## <a name="claims-mapping"></a>İddiaların eşleneme

Özel kimlik sağlayıcısı, Azure AD B2C'ye bir kimlik belirteci gönderdikten sonra, Azure AD B2C'nin talepleri alınan belirteciile Azure AD B2C'nin tanıdığı ve kullandığı iddialara kadar eşlenebilmesi gerekir. Aşağıdaki eşlemelerin her biri için, kimlik sağlayıcısının belirteçlerinde iade edilen talepleri anlamak için özel kimlik sağlayıcısının belgelerine bakın:

* **Kullanıcı Kimliği**: Oturum açmış kullanıcı için *benzersiz tanımlayıcıyı* sağlayan talebi girin.
* **Ekran Adı**: Kullanıcı için *görüntü adı* veya tam *adı* sağlayan talebi girin.
* **Verilen Ad**: Kullanıcının *adını* veren iddiayı girin.
* **Soyadı**: Kullanıcının *soyadını* veren iddiayı girin.
* **E-posta**: Kullanıcının *e-posta adresini* sağlayan talebi girin.
