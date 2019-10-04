---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 1f4cdf45fb643499f7d6ab717c6f36395ad292c8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827673"
---
@No__t-0 (veya **nesne tanımlayıcı türü**), bir role verilen kimliğin türünü ifade eder. @No__t-0 ve `UserDefinedFunctionId` türlerinden ayrı olarak, nesne tanımlayıcı türleri Azure Active Directory nesnelerinin özelliklerine karşılık gelir.

Aşağıdaki tabloda, Azure dijital TWINS 'de desteklenen nesne tanımlayıcı türleri yer almaktadır:

| Tür | Açıklama |
| --- | --- |
| UserID | Bir kullanıcıya bir rol atar. |
| DeviceID | Cihaza bir rol atar. |
| Etki | Bir etki alanı adına bir rol atar. Belirtilen etki alanı adına sahip her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Değerine | Bir kiracıya bir rol atar. Belirtilen Azure AD kiracı KIMLIĞINE ait olan her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Serviceprincipalıd | Hizmet sorumlusu nesne KIMLIĞINE bir rol atar. |
| Userdefinedfunctionıd | Kullanıcı tanımlı bir işleve (UDF) bir rol atar. |