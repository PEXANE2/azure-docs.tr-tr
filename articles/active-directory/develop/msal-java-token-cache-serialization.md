---
title: Java için MSAL içinde özel belirteç önbelleği serileştirme
titleSuffix: Microsoft identity platform
description: Java için MSAL için belirteç önbelleğini serileştirme hakkında bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0bd8cb07d30aa5223cf3d78d636f0d593fefbecf
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905504"
---
# <a name="custom-token-cache-serialization-in-msal-for-java-msal4j"></a>Java için MSAL içinde özel belirteç önbelleği serileştirme (MSAL4J)

Kalıcı bir belirteç önbelleği uygulamasına sahip olmak için Serileştirmeyi özelleştirmeniz gerekir. Belirteç önbelleği serileştirmesine dahil olan Java sınıfları ve arabirimleri şunlardır:

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

## <a name="learn-more"></a>Daha fazla bilgi edinin

[Java IÇIN msal kullanarak belirteç önbelleğinden hesap alma ve kaldırma](msal-java-get-remove-accounts-token-cache.md)hakkında bilgi edinin.
