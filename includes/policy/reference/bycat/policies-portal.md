---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/24/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0df4035c75c124b33e299e1ccb2258ef41caebf6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105033654"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Paylaşılan panolar satır içi içeriğe sahip markın kutucuklarına sahip olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F04c655fe-0ac7-48ae-9a32-3a2e208c7624) |Markaşağı kutucuklarında satır içi içeriğe sahip bir paylaşılan Pano oluşturulmasına izin vermeyin ve içeriğin çevrimiçi barındırılan bir markas dosyası olarak depolanması gerekir. Markaşağı kutucuğunda satır içi içerik kullanırsanız, içerik şifrelemesini yönetemezsiniz. Kendi depolama alanınızı yapılandırarak, kendi anahtarlarınızı şifreleyebilir, Çift şifreleyebilir ve hatta taşıyabilirsiniz. Bu ilkeyi etkinleştirmek, kullanıcıların 2020-09-01-Preview veya daha yüksek REST API paylaşılan panolar sürümünü kullanmasını kısıtlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Portal/SharedDashboardInlineContent_Deny.json) |
