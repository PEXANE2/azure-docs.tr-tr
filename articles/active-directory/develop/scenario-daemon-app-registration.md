---
title: Web API'lerini arayan daemon uygulamalarını kaydedin - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir daemon uygulaması oluşturmayı öğrenin - uygulama kaydı
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 922a484d111746e5073c08a64d7c92e2b6b4a7c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773380"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Web API'lerini çağıran Daemon uygulaması - uygulama kaydı

Bir daemon uygulaması için, uygulamayı kaydederken bilmeniz gerekenler şunlardır.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Daemon uygulamaları yalnızca Azure AD kiracılarında anlamlıdır. Bu nedenle uygulamayı oluştururken, aşağıdaki seçeneklerden birini seçmeniz gerekir:

- **Yalnızca bu kuruluş dizinindeki hesaplar.** Daemon uygulamaları genellikle iş hattı (LOB) geliştiricileri tarafından yazıldığından, bu seçim en yaygın olanıdır.
- **Herhangi bir kuruluş dizinindeki hesaplar.** Müşterilerinize yardımcı program sağlayan bir ISV iseniz bu seçimi yaparsınız. Bunu onaylamak için müşterilerinizin kiracı yöneticilerine ihtiyacınız vardır.

## <a name="authentication---no-reply-uri-needed"></a>Kimlik doğrulama - hiçbir cevap URI gerekli

Gizli istemci uygulamanızın *yalnızca* istemci kimlik bilgilerini kullandığı durumlarda, uri yanıtının kaydedilmesi gerekmez. Uygulama yapılandırması veya inşaat için gerekli değildir. İstemci kimlik bilgileri akışı bunu kullanmaz.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>API izinleri - uygulama izinleri ve yönetici onayı

Daemon uygulaması yalnızca API'lere uygulama izinleri isteyebilir (devredilen izinler değil). Uygulama kaydı için **API izinleri** sayfasında, izin **ekle'yi** seçtikten ve API ailesini seçtikten sonra, **Uygulama izinlerini**seçin ve ardından izinlerinizi seçin.

![Uygulama izinleri ve yönetici onayı](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> Aramak istediğiniz web API'sinin, yetkin *izinleri değil, uygulama izinlerini (uygulama rolleri)* tanımlaması gerekir. Böyle bir API'nin nasıl ortaya çıkarılacağına ilişkin ayrıntılar için [Korumalı web API: Uygulama kaydı - web API'nız bir daemon uygulaması tarafından çağrıldığında.](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app)

Daemon uygulamaları, kiracı yöneticinin web API'yi çağıran uygulamaya önceden onay göstermesini gerektirir. Kiracı yöneticiler, ** *kuruluşumuza* Grant admin onayı** seçerek aynı **API izin** sayfasında bu onayı sağlar

Çok kiracılı bir uygulama oluşturan bir ISV iseniz, dağıtım bölümünü okumalısınız [- çok kiracılı daemon uygulamaları.](scenario-daemon-production.md#deployment---multitenant-daemon-apps)

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daemon uygulaması - uygulama kodu yapılandırması](./scenario-daemon-app-configuration.md)
