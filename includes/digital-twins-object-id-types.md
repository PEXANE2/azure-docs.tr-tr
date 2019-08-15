---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 94baeb3d459b700cc95d88fb82f995957640aab6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012315"
---
( `objectIdType` Veya **nesne tanımlayıcı türü**), bir role verilen kimliğin türünü ifade eder. `DeviceId` Ve`UserDefinedFunctionId` türleri dışında, nesne tanımlayıcı türleri Azure Active Directory nesnelerinin özelliklerine karşılık gelir.

Aşağıdaki tabloda, Azure dijital TWINS 'de desteklenen nesne tanımlayıcı türleri yer almaktadır:

| Type | Açıklama |
| --- | --- |
| UserId | Bir kullanıcıya bir rol atar. |
| DeviceId | Cihaza bir rol atar. |
| Etki | Bir etki alanı adına bir rol atar. Belirtilen etki alanı adına sahip her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| TenantId | Bir kiracıya bir rol atar. Belirtilen Azure AD kiracı KIMLIĞINE ait olan her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Serviceprincipalıd | Hizmet sorumlusu nesne KIMLIĞINE bir rol atar. |
| Userdefinedfunctionıd | Kullanıcı tanımlı bir işleve (UDF) bir rol atar. |