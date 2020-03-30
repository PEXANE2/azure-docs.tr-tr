---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: c71f35a06d904b45cb014d5199197220b57cf230
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79485994"
---
`deploymentconfig.json` [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)parametrelerine belge haritasında girişleri . Aşağıdaki tabloda JSON belgesindeki varlıklar ile yöntemparametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Açıklama |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. AKS için değer `aks`. |
| `autoScaler` | NA | Otomatik ölçeklendirme için yapılandırma öğeleri içerir. Otomatik ölçeklendirici tablosuna bakın. |
| &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Web hizmeti için otomatik leştirin etkinleştirin ister. `numReplicas`  = Eğer `0` `True`, ; aksi `False`takdirde, . |
| &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Bu web hizmetini otomatikleştirirken kullanılacak minimum kapsayıcı sayısı. Varsayılan, `1`. |
| &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Bu web hizmetini otomatikleştirirken kullanılacak maksimum kapsayıcı sayısı. Varsayılan, `10`. |
| &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Otomatik ölçeklendirici nin bu web hizmetini ne sıklıkta ölçeklendirmeye çalıştığı. Varsayılan, `1`. |
| &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | Otomatik ölçeklendiricinin bu web hizmeti için sürdürmeye çalışması gereken hedef kullanımı (yüzde 100'de). Varsayılan, `70`. |
| `dataCollection` | NA | Veri toplama için yapılandırma öğeleri içerir. |
| &emsp;&emsp;`storageEnabled` | `collect_model_data` | Web hizmeti için model veri toplama etkinleştirmek için olsun. Varsayılan, `False`. |
| `authEnabled` | `auth_enabled` | Web hizmeti için anahtar kimlik doğrulamasını etkinleştirip etkinleştirmemek. Her `tokenAuthEnabled` `authEnabled` ikisi `True`de ve olamaz . Varsayılan, `True`. |
| `tokenAuthEnabled` | `token_auth_enabled` | Web hizmeti için belirteç kimlik doğrulamasını etkinleştirip etkinleştirmemek. Her `tokenAuthEnabled` `authEnabled` ikisi `True`de ve olamaz . Varsayılan, `False`. |
| `containerResourceRequirements` | NA | CPU ve bellek varlıkları için kapsayıcı. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Bu web hizmeti için ayrılacak CPU çekirdeği sayısı. Varsayılan`0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu web hizmeti için ayrılacak bellek miktarı (GB olarak). Varsayılan`0.5` |
| `appInsightsEnabled` | `enable_app_insights` | Web hizmeti için Uygulama Öngörüleri'ni etkinleştirin. Varsayılan, `False`. |
| `scoringTimeoutMs` | `scoring_timeout_ms` | Web hizmetine yapılan çağrıları puanlamak için uygulamak için bir zaman arası. Varsayılan, `60000`. |
| `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Bu web hizmeti için düğüm başına en fazla eşzamanlı istek. Varsayılan, `1`. |
| `maxQueueWaitMs` | `max_request_wait_time` | Bir 503 hatası döndürülmeden önce bir isteğin en fazla sırada (milisaniye cinsinden) kalacağı maksimum süre. Varsayılan, `500`. |
| `numReplicas` | `num_replicas` | Bu web hizmeti için ayrılacak kapsayıcı sayısı. Varsayılan değer yoktur. Bu parametre ayarlanmazsa, otomatik ölçeklendirici varsayılan olarak etkinleştirilir. |
| `keys` | NA | Anahtarlar için yapılandırma öğeleri içerir. |
| &emsp;&emsp;`primaryKey` | `primary_key` | Bu Web hizmeti için kullanılacak birincil auth anahtarı |
| &emsp;&emsp;`secondaryKey` | `secondary_key` | Bu Web hizmeti için kullanılacak ikincil bir auth anahtarı |
| `gpuCores` | `gpu_cores` | Bu Web hizmeti için ayrılacak GPU çekirdeği (kapsayıcı başına yineleme) sayısı. Varsayılan değer 1'dir. Yalnızca tam sayı değerlerini destekler. |
| `livenessProbeRequirements` | NA | Canlılık sondası gereksinimleri için yapılandırma öğeleri içerir. |
| &emsp;&emsp;`periodSeconds` | `period_seconds` | Ne sıklıkta (saniye cinsinden) canlılık sondası gerçekleştirmek için. Varsayılan olarak 10 saniye. Minimum değer 1'dir. |
| &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Canlılık sondaları başlatılmadan önce konteyner başladıktan saniye ler sonra. Varsayılan 310 |
| &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Saniye sayısı, canlılık sondası zamanları doluyor. Varsayılan olarak 2 saniye. Minimum değer 1'dir |
| &emsp;&emsp;`successThreshold` | `success_threshold` | Canlılık sondası için art arda en az başarı başarısız olduktan sonra başarılı kabul edilmelidir. Varsayılan olarak 1'dir. Minimum değer 1'dir. |
| &emsp;&emsp;`failureThreshold` | `failure_threshold` | Bir Pod başlatıldığında ve canlılık sondası başarısız olduğunda, Kubernetes vazgeçmeden önce başarısızlıkEşik sürelerini dener. Varsayılan olarak 3 olarak. Minimum değer 1'dir. |
| `namespace` | `namespace` | Web hizmetinin dağıtılan Kubernetes ad alanı. En fazla 63 küçük alfasayısal ('a'-'z', '0'-'9') ve tire ('-') karakter. İlk ve son karakterler tire olamaz. |

Aşağıdaki JSON CLI ile kullanılmak üzere örnek bir dağıtım yapılandırmasıdır:

```json
{
    "computeType": "aks",
    "autoScaler":
    {
        "autoscaleEnabled": true,
        "minReplicas": 1,
        "maxReplicas": 3,
        "refreshPeriodInSeconds": 1,
        "targetUtilization": 70
    },
    "dataCollection":
    {
        "storageEnabled": true
    },
    "authEnabled": true,
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    }
}
```
