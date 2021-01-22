---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 04ad00ba45f3323a497e4acfe4d3f0099673310a
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701075"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Paylaşılan panolar satır içi içeriğe sahip markın kutucuklarına sahip olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F04c655fe-0ac7-48ae-9a32-3a2e208c7624) |Markaşağı kutucuklarında satır içi içeriğe sahip bir paylaşılan Pano oluşturulmasına izin vermeyin ve içeriğin çevrimiçi barındırılan bir markas dosyası olarak depolanması gerekir. Markaşağı kutucuğunda satır içi içerik kullanırsanız, içerik şifrelemesini yönetemezsiniz. Kendi depolama alanınızı yapılandırarak, kendi anahtarlarınızı şifreleyebilir, Çift şifreleyebilir ve hatta taşıyabilirsiniz. Bu ilkeyi etkinleştirmek, kullanıcıların 2020-09-01-Preview veya daha yüksek REST API paylaşılan panolar sürümünü kullanmasını kısıtlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Portal/SharedDashboardInlineContent_Deny.json) |
