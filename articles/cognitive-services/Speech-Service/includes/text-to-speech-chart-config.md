---
title: Konuşma kapsayıcılarını yükleme
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya dümen grafiği yapılandırma seçeneklerini ayrıntılarıyla anlatır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 22168974ab8b285413b4fa6e947c05f65a73ae12
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874371"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Metinden Konuşmaya (alt grafik: grafikler/textToSpeech)

"Şemsiye" grafiğigeçersiz kılmak için, daha `textToSpeech.` spesifik hale getirmek için herhangi bir parametrenin önekini ekleyin. Örneğin, örneğin ilgili parametre geçersiz kılar, `textToSpeech.numberOfConcurrentRequest` geçersiz `numberOfConcurrentRequest`kılar.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | **Metinden konuşmaya** hizmetin etkin olup olmadığı. | `false` |
| `numberOfConcurrentRequest` | **Metinden konuşmaya** hizmet için eşzamanlı isteklerin sayısı. Bu grafik, bu değere bağlı olarak CPU ve bellek kaynaklarını otomatik olarak hesaplar. | `2` |
| `optimizeForTurboMode`| Hizmetin metin dosyaları üzerinden metin girişi için en iyi duruma getirilmesi gerekip gerekmediği. Eğer, `true`bu grafik hizmete daha fazla CPU kaynağı tahsis edecektir. | `false` |
| `image.registry`| **Metinden konuşmaya** docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Metinden konuşmaya** docker görüntü deposu. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | **Metinden konuşmaya** docker görüntü etiketi. | `latest` |
| `image.pullSecrets` | **Metinden konuşmaya** docker görüntü çekme için görüntü sırları. | |
| `image.pullByHash`| Docker görüntüsünün karma tarafından çekilip çekilmediği. `image.hash` Gerekirse. `true` | `false` |
| `image.hash`| **Metinden konuşmaya** docker görüntü karma. Sadece . `image.pullByHash: true`  | |
| `image.args.eula`(gerekli) | Lisansı kabul ettiğinizi gösterir. Tek geçerli değer`accept` | |
| `image.args.billing`(gerekli) | Faturalandırma bitiş noktası URI değeri Azure portalının Konuşmaya Genel Bakış sayfasında kullanılabilir. | |
| `image.args.apikey`(gerekli) | Fatura bilgilerini izlemek için kullanılır. ||
| `service.type` | **Metinden konuşmaya** hizmetin Kubernetes hizmet türü. Daha fazla ayrıntı için [Kubernetes hizmet türleri yönergelerine](https://kubernetes.io/docs/concepts/services-networking/service/) bakın ve bulut sağlayıcısı desteğini doğrulayın. | `LoadBalancer` |
| `service.port`|  **Metinden konuşmaya** hizmetinin bağlantı noktası. | `80` |
| `service.annotations` | Hizmet meta verileri için **metinden konuşmaya** ek açıklamalar. Ek açıklamalar anahtar değer çiftleridir. <br>`annotations:`<br>&nbsp;&nbsp;`some/annotation1: value1`<br>&nbsp;&nbsp;`some/annotation2: value2` | |
| `service.autoScaler.enabled` | Yatay [Pod Otomatik Ölçekleyici](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) etkin olup olmadığı. Eğer, `true` `text-to-speech-autoscaler` Kubernetes kümesinde dağıtılır. | `true` |
| `service.podDisruption.enabled` | Pod [Kesinti Bütçesi](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. Eğer, `true` `text-to-speech-poddisruptionbudget` Kubernetes kümesinde dağıtılır. | `true` |