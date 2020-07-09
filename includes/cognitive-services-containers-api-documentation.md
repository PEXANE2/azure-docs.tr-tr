---
author: IEvangelist
ms.author: dapine
ms.date: 08/22/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 8b3d856d255968b4a6736db908ce3999cbd56193
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108943"
---
## <a name="validate-that-a-container-is-running"></a>Bir kapsayıcının çalıştığını doğrulama 

Kapsayıcının çalıştığını doğrulamak için birkaç yol vardır. *Dış IP* adresini ve söz konusu kapsayıcının bağlantı noktasını bulun ve en sevdiğiniz web tarayıcınızı açın. Kapsayıcının çalıştığını doğrulamak için aşağıdaki çeşitli istek URL 'Lerini kullanın. Aşağıda listelenen örnek URL 'Ler aşağıda verilmiştir `http://localhost:5000` ancak belirli Kapsayıcınız farklılık gösterebilir. Kapsayıcının *dış IP* adresine ve açığa çıkarılan bağlantı noktasına güvendiğini aklınızda bulundurun.

| İstek URL’si | Amaç |
|--|--|
| `http://localhost:5000/` | Kapsayıcı bir giriş sayfası sağlar. |
| `http://localhost:5000/ready` | GET ile istendi, bu, kapsayıcının modelde bir sorgu kabul etmeye hazırlandığından emin olmak için bir doğrulama sağlar.  Bu istek, Kubernetes [limize ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)için kullanılabilir. |
| `http://localhost:5000/status` | GET ile de istenirse, bu, kapsayıcıyı başlatmak için kullanılan api anahtarının bir uç nokta sorgusuna neden olmadan geçerli olup olmadığını doğrular. Bu istek, Kubernetes [limize ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)için kullanılabilir. |
| `http://localhost:5000/swagger` | Kapsayıcı, uç noktalar ve bir **deneyin** özelliği için tam bir belge kümesi sağlar. Bu özellikle, ayarlarınızı bir Web tabanlı HTML biçiminde girebilir ve herhangi bir kod yazmak zorunda kalmadan sorguyu oluşturabilirsiniz. Sorgu çağrıldıktan sonra, gereken HTTP üst bilgilerini ve gövde biçimini göstermek için örnek bir KıVRıMLı komut sağlanır. |

![Kapsayıcının ana sayfası](./media/cognitive-services-containers-api-documentation/container-webpage.png)
