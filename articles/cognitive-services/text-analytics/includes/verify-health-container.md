---
title: Sistem durumu kapsayıcısı örneğini doğrulama
titleSuffix: Azure Cognitive Services
description: Sistem durumu kapsayıcısı örneğini doğrulamayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 5c598807f36000a18211e32eba53220bfbeea2f7
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108728"
---
### <a name="verify-that-a-container-is-running"></a>Bir kapsayıcının çalıştığını doğrulama

1. **Genel bakış** sekmesini SEÇIN ve IP adresini kopyalayın.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini girin. Örneğin, girin `http://<IP-address>:5000 (http://55.55.55.55:5000` ). Kapsayıcının ana sayfası görüntülenir ve bu da kapsayıcının çalıştığını bilmenizi sağlar.

    ![Çalıştığını doğrulamak için kapsayıcı giriş sayfasını görüntüleme](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Kapsayıcının Swagger sayfasına gitmek için **hizmet API 'Si açıklaması** bağlantısını seçin.

1. **Post** API 'lerinden birini seçin ve **dene**' yi seçin. Parametreler, örnek girişi içeren görüntülenir.

Kapsayıcının çalıştığını doğrulamak için kullanabileceğiniz çeşitli URL 'Ler vardır.

|İstek|Amaç|
|--|--|
|`http://localhost:5000/`|Kapsayıcı bir giriş sayfası sağlar.|
|`http://localhost:5000/ready`|GET ile istendi, bu, kapsayıcının modelde bir sorgu kabul etmeye hazırlandığından emin olmak için bir doğrulama sağlar. Bu istek, Kubernetes [limize ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)için kullanılabilir.|
|`http://localhost:5000/status`|GET ile istenen,/Ready uç noktası gibi, bu, kapsayıcının bir sorgu oluşturmadan çalıştığını doğrular. Bu istek, Kubernetes [limize ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)için kullanılabilir.|
|`http://localhost:5000/swagger`|Bu URL aracılığıyla kapsayıcı, uç noktalar ve bir özellik için tam bir belge kümesi sağlar `Try it now` . Bu özellikle, ayarlarınızı bir Web tabanlı HTML biçiminde girebilir ve herhangi bir kod yazmak zorunda kalmadan sorguyu oluşturabilirsiniz. Sorgu çağrıldıktan sonra, gereken HTTP üst bilgilerini ve gövde biçimini göstermek için örnek bir KıVRıMLı komut sağlanır. |
|`http://localhost:5000/demo`| Bir tarayıcı aracılığıyla istenen bu özellik, giriş metin örnekleri veya sağladığınız bir sorgudan elde edilen sonuçların etkileşimli bir görselleştirmesini sağlar.  |

`http://localhost:5000/text/analytics/v3.0-preview.1/domains/health`Kapsayıcıya bir sorgu göndermek için bu istek URL 'sini kullanın.
