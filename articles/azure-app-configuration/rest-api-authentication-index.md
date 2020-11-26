---
title: Azure uygulama yapılandırma REST API-kimlik doğrulama
description: Azure Uygulama yapılandırması 'nı kullanarak kimlik doğrulaması için başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 56416009395ebf8270ad0fa8d141277424dd6d9a
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183473"
---
# <a name="authentication"></a>Kimlik Doğrulaması

Tüm HTTP isteklerinin kimliği doğrulanmalıdır. Aşağıdaki kimlik doğrulama şemaları desteklenir.

## <a name="hmac"></a>HMAC

[HMAC kimlik doğrulaması](./rest-api-authentication-hmac.md) , istek yüklerini imzalamak için rastgele oluşturulmuş bir gizli anahtar kullanır. Bu kimlik doğrulama yöntemini kullanan isteklerin nasıl yetkilendirildiği hakkındaki ayrıntılar, [HMAC yetkilendirme](./rest-api-authorization-hmac.md) bölümünde bulunabilir.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory (Azure AD) kimlik doğrulaması](../active-directory/authentication/overview-authentication.md) , isteklerin kimliğini doğrulamak için Azure Active Directory alınan bir taşıyıcı belirtecini kullanır. Bu kimlik doğrulama yöntemini kullanan isteklerin [yetkilendirilmesine ilişkin ayrıntılar Azure AD yetkilendirme](./rest-api-authorization-azure-ad.md) bölümünde bulunabilir.