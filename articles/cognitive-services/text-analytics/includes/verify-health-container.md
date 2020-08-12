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
ms.openlocfilehash: 1a8ce0bc94c61a0cfe6cdad11375763ba954957d
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122479"
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

`http://localhost:5000/text/analytics/v3.2-preview.1/entities/health`Kapsayıcıya bir sorgu göndermek için bu istek URL 'sini kullanın.
