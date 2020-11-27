---
title: Azure için Azure Güvenlik temeli
description: Azure savunma güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: bastion
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 461d3800491dbe382beecbcdfe1f2a93bd6b2e5c
ms.sourcegitcommit: ab94795f9b8443eef47abae5bc6848bb9d8d8d01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2020
ms.locfileid: "96301572"
---
# <a name="azure-security-baseline-for-azure-bastion"></a>Azure için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' dan Azure 'a kılavuzluk uygular. Azure Güvenlik Karşılaştırması Azure’da bulut çözümlerinizin güvenliğini nasıl sağlayabileceğinize ilişkin öneriler getirir. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure savunma için geçerli olan ilgili kılavuza göre gruplandırılır. Azure savunma için geçerli olmayan **denetimler** dışlandı.

Azure savunma 'nın Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure savunma güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](/azure/security/benchmarks/security-controls-v2-network-security).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: iç trafik için güvenlik uygulama

**Kılavuz**: Azure savunma kaynaklarını dağıtırken, var olan bir sanal ağ oluşturmanız veya kullanmanız gerekir. Tüm Azure sanal ağlarının, iş risklerine göre bir kurumsal kesimleme ilkesini izlediğinden emin olun. Kuruluş için daha yüksek risk doğurabilecek tüm sistemleri kendi sanal ağı içinde yalıtılmalı ve bir ağ güvenlik grubu (NSG) ile yeterince güvenli hale gelmelidir.

Hizmetin düzgün çalışması için, Azure savunma hizmeti 'nin aşağıdaki bağlantı noktalarının açık olması gerekir:

- Giriş trafiği:
   - Genel internet 'ten gelen trafik: Azure savunma, giriş trafiği için genel IP üzerinde bağlantı noktası 443 ' ün etkin olması gereken genel bir IP oluşturacak. 3389/22 numaralı bağlantı noktası, AzureBastionSubnet açık olması gerekmez. 

   - Azure savunma denetim düzleminin giriş trafiği: denetim düzlemi bağlantısı Için GatewayManager hizmet etiketinden gelen bağlantı noktası 443 ' yı etkinleştirin. Bu, denetim düzlemi, diğer bir deyişle, Ağ Geçidi Yöneticisi tarafından Azure ile iletişim kurabiliyor.

- Çıkış trafiği:

   - Hedef sanal makinelere (VM 'Ler) giden çıkış trafiği: Azure savunma, hedef VM 'Lere özel IP üzerinden ulaşacaktır. NSG 'ler, bağlantı noktası 3389 ve 22 için diğer hedef VM alt ağlarına giden trafiğe izin vermek için gerekir.

   - Azure 'da diğer genel uç noktalara giden trafik çıkışı: Azure savunma, Azure 'daki çeşitli genel uç noktalara bağlanabilmelidir (örneğin, tanılama günlüklerini ve ölçüm günlüklerini depolamak için). Bu nedenle, Azure savunma 'nın 443 'e giden ve Azurecı hizmeti etiketi için çıkış yapması gerekir.

Gateway Manager ve Azure hizmet etiketi bağlantısı, Azure sertifikaları tarafından korunur (kilitlidir). Bu kaynakların tüketicileri de dahil olmak üzere dış varlıklar bu uç noktalar üzerinde iletişim kuramaz. 

- [Güvenlik kuralları ile ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

- [Burada savunma NSG gereksinimi hakkında daha fazla bilgi edinebilirsiniz](bastion-nsg.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ns-2-connect-private-networks-together"></a>NS-2: özel ağları birbirine bağlama

**Kılavuz**: Azure savunma, özel bir ağ aracılığıyla erişilebilen herhangi bir uç noktayı kullanıma sunmuyor. Azure savunma, savunma dağıtımınızı merkezileştirmek ve çapraz ağ bağlantısını etkinleştirmek için eşlenmiş bir ağa dağıtımı destekler.

- [Azure savunma ve sanal ağ eşlemesi](vnet-peering.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Kimlik Yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>KY-1: Azure Active Directory’yi merkezi kimlik ve kimlik doğrulaması sistemi olarak standartlaştırma

**Kılavuz**: Azure savunma, Azure 'un varsayılan kimlik ve erişim yönetimi hizmeti olan Azure Active Directory (Azure AD) ile tümleşiktir. Kullanıcılar, Azure savunma hizmetini (oluşturma, güncelleştirme ve silme kaynaklarını) yönetmek için Azure AD kimlik doğrulaması 'nı kullanarak Azure portal erişebilirler.

Azure ile sanal makinelere bağlantı kurma, bir SSH anahtarı veya Kullanıcı adı/parola kullanır ve şu anda Azure AD kimlik bilgilerinin kullanılmasını desteklememektedir.

Bir SSH anahtarına veya Kullanıcı adına/parolaya ek olarak, Azure 'a yönelik sanal makinelere bağlanırken, kullanıcılarınız aşağıdaki rol atamalarına ihtiyaç duyar:
- Hedef sanal makinede okuyucu rolü
- Hedef sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
- Azure Bastion kaynağında okuyucu rolü

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure savunma kullanarak bir Linux sanal makinesine bağlanma](bastion-connect-vm-ssh.md)

- [Azure savunma kullanarak bir Windows sanal makinesine bağlanma](bastion-connect-vm-rdp.md)

- [Azure yerleşik rolleri](../role-based-access-control/built-in-roles.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>KY-3: Uygulama erişiminde Azure AD çoklu oturum açmayı (SSO) kullanma

**Kılavuz**: Azure savunma, sanal makine kaynaklarında kimlik doğrulanırken kimlik doğrulaması için SSO 'yu desteklemez, yalnızca SSH veya Kullanıcı adı/parola desteklenir. Ancak, Azure savunma, genel hizmet için kimlik ve erişim yönetimi sağlamak üzere Azure Active Directory (Azure AD) kullanır. Kullanıcılar Azure savunma kaynaklarını erişmek ve yönetmek için Azure AD kimlik doğrulaması yapabilir ve Azure AD Connect aracılığıyla kendi eşitlenmiş kurumsal kimliklerle sorunsuz çoklu oturum açma deneyimi sağlar. 

- [Azure AD ile Uygulama SSO’yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Azure AD Connect](../active-directory/hybrid/whatis-azure-ad-connect.md)

- [Azure savunma kullanarak bir Linux sanal makinesine bağlanma](bastion-connect-vm-ssh.md)

- [Azure savunma kullanarak bir Windows sanal makinesine bağlanma](bastion-connect-vm-rdp.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>KY-4: Tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulaması denetimlerini kullanma

**Kılavuz**: Azure savunma, hizmetin erişimi ve yönetimi için Azure Active Directory (Azure AD) ile tümleşiktir. Azure AD kiracınız için Azure Multi-Factor Authentication 'i yapılandırın. Azure AD, Multi-Factor Authentication (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulama denetimlerini destekler.  
- Multi-Factor Authentication: Azure AD MFA 'yı etkinleştirin ve MFA kurulumlarınız için Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. MFA, tüm kullanıcılar, kullanıcılar veya oturum açma koşulları ve risk faktörleri temelinde Kullanıcı başına düzeyinde zorlanabilir. 

- Passwordless kimlik doğrulaması: üç adet passwordless kimlik doğrulama seçeneği mevcuttur: Iş için Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri. 

- [Azure’da MFA’yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Active Directory için parolasız kimlik doğrulaması seçeneklerine giriş](../active-directory/authentication/concept-authentication-passwordless.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>KY-5: Hesap anomalilerini izleme ve uyarı verme

**Kılavuz**: Azure savunma uzak oturumları için tanılama günlüklerini etkinleştirin ve bu günlükleri, hangi zaman, nerede ve diğer ilgili günlüğe kaydetme bilgilerini hangi iş yüklerine bağladığınızı görüntülemek için kullanın. Günlüklerde algılanan anormal olduğunda bildirim almak için Azure Izleyici 'yi kullanarak belirli günlüğe kaydedilen savunma oturumlarına yönelik uyarılar oluşturun.

- [Tanılama günlüğünü etkinleştirme hakkında daha fazla bilgiyi burada bulabilirsiniz](diagnostic-logs.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: koşullara göre Azure kaynak erişimini kısıtlama

**Kılavuz**: Azure savunma hizmetine yalnızca Azure Portal aracılığıyla erişebilirsiniz. Azure portal erişimi Azure Active Directory (Azure AD) koşullu erişim kullanılarak kısıtlanabilir. Belirli IP aralıklarından MFA 'yı kullanmak için Kullanıcı oturumu açma gerekliliği gibi Kullanıcı tanımlı koşullara dayalı daha ayrıntılı erişim denetimi için Azure AD koşullu erişimi kullanın.

Müşteri, sanal makineye erişimi daha fazla kısıtlamak için etki alanına katılmış sanal makineler düzeyinde farklı rol tabanlı erişim denetim ilkeleri de kullanabilir.

- [Burada Azure AD koşullu erişim hakkında daha fazla bilgi edinebilirsiniz](../active-directory/conditional-access/overview.md)

- [Azure koşullu erişimine genel bakış](../active-directory/conditional-access/overview.md)

- [Ortak koşullu erişim ilkeleri](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="privileged-access"></a>Ayrıcalıklı Erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Ayrıcalıklı Erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Kılavuz**: Azure savunma, belirli sanal makinelere bağlanmak için hangi hesaplara erişim verildiğini kısıtlayarak iş açısından kritik sistemlere erişimi yalıtmak için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanır. Kullanıcıların yalnızca belirli görevlerini gerçekleştirmek için gerekli izinlere sahip olması için en az ayrıcalık ilkesini izlediğinizden emin olun.

Active Directory Etki Alanı Denetleyicileri (DC 'Ler), güvenlik araçları ve iş açısından kritik sistemlerde yüklü aracılarla sistem yönetimi araçları gibi iş açısından kritik erişimlerinize yönetim erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi de kısıtlatığınızdan emin olun. Bu yönetim ve güvenlik sistemlerine zarar veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Azure savunma ile sanal makineye erişmek için gerekli roller](bastion-faq.md#roles)

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>AE-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve uzlaştırma

**Kılavuz**: Azure savunma, kaynaklarını yönetmek için Azure Active Directory (Azure AD) hesaplarını ve Azure RBAC 'yi kullanır. Hesapların ve erişiminin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. Azure AD erişim incelemelerini, grup üyeliklerini ve kurumsal uygulamalara erişimi ve rol atamalarını gözden geçirmek için kullanabilirsiniz. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.

Ayrıca, Azure Privileged Identity Management, çok fazla sayıda yönetici hesabı oluşturulduğunda ve eski veya yanlış yapılandırılmış yönetici hesaplarını belirlemek için de yapılandırılabilir.

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Bir temsilciye erişimi kaldırma](../lighthouse/how-to/remove-delegation.md)

- [Azure AD kimlik ve erişim gözden geçirmelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Azure AD 'de acil durum erişimi ayarlama

**Kılavuz**: Azure savunma, Azure Active Directory ve Azure RBAC ile tümleştirilerek kaynaklarını yönetebilir. Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.

Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-5-automate-entitlement-management"></a>PA-5: yetkilendirme yönetimini otomatikleştirme 

**Kılavuz**: Azure savunma, kaynakları yönetmek için Azure Active Directory (Azure AD) ve Azure RBAC ile tümleşiktir. Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere, erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir.

- [Azure AD erişim gözden geçirmeleri nelerdir?](../active-directory/governance/access-reviews-overview.md)

- [Azure AD yetkilendirme yönetimi nedir?](../active-directory/governance/entitlement-management-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

**Rehberlik**: güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Gereksinimlerinize bağlı olarak, üretim ortamlarında Azure savunma kaynaklarınızla yönetim yönetim görevlerini gerçekleştirmek için yüksek düzeyde güvenli Kullanıcı iş istasyonları kullanabilirsiniz. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory, Microsoft Defender Gelişmiş tehdit koruması (ATP) ve/veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri ve kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir. 

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../active-directory/devices/concept-azure-managed-workstation.md)

- [Ayrıcalıklı erişim iş istasyonu dağıtma](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>AE-7: Tam yetecek kadar yönetim uygulama (en düşük ayrıcalık ilkesi) 

**Kılavuz**: Azure savunma, kaynaklarını yönetmek için Azure rol tabanlı erişim denetımı (RBAC) ile tümleşiktir. Azure RBAC ağ atamaları aracılığıyla Azure kaynak erişimini yönetmenize olanak tanır. Kullanıcılar, gruplar, hizmet sorumluları ve yönetilen kimliklere bu yerleşik rolleri atayabilirsiniz. Belirli kaynaklar için önceden tanımlanmış yerleşik roller vardır ve Azure CLI, Azure PowerShell veya Azure portal gibi araçlardan yararlanarak bu rollerin envanteri çıkarılabilir ve bunlar sorgulanabilir. Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıklar her zaman yalnızca söz konusu rollere gerekenlerle sınırlanmalıdır. Bu yöntem Azure AD Privileged Identity Management’ın (PIM) tam zamanında (JIT) yaklaşımını tamamlayıcı niteliktedir ve düzenli aralıklarla gözden geçirilmelidir. İzin ayırmak için yerleşik roller kullanın ve gerektiğinde yalnızca özel roller oluşturun.

Azure savunma kullanarak sanal makinelere bağlanırken, kullanıcılarınız aşağıdaki rol atamalarına ihtiyaç duyar:
- Hedef sanal makinede okuyucu rolü
- Hedef sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
- Azure Bastion kaynağında okuyucu rolü

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure savunma kullanarak bir Linux sanal makinesine bağlanma](bastion-connect-vm-ssh.md)

- [Azure savunma kullanarak bir Windows sanal makinesine bağlanma](bastion-connect-vm-rdp.md)

- [Azure yerleşik rolleri](../role-based-access-control/built-in-roles.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Varlık Yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management).*

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

**Rehberlik**: Azure savunma kaynaklarınıza, kaynak gruplarına ve aboneliklerinize mantıksal olarak bunları bir taksonomi halinde düzenlemek için Etiketler uygulayın. Her etiket bir ad ve bir değer çiftinden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz. Güvenlik ekiplerinin Azure 'daki varlıkların sürekli güncelleştirilmiş envanterini erişimi olduğundan emin olun. Azure Hizmetleri, uygulamaları ve ağ kaynakları dahil olmak üzere aboneliklerinizde bulunan tüm kaynakları sorgulamak ve bulabilmesi için Azure Kaynak grafiğini kullanabilirler.

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../security-center/asset-inventory.md)

- [Varlıkları etiketleme hakkında daha fazla bilgi için bkz. Kaynak adlandırma ve etiketleme karar Kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

**Kılavuz**: Azure ilkesi 'ni kullanarak, kullanıcıların ortamınızda hangi hizmetleri sağlayabileceğini denetlemek ve kısıtlamak için Azure savunma kaynaklarının dağıtımına izin vermeyi veya bunları reddetmeyi de kapsar. Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma. Onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/built-in-policies.md#general)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>VY-4: Varlık yaşam döngüsü yönetiminin güvenliğini sağlama

**Rehberlik**: saldırı yüzeyini en aza indirmek için artık gerekli olmadığında dağıtılmış olan Azure savunma kaynaklarına erişimi kaldırın. Kullanıcılar Azure portal, CLı veya REST API 'Leri aracılığıyla Azure savunma kaynaklarını yönetebilir. Ayrıca, artık gerekmiyorsa veya olası bir tehdit olarak tanımlanmadıysa devam eden bir uzak oturumu silebilir veya kesebilirsiniz.

- [Uzak bir oturumun bağlantısını kesmeyi silme](session-monitoring.md#view)

- [Azure ağ CLı](https://docs.microsoft.com/powershell/module/az.network/?view=azps-5.1.0#networking&amp;preserve-view=true)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Kılavuz**: Azure savunma, kimlik ve kimlik doğrulama için Azure Active Directory (Azure AD) ile tümleşiktir. "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisi ile etkileşime geçmesini sınırlamak için Azure koşullu erişimi kullanabilirsiniz.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe Kaydetme ve Tehdit Algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Günlüğe Kaydetme ve Tehdit Algılama](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>GT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Kılavuz**: azure savunma Azure Active Directory (Azure AD) ile tümleştirilir ve hizmete Azure Portal üzerinden erişilir. Varsayılan olarak, hizmete yönelik yönetim eylemleri (oluşturma, güncelleştirme ve silme gibi) Azure etkinlik günlüğü aracılığıyla yakalanır. Kullanıcılar ayrıca, savunma oturumlarını izlemek için oturum BastionAuditLogs gibi Azure savunma kaynak günlüklerini de etkinleştirmelidir.

Azure AD, Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel veya diğer SıEM/izleme araçlarıyla tümleştirilen, daha karmaşık izleme ve analiz kullanım durumları için kullanılabilen aşağıdaki kullanıcı günlüklerini sağlar: 
-   Oturum açma bilgileri – Oturum açma bilgileri raporu, yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.

-   Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki herhangi bir kaynakta yapılan kullanıcı, uygulama, grup, rol ve ilkeleri ekleme veya kaldırma işlemleri gibi değişiklikler, denetim günlüklerine örnek gösterilebilir.

-   Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

-   Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. Azure Güvenlik Merkezi 'nin tehdit koruması modülü, temel güvenlik durumu izlemeye ek olarak, bireysel Azure işlem kaynaklarından (sanal makineler, kapsayıcılar, App Service), veri kaynakları (SQL DB ve depolama gibi) ve Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları da toplayabilirler. Bu özellik, hesap bozuklularını tek tek kaynaklar içinde görmenizi sağlar.

- [Azure savunma kaynak günlükleri](diagnostic-logs.md)

- [Azure AD 'de denetim etkinlik raporları](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure Kimlik Koruması’nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Azure ağ etkinlikleri için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Azure savunma kaynak günlüklerini etkinleştirme, hangi kullanıcıların hangi zamanda, ne zaman ve ilgili günlüğe kaydetme bilgilerinin hangi iş yüklerine bağlandığını görüntülemek için bu tanılama günlüklerini kullanın. Kullanıcılar, uzun süreli saklama ve denetim için bu günlükleri bir depolama hesabına gönderilmek üzere yapılandırabilir.

Azure savunma kaynağınızın dağıtıldığı sanal ağlara uygulanan ağ güvenlik gruplarında ağ güvenlik grubu (NSG) kaynak günlüklerini ve NSG akış günlüklerini etkinleştirin ve toplayın. Bu günlükler daha sonra ağ güvenliğini çözümlemek ve olay araştırmalar, tehdit güvenliği ve güvenlik uyarısı oluşturmayı desteklemek için kullanılabilir. Akış günlüklerini bir Azure Izleyici Log Analytics çalışma alanına gönderebilir ve sonra öngörüleri sağlamak için Trafik Analizi kullanabilirsiniz.

- [Azure savunma günlüklerini etkinleştirme ve bunlarla çalışma](diagnostic-logs.md)

- [Ağ güvenlik grubu akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Azure Güvenlik Duvarı günlükleri ve ölçümleri](../firewall/logs-and-metrics.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

- [Ağ Izleyicisi ile izleme](../network-watcher/network-watcher-monitoring-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: otomatik olarak kullanılabilen etkinlik günlükleri, okuma IŞLEMLERI (Get) haricinde Azure savunma kaynaklarınız için tüm yazma IŞLEMLERINI (put, Post, Delete) içerir. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

- [Azure için Azure Kaynak günlüklerini etkinleştirme ](diagnostic-logs.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik**: bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atadığınızdan emin olun.

Azure etkinlik günlüklerini merkezi günlüklerinizi tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin.

Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: günlük depolama bekletmesini yapılandırma

**Kılavuz**: Azure savunma günlüklerini depolamak için kullanılan tüm depolama hesapları veya Log Analytics çalışma alanları, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun.

Azure Izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayabilirsiniz.

- [Log Analytics çalışma alanı saklama süresini yapılandırma](../azure-monitor/platform/manage-cost-storage.md)

- [Kaynak günlüklerini bir Azure depolama hesabında depolama](/azure/azure-monitor/platform/resource-logs-collect-storage)

- [Azure savunma günlüklerini etkinleştirin ve bunlarla çalışın](diagnostic-logs.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Olay Yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response).*

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

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

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

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Duruş ve Güvenlik Açığı Yönetimi](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management).*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>BD-1: Azure hizmetleri için güvenli yapılandırma oluşturma 

**Kılavuz**: Azure Ilkesi ile Azure için standart güvenlik yapılandırması tanımlama ve uygulama. Azure savunma 'nizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın. Müşteriler, savunma kaynaklarını güvenli ve tutarlı bir şekilde dağıtmak için Azure şemaları veya ARM şablonlarından yararlanarak güvenli yapılandırmalar da oluşturabilir.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [ARM şablonları hakkında bilgi edinin](../azure-resource-manager/templates/overview.md)

- [Azure şemaları hakkında genel bakış](../governance/blueprints/overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>BD-2: Azure hizmetleri için güvenli yapılandırmalara dayanmalar

**Kılavuz**: Azure Ilkesi ile Azure için standart güvenlik yapılandırması tanımlama ve uygulama. Savunma kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pv-8-conduct-regular-attack-simulation"></a>DG-8: Düzenli saldırı simülasyonu çalıştırma

**Rehber**: Gerektiğinde Azure kaynaklarınızda sızma testi veya kırmızı ekip etkinlikleri çalıştırın ve tüm kritik güvenlik bulgularının düzeltildiğinden emin olun.

Sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak için Microsoft Cloud Sızma Testi Angajman Kuralları’na uyun. Microsoft tarafından yönetilen bulut altyapısında, hizmetlerde ve uygulamalarda Microsoft'un stratejisini ve Kırmızı Ekip ile canlı site sızma testi yürütmesini kullanın.

- [Azure’da sızma testi](../security/fundamentals/pen-testing.md)

- [Sızma Testi Angajman Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud Kırmızı Ekibi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: İdare ve Strateji](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

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
- [Azure Güvenlik Mimarisi Önerisi - Depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure Güvenlik Temelleri - Azure Veri güvenliği, şifrelemesi ve depolaması](../security/fundamentals/encryption-overview.md)

- [Bulut Benimseme Çerçevesi - Azure veri güvenliği ve şifrelemesi için en iyi yöntemler](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik Karşılaştırması - Varlık yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management)

- [Azure Güvenlik Karşılaştırması - Veri Koruma](/azure/security/benchmarks/security-controls-v2-data-protection)

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

- [Azure Güvenlik Karşılaştırması - Duruş ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management)

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

- [Azure Güvenlik Karşılaştırması - Ağ Güvenliği](/azure/security/benchmarks/security-controls-v2-network-security)

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

- [Azure Güvenlik Karşılaştırması - Kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management)

- [Azure Güvenlik Karşılaştırması - Ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access)

- [Azure Güvenliği En İyi Yöntemi 11 – Mimari. Tek birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure kimlik yönetimi güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>İS-7: Günlüğe kaydetme ve tehdit yanıtı stratejisini tanımlama

**Rehber**: Tehditleri hızla algılar ve giderirken uyumluluk gereksinimlerini de karşılamak için bir günlüğe kaydetme ve tehdit yanıtı stratejisi oluşturun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Güvenlik operasyonları (SecOps) kuruluşunun rolü ve sorumlulukları 

-   NIST veya başka bir endüstri çerçevesiyle uyumlu, iyi tanımlanmış bir olay yanıt süreci 

-   Tehdit algılama, olay yanıtı ve uyumluluk gereksinimlerini desteklemek için günlük yakalama ve saklama

-   SIEM’yi, yerel Azure özelliklerini ve diğer kaynakları kullanarak tehditlerin merkezi görünürlüğü ve tehditler hakkındaki bağıntı bilgileri 

-   Müşterileriniz, sağlayıcılarınız ve genel olarak ilgili taraflarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, adli araştırma, saldırı düzeltme ve yok etme gibi olayları işlemek için Azure yerel platformunu ve üçüncü taraf platformlarını kullanma

-   Olayları ve olay sonrası çıkarılan dersler ve kanıt saklama gibi etkinlikleri işlemeye yönelik süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik Karşılaştırması - Günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-detection)

- [Azure Güvenlik Karşılaştırması - Olay yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response)

- [Azure Güvenliği En İyi Yöntemi 4 – İşlem. Bulut için Olay Yanıtı Süreçlerini Güncelleştirme](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure Benimseme Çerçevesi, günlük ve raporlama karar kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure kurumsal ölçeği, yönetimi ve izlemesi](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Karşılaştırması V2’ye genel bakış](/azure/security/benchmarks/overview) konusuna bakın
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
