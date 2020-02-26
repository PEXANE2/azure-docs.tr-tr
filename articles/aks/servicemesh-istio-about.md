---
title: Istio 'ya genel bakış
description: Istio 'ya genel bakış edinme
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 8518e30a54c2486abf84cd9ac026cc4dccb3fa84
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77593909"
---
# <a name="istio"></a>İstio dili

## <a name="overview"></a>Genel Bakış

[Istio][istio] , tam özellikli, özelleştirilebilir ve genişletilebilir bir hizmet ağı.

## <a name="architecture"></a>Mimari

İstio, [Envoy][envoy-proxy]tabanlı parçalardan oluşan bir veri düzlemi sağlar. Bu akıllı proxy 'ler, yerleşik uygulamalarınızın ve iş yüklerinizin içindeki ve içindeki tüm ağ trafiğini denetler.

Denetim düzlemi yapılandırma, ilke ve Telemetriyi aşağıdaki [Bileşenler][what-is-istio]aracılığıyla yönetir:

- **Karıştırıcı** -erişim denetimi ve kullanım ilkelerini uygular. [Prometheus][prometheus]'a gönderilen proxy 'lerden telemetri toplar.

- **Pilot** -proxy 'ler için hizmet bulma ve trafik yönetimi ilkesi/yapılandırma sağlar.

- **CITADEL** -hizmetler arasında MTLS 'ye izin veren kimlik ve güvenlik özellikleri sağlar.

- **Gale** -Özet ve bileşenlere yapılandırma sağlar.

Aşağıdaki mimari diyagramda, veri düzlemi ve denetim düzlemi içindeki çeşitli bileşenlerin nasıl etkileşimde bulunduğu gösterilmektedir.


![Istio bileşenlerine ve mimarisine genel bakış.](media/servicemesh/istio/about-architecture.png)


## <a name="selection-criteria"></a>Seçim ölçütü

İş yükleriniz için bir Istil değerlendirirken aşağıdaki alanların anlaşılması ve dikkate alınması önemlidir:

- [Tasarım hedefleri](#design-goals)
- [Yetenek](#capabilities)
- [Senaryolar](#scenarios)


### <a name="design-goals"></a>Tasarım hedefleri

Aşağıdaki tasarım hedefleri, Istio projesine [kılavuzluk][design-goals] eden:

- **Saydamlığı en üst düzeye çıkarın** -sistemden gerçek değer almak için minimum iş miktarı ile benimseme olanağı sağlar.

- **Genişletilebilirlik** -değişen ihtiyaçları büyütmek ve uyarlayabilmelidir.

- **Taşınabilirlik** -bulutta, şirket içinde farklı ortamlarda kolayca çalışır.

- **İlke** tutarlılığı-çeşitli kaynaklar arasında ilke tanımında tutarlılık.


### <a name="capabilities"></a>Özellikler

İstio, aşağıdaki özellik kümesini sağlar:

- **Ağ-ağ** geçitleri (çoklu küme), sanal makineler (kafes genişletmesi)

- **Trafik yönetimi** – yönlendirme, bölme, zaman aşımları, devre kesiciler, yeniden denemeler, giriş, çıkış

- **İlke** – erişim denetimi, hız sınırı, kota, özel ilke bağdaştırıcıları

- **Güvenlik** – kimlik doğrulaması (JWT), yetkilendirme, şifreleme (MTLS), dış CA (HashiCorp Kasası)

- **Observability** – altın ölçümler, yansıtma, izleme, özel bağdaştırıcılar, Prometheus, grafana

### <a name="scenarios"></a>Senaryolar

İstio, aşağıdaki senaryolar için uygundur ve önerilir:

- Genişletilebilirlik ve zengin özellik kümesi gerektir

- VM tabanlı iş yüklerini içerecek şekilde ağ genişletmesi

- Çoklu küme hizmeti ağı

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgelerde, Azure Kubernetes Service (AKS) üzerine Istio 'u nasıl yükleyebileceğiniz açıklanmaktadır:

> [!div class="nextstepaction"]
> [Azure Kubernetes Service (AKS) ' de Istio 'yu kurma][istio-install]

Ayrıca, Istio kavramlarını ve ek dağıtım modellerini de inceleyebilirsiniz:

- [İstio kavramları][what-is-istio]
- [İstio dağıtım modelleri][deployment-models]

<!-- LINKS - external -->
[istio]: https://istio.io
[what-is-istio]: https://istio.io/docs/concepts/what-is-istio/
[design-goals]: https://istio.io/docs/concepts/what-is-istio/#design-goals
[deployment-models]: https://istio.io/docs/concepts/deployment-models/

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[istio-install]: ./servicemesh-istio-install.md
