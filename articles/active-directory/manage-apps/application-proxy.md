---
title: Şirket içi uygulamalara uzaktan erişim - Azure AD Application Proxy
description: Azure Active Directory'nin Uygulama Proxy'si şirket içi web uygulamalarına güvenli uzaktan erişim sağlar. Azure AD'de tek bir oturum açtıktan sonra, kullanıcılar hem bulut hem de şirket içi uygulamalara harici bir URL veya dahili uygulama portalı üzerinden erişebilir. Örneğin, Application Proxy uzak masaüstü, SharePoint, Takımlar, Tableau, Qlik ve iş hattı (LOB) uygulamalarına uzaktan erişim ve tek oturum açma sağlayabilir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274843"
---
# <a name="remote-access-to-on-premises-applications-through-azure-active-directorys-application-proxy"></a>Azure Active Directory'nin Uygulama Proxy'si aracılığıyla şirket içi uygulamalara uzaktan erişim 

Azure Active Directory'nin Uygulama Proxy'si şirket içi web uygulamalarına güvenli uzaktan erişim sağlar. Azure AD'de tek bir oturum açtıktan sonra, kullanıcılar hem bulut hem de şirket içi uygulamalara harici bir URL veya dahili uygulama portalı üzerinden erişebilir. Örneğin, Application Proxy uzak masaüstü, SharePoint, Takımlar, Tableau, Qlik ve iş hattı (LOB) uygulamalarına uzaktan erişim ve tek oturum açma sağlayabilir.

Azure AD Uygulama Ara Sunucusu şu özelliklere sahiptir:

- **Kullanımı basit.** Kullanıcılar şirket içi uygulamalarınıza, Azure AD ile entegre edilmiş O365 ve diğer SaaS uygulamalarına eriştisikleri gibi erişebilir. Uygulama Ara Sunucusu ile çalışması için uygulamanızı değiştirmeniz veya güncelleştirmeniz gerekmez. 

- **Güvenli**. Şirket içi uygulamalar Azure'un yetkilendirme denetimlerini ve güvenlik analitiğini kullanabilir. Örneğin, şirket içi uygulamalar Koşullu Erişim ve iki aşamalı doğrulamayı kullanabilir. Uygulama Proxy güvenlik duvarı üzerinden gelen bağlantıları açmanızı gerektirmez.
 
- **Uygun maliyetli.** Şirket içi çözümler genellikle arındırılmış bölgeler (DMZ'ler), kenar sunucuları veya diğer karmaşık altyapıları ayarlamanızı ve korumanızı gerektirir. Uygulama Proxy'si bulutta çalışır, bu da kullanımı kolaylaştırır. Uygulama Proxy'sini kullanmak için ağ altyapısını değiştirmeniz veya şirket içi ortamınıza ek cihazlar yüklemeniz gerekmez.

## <a name="what-is-application-proxy"></a>Uygulama Ara Sunucusu nedir?
Application Proxy, Kullanıcıların şirket içi web uygulamalarına uzak bir istemciden erişmesini sağlayan Azure AD özelliğidir. Application Proxy hem bulutta çalışan Application Proxy hizmetini hem de şirket içi sunucuda çalışan Application Proxy bağlayıcısını içerir. Azure AD, Uygulama Proxy hizmeti ve Application Proxy bağlayıcısı, kullanıcı oturum açma belirteci'ni Azure AD'den web uygulamasına güvenli bir şekilde geçirmek için birlikte çalışır.

Uygulama Proxy ile çalışır:

* Kimlik doğrulama için [Tümleşik Windows Kimlik Doğrulaması](application-proxy-configure-single-sign-on-with-kcd.md) kullanan Web uygulamaları  
* Form tabanlı veya [üstbilgi tabanlı](application-proxy-configure-single-sign-on-with-ping-access.md) erişimi kullanan web uygulamaları  
* Farklı cihazlardaki zengin uygulamalara maruz bırakmak istediğiniz Web API'leri  
* [Uzak Masaüstü Ağ Geçidi'nin](application-proxy-integrate-with-remote-desktop-services.md) arkasında barındırılan uygulamalar  
* Active Directory Authentication Library (ADAL) ile entegre edilmiş zengin istemci uygulamaları

Uygulama Proxy tek oturum açma destekler. Desteklenen yöntemler hakkında daha fazla bilgi için [bkz.](what-is-single-sign-on.md#choosing-a-single-sign-on-method)

Uygulama Proxy uzak kullanıcılara iç kaynaklara erişim vermek için önerilir. Uygulama Proxy vpn veya ters proxy gereksinimini değiştirir. Şirket ağındaki dahili kullanıcılar için tasarlanmamıştır.  Gereksiz yere Uygulama Proxy'si kullanan bu kullanıcılar beklenmeyen ve istenmeyen performans sorunlarına neden olabilir.

## <a name="how-application-proxy-works"></a>Uygulama Proxy nasıl çalışır?

Aşağıdaki diyagram, Azure AD ve Uygulama Proxy'sinin şirket içi uygulamalarda tek oturum açma sağlamak için birlikte nasıl çalıştığını gösterir.

![AzureAD Uygulama Proxy diyagramı](./media/application-proxy/azureappproxxy.png)

1. Kullanıcı uygulamanın bitiş noktası üzerinden erişiminden sonra, kullanıcı Azure AD oturum açma sayfasına yönlendirilir. 
2. Başarılı bir oturum açmadan sonra Azure AD, kullanıcının istemci aygıtına bir belirteç gönderir.
3. İstemci belirteci, kullanıcı ana adı (UPN) ve güvenlik soyadı (SPN) belirteci alır Uygulama Proxy hizmetine gönderir. Application Proxy daha sonra isteği Application Proxy bağlayıcısına gönderir.
4. Tek oturum açma yapılandırıldıysanız, bağlayıcı kullanıcı adına gerekli olan ek kimlik doğrulamasını gerçekleştirir.
5. Bağlayıcı, isteği şirket içi uygulamaya gönderir.  
6. Yanıt, bağlayıcı ve Uygulama Proxy hizmeti aracılığıyla kullanıcıya gönderilir.

| Bileşen | Açıklama |
| --------- | ----------- |
| Uç Nokta  | Bitiş noktası bir URL veya [son kullanıcı portalıdır.](end-user-experiences.md) Kullanıcılar harici bir URL'ye erişerek ağınızın dışındayken uygulamalara ulaşabilirler. Ağınızdaki kullanıcılar uygulamaya bir URL veya son kullanıcı portalı üzerinden erişebilir. Kullanıcılar bu uç noktalardan birine gittiklerinde, Azure AD'de kimlik doğrulaması yaparlar ve daha sonra bağlayıcı aracılığıyla şirket içi uygulamaya yönlendirilirler.|
| Azure AD | Azure AD, bulutta depolanan kiracı dizinini kullanarak kimlik doğrulamasını gerçekleştirir. |
| Uygulama Proxy hizmeti | Bu Uygulama Proxy hizmeti, Azure AD'nin bir parçası olarak bulutta çalışır. Oturum açma belirteci kullanıcıdan Uygulama Proxy Bağlayıcısı'na geçer. Application Proxy, istek üzerine erişilebilen tüm üstbilgileri ileter ve üstbilgileri protokolüne göre istemci IP adresine ayarlar. Proxy'ye gelen istek zaten bu üstbilgi varsa, istemci IP adresi üstbilginin değeri olan virgülayrılmış listenin sonuna eklenir.|
| Uygulama Proxy Konektörü | Bağlayıcı, ağınızın içindeki bir Windows Server'da çalışan hafif bir aracıdır. Bağlayıcı, buluttaki Uygulama Proxy hizmeti ile şirket içi uygulama arasındaki iletişimi yönetir. Bağlayıcı yalnızca giden bağlantıları kullanır, böylece gelen bağlantı noktalarını açmanız veya DMZ'ye herhangi bir şey koymanız gerekmez. Bağlayıcılar devletsizdir ve gerektiğinde buluttan bilgi çeker. Bağlayıcılar hakkında yük bakiyesi ve kimlik doğrulaması gibi daha fazla bilgi için Azure [AD Uygulama Proxy bağlayıcılarını anlayın.](application-proxy-connectors.md)|
| Aktif Dizin (AD) | Etkin Dizin, etki alanı hesapları için kimlik doğrulaması gerçekleştirmek için şirket içinde çalışır. Tek oturum açma yapılandırıldığında, bağlayıcı gerekli herhangi bir ek kimlik doğrulaması gerçekleştirmek için AD ile iletişim kurar.
| Şirket içi uygulama | Son olarak, kullanıcı şirket içi bir uygulamaya erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar
Uygulama Proxy'sini kullanmaya başlamak [için Bkz. Öğretici: Uygulama Proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](application-proxy-add-on-premises-application.md) 

En son haberler ve güncellemeler için [Uygulama Proxy bloguna](https://blogs.technet.com/b/applicationproxyblog/) bakın


