---
title: Konuşma kapsayıcılarını yükler
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne hele grafik yapılandırma seçeneklerinin ayrıntılarını sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1b46c58d3f3c804052e637f7bde2e1a456764dba
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67717241"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Konuşmayı metne dönüştürme (alt grafik: grafikler/speechToText)

"Şemsiye" grafiğini geçersiz kılmak için, öneki `speechToText.` daha özel hale getirmek için herhangi bir parametreye ekleyin. Örneğin, buna karşılık gelen parametreyi `speechToText.numberOfConcurrentRequest` geçersiz kılar. `numberOfConcurrentRequest`

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | **Konuşmaya metin** hizmetinin etkinleştirilip etkinleştirilmeyeceğini belirtir. | `false` |
| `numberOfConcurrentRequest` | **Konuşmaya metin** hizmetine yönelik eşzamanlı istek sayısı. Bu grafik, CPU ve bellek kaynaklarını bu değere göre otomatik olarak hesaplar. | `2` |
| `optimizeForAudioFile`| Hizmetin ses dosyaları aracılığıyla ses girişi için iyileştirilmesi gerekip gerekmediği. Bu `true`grafik, hizmete daha fazla CPU kaynağı ayırır. | `false` |
| `image.registry`| **Konuşmadan metne** Docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Konuşmadan metne** Docker görüntü deposu. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | **Konuşmayı metne** Docker resim etiketi. | `latest` |
| `image.pullSecrets` | **Konuşmayı metne** dönüştürme Docker görüntüsünü çekmek için görüntü gizli dizileri. | |
| `image.pullByHash`| Docker görüntüsünün karma tarafından çekilip çekilmediğini belirtir. Varsa `true` ,`image.hash` gereklidir. | `false` |
| `image.hash`| **Konuşmadan metne** Docker görüntü karması. Yalnızca ne zaman `image.pullByHash: true`kullanılır.  | |
| `image.args.eula`istenir | Lisansı kabul ettiğinizi gösterir. Geçerli tek değer`accept` | |
| `image.args.billing`istenir | Faturalandırma uç noktası URI değeri Azure portal konuşmaya Genel Bakış sayfasında kullanılabilir. | |
| `image.args.apikey`istenir | Fatura bilgileri izlemek için kullanılır. ||
| `service.type` | **Konuşma-metin** hizmetinin Kubernetes hizmet türü. Daha fazla ayrıntı için bkz. [Kubernetes hizmet türleri yönergeleri](https://kubernetes.io/docs/concepts/services-networking/service/) ve bulut sağlayıcısı desteğini doğrulayın. | `LoadBalancer` |
| `service.port`|  **Konuşmadan metne** hizmetinin bağlantı noktası. | `80` |
| `service.autoScaler.enabled` | [Yatay Pod otomatik Scaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) 'ın etkinleştirilip etkinleştirilmeyeceğini belirtir. İse `true`,Kuberneteskümesindedağıtılır. `speech-to-text-autoscaler` | `true` |
| `service.podDisruption.enabled` | [Pod kesintisi bütçesinin](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. İse `true`,Kuberneteskümesindedağıtılır. `speech-to-text-poddisruptionbudget` | `true` |