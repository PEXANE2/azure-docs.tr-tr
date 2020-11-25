---
title: Sistem durumu kapsayıcısı örneğini doğrulama
titleSuffix: Azure Cognitive Services
description: Sistem durumu kapsayıcısı örneğini doğrulamayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 83a9eeb7644d107a808494ad06a8bef91d471fe1
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96009929"
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
