---
title: Linkerd'a Genel Bakış
description: Linkerd'a genel bakış elde edin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 3181be62a14ec1b3450bd181172b5323ca176427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593776"
---
# <a name="linkerd"></a>Linkerd

## <a name="overview"></a>Genel Bakış

[Linkerd][linkerd] kullanımı kolay ve hafif servis örgü.

## <a name="architecture"></a>Mimari

Linkerd ultralight [Linkerd][linkerd-proxy] özel proxy sidecars oluşan bir veri düzlemi sağlar. Bu akıllı yakınlıklar, meshed uygulamalarınıza ve iş yüklerinizdeki tüm ağ trafiğini kontrol eder. [Prometheus][prometheus] ölçümleri uç noktaları üzerinden ölçümleri de ortaya çıkarır.

Kontrol düzlemi aşağıdaki [bileşenler][linkerd-architecture]aracılığıyla yapılandırmayı ve toplu telemetriyi yönetir:

- **Denetleyici** - Linkerd CLI ve Pano'yu yönlendiren api sağlar. Yakınlıklar için yapılandırma sağlar.

- **Dokunun** - İstek ve yanıtlar üzerinde gerçek zamanlı saatler kurun.

- **Kimlik** - Hizmetler arasında mTLS'ye izin veren kimlik ve güvenlik yetenekleri sağlar.

- **Web** - Linkerd panosusağlar.


Aşağıdaki mimari diyagram, veri düzlemi ve kontrol düzlemindeki çeşitli bileşenlerin nasıl etkileştiğini gösterir.


![Linkerd bileşenlerine ve mimarisine genel bakış.](media/servicemesh/linkerd/about-architecture.png)


## <a name="selection-criteria"></a>Seçim kriterleri

İş yükleriniz için Linkerd'i değerlendirirken aşağıdaki alanları anlamak ve göz önünde bulundurmak önemlidir:

- [Tasarım İlkeleri](#design-principles)
- [Özellikler](#capabilities)
- [Senaryolar](#scenarios)


### <a name="design-principles"></a>Tasarım ilkeleri

Aşağıdaki tasarım ilkeleri Linkerd projesini [yönlendirir:][design-principles]

- **Basit tutun** - kullanımı ve anlaşılması kolay olmalıdır.

- **Kaynak Gereksinimlerini En Aza Indirin** - En az performans ve kaynak maliyeti uygulayın.

- **Sadece Çalışma** - Varolan uygulamaları bozmayın ve karmaşık yapılandırma gerektirmez.


### <a name="capabilities"></a>Özellikler

Linkerd aşağıdaki yetenekleri sağlar:

- **Mesh** – hata ayıklama seçeneğiyerleşik

- **Trafik Yönetimi** – bölme, zaman ayırma, yeniden deneme, giriş

- **Güvenlik** – şifreleme (mTLS), her 24 saatte bir otomatik olarak dönen sertifikalar

- **Gözlemlenebilirlik** – altın ölçümler, musluk, izleme, servis profilleri ve rota ölçümleri başına, topoloji grafikleri ile web panosu, prometheus, grafana


### <a name="scenarios"></a>Senaryolar

Linkerd aşağıdaki senaryolar için uygundur ve önerilir:

- Sadece temel yetenek gereksinimleri kümesiyle kullanımı basit

- Düşük gecikme, düşük genel yük, gözlemlenebilirlik ve basit trafik yönetimi odaklı


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, Azure Kubernetes Hizmeti'ne (AKS) Linkerd'i nasıl yükleyebileceğinizi açıklar:

> [!div class="nextstepaction"]
> [Azure Kubernetes Hizmetinde (AKS) Linkerd'i Yükleyin][linkerd-install]

Linkerd özelliklerini ve mimarisini daha fazla keşfedebilirsiniz:

- [Linkerd Özellikleri][linkerd-features]
- [Linkerd Mimarlık][linkerd-architecture]

<!-- LINKS - external -->
[linkerd]: https://linkerd.io/2/overview/
[linkerd-architecture]: https://linkerd.io/2/reference/architecture/
[linkerd-features]: https://linkerd.io/2/features/
[design-principles]: https://linkerd.io/2/design-principles/
[linkerd-proxy]: https://github.com/linkerd/linkerd2-proxy

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[linkerd-install]: ./servicemesh-linkerd-install.md