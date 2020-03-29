---
title: Özel belirteç önbellek serileştirme (MSAL4j)
titleSuffix: Microsoft identity platform
description: Java için MSAL için belirteç önbelleğini nasıl serihale edeceğinizi öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696172"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Java için MSAL'da özel belirteç önbelleği serileştirme

Uygulamanızın örnekleri arasındaki belirteç önbelleğini sürdürmek için serileştirmeyi özelleştirmeniz gerekir. Belirteç önbelleği serileştirmesinde yer alan Java sınıfları ve arabirimleri şunlardır:

- [ITokenCache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html): Güvenlik belirteci önbelleğini temsil eden arabirim.
- [ITokenCacheAccessAspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): Erişimden önce ve sonra kodun yürütülmesiişlemini temsil eden arayüz. Önbelleği serihale ve deserializing sorumlu mantıkile Önbellek ve @Override *afterCacheAccess* *önce* olur.
- [ITokenCacheContext](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessContext.html): Belirteç önbelleğine erişildiği bağlamı temsil eden arabirim. 

Aşağıda belirteç önbellek serileştirme / deserialization özel serileştirme saf bir uygulamadır. Bunu bir üretim ortamına kopyalayıp yapıştırmayın.

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

## <a name="learn-more"></a>Daha fazla bilgi edinin

Java [için MSAL'ı kullanarak hesapları belirteç önbelleğinden alın ve kaldırın](msal-java-get-remove-accounts-token-cache.md)hakkında bilgi edinin.
