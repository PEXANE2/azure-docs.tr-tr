---
title: Belirteç önbelleğinden hesapları al & Kaldır (MSAL4j) | Mavisi
titleSuffix: Microsoft identity platform
description: Java için Microsoft kimlik doğrulama kitaplığı 'nı kullanarak, belirteç önbelleğinde hesapları görüntülemeyi ve kaldırmayı öğrenin.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2f800d8d8665667c51ee57e3fe1c556c3a2d8eb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75424336"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Java için MSAL kullanarak belirteç önbelleğinden hesap alma ve kaldırma

Java için MSAL, varsayılan olarak bellek içi belirteç önbelleği sağlar. Bellek içi belirteç önbelleği, uygulama örneğinin süresini sürer.

## <a name="see-which-accounts-are-in-the-cache"></a>Hangi hesapların önbellekte olduğunu görün

Aşağıdaki örnekte gösterildiği gibi `PublicClientApplication.getAccounts()` çağırarak hangi hesapların önbellekte olduğunu kontrol edebilirsiniz:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Hesapları önbellekten kaldır

Bir hesabı önbellekten kaldırmak için, kaldırılması gereken hesabı bulun ve aşağıdaki örnekte gösterildiği gibi `PublicClientApplicatoin.removeAccount()` çağırın:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Daha fazla bilgi

Java için MSAL kullanıyorsanız, [Java IÇIN msal Içindeki özel belirteç önbelleği serileştirme](msal-java-token-cache-serialization.md)hakkında bilgi edinin.
