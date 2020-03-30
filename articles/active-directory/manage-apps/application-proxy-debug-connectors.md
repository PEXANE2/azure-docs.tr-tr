---
title: Hata Ayıklama Uygulama Proxy bağlayıcıları - Azure Active Directory | Microsoft Dokümanlar
description: Azure Etkin Dizin (Azure AD) Uygulama Proxy bağlayıcılarıyla hata ayıklama sorunları.
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
ms.openlocfilehash: c041578932bd33eb0a2d3afc18a35c2c0458dc8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72311854"
---
# <a name="debug-application-proxy-connector-issues"></a>Uygulama Ara Sunucusu bağlayıcı hatalarını ayıklama 

Bu makale, Azure Etkin Dizin (Azure AD) Uygulama Proxy bağlayıcıları ile sorunları gidermenize yardımcı olur. Uygulama Proxy hizmetini şirket içi bir web uygulamasına uzaktan erişim için kullanıyorsanız, ancak uygulamaya bağlanmada sorun yaşıyorsanız, bağlayıcıhata ayıklama sorunlarını ayıklamak için bu akış şemasını kullanın. 

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Uygulama Proxy bağlayıcısı yüklü ve bir sorun yaşıyorsanız varsayar. Uygulama Proxy sorunları giderme, Uygulama Proxy bağlayıcıları doğru yapılandırılmış olup olmadığını belirlemek için bu sorun giderme akışı ile başlamanızı öneririz. Uygulamaya bağlanmada sorun yaşıyorsanız, [Hata Ayıklama Uygulama Proxy uygulama sorunlarındaki](application-proxy-debug-apps.md)sorun giderme akışını izleyin.  


Uygulama Proxy ve bağlayıcılarını kullanma hakkında daha fazla bilgi için bkz:

- [Uygulama Proxy aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)
- [Uygulama Proxy konektörleri](application-proxy-connectors.md)
- [Konektör yükleme ve kaydetme](application-proxy-add-on-premises-application.md)
- [Sorun Giderme Uygulama Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)

## <a name="flowchart-for-connector-issues"></a>Konektör sorunları için akış şeması

Bu akış şeması, daha yaygın bağlayıcı sorunlarının bazılarını hata ayıklama adımlarından geçirmenize yol alar. Her adımla ilgili ayrıntılar için akış şemasının ardından tabloya bakın.

![Bağlayıcıhata ayıklama adımlarını gösteren akış şeması](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

|  | Eylem | Açıklama | 
|---------|---------|---------|
|1 | Uygulamaya atanan bağlayıcı grubunu bulma | Büyük olasılıkla birden çok sunucuda yüklü bir bağlayıcınız vardır, bu durumda bağlayıcılar [bağlayıcı gruplarına atanmalıdır.](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups) Bağlayıcı grupları hakkında daha fazla bilgi edinmek için [bkz.](application-proxy-connector-groups.md) |
|2 | Konektörü yükleyin ve bir grup atayın | Yüklü bir konektörüniz yoksa, [bir bağlayıcıyı yükle'ye bakın ve kaydedin.](application-proxy-add-on-premises-application.md#install-and-register-a-connector)<br></br> Bağlayıcıyı yüklerken sorun yaşıyorsanız, [bkz.](application-proxy-connector-installation-problem.md)<br></br> Bağlayıcı bir gruba atanmamışsa, [bağlayıcıyı bir gruba atay'a](application-proxy-connector-groups.md#create-connector-groups)bakın.<br></br>Uygulama bir bağlayıcı grubuna atanmamışsa, [bkz.](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups)|
|3 | Bağlayıcı sunucusunda bağlantı noktası testi çalıştırma | Bağlayıcı sunucusunda, 443 ve 80 bağlantı noktalarının açık olup olmadığını kontrol etmek için [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) veya başka bir bağlantı noktası test aracı kullanarak bir bağlantı noktası testi çalıştırın.|
|4 | Etki alanlarını ve bağlantı noktalarını yapılandırma | [Etki alanlarınızın ve bağlantı noktalarınızın doğru şekilde yapılandırıldığından emin olun](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) Bağlayıcının düzgün çalışması için, sunucunuzun erişebilmesi gereken açık olması gereken belirli bağlantı noktaları ve URL'ler vardır. |
|5 | Arka uç proxy'nin kullanılıp kullanılmamasını denet | Konektörlerin arka uç proxy sunucularını mı kullandığını yoksa atlayıp atlayıp atlayıp geçirmemelerini denetleyin. Ayrıntılar için, [Sorun Giderme bağlayıcısı proxy sorunları ve hizmet bağlantısı sorunları](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues)bakın. |
|6 | Arka uç proxy'sini kullanmak için bağlayıcıyı ve güncelleyiciyi güncelleştirin | Arka uç proxy kullanılıyorsa, bağlayıcının aynı proxy'yi kullandığından emin olmak istersiniz. Proxy sunucularıyla çalışacak şekilde sorun giderme ve konektörleri yapılandırma hakkında ayrıntılı bilgi için, [mevcut şirket içi proxy sunucularıyla Çalışma'ya](application-proxy-configure-connectors-with-proxy-servers.md)bakın. |
|7 | Uygulamanın dahili URL'sini bağlayıcı sunucusuna yükleme | Bağlayıcı sunucusunda, uygulamanın dahili URL'sini yükleyin. |
|8 | Dahili ağ bağlantısını denetleme | Dahili ağınızda, bu hata ayıklama akışının tanılayamadığı bir bağlantı sorunu var. Bağlayıcıların çalışması için uygulama dahili olarak erişilebilir olmalıdır. [Uygulama Proxy bağlayıcılarında](application-proxy-connectors.md#under-the-hood)açıklandığı gibi bağlayıcı olay günlüğünü etkinleştirebilir ve görüntüleyebilirsiniz. |
|9 | Arka uçtaki zaman sonu değerini uzatma | Uygulamanızın Ek Ayarları'nda, **Arka Uç Uygulama Zaman Sonu** ayarını **Uzun**olarak değiştirin. **Additional Settings** Bkz. [Azure AD'ye şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). |
|10 | Sorunlar devam ederse, belirli akış sorunlarını hedefleyin, uygulamayı ve SSO hata ayıklama akışlarını gözden geçirin | Hata [Ayıklama Uygulama Proxy uygulaması](application-proxy-debug-apps.md) sorun giderme akışını kullanın. |

## <a name="next-steps"></a>Sonraki adımlar


* [Bağlayıcı gruplarını kullanarak uygulamaları ayrı ağlarda ve konumlarda yayımlama](application-proxy-connector-groups.md)
* [Varolan şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
* [Sorun Giderme Uygulama Proxy ve bağlayıcı hataları](application-proxy-troubleshoot.md)
* [Azure AD Uygulama Proxy Bağlayıcısı sessizce nasıl yüklenir?](application-proxy-register-connector-powershell.md)
