---
title: Azure AD Uygulaması Proxy ve Qlik Sense | Microsoft Docs
description: Azure portal uygulama proxy 'Sini açın ve ters proxy için bağlayıcıları yükledikten sonra.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 09/06/2018
ms.author: kenwith
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9696d48db7d051f3a8bdf16f93438fb71f025dc
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84760057"
---
# <a name="application-proxy-and-qlik-sense"></a>Uygulama proxy 'Si ve Qlik Sense 
Azure Active Directory Uygulama Ara Sunucusu ve Qlik Sense, Qlik Sense dağıtımınız için uzaktan erişim sağlamak üzere kolayca uygulama proxy 'Si kullanabildiğinizden emin olmak için iş ortağı ile işbirliği yaptı.  

## <a name="prerequisites"></a>Ön koşullar 
Bu senaryonun geri kalanı şunları yaptığınızı varsayar:
 
- [Qlik Sense](https://community.qlik.com/docs/DOC-19822)yapılandırıldı. 
- [Uygulama proxy bağlayıcısını yükleme](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure 'da yayımlayın 
Qliksiz yayımlamak için, Azure 'da iki uygulama yayımlamanız gerekir.  

### <a name="application-1"></a>Uygulama #1: 
Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8 adımları hakkında daha ayrıntılı yönergeler için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md). 


1. Azure portalında genel yönetici olarak oturum açın. 
2. **Azure Active Directory**  >  **Kurumsal uygulamalar**' ı seçin. 
3. Dikey pencerenin üst kısmında **Ekle** ' yi seçin. 
4. **Şirket içi uygulama '** yı seçin. 
5. Gerekli alanları yeni uygulamanız hakkındaki bilgilerle doldurun. Ayarlar için aşağıdaki kılavuzu kullanın: 
   - **Iç URL**: Bu uygulamanın, QlikSense URL 'sinin kendisi olan BIR Iç URL 'si olmalıdır. Örneğin, **https&#58;//demo.qlikemm.com:4244** 
   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory (önerilir, ancak gerekli değildir) 
1. Dikey pencerenin alt kısmında **Ekle** ' yi seçin. Uygulamanız eklenir ve hızlı başlangıç menüsü açılır. 
2. Hızlı Başlangıç menüsünde, **test için Kullanıcı ata**' yı seçin ve uygulamaya en az bir kullanıcı ekleyin. Bu sınama hesabının şirket içi uygulamaya erişimi olduğundan emin olun. 
3. Test Kullanıcı atamasını kaydetmek için **ata** ' yı seçin. 
4. Seçim Uygulama yönetimi dikey penceresinde çoklu oturum açma ' yı seçin. Açılan menüden **Kerberos kısıtlanmış temsilciyi** seçin ve Qlik yapılandırmanıza göre gerekli alanları doldurun. **Kaydet**’i seçin. 

### <a name="application-2"></a>Uygulama #2: 
Aşağıdaki özel durumlarla birlikte uygulama #1 ile aynı adımları izleyin: 

**Adım #5**: İç URL artık uygulama tarafından kullanılan kimlik doğrulama bağlantı noktası Ile QlikSense URL 'si olmalıdır. Varsayılan değer, HTTPS için **4244** ve 2018 Nisan 'Dan önce QlikSense sürümleri için **4248** ' dir. 2018 Nisan sonrasında QlikSense yayınları için varsayılan değer, HTTPS için **443** ve HTTP için **80** .  Ex: **https&#58;//demo.Qlik.com:4244**</br></br>
**Adım #10:** SSO 'yu ayarlama ve **Çoklu oturum açmayı devre dışı** bırakma
 
 
## <a name="testing"></a>Test Etme 
Uygulamanız şimdi test etmeye hazırdır. Uygulama #1 'da QlikSense yayımlamak için kullandığınız dış URL 'ye erişin ve her iki uygulama için de bir kullanıcı olarak oturum açın.  

## <a name="additional-references"></a>Ek başvurular
Uygulama proxy 'Si ile Qlik Sense yayımlama hakkında daha fazla bilgi için, Qlik Community makalelerini inceleyin: 
- [Qlik Sense ile Kerberos kısıtlanmış bir temsilciyi kullanarak tümleşik Windows kimlik doğrulaması ile Azure AD](https://community.qlik.com/docs/DOC-20183)
- [Azure AD Uygulama Ara Sunucusu Qlik Sense tümleştirmesi](https://community.qlik.com/t5/Technology-Partners-Ecosystem/Azure-AD-Application-Proxy/ta-p/1528396)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
- [Uygulama proxy bağlayıcıları ile çalışma](application-proxy-connector-groups.md)

