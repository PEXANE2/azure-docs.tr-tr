---
title: Kapsayıcı güvenliği
titleSuffix: Azure Cognitive Services
description: Kabınızı nasıl güvene aldığınızı öğrenin
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/30/2019
ms.author: dapine
ms.openlocfilehash: 20f78d9269d4b2270293c8746157ba495c694562
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272781"
---
## <a name="azure-cognitive-services-container-security"></a>Azure Bilişsel Hizmetler konteyner güvenliği

Uygulamalar geliştirirken güvenlik birincil odak noktası olmalıdır. Güvenliğin önemi başarı için bir ölçüdür. Bilişsel Hizmetler kapsayıcılarını içeren bir yazılım çözümüslürken, kullanabileceğiniz sınırlamaları ve yetenekleri anlamak çok önemlidir. Ağ güvenliği hakkında daha fazla bilgi için Azure [Bilişsel Hizmetleri sanal ağlarını Yapılandırma'ya][az-security]bakın.

> [!IMPORTANT]
> Varsayılan olarak Bilişsel Hizmetler kapsayıcı API'si üzerinde *güvenlik yoktur.* Bunun nedeni, konteyner in en sık bir ağ köprüsü tarafından dışarıdan korunan bir pod bir parçası olarak çalışacak olmasıdır. Ancak, bulut tabanlı Bilişsel Hizmetlererişirken kullanılan kimlik doğrulama ile [cloud-based Cognitive Services][request-authentication]aynı şekilde çalışan kimlik doğrulamasını etkinleştirmek mümkündür.

Aşağıdaki diyagram varsayılan ve **güvenli olmayan** yaklaşımı göstermektedir:

![Kapsayıcı güvenliği](../media/container-security.svg)

Alternatif ve *güvenli* bir yaklaşım olarak, Bilişsel Hizmetler kapsayıcılarının tüketicileri, konteyner uç noktasını gizli tutarak, bir konteyneri ön bileşene sahip bir kap genişletebilir. [Istio'yu][istio] giriş kapısı olarak kullandığımız bir senaryo yu ele alalım. Istio HTTPS/TLS ve istemci sertifikası kimlik doğrulamasını destekler. Bu senaryoda, Istio ön uç kapsayıcı erişimini ortaya çıkarır ve Istio ile önceden beyaz listeye alınmış istemci sertifikasını sunar.

[Nginx][nginx] aynı kategoride başka bir popüler seçimdir. Hem Istio hem de Nginx bir servis örgüsi olarak hareket eder ve yük dengeleme, yönlendirme ve fiyat kontrolü gibi ek özellikler sunar.

### <a name="container-networking"></a>Kapsayıcı ağ iletişimi

Bilişsel Hizmetler kapları faturalandırma amacıyla ölçüm bilgilerini göndermek le yükümlüdür. Tek istisna, farklı bir faturalandırma metodolojisi izleyin gibi *Çevrimdışı kapsayıcılar* olduğunu. Bilişsel Hizmetler kapsayıcılarının güvendiği çeşitli ağ kanallarının listelenmemesine izin verilmemesi, kapsayıcının çalışmasını engeller.

#### <a name="allow-list-cognitive-services-domains-and-ports"></a>Listeye Bilişsel Hizmetler etki alanları ve bağlantı noktaları izin ver

Ana bilgisayar liste **bağlantı noktası 443** ve aşağıdaki etki alanları izin vermelidir:

* `*.cognitive.microsoft.com`
* `*.cognitiveservices.azure.com`

#### <a name="disable-deep-packet-inspection"></a>Derin paket denetimini devre dışı

> [Derin paket denetimi](https://en.wikipedia.org/wiki/Deep_packet_inspection) (DPI), bir bilgisayar ağı üzerinden gönderilen verileri ayrıntılı olarak inceleyen ve genellikle engelleyerek, yeniden yönlendirme yaparak veya buna göre günlüğe kaydederek harekete geçen bir veri işleme türüdür.

Bilişsel Hizmetler kapsayıcılarının Microsoft sunucularına oluşturduğu güvenli kanallardaki DPI'yi devre dışı kullanabilirsiniz. Aksi takdirde kapsayıcının düzgün çalışmasını engeller.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../cognitive-services-virtual-networks.md
