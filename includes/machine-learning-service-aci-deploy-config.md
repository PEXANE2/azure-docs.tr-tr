---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/26/2019
ms.author: larryfr
ms.openlocfilehash: 6e2a3a75181cf381f09e06b52c9bb3928dee4896
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68556859"
---
`deploymentconfig.json` Belgedeki girişler [aciwebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. ACI için değer olmalıdır `ACI`. |
| `containerResourceRequirements` | NA | CPU ve bellek varlıklarının kapsayıcısı. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Ayrılacak CPU çekirdeklerinin sayısı. Olarak`0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu Web hizmeti için ayrılacak bellek miktarı (GB cinsinden). Varsayılanını`0.5` |
| `location` | `location` | Bu Web hizmeti 'nin dağıtılacağı Azure bölgesi. Belirtilmemişse, çalışma alanı konumu kullanılacaktır. Kullanılabilir bölgeler hakkında daha fazla ayrıntı için şurada bulunabilir: [ACI bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Bu Web hizmeti için kimlik doğrulamanın etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir |
| `sslEnabled` | `ssl_enabled` | Bu Web hizmeti için SSL etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir. |
| `appInsightsEnabled` | `enable_app_insights` | Bu Web hizmeti için Appınsights etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir |
| `sslCertificate` | `ssl_cert_pem_file` | SSL etkinse gereken sertifika dosyası |
| `sslKey` | `ssl_key_pem_file` | SSL etkinse gereken anahtar dosya |
| `cname` | `ssl_cname` | SSL etkinse için CNAME |
| `dnsNameLabel` | `dns_name_label` | Puanlama uç noktası için DNS ad etiketi. Belirtilmemişse, Puanlama uç noktası için benzersiz bir DNS ad etiketi oluşturulacaktır. |

Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

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