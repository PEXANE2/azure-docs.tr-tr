---
title: Kapsayıcı güvenliği
titleSuffix: Azure Cognitive Services
description: Kapsayıcınızı güvenli hale getirme hakkında bilgi edinin
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: dapine
ms.openlocfilehash: d8d069dddbce6ab6ddb541db460634ad3f6fa067
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70994937"
---
## <a name="azure-cognitive-services-container-security"></a>Azure bilişsel hizmetler kapsayıcı güvenliği

Uygulamalar geliştirirken güvenlik birincil bir odak olmalıdır. Güvenliğin önemi başarı için bir ölçümdür. Bilişsel hizmetler kapsayıcıları içeren bir yazılım çözümünü mimarmaya çalışırken, kullanabileceğiniz sınırlamaları ve özellikleri anlamak çok önemlidir. Daha fazla bilgi için bkz. [Azure güvenliği][az-security].

> [!IMPORTANT]
> Varsayılan olarak bilişsel hizmetler kapsayıcı API 'sinde bir *güvenlik yoktur* . Bunun nedeni, kapsayıcının bir ağ köprüsü tarafından dışından korunan Pod 'ın bir parçası olarak çalışacaktır. Ancak, [bulut tabanlı][request-authentication]bilişsel hizmetlere erişirken kullanılan kimlik doğrulaması ile aynı şekilde çalışacak kimlik doğrulamasını etkinleştirmek mümkündür.

Aşağıdaki diyagramda varsayılan ve **güvenli olmayan** yaklaşım gösterilmektedir:

![Kapsayıcı güvenliği](../media/container-security.svg)

Alternatif ve *güvenli* bir yaklaşım olarak bilişsel hizmet kapsayıcıları tüketicileri, bir kapsayıcıyı öne bakan bir bileşen ile artırabilir ve kapsayıcı uç noktasını özel olarak tutun. Bir giriş ağ [geçidi olarak][istio] sunduğumuz bir senaryoyu ele alalım. İstio, HTTPS/SSL ve istemci sertifikası kimlik doğrulamasını destekler. Bu senaryoda, Istio ön ucu kapsayıcı erişimini kullanıma sunarak, daha önce Istio ile daha önce listelenmiş olan istemci sertifikasını sunar.

[NGINX][nginx] aynı kategoride yer alan başka bir popüler seçenektir. Hem Istio hem de NGINX hizmet ağı olarak davranır ve Yük Dengeleme, Yönlendirme ve oran denetimi gibi ek özellikler sunar.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../../security/fundamentals/overview.md
