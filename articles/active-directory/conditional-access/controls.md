---
title: Azure AD koşullu erişim 'de özel denetimler
description: Koşullu erişim Azure Active Directory içindeki özel denetimlerin nasıl çalıştığını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 02/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f1df037b66c72177a96f77231cee70782d04992
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620706"
---
# <a name="custom-controls-preview"></a>Özel denetimler (Önizleme)

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

## <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel bir denetim oluşturmak için önce kullanmak istediğiniz sağlayıcıya başvurmanız gerekir. Her Microsoft dışı sağlayıcının, hizmetin bir parçası haline gelmesi, abone olması veya başka bir şekilde hizmet vermek ve koşullu erişimle tümleştirileceğini belirtmek için kendi işlemi ve gereksinimleri vardır. Bu noktada, sağlayıcı size JSON biçiminde bir veri bloğu sağlar. Bu veriler, sağlayıcının ve Koşullu erişimin kiracınız için birlikte çalışmasına izin verir, yeni denetimi oluşturur ve kullanıcılarınızın sağlayıcı ile doğrulamayı başarıyla gerçekleştirdiyse Koşullu erişimin nasıl bildirebileceğini tanımlar.

Özel denetimler, çok faktörlü kimlik doğrulaması gerektiren veya ayrıcalıklı kimlik yöneticisi 'ndeki (PıM) rolleri yükseltmek için kimlik koruması otomasyonu ile kullanılamaz.

JSON verilerini kopyalayın ve ilgili metin kutusuna yapıştırın. Yaptığınız değişikliği açıkça anlamadığınız müddetçe JSON üzerinde herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmak, sağlayıcı ile Microsoft arasındaki bağlantıyı bozabilir ve sizin ve kullanıcılarınızı sizin hesaplarınızdan geçirebilir.

Özel denetim oluşturma seçeneği, **koşullu erişim** sayfasının **Yönet** bölümünde bulunur.

![Denetim](./media/controls/82.png)

**Yeni özel denetim**' i tıklattığınızda, denetiminizin JSON verileri için TextBox ile bir dikey pencere açılır.  

![Denetim](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>Özel denetimleri silme

Özel bir denetimi silmek için, önce herhangi bir koşullu erişim ilkesinde kullanılmadığından emin olmalısınız. Tamamlandıktan sonra:

1. Özel denetimler listesine git
1. Tıklayın...  
1. **Sil**’i seçin.

## <a name="editing-custom-controls"></a>Özel denetimleri Düzenle

Özel bir denetimi düzenlemek için geçerli denetimi silmeniz ve güncelleştirilmiş bilgilerle yeni bir denetim oluşturmanız gerekir.

## <a name="session-controls"></a>Oturum denetimleri

Oturum denetimleri, bulut uygulaması içinde sınırlı deneyimi etkinleştirir. Oturum denetimleri, bulut uygulamaları tarafından zorlanır ve Azure AD tarafından oturum hakkında uygulamaya sunulan ek bilgilere güvenir.

![Denetim](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Uygulama tarafından zorlanan kısıtlamaları kullan

Bu denetimi, Azure AD 'nin seçili bulut uygulamalarına cihaz bilgilerini geçmesini gerektirmek için kullanabilirsiniz. Cihaz bilgileri, bulut uygulamalarının bir bağlantının uyumlu veya etki alanına katılmış bir cihazdan başlatılıp başlatılmayacağını bilmesini sağlar. Bu denetim yalnızca seçili bulut uygulamaları olarak SharePoint Online ve Exchange Online 'ı destekler. Seçildiğinde, bulut uygulaması, cihaz durumuna bağlı olarak, sınırlı veya tam bir deneyimle, cihaz bilgilerini Kullanıcı sağlamak için kullanır.

Daha fazla bilgi için bkz:

- [SharePoint Online ile sınırlı erişimi etkinleştirme](https://aka.ms/spolimitedaccessdocs)
- [Exchange Online ile sınırlı erişimi etkinleştirme](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz bkz. [koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)bakın.
