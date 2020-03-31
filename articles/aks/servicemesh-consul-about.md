---
title: Konsolosa Genel Bakış
description: Konsoloshakkında genel bilgi edinin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: c518985b360fa3264bd5ac1e3fe76d61b2810b9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594218"
---
# <a name="consul"></a>Konsül

## <a name="overview"></a>Genel Bakış

[Konsolos,][consul] çalışma zamanı platformlarında hizmetleri bağlamak ve güvence altına almak için çok veri merkezi bilinçli hizmet ağı çözümüdür. [Connect,][consul-features] hizmet kafesi özellikleri sağlayan bileşendir.

## <a name="architecture"></a>Mimari

Konsolos varsayılan olarak [Elçi][envoy-proxy]tabanlı [sidecars][consul-sidecar] oluşan bir veri düzlemi sağlar. Konsolosun takılabilir bir proxy mimarisi vardır. Bu akıllı yakınlıklar, meshed uygulamalarınıza ve iş yüklerinizdeki tüm ağ trafiğini kontrol eder.

Denetim düzlemi yapılandırmayı ve ilkeyi aşağıdaki [bileşenler][consul-architecture]aracılığıyla yönetir:

- **Sunucu** - Consul küme durumunu koruyan Sunucu modunda çalışan bir Konsolos Aracısı.

- **Müşteri** - Hafif Müşteri Modunda Çalışan Bir Konsolos Temsilcisi. Her işlem düğümünde çalışan bir Müşteri aracısı olmalıdır. Bu istemci, iş yükleri ve Konsolos yapılandırması arasında yapılandırma ve ilke aracılık eder. 

Aşağıdaki mimari diyagram, veri düzlemi ve kontrol düzlemindeki çeşitli bileşenlerin nasıl etkileştiğini gösterir.

![Konsül bileşenlerine ve mimarisine genel bakış.](media/servicemesh/consul/about-architecture.png)


## <a name="selection-criteria"></a>Seçim kriterleri

Konsolosu iş yüklerinizle değerlendirirken aşağıdaki alanları anlamak ve göz önünde bulundurmak önemlidir:

- [Konsolos İlkeleri](#consul-principles)
- [Özellikler](#capabilities)
- [Senaryolar](#scenarios)


### <a name="consul-principles"></a>Konsolos ilkeleri

Aşağıdaki ilkeler Konsolos projesine [rehberlik edin:][consul-principles]

- **API-Driven** - Tüm yapılandırma ve ilke kodlar.

- **Her Yerde Çalıştır ve Bağlan** - İş yüklerini çalışma zamanı platformlarında (Kubernetes, VM'ler, Serverless) bağlayın.

- **Genişlet ve Tümleştir** - Altyapı daki iş yüklerini güvenli bir şekilde bağlayın.


### <a name="capabilities"></a>Özellikler

Konsolos aşağıdaki yetenekler kümesini sağlar:

- **Mesh** – ağ geçidi (çoklu veri merkezi), sanal makineler (küme düğümleri dışında), hizmet eşitleme, hata ayıklama seçeneği yerleşik

- **Proxy'ler** – Elçi, yerleşik proxy, takılabilir, Windows iş yükleri için kullanılabilir l4 proxy

- **Trafik Yönetimi** – yönlendirme, bölme, çözüm

- **Politika** – niyetler, ALA'lar

- **Güvenlik** – yetkilendirme, kimlik doğrulama, şifreleme, SPIFFE tabanlı kimlikler, harici CA (Vault), sertifika yönetimi ve döndürme

- **Gözlemlenebilirlik** – ölçümler, ui pano, prometheus, grafana


### <a name="scenarios"></a>Senaryolar

Konsolos aşağıdaki senaryolar için uygundur ve önerilmektedir:

- Mevcut Konsolos bağlantılı iş yüklerinin genişletilmesi

- Sertifika yönetimi yle ilgili uyumluluk gereksinimleri

- Çok kümeli hizmet örgü

- VM tabanlı iş yükleri hizmet örgüse dahil edilecek



## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, Azure Kubernetes Hizmetine (AKS) Konsolos'u nasıl yükleyebileceğinizi açıklar:

> [!div class="nextstepaction"]
> [Azure Kubernetes Hizmetine Konsolos Yükle (AKS)][consul-install]

Ayrıca Konsolos özelliklerini ve mimarisini daha fazla keşfedebilirsiniz:

- [Konsolos Özellikleri][consul-features]
- [Konsolosluk Mimarisi][consul-architecture]
- [Konsolos - Connect Nasıl Çalışır?][consul-how-connect-works]

<!-- LINKS - external -->
[consul]: https://www.consul.io/mesh.html
[consul-features]: https://www.consul.io/docs/connect/index.html
[consul-architecture]: https://www.consul.io/docs/internals/architecture.html
[consul-sidecar]: https://www.consul.io/docs/connect/proxies.html
[consul-how-connect-works]: https://www.consul.io/docs/connect/connect-internals.html
[consul-principles]: https://www.consul.io/

[envoy-proxy]: https://www.envoyproxy.io/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[consul-install]: ./servicemesh-consul-install.md
