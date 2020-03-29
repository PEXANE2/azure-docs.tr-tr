---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: cb43c8b8c952d8db6cf450a7015c22c85e7fe4b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76268200"
---
`objectIdType` (veya **nesne tanımlayıcı türü)** bir role verilen kimlik türünü ifade eder. Nesne tanımlayıcı `DeviceId` `UserDefinedFunctionId` türleri ve türleri dışında Azure Active Directory nesnelerinin özelliklerine karşılık gelir.

Aşağıdaki tablo, Azure Dijital İkizler'de desteklenen nesne tanımlayıcı türlerini içerir:

| Tür | Açıklama |
| --- | --- |
| UserId | Bir kullanıcıya bir rol atar. |
| DeviceId | Bir aygıta bir rol atar. |
| DomainName | Etki alanı adına bir rol atar. Belirtilen etki alanı adı olan her kullanıcı, ilgili rolün erişim haklarına sahiptir. |
| TenantId | Bir kiracıya bir rol atar. Belirtilen Azure AD kiracı kimliğine ait her kullanıcı, ilgili rolün erişim haklarına sahiptir. |
| HizmetMüdürü | Bir hizmet asıl nesne kimliğine bir rol atar. |
| UserDefinedFunctionId | Kullanıcı tanımlı bir işleve (UDF) bir rol atar. |