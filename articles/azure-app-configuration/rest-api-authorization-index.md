---
title: Azure uygulama yapılandırma REST API-yetkilendirme
description: Azure Uygulama yapılandırması ile yetkilendirme için başvuru sayfaları REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 54f9cfab1f49837a3765c978de6deeb9e5e7d644
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424407"
---
# <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir çağıranın istek yaparken sahip olduğu izinleri belirlemede kullanılan yordamı ifade eder. Birden çok yetkilendirme modeli vardır. Bir istek için kullanılan yetkilendirme modeli, kullanılan [kimlik doğrulama](./rest-api-authentication-index.md) yöntemine bağlıdır. Yetkilendirme modelleri aşağıda listelenmiştir.

## <a name="hmac"></a>HMAC

HMAC kimlik doğrulamasıyla ilişkili [yetkilendirme modeli](./rest-api-authorization-hmac.md) modeli, izinleri salt okunurdur veya okuma-yazma olarak böler. Ayrıntılar için [HMAC yetkilendirme](./rest-api-authorization-hmac.md) sayfasına bakın.

## <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory (Azure AD) kimlik doğrulamasıyla ilişkili [yetkilendirme modeli](./rest-api-authorization-azure-ad.md) , izinleri denetlemek IÇIN Azure RBAC kullanır. Ayrıntılar için bkz. [Azure AD yetkilendirme](./rest-api-authorization-azure-ad.md) sayfası.
