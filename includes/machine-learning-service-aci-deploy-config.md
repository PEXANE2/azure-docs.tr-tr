---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 4f13c171c5fafb13875f5f87d4eb3d6013f0ff30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79485929"
---
`deploymentconfig.json` [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py)parametrelerine belge haritasındaki girişler. Aşağıdaki tabloda JSON belgesindeki varlıklar ile yöntemparametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. ACI için değer `ACI`. |
| `containerResourceRequirements` | NA | CPU ve bellek varlıkları için kapsayıcı. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Ayrılacak CPU çekirdeği sayısı. Varsayılan`0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu web hizmeti için ayrılacak bellek miktarı (GB olarak). Varsayılan`0.5` |
| `location` | `location` | Bu Web hizmetini dağıtmak için Azure bölgesi. Belirtilmemişse Çalışma Alanı konumu kullanılır. Mevcut bölgeler hakkında daha fazla bilgi için burada bulunabilir: [ACI Bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Bu Web hizmeti için auth etkinleştirmek ister. False varsayılanları |
| `sslEnabled` | `ssl_enabled` | Bu Web hizmeti için SSL'yi etkinleştirip etkinleştirmeyeceğiniz. Varsayılan olarak False için. |
| `appInsightsEnabled` | `enable_app_insights` | Bu Web hizmeti için AppInsights etkinleştirin olsun. False varsayılanları |
| `sslCertificate` | `ssl_cert_pem_file` | SSL etkinse gerekli sertifika dosyası |
| `sslKey` | `ssl_key_pem_file` | SSL etkinse gereken anahtar dosyası |
| `cname` | `ssl_cname` | SSL etkinse cname |
| `dnsNameLabel` | `dns_name_label` | Puanlama bitiş noktası için dns ad etiketi. Belirtilmezse, puanlama bitiş noktası için benzersiz bir dns adı etiketi oluşturulur. |

Aşağıdaki JSON CLI ile kullanılmak üzere örnek bir dağıtım yapılandırmasıdır:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```