---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e28dc20fe5e789bbaa5aad9402d37f039eabe6e5
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107504224"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Paylaşılan panolar satır içi içeriğe sahip markın kutucuklarına sahip olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F04c655fe-0ac7-48ae-9a32-3a2e208c7624) |Markaşağı kutucuklarında satır içi içeriğe sahip bir paylaşılan Pano oluşturulmasına izin vermeyin ve içeriğin çevrimiçi barındırılan bir markas dosyası olarak depolanması gerekir. Markaşağı kutucuğunda satır içi içerik kullanırsanız, içerik şifrelemesini yönetemezsiniz. Kendi depolama alanınızı yapılandırarak, kendi anahtarlarınızı şifreleyebilir, Çift şifreleyebilir ve hatta taşıyabilirsiniz. Bu ilkeyi etkinleştirmek, kullanıcıların 2020-09-01-Preview veya daha yüksek REST API paylaşılan panolar sürümünü kullanmasını kısıtlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Portal/SharedDashboardInlineContent_Deny.json) |
