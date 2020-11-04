---
title: Sanal WAN için Azure Güvenlik temeli
description: Sanal WAN güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 409c8d163b52e56a1eef68a23e4db79956a68722
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328728"
---
# <a name="azure-security-baseline-for-virtual-wan"></a>Sanal WAN için Azure Güvenlik temeli

Bu güvenlik temeli, [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' dan sanal WAN Microsoft Azure kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve sanal WAN için geçerli olan ilgili kılavuza göre gruplandırılır. Sanal WAN için geçerli olmayan **denetimler** dışlandı.

Sanal WAN 'ın Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için [tam sanal WAN güvenlik taban çizgisi eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: iç trafik için güvenlik uygulama

**Kılavuz** : Azure sanal WAN, doğrudan bir sanal ağa dağıtımı desteklemez. Ancak, bağlı olan kaynaklarda ağ güvenlik grubu kuralları uygulayabilir, Azure Güvenlik duvarının korumalarının kullanılması veya özel trafiğe izin vermek veya bu trafiği engellemek için özel rota tabloları oluşturmanız da gerekebilir.

- [Özel yönlendirme senaryoları](scenario-any-to-any.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ns-2-connect-private-networks-together"></a>NS-2: özel ağları birbirine bağlama 

**Rehberlik** : bir birlikte bulundurma ortamında Azure veri merkezleri ile şirket içi altyapı arasında özel bağlantılar oluşturmak Için Azure ExpressRoute veya Azure sanal özel ağ (VPN) kullanın. ExpressRoute bağlantıları, genel İnternet üzerinden geçmez, tipik İnternet bağlantılarına göre daha fazla güvenilirlik, daha hızlı hız ve daha düşük gecikme süresi sunar. 

Noktadan siteye VPN ve siteden siteye VPN için, bu VPN seçeneklerinin ve ExpressRoute 'un herhangi bir birleşimini kullanarak şirket içi cihazları veya ağları bir sanal ağa bağlayın. İki veya daha fazla sanal ağı Azure 'da birlikte bağlamak için sanal ağ eşlemesi ' ni kullanın. Eşlenen sanal ağlar arasındaki ağ trafiği özeldir ve Azure omurga ağında tutulur.

- [Sanal WAN 'da ExpressRoute](virtual-wan-expressroute-portal.md)

- [Siteden siteye VPN 'ye Genel Bakış](virtual-wan-site-to-site-portal.md)

- [Noktadan siteye VPN 'ye Genel Bakış](virtual-wan-point-to-site-portal.md)

- [Özel Bağlantı](howto-private-link.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : paylaşılan

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Azure hizmetlerine özel ağ erişimi oluşturma

**Rehberlik** : internet 'ten çıkmadan sanal ağlarınızdan Azure sanal WAN 'a özel erişimi etkinleştirmek Için Azure özel bağlantısı 'nı kullanın. Özel erişim, Azure hizmetleri tarafından sunulan kimlik doğrulama ve trafik güvenliğine ek olarak, derinlemesine bir savunma ölçümüdür.

- [Azure özel bağlantısını anlama](../private-link/private-link-overview.md)

- [Sanal WAN özel bağlantısı](howto-private-link.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: uygulamaları ve Hizmetleri dış ağ saldırılarına karşı koruma

**Rehberlik** : dağıtılmış hizmet reddi (DDoS) saldırıları, uygulamaya özgü saldırılar ve istenmeyen ve potansiyel olarak kötü amaçlı internet trafiği dahil olmak üzere dış ağlardan gelen saldırılara karşı Azure sanal WAN kaynaklarınızı koruyun. 

Uygulamaları ve Hizmetleri Internet 'ten ve diğer dış konumlardan gelen kötü amaçlı olabilecek trafiğe karşı korumak için Azure Güvenlik Duvarı 'nı kullanın. Azure sanal ağlarınızdaki saldırılara karşı, varlıklarınız için Azure DDoS Koruması ' nı seçin. Ağınızla ilgili kaynaklarınızla ilgili yanlış yapılandırma risklerini algılamak için Azure Güvenlik Merkezi 'ni kullanın.

- [Azure Güvenlik Duvarı belgeleri](/azure/firewall)

- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](/azure/virtual-network/manage-ddos-protection) 

- [Azure Güvenlik Merkezi önerileri](../security-center/recommendations-reference.md#recs-network)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: izinsiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz** : Azure sanal WAN 'da yetkisiz giriş algılama veya önleme sistemleri dağıtmanın en iyi yolu, sanal hub 'a bağlı olan bağlı olan ağlarda bir ağ sanal gereci kullanmaktır.  Daha fazla bilgi, başvurulan bağlantıların yönlendirme senaryolarında bulunabilir. 

- [Senaryo - Trafiği bir NVA aracılığıyla yönlendirme](scenario-route-through-nva.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: ağ güvenlik kurallarını basitleştirme

**Kılavuz** : Azure ağ güvenlik gruplarında veya sanal WAN kaynaklarınız için yapılandırılmış Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak Için Azure sanal ağ hizmeti etiketlerini kullanın. 

Güvenlik kuralları oluştururken belirli IP adreslerinin yerine hizmet etiketleri kullanın. Bir kuralın uygun kaynak veya hedef alanındaki hizmet etiketi adını (örneğin: {VirtualWAN: sanal ağ}) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: güvenli etki alanı adı hizmeti (DNS)

**Kılavuz** : Azure sanal WAN, Noktadan siteye VPN ağ geçitleri IÇIN özel DNS sunucuları sunar. 

DNS güvenliği için en iyi uygulamaları izleyin DNS, DNS yükseltme saldırıları, DNS kirletme ve yanıltma gibi yaygın saldırıları azaltır.

Yetkili DNS hizmetiniz olarak Azure DNS kullanıldığında, DNS bölgelerinin ve kayıtlarının Azure rol tabanlı erişim denetimi (Azure RBAC) ve kaynak kilitleri kullanılarak yanlışlıkla veya kötü amaçlı değişikliklere karşı korunduğundan emin olun.

- [Azure DNS genel bakış](../dns/dns-overview.md) 

- [Güvenli etki alanı adı sistemi (DNS) dağıtım kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) 

- [DNS girişlerinin tehlikini önleyin ve alt etki alanı devralmayı önleyin](../security/fundamentals/subdomain-takeover.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>Im-1: merkezi kimlik ve kimlik doğrulama sistemi olarak Azure Active Directory standartlaştırma

**Kılavuz** : Azure sanal WAN 'ın Noktadan siteye VPN 'i Azure 'un varsayılan kimlik ve erişim yönetimi hizmeti olan Azure Active Directory (Azure AD) ile tümleşiktir. Kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'yi standartlaşmanız gerekir:

- Azure portal, Azure depolama, Azure sanal makineleri (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft Bulut kaynaklar.
- Kuruluşunuzun Azure veya kurumsal ağ kaynaklarınızın uygulamaları gibi kaynakları.

Kuruluşunuzun bulut güvenliği pratikte Azure AD 'yi yüksek öncelikli olarak güvenli hale getirin. Yapılandırmanızın en iyi yöntem önerilerini nasıl benzedireceğini ölçmek için Azure Güvenlik Merkezi 'nin güvenlik puanı özelliği ile kimliğinizi ve güvenlik duruşunuzu değerlendirin. Güvenlik durinize yönelik iyileştirmeler için Microsoft 'un en iyi yöntem önerilerini uygulayın.

Azure AD Ayrıca, Microsoft hesabı olmayan kullanıcıların kendi uygulamalarına ve kaynaklarına kendi dış kimlikleriyle oturum açmasını sağlayan dış kimlikleri de destekler. Başvurulan bağlantılarda Noktadan siteye VPN senaryolarında Azure AD kullanma hakkındaki bilgileri gözden geçirin.

- [P2S OpenVPN Protokolü bağlantıları için Azure Active Directory (AD) kiracısı oluşturma](openvpn-azure-ad-tenant-multi-app.md)

- [Kullanıcı VPN için Azure Active Directory kimlik doğrulamasını yapılandırma](virtual-wan-point-to-site-azure-ad.md)

- [Azure Active Directory kiracı](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>Im-4: tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulama denetimleri kullanma

**Kılavuz** : Azure sanal WAN 'ın Noktadan siteye VPN, çok faktörlü kimlik doğrulaması ile güçlü kimlik doğrulama denetimlerini destekleyen Azure Active Directory (Azure AD) ile tümleşiktir ve güçlü parolasız Yöntemler.

- Çok faktörlü Authentication-Azure AD çok faktörlü kimlik doğrulamasını etkinleştirin ve Azure Güvenlik Merkezi 'nin kimlik ve erişim yönetimi önerilerini izleyerek çok faktörlü kimlik doğrulama kurulumlarınız için en iyi yöntemleri kullanın. Çok faktörlü kimlik doğrulaması, tüm kullanıcılara veya oturum açma koşullarını ve risk etmenlerine göre Kullanıcı başına düzeyinde uygulanabilir.

- Passwordless kimlik doğrulaması – üç adet passwordless kimlik doğrulama seçeneği mevcuttur. Bunlar, Iş için Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri içerir.

Yönetici ve ayrıcalıklı kullanıcılar için, güçlü kimlik doğrulama yönteminin en yüksek düzeyinin kullanıldığından ve ardından uygun güçlü kimlik doğrulama ilkesinin diğer kullanıcılara bir tanesi tarafından kullanıldığından emin olun.

- [Sanal WAN için P2S VPN 'de MFA 'yı etkinleştirme](openvpn-azure-ad-mfa.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: koşullara göre Azure kaynak erişimini kısıtlama

**Kılavuz** : Azure sanal WAN 'ın Noktadan siteye VPN 'i, Kullanıcı tanımlı koşullara göre daha ayrıntılı erişim denetimi IÇIN Azure AD koşullu erişimini destekler. Örneğin, belirli IP aralıklarından kullanıcı oturumlarının, oturum açmak için çok faktörlü kimlik doğrulamasını kullanması gerekir. Ayrıntılı kimlik doğrulama oturumu yönetimi ilkesi, farklı kullanım durumları için de kullanılabilir.

- [Ortak koşullu erişim ilkeleri](../active-directory/conditional-access/concept-conditional-access-policy-common.md) 

- [Koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

- [Sanal WAN P2S VPN koşullu erişimi](openvpn-azure-ad-mfa.md#conditional)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="im-7-eliminate-unintended-credential-exposure"></a>Im-7: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik** : kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

GitHub için, kod içindeki kimlik bilgilerini veya diğer gizli dizileri belirlemek için yerel gizli anahtar tarama özelliğini kullanabilirsiniz.

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html) 

- [GitHub gizli taraması](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

## <a name="privileged-access"></a>Ayrıcalıklı erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Kılavuz** : Azure sanal WAN, iş açısından kritik sistemlere erişimi yalıtmak için Azure rol tabanlı erişim denetimleri (Azure RBAC) kullanır.

Ayrıca, iş açısından kritik sistemlere yüklenen aracılarla birlikte Active Directory Etki Alanı Denetleyicileri, güvenlik araçları ve sistem yönetimi araçları gibi iş açısından kritik erişimlerinize yönetim erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi de kısıtlayın. Bu yönetim ve güvenlik sistemlerine zarar veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access) 

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

**Rehberlik** : güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Yönetim görevleri için yüksek düzeyde güvenli Kullanıcı iş istasyonları veya Azure savunma kullanın. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory (Azure AD), Microsoft Defender Gelişmiş tehdit koruması (ATP) veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri, kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir.

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../active-directory/devices/concept-azure-managed-workstation.md)

- [Ayrıcalıklı erişim iş istasyonu dağıtma](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: geçiş sırasında hassas bilgileri şifreleyin

**Rehberlik** : şifreleme, dış ve genel ağlardaki trafik için kritik öneme sahiptir.

- Erişim denetimlerini kullanın, saldırganların verileri kolayca okuyamadığından veya değiştiremeyeceği için şifreleme kullanılarak ' bant dışı ' saldırılarına (örneğin trafik yakalama) karşı korunması gerekir.

- Azure kaynaklarınıza bağlanan tüm istemcilerin TLS v 1.2 veya üzerini anlaşamayacağı HTTP trafiğinden emin olun.

- Uzaktan Yönetim için şifrelenmemiş protokol yerine SSH (Linux için) veya RDP/TLS (Windows için) kullanın. Kullanımdan çıkarıldı SSL/TLS/SSH sürümleri, protokoller ve zayıf şifrelemeler devre dışı bırakılmalıdır.-

- Azure, temel alınan altyapıda Azure veri merkezleri arasındaki veri trafiği için varsayılan olarak aktarım şifrelemesi ' nde veri sağlar.

Genel olarak, siteden siteye VPN, siteden siteye VPN, Azure ExpressRoute ve Noktadan siteye kullanıcı VPN üzerinden trafiği şifrelemek için güvenli Microsoft omurgası ve fırsatlarıyla ilgili şifreleme sağlıyoruz.

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [TLS güvenliği hakkında bilgi](/security/engineering/solving-tls1-problem)

- [Yoldaki Azure verileri için çift şifreleme](../security/fundamentals/double-encryption.md#data-in-transit)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: varlık yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>-2: güvenlik ekibinin varlık envanterini ve meta verileri erişimine sahip olduğundan emin olun

**Rehberlik** : Azure kaynaklarınıza, kaynak gruplarına ve aboneliklerinize mantıksal olarak bunları bir taksonomi halinde düzenlemek için Etiketler uygulayın. Her etiket bir ad ve bir değer çiftinden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Azure sanal WAN Ayrıca Azure Resource Manager tabanlı kaynak dağıtımlarını ve Azure Kaynak Grafiği sorgularını destekler. 

- [Varlıkları etiketleme hakkında daha fazla bilgi için bkz. Kaynak adlandırma ve etiketleme karar Kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md) 

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../security-center/asset-inventory.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : müşteri

### <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

**Rehberlik** : ortamınızda sağlanabilen hizmetleri kısıtlamak Için Azure ilkesini kullanın. Aboneliklerinizde Azure Resource Graph ile kaynakları sorgulama ve bulma ve onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi kullanma.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli kaynak türlerini reddetme](../governance/policy/samples/built-in-policies.md#general)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik** : "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisi ile etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe kaydetme ve tehdit algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Rehberlik** : Azure Active Directory (Azure AD), daha karmaşık izleme ve analiz kullanım örnekleri IÇIN Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel, SIEM veya izleme araçlarıyla tümleştirilen aşağıdaki kullanıcı günlüklerini sağlar. Bunlar:

- Oturum aç – oturum açma raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.
- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi herhangi bir kaynak üzerinde yapılan değişiklikler verilebilir.
- Riskli oturum açma-riskli oturum açma, bir kullanıcı hesabının meşru sahibi olmayan birisi tarafından gerçekleştirilmiş olabilecek oturum açma girişimine yönelik bir göstergedir.
- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Abonelikte kullanımdan kaldırılan hesaplar dahil olmak üzere çok sayıda başarısız kimlik doğrulama girişimi gibi belirli şüpheli etkinliklerde uyarı oluşturmak için Azure Güvenlik Merkezi 'ni kullanın. Temel güvenlik durumu izlemeye ek olarak, Azure işlem kaynaklarından (sanal makineler, kapsayıcılar, App Service), veri kaynakları (SQL DB ve depolama) ve Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları toplamak için Güvenlik Merkezi 'nin tehdit koruması modülünü kullanın. Bu özellik, bireysel kaynakların içindeki hesap bozukluklarındaki görünürlüğünüzü görmenizi sağlar.

- [Azure Active Directory Denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure kimlik koruması 'nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Azure ağ etkinlikleri için günlüğe kaydetmeyi etkinleştirme

**Kılavuz** : Azure sanal WAN kaynaklarınız arasında seyahat eden ağ paketlerini kaydetmek için VPN paketi yakalama araçlarını kullanın. Bu, karma ağınızla ilgili hata ayıklama işlemine yardımcı olabilir. Sanal WAN üzerinde bir ağ güvenlik grubu dağıtamazsınız, bunu bağlı sanal ağ kaynaklarınıza dağıtabilirsiniz.

Trafik denetimi için ağ güvenlik gruplarında ağ güvenlik grubu akış günlüklerini etkinleştirin.

Depolama hesabında tutulan akış günlüklerini işlemek için Azure Trafik Analizi özelliğini etkinleştirin ve sonra bunları bir Log Analytics çalışma alanına gönderin. Trafik Analizi, Azure ağlarınız için trafik akışına ek öngörüler sağlar. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [Ağ güvenlik grubu akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md) 

Sanal WAN, etkinleştirilmesi gereken DNS sorgu günlüklerini oluşturmaz veya işlemez.

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Kılavuz** : otomatik olarak etkinleştirilen Azure etkinlik günlükleri, okuma (Get) Işlemleri hariç Azure sanal WAN kaynaklarınız için tüm yazma IŞLEMLERINI (put, Post, Delete) içerir. Etkinlik günlükleri, sorun giderme sırasında hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir. Ancak, sanal WAN Azure Kaynak günlükleri oluşturmaz.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 
- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik** : bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atadığınızdan emin olun. Azure etkinlik günlüklerini Merkezi günlük sistemlerinizle tümleştirdiğinizden emin olun. 

Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın. Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin.

Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : paylaşılan

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: hazırlık – Azure için olay yanıtı işlemini güncelleştirme

**Rehberlik** : kuruluşunuzun güvenlik olaylarına yanıt vermek için işlem yapıldığından emin olun, bu işlemlerin Azure 'da güncelleştirildiğini ve hazırlık sağlamak için bunları düzenli olarak bir şekilde kullanıma hazırlıyoruz. Hizmet sunumunun, uygun olduğu şekilde olay yanıtı işlemine dahil olduğundan emin olun.

- [Kurumsal ortam genelinde güvenliği uygulama](https://aka.ms/AzSec4) 
- [Olay yanıtı başvuru kılavuzu](https://aka.ms/IRRG)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: hazırlık – olay bildirimini kurma

**Rehberlik** : Azure Güvenlik Merkezi 'nde güvenlik olayı iletişim bilgilerini ayarlama. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca, olay uyarı gereksinimlerinize göre farklı Azure hizmetlerinde olay uyarısını ve bildirimi özelleştirme seçenekleriniz vardır.

Microsoft Güvenlik Yanıt Merkezi (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların tam olarak çözümlendiğinden emin olmak için olgudan sonraki olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md) 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : müşteri

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: algılama ve analiz – yüksek kaliteli uyarılara göre olaylar oluşturun

**Kılavuz** : Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analizte ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin yanı sıra güven düzeyini temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örneğin, üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md) 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : müşteri

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: algılama ve analiz – bir olayı araştırın

**Rehberlik** : kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md) 

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process) 

- [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: algılama ve analiz – olayların önceliklerini belirleme

**Kılavuz** : Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri (örneğin, üretim, üretim dışı) açıkça işaretleyin ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md) 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: kapsama, eradleme ve kurtarma – olay işlemeyi otomatikleştirin

**Rehberlik** : güvenlik uyarılarında ve önerilerinde "Azure Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : müşteri

## <a name="posture-and-vulnerability-management"></a>Posture ve güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-8-conduct-regular-attack-simulation"></a>BD-8: normal saldırı simülasyonu gerçekleştir

**Rehberlik** : gereksinimlerinize göre, Azure kaynaklarınızda sızma testi veya Red takım etkinlikleri gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin.

Microsoft Bulut penme test kurallarını izleyerek, sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olun. Microsoft 'un Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı, kırmızı ekip oluşturma ve canlı site sızma sınamasını yürütme, Microsoft 'un stratejisini kullanın.

- [Azure 'da sızma testi](../security/fundamentals/pen-testing.md)

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme** : uygulanamaz

**Sorumluluk** : müşteri

## <a name="endpoint-security"></a>Uç nokta güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: uç nokta güvenliği](/azure/security/benchmarks/security-controls-v2-endpoint-security).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uç nokta algılamayı ve yanıtını kullanın (EDR)

**Rehberlik** : müşterilerin Endpoint Detection ve yanıt ayarlarını yapılandırmasına açıkça izin verilmez. Ancak, Azure sanal WAN sunumunda kullanılan sanal makineler bu özellikleri kullanır. Başvurulan bağlantılarda bu genel yetenekler hakkında daha fazla bilgi edinin. 

- [Microsoft Defender Gelişmiş tehdit korumasına genel bakış](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Windows Server 'lar için Microsoft Defender ATP hizmeti](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Windows dışı sunucular için Microsoft Defender ATP hizmeti](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Azure Güvenlik Merkezi izleme** : Evet

**Sorumluluk** : paylaşılan

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: idare ve strateji](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: varlık yönetimi ve veri koruma stratejisini tanımlama 

**Rehberlik** : sistemlerin ve verilerin sürekli izlenmesi ve korunması için açık bir strateji belgelemenizi ve iletdiğinizden emin olun. İş açısından kritik veri ve sistemlerinin bulunmasını, değerlendirmesini, korunmasını ve izlenmesini önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

- Veri sınıflandırma standardı, iş riskleriyle uyumlu
- Risk ve varlık envanterinde güvenlik kuruluşu görünürlüğü 
- Azure hizmetlerinin kullanım için güvenlik kuruluşu onayı 
- Varlıkların yaşam döngülerinde güvenliği
- Kurumsal veri sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi
- Azure yerel ve üçüncü taraf veri koruma özellikleri kullanımı
- Geçiş içi ve REST kullanım örnekleri için veri şifreleme gereksinimleri
- Uygun şifreleme standartları

Başvurulan bağlantılarda bulunan ek bilgileri gözden geçirin.

- [Azure Güvenlik mimarisi önerisi-depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure güvenlik temelleri-Azure veri güvenliği, şifreleme ve depolama](../security/fundamentals/encryption-overview.md)

- [Bulut benimseme çerçevesi-Azure veri güvenliği ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentleme stratejisini tanımlama 

**Rehberlik** : kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimlerin birleşimini kullanarak varlıklara erişimi parçalara ayırma konusunda kurumsal çapta bir strateji oluşturun. Güvenlik ayrımı gereksinimini, birbirleriyle iletişim kurması gereken sistemlerin günlük işlemlerini etkinleştirme gereksinimiyle dikkatle dengeleyin ve verilere erişin.

Segmentasyon stratejisinin ağ güvenliği, kimlik ve erişim modelleri ve uygulama izni ya da erişim modelleri ve insan işlem denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Kurumsal segmentasyon stratejisi ile ağ segmentlemesini hizalayın](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: güvenlik sonrası yönetim stratejisini tanımlama

**Rehberlik** : bireysel varlıklarınızla ve barındırıldığı ortamda riskleri sürekli olarak ölçün ve azaltır. Yayımlanan uygulamalar, ağ giriş ve çıkış noktaları, Kullanıcı ve yönetici uç noktaları gibi yüksek değerli varlıkların ve yüksek oranda sunulan saldırı yüzeylerinin önceliklerini belirleyin.

- [Azure Güvenlik kıyaslaması-Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: kuruluş rollerini, sorumlulukları ve accountacakları hizalayın

**Rehberlik** : güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararlarına yönelik açık sorumluluğa dayanarak çabalarınızı önceliklendirin, paylaşılan sorumluluk modelinde herkese eğitilendir ve bulutu güvenli hale getirmek için teknik ekipleri teknolojide eğitin.

- [Azure Güvenlik En Iyi Yöntem 1 – Insanlar: ekipleri bulut güvenlik yolculuğunda eğitin](https://aka.ms/AzSec1)

- [Azure Güvenlik En Iyi uygulaması 2-Insanlar: bulut güvenlik teknolojisindeki takımları eğitin](https://aka.ms/AzSec2)

- [Azure Güvenlik En Iyi Yöntem 3-Işlem: bulut güvenlik kararları için sorumluluk atama](https://aka.ms/AzSec3)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-5-define-network-security-strategy"></a>GS-5: ağ güvenlik stratejisini tanımlayın

**Rehberlik** : kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure ağ güvenlik yaklaşımı oluşturun. Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

- Merkezi ağ yönetimi ve güvenlik sorumluluğu
- Kurumsal segmentasyon stratejisi ile hizalanan sanal ağ kesimleme modeli
- Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi
- Internet Edge ve giriş ve çıkış stratejisi
- Karma bulut ve şirket içi bağlantı stratejisi
- Güncel ağ güvenlik yapıtları (ör. ağ diyagramları, başvuru ağ mimarisi)

Başvurulan bağlantılarda bulunan ek bilgileri gözden geçirin.

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Güvenlik kıyaslaması-ağ güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: kimlik ve ayrıcalıklı erişim stratejisi tanımlama

**Rehberlik** : kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun. Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

- Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı
- Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri
- Yüksek ayrıcalıklı kullanıcıların korunması
- Anomali Kullanıcı etkinlikleri izleme ve işleme  
- Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Başvurulan bağlantılarda bulunan ek bilgileri gözden geçirin.

- [Azure Güvenlik kıyaslaması-kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik kıyaslaması-ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Identity Management güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: günlüğe kaydetme ve tehdit yanıt stratejisini tanımlama

**Rehberlik** : uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi kurun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji, aşağıdaki öğeler için belgelenmiş kılavuzluk, ilke ve standartları içermelidir: 

- Güvenlik işlemleri (SecOps) kuruluşun rolü ve sorumlulukları
- NıST veya başka bir sektör çerçevesiyle hizalanan iyi tanımlanmış bir olay yanıt işlemi
- Tehdit algılamayı, olay yanıtını ve uyumluluk gereksinimlerini desteklemek için günlüğü yakalama ve bekletme
- SıEM, yerel Azure özellikleri ve diğer kaynakları kullanarak tehditler hakkındaki merkezi ve bağıntı bilgilerinin yanı sıra müşterilerinizle, tedarikçilerle ve kamu kurumlarınızla ilgili Iletişim ve bildirim planı
- Günlüğe kaydetme ve tehdit algılama, Forensics ve saldırı düzeltme ve doğrulama gibi olay işleme için Azure yerel ve üçüncü taraf platformlarının kullanımı
- Olayları ve olay sonrası etkinliklerini (öğrenilmiş dersler ve kanıt bekletme gibi) işleme için süreçler

Başvurulan bağlantılarda bulunan ek bilgileri gözden geçirin.
- [Azure Güvenlik kıyaslaması-günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik kıyaslaması-olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenlik En Iyi Yöntem 4-Işlem. Bulut için olay yanıtı Işlemini Güncelleştir](https://aka.ms/AzSec4)

- [Azure benimseme çerçevesi, günlüğe kaydetme ve raporlama kararı Kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

**Azure Güvenlik Merkezi izleme** : Şu anda kullanılamıyor

**Sorumluluk** : müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması v2 genel bakış](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
