---
title: Web API 'Lerini çağıran Daemon uygulaması (uygulama kaydı)-Microsoft Identity platform
description: Web API 'Leri çağıran bir Daemon uygulaması oluşturmayı öğrenin-uygulama kaydı
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: fddd95564e3683aaee067f0442573c41e93376dd
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71264386"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Web API 'Lerini çağıran Daemon uygulaması-uygulama kaydı

Bir Daemon uygulaması için, uygulamayı kaydederken bilmeniz gerekenler aşağıda verilmiştir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Bu arka plan programı uygulamalarının yalnızca Azure AD kiracılarında anlam verdiği için, uygulamayı oluştururken şunları seçmeniz gerekir:

- **yalnızca bu kuruluş dizinindeki hesaplar**. Daemon uygulamalarının genellikle iş kolu (LOB) geliştiricileri tarafından yazıldığı, bu seçim en yaygın durumdur.
- ya da **herhangi bir kuruluş dizinindeki hesaplar**. Müşterileriniz için bir yardımcı program aracı sağlayan bir ISV 'niz varsa, bu seçimi yaparsınız. Müşterinin kiracılar yöneticilerinin onaylaması gerekir.

## <a name="authentication---no-reply-uri-needed"></a>Kimlik doğrulama-yanıt URI 'SI gerekli değil

Gizli istemci uygulamanızın **yalnızca** istemci kimlik bilgileri akışını kullanması durumunda, yanıt URL 'sinin kayıtlı olması gerekmez. Uygulama yapılandırması/oluşturma için gerekli değildir. İstemci kimlik bilgileri akışı bunu kullanmaz.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>API Izinleri-uygulama izinleri ve yönetici onayı

Daemon uygulaması, API 'lere yalnızca uygulama izinleri isteyebilir (temsilci izinleri yoktur). Uygulama kaydı için **API izin** sayfasında, **izin Ekle** ' yi seçtikten sonra API ailesini seçtikten sonra **Uygulama izinleri**' ni seçin ve ardından izinlerinizi seçin

![Uygulama izinleri ve yönetici onayı](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> Çağırmak istediğiniz Web API 'sinin, temsilci izinleri değil **Uygulama izinleri (uygulama rolleri)** tanımlamasına ihtiyacı vardır. Böyle bir API 'yi kullanıma sunma hakkında daha fazla bilgi için [bkz. korumalı Web API 'si: Uygulama kaydı-Web API 'niz bir Daemon uygulaması tarafından çağrıldığında](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-protected-web-api-app-registration#if-your-web-api-is-called-by-a-daemon-app)

Daemon uygulamalarının, Web API 'sini çağıran uygulama için bir kiracı yöneticisi ön onayını olması gerekir. Bu izin, kiracı yöneticisinin  ***kuruluşa* yönetici izni ver** ' i seçen aynı **API izin** sayfasında sağlanır

Çok kiracılı bir uygulama oluşturmak için bir ISV iseniz, [çok kiracılı Daemon uygulamaları paragrafına dağıtım durumunu](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps) denetlemek isteyeceksiniz.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daemon uygulaması-uygulama kodu yapılandırması](./scenario-daemon-app-configuration.md)
