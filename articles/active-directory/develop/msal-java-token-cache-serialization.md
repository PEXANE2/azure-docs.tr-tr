---
title: Özel belirteç önbelleği serileştirme (MSAL4j)
titleSuffix: Microsoft identity platform
description: Java için MSAL için belirteç önbelleğini serileştirme hakkında bilgi edinin
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev, devx-track-java
ms.openlocfilehash: e85cfb8c03111b889a9f95509f337d415c517163
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87312376"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Java için MSAL içinde özel belirteç önbelleği serileştirme

Uygulamanızın örnekleri arasında belirteç önbelleğini kalıcı hale getirmek için serileştirme özelleştirmeniz gerekir. Belirteç önbelleği serileştirmesine dahil olan Java sınıfları ve arabirimleri şunlardır:

- [Itokencache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html): güvenlik belirteci önbelleğini temsil eden arabirim.
- [Itokencacheaccessaspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): Access 'ten önce ve sonra kod yürütme işlemini temsil eden arabirim. @OverrideÖnbelleğin serileştirilmesinin ve serisini kaldırmada sorumlu mantığı ile *beforecacheaccess* ve *aftercacheaccess* ' i kullanırsınız.
- [Itokencachecontext](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessContext.html): belirteç önbelleğine erişildiği bağlamı temsil eden arabirim. 

Belirteç önbelleği serileştirme/seri hale getirme için özel serileştirme bir Naïve uygulamasıdır. Bunu kopyalayıp bir üretim ortamına yapıştırmayın.

```Java
static class TokenPersistence implements ITokenCacheAccessAspect {
String data;

TokenPersistence(String data) {
        this.data = data;
}

@Override
public void beforeCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        iTokenCacheAccessContext.tokenCache().deserialize(data);
}

@Override
public void afterCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        data = iTokenCacheAccessContext.tokenCache().serialize();
}
```

```Java
// Loads cache from file
String dataToInitCache = readResource(this.getClass(), "/cache_data/serialized_cache.json");

ITokenCacheAccessAspect persistenceAspect = new TokenPersistence(dataToInitCache);

// By setting *TokenPersistence* on the PublicClientApplication, MSAL will call *beforeCacheAccess()* before accessing the cache and *afterCacheAccess()* after accessing the cache. 
PublicClientApplication app = 
PublicClientApplication.builder("my_client_id").setTokenCacheAccessAspect(persistenceAspect).build();
```

## <a name="learn-more"></a>Daha fazlasını öğrenin

[Java IÇIN msal kullanarak belirteç önbelleğinden hesap alma ve kaldırma](msal-java-get-remove-accounts-token-cache.md)hakkında bilgi edinin.
