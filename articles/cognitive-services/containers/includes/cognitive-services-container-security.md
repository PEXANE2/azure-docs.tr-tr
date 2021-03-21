---
title: Kapsayıcı güvenliği
titleSuffix: Azure Cognitive Services
description: Kapsayıcınızı güvenli hale getirme hakkında bilgi edinin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 12/17/2020
ms.author: aahi
ms.openlocfilehash: 4b106ebc5606c4e5a290e12728d4e2011e80f6a2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97861831"
---
## <a name="azure-cognitive-services-container-security"></a>Azure bilişsel hizmetler kapsayıcı güvenliği

Uygulamalar geliştirirken güvenlik birincil bir odak olmalıdır. Güvenliğin önemi başarı için bir ölçümdür. Bilişsel hizmetler kapsayıcıları içeren bir yazılım çözümünü mimarmaya çalışırken, kullanabileceğiniz sınırlamaları ve özellikleri anlamak çok önemlidir. Ağ güvenliği hakkında daha fazla bilgi için bkz. Azure bilişsel [Hizmetler sanal ağlarını yapılandırma][az-security].

> [!IMPORTANT]
> Varsayılan olarak bilişsel hizmetler kapsayıcı API 'sinde bir *güvenlik yoktur* . Bunun nedeni, kapsayıcının bir ağ köprüsü tarafından dışından korunan Pod 'ın bir parçası olarak çalışacaktır. Ancak, [bulut tabanlı][request-authentication]bilişsel hizmetlere erişirken kullanılan kimlik doğrulaması ile aynı şekilde çalışacak kimlik doğrulamasını etkinleştirmek mümkündür.

Aşağıdaki diyagramda varsayılan ve **güvenli olmayan** yaklaşım gösterilmektedir:

![Kapsayıcı güvenliği](../media/container-security.svg)

Alternatif ve *güvenli* bir yaklaşım olarak bilişsel hizmet kapsayıcıları tüketicileri, bir kapsayıcıyı öne bakan bir bileşen ile artırabilir ve kapsayıcı uç noktasını özel olarak tutun. Bir giriş ağ [geçidi olarak][istio] sunduğumuz bir senaryoyu ele alalım. İstio, HTTPS/TLS ve istemci sertifikası kimlik doğrulamasını destekler. Bu senaryoda, Istio ön ucu kapsayıcı erişimini kullanıma sunar ve bu durumda Istio ile önceden onaylanan istemci sertifikasını sağlar.

[NGINX][nginx] aynı kategoride yer alan başka bir popüler seçenektir. Hem Istio hem de NGINX hizmet ağı olarak davranır ve Yük Dengeleme, Yönlendirme ve oran denetimi gibi ek özellikler sunar.

### <a name="container-networking"></a>Kapsayıcı ağ iletişimi

Bilişsel hizmetler kapsayıcıları, faturalandırma amaçlarıyla ölçüm bilgilerini göndermek için gereklidir. İzin verme hatası, bilişsel hizmetler kapsayıcılarının bağlı olduğu çeşitli ağ kanallarının kapsayıcının çalışmasını engelleyecek.

#### <a name="allow-list-cognitive-services-domains-and-ports"></a>İzin verilenler listesi bilişsel hizmetler etki alanları ve bağlantı noktaları

Ana bilgisayar, liste **bağlantı noktası 443** ve aşağıdaki etki alanlarına izin verir:

* `*.cognitive.microsoft.com`
* `*.cognitiveservices.azure.com`

#### <a name="disable-deep-packet-inspection"></a>Derin paket incelemesini devre dışı bırak

[Derin paket incelemesi](https://en.wikipedia.org/wiki/Deep_packet_inspection) (DPI), bir bilgisayar ağı üzerinden gönderilen verilerin ayrıntılarını inceleyerek ve genellikle işlem gerçekleştirerek, yeniden yönlendirerek veya uygun şekilde kaydederek işlem yapması gereken veri işleme türüdür.

Bilişsel hizmetler kapsayıcılarının Microsoft sunucuları 'nda oluşturmakta olduğu güvenli kanallarda DPı 'yi devre dışı bırakın. Bunun başarısız olması, kapsayıcının düzgün çalışmasını engeller.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../cognitive-services-virtual-networks.md
