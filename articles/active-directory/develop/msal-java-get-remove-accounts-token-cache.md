---
title: Hesapları belirteç önbelleğinden (MSAL4j) kaldırmak & alın | Azure
titleSuffix: Microsoft identity platform
description: Java için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak hesapları belirteç önbelleğinden nasıl görüntüleyip kaldıracağınızı öğrenin.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.openlocfilehash: 2b138678b186cc41b76254658ad604c2da2d76c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696206"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Java için MSAL'ı kullanarak hesapları belirteç önbelleğinden alın ve kaldırın

Java için MSAL varsayılan olarak bellek içi belirteç önbelleği sağlar. Bellek içi belirteç önbelleği uygulama örneğinin süresine dayanır.

## <a name="see-which-accounts-are-in-the-cache"></a>Önbellekte hangi hesapların olduğunu görme

Aşağıdaki örnekte gösterildiği gibi arayarak `PublicClientApplication.getAccounts()` önbellekte hangi hesapların olduğunu kontrol edebilirsiniz:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Hesapları önbellekten kaldırma

Bir hesabı önbellekten kaldırmak için kaldırılması gereken hesabı bulun `PublicClientApplicatoin.removeAccount()` ve aşağıdaki örnekte gösterildiği gibi arayın:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Java için MSAL kullanıyorsanız, [Java için MSAL'da Özel belirteç önbelleği serileştirme](msal-java-token-cache-serialization.md)si hakkında bilgi edinin.
