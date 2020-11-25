---
title: Azure HPC önbelleği için Azure Güvenlik temeli
description: Azure HPC önbellek güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 3e040146673ba16c07856f30b88771d0063713e9
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95896892"
---
# <a name="azure-security-baseline-for-azure-hpc-cache"></a>Azure HPC önbelleği için Azure Güvenlik temeli

Bu güvenlik temeli, [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' dan HPC önbelleğine Microsoft Azure kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure HPC Cache için geçerli olan ilgili kılavuza göre gruplandırılır. Azure HPC önbelleği için geçerli olmayan **denetimler** dışlandı.

Azure HPC önbelleğinin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure HPC Cache Security taban çizgisi eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: iç trafik için güvenlik uygulama

**Kılavuz**: Azure HPC önbellek kaynaklarını dağıtırken, var olan bir sanal ağ oluşturmanız veya kullanmanız gerekir. 

Tüm Azure sanal ağlarının, iş risklerine göre bir kurumsal kesimleme ilkesini izlediğinden emin olun. Kuruluş için daha yüksek risk doğurabilecek tüm sistemleri kendi sanal ağı içinde yalıtılmalı ve bir ağ güvenlik grubu (NSG) ve/veya Azure güvenlik duvarıyla yeterince güvenli hale gelmelidir.

Önbelleğinizi kullanabilmeniz için ağ ile ilgili iki önkoşul ayarlanmalıdır: 

- Azure HPC önbellek örneği için ayrılmış alt ağ

- Önbelleğin depolamaya ve diğer kaynaklara erişebilmesi için DNS desteği

Azure HPC önbelleği, bu kaliteleri olan ayrılmış bir alt ağa ihtiyaç duyuyor: 

- Alt ağda en az 64 IP adresi kullanılabilir olmalıdır.

- Alt ağ, istemci makineler gibi ilgili hizmetler için bile diğer VM 'Leri barındıramaz.

- Birden çok Azure HPC önbellek örneği kullanıyorsanız, her birinin kendi alt ağına ihtiyacı vardır.

En iyi yöntem, her önbellek için yeni bir alt ağ oluşturmaktır. Önbellek oluşturmanın bir parçası olarak yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.

HPC önbelleğini şirket içi NAS depolamayla birlikte kullanmak için, şirket içi ağdaki belirli bağlantı noktalarının Azure HPC önbelleğinin alt ağından sınırsız trafiğe izin verildiğinden emin olmanız gerekir. 

- [NFS depolama erişimi için bağlantı noktalarını yapılandırma](hpc-cache-prerequisites.md#nfs-storage-requirements)

- [Güvenlik kuralları ile ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

 

- [Azure Güvenlik duvarını dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="ns-2-connect-private-networks-together"></a>NS-2: özel ağları birbirine bağlama

**Rehberlik**: bir birlikte bulundurma ortamında Azure veri merkezleri ile şirket içi altyapı arasında özel bağlantılar oluşturmak Için Azure ExpressRoute veya Azure sanal özel ağ (VPN) kullanın. ExpressRoute bağlantıları, genel İnternet üzerinden geçmez ve tipik İnternet bağlantılarına göre daha fazla güvenilirlik, daha hızlı hız ve daha düşük gecikme süresi sunar. Noktadan siteye VPN ve siteden siteye VPN için, bu VPN seçeneklerinin ve Azure ExpressRoute 'un herhangi bir birleşimini kullanarak şirket içi cihazları veya ağları bir sanal ağa bağlayabilirsiniz. 

İki veya daha fazla sanal ağı Azure 'da birlikte bağlamak için sanal ağ eşlemesi ' ni kullanın. Eşlenen sanal ağlar arasındaki ağ trafiği özeldir ve Azure omurga ağında tutulur.

- [ExpressRoute bağlantı modelleri nelerdir?](../expressroute/expressroute-connectivity-models.md) 

- [Azure VPN 'ye Genel Bakış](../vpn-gateway/vpn-gateway-about-vpngateways.md) 

- [Sanal ağ eşleme](../virtual-network/virtual-network-peering-overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Azure hizmetlerine özel ağ erişimi oluşturma

**Rehberlik**: HPC önbelleğine güvenli erişim sağlamak Için Azure sanal ağ hizmet uç noktalarını kullanın. Hizmet uç noktaları, Azure omurga ağı üzerinden İnternet 'ten çıkmadan iyileştirilmiş bir yoldur. 

HPC önbelleği, yönetim uç noktalarının özel bir ağa güvenliğini sağlamak için Azure özel bağlantısının kullanılmasını desteklemez. 

Özel erişim, Azure hizmetleri tarafından sunulan kimlik doğrulama ve trafik güvenliğine ek olarak, ek bir derinlemesine savunma ölçümüdür.

- [Sanal ağ hizmeti uç noktalarını anlama](../virtual-network/virtual-network-service-endpoints-overview.md) 

- [Azure HPC önbelleğini nasıl bağlayacağınızı öğrenin](hpc-cache-mount.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: uygulamaları ve Hizmetleri dış ağ saldırılarına karşı koruma

**Rehberlik**: dağıtılmış hizmet reddi (DDoS) saldırıları, uygulamaya özgü saldırılar ve istenmeyen ve potansiyel olarak kötü amaçlı internet trafiği dahil olmak üzere, HPC önbellek kaynaklarınızı dış ağlardan gelen saldırılara karşı koruyun. 

Azure, bu koruma için yerel özellikleri içerir: 

- Uygulamaları ve Hizmetleri Internet 'ten ve diğer dış konumlardan gelen kötü amaçlı olabilecek trafiğe karşı korumak için Azure Güvenlik Duvarı 'nı kullanın. 
- Azure sanal ağlarınızda DDoS standart korumasını etkinleştirerek varlıklarınızı DDoS saldırılarına karşı koruyun. 
- Ağ kaynaklarınızla ilgili yanlış yapılandırma risklerini algılamak için Azure Güvenlik Merkezi 'ni kullanın.

Azure HPC Cache, Web uygulamalarını çalıştırmak üzere tasarlanmamıştır ve Web uygulamalarını hedef alan dış ağ saldırılarına karşı korumak için herhangi bir ek ağ hizmeti yapılandırmanıza veya ek ağ hizmetleri dağıtmanıza gerek yoktur.

- [Azure Güvenlik Duvarı belgeleri](/azure/firewall/) 

- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](/azure/virtual-network/manage-ddos-protection) 

- [Azure Güvenlik Merkezi önerileri](../security-center/recommendations-reference.md#recs-network)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: izinsiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: bilinen kötü amaçlı IP adreslerine ve etki alanlarına gelen ve giden trafiği engellemek ve/veya bu kaynaklardan gelen trafiği engellemek için tehdit zekası tabanlı filtreleme Ile Azure Güvenlik Duvarı 'nı kullanın. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır. 

Yük incelemesi gerektiğinde, Azure Marketi 'nden yük İnceleme özelliklerine sahip bir üçüncü taraf yetkisiz giriş algılama/izinsiz erişim engelleme/yetkisiz çözüm yükleyebilirsiniz. Alternatif olarak, ağ tabanlı KIMLIKLER/IP 'ler yerine veya ile birlikte, ana bilgisayar tabanlı KIMLIKLERI/IP 'leri ya da ana bilgisayar tabanlı bir uç nokta algılama ve yanıt (EDR) çözümünü kullanmayı tercih edebilirsiniz.

Dikkat: KIMLIKLER/IP 'ler için bir yasal düzenleme veya başka gereksinimleriniz varsa, SıEM çözümünüz için yüksek kaliteli uyarılar sağlamak üzere her zaman ayarlanmış olduğundan emin olun.

- [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md) 

- [Azure Market üçüncü taraf KIMLIKLERI özellikleri](https://azuremarketplace.microsoft.com/marketplace?search=IDS) 

- [Microsoft Defender ATP EDR özelliği](/windows/security/threat-protection/microsoft-defender-atp/overviewendpoint-detection-response)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: ağ güvenlik kurallarını basitleştirme

**Rehberlik**: uygulanamaz; Bu öneri, Azure sanal ağlarına dağıtılabilecek veya verimli yönetim için izin verilen IP aralıklarının gruplamalarını tanımlama yeteneğine sahip olan tekliflere yöneliktir. HPC Cache Şu anda hizmet etiketlerini desteklemiyor. 

En iyi yöntem, her önbellek için yeni bir alt ağ oluşturmaktır. Önbellek oluşturmanın bir parçası olarak yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: güvenli etki alanı adı hizmeti (DNS)

**Rehberlik**: DNS güvenliği için en iyi yöntemleri izleyerek DNS, DNS yükseltme SALDıRıLARı, DNS kirletme ve yanıltma ve diğer kişiler gibi yaygın saldırıları azaltır.

Azure HPC Cache 'in, önbelleğin özel sanal ağının dışındaki kaynaklara erişmesi için DNS ihtiyacı vardır. İş akışınız Azure dışındaki kaynakları içeriyorsa, Azure DNS kullanmaya ek olarak kendi DNS sunucunuzu ayarlamanız ve güvence altına almanız gerekir.

- Azure Blob depolama uç noktalarına, Azure tabanlı istemci makinelerine veya diğer Azure kaynaklarına erişmek için Azure DNS kullanın.
- Şirket içi depolamaya erişmek veya Azure dışındaki istemcilerden gelen önbelleğe bağlanmak için, bu konak adlarını çözebilecek özel bir DNS sunucusu oluşturmanız gerekir.
- İş akışınız hem iç hem de dış kaynak içeriyorsa, Azure 'a özgü çözümleme isteklerini Azure DNS sunucusuna iletmek için özel DNS sunucunuzu ayarlayın. 

Yetkili DNS hizmetiniz olarak Azure DNS kullanıldığında, DNS bölgelerinin ve kayıtlarının Azure RBAC ve kaynak kilitleri kullanılarak yanlışlıkla veya kötü amaçlı değişimlerden korunduğundan emin olun.

Kendi DNS sunucunuzu yapılandırıyorsanız, bu güvenlik yönergelerini izlediğinizden emin olun:

- [Güvenli etki alanı adı sistemi (DNS) dağıtım kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-81/2/final) 

- [DNS girişlerinin tehlikini önleyin ve alt etki alanı devralmayı önleyin](../security/fundamentals/subdomain-takeover.md) 

- [HPC önbelleği için DNS erişim gereksinimleri hakkında daha fazla bilgi edinin](hpc-cache-prerequisites.md#dns-access)

- [Azure DNS genel bakış](../dns/dns-overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>Im-1: merkezi kimlik ve kimlik doğrulama sistemi olarak Azure Active Directory standartlaştırma

**Kılavuz**: Azure HPC Cache, iç işlemler için Azure Active Directory ile tümleştirilmiştir. Ancak, Azure AD, HPC önbellek dağıtımlarını ve ilgili bileşenleri oluşturmak, görüntülemek ve yönetmek için Azure portal veya CLı içindeki kullanıcıların kimliğini doğrulamak için kullanılabilir.

Azure Active Directory (Azure AD), Azure 'daki varsayılan kimlik ve erişim yönetimi hizmetidir. Kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'yi standartlaşmanız gerekir:

- Azure portal, Azure depolama, Azure sanal makinesi (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft Bulut kaynaklar.

- Kuruluşunuzun Azure veya kurumsal ağ kaynaklarınızın uygulamaları gibi kaynakları.

Azure AD 'nin güvenliğini sağlamak, kuruluşunuzun bulut güvenlik pratikte yüksek öncelikli olmalıdır. Azure AD, Microsoft 'un en iyi yöntem önerilerine göre kimlik güvenliğini değerlendirmenize yardımcı olacak bir kimlik güvenli puanı sağlar. Yapılandırmanızın en iyi yöntem önerilerini ne kadar yakından eşleştirmekte olduğunu ölçmek ve güvenlik duruşunuz üzerinde geliştirmeler yapmak için puanı kullanın.

Note: Azure AD, Microsoft hesabı olmayan kullanıcıların kendi uygulamalarına ve kaynaklarına kendi dış kimlik bilgileriyle oturum açmasına olanak tanıyan dış kimliği destekler.

- [Azure Active Directory kiracı](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Bir uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers) 

- [Azure Active Directory kimlik güvenli puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>Im-2: uygulama kimliklerini güvenli ve otomatik olarak yönetme

**Rehberlik**: HPC önbelleği, hizmetler veya Otomasyon gibi insan dışı hesaplar için Azure tarafından yönetilen kimlikleri kullanır. Kaynaklarınıza erişmek veya onları yürütmek üzere daha güçlü bir insan hesabı oluşturmak yerine Azure 'un yönetilen kimlik özelliğinin kullanılması önerilir. 

HPC önbelleği, önceden tanımlanmış erişim verme kuralları aracılığıyla Azure AD kimlik doğrulamasını destekleyen Azure Hizmetleri/kaynakları için yerel olarak kimlik doğrulaması yapabilir. Bu, kaynak kodunda veya yapılandırma dosyalarında sabit kodlu kimlik bilgileri kullanılması gereksinimini ortadan kaldırır.

- [Azure tarafından yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) 

- [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: uygulama erişimi için Azure AD çoklu oturum açma (SSO) kullanma

**Kılavuz**: Azure HPC önbelleği, iç Işlemler IÇIN Azure AD ile tümleştirilemez. Ancak, Azure AD, HPC önbellek dağıtımlarını ve ilgili bileşenleri oluşturmak, görüntülemek ve yönetmek için Azure portal veya CLı içindeki kullanıcıların kimliğini doğrulamak için kullanılabilir.

Azure Active Directory Azure kaynaklarına, bulut uygulamalarına ve şirket içi uygulamalara kimlik ve erişim yönetimi sağlar. Bu, çalışanlar gibi kuruluş kimliklerini, ayrıca iş ortakları, satıcılar ve tedarikçiler gibi dış kimlikleri içerir. Bu, çoklu oturum açma (SSO) ile şirket içinde ve bulutta kuruluşunuzun verilerine ve kaynaklarına erişimi yönetmesini ve güvenliğini sağlar. Sorunsuz, güvenli erişim ve daha fazla görünürlük ve denetim sağlamak için tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD 'ye bağlayın.

- [Azure AD ile uygulama SSO 'SU anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>Im-4: tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulama denetimleri kullanma

**Kılavuz**: Azure HPC önbelleği iç Işlemler IÇIN Azure AD ile tümleştirilemez, ancak Azure AD, HPC önbellek dağıtımlarını ve ilgili bileşenleri oluşturmak, görüntülemek ve yönetmek için Azure Portal veya CLI içindeki kullanıcıların kimliğini doğrulamak için kullanılabilir.  

Azure AD, Multi-Factor Authentication (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulama denetimlerini destekler.

- Multi-Factor Authentication: Azure AD MFA 'yı etkinleştirin ve MFA kuruluminizdeki bazı en iyi uygulamalar için Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. MFA, tüm kullanıcılar, kullanıcılar veya oturum açma koşulları ve risk faktörleri temelinde Kullanıcı başına düzeyinde zorlanabilir.
- Passwordless kimlik doğrulaması – üç adet passwordless kimlik doğrulama seçeneği mevcuttur: Iş için Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri.

Yönetici ve ayrıcalıklı kullanıcılar için güçlü kimlik doğrulama yönteminin en üst düzeyini kullandığınızdan emin olun. Uygun güçlü kimlik doğrulama ilkesini diğer kullanıcılara alın.

Azure HPC Cache Ayrıca, önbelleğe bağlanan istemci sistemleri için eski parola tabanlı kimlik doğrulamasını destekler. Bu tür bağlantılar, şirket içi parola ilkelerini izleyecek bir temel parola ilkesi veya karma hesapları (Şirket içi Active Directory gelen Kullanıcı hesapları) içeren yalnızca bulut hesapları (doğrudan Azure 'da oluşturulan kullanıcı hesapları) içerir. 

Azure AD, parola tabanlı kimlik doğrulaması kullanırken kullanıcıların tahmin edilmesi kolay parolalar ayarlamamasını engelleyen bir parola koruma yeteneği sağlar. Microsoft, telemetri temelinde güncellenen yasaklanmış parolaların küresel bir listesini sağlar ve müşteriler bu listeyi gereksinimlerine göre artırabilir (örneğin, marka, kültürel başvuruları vb.). Bu parola koruması, yalnızca bulut ve karma hesaplar için kullanılabilir.

Note: tek başına parola kimlik bilgileri tabanlı kimlik doğrulaması popüler saldırı yöntemlerine açıktır. Daha yüksek güvenlik için MFA ve güçlü bir parola ilkesi gibi güçlü kimlik doğrulaması kullanın. Varsayılan parolalara sahip üçüncü taraf uygulamaları ve Market hizmetlerini kullanıyorsanız, hizmeti ilk kez ayarlarken yeni, güvenli bir parola ayarlayın. 

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Active Directory için passwordless kimlik doğrulama seçeneklerine giriş](../active-directory/authentication/concept-authentication-passwordless.md)

- [Azure AD varsayılan parola ilkesi](../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Azure AD parola korumasını kullanarak kötü parolaları kaldırma](../active-directory/authentication/concept-password-ban-bad.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>Im-5: hesap bozukluklarını Izleyin ve uyarır

**Kılavuz**: Azure HPC önbelleği, Azure Portal Kullanıcı yönetimi için Azure Active Directory kullanabilir.  Azure Active Directory aşağıdaki veri kaynaklarını sağlar:

- Oturum açma işlemleri raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.

- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi herhangi bir kaynak üzerinde yapılan değişiklikler bulunur.

- Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Bu veri kaynakları Azure Izleyici, Azure Sentinel veya üçüncü taraf SıEM sistemleriyle tümleştirilebilir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir.

Azure Gelişmiş tehdit koruması (ATP), gelişmiş tehditleri, güvenliği aşılmış kimlikleri ve kötü amaçlı Insider eylemlerini belirlemek, algılamak ve araştırmak için Active Directory sinyalleri kullanan bir güvenlik çözümüdür.

- [Azure Active Directory içindeki denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins) 

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](/azure/active-directory/reports-monitoring/concept-user-at-risk) 

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

 

- [Azure Güvenlik Merkezi 'nin tehdit bilgileri koruma modülündeki uyarılar](../security-center/alerts-reference.md) 

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="im-7-eliminate-unintended-credential-exposure"></a>Im-7: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: uygulanamaz; HPC önbelleği, müşterilerin kalıcı verileri çalışan ortama dağıtılmasına izin vermez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="privileged-access"></a>Ayrıcalıklı erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Rehberlik**: HPC önbelleği, hangi hesapların içinde bulundukları aboneliklere ve yönetim gruplarına ayrıcalıklı erişim verildiğini kısıtlayarak iş açısından kritik sistemlere erişimi yalıtmak IÇIN Azure RBAC kullanır.

Adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Bir önbellek oluşturmak için HPC Cache, kullanıcıların NIC 'ler oluşturmak üzere abonelikte yeterli ayrıcalıklara sahip olmasını gerektirir. BLOB depolama kullanılıyorsa, HPC önbelleğinin depolamaya erişmesi için Azure rolleri depolama hesabı katılımcısı ve Depolama Blobu veri katılımcısı gerekir. 

Active Directory Etki Alanı Denetleyicileri (DC 'Ler), güvenlik araçları ve iş açısından kritik sistemlerde yüklü aracılarla sistem yönetimi araçları gibi iş açısından kritik varlıklarınıza yönetici erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi de kısıtlatığınızdan emin olun. Bu yönetim ve güvenlik sistemlerinden ödün veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Azure HPC Cache RBAC izinleri](hpc-cache-prerequisites.md#permissions)

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve mutabık kılma

**Rehberlik**: hesapların ve erişim düzeylerinin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. 

Azure HPC önbelleği, Azure portal ve ilgili arabirimler aracılığıyla Kullanıcı erişimini yönetmek için Azure Active Directory (Azure AD) hesaplarını kullanabilir. Azure AD, grup üyeliklerini incelemenizi, kurumsal uygulamalara erişiminizi ve rol atamalarına yardımcı olan erişim İncelemeleri sunmaktadır. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.

Ayrıca, Azure Privileged Identity Management, çok fazla sayıda yönetici hesabı oluşturulduğunda ve eski veya yanlış yapılandırılmış yönetici hesaplarını belirlemek için de kullanılabilir.

Not: bazı Azure Hizmetleri, Azure AD aracılığıyla yönetilmeyen yerel kullanıcıları ve rolleri destekler. Bu kullanıcıları ayrı ayrı yönetmeniz gerekir.

NFS depolama hedeflerini kullanırken, erişim ayarlarını doğrulamak ve Azure HPC önbelleğinin NFS depolama sistemleriyle iletişim kurabildiğinden emin olmak için ağ yöneticilerinizle ve güvenlik duvarı yöneticileriyle çalışmanız gerekir.

- [HPC önbellek izinlerinin listesi için Azure HPC önbellek önkoşullarını okuyun](hpc-cache-prerequisites.md) 

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Azure AD 'de acil durum erişimi ayarlama

**Rehberlik**: HPC önbelleği, Azure Portal üzerinden kaynak erişimini yönetmek için Azure Active Directory kullanabilir. Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.

Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="pa-5-automate-entitlement-management"></a>PA-5: yetkilendirme yönetimini otomatikleştirme 

**Rehberlik**: HPC önbelleği, Azure Portal üzerinden önbellek kaynaklarına erişimi yönetmek için Azure Active Directory kullanabilir. 

Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere, erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir. 

- [Azure AD erişim gözden geçirmeleri nelerdir?](../active-directory/governance/access-reviews-overview.md) 

- [Azure AD yetkilendirme yönetimi nedir?](../active-directory/governance/entitlement-management-overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: yalnızca yeterli yönetim (en az ayrıcalık ilkesi) Izleyin 

**Rehberlik**: HPC önbelleği, kaynaklarını yönetmek için Azure rol tabanlı erişim denetımı (RBAC) ile tümleşiktir. Azure RBAC, rol atamaları aracılığıyla Azure kaynak erişimini yönetmenizi sağlar. Bu rolleri kullanıcılara atayabilir, hizmet sorumlularını ve yönetilen kimlikleri gruplara atayabilirsiniz. Belirli kaynaklar için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. 

Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıkların her zaman roller için gerekli olan ile sınırlı olması gerekir. Bu, Azure AD Privileged Identity Management (PıM) ' nin tam zamanında (JıT) yaklaşımını tamamlar ve düzenli olarak incelenmelidir.

İzin ayırmak ve yalnızca gerektiğinde özel rol oluşturmak için yerleşik rolleri kullanın.

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../role-based-access-control/overview.md) 

- [Azure 'da RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-1-discover-classify-and-label-sensitive-data"></a>DP-1: hassas verileri bulma, sınıflandırma ve etiketleme 

**Rehberlik**: HPC Cache hassas verileri yönetir ancak hassas verileri bulma, sınıflandırma ve etiketleme yeteneğine sahip değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="dp-2-protect-sensitive-data"></a>DP-2: hassas verileri koruma

**Rehberlik**: Azure rol tabanlı Access Control (Azure RBAC), ağ tabanlı erişim denetimleri ve Azure hizmetlerindeki belirli DENETIMLERI (SQL ve diğer veritabanlarında şifreleme gibi) kullanarak erişimi kısıtlayarak hassas verileri koruyun.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır. Kurumsal segmentasyon stratejisi Ayrıca hassas veya iş açısından kritik veri ve sistemlerinin konumu ile de bilgilendirilir.

Microsoft tarafından yönetilen temel platform için, Microsoft tüm müşteri içeriklerini müşteri veri kaybına ve pozlamaya karşı hassas ve koruyucuları olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için, Microsoft bazı varsayılan veri koruma denetimleri ve yeteneklerini uygulamıştır.

- [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md) 

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: hassas verilerin yetkisiz aktarımını Izleme

**Rehberlik**: HPC Cache hassas verileri iletir ancak hassas verilerin yetkisiz aktarımını izlemeyi desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: geçiş sırasında hassas bilgileri şifreleyin

**Rehberlik**: HPC Cache, TLS v 1.2 veya üzerini kullanarak geçiş sırasında veri şifrelemesini destekler.

Bu, özel ağlardaki trafik için isteğe bağlı olsa da, bu, dış ve genel ağlardaki trafik için önemlidir. HTTP trafiği için, Azure kaynaklarınıza bağlanan istemcilerin TLS v 1.2 veya üzerini anlaştığınızdan emin olun. Uzaktan Yönetim için şifrelenmemiş bir protokol yerine SSH (Linux için) veya RDP/TLS (Windows için) kullanın. Kullanımdan çıkarıldı SSL, TLS ve SSH sürümleri ve protokolleri ve zayıf şifrelemeler devre dışı bırakılmalıdır.

Azure, varsayılan olarak Azure veri merkezleri arasında aktarım halindeki veriler için şifreleme sağlar.

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) 

- [TLS güvenliği hakkında bilgi](/security/engineering/solving-tls1-problem) 

- [Yoldaki Azure verileri için çift şifreleme](../security/fundamentals/double-encryption.md#data-in-transit)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: hassas verileri bekleyen şifreleme

**Kılavuz**: erişim denetimlerini tamamlamak için REST 'teki veriler, şifreleme kullanılarak "bant dışı" saldırılarına karşı korunmalıdır (örneğin, temeldeki depolamaya erişme). Bu, saldırganların verileri kolayca okuyabilmesini veya değiştirememesini sağlamaya yardımcı olur.

Azure, varsayılan olarak bekleyen şifreleme verileri sağlar. Son derece hassas veriler için, kullanılabilir olduğunda tüm Azure kaynaklarında bekleyen ek şifreleme uygulama seçenekleriniz vardır. Azure, şifreleme anahtarlarınızı varsayılan olarak yönetir, ancak Azure belirli Azure hizmetleri için kendi anahtarlarınızı (müşteri tarafından yönetilen anahtarlar) yönetme seçenekleri sunar.

Azure 'da depolanan tüm veriler, önbellek disklerinde de dahil olmak üzere varsayılan olarak Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Yalnızca verilerinizi şifrelemek için kullanılan anahtarları yönetmek istiyorsanız Azure HPC önbellek ayarlarını özelleştirmeniz gerekir.

İşlem kaynaklarında uyumluluk için gerekliyse, verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

- [Azure HPC Cache ile müşteri tarafından yönetilen şifreleme anahtarlarını kullanma](https://docs.microsoft.com/azure/hpc-cache/hpc-cache-create?tabs=azure-portal#enable-azure-key-vault-encryption-optional)

- [Azure 'da bekleyen şifrelemeyi anlama](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](/azure/storage/common/storage-encryption-keys-portal) 

- [Şifreleme modeli ve anahtar yönetimi tablosu](../security/fundamentals/encryption-atrest.md)

 

- [Azure 'da Rest çift Şifrelemeli veriler](../security/fundamentals/double-encryption.md#data-at-rest)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: varlık yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>ÖÖ-1: güvenlik ekibinin, varlıklar için riskleri görebilirliği sağlayın

**Rehberlik**: güvenlik ekiplerinde Azure Güvenlik Merkezi 'ni kullanarak güvenlik risklerini izleyebilmeleri için Azure kiracınızda ve aboneliklerinizde güvenlik okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibinin sorumluluklarına bağlı olarak, güvenlik risklerini izlemek, merkezi bir güvenlik ekibinin veya yerel ekibin sorumluluğunda olabilir. Bu şekilde, güvenlik öngörüleri ve riskler her zaman bir kuruluş içinde toplanmalıdır. 

Güvenlik okuyucusu izinleri bir kiracının tamamına (kök yönetim grubu) veya yönetim gruplarına veya belirli aboneliklere göre uygulanabilir. 

Note: iş yüklerinin ve hizmetlerin görünürlüğünü almak için ek izinler gerekebilir. 

- [Güvenlik okuyucusu rolüne genel bakış](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Grupları 'ye Genel Bakış](../governance/management-groups/overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>-2: güvenlik ekibinin varlık envanterini ve meta verileri erişimine sahip olduğundan emin olun

**Kılavuz**: Azure HPC Cache etiketlerin kullanılmasını destekler. Bunları bir taksonomiye mantıksal olarak düzenlemek için Azure kaynaklarınıza, kaynak gruplarına ve aboneliklerinize Etiketler uygulayın. Her etiket bir ad ve bir değer çiftinden oluşur. 

Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz. Bir önbellek ve önbellek dağıtıldıktan sonra da Etiketler eklenebilir. 

Sanal makinelerde yazılım hakkında bilgi toplamayı otomatik hale getirmek için Azure sanal makine envanterini kullanın. Yazılım adı, sürüm, yayımcı ve yenileme süresi Azure portal kullanılabilir. Yüklemeyi tarih ve diğer bilgilere erişim sağlamak için, Konuk düzeyinde tanılamayı etkinleştirin ve Windows olay günlüklerini Log Analytics çalışma alanına getirin.
HPC önbelleği, kaynak üzerinde bir uygulamanın veya yazılımın yüklemesinin çalıştırılmasına izin vermiyor. 

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md) 

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../security-center/asset-inventory.md) 

- [Kaynak adlandırma ve etiketleme kararı kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json) 

- [Azure sanal makine envanterini etkinleştirme](../automation/automation-tutorial-installed-software.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

**Rehberlik**: HPC önbelleği Azure Resource Manager dağıtımlarını destekler. Kullanıcıların ortamınızda hangi hizmetleri sağlayabileceğini denetlemek ve kısıtlamak için Azure Ilkesi ' ni kullanın. Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma. Onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types) 

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>ÖÖ-4: varlık yaşam döngüsü yönetiminin güvenliğinin yapıldığından emin olun

**Rehberlik**: uygulanamaz. Azure HPC önbelleği, bir yaşam döngüsü yönetimi işlemindeki varlıkların güvenliğini sağlamak için kullanılamaz. Bu, yüksek etki olarak kabul edilen varlıkların özniteliklerinin ve ağ yapılandırmalarının korunması için müşterinin sorumluluğundadır. 

Müşterinin özniteliği ve ağ yapılandırması değişikliklerini yakalamak, değişiklik etkisini ölçmek ve uygun şekilde düzeltme görevleri oluşturmak için bir işlem oluşturması önerilir.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe kaydetme ve tehdit algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Azure kaynakları için tehdit algılamayı etkinleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'nin yerleşik tehdit algılama özelliğini kullanın ve HPC önbellek kaynaklarınız Için Azure Defender 'ı (resmi olarak Azure Gelişmiş tehdit koruması) etkinleştirin. HPC için Azure Defender önbelleği, önbellek kaynaklarınıza erişmek veya bu kaynaklara yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik bilgileri katmanı sağlar.

HPC önbelleğinden tüm günlükleri, özel tehdit algılamalarını ayarlamak için kullanılabilen SıEM 'nize iletin. Olası tehditler ve bozukluklar için farklı türlerde Azure varlıklarını izlemediğinizden emin olun. Analistlerin sıralama için yanlış pozitif durumları azaltmaya yönelik yüksek kaliteli uyarılar almaya odaklanın. Uyarılar, günlük verilerinden, aracılardan veya diğer verilerden kaynaklıdır.

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection) 

- [Azure Güvenlik Merkezi güvenlik uyarıları başvuru kılavuzu](../security-center/alerts-reference.md) 

- [Tehditleri algılamak için özel analiz kuralları oluşturma](../sentinel/tutorial-detect-threats-custom.md) 

- [Azure Sentinel ile Cyber Threat Intelligence](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Kılavuz**: Azure AD, Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel veya diğer SIEM/izleme araçlarıyla tümleştirilen, daha gelişmiş izleme ve analiz kullanım durumları için kullanılabilen aşağıdaki kullanıcı günlüklerini sağlar:
- Oturum açma işlemleri – oturum açma işlemleri raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.

- Denetim günlükleri-Azure AD içindeki çeşitli özelliklerle yapılan tüm değişiklikler için günlüklere yönelik izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi her türlü kaynak üzerinde yapılan değişiklikler verilebilir.

- Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi veya abonelikte kullanım dışı bırakılmış hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. Azure Güvenlik Merkezi 'nin tehdit koruması modülü, temel güvenlik durumu izlemeye ek olarak, bireysel Azure işlem kaynakları (sanal makineler, kapsayıcılar, App Service), veri kaynakları (SQL DB ve depolama) ve Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları da toplayabilirler. Bu özellik, bireysel kaynakların içindeki hesap bozukluklarını görünürlüğe sahip etmenize olanak tanır.

- [Azure Active Directory içindeki denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure kimlik koruması 'nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md) 

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Azure ağ etkinlikleri için günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: VPN ağ geçitlerini ve paket yakalama yeteneklerini, sanal ağlarınız arasında seyahat edilen ağ paketlerini kaydetmek için yaygın olarak kullanılabilir paket yakalama araçlarına ek olarak kullanabilirsiniz.

Azure HPC önbellek kaynaklarınızın dağıtıldığı ağ üzerinde bir ağ güvenlik grubu dağıtın. Trafik denetimi için ağ güvenlik gruplarında ağ güvenlik grubu akış günlüklerini etkinleştirin.

Akış günlüklerinizin bir depolama hesabında tutulur. Trafik Analizi çözümünü, bu akış günlüklerini işlemek ve Log Analytics çalışma alanına göndermek için etkinleştirin. Trafik Analizi, Azure ağlarınız için trafik akışına ek öngörüler sağlar. Trafik Analizi, ağ etkinliğini görselleştirmenize, etkin noktaları belirlemenize, güvenlik tehditlerini belirlemenize, trafik akışı düzenlerini anlamanıza ve ağ yapılandırmalarını belirlemenize yardımcı olabilir.

Önbelleğin, sanal ağının dışındaki kaynaklara erişmesi için DNS ihtiyacı vardır. Kullandığınız kaynaklara bağlı olarak, özelleştirilmiş bir DNS sunucusu ayarlamanız ve bu sunucu ile Azure DNS sunucuları arasında iletmeyi yapılandırmanız gerekebilir. 

Kuruluşunuzun ihtiyaç duyduğu şekilde, DNS günlüğü çözümü için Azure Marketi 'nden bir üçüncü taraf çözümü uygulayın.

- [VPN ağ geçitleri için paket yakalamaları yapılandırma](../vpn-gateway/packet-capture.md) 

- [Ağ güvenlik grubu akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md) 

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md) 

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md) 

- [DNS önkoşulları hakkında daha fazla bilgi edinin](hpc-cache-prerequisites.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Azure HPC önbellek kaynakları otomatik olarak etkinlik günlükleri oluşturur. Bu Günlükler tüm yazma işlemlerini (PUT, POST, DELETE) içerir, ancak okuma işlemlerini (GET) içermez. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

Ayrıca, Azure Güvenlik Merkezi 'ni ve Azure Ilkesini kullanarak HPC önbelleği için Azure Kaynak günlüklerini etkinleştirebilir ve veri toplamayı günlüğe kaydedebilir. Bu Günlükler, daha sonra güvenlik olaylarını araştırmak ve bu uygulamaların gerçekleştirilmesi için kritik öneme sahip olabilir.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md) 

- [Azure Güvenlik Merkezi veri toplamayı anlama](../security-center/security-center-enable-data-collection.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik**: bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atamış olduğunuzdan emin olun.

Azure etkinlik günlüklerini merkezi günlüklerinizi tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin.

Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: hazırlık – Azure için olay yanıtı işlemini güncelleştirme

**Rehberlik**: kuruluşunuzun güvenlik olaylarına yanıt vermek için işlem yapıldığından emin olun, bu işlemlerin Azure 'da güncelleştirildiğini ve hazırlık sağlamak için bunları düzenli olarak bir şekilde kullanıma hazırlıyoruz.

- [Kurumsal ortam genelinde güvenliği uygulama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ir-2-preparation--set-up-incident-notification"></a>IR-2: hazırlık – olay bildirimini ayarlama 

**Rehberlik**: Azure Güvenlik Merkezi 'nde güvenlik olayı iletişim bilgilerini ayarlama. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca, olay uyarı gereksinimlerinize göre farklı Azure hizmetlerinde olay uyarısını ve bildirimi özelleştirme seçenekleriniz vardır. 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: algılama ve analiz – yüksek kaliteli uyarılara göre olaylar oluşturun

**Rehberlik**: yüksek kaliteli uyarılar oluşturmak ve uyarı kalitesini ölçmek için bir işleminiz olduğundan emin olun. Bu, geçmiş olaylardan dersler öğrenmenize ve analistlere yönelik olarak bu uyarıların önceliklendirmesine izin verir, böylelikle hatalı pozitif sonuçlar üzerinde zaman harcanmazlar. 

Yüksek kaliteli uyarılar, geçmiş olaylar, doğrulanan topluluk kaynakları ve uyarılar oluşturmak ve temizlemek için tasarlanan araçlar ve farklı sinyal kaynaklarını kullanarak oluşturulabilir. 

Azure Güvenlik Merkezi, birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. Azure Sentinel 'e uyarı akışı sağlamak için ASC Data bağlayıcısını kullanabilirsiniz. Azure Sentinel, bir araştırma için olayları otomatik olarak oluşturmak üzere gelişmiş uyarı kuralları oluşturmanıza olanak sağlar. 

Azure kaynakları için riskleri belirlemenize yardımcı olmak üzere dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışarı aktarın.

- [Dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: algılama ve analiz – bir olayı araştırın

**Rehberlik**: analistlerin, olası olayları araştırarak, ne olduğunu tam olarak bir görünüm oluşturmak için farklı veri kaynaklarını sorgulayabileceği ve kullanabilmesi için analist olun. Görünmeyen noktaları önlemek için, bir olası saldırganın sonlandırma zincirindeki etkinliklerini izlemek üzere farklı Günlükler toplanmalıdır.  Ayrıca, Öngörüler ve dersleri diğer analistler için yakalandığından ve gelecekteki geçmiş başvurusuyla emin olmanız gerekir.  

Araştırmaya yönelik veri kaynakları, zaten kapsam içi hizmetlerden ve çalışan sistemlerde toplanmakta olan Merkezi günlük kaynakları içerir, ancak şunları da içerebilir:

- Ağ verileri – ağ akışı günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik grupları ' akış günlükleri, Azure ağ Izleyicisi ve Azure Izleyicisi ' ni kullanın. 

- Çalışan sistemlerin anlık görüntüleri: 

    - Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

    - Çalışan sistem belleğinin bir anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

    - Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel, olayların tam yaşam döngüsünü yönetmek için neredeyse tüm günlük kaynakları ve bir servis talebi yönetim portalı genelinde kapsamlı veri analizi sağlar. İnceleme sırasında zeka bilgileri, izleme ve raporlama amaçları için bir olayla ilişkilendirilebilir. 

- [Windows makinenin diskinin anlık görüntüsünü alma](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesinin diskinin anlık görüntüsünü alma](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure tanılama bilgilerini ve bellek dökümü toplamayı destekler](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırın](../sentinel/tutorial-investigate-cases.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: algılama ve analiz – olayların önceliklerini belirleme

**Rehberlik**: olayların, uyarı önem derecesine ve varlık duyarlığına göre ilk olarak odaklanacağı analistlere bağlam sağlar. 

Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için bulma veya analiz etme sırasında ne kadar önemli olduğunu, hatta uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin güven düzeyini temel alır.

Ayrıca, etiketleri kullanarak kaynakları işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: kapsama, eradleme ve kurtarma – olay işlemeyi otomatikleştirin

**Rehberlik**: yanıt süresini hızlandırmak ve analistlerin yükünü azaltmak için el ile yinelenen görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütülmesi daha uzun sürer, her olayı yavaşlatarak analist 'nin işleyebileceği olayların sayısını azaltır. El ile gerçekleştirilen görevler Ayrıca analist yükselini artırır ve bu da gecikme ve analistlerin karmaşık görevlere etkin bir şekilde odaklanmasına olanak tanır. İşlemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarına yanıt vermek üzere bir PlayBook çalıştırmak için Azure Güvenlik Merkezi 'nde ve Azure Sentinel 'de iş akışı Otomasyonu özelliklerini kullanın. PlayBook, bildirimler gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi işlemleri gerçekleştirir. 

- [Güvenlik Merkezi 'nde iş akışı Otomasyonu 'nu yapılandırma](../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi 'nde otomatik tehdit yanıtlarını ayarlama](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="posture-and-vulnerability-management"></a>Posture ve güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>BD-3: işlem kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: VM 'ler, kapsayıcılar ve diğerleri dahil tüm işlem kaynaklarında güvenli konfigürasyonlar oluşturmak Için Azure Güvenlik Merkezi 'Ni ve Azure ilkesini kullanın.

- [Azure Güvenlik Merkezi önerilerini izleme](../security-center/security-center-recommendations.md) 

- [Güvenlik önerileri - başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pv-8-conduct-regular-attack-simulation"></a>BD-8: normal saldırı simülasyonu gerçekleştir

**Rehberlik**: gerektiğinde, Azure kaynaklarınızda sızma testi veya Red takım etkinlikleri gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin.
Microsoft Bulut penme test kurallarını izleyerek, sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olun. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Azure 'da sızma testi](../security/fundamentals/pen-testing.md)

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="backup-and-recovery"></a>Yedekleme ve Kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: yedekleme ve kurtarma](/azure/security/benchmarks/security-controls-v2-backup-recovery).*

### <a name="br-1-ensure-regular-automated-backups"></a>BR-1: düzenli otomatik yedeklemeler sağlayın

**Kılavuz**: Azure HPC Cache bir depolama sistemi değil bir önbelleğe alma çözümü olduğundan, depolama hedeflerine ait verilerin düzenli olarak yedeklenmesini sağlamaya odaklanın. Azure Blob kapsayıcıları ve şirket içi depolama hedeflerini yedeklemek için standart yordamları izleyin. 

Bölgesel bir kesinti durumunda kesintiyi en aza indirmek için, bölgeler arası veri erişimi sağlamak için gerekli adımları izleyebilirsiniz.  

Her Azure HPC Cache örneği belirli bir abonelik içinde ve tek bir bölgede çalışır. Bu, bölgenin tam bir kesinti olması durumunda önbellek iş akışınızın kesintiye uğramaması anlamına gelir. Bu kesintiyi en aza indirmek için kuruluş, birden çok bölgeden erişilebilen arka uç depolamayı kullanmalıdır. Bu depolama alanı, uygun DNS desteğiyle şirket içi bir NAS sistemi ya da önbellekten farklı bir bölgede bulunan Azure Blob depolama alanı olabilir.

İş akışınız birincil bölgenize devam ettikçe, veriler bölge dışında uzun vadeli depolamaya kaydedilir. Önbellek bölgesi kullanılamaz hale gelirse, ikincil bölgede yinelenen bir Azure HPC önbellek örneği oluşturabilir, aynı depolamaya bağlanabilir ve yeni önbellekten çalışmayı sürdürebilirsiniz.

- [Bölgesel yük devretme hakkında daha fazla bilgi edinin](hpc-region-recovery.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="br-2-encrypt-backup-data"></a>BR-2: yedekleme verilerini şifreleme

**Rehberlik**: yedeklemelerinizin saldırılara karşı korunduğundan emin olun. Bu, gizlilik kaybına karşı korunmak için yedeklemelerin şifrelenmesini içermelidir.

Azure Backup kullanarak şirket içi yedekleme için, sağladığınız parola kullanılarak bekleyen şifreleme sağlanır. Normal Azure hizmeti yedeklemesi için, yedekleme verileri Azure platformu tarafından yönetilen anahtarlar kullanılarak otomatik olarak şifrelenir. Yedeklemeyi, müşteri tarafından yönetilen anahtarları kullanarak şifrelemeyi seçebilirsiniz. Bu durumda, anahtar kasasındaki bu müşterinin yönettiği anahtarın de yedekleme kapsamında olduğundan emin olun.

Azure HPC önbelleği, önbellek diskleri için bir müşteri anahtarı eklemeseniz bile, önbelleğe alınmış verilerinizi tutan yönetilen disklerde VM ana bilgisayar şifrelemesi ile de korunur. Çift şifreleme için müşteri tarafından yönetilen bir anahtar eklemek, yüksek güvenlik gereksinimlerine sahip müşteriler için ek bir güvenlik düzeyi sağlar. Ayrıntılar için Azure disk depolaması 'nın sunucu tarafı şifrelemesini okuyun.

Yedeklemeleri ve müşteri tarafından yönetilen anahtarları korumak için Azure Backup, Azure Key Vault veya diğer kaynaklarda rol tabanlı erişim denetimi kullanın. Ayrıca, yedeklemelerin değiştirilemeyeceği veya silinebilmesi için gelişmiş güvenlik özelliklerinin MFA gerektirmesini sağlayabilirsiniz.

- [VM konak şifrelemesi](../virtual-machines/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

- [Azure disk depolama 'nın sunucu tarafı şifrelemesi](../virtual-machines/disk-encryption.md)

- [Azure Backup güvenlik özelliklerine genel bakış](../backup/security-overview.md) 

- [Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelenmesi](../backup/encryption-at-rest-with-cmk.md)  

- [Azure 'da Key Vault anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-5.1.0&amp;preserve-view=true)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: düzenli olarak, yedeklenen müşteri tarafından yönetilen anahtarları geri yüklemenize emin olun.

- [Azure 'da Key Vault anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-5.1.0&amp;preserve-view=true)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: kayıp anahtarların riskini azaltma

**Rehberlik**: anahtar kaybını engellemek ve kurtarmak için ölçülerde yer aldığından emin olun. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin.

- [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: idare ve strateji](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: varlık yönetimi ve veri koruma stratejisini tanımlama 

**Rehberlik**: sistemlerin ve verilerin sürekli izlenmesi ve korunması için açık bir strateji belgelemenizi ve iletdiğinizden emin olun. İş açısından kritik veri ve sistemlerinin bulunmasını, değerlendirmesini, korunmasını ve izlenmesini önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Veri sınıflandırma standardı, iş riskleriyle uyumlu

-   Risk ve varlık envanterinde güvenlik kuruluşu görünürlüğü 

-   Azure hizmetlerinin kullanım için güvenlik kuruluşu onayı 

-   Varlıkların yaşam döngülerinde güvenliği

-   Kurumsal veri sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özellikleri kullanımı

-   Geçiş içi ve REST kullanım örnekleri için veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik mimarisi önerisi-depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure güvenlik temelleri-Azure veri güvenliği, şifreleme ve depolama](../security/fundamentals/encryption-overview.md)

- [Bulut benimseme çerçevesi-Azure veri güvenliği ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik kıyaslaması-varlık yönetimi](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Güvenlik kıyaslaması-veri koruma](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentleme stratejisini tanımlama 

**Rehberlik**: kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimlerin birleşimini kullanarak varlıklara erişimi parçalara ayırma konusunda kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı gereksinimini, birbirleriyle iletişim kurması gereken sistemlerin günlük işlemlerini etkinleştirme gereksinimiyle dikkatle dengeleyin ve verilere erişin.

Segmentleme stratejisinin ağ güvenliği, kimlik ve erişim modelleri ve uygulama izni/erişim modelleri ve insan işlem denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Kurumsal segmentasyon stratejisi ile ağ segmentlemesini hizalayın](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: güvenlik sonrası yönetim stratejisini tanımlama

**Rehberlik**: bireysel varlıklarınızda ve barındırdıkları ortamda riskleri sürekli olarak ölçün ve azaltır. Yayımlanan uygulamalar, ağ giriş ve çıkış noktaları, Kullanıcı ve yönetici uç noktaları vb. gibi yüksek değerli varlıkların ve yüksek oranda ortaya çıkarılan saldırı yüzeylerinin önceliklerini belirleyin.

- [Azure Güvenlik kıyaslaması-Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: kuruluş rollerini, sorumlulukları ve accountacakları hizalayın

**Rehberlik**: güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararlarını açık bir şekilde yapın, paylaşılan sorumluluk modelinde herkese eğitilendir ve bulutu güvenli hale getirmek için teknik ekipleri teknolojide eğitin.

- [Azure Güvenlik En Iyi Yöntem 1 – Insanlar: ekipleri bulut güvenlik yolculuğunda eğitin](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenlik En Iyi uygulaması 2-Insanlar: bulut güvenlik teknolojisindeki takımları eğitin](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenlik En Iyi Yöntem 3-Işlem: bulut güvenlik kararları için sorumluluk atama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-5-define-network-security-strategy"></a>GS-5: ağ güvenlik stratejisini tanımlayın

**Rehberlik**: kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure ağ güvenlik yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisi ile hizalanan sanal ağ kesimleme modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   Internet Edge ve giriş ve çıkış stratejisi

-   Karma bulut ve şirket içi bağlantı stratejisi

-   Güncel ağ güvenlik yapıtları (ör. ağ diyagramları, başvuru ağ mimarisi)

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik kıyaslaması-ağ güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: kimlik ve ayrıcalıklı erişim stratejisi tanımlama

**Rehberlik**: kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Anomali Kullanıcı etkinlikleri izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik kıyaslaması-kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik kıyaslaması-ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Identity Management güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: günlüğe kaydetme ve tehdit yanıt stratejisini tanımlama

**Rehberlik**: uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi kurun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Güvenlik işlemleri (SecOps) kuruluşun rolü ve sorumlulukları 

-   NıST veya başka bir sektör çerçevesiyle hizalanan iyi tanımlanmış bir olay yanıt işlemi 

-   Tehdit algılamayı, olay yanıtını ve uyumluluk gereksinimlerini desteklemek için günlüğü yakalama ve bekletme

-   SıEM, yerel Azure özellikleri ve diğer kaynakları kullanarak tehditler hakkında merkezi görünürlük ve bağıntı bilgileri 

-   Müşterilerinizle, Tedarikçilerinizden ve genel ilgi şahıslarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, Forensics ve saldırı düzeltme ve doğrulama gibi olay işleme için Azure yerel ve üçüncü taraf platformlarının kullanımı

-   Olayları ve olay sonrası etkinliklerini (öğrenilmiş dersler ve kanıt bekletme gibi) işleme için süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik kıyaslaması-günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik kıyaslaması-olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenlik En Iyi Yöntem 4-Işlem. Bulut için olay yanıtı Işlemini Güncelleştir](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure benimseme çerçevesi, günlüğe kaydetme ve raporlama kararı Kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure Kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması v2 genel bakış](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
