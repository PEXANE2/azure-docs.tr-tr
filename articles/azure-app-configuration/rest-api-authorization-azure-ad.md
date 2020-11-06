---
title: Azure uygulama yapılandırma REST API-Azure Active Directory yetkilendirme
description: Azure Uygulama yapılandırmasında yetkilendirme için Azure Active Directory REST API kullanarak kullanın
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: bebab7c06062726f7b5c7868f984cadda3b4c98e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424412"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Azure Active Directory yetkilendirmesi-REST API başvurusu

Azure Active Directory (Azure AD) kimlik doğrulaması kullanıldığında, yetkilendirme, Azure rol tabanlı Access Control (RBAC) tarafından işlenir. Azure RBAC, kaynaklara erişim izni vermek için kullanıcıların rollere atanmasını gerektirir. Her rol, role atanan kullanıcıların gerçekleştirebilmesini sağlayacak bir dizi eylem içerir.

## <a name="roles"></a>Roller

Aşağıdaki roller, varsayılan olarak Azure aboneliklerinde kullanılabilen yerleşik rollerdir:

- **Azure Uygulama yapılandırması veri sahibi** : Bu rol, tüm işlemlere tam erişim sağlar.
- **Azure uygulama yapılandırma veri okuyucusu** : Bu rol, okuma işlemlerine izin vermez.

## <a name="actions"></a>Eylemler

Roller, bu role atanan kullanıcıların gerçekleştirebileceği eylemlerin bir listesini içerir. Azure Uygulama yapılandırması aşağıdaki eylemleri destekler:

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`: Bu eylem, uygulama yapılandırma anahtar-değer kaynakları için/KV ve/Labels. okuma erişimine izin verir.
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`: Bu eylem, uygulama yapılandırma anahtar-değer kaynaklarına yazma erişimine izin verir.
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`: Bu eylem, uygulama yapılandırma anahtar-değer kaynaklarının silinmesine izin verir. Bir kaynağın silinmesi, silinen anahtar değerini geri döndürdüğünü unutmayın.

## <a name="errors"></a>Hatalar

```http
HTTP/1.1 403 Forbidden
```

**Neden:** İsteği yapan asıl işlem, istenen işlemi gerçekleştirmek için gerekli izinlere sahip değil.
**Çözüm:** İstenen işlemi gerçekleştirmek için gereken rolü, isteği yapan sorumluya atayın.

## <a name="managing-role-assignments"></a>Rol atamalarını yönetme

Rol atamalarının yönetilmesi, tüm Azure hizmetlerinde standart olan [Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview) yordamları kullanılarak yapılır. Bunu Azure CLı, PowerShell, Azure portal ve daha fazlası aracılığıyla yapmak mümkündür. Rol atamalarının nasıl yapılacağı hakkında resmi belgeler [burada](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)bulunabilir.
