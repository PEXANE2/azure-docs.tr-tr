---
title: Özel belirteç önbelleği serileştirme (MSAL4j)
titleSuffix: Microsoft identity platform
description: Java için MSAL için belirteç önbelleğini serileştirme hakkında bilgi edinin
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: bcb34d83365112b97769186ad74dfd762b05c2e8
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696172"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Java için MSAL içinde özel belirteç önbelleği serileştirme

Uygulamanızın örnekleri arasında belirteç önbelleğini kalıcı hale getirmek için serileştirme özelleştirmeniz gerekir. Belirteç önbelleği serileştirmesine dahil olan Java sınıfları ve arabirimleri şunlardır:

- [Itokencache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html): güvenlik belirteci önbelleğini temsil eden arabirim.
- [Itokencacheaccessaspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): Access 'ten önce ve sonra kod yürütme işlemini temsil eden arabirim. Bu, önbelleğin serileştirilmesinin ve serisini kaldırmada sorumlu mantığa karşı *beforecacheaccess* ve *aftercacheaccess* @Override.
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

## <a name="learn-more"></a>Daha fazla bilgi

[Java IÇIN msal kullanarak belirteç önbelleğinden hesap alma ve kaldırma](msal-java-get-remove-accounts-token-cache.md)hakkında bilgi edinin.
