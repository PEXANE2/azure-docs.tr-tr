---
title: Web API'lerini arayan daemon uygulamalarını kaydedin - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir daemon uygulaması oluşturmayı öğrenin - uygulama kaydı
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 508101ad615dd96559b1c68a61be7c08772545db
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885489"
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
