---
author: IEvangelist
ms.author: dapine
ms.date: 08/22/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 081155c8984f39cc9cc8e905eb108c07dee98d2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70034422"
---
## <a name="validate-that-a-container-is-running"></a>Bir kapsayıcının çalıştığını doğrulayın 

Kapsayıcının çalıştığını doğrulamanın birkaç yolu vardır. Söz konusu kabın *Harici IP* adresini ve açık bağlantı noktasını bulun ve en sevdiğiniz web tarayıcısını açın. Kapsayıcının çalıştığını doğrulamak için aşağıdaki çeşitli istek URL'lerini kullanın. Aşağıda listelenen örnek istek URL'leri şunlardır, `http://localhost:5000`ancak belirli kapsayıcınız değişebilir. Konteynerinizin *Harici IP* adresine ve açık bağlantı noktasına güvenmeniz gerektiğini unutmayın.

| İstek URL'si | Amaç |
|--|--|
| `http://localhost:5000/` | Kapsayıcı bir giriş sayfası sağlar. |
| `http://localhost:5000/status` | Bir HTTP GET ile, kapsayıcı bir bitiş noktası sorgusu neden olmadan çalıştığını doğrulamak için istenir. Bu istek Kubernetes [canlılık ve hazırlık probları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)için kullanılabilir. |
| `http://localhost:5000/swagger` | Kapsayıcı uç noktalar için tam bir belge kümesi ve bir **Try out** özelliği sağlar. Bu özellik sayesinde, ayarlarınızı web tabanlı bir HTML formuna girebilir ve herhangi bir kod yazmak zorunda kalmadan sorguyu yapabilirsiniz. Sorgu döndükten sonra, gerekli olan HTTP üstbilgilerini ve gövde biçimini göstermek için örnek bir CURL komutu sağlanır. |

![Konteynerin ana sayfası](./media/cognitive-services-containers-api-documentation/container-webpage.png)
