---
title: Azure AD uygulama ara sunucusu ve Qlik Sense | Microsoft Docs
description: Azure portalında uygulama ara sunucusunu etkinleştirmek ve için ters proxy bağlayıcıları yükleyin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/06/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2edf63da0fd09f829f936b54eb088c34dfe029a1
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79036990"
---
# <a name="application-proxy-and-qlik-sense"></a>Uygulama Ara sunucusu ve Qlik Sense 
Azure Active Directory Uygulama Ara sunucusu ve Qlik Sense kolayca Qlik Sense dağıtımınız için uzaktan erişim sağlamak için uygulama ara sunucusu kullanmanız mümkün olduğundan emin olmak için birlikte kurdu.  

## <a name="prerequisites"></a>Önkoşullar 
Bu senaryonun kalanı aşağıdaki işlemleri olduğunu varsayar:
 
- [Qlik Sense](https://community.qlik.com/docs/DOC-19822)yapılandırıldı. 
- [Uygulama proxy bağlayıcısını yükleme](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure'da yayımlayın 
QlikSense yayımlamak için Azure'da iki uygulama yayımlamak gerekir.  

### <a name="application-1"></a>Uygulama #1: 
Uygulamanızı yayımlamak için aşağıdaki adımları izleyin. 1-8 adımları hakkında daha ayrıntılı yönergeler için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md). 


1. Azure portalına genel yönetici olarak oturum açın. 
2. **Kurumsal uygulamalar** > **Azure Active Directory** seçin. 
3. Dikey pencerenin üst kısmında **Ekle** ' yi seçin. 
4. **Şirket içi uygulama '** yı seçin. 
5. Yeni uygulamanız hakkındaki bilgilerle gerekli alanları doldurun. Ayarları için aşağıdaki yönergeleri kullanın: 
   - **Iç URL**: Bu uygulamanın, QlikSense URL 'sinin kendisi olan BIR Iç URL 'si olmalıdır. Örneğin, **https&#58;//demo.qlikemm.com:4244** 
   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory (önerilir, ancak gerekli değildir) 
1. Dikey pencerenin alt kısmında **Ekle** ' yi seçin. Uygulamanızı eklenir ve Hızlı Başlangıç menüsü açılır. 
2. Hızlı Başlangıç menüsünde, **test için Kullanıcı ata**' yı seçin ve uygulamaya en az bir kullanıcı ekleyin. Bu test hesabı şirket içi uygulamaya erişimi olduğundan emin olun. 
3. Test Kullanıcı atamasını kaydetmek için **ata** ' yı seçin. 
4. (İsteğe bağlı) Uygulama Yönetimi dikey penceresinde, çoklu oturum açma seçin. Açılan menüden **Kerberos kısıtlanmış temsilciyi** seçin ve Qlik yapılandırmanıza göre gerekli alanları doldurun. **Kaydet**’i seçin. 

### <a name="application-2"></a>Uygulama #2: 
Aşağıdaki istisnalarla birlikte uygulama #1 olduğu gibi aynı adımları izleyin: 

**Adım #5**: İç URL artık uygulama tarafından kullanılan kimlik doğrulama bağlantı noktası Ile QlikSense URL 'si olmalıdır. Varsayılan değer, HTTPS için **4244** ve 2018 Nisan 'Dan önce QlikSense sürümleri için **4248** ' dir. 2018 Nisan sonrasında QlikSense yayınları için varsayılan değer, HTTPS için **443** ve HTTP için **80** .  Ex: **https&#58;//demo.Qlik.com:4244**</br></br>
**Adım #10:** SSO 'yu ayarlama ve **Çoklu oturum açmayı devre dışı** bırakma
 
 
## <a name="testing"></a>Test Etme 
Uygulamanızı şimdi test etmek hazırdır. Her iki uygulama için atanan bir kullanıcı olarak uygulama #1 ve oturum açma QlikSense yayımlamak için kullanılan dış URL erişin.  

## <a name="additional-references"></a>Ek başvurular
Uygulama proxy 'Si ile Qlik Sense yayımlama hakkında daha fazla bilgi için, Qlik Community makalelerini inceleyin: 
- [Qlik Sense ile Kerberos kısıtlanmış bir temsilciyi kullanarak tümleşik Windows kimlik doğrulaması ile Azure AD](https://community.qlik.com/docs/DOC-20183)
- [Azure AD Uygulama Ara Sunucusu Qlik Sense tümleştirmesi](https://community.qlik.com/t5/Technology-Partners-Ecosystem/Azure-AD-Application-Proxy/ta-p/1528396)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama ara sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
- [Uygulama proxy bağlayıcıları ile çalışma](application-proxy-connector-groups.md)

