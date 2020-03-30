---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 60c754aeec3157abee02eb136dcf148c440d8da0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477281"
---
`deploymentconfig.json` [LocalWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py)parametrelerine belge haritasındaki girişler. Aşağıdaki tabloda JSON belgesindeki varlıklar ile yöntemparametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. Yerel hedefler için değer `local`. |
| `port` | `port` | Hizmetin HTTP bitiş noktasını ortaya çıkarmak için yerel bağlantı noktası. |

Bu JSON CLI ile kullanılmak üzere örnek bir dağıtım yapılandırmasıdır:

```json
{
    "computeType": "local",
    "port": 32267
}
```
