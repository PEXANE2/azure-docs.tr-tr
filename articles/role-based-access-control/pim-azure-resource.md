---
title: Azure AD ve PIM ile Azure kaynaklarına erişimi yönetme
description: Azure Etkin Dizin Ayrıcalıklı Kimlik Yönetimi (PIM) ve rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 53c6375ce04425eae27f4c935e613cc27d058dc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77138047"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Azure AD Ayrıcalıklı Kimlik Yönetimi ile Azure kaynaklarına erişimi yönetme

Ayrıcalıklı hesapları kötü amaçlı siber saldırılara karşı korumak için, ayrıcalıkların maruz kalma süresini azaltmak ve raporlar ve uyarılar aracılığıyla görünürlüğünüzü artırmak için Azure Active Directory Ayrıcalıklı Kimlik Yönetimi'ni (PIM) kullanabilirsiniz. PIM bunu, kullanıcıları yalnızca ayrıcalıklarını "tam zamanında" (JIT) almakla sınırlandırarak veya ayrıcalıkların otomatik olarak iptal edildiği kısaltılmış bir süre için ayrıcalıklar atayarak yapar. 

Artık Azure kaynaklarına erişimi yönetmek, denetlemek ve izlemek için PIM'yi Azure rol tabanlı erişim denetimine (RBAC) sahip kullanabilirsiniz. PIM, size yardımcı olmak için yerleşik ve özel rollerin üyeliğini yönetebilir: 

- Azure kaynaklarına isteğe bağlı , "tam zamanında" erişimi etkinleştirme
- Atanan kullanıcılar ve gruplar için kaynak erişiminin otomatik olarak sona ermesi
- Hızlı görevler veya çağrı üzerine zamanlamalar için Azure kaynaklarına geçici erişim atama
- Yeni kullanıcılara veya gruplara kaynak erişimi atandığında ve uygun atamaları etkinleştirdiklerinde uyarılar alın

Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](../active-directory/privileged-identity-management/pim-configure.md)