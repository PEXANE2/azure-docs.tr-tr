---
title: Azure AD Uygulama Proxy ve Qlik Sense| Microsoft Dokümanlar
description: Azure portalında Uygulama Proxy'sini açın ve ters proxy için Bağlayıcıları yükleyin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79036990"
---
# <a name="application-proxy-and-qlik-sense"></a>Uygulama Proxy ve Qlik Sense 
Azure Active Directory Application Proxy ve Qlik Sense, Qlik Sense dağıtımınız için uzaktan erişim sağlamak için Uygulama Proxy'sini kolayca kullanabileceğinizden emin olmak için birlikte iş ortaklığı na sahiptir.  

## <a name="prerequisites"></a>Ön koşullar 
Bu senaryonun geri kalanı aşağıdakileri yaptığınızı varsayar:
 
- Yapılandırılmış [Qlik Sense](https://community.qlik.com/docs/DOC-19822). 
- [Uygulama Proxy konektörü yükledi](application-proxy-add-on-premises-application.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Uygulamalarınızı Azure'da yayımlayın 
QlikSense'i yayımlamak için Azure'da iki uygulama yayımlamanız gerekir.  

### <a name="application-1"></a>Başvuru #1: 
Uygulamanızı yayınlamak için aşağıdaki adımları izleyin. 1-8 adımlarının daha ayrıntılı bir şekilde gözden geçirincesi için Azure [AD Application Proxy'yi kullanarak uygulamaları yayımla'ya](application-proxy-add-on-premises-application.md)bakın. 


1. Azure portalında genel yönetici olarak oturum açın. 
2. **Azure Active Directory** > **Enterprise uygulamalarını**seçin. 
3. Bıçağın üst kısmında **Ekle'yi** seçin. 
4. **Şirket içi uygulamayı**seçin. 
5. Yeni uygulamanız hakkında bilgi içeren gerekli alanları doldurun. Ayarlar için aşağıdaki kılavuzu kullanın: 
   - **Dahili URL**: Bu uygulamaqlikSense URL kendisi olan bir iç URL olmalıdır. Örneğin, **https&#58;//demo.qlikemm.com:4244** 
   - **Ön kimlik doğrulama yöntemi**: Azure Etkin Dizini (Önerilir, ancak gerekli değildir) 
1. Bıçağın altındaki **Ekle'yi** seçin. Uygulamanız eklenir ve hızlı başlatma menüsü açılır. 
2. Hızlı başlat menüsünde, **sınama için bir kullanıcı atay'ı**seçin ve uygulamaya en az bir kullanıcı ekleyin. Bu test hesabının şirket içi uygulamaya erişimi olduğundan emin olun. 
3. Test kullanıcı atamasını kaydetmek için **Atla'yı** seçin. 
4. (İsteğe bağlı) Uygulama yönetim bıçağında Tek oturum açma'yı seçin. Açılan menüden **Kerberos Kısıtlı** Temsilciliği'ni seçin ve Qlik yapılandırmanıza göre gerekli alanları doldurun. **Kaydet'i**seçin. 

### <a name="application-2"></a>Uygulama #2: 
Aşağıdaki istisnalar dışında, Uygulama #1 için aynı adımları izleyin: 

**Adım #5**: Dahili URL artık uygulama tarafından kullanılan kimlik doğrulama bağlantı noktasına sahip QlikSense URL olmalıdır. Varsayılan değer HTTPS için **4244** ve Nisan 2018'den önce QlikSense sürümleri için HTTP için **4248'dir.** Nisan 2018'den sonra QlikSense sürümleri için varsayılan değer HTTPS için **443** ve HTTP için **80'dir.**  Örn: **https&#58;//demo.qlik.com:4244**</br></br>
**Adım #10:** SSO'yu ayarlamayın ve **Tek oturum açma** yı devre dışı bırakın
 
 
## <a name="testing"></a>Sınama 
Başvurunuz artık test edilebiş hazırdır. QlikSense'i Uygulama #1'nde yayımlamak için kullandığınız harici URL'ye erişin ve her iki uygulamaya atanmış bir kullanıcı olarak oturum açın.  

## <a name="additional-references"></a>Ek başvurular
Uygulama Proxy ile Qlik Sense yayımlama hakkında daha fazla bilgi için, Qlik Topluluk Makaleler aşağıdaki bakın: 
- [Qlik Sense ile Kerberos Kısıtlı Delegasyon kullanarak Entegre Windows Kimlik Doğrulama ile Azure AD](https://community.qlik.com/docs/DOC-20183)
- [Azure AD Application Proxy ile Qlik Sense entegrasyonu](https://community.qlik.com/t5/Technology-Partners-Ecosystem/Azure-AD-Application-Proxy/ta-p/1528396)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
- [Uygulama Proxy konektörleri ile çalışma](application-proxy-connector-groups.md)

