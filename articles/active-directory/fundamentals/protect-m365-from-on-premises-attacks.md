---
title: Şirket içi saldırılardan Microsoft 365 koruma
description: Şirket içi bir saldırının etkilenmemesini sağlamaya yönelik yönergeler Microsoft 365
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/22/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71d64552520db28b3fe38cc354a30ee5470ee091
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060759"
---
# <a name="protecting-microsoft-365-from-on-premises-attacks"></a>Şirket içi saldırılardan Microsoft 365 koruma

Birçok müşteri, kullanıcılarını, cihazlarını ve uygulamalarını avantajlarına yönelik olarak kendi özel kurumsal ağlarını Microsoft 365. Bununla birlikte, bu özel ağların güvenliği tehlikeye girebilir çok iyi belgelenmiş yollar vardır. Microsoft 365 birçok kuruluş için "nervous sistemi" olarak davrandığı için, şirket içi altyapıya karşı koruma altına almak kritik öneme sahiptir.

Bu makalede, sistemlerinizi şirket içi uzlaşmadan Microsoft 365 bulut ortamınızı koruyacak şekilde nasıl yapılandıracağınız gösterilir. Birincil olarak Azure AD kiracılarının şirket içi sistemlere güvenli bir şekilde bağlı olması ve sistemlerinizi bulut sistemlerinizi şirket içi uzlaşmadan koruyan yollarla çalıştırmak için gereken tüm açılardan Azure AD Kiracı yapılandırma ayarlarına odaklanıyoruz.

Microsoft 365 Bulut ortamınızın güvenliğini sağlamak için bu kılavuzun uygulanması önemle önerilir.
> [!NOTE]
> Bu makale başlangıçta bir blog gönderisi olarak yayımlandı. Bu, longeçekimi ve bakım için buraya taşındı. <br>
Bu makalenin çevrimdışı sürümünü oluşturmak için, tarayıcınızın PDF 'yi Yazdır işlevini kullanın. Güncelleştirmeler için sık sık tekrar kontrol edin.

## <a name="primary-threat-vectors-from-compromised-on-premises-environments"></a>Güvenliği aşılmış şirket içi ortamlardan birincil tehdit vektörleri


Microsoft 365 bulut ortamınız, kapsamlı bir izleme ve güvenlik altyapısından faydalanır. Dünya çapındaki trafiğe yönelik makine öğrenimi ve insan zekası kullanımı, saldırıları hızla algılayabilir ve neredeyse gerçek zamanlı olarak yeniden yapılandırmanıza izin verebilir. Şirket içi altyapıyı Microsoft 365 'e bağlayan karma dağıtımlarda, birçok kuruluş, kritik kimlik doğrulama ve dizin nesnesi durum yönetimi kararları için şirket içi bileşenlere güven devredebilir.
Ne yazık ki, şirket içi ortamın güvenliği tehlikeye girerse, bu güven ilişkileri saldırganların Microsoft 365 ortamınızı tehlikeye sokmasına neden olur.

İki birincil tehdit vektörü, **Federasyon güven ilişkileri** ve **hesap eşitleme** ' dir. Her iki vektör de bir saldırganın buluta yönetici erişimine izin verebilir.

* SAML kimlik doğrulaması gibi **Federasyon güven ilişkileri**, şirket Içi kimlik altyapınız aracılığıyla Microsoft 365 kimlik doğrulaması için kullanılır. Bir SAML belirteç imzalama sertifikası tehlikeye atılırsa, Federasyon, bu sertifikaya sahip herkesin buluttaki herhangi bir kullanıcının kimliğine bürünmesini sağlar. **Mümkün olduğunda Microsoft 365 kimlik doğrulaması için Federasyon güven ilişkilerini devre dışı bırakmanızı öneririz.**

* **Hesap eşitleme** , ayrıcalıklı kullanıcıları (kimlik bilgileri dahil) veya Microsoft 365 yönetim ayrıcalıklarına sahip grupları değiştirmek için kullanılabilir. Eşitlenmiş nesnelerin, doğrudan veya güvenilen roller ya da gruplara dahil **olmak üzere Microsoft 365 içindeki bir kullanıcının ötesinde hiçbir ayrıcalık olmamasını öneririz** . Bu nesnelerin güvenilen bulut rollerinde veya gruplarında doğrudan veya iç içe atanması bulunmadığından emin olun.

## <a name="protecting-microsoft-365-from-on-premises-compromise"></a>Şirket içi uzlaşmadan Microsoft 365 koruma


Yukarıda özetlenen tehdit vektörlerini ele almak için aşağıda gösterilen ilkelere bağlı kalmanızı öneririz:

![Microsoft 365 korumak için başvuru mimarisi ](media/protect-m365/protect-m365-principles.png)

*  **Microsoft 365 Yönetici hesaplarınızı tamamen yalıtın.** Olmaları gerekir

    * Azure AD 'de ana kopyalı.

     * Multi-Factor Authentication (MFA) ile kimliği doğrulandı.

     *  Azure AD koşullu erişim tarafından güvenli hale getirilir.

     *  Yalnızca Azure yönetilen Iş Istasyonları kullanılarak erişilir.

Bunlar kısıtlı kullanım hesaplarıdır. **Microsoft 365 üzerinde yönetici ayrıcalıklarına sahip şirket içi hesap olmaması gerekir.** Daha fazla bilgi için, [Microsoft 365 yönetici rollerine genel bakış](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide)bölümüne bakın.
Ayrıca bkz. [Azure Active Directory Microsoft 365 rolleri](../roles/m365-workload-docs.md).

*  **Microsoft 365 cihazları yönetin.** Şirket içi cihaz yönetimi altyapınızdaki bağımlılıkları ortadan kaldırmak için Azure AD JOIN ve bulut tabanlı mobil cihaz yönetimi (MDM) kullanın ve cihaz ve güvenlik denetimlerini tehlikeye atabilir.

* **Şirket içi hesabın Microsoft 365 için yükseltilmiş ayrıcalıkları yoktur.**
    NTLM, LDAP veya Kerberos kimlik doğrulaması gerektiren şirket içi uygulamalara erişen hesapların, kuruluşun şirket içi kimlik altyapısında bir hesabı olması gerekir. Hizmet hesapları da dahil olmak üzere bu hesapların ayrıcalıklı bulut rollerine veya gruplarına dahil olmadığından ve bu hesaplara yapılan değişikliklerin Bulut ortamınızın bütünlüğünü etkilemeyeceğinden emin olun. Ayrıcalıklı şirket içi yazılımlar Microsoft 365 ayrıcalıklı hesapları veya rolleri etkilememelidir.

*  Şirket içi kimlik bilgilerinizle bağımlılıkları ortadan kaldırmak için **Azure AD bulut kimlik doğrulamasını kullanın** . Her zaman Windows Hello, FIDO, Microsoft Authenticator veya Azure AD MFA gibi güçlü kimlik doğrulaması kullanın.

## <a name="specific-recommendations"></a>Belirli öneriler


Aşağıdaki bölümlerde, yukarıda açıklanan ilkelerin nasıl uygulanacağı hakkında özel yönergeler sağlanmaktadır.

### <a name="isolate-privileged-identities"></a>Ayrıcalıklı kimlikleri yalıtma


Azure AD 'de yöneticiler gibi ayrıcalıklı rollere sahip kullanıcılar, ortamın geri kalanını derlemek ve yönetmek için güvenin köküdür. Tehlikeye aşılma etkisini en aza indirmek için aşağıdaki uygulamaları uygulayın.

* Azure AD ve Microsoft 365 ayrıcalıklı roller için yalnızca bulut hesapları kullanın. d

* Microsoft 365 ve Azure AD 'yi yönetmek için ayrıcalıklı erişim [cihazları](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) dağıtın.

*  Ayrıcalıklı rollere sahip tüm insan hesaplarına tam zamanında (JıT) erişim için [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) (PIM) dağıtın ve rolleri etkinleştirmek için güçlü kimlik doğrulaması gerekir.

* Yönetim rollerini, [görevlerini gerçekleştirmek için mümkün olan en az ayrıcalığa sahip olacak şekilde](../roles/delegate-by-task.md)sağlayın.

* Aynı anda temsilciyi ve birden çok rolü içeren daha zengin bir rol atama deneyimini etkinleştirmek için Azure AD güvenlik grupları 'nı veya Microsoft 365 gruplarını (topluca "bulut grupları") kullanmayı ve [rol tabanlı erişim denetimini etkinleştirmeyi](../roles/groups-assign-role.md)göz önünde bulundurun. Ayrıca, rol kapsamını kuruluşun bir bölümüyle kısıtlamak için [Yönetim birimlerini](../roles/administrative-units.md) de kullanabilirsiniz.

* [Acil durum erişim hesapları](../roles/security-emergency-access.md) dağıtın ve kimlik bilgilerini depolamak için şirket içi parola kasaları kullanmayın.

Daha fazla bilgi için, bkz. bu konuda ayrıntılı yönergeler içeren [ayrıcalıklı erişimin güvenliğini sağlama](https://aka.ms/SPA). Ayrıca bkz. [Azure AD 'de Yöneticiler Için güvenli erişim uygulamaları](../roles/security-planning.md).

### <a name="use-cloud-authentication"></a>Bulut kimlik doğrulaması kullan 

Kimlik bilgileri, birincil saldırı vektörüdür. Kimlik bilgilerini daha güvenli hale getirmek için aşağıdaki yöntemleri uygulayın.

* [Passwordless kimlik doğrulaması dağıtma](../authentication/howto-authentication-passwordless-deployment.md): passwordless kimlik bilgilerini dağıtarak parolaların kullanımını mümkün olduğunca azaltır. Bu kimlik bilgileri, bulutta yerel olarak yönetilir ve onaylanır. Aşağıdakilerden birini seçin:

   * [İş için Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/passwordless-strategy)

   * [Authenticator Uygulaması](../authentication/howto-authentication-passwordless-phone.md)

   * [FIDO2 güvenlik anahtarları](../authentication/howto-authentication-passwordless-security-key-windows.md)

* [Dağıtım Multi-Factor Authentication](https://aka.ms/deploymentplans/mfa): [Azure AD MFA kullanarak birden çok güçlü kimlik bilgileri](../fundamentals/resilience-in-credentials.md)sağlayın. Bu şekilde, bulut kaynaklarına erişim, Azure AD 'de yönetilebilen ve bir şirket içi parolaya ek olarak yönetilen bir kimlik bilgisi gerektirir.

   * Daha fazla bilgi için bkz. [Azure Active Directory ile esnek erişim denetimi yönetim stratejisi oluşturma](https://aka.ms/resilientaad).

**Sınırlamalar ve avantajları**

* Karma hesap parolası yönetimi, parola koruma aracıları ve parola geri yazma aracıları gibi karma bileşenleri gerektirir. Şirket içi altyapınız tehlikedeyse, saldırganlar bu aracıların bulunduğu makineleri denetleyebilir. Bu, bulut altyapınızın güvenliğini tehlikeye amayacak, bulut hesaplarınız bu bileşenleri şirket içi uzlaşmadan korumayacak.

*  Active Directory 'ten eşitlenen şirket içi hesaplar, şirket içi AD parola ilkelerinin bu sorunu azaltmasını sağlamak için Azure AD 'de hiçbir zaman sona ermeyecek şekilde işaretlenir. Şirket içi AD 'nin güvenliği tehlikeye atılırsa ve AD Connect 'ten eşitlemenin devre dışı bırakılması gerekiyorsa, [Enforcechoparlör Passwordpolicyforpasswordsyncedusers](../hybrid/how-to-connect-password-hash-synchronization.md)seçeneğini ayarlamanız gerekir.

## <a name="provision-user-access-from-the-cloud"></a>Buluttan Kullanıcı erişimi sağlama

Sağlama, uygulama veya kimlik sağlayıcılarındaki Kullanıcı hesaplarının ve grupların oluşturulmasına başvurur.

![Hazırlama mimarisi diyagramı](media/protect-m365/protect-m365-provision.png)

* **Cloud İK uygulamalarından Azure AD 'ye sağlama:** Bu, bulut HR uygulamalarınızdan Azure AD 'ye yönelik birleştirici-Mover-Leaver döngüsünü kesintiye uğratmadan, şirket içi bir güvenliğin yalıtılmasını sağlar.

* **Bulut uygulamaları:** Mümkün olduğunda şirket içi sağlama çözümlerinin aksine [Azure AD uygulaması sağlamayı](../app-provisioning/user-provisioning.md) dağıtın. Bu, şirket içi ihlal nedeniyle SaaS uygulamalarınızın bazılarının kötü amaçlı kullanıcı profilleriyle zarar olarak korunmasını sağlar. 

* **Dış kimlikler:** [Azure AD B2B işbirliğini](../external-identities/what-is-b2b.md)kullanın.
    Bu, iş ortakları, müşteriler ve tedarikçilerle harici işbirliği için şirket içi hesapların bağımlılığını azaltır. Diğer kimlik sağlayıcılarıyla doğrudan tüm Federasyonu dikkatle değerlendirin. B2B Konuk hesaplarını aşağıdaki yollarla kısıtlamanızı öneririz.

   *  Konuk erişimini gözatma gruplarına ve dizindeki diğer özelliklere sınırlayın. Konuk, üye olmadıkları grupları okuyabilme yeteneğini kısıtlamak için dış işbirliği ayarlarını kullanın. 

    *   Azure portal erişimini engelleyin. Nadir olarak gereken özel durumları yapabilirsiniz.  Tüm konukları ve dış kullanıcıları içeren bir koşullu erişim ilkesi oluşturun ve ardından [erişimi engellemek için bir ilke uygulayın](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management.md). 

* **Bağlantısı kesilen ormanlar:** [Azure AD bulut sağlamasını](../cloud-provisioning/what-is-cloud-provisioning.md)kullanın. Bu, bağlantısı kesilen ormanlara bağlanmanızı, ormanlar arası bağlantı veya güvenler oluşturma gereğini ortadan kaldırarak şirket içi ihlalin etkisini genişletmenizi sağlar. * 
 
**Sınırlamalar ve dengeler:**

* Karma hesapları sağlamak için kullanıldığında, bulut HR sistemlerinden Azure AD, AD 'den Azure AD 'ye veri akışını tamamlamaya yönelik şirket içi eşitlemeye dayanır. Eşitleme kesintiye uğrarsa, Azure AD 'de yeni çalışan kayıtları kullanılamaz.

## <a name="use-cloud-groups-for-collaboration-and-access"></a>İşbirliği ve erişim için bulut gruplarını kullanın

Bulut grupları, işbirliklerinizi ve şirket içi altyapınızdan erişiminizi kullanmanıza olanak sağlar.

* **İşbirliği:** Modern işbirliği için Microsoft 365 grupları ve Microsoft ekipleri kullanın. Şirket içi dağıtım listelerine yetki alın ve [dağıtım listelerini Outlook 'ta Microsoft 365 gruplara yükseltin](https://docs.microsoft.com/office365/admin/manage/upgrade-distribution-lists?view=o365-worldwide).

* **Erişim:** Azure AD 'de uygulamalara erişim yetkisi vermek için Azure AD güvenlik gruplarını veya Microsoft 365 gruplarını kullanın.
* **Office 365 lisansı:** Yalnızca bulutta bulunan grupları kullanarak Office 365 'e temin etmek için grup tabanlı lisanslama kullanın. Bu, şirket içi altyapıdan grup üyeliği denetimini ayırır.

Erişim için kullanılan grupların sahipleri, şirket içi uzlaşmadan üyelik olmaması için ayrıcalıklı kimlikler olarak düşünülmelidir.
Alma, grup üyeliğinin şirket içinde veya Microsoft 365 içindeki dinamik grup üyeliğini etkileyebilecek şirket içi özniteliklerin doğrudan işlemesini içerir.

## <a name="manage-devices-from-the-cloud"></a>Cihazları buluttan yönetme


Cihazları güvenli bir şekilde yönetmek için Azure AD yeteneklerini kullanın.

-   **Windows 10 Iş Istasyonlarını kullanın:** MDM ILKELERIYLE [Azure AD 'ye katılmış cihazları dağıtın](../devices/azureadjoin-plan.md) . Tam otomatik sağlama deneyimi için [Windows Autopilot](https://docs.microsoft.com/mem/autopilot/windows-autopilot) etkinleştirin.

    -   Windows 8.1 ve önceki makineler kullanımdan kaldırıldı.

    -   Sunucu işletim sistemi makinelerini iş istasyonları olarak dağıtmayın.

    -   Tüm cihaz yönetimi iş yüklerinin yetkilisinin kaynağı olarak [Microsoft Intune](https://www.microsoft.com/en/microsoft-365/enterprise-mobility-security/microsoft-intune) kullanın.

-   Microsoft 365 ve Azure AD 'yi yönetmek için ayrıcalıklı erişim [**cihazları dağıtın**](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) .

 ## <a name="workloads-applications-and-resources"></a>İş yükleri, uygulamalar ve kaynaklar 

-   Şirket **ıçı SSO sistemleri:** Şirket içi Federasyon ve Web Erişimi yönetim altyapısını kullanımdan kaldırma ve uygulamaları Azure AD kullanacak şekilde yapılandırma.  

-   **Modern kimlik doğrulama protokollerini destekleyen SaaS ve LOB uygulamaları:** [Çoklu oturum açma için Azure ad kullanın](../manage-apps/what-is-single-sign-on.md). Azure AD 'yi kimlik doğrulaması için kullanmak üzere yapılandırdığınız daha fazla uygulama, şirket içi bir uzlaşmaya karşı daha az risk taşır.


* **Eski uygulamalar** 

   * Modern kimlik doğrulamayı desteklemeyen eski uygulamalara kimlik doğrulaması, yetkilendirme ve uzaktan erişim, [Azure AD uygulama ara sunucusu](../manage-apps/application-proxy.md)aracılığıyla etkinleştirilebilir. Ayrıca,  [güvenli karma erişim ortağı tümleştirmeleri](../manage-apps/secure-hybrid-access.md)kullanılarak bir ağ veya uygulama teslim denetleyicisi çözümü aracılığıyla da etkinleştirilebilir.   

   * Modern kimlik doğrulamasını destekleyen ve kimlik doğrulamasını Azure AD ile tümleştiren bir VPN satıcısı seçin. Anon şirket içi güvenliğin tehlikeye girmesi durumunda, VPN 'yi devre dışı bırakarak erişimi devre dışı bırakmak veya engellemek için Azure AD 'yi kullanabilirsiniz.

*  **Uygulama ve iş yükü sunucuları**

   * Gerekli sunucuları Azure IaaS 'ye geçirilebileceği uygulamalar veya kaynaklar, şirket içi güven ve bağımlılığı ayırmak için [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/overview) (Azure AD DS) kullanır. Bu ayrılmayı başarmak için, Azure AD DS için kullanılan sanal ağların kurumsal ağlarla bağlantı sahibi olmaması gerekir.

   * [Kimlik bilgisi katmanlama](https://aka.ms/TierModel)kılavuzunu izleyin. Uygulama sunucuları genellikle katman 1 varlıkları olarak kabul edilir.

 ## <a name="conditional-access-policies"></a>Koşullu erişim Ilkeleri

Sinyalleri yorumlamak ve bunlara göre kimlik doğrulama kararları almak için Azure AD koşullu erişimini kullanın. Daha fazla bilgi için bkz [. koşullu erişim dağıtım planı.](https://aka.ms/deploymentplans/ca)

* [Eski kimlik doğrulama protokolleri](../fundamentals/auth-sync-overview.md): mümkün olduğunda [eski kimlik doğrulama protokollerini engellemek](../conditional-access/howto-conditional-access-policy-block-legacy.md) için koşullu erişimi kullanın. Ayrıca, uygulamaya özgü yapılandırmayı kullanarak, uygulama düzeyinde eski kimlik doğrulama protokollerini devre dışı bırakın.

   * [Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online#how-basic-authentication-works-in-exchange-online) ve [SharePoint Online](https://docs.microsoft.com/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps)için belirli ayrıntılara bakın.

* Önerilen [kimlik ve cihaz erişim yapılandırmasını uygulayın.](https://docs.microsoft.com/microsoft-365/security/office-365-security/identity-access-policies?view=o365-worldwide)

* Azure AD 'nin koşullu erişimi içermeyen bir sürümünü kullanıyorsanız [Azure AD güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md)kullandığınızdan emin olun.

   * Azure AD Özellik lisanslama hakkında daha fazla bilgi için bkz. [Azure AD fiyatlandırma Kılavuzu](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="monitoring"></a>İzleme 

Ortamınızı şirket içi bir uzlaşmadan Microsoft 365 korumak üzere yapılandırdıktan sonra, ortamı proaktif olarak [izleyin](../reports-monitoring/overview-monitoring.md) .
### <a name="scenarios-to-monitor"></a>Izlenecek senaryolar

Kuruluşunuza özgü tüm senaryolara ek olarak aşağıdaki önemli senaryoları izleyin. Örneğin, iş açısından kritik uygulamalarınızın ve kaynaklarınızın erişimini önceden izlemeniz gerekir.

* **Şüpheli etkinlik**: tüm [Azure AD risk olayları](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection#risk-detection-and-remediation) , şüpheli etkinlik için izlenmelidir. [Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection) , Azure Güvenlik Merkezi ile yerel olarak tümleşiktir.

   * Konum tabanlı sinyallerin gürültülü algılamalarını önlemek için konum [adlı](../reports-monitoring/quickstart-configure-named-locations.md) ağı tanımlayın. 
*  **Kullanıcı varlığı davranış analizi (UEBA) uyarıları** Anomali algılama hakkında Öngörüler almak için UEBA 'yi kullanın.
   * Microsoft Bulut uygulama bulma (MCAS) [bulutta Ueba](https://docs.microsoft.com/cloud-app-security/tutorial-ueba)sağlar.

   * [Şirket ıçı UEBA 'Yi Azure ATP 'den tümleştirebilirsiniz](https://docs.microsoft.com/defender-for-identity/install-step2). MCAS Azure AD Kimlik Koruması sinyalleri okur. 

* **Acil erişim hesapları etkinliği**: [acil durum erişim hesapları](../roles/security-emergency-access.md) kullanan tüm erişim izlenmeli ve araştırmalar için uyarılar oluşturulmalıdır. Bu izleme şunları içermelidir: 

   * Oturum açma işlemleri. 

   * Kimlik bilgisi yönetimi. 

   * Grup üyeliklerinde tüm güncelleştirmeler. 

   *    Uygulama atamaları. 
* **Ayrıcalıklı rol etkinliği**: [Azure AD PIM tarafından oluşturulan güvenlik uyarılarını](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-configure-security-alerts?tabs=new#security-alerts)yapılandırın ve gözden geçirin.
    Her kullanıcı doğrudan atandığında uyarı oluşturarak yalnızca, PıM dışında ayrıcalıklı rollerin doğrudan atamasını izleyin.
* **Azure AD kiracı genelinde yapılandırma**: kiracı genelindeki yapılandırmalarda yapılan herhangi bir değişiklik sistemde uyarılar üretmelidir. Bunlar arasında şunlar yer alır ancak bunlarla sınırlı değildir
  *  Özel etki alanlarını güncelleştirme  

  * Azure AD B2B izin verme/engelleme listesi değişiklikleri.
  * Azure AD B2B izin verilen kimlik sağlayıcıları (doğrudan Federasyon veya sosyal oturumlar aracılığıyla SAML IDPs).  
  * Koşullu erişim veya risk ilkesi değişiklikleri 

* **Uygulama ve hizmet sorumlusu nesneleri**:
   * Koşullu erişim ilkeleri gerektirebilecek yeni uygulamalar veya hizmet sorumluları. 

   * Hizmet sorumlularına ek kimlik bilgileri eklendi.
   * Uygulama onayı etkinliği. 

* **Özel roller**:
   * Özel rol tanımlarının güncelleştirmeleri. 

   * Yeni özel roller oluşturuldu. 

### <a name="log-management"></a>Günlük yönetimi

Azure Sentinel, ortak sorgular ve araştırma ve adli PlayBook gibi SIEM sistemleri gibi tutarlı bir araç takımını kolaylaştırmak için bir günlük depolama ve bekletme stratejisi, tasarım ve uygulama tanımlayın.

* **Azure AD günlükleri** Tanılama ayarları, günlük tutma ve SıEM alımı dahil olmak üzere tutarlı en iyi uygulamaları içeren alma günlükleri ve sinyali. Günlük stratejisi aşağıdaki Azure AD günlüklerini içermelidir:
   * Oturum açma etkinliği 

   * Denetim günlükleri 

   * Risk olayları 

Azure AD, oturum açma etkinlik günlüğü ve denetim günlükleri için [Azure izleyici tümleştirmesi](../reports-monitoring/concept-activity-logs-azure-monitor.md) sağlar. Risk olayları [MICROSOFT Graph API](https://aka.ms/AzureADSecuredAzure/32b)aracılığıyla alınabilir. Azure [ad günlüklerini Azure izleyici günlüklerine akışını](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)sağlayabilirsiniz.

* **Karma altyapı işletim sistemi güvenlik günlükleri.** Tüm karma kimlik altyapısı işletim sistemi günlüklerinin arşivlenmesi ve bir <br>Katman 0 sistemi, yüzey alanı etkilerine göre. Şunları içerir: 

   *  Azure AD Connect. Kimlik eşitlemesini izlemek için [Azure AD Connect Health](https://aka.ms/AzureADSecuredAzure/32e) dağıtılması gerekir.

   *  Uygulama proxy aracıları 


   * Parola geri yazma aracıları 

   * Parola koruma ağ geçidi makineleri  

   * Azure MFA RADIUS uzantısına sahip NPS 

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure AD ile kimlik ve erişim yönetimine dayanıklılık ekleme](resilience-overview.md)

* [Kaynaklara dış erişimi güvenli hale getirme](secure-external-access-resources.md) 
* [Tüm uygulamalarınızı Azure AD ile tümleştirme](five-steps-to-full-application-integration-with-azure-ad.md)
