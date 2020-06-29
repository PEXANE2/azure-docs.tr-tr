---
title: Belirteç önbelleğinden hesapları al & Kaldır (MSAL4j) | Mavisi
titleSuffix: Microsoft identity platform
description: Java için Microsoft kimlik doğrulama kitaplığı 'nı kullanarak, belirteç önbelleğinde hesapları görüntülemeyi ve kaldırmayı öğrenin.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.openlocfilehash: 150708e7d54e456e1a4e35cfe381e5de5d0f1e8c
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85479360"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Java için MSAL kullanarak belirteç önbelleğinden hesap alma ve kaldırma

Java için MSAL, varsayılan olarak bellek içi belirteç önbelleği sağlar. Bellek içi belirteç önbelleği, uygulama örneğinin süresini sürer.

## <a name="see-which-accounts-are-in-the-cache"></a>Hangi hesapların önbellekte olduğunu görün

Aşağıdaki örnekte gösterildiği gibi çağırarak hangi hesapların önbellekte olduğunu kontrol edebilirsiniz `PublicClientApplication.getAccounts()` :

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Hesapları önbellekten kaldır

Bir hesabı önbellekten kaldırmak için, kaldırılması gereken hesabı bulun ve `PublicClientApplicatoin.removeAccount()` Aşağıdaki örnekte gösterildiği gibi çağırın:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Daha fazla bilgi edinin

Java için MSAL kullanıyorsanız, [Java IÇIN msal Içindeki özel belirteç önbelleği serileştirme](msal-java-token-cache-serialization.md)hakkında bilgi edinin.
