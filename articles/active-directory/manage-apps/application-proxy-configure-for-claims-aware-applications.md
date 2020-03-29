---
title: Taleplere duyarlı uygulamalar - Azure AD App Proxy | Microsoft Dokümanlar
description: Kullanıcılarınız tarafından güvenli uzaktan erişim için ADFS taleplerini kabul eden şirket içi ASP.NET uygulamaları nasıl yayınlanır?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: cbc5c356ea5a542fdc12b11aff236c56b146b3d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68477251"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Application Proxy'de hak iddialarla ilgili uygulamalarla çalışma
[Taleplere duyarlı uygulamalar,](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) Güvenlik Belirteci Hizmeti'ne (STS) yeniden yönlendirme gerçekleştirir. STS, bir belirteç karşılığında kullanıcıdan kimlik bilgilerini ister ve kullanıcıyı uygulamaya yönlendirir. Uygulama Proxy'nin bu yönlendirmelerle çalışmasını etkinleştirmenin birkaç yolu vardır. Taleplere duyarlı uygulamalar için dağıtımınızı yapılandırmak için bu makaleyi kullanın. 

## <a name="prerequisites"></a>Ön koşullar
Taleplere duyarlı uygulamanın yönlendirilen STS'nin şirket içi ağınızın dışında kullanılabildiğinden emin olun. STS'yi bir proxy aracılığıyla veya dış bağlantılara izin vererek kullanılabilir hale getirebilirsiniz. 

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

1. [Uygulama Proxy ile uygulamaları yayımla'da](application-proxy-add-on-premises-application.md)açıklanan talimatlara göre başvurunuzu yayınlayın.
2. Portaldaki uygulama sayfasına gidin ve **Tek oturum açma'yı**seçin.
3. **Ön Kimlik Doğrulama Yönteminiz**olarak Azure **Active Directory'yi** seçtiyseniz, **Dahili Kimlik Doğrulama Yönteminiz**olarak Azure AD tek oturum açma **devre dışı bırakıldığını** seçin. **Ön Authentication Yönteminiz**olarak **Passthrough'ı** seçtiyseniz, hiçbir şeyi değiştirmeniz gerekmez.

## <a name="configure-adfs"></a>ADFS'yi yapılandırma

ADFS'yi taleplere duyarlı uygulamalar için iki şekilde yapılandırabilirsiniz. İlközel etki alanları kullanarak. İkincisi WS-Federation ile. 

### <a name="option-1-custom-domains"></a>Seçenek 1: Özel etki alanları

Uygulamalarınız için tüm dahili URL'ler tam nitelikli alan adları (FQDNs) ise, uygulamalarınız için [özel etki alanlarını](application-proxy-configure-custom-domain.md) yapılandırabilirsiniz. Dahili URL'lerle aynı olan harici URL'ler oluşturmak için özel etki alanlarını kullanın. Harici URL'leriniz dahili URL'lerinizle eşleştiğinde, STS kullanıcılarınızın şirket içinde veya uzak olup olmadığı konusunda yeniden yönlendirmeler sağlar. 

### <a name="option-2-ws-federation"></a>Seçenek 2: WS-Federasyon

1. ADFS Yönetimini açın.
2. **Güvenen Parti Güvenleri'ne**gidin , Uygulama Proxy ile yayınladığınız uygulamaya sağ tıklayın ve **Özellikler'i**seçin.  

   ![Güvenen Parti Güvenleri Uygulama adına sağ tıklayın - ekran görüntüsü](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. Uç **Noktalar** sekmesinde, **Bitiş Noktası türü** **altında, WS-Federation'ı**seçin.
4. **Güvenilen URL**altında, **Dış URL** altında Uygulama Proxy'ye girdiğiniz URL'yi girin ve **Tamam'ı**tıklatın.  

   ![Endpoint ekleme - Güvenilir URL değerini ayarla - ekran görüntüsü](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Sonraki adımlar
* Taleplerden haberdar olmayan uygulamalar için tek oturum açma yı [etkinleştirme](configure-single-sign-on-non-gallery-applications.md)
* [Yerel istemci uygulamalarının proxy uygulamalarıyla etkileşimkurmasını etkinleştirme](application-proxy-configure-native-client-application.md)


