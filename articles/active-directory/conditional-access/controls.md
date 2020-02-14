---
title: Koşullu erişim Azure Active Directory erişim denetimleri
description: Azure Active Directory Koşullu erişim çalışmalarında erişim denetimlerinin nasıl yapılacağını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 12/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad8894078a15bf37a5383cdff3721f4bf7be910
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186220"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Koşullu erişim Azure Active Directory erişim denetimleri nelerdir?

[Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md)ile, yetkili kullanıcıların bulut uygulamalarınıza nasıl erişdiğini denetleyebilirsiniz. Bir koşullu erişim ilkesinde, ("Bu durumda"), ilkenizi tetiklemenin nedenine ("bunu yapın

![Denetim](./media/controls/10.png)

Koşullu erişim bağlamında,

- "**Bu gerçekleştiğinde**" **koşullar** denir
- "Bunu**yapmak**için, **erişim denetimleri** olarak adlandırılır

Denetimleriniz ile bir koşul ifadesinin birleşimi, koşullu erişim ilkesini temsil eder.

![Denetim](./media/controls/61.png)

Her denetim, oturum açan kişi veya sistem tarafından yerine getirilmesi gereken bir gereksinimdir veya oturum açtıktan sonra kullanıcının yapabilecekleri bir kısıtlamadır.

İki tür denetim vardır:

- Denetim-ağ geçidi erişimine **Izin ver**
- **Oturum denetimleri** -bir oturum içindeki erişimi kısıtlamak için

Bu konu başlığı altında, Azure AD koşullu erişim 'de kullanılabilen çeşitli denetimler açıklanmaktadır. 

## <a name="grant-controls"></a>Atama denetimleri

İzin denetimleri sayesinde, istediğiniz denetimleri seçerek erişimi tamamen engelleyebilir veya ek gereksinimlere erişime izin verebilirsiniz. Birden çok denetim için şunları gerekli kılabilirsiniz:

- Yerine getirilmesi için seçilen tüm denetimler (*ve*)
- Yerine getirilmesi için seçilen bir denetim (*veya*)

![Denetim](./media/controls/18.png)

### <a name="multi-factor-authentication"></a>Multi-factor authentication

Bu denetimi, çok faktörlü kimlik doğrulamasının belirtilen bulut uygulamasına erişmesini gerektirmek için kullanabilirsiniz. Bu denetim aşağıdaki Multi-Factor sağlayıcılarını destekler:

- Azure Multi-Factor Authentication
- Şirket içi Multi-Factor Authentication sağlayıcısı, Active Directory Federasyon Hizmetleri (AD FS) (AD FS) ile birleştirilmiştir.

Multi-Factor Authentication 'ın kullanılması, kaynaklara geçerli bir kullanıcının birincil kimlik bilgilerine erişim elde etmiş olabilecek yetkisiz bir kullanıcı tarafından erişilmesini korumanıza yardımcı olur.

### <a name="compliant-device"></a>Uyumlu cihaz

Cihaz tabanlı koşullu erişim ilkelerini yapılandırabilirsiniz. Cihaz tabanlı bir koşullu erişim ilkesinin amacı, yalnızca [Yönetilen cihazlardan](require-managed-devices.md)seçilen bulut uygulamalarına erişim hakkı vermektedir. Bir cihazın uyumlu olarak işaretlenmesini gerektirmek, yönetilen cihazlara erişimi sınırlandırmanıza gerek kalmadan bir seçenektir. Bir cihaz, Intune (herhangi bir cihaz işletim sistemi için) veya Windows 10 cihazları için üçüncü taraf MDM sisteminiz tarafından uyumlu olarak işaretlenebilir. Windows 10 dışındaki cihaz işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez. 

Cihazınızın, uyumlu olarak işaretlenmeden önce Azure AD 'ye kayıtlı olması gerekir. Bir cihazı kaydetmek için üç seçeneğiniz vardır: 

- Azure AD kayıtlı cihazlar
- Azure AD’ye katılmış cihazlar  
- Hibrit Azure AD’ye katılmış cihazlar

Bu üç seçenek, [cihaz kimliği](../devices/overview.md) nedir makalesinde açıklanmaktadır.

Daha fazla bilgi için bkz. [koşullu erişim ile bulut uygulama erişimi için yönetilen cihazlar gerektirme](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Karma Azure AD 'ye katılmış cihaz

Karma bir Azure AD 'ye katılmış cihaz istemek, cihaz tabanlı koşullu erişim ilkelerini yapılandırmak için başka bir seçenektir. Bu gereksinim, şirket içi Active Directory katılmış Windows masaüstleri, dizüstü bilgisayarlar ve kurumsal tabletlere başvurur. Bu seçenek işaretliyse, koşullu erişim ilkeniz şirket içi Active Directory ve Azure Active Directory katılmış cihazlarla yapılan erişim girişimlerini erişim izni verir. Mac cihazları karma Azure AD JOIN 'i desteklemez.

Daha fazla bilgi için bkz. [Azure Active Directory cihaz tabanlı koşullu erişim ilkeleri ayarlama](require-managed-devices.md).

### <a name="approved-client-app"></a>Onaylanan istemci uygulaması

Çalışanlarınız hem kişisel hem de iş görevleri için mobil cihaz kullandığından, cihazları sizin yönetmediği durumlarda bile cihazları kullanarak erişilen şirket verilerini koruma olanağına sahip olmak isteyebilirsiniz.
Kuruluşunuzun verilerini mobil cihaz yönetimi (MDM) çözümünden bağımsız olarak korumanıza yardımcı olması için [Intune uygulama koruma ilkelerini](https://docs.microsoft.com/intune/app-protection-policy) kullanabilirsiniz.

Onaylanan istemci uygulamalarıyla, [Intune uygulama koruma ilkelerini](https://docs.microsoft.com/intune/app-protection-policy)desteklemek için bulut uygulamalarınıza erişmeyi deneyen bir istemci uygulaması isteyebilirsiniz. Örneğin, Exchange Online 'a erişimi Outlook uygulamasıyla kısıtlayabilirsiniz. Onaylanan istemci uygulamaları gerektiren bir koşullu erişim ilkesi, [uygulama tabanlı koşullu erişim ilkesi](app-based-conditional-access.md)olarak da bilinir. Desteklenen onaylanan istemci uygulamalarının listesi için bkz. [onaylanan istemci uygulaması gereksinimi](concept-conditional-access-grant.md#require-approved-client-app).

### <a name="app-protection-policy-preview"></a>Uygulama koruma ilkesi (Önizleme)

Çalışanlarınız hem kişisel hem de iş görevleri için mobil cihaz kullandığından, cihazları sizin yönetmediği durumlarda bile cihazları kullanarak erişilen şirket verilerini koruma olanağına sahip olmak isteyebilirsiniz.
Kuruluşunuzun verilerini mobil cihaz yönetimi (MDM) çözümünden bağımsız olarak korumanıza yardımcı olması için [Intune uygulama koruma ilkelerini](https://docs.microsoft.com/intune/app-protection-policy) kullanabilirsiniz.

Uygulama koruma ilkesi ile, Azure AD 'ye bildirilen istemci uygulamalarına erişimi, [Intune uygulama koruma ilkelerini](https://docs.microsoft.com/intune/app-protection-policy)almış olabilir. Örneğin, Exchange Online 'a erişimi bir Intune uygulama koruma ilkesine sahip Outlook uygulamasıyla kısıtlayabilirsiniz. Uygulama koruma ilkesi gerektiren bir koşullu erişim ilkesi, [Uygulama koruma tabanlı koşullu erişim ilkesi](concept-conditional-access-session.md#application-enforced-restrictions)olarak da bilinir. 

Bir uygulamanın ilke korumalı olarak işaretlenbilmesi için cihazınızın Azure AD 'ye kayıtlı olması gerekir.

Desteklenen İlkeyle korunan istemci uygulamalarının listesi için bkz. [Uygulama koruma ilkesi gereksinimi](concept-conditional-access-session.md#application-enforced-restrictions).

### <a name="terms-of-use"></a>Kullanım koşulları

Bir kaynağa erişim izni verilmeden önce kiracınızdaki bir kullanıcının kullanım koşullarına onay vermesini zorunlu kılabilirsiniz. Yönetici olarak, bir PDF belgesini karşıya yükleyerek kullanım koşullarını yapılandırabilir ve özelleştirebilirsiniz. Bir Kullanıcı bu denetim kapsamında kalırsa, yalnızca kullanım koşulları kabul ediyorsanız, bir uygulamaya erişim izni verilir.

## <a name="custom-controls-preview"></a>Özel denetimler (Önizleme)

Özel denetimler Azure Active Directory Premium P1 sürümünün bir özelliğidir. Özel denetimler kullanılırken, kullanıcılarınız Azure Active Directory dışında daha fazla gereksinimi karşılamak için uyumlu bir hizmete yönlendirilir. Bu denetimi karşılamak için, bir kullanıcının tarayıcısı dış hizmete yönlendirilir, gerekli kimlik doğrulama veya doğrulama etkinliklerini gerçekleştirir ve ardından Azure Active Directory yeniden yönlendirilir. Azure Active Directory yanıtı doğrular ve kullanıcının kimliği başarıyla doğrulanır veya doğrulandıysa, Kullanıcı koşullu erişim akışında devam eder.

Bu denetimler, belirli dış veya özel hizmetlerin koşullu erişim denetimleri olarak kullanılmasına izin verir ve genellikle Koşullu erişimin yeteneklerini genişletir.

Şu anda uyumlu bir hizmet sunan sağlayıcılar şunlardır:

- [Duo güvenliği](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Ping kimliği](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- [RSA](https://community.rsa.com/docs/DOC-81278)
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Thales (Gemalto)](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Truslona](https://www.trusona.com/docs/azure-ad-integration-guide)

Bu hizmetler hakkında daha fazla bilgi için, sağlayıcılara doğrudan başvurun.

### <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel bir denetim oluşturmak için önce kullanmak istediğiniz sağlayıcıya başvurmanız gerekir. Her Microsoft dışı sağlayıcının, hizmetin bir parçası haline gelmesi, abone olması veya başka bir şekilde hizmet vermek ve koşullu erişimle tümleştirileceğini belirtmek için kendi işlemi ve gereksinimleri vardır. Bu noktada, sağlayıcı size JSON biçiminde bir veri bloğu sağlar. Bu veriler, sağlayıcının ve Koşullu erişimin kiracınız için birlikte çalışmasına izin verir, yeni denetimi oluşturur ve kullanıcılarınızın sağlayıcı ile doğrulamayı başarıyla gerçekleştirdiyse Koşullu erişimin nasıl bildirebileceğini tanımlar.

Özel denetimler, çok faktörlü kimlik doğrulaması gerektiren veya ayrıcalıklı kimlik yöneticisi 'ndeki (PıM) rolleri yükseltmek için kimlik koruması otomasyonu ile kullanılamaz.

JSON verilerini kopyalayın ve ilgili metin kutusuna yapıştırın. Yaptığınız değişikliği açıkça anlamadığınız müddetçe JSON üzerinde herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmak, sağlayıcı ile Microsoft arasındaki bağlantıyı bozabilir ve sizin ve kullanıcılarınızı sizin hesaplarınızdan geçirebilir.

Özel denetim oluşturma seçeneği, **koşullu erişim** sayfasının **Yönet** bölümünde bulunur.

![Denetim](./media/controls/82.png)

**Yeni özel denetim**' i tıklattığınızda, denetiminizin JSON verileri için TextBox ile bir dikey pencere açılır.  

![Denetim](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Özel denetimleri silme

Özel bir denetimi silmek için, önce herhangi bir koşullu erişim ilkesinde kullanılmadığından emin olmalısınız. Tamamlandıktan sonra:

1. Özel denetimler listesine git
1. Tıklayın...  
1. **Sil**’i seçin.

### <a name="editing-custom-controls"></a>Özel denetimleri Düzenle

Özel bir denetimi düzenlemek için geçerli denetimi silmeniz ve güncelleştirilmiş bilgilerle yeni bir denetim oluşturmanız gerekir.

## <a name="session-controls"></a>Oturum denetimleri

Oturum denetimleri, bulut uygulaması içinde sınırlı deneyimi etkinleştirir. Oturum denetimleri, bulut uygulamaları tarafından zorlanır ve Azure AD tarafından oturum hakkında uygulamaya sunulan ek bilgilere güvenir.

![Denetim](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Uygulama tarafından zorlanan kısıtlamaları kullan

Bu denetimi, Azure AD 'nin seçili bulut uygulamalarına cihaz bilgilerini geçmesini gerektirmek için kullanabilirsiniz. Cihaz bilgileri, bulut uygulamalarının bir bağlantının uyumlu veya etki alanına katılmış bir cihazdan başlatılıp başlatılmayacağını bilmesini sağlar. Bu denetim yalnızca seçili bulut uygulamaları olarak SharePoint Online ve Exchange Online 'ı destekler. Seçildiğinde, bulut uygulaması, cihaz durumuna bağlı olarak, sınırlı veya tam bir deneyimle, cihaz bilgilerini Kullanıcı sağlamak için kullanır.

Daha fazla bilgi için bkz.:

- [SharePoint Online ile sınırlı erişimi etkinleştirme](https://aka.ms/spolimitedaccessdocs)
- [Exchange Online ile sınırlı erişimi etkinleştirme](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz bkz. [koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)bakın.
