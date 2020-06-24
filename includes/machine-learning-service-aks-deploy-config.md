---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: c71f35a06d904b45cb014d5199197220b57cf230
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79485994"
---
Belgedeki girişler, `deploymentconfig.json` [Akswebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

| JSON varlığı | Yöntem parametresi | Description |
| ----- | ----- | ----- |
| `computeType` | NA | Bilgi işlem hedefi. AKS için değer olmalıdır `aks` . |
| `autoScaler` | NA | Otomatik ölçeklendirme için yapılandırma öğelerini içerir. Bkz. otomatik Scaler tablosu. |
| &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Web hizmeti için otomatik ölçeklendirmenin etkinleştirilip etkinleştirilmeyeceğini belirtir. Eğer `numReplicas`  =  `0` ,, yoksa `True` , `False` . |
| &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı alt sınırı. Varsayılan, `1` . |
| &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı üst sınırı. Varsayılan, `10` . |
| &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Otomatik Scaler, bu Web hizmetini ölçeklendirmeye ne sıklıkta çalışır. Varsayılan, `1` . |
| &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | Otomatik Scaler 'nın bu Web hizmeti için bakımını denemesi gereken hedef kullanım (100 ' dan fazla). Varsayılan, `70` . |
| `dataCollection` | NA | Veri toplama için yapılandırma öğelerini içerir. |
| &emsp;&emsp;`storageEnabled` | `collect_model_data` | Web hizmeti için model veri toplamayı etkinleştirip etkinleştirmeyeceğinizi belirtir. Varsayılan, `False` . |
| `authEnabled` | `auth_enabled` | Web hizmeti için anahtar kimlik doğrulamasının etkinleştirilip etkinleştirilmeyeceğini belirtir. Her ikisi de olamaz `tokenAuthEnabled` `authEnabled` `True` . Varsayılan, `True` . |
| `tokenAuthEnabled` | `token_auth_enabled` | Web hizmeti için belirteç kimlik doğrulamasının etkinleştirilip etkinleştirilmeyeceğini belirtir. Her ikisi de olamaz `tokenAuthEnabled` `authEnabled` `True` . Varsayılan, `False` . |
| `containerResourceRequirements` | NA | CPU ve bellek varlıklarının kapsayıcısı. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Bu Web hizmeti için ayrılacak CPU çekirdeklerinin sayısı. Olarak`0.1` |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu Web hizmeti için ayrılacak bellek miktarı (GB cinsinden). Varsayılanını`0.5` |
| `appInsightsEnabled` | `enable_app_insights` | Web hizmeti için Application Insights günlüğü etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan, `False` . |
| `scoringTimeoutMs` | `scoring_timeout_ms` | Web hizmetine yönelik Puanlama çağrılarına zorlamak için zaman aşımı. Varsayılan, `60000` . |
| `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Bu Web hizmeti için düğüm başına en fazla eşzamanlı istek. Varsayılan, `1` . |
| `maxQueueWaitMs` | `max_request_wait_time` | Bir 503 hatası döndürülmeden önce bir isteğin en uzun süre (milisaniye cinsinden) kalacağız. Varsayılan, `500` . |
| `numReplicas` | `num_replicas` | Bu Web hizmeti için ayrılacak kapsayıcı sayısı. Varsayılan değer yoktur. Bu parametre ayarlanmamışsa otomatik olarak varsayılan olarak etkindir. |
| `keys` | NA | Anahtarlar için yapılandırma öğelerini içerir. |
| &emsp;&emsp;`primaryKey` | `primary_key` | Bu Web hizmeti için kullanılacak birincil bir kimlik doğrulama anahtarı |
| &emsp;&emsp;`secondaryKey` | `secondary_key` | Bu Web hizmeti için kullanılacak ikincil bir kimlik doğrulama anahtarı |
| `gpuCores` | `gpu_cores` | Bu Web hizmeti için ayrılacak GPU çekirdekleri (kapsayıcı başına çoğaltma) sayısı. Varsayılan değer 1 ' dir. Yalnızca tam sayı değerlerini destekler. |
| `livenessProbeRequirements` | NA | Lizleştirme araştırma gereksinimleri için yapılandırma öğelerini içerir. |
| &emsp;&emsp;`periodSeconds` | `period_seconds` | Lizleştirme araştırması gerçekleştirmek için ne sıklıkla (saniye cinsinden). Varsayılan değer 10 saniyedir. En küçük değer 1 ' dir. |
| &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Ebilzleştirme araştırmadan önce kapsayıcının başladıktan sonra geçmesi gereken saniye sayısı. Varsayılan değer 310 ' dir |
| &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Lizleştirme araştırması zaman aşımına uğramadan geçen saniye sayısı. Varsayılan değer 2 saniyedir. En düşük değer 1 ' dir |
| &emsp;&emsp;`successThreshold` | `success_threshold` | Hatalı olduktan sonra başarılı olarak değerlendirilme araştırması için en düşük ardışık başarı. Varsayılan değer 1 ' dir. En küçük değer 1 ' dir. |
| &emsp;&emsp;`failureThreshold` | `failure_threshold` | Bir pod başlatıldığında ve elek araştırması başarısız olduğunda Kubernetes, başlamadan önce failureThreshold sürelerini dener. Varsayılan olarak 3 ' e döner. En küçük değer 1 ' dir. |
| `namespace` | `namespace` | Web hizmeti 'nin dağıtıldığı Kubernetes ad alanı. 63 ' e kadar küçük alfasayısal (' A'-'z ', ' 0 '-' 9 ') ve tire ('-') karakterleri. İlk ve son karakterler kısa çizgi olamaz. |

Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

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
