---
title: Konuşma kapsayıcılarını yükler
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne hele grafik yapılandırma seçeneklerinin ayrıntılarını sağlar.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.openlocfilehash: 267c2fb72b38053429019746a573c740d812c38c
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82608395"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Konuşmayı metne dönüştürme (alt grafik: grafikler/speechToText)

"Şemsiye" grafiğini geçersiz kılmak için, öneki `speechToText.` daha özel hale getirmek için herhangi bir parametreye ekleyin. Örneğin, karşılık gelen parametreyi `speechToText.numberOfConcurrentRequest` geçersiz kılacak şekilde geçersiz kılar. `numberOfConcurrentRequest`

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | **Konuşmaya metin** hizmetinin etkinleştirilip etkinleştirilmeyeceğini belirtir. | `false` |
| `numberOfConcurrentRequest` | **Konuşmaya metin** hizmetine yönelik eşzamanlı istek sayısı. Bu grafik, CPU ve bellek kaynaklarını bu değere göre otomatik olarak hesaplar. | `2` |
| `optimizeForAudioFile`| Hizmetin ses dosyaları aracılığıyla ses girişi için iyileştirilmesi gerekip gerekmediği. Bu `true`grafik, hizmete daha fazla CPU kaynağı ayırır. | `false` |
| `image.registry`| **Konuşmadan metne** Docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Konuşmadan metne** Docker görüntü deposu. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | **Konuşmayı metne** Docker resim etiketi. | `latest` |
| `image.pullSecrets` | **Konuşmayı metne** dönüştürme Docker görüntüsünü çekmek için görüntü gizli dizileri. | |
| `image.pullByHash`| Docker görüntüsünün karma tarafından çekilip çekilmediğini belirtir. Varsa `true`, `image.hash` gereklidir. | `false` |
| `image.hash`| **Konuşmadan metne** Docker görüntü karması. Yalnızca ne zaman `image.pullByHash: true`kullanılır.  | |
| `image.args.eula`istenir | Lisansı kabul ettiğinizi gösterir. Geçerli tek değer`accept` | |
| `image.args.billing`istenir | Faturalandırma uç noktası URI değeri Azure portal konuşmaya Genel Bakış sayfasında kullanılabilir. | |
| `image.args.apikey`istenir | Faturalandırma bilgilerini izlemek için kullanılır. ||
| `service.type` | **Konuşma-metin** hizmetinin Kubernetes hizmet türü. Daha fazla ayrıntı için bkz. [Kubernetes hizmet türleri yönergeleri](https://kubernetes.io/docs/concepts/services-networking/service/) ve bulut sağlayıcısı desteğini doğrulayın. | `LoadBalancer` |
| `service.port`|  **Konuşmadan metne** hizmetinin bağlantı noktası. | `80` |
| `service.annotations` | Hizmet meta verileri için **konuşmaya metin** ek açıklamaları. Ek açıklamalar anahtar değer çiftleridir. <br>`annotations:`<br>&nbsp;&nbsp;`some/annotation1: value1`<br>&nbsp;&nbsp;`some/annotation2: value2` | |
| `service.autoScaler.enabled` | [Yatay Pod otomatik Scaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) 'ın etkinleştirilip etkinleştirilmeyeceğini belirtir. `speech-to-text-autoscaler` Ise `true`, Kubernetes kümesinde dağıtılır. | `true` |
| `service.podDisruption.enabled` | [Pod kesintisi bütçesinin](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. `speech-to-text-poddisruptionbudget` Ise `true`, Kubernetes kümesinde dağıtılır. | `true` |

#### <a name="sentiment-analysis-sub-chart-chartsspeechtotext"></a>Yaklaşım Analizi (alt grafik: grafikler/speechToText)

Konuşmayı metin kapsayıcısının v 2.2.0 'den başlayarak, Metin Analizi API'si kullanarak yaklaşım analizi için aşağıdaki parametreler kullanılır.

|Parametre|Açıklama|Değerler|Varsayılan|
| --- | --- | --- | --- |
|`textanalytics.enabled`| **Metin analizi** hizmetinin etkin olup olmadığı| doğru/yanlış| `false`|
|`textanalytics.image.registry`| **Metin analizi** Docker görüntü kayıt defteri| geçerli Docker görüntü kayıt defteri| |
|`textanalytics.image.repository`| **Metin analizi** Docker görüntü deposu| geçerli Docker görüntü deposu| |
|`textanalytics.image.tag`| **Metin analizi** Docker resim etiketi| geçerli Docker resmi etiketi| |
|`textanalytics.image.pullSecrets`| **Metin analizi** Docker görüntüsü çekme için görüntü gizli dizileri| geçerli gizli diziler adı| |
|`textanalytics.image.pullByHash`| Karma olarak Docker görüntüsü çekiliyor olup olmadığını belirtir.  Varsa `yes`, `image.hash` bunun da olması gerekir. Varsa `no`, ' false ' olarak ayarlayın. `false` varsayılan değerdir.| doğru/yanlış| `false`|
|`textanalytics.image.hash`| **Metin analizi** Docker görüntü karması. Yalnızca ile `image.pullByHash:true`kullanın.| geçerli Docker görüntü karması | |
|`textanalytics.image.args.eula`| , Lisansı kabul ettiğinizi belirten, **metin analizi** kapsayıcısına göre gerekli bağımsız değişkenlerden biri. Bu seçeneğin değeri şu olmalıdır: `accept`.| `accept`kapsayıcısını kullanmak istiyorsanız | |
|`textanalytics.image.args.billing`| Faturalama uç noktası URI 'sini belirten, **metin analizi** kapsayıcısına göre gerekli bağımsız değişkenlerden biri. Faturalandırma uç noktası URI değeri Azure portal konuşmaya Genel Bakış sayfasında kullanılabilir.|Geçerli faturalandırma uç noktası URI 'SI||
|`textanalytics.image.args.apikey`| Fatura bilgilerini izlemek için kullanılan, **metin analizi** kapsayıcısına göre gerekli bağımsız değişkenlerden biri.| geçerli apikey||
|`textanalytics.cpuRequest`| **Metin analizi** kapsayıcısı IÇIN istenen CPU| int| `3000m`|
|`textanalytics.cpuLimit`| **Metin analizi** kapsayıcısı IÇIN sınırlı CPU| | `8000m`|
|`textanalytics.memoryRequest`| **Metin analizi** kapsayıcısı için istenen bellek| | `3Gi`|
|`textanalytics.memoryLimit`| **Metin analizi** kapsayıcısı için sınırlı bellek| | `8Gi`|
|`textanalytics.service.sentimentURISuffix`| Yaklaşım Analizi URI soneki, tüm URI "`<service>`http://:`<port>`/`<sentimentURISuffix>`" biçimindedir. | | `text/analytics/v3.0-preview/sentiment`|
|`textanalytics.service.type`| Kubernetes 'teki **metin analizi** hizmetinin türü. Bkz. [Kubernetes hizmet türleri](https://kubernetes.io/docs/concepts/services-networking/service/) | geçerli Kubernetes hizmet türü | `LoadBalancer` |
|`textanalytics.service.port`| **Metin analizi** hizmetinin bağlantı noktası| int| `50085`|
|`textanalytics.service.annotations`| Kullanıcılar **metin analizi** hizmeti meta verilerine eklenebilir. Örneğin:<br/> **açıklamaları**<br/>`   `**Some/annotation1: değer1**<br/>`  `**Some/annotation2: değer2** | her satır için bir adet ek açıklama| |
|`textanalytics.serivce.autoScaler.enabled`| [Yatay Pod otomatik Scaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) etkinleştirilip etkinleştirilmediği. Etkinleştirilirse, `text-analytics-autoscaler` Kubernetes kümesinde dağıtılır | doğru/yanlış| `true`|
|`textanalytics.service.podDisruption.enabled`| [Pod kesintisi bütçesinin](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. Etkinleştirilirse, `text-analytics-poddisruptionbudget` Kubernetes kümesinde dağıtılır| doğru/yanlış| `true`|
