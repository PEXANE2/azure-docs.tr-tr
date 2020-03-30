---
title: Servis meshes hakkında
description: Hizmet meshes, mimari ve yetenekleri ve dağıtmak için birini seçerken göz önünde bulundurmanız gereken ölçütler genel bir bakış elde edin.
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: eca49a3fac1ea0398ebe1d05bde20fbca3c81232
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594320"
---
# <a name="about-service-meshes"></a>Servis meshes hakkında

Hizmet kafesi, trafik yönetimi, esneklik, ilke, güvenlik, güçlü kimlik ve iş yüklerinize uyulabilirlik gibi özellikler sağlar. Uygulamanız bu operasyonel yeteneklerden ayrılmış ve servis örgüsü bunları uygulama katmanının dışına ve altyapı katmanına taşır.

## <a name="scenarios"></a>Senaryolar

Bunlar, bir hizmet kafesi kullandığınızda iş yükleri için etkinleştirilen senaryolardan bazılarıdır:

- **Kümedeki tüm trafiği şifreleyin** - Kümede belirtilen hizmetler arasında karşılıklı TLS etkinleştirin. Bu, ağ çevresinde giriş ve çıkış alabilirsiniz. Uygulama kodu ve altyapı için değişiklik gerekmeden varsayılan olarak güvenli bir seçenek sağlar.

- **Kanarya ve aşamalı rollouts** - Kümedeki yeni hizmetler kümesine yönlendirilecek bir trafik alt kümesi için koşulları belirtin. Kanarya salınımı başarılı test, koşullu yönlendirme ve faz yavaş yavaş yeni hizmet için tüm trafiğin % artan kaldırın. Sonunda tüm trafik yeni hizmete yönlendirilecektir.

- **Trafik yönetimi ve işleme** - Tüm trafiği belirli bir kaynaktan gelen bir hizmetin sürümüyle sınırlandıracak bir hizmet ilkesi oluşturun. Veya belirtilen hizmetler arasındaki hata sınıflarına yeniden deneme stratejisi uygulayan bir ilke. Geçiş sırasında veya hata ayıklama sorunları sırasında hizmetlerin yeni sürümlerine canlı trafiği yansıtın. Esnekliği test etmek için test ortamındaki hizmetler arasında hatalar enjekte edin.

- **Gözlemlenebilirlik** - Hizmetlerinizin aralarında akan trafiği nasıl birbirine bağladığını öğrenin. Kümedeki tüm trafik ve giriş/çıkış için ölçümler, günlükler ve izlemeler edinin. Uygulamalarınız için dağıtılmış izleme yetenekleri ekleyin.

## <a name="architecture"></a>Mimari

Servis ağı genellikle bir denetim düzlemi ve veri düzleminden oluşur.

**Denetim düzlemi,** hizmet örgüsünün yönetilmesini destekleyen bir dizi bileşene sahiptir. Bu genellikle bir UI veya API olabilir bir yönetim arabirimi içerir. Genellikle, hizmet örgüsünden belirli yetenekleri nasıl uygulayacağını tanımlayan kural ve ilke tanımlarını yöneten bileşenler de olacaktır. Güçlü kimlik ve mTLS sertifikaları gibi güvenlik yönlerini yöneten bileşenler de vardır. Hizmet meshes da genellikle toplar ve iş yüklerinden ölçümleri ve telemetri toplar gözlemlenebilirlik bileşeni vardır.

**Veri düzlemi** genellikle iş yükleri için bir sidecar olarak şeffaf enjekte bir proxy oluşur. Bu proxy, iş yükünüzü içeren bölmeye girip çıkan tüm ağ trafiğini denetlemek üzere yapılandırılır. Bu, proxy'nin mTLS üzerinden trafiği güvenli hale getirmek, trafiği dinamik olarak yönlendirmek, trafiğe ilke uygulamak ve ölçümleri toplamak ve bilgileri izlemek için yapılandırılmasına olanak tanır. 

![Tipik servis kafes mimarisi](media/servicemesh/typical-architecture.png)

## <a name="capabilities"></a>Özellikler

Hizmet meshes her doğal bir uyum var ve belirli senaryoları destekleyen odaklanmak, ancak genellikle çoğu aşağıdaki yetenekleri, hepsi değilse de bir dizi uygulayacak bulacaksınız.

### <a name="traffic-management"></a>Trafik yönetimi 

- **Protokol** – katman 7 (http, grpc)
- **Dinamik Yönlendirme** – koşullu, ağırlık, yansıtma
- **Esneklik** – zaman ekmeleri, yeniden denemeler, devre kesiciler
- **İlke** – erişim kontrolü, fiyat limitleri, kotalar
- Test - arıza **enjeksiyonu**

### <a name="security"></a>Güvenlik

- **Şifreleme** – mTLS, sertifika yönetimi, harici CA
- **Güçlü Kimlik** – SPIFFE veya benzeri
- **Auth** – kimlik doğrulama, yetkilendirme

### <a name="observability"></a>Gözlemlenebilirlik

- **Ölçümler** – altın ölçümler, prometheus, grafana
- **İzleme** - iş yükleri arasında izleme
- **Trafik** – küme, giriş/çıkış

### <a name="mesh"></a>Örgü

- **Desteklenen Bilgi İşlem** - Kubernetes, sanal makineler
- **Çok kümeli** - ağ geçitleri, federasyon

## <a name="selection-criteria"></a>Seçim kriterleri

Bir hizmet kafesi seçmeden önce, gereksinimlerinizi ve hizmet ağı yükleme nedenlerini anladığınızdan emin olun. Aşağıdaki soruları sormayı deneyin.

- **Ingress Controller benim ihtiyaçlarım için yeterli mi?** - Bazen a/b testi veya girişte trafik bölme gibi bir yeteneğe sahip olmak, gerekli senaryoyu desteklemek için yeterlidir. Ortamınıza hiçbir ters yol olmadan karmaşıklık eklemeyin.

- **İş yüklerim ve çevrem ek genel giderlere tahammül edebilir mi?** - Hizmet örgüsi desteklemek için gereken tüm ek bileşenler işlemci ve bellek gibi ek kaynaklar gerektirir. Buna ek olarak, tüm eksekler ve ilişkili ilke denetimleri trafiğinize gecikme sonu ekler. Gecikmeye karşı çok hassas iş yüklerin varsa veya hizmet kafesbileşenlerini kapsayacak ek kaynakları sağlayamazsa, yeniden düşünün.

- **Bu gereksiz yere ek karmaşıklık ekliyor mu?** - Bir servis örgüsünün yüklenmesinin nedeni, işletme veya operasyonel ekipler için kritik olmayan bir özellik elde etmekse, yükleme, bakım ve yapılandırmanın ek karmaşıklığının buna değip değmeyeceğini düşünün.

- **Bu artımlı bir yaklaşımla benimsenebilir mi?** - Birçok yetenek sağlayan hizmet meshes bazıları daha artımlı bir yaklaşım benimsenebilir. Başarınızı sağlamak için gereken bileşenleri yükleyin. Bir kez daha emin ve ek yetenekleri gereklidir, o zaman bu keşfetmek. Başından itibaren *her şeyi* yükleme dürtüsüne karşı koy.

Dikkatli bir değerlendirmeden sonra, gerekli yetenekleri sağlamak için bir hizmet örgüse ihtiyacınız olduğuna karar verirseniz, bir sonraki kararınız *hangi hizmet örgüsi?*

Aşağıdaki alanları ve bunlardan hangisinin gereksinimlerinize en uygun olduğunu göz önünde bulundurun. Bu, çevrenize ve iş yüklerinize en uygun olduğunuza doğru size rehberlik edecektir. [Sonraki adımlar](#next-steps) bölümünde, belirli hizmet meshes ve nasıl bu alanlarda eşleniniz hakkında daha ayrıntılı bilgi için götürecektir.

- **Teknik** - trafik yönetimi, politika, güvenlik, gözlemlenebilirlik

- **İş** - ticari destek, vakıf (CNCF), OSS lisansı, yönetişim

- **Operasyonel** – kurulum/yükseltmeler, kaynak gereksinimleri, performans gereksinimleri, entegrasyonlar (ölçümler, telemetri, panolar, araçlar, SMI), karma iş yükleri (Linux ve Windows düğüm havuzları), bilgi işlem (Kubernetes, sanal makineler), çoklu küme

- **Güvenlik** - auth, kimlik, sertifika yönetimi ve rotasyon, takılabilir harici CA


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, Azure Kubernetes Hizmeti 'nde (AKS) deneyebileceğiniz hizmet meshes hakkında daha fazla bilgi sağlar:

> [!div class="nextstepaction"]
> [Istio hakkında daha fazla bilgi edinin ...][istio-about]

> [!div class="nextstepaction"]
> [Linkerd hakkında daha fazla bilgi edinin ...][linkerd-about]

> [!div class="nextstepaction"]
> [Konsolos hakkında daha fazla bilgi edinin ...][consul-about]

Ayrıca, Kubernetes'teki hizmet kafesleri için standart bir arayüz olan Service Mesh Interface'i (SMI) da keşfetmek isteyebilirsiniz:

- [Hizmet Örgü Arabirimi (SMI)][smi]


<!-- LINKS - external -->
[smi]: https://smi-spec.io/

<!-- LINKS - internal -->
[istio-about]: ./servicemesh-istio-about.md
[linkerd-about]: ./servicemesh-linkerd-about.md
[consul-about]: ./servicemesh-consul-about.md