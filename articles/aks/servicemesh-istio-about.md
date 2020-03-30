---
title: Istio'ya Genel Bakış
description: Istio hakkında genel bilgi edinin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 8518e30a54c2486abf84cd9ac026cc4dccb3fa84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593909"
---
# <a name="istio"></a>Istio

## <a name="overview"></a>Genel Bakış

[Istio][istio] tam özellikli, özelleştirilebilir ve genişletilebilir hizmet örgü.

## <a name="architecture"></a>Mimari

Istio, [Elçi][envoy-proxy]tabanlı sidecars oluşan bir veri düzlemi sağlar. Bu akıllı yakınlıklar, meshed uygulamalarınıza ve iş yüklerinizdeki tüm ağ trafiğini kontrol eder.

Kontrol düzlemi yapılandırmayı, ilkeyi ve telemetriyi aşağıdaki [bileşenler][what-is-istio]aracılığıyla yönetir:

- **Mikser** - Erişim denetimi ve kullanım ilkelerini uygular. [Prometheus][prometheus]içine itilir vekillerden telemetri toplar.

- **Pilot** - Vekiller için servis bulma ve trafik yönetimi politikası/yapılandırması sağlar.

- **Citadel** - Hizmetler arasında mTLS'ye izin veren kimlik ve güvenlik özellikleri sağlar.

- **Kadırga** - Özetler ve bileşenlere yapılandırma sağlar.

Aşağıdaki mimari diyagram, veri düzlemi ve kontrol düzlemindeki çeşitli bileşenlerin nasıl etkileştiğini gösterir.


![Istio bileşenlerine ve mimarisine genel bakış.](media/servicemesh/istio/about-architecture.png)


## <a name="selection-criteria"></a>Seçim kriterleri

İş yükleriniz için Istio'yu değerlendirirken aşağıdaki alanları anlamak ve göz önünde bulundurmak önemlidir:

- [Tasarım Hedefleri](#design-goals)
- [Özellikler](#capabilities)
- [Senaryolar](#scenarios)


### <a name="design-goals"></a>Tasarım hedefleri

Aşağıdaki tasarım hedefleri Istio projesine [yol gösterin:][design-goals]

- **Saydamlığı En Üst Düzeye Çıkarın** - Sistemden gerçek değer elde etmek için minimum çalışma miktarıyla benimsenmesine izin verin.

- **Genişletilebilirlik** - Büyümek ve değişen ihtiyaçlarına uyum sağlayabilmeli.

- **Taşınabilirlik** - Bulut, şirket içi gibi farklı ortamlarda kolayca çalıştırın.

- **İlke Tekdüzeliği** - Çeşitli kaynaklar arasında ilke tanımında tutarlılık.


### <a name="capabilities"></a>Özellikler

Istio aşağıdaki yetenekleri sağlar:

- **Mesh** – ağ geçitleri (çoklu küme), sanal makineler (örgü genişletme)

- **Trafik Yönetimi** – yönlendirme, bölme, zaman aşımları, devre kesiciler, yeniden denemeler, giriş, çıkış

- **İlke** – erişim kontrolü, fiyat sınırı, kota, özel ilke bağdaştırıcıları

- **Güvenlik** – kimlik doğrulama (jwt), yetkilendirme, şifreleme (mTLS), harici CA (HashiCorp Vault)

- **Gözlemlenebilirlik** – altın ölçümler, ayna, izleme, özel adaptörler, prometheus, grafana

### <a name="scenarios"></a>Senaryolar

Istio aşağıdaki senaryolar için uygundur ve önerilmektedir:

- Genişletilebilirlik ve zengin yetenekler kümesi gerektirir

- VM tabanlı iş yüklerini içerecek şekilde kafes genişletme

- Çok kümeli hizmet örgü

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, Istio'yu Azure Kubernetes Hizmetine (AKS) nasıl yükleyebileceğinizi açıklar:

> [!div class="nextstepaction"]
> [Azure Kubernetes Hizmetinde (AKS) Istio'ya Yükleme][istio-install]

Ayrıca Istio kavramlarını ve ek dağıtım modellerini de keşfedebilirsiniz:

- [Istio Kavramlar][what-is-istio]
- [Istio Dağıtım Modelleri][deployment-models]

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
