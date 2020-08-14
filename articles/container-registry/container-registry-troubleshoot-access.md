---
title: Kayıt defteri ile ağ sorunlarını giderme
description: Bir sanal ağda veya bir güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişirken oluşan belirtiler, nedenler ve çözümler
ms.topic: article
ms.date: 08/11/2020
ms.openlocfilehash: 227eeeadb2aef4b4d3feb7923a198b129a6267d3
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88227534"
---
# <a name="troubleshoot-network-issues-with-registry"></a>Kayıt defteri ile ağ sorunlarını giderme

Bu makale, bir sanal ağda veya güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişirken karşılaşabileceğiniz sorunları gidermenize yardımcı olur. 

## <a name="symptoms"></a>Belirtiler

Aşağıdakilerden birini veya daha fazlasını içerebilir:

* Görüntüler gönderilemiyor veya çekilemiyor ve hata alıyorsunuz `dial tcp: lookup myregistry.azurecr.io`
* Görüntüler gönderilemiyor veya çekilemiyor ve Azure CLı hatası alıyorsunuz `Could not connect to the registry login server`
* Görüntüler kayıt defterinden Azure Kubernetes hizmetine veya başka bir Azure hizmetine çekilemiyor
* HTTPS proxy arkasındaki bir kayıt defterine erişilemiyor ve hata alıyorsunuz `Error response from daemon: login attempt failed with status: 403 Forbidden`
* Azure portal kayıt defteri ayarlarına erişilemiyor veya Azure CLı kullanarak kayıt defterini yönetme
* Sanal ağ ayarları veya genel erişim kuralları eklenemiyor veya değiştirilemiyor
* ACR görevleri görüntüleri alamıyor veya çekmiyor
* Azure Güvenlik Merkezi, kayıt defterindeki görüntüleri tarayamaz veya tarama sonuçları Azure Güvenlik Merkezi 'nde görünmüyor

## <a name="causes"></a>Nedenler

* Bir istemci güvenlik duvarı veya proxy, erişim [çözümünü](#configure-client-firewall-access) engelliyor
* Kayıt defterindeki genel ağ erişim kuralları erişimi engelliyor- [çözüm](#configure-public-access-to-registry)
* Sanal ağ yapılandırması erişimi engelliyor- [çözüm](#configure-vnet-access)
* Azure Güvenlik Merkezi 'ni özel uç nokta veya hizmet uç noktası olan bir kayıt defteriyle tümleştirmeye çalıştığınızda [çözüm](#configure-image-scanning-solution)

## <a name="further-diagnosis"></a>Daha fazla tanılama 

Kayıt defteri ortamının sistem durumu ve isteğe bağlı olarak bir hedef kayıt defterine erişim hakkında daha fazla bilgi almak için [az ACR Check-Health](/cli/azure/acr#az-acr-check-health) komutunu çalıştırın. Örneğin, belirli ağ bağlantısını veya yapılandırma sorunlarını tanılayın. 

Bkz. komut örnekleri için [Azure Container Registry 'nin sistem durumunu denetleme](container-registry-check-health.md) . Hatalar raporlandıysanız, önerilen çözümler için [hata başvurusunu](container-registry-health-error-reference.md) ve aşağıdaki bölümleri gözden geçirin.

> [!NOTE]
> Bazı ağ bağlantı belirtileri, kayıt defteri kimlik doğrulamasıyla veya yetkilendirmeyle ilgili sorunlar olduğunda da gerçekleşebilir. Bkz. [kayıt defteri oturum açma sorunlarını giderme](container-registry-troubleshoot-login.md).

## <a name="potential-solutions"></a>Olası çözümler

### <a name="configure-client-firewall-access"></a>İstemci güvenlik duvarı erişimini yapılandırma

Bir istemci güvenlik duvarı veya proxy sunucusunun arkasındaki bir kayıt defterine erişmek için, güvenlik duvarı kurallarını kayıt defterinin REST ve veri uç noktalarına erişecek şekilde yapılandırın. [Adanmış veri uç noktaları](container-registry-firewall-access-rules.md#enable-dedicated-data-endpoints) etkinleştirilirse, erişim kuralları gerekir:

* REST uç noktası: `<registryname>.azurecr.io`
* Veri uç noktaları: `<registry-name>.<region>.data.azurecr.io`

Coğrafi olarak çoğaltılan bir kayıt defteri için, her bölgesel çoğaltma için veri uç noktasına erişimi yapılandırın.

HTTPS proxy 'nin arkasında, hem Docker istemciniz hem de Docker Daemon 'ın proxy davranışı için yapılandırıldığından emin olun.

ContainerRegistryLoginEvents tablosundaki kayıt defteri kaynak günlükleri, engellenen bir bağlantıyı tanılamanıza yardımcı olabilir.

İlgili bağlantılar:

* [Güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırma](container-registry-firewall-access-rules.md)
* [HTTP/HTTPS proxy yapılandırması](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
* [Azure Container Registry coğrafi replicationın](container-registry-geo-replication.md)
* [Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri](container-registry-diagnostics-audit-logs.md)

### <a name="configure-public-access-to-registry"></a>Kayıt defterine genel erişimi yapılandırma

Internet üzerinden bir kayıt defterine erişiyorsanız, kayıt defterinin istemcinizden ortak ağ erişimine izin verdiğini doğrulayın. Varsayılan olarak, bir Azure Container Registry, tüm ağlardan ortak kayıt defteri uç noktalarına erişim sağlar. Kayıt defteri, seçilen ağlara veya seçili IP adreslerine erişimi sınırlayabilir. 

Kayıt defteri bir hizmet uç noktası olan bir sanal ağ için yapılandırılmışsa, genel ağ erişimini devre dışı bırakmak hizmet uç noktası üzerinden erişimi de devre dışı bırakır. Kayıt defteriniz özel bağlantı içeren bir sanal ağ için yapılandırılmışsa, IP ağ kuralları kayıt defterinin özel uç noktaları için uygulanmaz. 

İlgili bağlantılar:

* [Genel IP ağ kurallarını yapılandırma](container-registry-access-selected-networks.md)
* [Azure özel bağlantısını kullanarak bir Azure Container Registry 'ye özel olarak bağlanma](container-registry-private-link.md)
* [Bir Azure sanal ağında hizmet uç noktası kullanarak bir kapsayıcı kayıt defterine erişimi kısıtlama](container-registry-vnet.md)


### <a name="configure-vnet-access"></a>VNet erişimini yapılandırma

Sanal ağın özel bağlantı için özel bir uç nokta veya bir hizmet uç noktası (Önizleme) ile yapılandırıldığını doğrulayın. Şu anda bir Azure savunma uç noktası desteklenmiyor.

Ağdaki diğer kaynaklardan gelen trafiği kayıt defterine sınırlamak için kullanılan NSG kurallarını ve hizmet etiketlerini gözden geçirin. 

Kayıt defterine yönelik bir hizmet uç noktası yapılandırılmışsa, bu ağ alt ağından erişime izin veren bir ağ kuralının kayıt defterine eklendiğini doğrulayın. Hizmet uç noktası yalnızca ağdaki sanal makinelerden ve AKS kümelerinden erişimi destekler.

Ağda Azure Güvenlik Duvarı veya benzer bir çözüm yapılandırılmışsa, bir AKS kümesi gibi diğer kaynaklardan gelen çıkış trafiğinin kayıt defteri uç noktalarına ulaşmak için etkinleştirildiğini denetleyin.

İlgili bağlantılar:

* [Azure özel bağlantısını kullanarak bir Azure Container Registry 'ye özel olarak bağlanma](container-registry-private-link.md)
* [Bir Azure sanal ağında hizmet uç noktası kullanarak bir kapsayıcı kayıt defterine erişimi kısıtlama](container-registry-vnet.md)
* [AKS kümeleri için gerekli giden ağ kuralları ve FQDN 'Ler](../aks/limit-egress-traffic.md#required-outbound-network-rules-and-fqdns-for-aks-clusters)
* [Kubernetes: DNS çözümlemesinde hata ayıklama](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)
* [Sanal ağ hizmeti etiketleri](../virtual-network/service-tags-overview.md)

### <a name="configure-image-scanning-solution"></a>Görüntü tarama çözümünü Yapılandır

Kayıt defteriniz özel bir uç nokta veya hizmet uç noktasıyla yapılandırılmışsa, şu anda görüntü tarama için Azure Güvenlik Merkezi ile tümleştirilemiyor. İsteğe bağlı olarak, Azure Marketi 'nde bulunan diğer görüntü tarama çözümlerini şu şekilde yapılandırın:

* [Deniz mavisi Cloud Native Security platformu](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security)
* [Twistlock Enterprise sürümü](https://azuremarketplace.microsoft.com/marketplace/apps/twistlock.twistlock)

İlgili bağlantılar:

* [Azure Container Registry resim taramasını Güvenlik Merkezi 'Ne göre](../security-center/azure-container-registry-integration.md)
* [Geri bildirim](https://feedback.azure.com/forums/347535-azure-security-center/suggestions/41091577-enable-vulnerability-scanning-for-images-that-are) sağlayın


## <a name="advanced-troubleshooting"></a>Gelişmiş sorun giderme

Kayıt defterinde [kaynak günlüklerinin toplanması](container-registry-diagnostics-audit-logs.md) etkinse, Kirınterregistryloginevents günlüğünü gözden geçirin. Bu günlük, gelen kimlik ve IP adresi dahil olmak üzere kimlik doğrulama olaylarını ve durumunu depolar. [Kayıt defteri kimlik doğrulama hatalarıyla](container-registry-diagnostics-audit-logs.md#registry-authentication-failures)ilgili günlüğü sorgulayın. 

İlgili bağlantılar:

* [Tanılama değerlendirmesi ve denetimi günlükleri](container-registry-diagnostics-audit-logs.md)
* [Kapsayıcı kayıt defteri SSS](container-registry-faq.md)
* [Azure Container Registry için Azure Güvenlik temeli](security-baseline.md)
* [Azure Container Registry için en iyi yöntemler](container-registry-best-practices.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada çözemezseniz, aşağıdaki seçeneklere bakın.

* Diğer kayıt defteri sorunlarını giderme konuları şunları içerir:
  * [Kayıt defteri oturum açma sorunlarını giderme](container-registry-troubleshoot-login.md) 
  * [Kayıt defteri performansının sorunlarını giderme](container-registry-troubleshoot-performance.md)
* [Topluluk desteği](https://azure.microsoft.com/support/community/) seçenekleri
* [Microsoft Soru-Cevap](https://docs.microsoft.com/answers/products/)
* [Destek bileti açma](https://azure.microsoft.com/support/create-ticket/)


