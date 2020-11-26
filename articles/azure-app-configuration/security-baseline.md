---
title: Azure Uygulama yapılandırması için Azure Güvenlik temeli
description: Azure Uygulama yapılandırması güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 61dc3b9376737f89643473dffc3c915d3e0d9c44
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183456"
---
# <a name="azure-security-baseline-for-azure-app-configuration"></a>Azure Uygulama yapılandırması için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' dan Azure Uygulama yapılandırmasına kılavuzluk uygular. Azure Güvenlik Karşılaştırması Azure’da bulut çözümlerinizin güvenliğini nasıl sağlayabileceğinize ilişkin öneriler getirir. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Uygulama yapılandırması için geçerli olan ilgili kılavuza göre gruplandırılır. Azure Uygulama yapılandırması için geçerli olmayan **denetimler** dışlandı.

Azure uygulama yapılandırmasının Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure Uygulama yapılandırması güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: iç trafik için güvenlik uygulama

**Kılavuz**: Azure Uygulama yapılandırması, hiçbir kaynağı doğrudan bir sanal ağa dağıtmaz. Hizmet bir sanal ağa dağıtılmadığından, hizmetin iç trafiğinin güvenliğini sağlamak için belirli ağ özelliklerinden yararlanamaz: ağ güvenlik grupları, yol tabloları veya Azure Güvenlik Duvarı gibi diğer ağ araçları. Ancak, uygulama yapılandırması, bir sanal ağdan Azure Uygulama yapılandırmasına güvenli bir şekilde bağlanmak için özel uç noktaları kullanmanıza olanak sağlar.

SSL/TLSv1, SMBv1, LM/NTLMv1, wDigest, Imzasız LDAP bağlamaları ve zayıf şifrelemeler gibi eski güvenli olmayan protokollerin kullanımını öğrenmek için Azure Sentinel kullanın.

- [Azure Uygulama yapılandırması için özel uç noktaları kullanma](concept-private-endpoint.md)

- [Azure Sentinel güvenli olmayan protokoller çalışma kitabı](../sentinel/quickstart-get-visibility.md#use-built-in-workbooks)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

### <a name="ns-2-connect-private-networks-together"></a>NS-2: özel ağları birbirine bağlama

**Kılavuz**: Azure Uygulama yapılandırması özel bir bağlantı üzerinden güvenli bir şekilde veri göndermek için özel uç noktaların kullanılmasını destekler. Azure veri merkezleri ile şirket içi altyapı arasında özel bağlantılar oluşturmak için Azure ExpressRoute veya Azure sanal özel ağ (VPN) kullanın. ExpressRoute bağlantıları, genel İnternet üzerinden geçmez ve tipik İnternet bağlantılarına göre daha fazla güvenilirlik, daha hızlı hız ve daha düşük gecikme süresi sunar. Noktadan siteye VPN ve siteden siteye VPN için, bu VPN seçeneklerinin ve Azure ExpressRoute 'un herhangi bir birleşimini kullanarak şirket içi cihazları veya ağları bir sanal ağa bağlayabilirsiniz.

İki veya daha fazla sanal ağı Azure 'da birlikte bağlamak için sanal ağ eşlemesi ' ni kullanın. Eşlenen sanal ağlar arasındaki ağ trafiği özeldir ve Azure omurga ağında tutulur.

- [ExpressRoute bağlantı modelleri nelerdir?](../expressroute/expressroute-connectivity-models.md)

- [Azure VPN 'ye Genel Bakış](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [Sanal ağ eşleme](../virtual-network/virtual-network-peering-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Azure hizmetlerine özel ağ erişimi oluşturma

**Kılavuz**: internet 'ten çıkmadan sanal ağlarınızdan Azure Uygulama yapılandırmasına özel erişimi etkinleştirmek Için Azure özel bağlantısı 'nı kullanın.

Özel erişim, Azure hizmetleri tarafından sunulan kimlik doğrulama ve trafik güvenliğine ek olarak ayrıntılı bir savunma ölçümüdür.

- [Azure özel bağlantısını anlama](../private-link/private-link-overview.md)

- [Azure Uygulama yapılandırması 'nda özel bağlantı ayarlama](concept-private-endpoint.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: uygulamaları ve Hizmetleri dış ağ saldırılarına karşı koruma

**Rehberlik**: bir sanal ağ üzerinden yapılandırma değerlerine erişirken, dağıtılmış hizmet reddi (DDoS) saldırıları, uygulamaya özgü saldırılar ve istenmeyen ve potansiyel olarak kötü amaçlı internet trafiği dahil olmak üzere, kaynaklarınızı dış ağlardan gelen saldırılara karşı koruyun. Uygulamaları ve Hizmetleri Internet 'ten ve diğer dış konumlardan gelen kötü amaçlı olabilecek trafiğe karşı korumak için Azure Güvenlik Duvarı 'nı kullanın. Azure sanal ağlarınızda DDoS standart korumasını etkinleştirerek varlıklarınızı DDoS saldırılarına karşı koruyun. Ağınızla ilgili kaynaklarınızla ilgili yanlış yapılandırma risklerini algılamak için Azure Güvenlik Merkezi 'ni kullanın.

Azure Uygulama yapılandırması Web uygulamalarını çalıştırmak için tasarlanmamıştır, bu Web uygulamaları için yapılandırma sağlar. Web uygulamalarını hedefleyen dış ağ saldırılarına karşı korumak için ek ayarları yapılandırmanız veya ek ağ hizmetlerini dağıtmanız gerekmez.

- [Azure Güvenlik Duvarı belgeleri](../firewall/index.yml)

- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](../ddos-protection/manage-ddos-protection.md)

- [Azure Güvenlik Merkezi önerileri](../security-center/recommendations-reference.md#recs-network)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: izinsiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: bilinen kötü amaçlı IP adreslerine ve etki alanlarına gelen ve giden trafiği filtrelemek ve/veya engellemek için tehdit zekası tabanlı filtreleme Ile Azure Güvenlik Duvarı 'nı kullanın. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır. Yük incelemesi gerektiğinde, Azure Marketi 'nden yük İnceleme özelliklerine sahip bir üçüncü taraf KIMLIKLERI/IP çözümü dağıtabilirsiniz. Alternatif olarak, ağ tabanlı KIMLIKLER/IP 'ler yerine veya ile birlikte, ana bilgisayar tabanlı KIMLIKLERI/IP 'leri ya da ana bilgisayar tabanlı bir uç nokta algılama ve yanıt (EDR) çözümünü kullanmayı tercih edebilirsiniz.

Dikkat: KIMLIKLER/IP 'ler için bir yasal düzenleme veya başka gereksinimleriniz varsa, SıEM çözümünüz için yüksek kaliteli uyarılar sağlamak üzere her zaman ayarlanmış olduğundan emin olun.

- [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketi 3. taraf KIMLIĞI özellikleri içerir](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [Microsoft Defender ATP EDR özelliği](/windows/security/threat-protection/microsoft-defender-atp/overviewendpoint-detection-response)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: ağ güvenlik kurallarını basitleştirme

**Rehberlik**: ağ güvenlik gruplarında veya uygulama yapılandırma kaynaklarınız Için yapılandırılmış Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak Için Azure sanal ağ hizmeti etiketlerini kullanın. Uygulamanızın ağındaki giden trafiğe yönelik güvenlik kuralları oluştururken, belirli IP adreslerinin yerine "AppConfiguration" hizmet etiketini kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adını belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

- [Hizmet etiketlerini anlama ve kullanma](../virtual-network/service-tags-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Kimlik Yönetimi](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>KY-1: Azure Active Directory’yi merkezi kimlik ve kimlik doğrulaması sistemi olarak standartlaştırma

**Kılavuz**: Azure Uygulama yapılandırması, Azure 'un varsayılan kimlik ve erişim yönetimi hizmeti olan Azure Active Directory (Azure AD) ile tümleşiktir. Kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'yi standartlaşmanız gerekir:
- Azure portal, Azure depolama, Azure sanal makinesi (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft Bulut kaynaklar.
- Kuruluşunuzun kaynakları. Örneğin Azure’daki uygulamalar veya kurumsal ağ kaynaklarınız.

Kuruluşunuzun bulut güvenliği uygulamasında Azure AD’nin güvenliğini sağlamaya yüksek öncelik verilmelidir. Azure AD, Microsoft 'un en iyi yöntem önerilerine göre kimlik güvenliğini değerlendirmenize yardımcı olacak bir kimlik güvenli puanı sağlar. Bu puanı kullanarak yapılandırmanızın en iyi yöntem önerileriyle ne kadar uyumlu olduğunu ölçebilir ve güvenlik duruşunuzda geliştirmeler yapabilirsiniz.

Azure, Azure AD ve OAuth kullanarak uygulama yapılandırma verilerine erişim yetkisi veren aşağıdaki Azure yerleşik rollerini sağlar:

- Uygulama yapılandırma verileri sahibi: uygulama yapılandırma verilerine okuma/yazma/silme erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.

- Uygulama yapılandırma veri okuyucusu: uygulama yapılandırma verilerine okuma erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.

- Katkıda bulunan: uygulama yapılandırma kaynağını yönetmek için bu rolü kullanın. Uygulama yapılandırma verilerine erişim tuşları kullanılarak erişilebilirken, bu rol Azure AD kullanarak verilere doğrudan erişim vermez.

- Okuyucu: uygulama yapılandırma kaynağına okuma erişimi sağlamak için bu rolü kullanın. Bu, kaynağın erişim anahtarlarına veya uygulama yapılandırmasında depolanan verilere erişim vermez.

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure Active Directory kimlik güvenli puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

- [Azure AD kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme](concept-enable-rbac.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>Im-2: uygulama kimliklerini güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure tarafından yönetilen kimlikleri kullanarak, diğer Azure hizmetleri gibi insan dışı hesaplardan Azure Uygulama yapılandırmasına erişin. Ek kimlik bilgilerini yönetme gereksinimini sınırlamak üzere kaynaklarınıza erişmek veya onları yürütmek üzere daha güçlü bir insan hesabı oluşturmak yerine Azure yönetilen kimlik özelliğinin kullanılması önerilir. Azure Uygulama yapılandırmasına, Azure AD kimlik doğrulamasını destekleyen diğer Azure Hizmetleri/kaynakları için yerel olarak kimlik doğrulaması yapmak üzere bir yönetilen kimlik de atanabilir. Bu, uygulama yapılandırmasından parolaları alırken Azure Key Vault için kolay erişim sağlamak için yararlı olabilir. Yönetilen kimlikler kullanılırken, kimlik Azure platformu tarafından yönetilir ve tüm gizli dizileri sağlamanıza veya döndürmenize gerek yoktur.

Azure Uygulama yapılandırması, uygulamanıza iki tür kimlik verilmesini destekler:
- Sistem tarafından atanan bir kimlik yapılandırma kaynağına bağlıdır. Yapılandırma kaynağınız silinirse silinir. Bir yapılandırma kaynağında yalnızca bir sistem tarafından atanan kimlik olabilir.
- Kullanıcı tarafından atanan bir kimlik, yapılandırma kaynağınız için atanabilecek tek başına bir Azure kaynağıdır. Bir yapılandırma kaynağında birden çok kullanıcı tarafından atanan kimlik olabilir.

Yönetilen kimlikler yararlanılabilir olamaz, Azure uygulama yapılandırma kaynak düzeyinde kısıtlanmış izinlerle bir hizmet sorumlusu oluşturun. Bu hizmet sorumlularını sertifika kimlik bilgileriyle yapılandırın ve yalnızca istemci gizli bilgilerine geri dönün. Her iki durumda da, Azure Key Vault çalışma zamanı ortamı (örneğin, bir Azure işlevi) anahtar kasasından kimlik bilgisini alabilmesi için Azure tarafından yönetilen kimliklerle birlikte kullanılabilir.

- [Azure Uygulama yapılandırması için Yönetilen kimlikler kullanma](overview-managed-identity.md)

- [Azure Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)

- [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Uygulama Yapılandırması’na erişmek için yönetilen kimlikleri kullanma](howto-integrate-azure-managed-service-identity.md)

- [Azure hizmet sorumlusu](/powershell/azure/create-azure-service-principal-azureps) 

- [Sertifikalarla hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md) 

- [Güvenlik sorumlusu kaydı için Azure Key Vault kullanma](../key-vault/general/authentication.md#app-identity-and-security-principals)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>KY-3: Uygulama erişiminde Azure AD çoklu oturum açmayı (SSO) kullanma

**Kılavuz**: Azure Uygulama yapılandırması, Azure kaynaklarına, bulut uygulamalarına ve şirket içi uygulamalara kimlik ve erişim yönetimi sağlamak için Azure Active Directory (Azure AD) kullanır. Bu hem çalışanlar gibi kurumsal kimlikleri hem de iş ortakları, satıcılar ve sağlayıcılar gibi dış kimlikleri içerir. Azure AD, eşitlenen tüm kurumsal Active Directory kimliklerini kullanarak Azure portal aracılığıyla uygulama yapılandırma hizmetini yönetmek için çoklu oturum açma (SSO) sağlar. Sorunsuz, güvenli erişim, daha fazla görünürlük ve denetim için tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD’ye bağlayın.

- [Azure AD ile Uygulama SSO’yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>KY-4: Tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulaması denetimlerini kullanma

**Kılavuz**: Azure Uygulama yapılandırması, çok faktörlü kimlik doğrulaması (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulama denetimlerini destekleyen Azure Active Directory kullanır.
- Çok faktörlü kimlik doğrulaması - Azure AD MFA’yı etkinleştirin ve MFA kurulumunuzda izleyebileceğiniz en iyi yöntemlerden bazıları için Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerine uyun. MFA oturum açma koşulları ve risk faktörleri temelinde tüm kullanıcılarda, seçilen kullanıcılarda veya tek tek kullanıcılar düzeyinde zorunlu tutulabilir.
- Parolasız kimlik doğrulaması – Üç parolasız kimlik doğrulaması seçeneği kullanılabilir: İş İçin Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri.

Yönetici ve ayrıcalıklı kullanıcılar için, en üst düzey güçlü kimlik doğrulama yönteminin kullanıldığından emin olun ve ardından uygun güçlü kimlik doğrulama ilkesini diğer kullanıcılara teslim edin.

Not: MFA, uygulama yapılandırmasına erişen ve yöneten, ancak programlı hizmet hesaplarında bulunmayan Kullanıcı hesaplarında zorlanabilir. Mümkün olduğunca Yönetilen kimlikler gibi passwordless kimlik doğrulaması kullanın ve herhangi bir kullanıcı hesabında MFA 'yı zorlayın.

- [Azure’da MFA’yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Active Directory için parolasız kimlik doğrulaması seçeneklerine giriş](../active-directory/authentication/concept-authentication-passwordless.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>KY-5: Hesap anomalilerini izleme ve uyarı verme

**Kılavuz**: Azure Uygulama yapılandırması, aşağıdaki veri kaynaklarını sağlayan Azure Active Directory ile tümleşiktir:

-   Oturum açma bilgileri – Oturum açma bilgileri raporu, yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.

-   Denetim günlükleri-Azure AD 'deki çeşitli özelliklerle yapılan tüm değişiklikler için Günlükler aracılığıyla izlenebilirlik sağlar. Günlüğe kaydedilen değişiklikler denetim günlüklerine örnek olarak Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma sayılabilir.

-   Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

-   Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Bu veri kaynakları Azure Izleyici, Azure Sentinel veya üçüncü taraf SıEM sistemleriyle tümleştirilebilir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. 

Azure Gelişmiş tehdit koruması (ATP), gelişmiş tehditleri, güvenliği aşılmış kimlikleri ve kötü amaçlı Insider eylemlerini belirlemek, algılamak ve araştırmak için şirket içi Active Directory sinyalleri kullanan bir güvenlik çözümüdür.

- [Azure AD 'de denetim etkinlik raporları](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

- [Azure Güvenlik Merkezi’nin tehdit analizi koruma modülündeki uyarılar](../security-center/alerts-reference.md)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Azure AD Kimlik Koruması verileri bağlama](../sentinel/connect-azure-ad-identity-protection.md)

- [Azure Gelişmiş Tehdit Koruması](/azure-advanced-threat-protection/what-is-atp)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: koşullara göre Azure kaynak erişimini kısıtlama

**Kılavuz**: Azure Uygulama yapılandırması, Kullanıcı tanımlı koşullara göre daha ayrıntılı erişim denetimi için Azure Active Directory (Azure AD) koşullu erişimini destekler (örneğin, belirli IP aralıklarından Kullanıcı oturumu açma işlemleri gibi), oturum açma için MFA kullanılması gerekir. Ayrıntılı kimlik doğrulama oturumu yönetimi ilkesi, farklı kullanım durumları için de kullanılabilir. Bu koşullu erişim ilkeleri yalnızca, uygulama yapılandırma hizmetine erişmek ve bunları yönetmek için Azure AD kimlik doğrulaması yapan kullanıcı hesapları için geçerlidir, ancak bunlar yapılandırma kaynağına bağlanan hizmet sorumluları veya bağlantı dizelerine uygulanmaz.

- [Azure koşullu erişimine genel bakış](../active-directory/conditional-access/overview.md)

- [Ortak koşullu erişim ilkeleri](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-7-eliminate-unintended-credential-exposure"></a>Im-7: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Kılavuz**: Azure Uygulama yapılandırması, müşterilerin kimlikleri veya gizli dizileri içerebilen yapılandırmaları depolamasına izin verir. Yapılandırmaların içindeki kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı uygulamanız önerilir. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Azure Key Vault ile birlikte Azure uygulama yapılandırma hizmetini kullanın. Key Vault tüm kimlik bilgilerini depolayın, ardından uygulama yapılandırma kaynağınız içinde bir Key Vault başvurusu oluşturarak bu kimlik bilgilerine bağlayın. Uygulama yapılandırması bu başvuruları oluşturduğunda, değerlerinin kendileri yerine Key Vault değerlerinin URI 'Lerini depolar. Uygulamalar, Key Vault kimlik bilgilerini almak için uygulama yapılandırmasına bağlanabilir.

GitHub için, kod içindeki kimlik bilgilerini veya diğer gizli dizileri belirlemek için yerel gizli anahtar tarama özelliğini kullanabilirsiniz.

- [ASP.NET Core uygulamasında Key Vault başvurularını kullanma öğreticisi](use-key-vault-references-dotnet-core.md)

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [GitHub gizli taraması](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="privileged-access"></a>Ayrıcalıklı Erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Ayrıcalıklı Erişim](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>AE-1: Yüksek ayrıcalıklı kullanıcıları koruma ve sınırlama

**Rehberlik**: Bu ayrıcalığa sahip olan kullanıcılar Azure ortamınızdaki her kaynağı doğrudan veya dolaylı olarak okuyup değiştirebildiğinden, yüksek ayrıcalıklı hesap veya rol sayısını sınırlayın ve bu hesapları yükseltilmiş bir düzeyde koruyun.

Azure AD Privileged Identity Management’ı (PIM) kullanarak Azure kaynaklarına ve Azure AD’ye tam zamanında (JIT) ayrıcalıklı erişimi etkinleştirebilirsiniz. JIT yalnızca kullanıcıların ihtiyacı olduğunda ayrıcalıklı görevler gerçekleştirmeleri için geçici izinler verir. Azure AD kuruluşunuzda güvenli olmayan veya şüpheli etkinlikler olduğunda da PIM güvenlik uyarıları oluşturabilir.

Erişim tuşları son derece ayrıcalıklı ve en iyi güvenlik uygulaması olarak düzenli olarak döndürülmelidir. Erişim anahtarları, kimlik bilgisi bilgilerini içeren ve gizli dizi olarak kabul edilen bağlantı dizelerini içerir. Bu gizli dizileri Azure Key Vault depolanması gerekir ve kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir. Erişim anahtarları, bir uygulamaya okuma yazma veya okuma erişimi verebilir. Yetkisiz erişimi engellemek için doğru erişim anahtarı türünün verildiğinden emin olun. Daha güvenli hale getirmek için Azure AD 'de Yönetilen kimlikler özelliğini kullanın. Bu yalnızca uygulamaların yapılandırma değerlerine erişmek için yapılandırma uç noktası URL 'sine sahip olmasını gerektirir.

- [Uygulama yapılandırması En Iyi uygulamaları](howto-best-practices.md#app-configuration-bootstrap)

- [Uygulama Yapılandırması’na erişmek için yönetilen kimlikleri kullanma](howto-integrate-azure-managed-service-identity.md)
- [Azure AD'de yönetici rolü izinleri](../active-directory/roles/permissions-reference.md)

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../active-directory/roles/security-planning.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Kılavuz**: Azure Uygulama yapılandırması, ayrıcalıklı erişim verilen hesaplara kısıtlama yaparak iş açısından kritik sistemlere erişimi yalıtmak IÇIN Azure RBAC kullanır. Azure RBAC, kaynak düzeyinde uygulama yapılandırması tarafından desteklenir. Güvenli siloda iş açısından kritik yapılandırmalar bu bilgileri kendi uygulama yapılandırma kaynağında depolar. Bir kaynak içinde, yalnızca okuma erişim hesapları veya anahtarları ile etiketleme ve etiketleme aracılığıyla ayrıntılı erişim de mevcuttur.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

- [Azure AD kullanarak RBAC 'yi uygulama yapılandırmasıyla tümleştirme](concept-enable-rbac.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>AE-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve uzlaştırma

**Kılavuz**: Azure Uygulama yapılandırması, kaynaklarını yönetmek için Azure Active Directory (Azure AD) hesapları kullanır, hesapların ve erişiminin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. 

Azure, Azure AD ve OAuth kullanarak uygulama yapılandırma verilerine erişim yetkisi veren aşağıdaki Azure yerleşik rollerini sağlar:

- Uygulama yapılandırma verileri sahibi: uygulama yapılandırma verilerine okuma/yazma/silme erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.

- Uygulama yapılandırma veri okuyucusu: uygulama yapılandırma verilerine okuma erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez

Azure AD erişim incelemelerini, grup üyeliklerini, kurumsal uygulamalara erişimi ve yukarıdaki uygulama yapılandırma rolleri gibi rol atamalarını gözden geçirmek için kullanabilirsiniz. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.

Note: Yönetilen kimlikler, uygulama yapılandırmasında başka bir hizmet veya uygulamadan kimlik doğrulaması için mümkün olan durumlarda önerilir. Kullanıldığında, uygulama yapılandırmasına erişim ile yapılandırılmış tüm hizmet sorumlularını veya bağlantı dizelerini yönetmeniz gerekir.

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Azure AD kimlik ve erişim gözden geçirmelerini kullanma](/azure/active-directory/governance/access-reviews-overvie)

- [Azure AD kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme](concept-enable-rbac.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Azure AD 'de acil durum erişimi ayarlama

**Rehberlik**: Azure Uygulama yapılandırması, kaynaklarını yönetmek için Azure Active Directory ile tümleşiktir. Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.

Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](../active-directory/roles/security-emergency-access.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-5-automate-entitlement-management"></a>PA-5: yetkilendirme yönetimini otomatikleştirme 

**Rehberlik**: Azure Uygulama yapılandırması, kaynaklarını yönetmek için Azure Active Directory ile tümleşiktir. Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere, erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir.

- [Azure AD erişim gözden geçirmeleri nelerdir?](../active-directory/governance/access-reviews-overview.md)

- [Azure AD yetkilendirme yönetimi nedir?](../active-directory/governance/entitlement-management-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

**Rehberlik**: güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Uygulama yapılandırmasıyla ilgili yönetim görevleri için yüksek düzeyde güvenli Kullanıcı iş istasyonları ve/veya Azure savunma kullanın. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory, Microsoft Defender Gelişmiş tehdit koruması (ATP) ve/veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri, kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir.

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../active-directory/devices/concept-azure-managed-workstation.md) 

- [Ayrıcalıklı erişim iş istasyonu dağıtma](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>AE-7: Tam yetecek kadar yönetim uygulama (en düşük ayrıcalık ilkesi) 

**Kılavuz**: Azure Uygulama yapılandırması, kaynaklarını yönetmek için Azure rol tabanlı erişim denetımı (RBAC) ile tümleşiktir. Azure RBAC ağ atamaları aracılığıyla Azure kaynak erişimini yönetmenize olanak tanır. Bu rolleri kullanıcılara atayabilir, hizmet sorumlularını ve yönetilen kimlikleri gruplara atayabilirsiniz. Azure Uygulama yapılandırması için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıklar her zaman yalnızca söz konusu rollere gerekenlerle sınırlanmalıdır. Bu yöntem Azure AD Privileged Identity Management’ın (PIM) tam zamanında (JIT) yaklaşımını tamamlayıcı niteliktedir ve düzenli aralıklarla gözden geçirilmelidir.

Azure, Azure AD ve OAuth kullanarak uygulama yapılandırma verilerine erişim yetkisi veren aşağıdaki Azure yerleşik rollerini sağlar:
- Uygulama yapılandırma verileri sahibi: uygulama yapılandırma verilerine okuma/yazma/silme erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.
- Uygulama yapılandırma veri okuyucusu: uygulama yapılandırma verilerine okuma erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.

İzin ayırmak için yerleşik roller kullanın ve gerektiğinde yalnızca özel roller oluşturun. 

Uygulama yapılandırması, birden çok uygulamanın yapılandırmasını tek bir uygulama yapılandırma kaynağında depolamayı destekler. Uygulamalar arasında bilgilere erişimi sınırlandırmak için, uygulama başına bir uygulama yapılandırma kaynağı oluşturun ve Azure RBAC 'yi uygun şekilde ayarlayın.

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../role-based-access-control/overview.md)

- [Azure’da RBAC’yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

- [Azure AD kimlik ve erişim gözden geçirmelerini kullanma](../active-directory/governance/access-reviews-overview.md)

- [Azure AD kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme](concept-enable-rbac.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-8-choose-approval-process-for-microsoft-support"></a>PA-8: Microsoft desteği için onay işlemini seçin  

**Rehberlik**: Microsoft 'un uygulama yapılandırma verilerinize erişmesi gerekebilecek destek senaryoları için bir kuruluş onay süreci uygulayın. Müşteri Kasası, uygulama yapılandırma desteği senaryolarında Şu anda kullanılamıyor.

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Veri Koruma](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-1-discover-classify-and-label-sensitive-data"></a>DP-1: hassas verileri bulma, sınıflandırma ve etiketleme

**Rehberlik**: hassas bilgilerin depolanmasını, işlenmesini ve kuruluşun teknoloji sistemleri tarafından güvenli bir şekilde aktarılmasını sağlamak üzere uygun denetimleri tasarlayabilmeniz için hassas verilerinizi bulun, sınıflandırın ve etiketleyin. Etiketleme biçimindeki gizli bilgilerin etiketlenmesi, uygulama yapılandırma kaynakları için desteklenir, ancak bunlar içinde yer alan yapılandırma değerleri için desteklenmez. Bir uygulamanın bir yapılandırma deposuna okuma veya okuma/yazma erişimi olduğunda, bu depodaki yapılandırmaların herhangi birine tam erişimi vardır.

- [Azure Information Protection kullanarak hassas bilgileri etiketleme](/azure/information-protection/what-is-information-protection)

- [Azure 'da veri sınıflandırmalarını etiketleme](/azure/cloud-adoption-framework/govern/policy-compliance/data-classification#tagging-data-classification-in-azure)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="dp-2-protect-sensitive-data"></a>VK-2: Hassas verileri koruma

**Kılavuz**: Microsoft tarafından yönetilen temel platform Için, Microsoft, tüm müşteri içeriklerini müşteri veri kaybına ve pozlamaya karşı hassas ve koruyucuları olarak değerlendirir. Azure içindeki müşteri verilerinin güvenlik altında kalmasını sağlamak için Microsoft bazı varsayılan veri koruma denetimleri ve özellikleri uygulamıştır. Erişim anahtarlarını uygulama yapılandırma kaynaklarınıza düzenli olarak döndürtığınızdan emin olun. Bağlantı dizelerinden gelen kimlik bilgileri Azure Key Vault içinde depolanabilir ve kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir. Erişim anahtarları, bir uygulamaya okuma yazma veya okuma erişimi verebilir. Yetkisiz erişimi engellemek için doğru erişim anahtarı türünün verildiğinden emin olun. Daha güvenli hale getirmek için Azure AD 'de Yönetilen kimlikler özelliğini kullanın. Bu yalnızca uygulamaların yapılandırma değerlerine erişmek için yapılandırma uç noktası URL 'sine sahip olmasını gerektirir.

Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak erişimi kısıtla:

- Hassas verileri kendi uygulama yapılandırma kaynağına ayırın, ardından yalnızca yetkili erişimin etkin olması için RBAC ilkelerini uygun şekilde ayırın 

- Ağ tabanlı erişim denetimlerini kullanma

- Azure hizmetlerindeki belirli denetimler (SQL ve diğer veritabanlarında şifreleme gibi) ve tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- Ayrıca kurumsal segmentasyon stratejisi, hassas ve iş açısından kritik verilerle sistemlerin konumu hakkında da bilgilendirilmelidir.

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Active Directory kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme](concept-enable-rbac.md)

- [Uygulama yapılandırma veri şifrelemesi](faq.md#does-app-configuration-encrypt-my-data)

- [Azure Rol Tabanlı Erişim Denetimi (RBAC)](../role-based-access-control/overview.md) 

- [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: geçiş sırasında hassas bilgileri şifreleyin

**Kılavuz**: erişim denetimlerini tamamlamak için, yoldaki verilerin şifreleme kullanılarak ' bant dışı ' saldırılarına karşı korunması gerekir. Bu, saldırganların verileri kolayca okuyabilmesini veya değiştirememesini sağlamaya yardımcı olur.

Azure Uygulama yapılandırması tüm HTTP istekleri için TLS şifrelemesini kullanır. Azure altyapısı, Azure veri merkezleri arasındaki tüm istekler için ağ düzeyinde şifreleme için eklenen bir katman sağlar. Uygulama yapılandırma kaynaklarınıza bağlanan istemcilerin TLS v 1.2 veya üzerini anlaşamayacağı HTTP trafiğinden emin olun.

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>VK-5: Bekleyen hassas verileri şifreleme

**Kılavuz**: erişim denetimlerini tamamlamak için, Rest 'teki verilerin şifreleme kullanılarak ' bant dışı ' saldırılarına (temeldeki depolamaya erişme gibi) karşı korunması gerekir. Bu, saldırganların verileri kolayca okuyabilmesini veya değiştirememesini sağlamaya yardımcı olur.

Azure, varsayılan olarak bekleyen şifreleme verileri sağlar. Son derece hassas veriler için, kullanılabilir olduğunda tüm Azure kaynaklarında bekleyen ek şifreleme uygulama seçenekleriniz vardır. Azure, şifreleme anahtarlarınızı varsayılan olarak yönetir, ancak Azure, Azure Uygulama yapılandırması için kendi anahtarlarınızı (müşteri tarafından yönetilen anahtarlar) yönetme seçeneği sunar.

- [Azure Uygulama yapılandırmasındaki verilerinizi şifrelemek için müşteri tarafından yönetilen anahtarları kullanın](concept-customer-managed-keys.md)

- [Azure’da bekleyen veri şifrelemesini anlama](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [Şifreleme modeli ve anahtar yönetimi tablosu](../security/fundamentals/encryption-atrest.md#azure-resource-providers-encryption-model-support)

- [Azure 'da Rest çift Şifrelemeli veriler](../security/fundamentals/double-encryption.md#data-at-rest)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Varlık Yönetimi](../security/benchmarks/security-controls-v2-asset-management.md).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>VY-1: Güvenlik ekibinin varlıklara yönelik risklerde görünürlük elde etmesini sağlama

**Rehber**: Güvenlik ekiplerinin Azure Güvenlik Merkezi’ni kullanarak güvenlik risklerini izlemesini sağlamak için bu ekiplere Azure kiracınız ve abonelikleriniz üzerinde Güvenlik Okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibi sorumluluklarının yapılandırmasına bağlı olarak, güvenlik risklerinin izlenmesi merkezi güvenlik ekibinin veya yerel bir ekibin sorumluluğunda olabilir. Gerçi güvenlik içgörüleri ve risklerinin kuruluş içinde her zaman merkezi bir konumda toplanması gerekir. 

Güvenlik Okuyucusu izinleri kiracının tamamına (Kök Yönetim Grubu) geniş kapsamlı olarak uygulanabileceği gibi, izinlerin kapsamı yönetim gruplarıyla veya belirli aboneliklerle de sınırlanabilir. 

Not: İş yükleri ve hizmetlerin görünürlüğünü elde etmek için ek izinler gerekebilir. 

- [Güvenlik Okuyucusu Rolüne Genel Bakış](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Gruplarına Genel Bakış](../governance/management-groups/overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>-2: güvenlik ekibinin varlık envanterini ve meta verileri erişimine sahip olduğundan emin olun

**Kılavuz**: Azure Uygulama yapılandırması gibi güvenlik ekiplerinin Azure 'daki varlıkların sürekli güncelleştirilmiş envanterini erişimi olduğundan emin olun. Güvenlik ekipleri genellikle bu envanterin, kuruluşlarının riskli olma olasılığını değerlendirmek ve sürekli güvenlik geliştirmeleri için bir girdi olarak gereklidir. Kuruluşunuzun yetkili güvenlik ekibini içerecek bir Azure Active Directory grubu oluşturun ve bunlara tüm Azure uygulama yapılandırma kaynaklarına okuma erişimi atayın, bu, aboneliğinizde tek bir üst düzey rol ataması tarafından basitleştirilebilir.

Azure Güvenlik Merkezi envanter özelliği ve Azure Kaynak Grafiği, aboneliklerinizde Azure Hizmetleri, uygulamalar ve ağ kaynakları dahil tüm kaynakları sorgulayabilir ve bulabilir.

Bunları bir taksonomiye mantıksal olarak düzenlemek için Azure kaynaklarınıza, kaynak gruplarına ve aboneliklerinize Etiketler uygulayın. Her etiket bir ad ve bir değer çiftinden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../security-center/asset-inventory.md)

- [Varlıkları etiketleme hakkında daha fazla bilgi için bkz. Kaynak adlandırma ve etiketleme karar Kılavuzu](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

**Kılavuz**: Azure Uygulama yapılandırması, Azure ilkesi kullanarak Azure Resource Manager tabanlı dağıtımları ve yapılandırma zorlamayı destekler. Kullanıcıların ortamınızda hangi hizmetleri sağlayabileceğini denetlemek ve kısıtlamak için Azure Ilkesi ' ni kullanın. Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma. Onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/built-in-policies.md#general)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>VY-4: Varlık yaşam döngüsü yönetiminin güvenliğini sağlama

**Rehber**: Çok etkili olabilecek değişiklikler için varlık yaşam döngüsü yönetim süreçleri sağlayan güvenlik ilkelerini oluşturun veya güncelleştirin. Bu değişiklikler değişiklikleri içerir, ancak bunlarla sınırlı değildir: kimlik sağlayıcıları ve erişim, veri duyarlılığı, ağ yapılandırması ve yönetim ayrıcalıkları atama.

Artık gerekmeyen Azure kaynaklarını kaldırın. Yöneticilerin yalnızca kimliği doğrulanmış kullanıcıların yapılandırma kaynaklarına erişimi olduğundan emin olmak için erişim anahtarlarını düzenli olarak döndürürken emin olun.

- [Uygulama yapılandırması için kullanılan şifreleme anahtarlarını döndürün](concept-customer-managed-keys.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisiyle etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe Kaydetme ve Tehdit Algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Günlüğe Kaydetme ve Tehdit Algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>GT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Rehberlik**: uygulama yapılandırması Azure Active Directory (Azure AD) ile tümleşir. Bu, daha karmaşık izleme ve analiz kullanım örnekleri için Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel veya diğer SıEM/izleme araçlarıyla tümleştirilen aşağıdaki kullanıcı günlüklerini sağlar:
- Oturum açma bilgileri – Oturum açma bilgileri raporu, yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.
- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki herhangi bir kaynakta yapılan kullanıcı, uygulama, grup, rol ve ilkeleri ekleme veya kaldırma işlemleri gibi değişiklikler, denetim günlüklerine örnek gösterilebilir.
- Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.
- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi aşırı fazla sayıda başarısız kimlik doğrulaması girişimi, abonelikte kullanım dışı kalan hesaplar gibi bazı şüpheli etkinliklerde de uyarı verebilir. Azure Güvenlik Merkezi 'nin tehdit koruması modülü, temel güvenlik durumu izlemeye ek olarak Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları da toplayabilirler. Bu özellik, bireysel kaynakların içindeki hesap bozukluklarını ayrıntılı olarak sunar.

Uygulama yapılandırma yapılandırma kaynağına erişim kazanmanız gereken başka bir yöntem ise erişim anahtarlarını kullanmaktır. Bu, yetkisiz aracıların yapılandırma kaynağına erişimi olmadığından emin olmak için düzenli olarak döndürülmelidir. Bunları döndürmek, doğrudan portalda "erişim tuşları" altında yapılabilir.

- [Azure uygulama yapılandırmasının yönetilen bir kimlik kullanarak diğer Azure AD korumalı kaynaklarına erişmesine izin vermek için](overview-managed-identity.md)

- [Azure Uygulama yapılandırması ile yönetilen kimlikler kullanma](howto-integrate-azure-managed-service-identity.md)

- [Azure Active Directory’de denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure Kimlik Koruması’nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure AD kullanarak Azure Uygulama yapılandırmasına erişimi yetkilendirme](concept-enable-rbac.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Azure ağ etkinlikleri için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Azure Uygulama yapılandırması, hiçbir kaynağı doğrudan bir sanal ağa dağıtmaz. Ancak, uygulama yapılandırması, bir sanal ağdan Azure Uygulama yapılandırmasına güvenli bir şekilde bağlanmak için özel uç noktaları kullanmanıza olanak sağlar. Azure Uygulama yapılandırması Ayrıca etkinleştirilmesi gereken DNS sorgu günlüklerini oluşturmaz veya işlemez.

Yapılandırılmış uygulama yapılandırması özel uç noktalarında yakalamak için günlük kaydını etkinleştirin:
- Özel uç nokta tarafından işlenen veriler (ın/OUT)
- Özel bağlantı hizmeti tarafından işlenen veriler (ın/OUT)
- NAT bağlantı noktası kullanılabilirliği

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure özel bağlantı Izleme](../private-link/private-link-overview.md#logging-and-monitoring)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: otomatik olarak kullanılabilen etkinlik günlükleri, okuma IŞLEMLERI (Get) haricinde uygulama yapılandırma kaynaklarınız için tüm yazma IŞLEMLERINI (put, Post, Delete) içerir. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir. Uygulama yapılandırması için, etkinlik günlükleri yalnızca denetim düzlemine kullanılabilir ve Azure Resource Manager (ARM) tarafından ortaya çıkmış. Uygulama yapılandırması için müşteriye yönelik veri düzlemi günlüğü şu anda desteklenmiyor. Azure Kaynak günlükleri de yapılandırılamaz.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik**: bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atadığınızdan emin olun.

Azure etkinlik günlüklerini merkezi günlüklerinizi tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin. Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: günlük depolama bekletmesini yapılandırma

**Rehberlik**: uygulama yapılandırma günlüklerini depolamak için kullanılan tüm depolama hesaplarının veya Log Analytics çalışma alanlarının, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun. Uzun süreli ve arşiv depolama için Azure depolama, Data Lake veya Log Analytics çalışma alanı hesapları kullanın.

Azure Izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayabilirsiniz.

- [Log Analytics çalışma alanı saklama süresini yapılandırma](../azure-monitor/platform/manage-cost-storage.md)

- [Kaynak günlüklerini bir Azure depolama hesabında depolama](../azure-monitor/platform/resource-logs.md#send-to-azure-storage)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Olay Yanıtı](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>OY-1: Hazırlık – Azure için olay yanıtı sürecini güncelleştirme

**Rehber**: Kuruluşunuzda güvenlik olaylarını yanıtlama süreçleri olduğundan, bu süreçlerin Azure için güncelleştirildiğinden ve hazırlıklı olmak için bu süreçlerin düzenli olarak uygulandığından emin olun.

- [Kurumsal ortam genelinde güvenlik uygulama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ir-2-preparation--setup-incident-notification"></a>OY-2: Hazırlık – olay bildirimi ayarlama

**Rehber**: Azure Güvenlik Merkezi’nde güvenli olayı iletişim bilgilerini ayarlayın. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca olay yanıtı gereksinimleriniz temelinde farklı Azure hizmetlerinde olay uyarı ve bildirimlerini özelleştirme seçenekleriniz de vardır. 

- [Azure Güvenlik Merkezi güvenlik iletişimini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>OY-3: Algılama ve analiz – yüksek kaliteli uyarılar temelinde olaylar oluşturma

**Rehberlik**: yüksek kaliteli uyarılar oluşturmak ve uyarı kalitesini ölçmek için bir işleminiz olduğundan emin olun. Bu sayede geçmiş olaylardan ders çıkarabilir ve analistler için uyarıların önceliğini belirleyerek yanlış pozitif sonuçlarla zaman kaybetmelerini önleyebilirsiniz. 

Yüksek kaliteli uyarılar, geçmiş olaylar, doğrulanan topluluk kaynakları ve uyarılar oluşturmak ve temizlemek için tasarlanan araçlar ve farklı sinyal kaynaklarını kullanarak oluşturulabilir. 

Azure Güvenlik Merkezi, birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. ASC veri bağlayıcısını kullanarak uyarıların Azure Sentinel’e akışını yapabilirsiniz. Azure Sentinel bir araştırmaya yönelik olayların otomatik olarak oluşturulması için gelişmiş uyarı kuralları oluşturur. 

Azure kaynaklarına yönelik riskleri tanımlamaya yardımcı olmak için, dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya sürekli bir biçimde dışarı aktarabilirsiniz.

- [Dışarı aktarma işlemini yapılandırma](../security-center/continuous-export.md)

- [Uyarıların Azure Sentinel’e akışını yapma](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>OY-4: Algılama ve analiz – olayı araştırma

**Rehber**: Analistlerin olası olayları araştırırken neler olup bittiğini tam olarak görmek için çeşitli veri kaynaklarını sorgulayabildiğinden ve kullanabildiğinden emin olun. Sonlandırma zinciri boyunca olası saldırganın etkinliklerinin izlenebilmesi için çeşitli günlükler toplanmalı, kör noktaların olması önlenmelidir.  Ayrıca diğer analistler için ve gelecekte başvurmak üzere içgörülerin ve çıkarılan derslerin yakalandığından da emin olmalısınız.  

Araştırmaya yönelik veri kaynakları kapsam içindeki hizmetlerden ve çalışan sistemlerden zaten toplanmakta olan merkezi günlük kaynaklarının yanı sıra şunları da içerebilir:

- Ağ verileri – ağ akış günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik gruplarının akış günlüklerini, Azure Ağ İzleyicisi’ni ve Azure İzleyici’ni kullanın. 

- Çalışan sistemlerin anlık görüntüleri: 

    - Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

    - Çalışan sistemin belleğinin anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

    - Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel hemen her veri kaynağı üzerinde kapsamlı veri analizi gerçekleştirdiği gibi, olayların tüm yaşam döngüsünü yöneten bir olay yönetim portalı da sağlar. Araştırma sırasında elde edilen inceleme bilgileri izleme ve raporlama amacıyla bir olayla ilişkilendirilebilir. 

- [Windows makinesi diskinin anlık görüntüsü](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesi diskinin anlık görüntüsü](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure Desteği tanılama bilgileri ve bellek dökümü toplama](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırma](../sentinel/tutorial-investigate-cases.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>OY-5: Algılama ve analiz – olayların önceliklerini belirleme

**Rehber**: Uyarı önem derecesi ve uyarı hassasiyeti temelinde analistlere önce hangi olaylara odaklanılacağı konusunda bağlam sağlayın. 

Azure Güvenlik Merkezi önce hangi uyarıların araştırılması gerektiğini belirlemenize yardımcı olmak için her uyarıya bir önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analizde ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin yanı sıra güven düzeyini temel alır.

Buna ek olarak Azure kaynaklarını, özellikle hassas verileri işleyen kaynakları belirlemek ve kategorilere ayırmak için etiketleri kullanarak kaynakları işaretleyin ve bir adlandırma sistemi oluşturun.  Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>OY-6: Sınırlama, yok etme ve kurtarma – olay işlemeyi otomatikleştirme

**Rehber**: Yanıt süresini kısaltmak ve analistlerin önündeki engelleri azaltmak için el ile gerçekleştirilen yinelemeli görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütme süresi daha uzun olduğundan her olayı yavaşlatır ve bir analistin işleyebileceği olay sayısını azaltır. Ayrıca el ile gerçekleştirilen görevler analistler açısından daha yorucudur. Dolayısıyla gecikmelere neden olan insan hatası riskini artırır ve analistlerin karmaşık görevlere etkili bir şekilde odaklanma becerisini azaltır. Eylemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarını yanıtlamak üzere bir playbook çalıştırmak için Azure Güvenlik Merkezi’nin ve Azure Sentinel’in iş akışı otomasyonu özelliklerini kullanın. Playbook bildirimleri gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi eylemleri gerçekleştirir. 

- [Güvenlik Merkezi’nde iş akışı otomasyonunu yapılandırma](../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi’nde otomatik tehdit yanıtlarını ayarlama](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel’de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="posture-and-vulnerability-management"></a>Duruş ve Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Duruş ve Güvenlik Açığı Yönetimi](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>BD-1: Azure hizmetleri için güvenli yapılandırma oluşturma 

**Kılavuz**: Azure Uygulama yapılandırması, Azure kaynaklarınızın yapılandırmalarını denetlemek ve zorlamak Için Azure Güvenlik Merkezi 'nde mevcut olan hizmete özgü ilkelerin altına de olanak sağlar. Bu, Azure Güvenlik Merkezi veya Azure Ilke girişimleri içinde yapılandırılabilir.
- Uygulama yapılandırması, müşteri tarafından yönetilen anahtar kullanmalıdır: müşteri tarafından yönetilen anahtarlar, şifreleme anahtarlarınızı yönetmenize olanak tanıyarak gelişmiş veri koruması sağlar. Bu, genellikle uyumluluk gereksinimlerini karşılamak için gereklidir.
- Uygulama yapılandırması özel bir bağlantı kullanmalıdır: özel uç nokta bağlantıları bir sanal ağdaki istemcilerin, Azure Uygulama yapılandırmasına özel bir bağlantı üzerinden güvenli bir şekilde erişmesini sağlar.

Azure şemaları, Azure kaynakları Yöneticisi şablonları, Azure RBAC denetimleri ve ilkeleri de dahil olmak üzere hizmetlerin ve uygulama ortamlarının dağıtımını ve yapılandırmasını tek bir şema tanımında otomatik hale getirmek için kullanabilirsiniz.

- [Uygulama yapılandırma ilkeleriyle ilgili daha fazla bilgi](../governance/policy/samples/built-in-policies.md#app-configuration)

- [Azure Güvenlik Merkezi 'nde güvenlik ilkeleriyle çalışma](../security-center/tutorial-security-policy.md)

- [Kurumsal ölçek giriş bölgesinde Guardrayın uygulamasının çizimi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture#landing-zone-expanded-definition)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprints](../governance/blueprints/overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşımlı

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>BD-2: Azure hizmetleri için güvenli yapılandırmalara dayanmalar

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanarak yapılandırma temelinizi Izleyin ve Azure ilkesi 'ni kullanmaya zorlayın. Uygulama yapılandırması için Azure Ilkesi şunları içerir: 
- Uygulama yapılandırması, müşteri tarafından yönetilen anahtar kullanmalıdır: müşteri tarafından yönetilen anahtarlar, şifreleme anahtarlarınızı yönetmenize olanak tanıyarak gelişmiş veri koruması sağlar. Bu, genellikle uyumluluk gereksinimlerini karşılamak için gereklidir.
- Uygulama yapılandırması özel bir bağlantı kullanmalıdır: özel uç nokta bağlantıları bir sanal ağdaki istemcilerin, Azure Uygulama yapılandırmasına özel bir bağlantı üzerinden güvenli bir şekilde erişmesini sağlar.

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md) 

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşımlı

### <a name="pv-8-conduct-regular-attack-simulation"></a>DG-8: Düzenli saldırı simülasyonu çalıştırma

**Rehber**: Gerektiğinde Azure kaynaklarınızda sızma testi veya kırmızı ekip etkinlikleri çalıştırın ve tüm kritik güvenlik bulgularının düzeltildiğinden emin olun.
Sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak için Microsoft Cloud Sızma Testi Angajman Kuralları’na uyun. Microsoft tarafından yönetilen bulut altyapısında, hizmetlerde ve uygulamalarda Microsoft'un stratejisini ve Kırmızı Ekip ile canlı site sızma testi yürütmesini kullanın.

- [Azure’da sızma testi](../security/fundamentals/pen-testing.md)

- [Sızma Testi Angajman Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Kırmızı Ekibi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

## <a name="backup-and-recovery"></a>Yedekleme ve Kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: yedekleme ve kurtarma](../security/benchmarks/security-controls-v2-backup-recovery.md).*

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: kayıp anahtarların riskini azaltma

**Rehberlik**: anahtar kaybını engellemek ve kurtarmak için ölçülerde yer aldığından emin olun. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin.

- [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: İdare ve Strateji](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>İS-1: Varlık yönetimi ve veri koruma stratejisini tanımlama 

**Rehber**: Sistemlerin ve verilerin sürekli izlenmesi ve korunması için tek bir stratejiyi belgelediğinizden ve ilettiğinizden emin olun. İş açısından kritik veriler ve sistemlerin bulunması, değerlendirilmesi, korunması ve izlenmesine öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   İş risklerine uygun olarak veri sınıflandırma standardı

-   Riskler ve varlık envanterinde güvenlik organizasyonu görünürlüğü 

-   Kullanılacak Azure hizmetleri konusunda güvenlik organizasyonu onayı 

-   Varlıkların yaşam döngüleri boyunca güvenliği

-   Kurumsal verilerin sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özellikleri kullanımı

-   Aktarımdaki ve bekleyen veri kullanım örneklerine yönelik veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik Mimarisi Önerisi - Depolama, veri ve şifreleme](/azure/architecture/framework/security/storage-data-encryption?amp;bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Azure Güvenlik Temelleri - Azure Veri güvenliği, şifrelemesi ve depolaması](../security/fundamentals/encryption-overview.md)

- [Bulut Benimseme Çerçevesi - Azure veri güvenliği ve şifrelemesi için en iyi yöntemler](../security/fundamentals/data-encryption-best-practices.md?amp;bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Güvenlik Karşılaştırması - Varlık yönetimi](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Güvenlik Karşılaştırması - Veri Koruma](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>İS-2: Kurumsal segmentasyon stratejisini tanımlama 

**Rehber**: Kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimleri kullanarak varlıklara erişimi segmentlere ayırmak için kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı ihtiyacı ile birbiriyle iletişim kurması ve verilere erişmesi gereken sistemlerin günlük operasyonuna olanak sağlama ihtiyacını dikkatle dengeleyin.

Segmentasyon stratejisinin ağ güvenliği, kimlik ve erişim modelleri, uygulama izni/erişimi modelleri ve insan işlemi denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure’da segmentasyon stratejisi rehberliği (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure’da segmentasyon stratejisi rehberliği (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ağ segmentasyonunu kurumsal segmentasyon stratejisiyle uyumlu hale getirme](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>İS-3: Güvenlik duruşu yönetim stratejisini tanımlama

**Rehber**: Tek tek varlıklarınıza ve bunların barındırıldığı ortama yönelik riskleri sürekli ölçün ve azaltın. Yayımlanmış uygulamalar, ağ giriş ve çıkış noktaları, kullanıcı ve yönetici uç noktaları gibi yüksek değerli varlıklara ve çok fazla kullanıma sunulan yüzeylere öncelik verin.

- [Azure Güvenlik Karşılaştırması - Duruş ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>İS-4: Kuruluş rollerini, sorumluluklarını ve hesap verilebilirliğini uyumlu hale getirme

**Rehber**: Güvenlik kuruluşunuzdaki roller ve sorumluluklar için açık bir stratejiyi belgelediğinizden ve ilettiğinizden emin olun. Paylaşılan sorumluluk modeli için herkesi eğiterek ve bulutun güvenliğini sağlayacak teknolojiler için teknik ekipleri eğiterek güvenlik kararlarında net bir hesap verilebilirlik sağlamaya öncelik verin.

- [Azure Güvenliği En İyi Yöntemi 1 – Kişiler: Bulut Güvenliği Yolculuğunda Ekipleri Eğitme](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenliği En İyi Yöntemi 2 – Kişiler: Bulut Güvenliği Teknolojinde Ekipleri Eğitme](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenliği En İyi Yöntemi 3 – İşlem: Bulut Güvenliği Kararları için Sorumluluk Atama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-5-define-network-security-strategy"></a>İS-5: Ağ güvenliği stratejisini tanımlama

**Rehber**: Kuruluşunuzun genel güvenlik erişim denetimi stratejisinin bir parçası olarak bir Azure ağ güvenliği yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisiyle uyumlu bir sanal ağ segmentasyon modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   İnternet ucu, giriş ve çıkış stratejisi

-   Hibrit bulut ve şirket içi karşılıklı bağlantı stratejisi

-   Güncel ağ güvenliği yapıtları (örneğin ağ diyagramları, referans ağ mimarisi)

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenliği En İyi Yöntemi 11 – Mimari. Tek birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik Karşılaştırması - Ağ Güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>İS-6: Kimlik ve ayrıcalıklı erişim stratejisi tanımlama

**Rehber**: Kuruluşunuzun genel güvenlik erişim denetimi stratejisinin bir parçası olarak bir Azure kimlik ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Merkezi kimlik ve kimlik doğrulaması sistemi ile bu sistemin diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım örnekleri ve koşullarında güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıları koruma

-   Anormal kullanıcı etkinliklerini izleme ve işleme  

-   Kullanıcı kimliği ve erişimini gözden geçirme ve mutabakat süreci

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik Karşılaştırması - Kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik Karşılaştırması - Ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenliği En İyi Yöntemi 11 – Mimari. Tek birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure kimlik yönetimi güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>İS-7: Günlüğe kaydetme ve tehdit yanıtı stratejisini tanımlama

**Rehber**: Tehditleri hızla algılar ve giderirken uyumluluk gereksinimlerini de karşılamak için bir günlüğe kaydetme ve tehdit yanıtı stratejisi oluşturun. Analistlerin tümleştirmeyle ve el ile uygulanan adımlarla ilgilenmek yerine tehditlere odaklanabilmelerini sağlamak için onlara yüksek kaliteli uyarılar ve sorunsuz deneyimler sağlamaya öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Güvenlik operasyonları (SecOps) kuruluşunun rolü ve sorumlulukları 

-   NIST veya başka bir endüstri çerçevesiyle uyumlu, iyi tanımlanmış bir olay yanıt süreci 

-   Tehdit algılama, olay yanıtı ve uyumluluk gereksinimlerini desteklemek için günlük yakalama ve saklama

-   SIEM’yi, yerel Azure özelliklerini ve diğer kaynakları kullanarak tehditlerin merkezi görünürlüğü ve tehditler hakkındaki bağıntı bilgileri 

-   Müşterileriniz, sağlayıcılarınız ve genel olarak ilgili taraflarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, adli araştırma, saldırı düzeltme ve yok etme gibi olayları işlemek için Azure yerel platformunu ve üçüncü taraf platformlarını kullanma

-   Olayları ve olay sonrası çıkarılan dersler ve kanıt saklama gibi etkinlikleri işlemeye yönelik süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik Karşılaştırması - Günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik Karşılaştırması - Olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenliği En İyi Yöntemi 4 – İşlem. Bulut için Olay Yanıtı Süreçlerini Güncelleştirme](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure Benimseme Çerçevesi, günlük ve raporlama karar kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure kurumsal ölçeği, yönetimi ve izlemesi](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Karşılaştırması V2’ye genel bakış](../security/benchmarks/overview.md) konusuna bakın
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin