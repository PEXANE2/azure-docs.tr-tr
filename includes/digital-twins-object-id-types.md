---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 11/20/2019
ms.custom: include file
ms.openlocfilehash: e46041a33c12b3fcb40e5a04de11108471e59855
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74307213"
---
`objectIdType` (veya **nesne tanımlayıcı türü**), bir role verilen kimliğin türünü ifade eder. `DeviceId` ve `UserDefinedFunctionId` türlerinden ayrı olarak, nesne tanımlayıcı türleri Azure Active Directory nesnelerinin özelliklerine karşılık gelir.

Aşağıdaki tabloda, Azure dijital TWINS 'de desteklenen nesne tanımlayıcı türleri yer almaktadır:

| Tür | Açıklama |
| --- | --- |
| UserID | Bir kullanıcıya bir rol atar. |
| DeviceId | Cihaza bir rol atar. |
| Etki | Bir etki alanı adına bir rol atar. Belirtilen etki alanı adına sahip her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Değerine | Bir kiracıya bir rol atar. Belirtilen Azure AD kiracı KIMLIĞINE ait olan her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Serviceprincipalıd | Hizmet sorumlusu nesne KIMLIĞINE bir rol atar. |
| Userdefinedfunctionıd | Kullanıcı tanımlı bir işleve (UDF) bir rol atar. |