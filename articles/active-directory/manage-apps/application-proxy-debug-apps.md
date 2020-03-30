---
title: Hata Ayıklama Uygulama Proxy uygulamaları - Azure Active Directory | Microsoft Dokümanlar
description: Azure Etkin Dizin (Azure AD) Uygulama Proxy uygulamalarıyla hata ayıklama sorunları.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: 575891d99c077299f5e7abf008c1ebb2b158373f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74382070"
---
# <a name="debug-application-proxy-application-issues"></a>Uygulama Ara Sunucusu uygulama hatalarını ayıklama 

Bu makale, Azure Etkin Dizin (Azure AD) Uygulama Proxy uygulamalarıyla ilgili sorunları gidermenize yardımcı olur. Uygulama Proxy hizmetini şirket içi bir web uygulamasına uzaktan erişim için kullanıyorsanız, ancak uygulamaya bağlanmada sorun yaşıyorsanız, uygulama hatasını ayıklamak için bu akış şemasını kullanın. 

## <a name="before-you-begin"></a>Başlamadan önce

Uygulama Proxy sorunları giderme, size bağlayıcılar ile başlamanızı öneririz. İlk olarak, Uygulama Proxy bağlayıcılarının doğru şekilde yapılandırıldığından emin olmak için [Hata Ayıklama Uygulama Proxy Bağlayıcısı sorunlarındaki](application-proxy-debug-connectors.md) sorun giderme akışını izleyin. Sorun yaşadığınız için, uygulamayı gidermek için bu makaleye dönün.  

Uygulama Proxy hakkında daha fazla bilgi için bkz:

- [Uygulama Proxy aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)
- [Uygulama Proxy konektörleri](application-proxy-connectors.md)
- [Konektör yükleme ve kaydetme](application-proxy-add-on-premises-application.md)
- [Sorun Giderme Uygulama Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)

## <a name="flowchart-for-application-issues"></a>Uygulama sorunları için akış şeması

Bu akış şeması, uygulamaya bağlanmayla ilgili daha yaygın sorunlardan bazılarını hata ayıklama adımlarından geçirmenize yönelik adımlarboyunca size yol alar. Her adımla ilgili ayrıntılar için akış şemasının ardından tabloya bakın.

![Bir uygulamanın hata ayıklama adımlarını gösteren akış şeması](media/application-proxy-debug-apps/application-proxy-apps-debugging-flowchart.png)

|  | Eylem | Açıklama | 
|---------|---------|---------|
|1 | Bir tarayıcı açın, uygulamaya erişin ve kimlik bilgilerinizi girin | Uygulamada oturum açtırmak için kimlik bilgilerinizi kullanmayı deneyin ve [bu kurumsal uygulamaya erişilemeyecek](application-proxy-sign-in-bad-gateway-timeout-error.md)gibi kullanıcıyla ilgili hataları kontrol edin. |
|2 | Uygulamanın kullanıcı atamasını doğrulama | Kullanıcı hesabınızın uygulamaya şirket ağı içinden erişme iznine sahip olduğundan emin olun ve [uygulamayı test](application-proxy-add-on-premises-application.md#test-the-application)etme adımlarını izleyerek uygulamaya giriş yapma testi yapın. Oturum açma sorunları devam ederse, [oturum açma hatalarını nasıl gidereceklerini](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)öğrenin.  |
|3 | Bir tarayıcı açın ve uygulamaya erişmeye çalışın | Hemen bir hata çıkarsa, Uygulama Proxy'nin doğru şekilde yapılandırıldığından denetleyin. Belirli hata iletileri hakkında ayrıntılı bilgi için, [Sorun Giderme Uygulaması Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)bakın.  |
|4 | Özel etki alanı kurulumunuzu kontrol edin veya hatayı giderin | Sayfa hiç görüntülenmiyorsa, [özel etki alanlarıyla Çalışma'yı](application-proxy-configure-custom-domain.md)gözden geçirerek özel etki alanınızın doğru şekilde yapılandırıldığından emin olun.<br></br>Sayfa yüklenmezse ve bir hata iletisi görüntülanırsa, Sorun [Giderme Uygulaması Proxy sorunlarına ve hata iletilerine](application-proxy-troubleshoot.md)atıfta bulunarak hatayı giderin. <br></br>Bir hata iletisi görünmesi 20 saniyeden uzun sürerse, bağlantı sorunu olabilir. [Hata Ayıklama Uygulama Proxy bağlayıcıları](application-proxy-debug-connectors.md) sorun giderme makalesine gidin.  |
|5 | Sorunlar devam ederse, bağlayıcı hata ayıklama | Proxy ve bağlayıcı arasında veya bağlayıcı ile arka uç arasında bir bağlantı sorunu olabilir. [Hata Ayıklama Uygulama Proxy bağlayıcıları](application-proxy-debug-connectors.md) sorun giderme makalesine gidin. |
|6 | Tüm kaynakları yayımlayın, tarayıcı geliştirici araçlarını denetleyin ve bağlantıları düzeltin | Yayımlama yolunun uygulamanız için gerekli tüm görüntüleri, komut dosyalarını ve stil sayfalarını içerdiğinden emin olun. Ayrıntılar için Azure [AD'ye şirket içi uygulama ekleme'ye](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad)bakın. <br></br>Tarayıcının geliştirici araçlarını (Internet Explorer veya Microsoft Edge'deki F12 araçları) kullanın ve Uygulama sayfasında açıklandığı gibi yayımlama sorunlarını [doğru görüntülemediğini](application-proxy-page-appearance-broken-problem.md)denetleyin. <br></br>Sayfadaki Bağlantılar'da bozuk bağlantıları çözme seçenekleri [çalışmaz.](application-proxy-page-links-broken-problem.md) |
|7 | Ağ gecikmesi olup yok olup yok olup yok | Sayfa yavaş yüklenirse, gecikme süresini azaltmak [için Göz önünde bulundurulması gereken](application-proxy-network-topology.md#considerations-for-reducing-latency)ağ gecikmesini en aza indirmenin yolları hakkında bilgi edinin. | 
|8 | Ek sorun giderme yardımına bakın | Sorunlar devam ederse, Uygulama Proxy [sorun giderme belgelerinde](application-proxy-troubleshoot.md)ek sorun giderme makaleleri bulabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar


* [Bağlayıcı gruplarını kullanarak uygulamaları ayrı ağlarda ve konumlarda yayımlama](application-proxy-connector-groups.md)
* [Varolan şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
* [Sorun Giderme Uygulama Proxy ve bağlayıcı hataları](application-proxy-troubleshoot.md)
* [Azure AD Uygulama Proxy Bağlayıcısı sessizce nasıl yüklenir?](application-proxy-register-connector-powershell.md)
