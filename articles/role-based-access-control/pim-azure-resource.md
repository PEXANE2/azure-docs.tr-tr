---
title: Azure AD ve PıM ile Azure kaynaklarına erişimi yönetme
description: Azure Active Directory Privileged Identity Management (PıM) ve rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetme hakkında bilgi edinin.
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
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77138047"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management ile Azure kaynaklarına erişimi yönetme

Ayrıcalıklı hesapları kötü niyetli Cyber saldırılarına karşı korumak için Azure Active Directory Privileged Identity Management (PıM) kullanarak ayrıcalıkların etkilenme süresini azaltır ve raporlar ve uyarılar aracılığıyla kullanım için görünürlüğünüzü artırabilirsiniz. PıM, kullanıcıları yalnızca kendi ayrıcalıklarını "zamanında" (JıT) olarak sınırlayarak veya ayrıcalık otomatik olarak iptal edildikten sonra kısaltılmış bir süre için ayrıcalıklar atayarak bunu yapar. 

Artık Azure kaynaklarına erişimi yönetmek, denetlemek ve izlemek için Azure rol tabanlı erişim denetimi (RBAC) ile PıM 'yi kullanabilirsiniz. PıM, size yardımcı olmak için yerleşik ve özel rollerin üyeliğini yönetebilir: 

- İsteğe bağlı, "tam zamanında" Azure kaynaklarına erişimi etkinleştirme
- Atanan kullanıcılar ve gruplar için kaynak erişiminin otomatik olarak dolmasına
- Hızlı Görevler veya çağrı zamanlamaları için Azure kaynaklarına geçici erişim atama
- Yeni kullanıcılara veya gruplara kaynak erişimi atandığında ve uygun atamaları etkinleştirdiklerinde uyarı alın

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md).