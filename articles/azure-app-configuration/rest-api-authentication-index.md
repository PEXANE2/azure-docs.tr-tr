---
title: Azure uygulama yapılandırma REST API-kimlik doğrulama
description: Azure Uygulama yapılandırması 'nı kullanarak kimlik doğrulaması için başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 21a43a005b78c8916d06e97ca9d2ba21d5a585a3
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424530"
---
# <a name="authentication"></a>Kimlik Doğrulaması

Tüm HTTP isteklerinin kimliği doğrulanmalıdır. Aşağıdaki kimlik doğrulama şemaları desteklenir.

## <a name="hmac"></a>HMAC

[HMAC kimlik doğrulaması](./rest-api-authentication-hmac.md) , istek yüklerini imzalamak için rastgele oluşturulmuş bir gizli anahtar kullanır. Bu kimlik doğrulama yöntemini kullanan isteklerin nasıl yetkilendirildiği hakkındaki ayrıntılar, [HMAC yetkilendirme](./rest-api-authorization-hmac.md) bölümünde bulunabilir.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory (Azure AD) kimlik doğrulaması](/azure/active-directory/authentication/overview-authentication) , isteklerin kimliğini doğrulamak için Azure Active Directory alınan bir taşıyıcı belirtecini kullanır. Bu kimlik doğrulama yöntemini kullanan isteklerin [yetkilendirilmesine ilişkin ayrıntılar Azure AD yetkilendirme](./rest-api-authorization-azure-ad.md) bölümünde bulunabilir.
