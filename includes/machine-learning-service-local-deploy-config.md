---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 0eeb82245a53c93af75fc3ce3f37cb588295e5b7
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102508104"
---
`deploymentconfig.json`Belgedeki girişler [LocalWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. Yerel hedefler için değer olmalıdır `local` . |
| `port` | `port` | Hizmetin HTTP uç noktasının sergilebileceği yerel bağlantı noktası. |

Bu JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

```json
{
    "computeType": "local",
    "port": 32267
}
```