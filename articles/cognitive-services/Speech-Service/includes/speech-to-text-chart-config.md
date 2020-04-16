---
title: Konuşma kapsayıcılarını yükleme
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne tümse grafik yapılandırma seçeneklerini ayrıntılarıyla anlatır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/22/2019
ms.author: trbye
ms.openlocfilehash: f7ca8fdaddab9757292939c4f7e658179d6e517c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421796"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Konuşma-Metin (alt grafik: grafikler/speechToText)

"Şemsiye" grafiğigeçersiz kılmak için, daha `speechToText.` spesifik hale getirmek için herhangi bir parametrenin önekini ekleyin. Örneğin, örneğin ilgili parametre geçersiz kılar, `speechToText.numberOfConcurrentRequest` geçersiz `numberOfConcurrentRequest`kılar.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | **Konuşmadan metne** hizmetin etkin olup olmadığı. | `false` |
| `numberOfConcurrentRequest` | **Konuşmadan metne** hizmet için eşzamanlı isteklerin sayısı. Bu grafik, bu değere bağlı olarak CPU ve bellek kaynaklarını otomatik olarak hesaplar. | `2` |
| `optimizeForAudioFile`| Hizmetin ses dosyaları aracılığıyla ses girişi için optimize edilmesi gerekip gerekmediği. Eğer, `true`bu grafik hizmete daha fazla CPU kaynağı tahsis edecektir. | `false` |
| `image.registry`| **Konuşma-to-metin** docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Konuşma-to-metin** docker görüntü deposu. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | **Konuşma-to-metin** docker görüntü etiketi. | `latest` |
| `image.pullSecrets` | **Konuşma-to-metin** docker görüntü çekmek için görüntü sırları. | |
| `image.pullByHash`| Docker görüntüsünün karma tarafından çekilip çekilmediği. `image.hash` Gerekirse. `true` | `false` |
| `image.hash`| **Konuşma-to-metin** docker görüntü karma. Sadece . `image.pullByHash: true`  | |
| `image.args.eula`(gerekli) | Lisansı kabul ettiğinizi gösterir. Tek geçerli değer`accept` | |
| `image.args.billing`(gerekli) | Faturalandırma bitiş noktası URI değeri Azure portalının Konuşmaya Genel Bakış sayfasında kullanılabilir. | |
| `image.args.apikey`(gerekli) | Fatura bilgilerini izlemek için kullanılır. ||
| `service.type` | **Konuşma-metin** hizmetinin Kubernetes hizmet türü. Daha fazla ayrıntı için [Kubernetes hizmet türleri yönergelerine](https://kubernetes.io/docs/concepts/services-networking/service/) bakın ve bulut sağlayıcısı desteğini doğrulayın. | `LoadBalancer` |
| `service.port`|  **Konuşma-metin** hizmetinin bağlantı noktası. | `80` |
| `service.annotations` | Hizmet meta verileri için **konuşmadan metne** ek açıklamalar. Ek açıklamalar anahtar değer çiftleridir. <br>`annotations:`<br>&nbsp;&nbsp;`some/annotation1: value1`<br>&nbsp;&nbsp;`some/annotation2: value2` | |
| `service.autoScaler.enabled` | Yatay [Pod Otomatik Ölçekleyici](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) etkin olup olmadığı. Eğer, `true` `speech-to-text-autoscaler` Kubernetes kümesinde dağıtılır. | `true` |
| `service.podDisruption.enabled` | Pod [Kesinti Bütçesi](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkin olup olmadığı. Eğer, `true` `speech-to-text-poddisruptionbudget` Kubernetes kümesinde dağıtılır. | `true` |