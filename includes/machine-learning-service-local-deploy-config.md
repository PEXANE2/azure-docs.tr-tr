---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/04/2019
ms.author: larryfr
ms.openlocfilehash: c288ea4a2c62999f27b3f07c39df40dae7ab1426
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926980"
---
`deploymentconfig.json` belgesindeki girdiler [Localwebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py)parametreleriyle eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | Yok | Bilgi işlem hedefi. Yerel hedefler için değerin `local`olması gerekir. |
| `port` | `port` | Hizmetin HTTP uç noktasının sergilebileceği yerel bağlantı noktası. |

Bu JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

```json
{
    "computeType": "local",
    "port": 32267
}
```
