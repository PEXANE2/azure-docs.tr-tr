---
title: App Proxy uygulaması ile bu Kurumsal Uygulama hatasına erişemiyorum
description: Azure AD Application Proxy uygulamalarıyla ilgili yaygın erişim sorunlarını çözme.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d61f4b4bce9b8287dc13237f071684ea5d135fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275463"
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Bir Uygulama Proxy uygulaması kullanırken "Bu Kurumsal Uygulamaya Erişemiyorum" hatası

Bu makale, Azure AD Uygulama Proxy uygulamasındaki "Bu kurumsal uygulamaya erişilemez" hatasıyla ilgili sık karşılaşılan sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış

Bu hatayı gördüğünüzde, hata sayfasındaki durum kodunu bulun. Bu kod büyük olasılıkla aşağıdaki durum kodlarından biridir:

- **Ağ Geçidi Zaman Süresi**: Uygulama Proxy hizmeti bağlayıcıya ulaşamıyor. Bu hata genellikle bağlayıcı atama, bağlayıcının kendisi veya bağlayıcının etrafındaki ağ kurallarıyla ilgili bir sorunu gösterir.
- **Kötü Ağ Geçidi**: Bağlayıcı arka uç uygulamasına erişemiyor. Bu hata, uygulamanın yanlış bir yapılandırmasını gösterebilir.
- **Yasak**: Kullanıcının uygulamaya erişme yetkisi yoktur. Bu hata, kullanıcı Azure Etkin Dizin'de uygulamaya atanmamışsa veya arka uçta kullanıcının uygulamaya erişim izni yoksa meydana gelebilir.

Kodu bulmak için, "Durum Kodu" alanı için hata iletisinin sol alt kısmındaki metne bakın. Ayrıca sayfanın altındaki ek ipuçlarıarayın.

![Örnek: Ağ geçidi zaman alamı hatası](./media/application-proxy-sign-in-bad-gateway-timeout-error/connection-problem.png)

Bu hataların temel nedenini nasıl gidereceğimiz ve önerilen düzeltmeler hakkında daha fazla ayrıntı hakkında ayrıntılı bilgi için aşağıdaki ilgili bölüme bakın.

## <a name="gateway-timeout-errors"></a>Ağ Geçidi Zaman Alamı hataları

Hizmet bağlayıcıya ulaşmaya çalıştığında ve zaman dışında zaman dışında bir zaman alamadığında bir ağ geçidi zaman acısı oluşur. Bu hata genellikle çalışan bağlayıcısı olmayan bir Bağlayıcı Grubu'na atanan bir uygulamadan kaynaklanır veya Bağlayıcı tarafından gerekli olan bazı bağlantı noktaları açık değildir.

## <a name="bad-gateway-errors"></a>Hatalı Ağ Geçidi hataları

Hatalı ağ geçidi hatası, bağlayıcının arka uç uygulamasına erişemediğini gösterir. doğru uygulamayı yayımladığınızdan emin olun. Bu hataya neden olan yaygın hatalar şunlardır:

- İç URL'de yazım hatası veya hata
- Uygulamanın kökünü yayımlamamak. Örneğin, yayımlama <http://expenses/reimbursement> ama erişmeye çalışıyor<http://expenses>
- Kerberos Kısıtlı Delegasyonu (KCD) yapılandırmasındaki sorunlar
- Arka uç uygulamasıyla ilgili sorunlar

## <a name="forbidden-errors"></a>Yasak hatalar

Yasak bir hata görürseniz, kullanıcı uygulamaya atanmamış olur. Bu hata Azure Etkin Dizin'de veya arka uç uygulamasında olabilir.

Azure'da uygulamaya kullanıcıları nasıl ataygöreceğinizi öğrenmek için [yapılandırma belgelerine](application-proxy-add-on-premises-application.md#test-the-application)bakın.

Kullanıcının Azure'da uygulamaya atandığını onaylarsanız, arka uç uygulamasındaki kullanıcı yapılandırmasını denetleyin. Kerberos Kısıtlı Temsilciliği/Tümleşik Windows Kimlik Doğrulaması kullanıyorsanız, yönergeler için KCD Sorun Giderme sayfasına bakın.

## <a name="check-the-applications-internal-url"></a>Uygulamanın dahili URL'sini kontrol edin

İlk hızlı adım olarak, **uygulamayı Kurumsal Uygulamalar**aracılığıyla açarak dahili URL'yi iki kez kontrol edin ve düzeltin, ardından Application **Proxy** menüsünü seçin. Dahili URL'nin, uygulamayla erişmek için şirket içi ağınızda kullanılan URL olduğunu doğrulayın.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Uygulamanın çalışan bir Bağlayıcı Grubuna atandığını kontrol edin

Uygulamanın çalışan bir Bağlayıcı Grubuna atandığını doğrulamak için:

1. **Azure Active Directory'ye**giderek uygulamayı portalda açın , **Kurumsal Uygulamalar'a**tıklayarak, ardından **Tüm Uygulamalar'a tıklayın.** Uygulamayı açın ve sol menüden **Uygulama Proxy'sini** seçin.
1. Bağlayıcı Grup alanına bakın. Grupta etkin bağlayıcı yoksa, bir uyarı görürsünüz. Herhangi bir uyarı görmüyorsanız, gerekli tüm bağlantı noktalarına izin verildiğini doğrulamak için devam edin.
1. Yanlış Bağlayıcı Grubu görünüyorsa, doğru grubu seçmek ve artık herhangi bir uyarı görmediğinizi onaylamak için açılır dosyayı kullanın. Amaçlanan Bağlayıcı Grubu gösteriliyorsa, Bağlayıcı yönetimi yle sayfayı açmak için uyarı iletisini tıklatın.
1. Buradan, daha fazla sondaj için birkaç yolu vardır:

   - Etkin bir Bağlayıcıyı gruba taşıyın: Bu gruba ait olması gereken etkin bir Bağlayıcınız varsa ve hedef arka uç uygulamasına görüş hattı varsa, Bağlayıcıyı atanan gruba taşıyabilirsiniz. Bunu yapmak için Bağlayıcı'yı tıklatın. "Bağlayıcı Grubu" alanında, doğru grubu seçmek için açılır dosyayı kullanın ve kaydet'i tıklatın.
   - Bu grup için yeni bir Bağlayıcı indirin: Bu [sayfadan, yeni bir Bağlayıcı indirmek](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download)için bağlantı alabilirsiniz. Konektörü arka uç uygulamasına doğrudan görüş hattı olan bir makineye takın. Genellikle, Bağlayıcı uygulamayla aynı sunucuya yüklenir. Hedef makineye bir bağlayıcı indirmek için download Connector bağlantısını kullanın. Ardından, Bağlayıcı'yı tıklatın ve doğru gruba ait olduğundan emin olmak için "Bağlayıcı Grubu" açılır masını kullanın.
   - Etkin olmayan bir Bağlayıcıyı araştırın: Bir bağlayıcı etkin değil olarak gösterirse, hizmete erişemez. Bu hata genellikle bazı gerekli bağlantı noktalarının engellenmesi nedeniyledir. Bu sorunu çözmek için, gerekli tüm bağlantı noktalarına izin verildiğini doğrulamak için devam edin.

Uygulamanın çalışan Bağlayıcılara sahip bir gruba atanmasını sağlamak için bu adımları kullandıktan sonra, uygulamayı yeniden sınatın. Hala çalışmıyorsa, sonraki bölüme devam edin.

## <a name="check-all-required-ports-are-open"></a>Gerekli tüm bağlantı noktalarının açık olup olduğunu kontrol edin

Gerekli tüm bağlantı noktalarının açık olduğunu doğrulamak için bağlantı noktalarını açma yla ilgili belgelere bakın. Gerekli tüm bağlantı noktaları açıksa, bir sonraki bölüme geçin.

## <a name="check-for-other-connector-errors"></a>Diğer Bağlayıcı Hatalarını Denetle

Yukarıdakilerden hiçbiri sorunu çözmezse, bir sonraki adım Bağlayıcının kendisiyle ilgili sorunları veya hataları aramaktır. [Sorun Giderme belgesinde](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors)bazı sık karşılaşılan hataları görebilirsiniz.

Ayrıca, hataları tanımlamak için doğrudan Bağlayıcı günlüklerine bakabilirsiniz. Hata iletilerinin çoğu düzeltmeler için belirli önerileri paylaşır. Günlükleri görüntülemek için [bağlayıcılar belgelerine](application-proxy-connectors.md#under-the-hood)bakın.

## <a name="additional-resolutions"></a>Ek Kararlar

Yukarıdaki sorunu gidermek vermedi, birkaç farklı olası nedenleri vardır. Sorunu tanımlamak için:

Uygulamanız Tümleşik Windows Kimlik Doğrulaması (IWA) kullanacak şekilde yapılandırıldıysa, uygulamayı tek bir oturum açmadan sınayın. Değilse, bir sonraki paragrafa geçin. Tek oturum açmadan uygulamayı kontrol etmek **için, Kurumsal Uygulamalar** aracılığıyla başvurunuzu açın ve **Tek Oturum Açma** menüsüne gidin. Açılan pencereyi "Tümleşik Windows Kimlik Doğrulaması"ndan "Azure AD tek oturum açma devre dışı" olarak değiştirin.

Şimdi bir tarayıcı açın ve uygulamaya yeniden erişmeye çalışın. Kimlik doğrulamaiçin istenmeli ve uygulamaya girmelisiniz. Eğer kimlik doğrulaması yapabiliyorsanız, sorun tek oturum açmayı sağlayan Kerberos Kısıtlı Delegasyonu (KCD) yapılandırmasındadır. Daha fazla bilgi için KCD Sorun Giderme sayfasına bakın.

Hatayı görmeye devam ederseniz, Bağlayıcı'nın yüklü olduğu makineye gidin, bir tarayıcı açın ve uygulama için kullanılan dahili URL'ye erişmeye çalışın. Bağlayıcı aynı makineden başka bir istemci gibi davranır. Uygulamaya erişemiyorsanız, bu makinenin uygulamaya neden erişemediğini araştırın veya uygulamaya erişebilen bir sunucuda bağlayıcı kullanın.

Bu makineden uygulamaya ulaşabiliyorsanız, Bağlayıcı'nın kendisiyle ilgili sorunları veya hataları aramak için. [Sorun Giderme belgesinde](application-proxy-troubleshoot.md#connector-errors)bazı sık karşılaşılan hataları görebilirsiniz. Ayrıca, hataları tanımlamak için doğrudan Bağlayıcı günlüklerine bakabilirsiniz. Hata iletilerimizin çoğu düzeltmeler için daha özel öneriler paylaşabilir. Günlükleri nasıl görüntüleyin, [bağlayıcı belgelerimize](application-proxy-connectors.md#under-the-hood)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
