---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: e8c0b35f2ac6d3468a5e38a90cc7f4de09d3682b
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348569"
---
`deploymentconfig.json` Belgedeki girişler [localwebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. Yerel için değer olmalıdır `local`. |
| `port` | `port` | Hizmetin HTTP uç noktasının sergilebileceği yerel bağlantı noktası. |

Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

```json
{
    "computeType": "local",
    "port": 32267
}
```
