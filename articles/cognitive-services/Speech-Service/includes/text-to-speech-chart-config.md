---
title: Konuşma kapsayıcılarını yükler
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya helb grafik yapılandırma seçeneklerinin ayrıntılarını sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: e6c7dcd3015b0b8ab5b3c719ebd2397bc814b81a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67717230"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Metinden konuşmaya (alt grafik: grafikler/textToSpeech)

"Şemsiye" grafiğini geçersiz kılmak için, öneki `textToSpeech.` daha özel hale getirmek için herhangi bir parametreye ekleyin. Örneğin, buna karşılık gelen parametreyi `textToSpeech.numberOfConcurrentRequest` geçersiz kılar. `numberOfConcurrentRequest`

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | **Metin okuma** hizmetinin etkin olup olmadığı. | `false` |
| `numberOfConcurrentRequest` | **Metinden konuşmaya** hizmeti için eşzamanlı istek sayısı. Bu grafik, CPU ve bellek kaynaklarını bu değere göre otomatik olarak hesaplar. | `2` |
| `optimizeForTurboMode`| Hizmetin metin dosyaları aracılığıyla metin girişi için iyileştirilmesi gerekip gerekmediği. Bu `true`grafik, hizmete daha fazla CPU kaynağı ayırır. | `false` |
| `image.registry`| **Metinden konuşmaya** Docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Metinden konuşmaya** Docker görüntü deposu. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | **Metinden konuşmaya** Docker görüntü etiketi. | `latest` |
| `image.pullSecrets` | **Metinden konuşmaya** Docker görüntüsünü çekmek için görüntü gizli dizileri. | |
| `image.pullByHash`| Docker görüntüsünün karma tarafından çekilip çekilmediğini belirtir. Varsa `true` ,`image.hash` gereklidir. | `false` |
| `image.hash`| **Metinden konuşmaya** Docker görüntü karması. Yalnızca ne zaman `image.pullByHash: true`kullanılır.  | |
| `image.args.eula`istenir | Lisansı kabul ettiğinizi gösterir. Geçerli tek değer`accept` | |
| `image.args.billing`istenir | Faturalandırma uç noktası URI değeri Azure portal konuşmaya Genel Bakış sayfasında kullanılabilir. | |
| `image.args.apikey`istenir | Fatura bilgileri izlemek için kullanılır. ||
| `service.type` | **Metin okuma** hizmeti 'Nin Kubernetes hizmet türü. Daha fazla ayrıntı için bkz. [Kubernetes hizmet türleri yönergeleri](https://kubernetes.io/docs/concepts/services-networking/service/) ve bulut sağlayıcısı desteğini doğrulayın. | `LoadBalancer` |
| `service.port`|  **Metin okuma** hizmetinin bağlantı noktası. | `80` |
| `service.autoScaler.enabled` | [Yatay Pod otomatik Scaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) 'ın etkinleştirilip etkinleştirilmeyeceğini belirtir. İse `true`,Kuberneteskümesindedağıtılır. `text-to-speech-autoscaler` | `true` |
| `service.podDisruption.enabled` | [Pod kesintisi bütçesinin](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. İse `true`,Kuberneteskümesindedağıtılır. `text-to-speech-poddisruptionbudget` | `true` |