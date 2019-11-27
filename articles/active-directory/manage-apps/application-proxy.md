---
title: Şirket içi uygulamalara uzaktan erişim-Azure AD Uygulama Ara Sunucusu
description: Azure Active Directory uygulama proxy 'Si, şirket içi Web uygulamalarına güvenli uzaktan erişim sağlar. Azure AD 'de çoklu oturum açma işleminden sonra, kullanıcılar harici bir URL veya dahili uygulama portalı aracılığıyla hem buluta hem de şirket içi uygulamalara erişebilir. Örneğin, uygulama proxy 'Si Uzak Masaüstü, SharePoint, takımlar, Tableau, Qlik ve iş kolu (LOB) uygulamaları için uzaktan erişim ve çoklu oturum açma sağlayabilir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4780786f0caea2c211b6b93fb0736feaade8de80
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74274843"
---
# <a name="remote-access-to-on-premises-applications-through-azure-active-directorys-application-proxy"></a>Azure Active Directory uygulama proxy 'Si aracılığıyla şirket içi uygulamalara uzaktan erişim 

Azure Active Directory uygulama proxy 'Si, şirket içi Web uygulamalarına güvenli uzaktan erişim sağlar. Azure AD 'de çoklu oturum açma işleminden sonra, kullanıcılar harici bir URL veya dahili uygulama portalı aracılığıyla hem buluta hem de şirket içi uygulamalara erişebilir. Örneğin, uygulama proxy 'Si Uzak Masaüstü, SharePoint, takımlar, Tableau, Qlik ve iş kolu (LOB) uygulamaları için uzaktan erişim ve çoklu oturum açma sağlayabilir.

Azure AD uygulama ara sunucusu aşağıdaki gibidir:

- **Kullanımı basittir**. Kullanıcılar, şirket içi uygulamalarınıza O365 ve Azure AD ile tümleştirilmiş diğer SaaS uygulamalarına erişen şekilde erişebilirler. Değiştirmeniz veya uygulamalarınızı uygulaması Ara sunucusu ile çalışacak şekilde güncelleştirmeniz gerekmez. 

- **Güvenli**. Şirket içi uygulamalar, Azure 'un yetkilendirme denetimlerini ve güvenlik analizlerini kullanabilir. Örneğin, şirket içi uygulamalar koşullu erişim ve iki adımlı doğrulama kullanabilir. Uygulama proxy 'Si, gelen bağlantıları güvenlik duvarınız aracılığıyla açmanızı gerektirmez.
 
- Uygun **maliyetli**. Şirket içi çözümler genellikle, Parçalandırılmış bölgeleri (DMZs), Edge sunucularını veya diğer karmaşık altyapıları ayarlamanızı ve bakımını korumanızı gerektirir. Uygulama proxy 'Si bulutta çalışır ve bu sayede kullanımı kolaylaşır. Uygulama proxy 'Sini kullanmak için, ağ altyapısını değiştirmeniz veya şirket içi ortamınıza ek gereçler yüklemeniz gerekmez.

## <a name="what-is-application-proxy"></a>Uygulama Ara Sunucusu nedir?
Uygulama proxy 'Si, kullanıcıların uzak bir istemciden şirket içi Web uygulamalarına erişmesini sağlayan bir Azure AD özelliğidir. Uygulama proxy 'Si, hem bulutta çalışan uygulama proxy hizmetini hem de şirket içi sunucuda çalışan uygulama proxy bağlayıcısını içerir. Azure AD, uygulama proxy hizmeti ve uygulama proxy Bağlayıcısı, Azure AD 'deki Kullanıcı oturum açma belirtecini Web uygulamasına güvenli bir şekilde geçirmek için birlikte çalışır.

Uygulama proxy 'Si ile birlikte kullanılabilir:

* Kimlik doğrulaması için [Tümleşik Windows kimlik doğrulaması](application-proxy-configure-single-sign-on-with-kcd.md) kullanan Web uygulamaları  
* Form tabanlı veya [üst bilgi tabanlı](application-proxy-configure-single-sign-on-with-ping-access.md) erişim kullanan Web uygulamaları  
* Web için zengin uygulamalar farklı cihazlarda kullanıma sunmak istiyorsanız API'leri  
* [Uzak Masaüstü Ağ Geçidi](application-proxy-integrate-with-remote-desktop-services.md) arkasında barındırılan uygulamalar  
* Active Directory Authentication Library (ADAL) ile tümleşik olan zengin istemci uygulamaları

Uygulama proxy 'Si çoklu oturum açmayı destekler. Desteklenen yöntemler hakkında daha fazla bilgi için bkz. [Çoklu oturum açma yöntemi seçme](what-is-single-sign-on.md#choosing-a-single-sign-on-method).

Uzak kullanıcılara iç kaynaklara erişim izni vermek için uygulama proxy 'Si önerilir. Uygulama proxy 'Si, VPN veya ters proxy gereksinimini yerine koyar. Şirket ağındaki dahili kullanıcılara yönelik değildir.  Uygulama proxy 'Sini gereksiz şekilde kullanan bu kullanıcılar beklenmedik ve istenmeyen performans sorunları ortaya çıkarabilir.

## <a name="how-application-proxy-works"></a>Uygulama proxy 'Si nasıl kullanılır

Aşağıdaki diyagramda, şirket içi uygulamalarda çoklu oturum açma sağlamak için Azure AD ve uygulama proxy 'Sinin birlikte nasıl çalıştığı gösterilmektedir.

![AzureAD uygulama proxy'si diyagramı](./media/application-proxy/azureappproxxy.png)

1. Kullanıcı, bir uç noktası aracılığıyla uygulama eriştiğini sonra kullanıcının Azure AD oturum açma sayfasına yönlendirilir. 
2. Başarılı bir oturum açma işleminden sonra Azure AD, kullanıcının istemci cihazına bir belirteç gönderir.
3. İstemci belirteci, belirteç üzerinden Kullanıcı asıl adını (UPN) ve güvenlik asıl adını (SPN) alan uygulama ara sunucusu hizmetine gönderir. Ardından uygulama proxy 'si, isteği uygulama proxy Bağlayıcısı 'na gönderir.
4. Çoklu oturum açma yapılandırdıysanız, bağlayıcı kullanıcı adına gerekli tüm ek kimlik doğrulaması gerçekleştirir.
5. Bağlayıcıyı şirket içi uygulamaya isteği gönderir.  
6. Yanıt, kullanıcıya bağlayıcı ve uygulama proxy hizmeti üzerinden gönderilir.

| Bileşen | Açıklama |
| --------- | ----------- |
| Uç Nokta  | Uç nokta bir URL veya [Son Kullanıcı portalıdır](end-user-experiences.md). Kullanıcılar, uygulamalar, Ağ üzerindeyken dışında bir dış URL erişerek ulaşabilirsiniz. Ağınızdaki kullanıcılar, bir URL veya bir son kullanıcı portalı uygulamaya erişebilir. Kullanıcılar bu uç noktaları birine gittiğinizde, Azure AD'de kimlik doğrulaması yapmak ve şirket içi uygulama Bağlayıcısı üzerinden yönlendirilir.|
| Azure AD | Azure AD, bulutta depolanan kiracı dizinini kullanarak kimlik doğrulamasını gerçekleştirir. |
| Uygulama proxy hizmeti | Bu uygulama proxy hizmeti, Azure AD 'nin bir parçası olarak bulutta çalışır. Kullanıcının oturum açma belirtecini, uygulama proxy Bağlayıcısı 'na geçirir. Uygulama proxy 'Si, istekteki tüm erişilebilir üstbilgileri iletir ve üst bilgileri protokol başına, istemci IP adresine ayarlar. Ara sunucuya gelen istek zaten bu üstbilgiye sahipse, istemci IP adresi üst bilginin değeri olan virgülle ayrılmış listenin sonuna eklenir.|
| Uygulama proxy Bağlayıcısı | Bağlayıcı, ağınız içindeki bir Windows Server üzerinde çalışan hafif bir aracıdır. Bağlayıcı, bulutta ve şirket içi uygulamada bulunan uygulama proxy hizmeti arasındaki iletişimi yönetir. Bağlayıcı yalnızca giden bağlantıları kullanır, bu nedenle herhangi bir gelen bağlantı noktasını açmanız veya DMZ 'e herhangi bir şey yerleştirmeniz gerekmez. Bağlayıcılar, durum bilgisiz olduğundan ve gerektiği şekilde buluttan bilgi isteme. Yük Dengeleme ve kimlik doğrulama gibi bağlayıcılar hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu bağlayıcılarını anlama](application-proxy-connectors.md).|
| Active Directory (AD) | Active Directory etki alanı hesapları için kimlik doğrulaması yapmak üzere şirket içinde çalışır. Çoklu oturum açma yapılandırıldığında, bağlayıcı, gereken ek kimlik doğrulamasını gerçekleştirmek için AD ile iletişim kurar.
| Şirket içi uygulama | Son olarak, Kullanıcı şirket içi bir uygulamaya erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar
Uygulama proxy 'Sini kullanmaya başlamak için bkz. [öğretici: uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md). 

En son haberler ve güncelleştirmeler için bkz. [uygulama proxy blogu](https://blogs.technet.com/b/applicationproxyblog/)


