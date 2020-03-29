---
title: Azure Active Directory Application Proxy ve Tableau | Microsoft Dokümanlar
description: Tableau dağıtımınız için uzaktan erişim sağlamak için Azure Active Directory (Azure AD) Uygulama Proxy'sini nasıl kullanacağınızı öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1aa99e7e71ad78a62c1a9da303b2ecc8347ebeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65783860"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory Application Proxy ve Tableau 

Azure Active Directory Application Proxy ve Tableau, Tableau dağıtımınız için uzaktan erişim sağlamak için Uygulama Proxy'sini kolayca kullanabileceğinizden emin olmak için ortaklık sağladı. Bu makalede, bu senaryonun nasıl yapılandırılabildiğini açıklanmaktadır.  

## <a name="prerequisites"></a>Ön koşullar 

Bu makaledeki senaryo, sahip olduğunuzu varsayar:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) yapılandırıldı. 

- Bir [Uygulama Proxy bağlayıcısı](application-proxy-add-on-premises-application.md) yüklendi. 

 
## <a name="enabling-application-proxy-for-tableau"></a>Tableau için Uygulama Proxy'sini Etkinleştirme 

Uygulama Proxy Tableau düzgün çalışması için gerekli olan OAuth 2.0 Hibe Akışı, destekler. Bu, bu uygulamayı etkinleştirmek için aşağıdaki yayımlama adımlarını izleyerek yapılandırmak dışında artık özel adımlar gerektirmemektedir.


## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure'da yayımlayın 

Tableau'yu yayınlamak için Azure Portalı'nda bir uygulama yayımlamanız gerekir.

Için:

- 1-8 adımlarının ayrıntılı yönergeleri, [bkz.](application-proxy-add-on-premises-application.md) 
- App Proxy alanları için Tableau değerlerini nasıl bulacağınız hakkında bilgi, lütfen Tableau belgelerine bakın.  

**Uygulamanızı yayınlamak için:** 


1. Uygulama yöneticisi olarak [Azure portalında](https://portal.azure.com) oturum açın. 

2. **Azure Active Directory > Kurumsal uygulamalarını**seçin. 

3. Bıçağın üst kısmında **Ekle'yi** seçin. 

4. **Şirket içi uygulamayı**seçin. 

5. Yeni uygulamanız hakkında bilgi içeren gerekli alanları doldurun. Ayarlar için aşağıdaki kılavuzu kullanın: 

    - **Dahili URL**: Bu uygulamanın Tablo URL'si olan dahili bir URL'si olmalıdır. Örneğin, `https://adventure-works.tableau.com`. 

    - **Ön kimlik doğrulama yöntemi**: Azure Active Directory (önerilir, ancak gerekli değildir). 

6. Bıçağın üst kısmında **Ekle'yi** seçin. Uygulamanız eklenir ve hızlı başlatma menüsü açılır. 

7. Hızlı başlat menüsünde, **sınama için bir kullanıcı atay'ı**seçin ve uygulamaya en az bir kullanıcı ekleyin. Bu test hesabının şirket içi uygulamaya erişimi olduğundan emin olun. 

8. Test kullanıcı atamasını kaydetmek için **Atla'yı** seçin. 

9. (İsteğe bağlı) Uygulama yönetimi sayfasında **Tek oturum açma'yı**seçin. Açılan menüden **Tümleşik Windows Kimlik Doğrulaması'nı** seçin ve Tableau yapılandırmanıza göre gerekli alanları doldurun. **Kaydet'i**seçin. 

 

## <a name="testing"></a>Sınama 

Başvurunuz artık test edilebiş hazırdır. Tableau'yu yayımlamak için kullandığınız harici URL'ye erişin ve her iki uygulamaya atanan bir kullanıcı olarak oturum açın.



## <a name="next-steps"></a>Sonraki adımlar

Azure AD Uygulama Proxy hakkında daha fazla bilgi için [şirket içi uygulamalara nasıl güvenli uzaktan erişim sağlayacağınız](application-proxy.md)bilgisini öğrenin.

