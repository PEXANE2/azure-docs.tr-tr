---
title: Bağlantıları ve URL'leri Azure AD App Proxy'ye çevirin | Microsoft Dokümanlar
description: Azure AD Application Proxy bağlayıcıları hakkındaki temel bilgileri kapsar.
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
ms.date: 08/15/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa0dc2081aff5a24fb830b756131cccd5c6ce810
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69533689"
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Azure AD Application Proxy ile yayınlanan uygulamalar için kodlanmış bağlantıları yeniden yönlendirme

Azure AD Application Proxy, şirket içi uygulamalarınızı uzak veya kendi aygıtlarında olan kullanıcıların kullanımına sunar. Ancak bazı uygulamalar, HTML'ye gömülü yerel bağlantılarla geliştirilmiştir. Uygulama uzaktan kullanıldığında bu bağlantılar doğru çalışmaz. Birkaç şirket içi uygulama birbirini işaret ettiğinde, kullanıcılarınız bağlantıların ofiste değilken çalışmaya devam etmesini bekler. 

Bağlantıların hem şirket ağınızın içinde hem de dışında aynı şekilde çalışmasını sağlamanın en iyi yolu, uygulamalarınızın harici URL'lerini dahili URL'leriyle aynı olacak şekilde yapılandırmaktır. Dış URL'lerinizi varsayılan uygulama proxy etki alanı yerine şirket etki alanı adınızı olacak şekilde yapılandırmak için [özel etki alanlarını](application-proxy-configure-custom-domain.md) kullanın.


Kiracınızda özel etki alanlarını kullanamıyorsanız, bu işlevselliği sağlamak için başka seçenekler de vardır. Bunların tümü de özel etki alanları ve birbirleri ile uyumludur, böylece gerekirse özel etki alanlarını ve diğer çözümleri yapılandırabilirsiniz.

> [!NOTE]
> Bağlantı çevirisi Javascript üzerinden oluşturulan sabit kodlu dahili URL'ler için desteklenmez.

**Seçenek 1: Yönetilen Tarayıcıyı veya Microsoft Edge'i kullanın** - Bu çözüm yalnızca kullanıcıların Uygulamaya Intune Yönetilen Tarayıcı veya Microsoft Edge Tarayıcısı aracılığıyla erişmelerini önermeyi veya gerektirmeyi planlıyorsanız uygulanabilir. Yayınlanan tüm URL'leri işleyecek. 

**Seçenek 2: MyApps Uzantısını Kullanın** – Bu çözüm, kullanıcıların istemci tarafındaki bir tarayıcı uzantısı yüklemesini gerektirir, ancak tüm yayınlanmış URL'leri işler ve en popüler tarayıcılarla çalışır. 

**Seçenek 3: Bağlantı çeviri ayarını kullanın** – Bu, kullanıcılar tarafından görülemeyen bir yönetici yan ayarıdır. Ancak, yalnızca HTML ve CSS URL'leri işleyecek.   

Bu üç özellik, kullanıcılarınız nerede olursa olsun bağlantılarınızın çalışmasını sağlar. Doğrudan dahili uç noktaları veya bağlantı noktalarını gösteren uygulamalarınız olduğunda, bu dahili URL'leri yayımlanmış dış Uygulama Proxy URL'lerine eşleyebilirsiniz. 

 
> [!NOTE]
> Son seçenek, yalnızca, herhangi bir nedenle, uygulamaları için aynı dahili ve harici URL'lere sahip olmak için özel etki alanlarını kullanabilen kiracılar içindir. Bu özelliği etkinleştirmeden önce, [Azure AD Application Proxy'deki özel etki alanlarının](application-proxy-configure-custom-domain.md) sizin için çalışıp çalışmayada olmadığını görün. 
> 
> Veya bağlantı çevirisiyle yapılandırmanız gereken uygulama SharePoint ise, bağlantı eşleme bağlantılarına başka bir yaklaşım için [SharePoint 2013 için alternatif erişim eşlemelerini yapılandır'a](https://technet.microsoft.com/library/cc263208.aspx) bakın. 

 
### <a name="option-1-intune-managed-browser-and-microsoft-edge-integration"></a>Seçenek 1: Intune Yönetilen Tarayıcı ve Microsoft Edge Tümleştirmesi 

Uygulamanızı ve içeriğinizi daha fazla korumak için Intune Yönetilen Tarayıcı'yı veya Microsoft Edge'i kullanabilirsiniz. Bu çözümü kullanmak için, kullanıcıların Uygulamaya Intune Yönetilen Tarayıcı üzerinden erişmelerini gerektirmeniz/önermeniz gerekir. Application Proxy ile yayınlanan tüm dahili URL'ler Yönetilen Tarayıcı tarafından tanınacak ve ilgili dış URL'ye yönlendirilir. Bu, tüm sabit kodlanmış dahili URL'lerin çalışmasını sağlar ve bir kullanıcı tarayıcıya giderse ve doğrudan dahili URL'yi işlerse, kullanıcı uzak olsa bile çalışır.  

Bu seçeneği nasıl yapılandırılabildiğiniz de dahil olmak üzere daha fazla bilgi edinmek için lütfen [Yönetilen Tarayıcı](https://docs.microsoft.com/intune/app-configuration-managed-browser) belgelerine bakın.  

### <a name="option-2-myapps-browser-extension"></a>Seçenek 2: MyApps Tarayıcı Uzantısı 

MyApps Tarayıcı Uzantısı ile, Application Proxy ile yayınlanan tüm dahili URL'ler uzantı tarafından tanınır ve ilgili dış URL'ye yönlendirilir. Bu, tüm sabit kodlanmış dahili URL'lerin çalışmasını sağlar ve bir kullanıcı tarayıcının adres çubuğuna giderse ve dahili URL'yi doğrudan işlerse, kullanıcı uzak olsa bile çalışır.  

Bu özelliği kullanmak için, kullanıcının uzantıyı karşıdan yüklemesi ve oturum açması gerekir. Yöneticiler veya kullanıcılar için gereken başka bir yapılandırma yoktur. 

Bu seçeneği nasıl yapılandırılabildiğiniz de dahil olmak üzere daha fazla bilgi edinmek için lütfen [MyApps Tarayıcı Uzantısı](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access#download-and-install-the-my-apps-secure-sign-in-extension) belgelerine bakın.

### <a name="option-3-link-translation-setting"></a>Seçenek 3: Bağlantı Çeviri Ayarı 

Bağlantı çevirisi etkinleştirildiğinde, Uygulama Proxy hizmeti yayınlanan dahili bağlantılar için HTML ve CSS üzerinden arama yaparken, kullanıcılarınızın kesintisiz bir deneyim yaşamasını sağlar. MyApps Tarayıcı Uzantısı'nı kullanmak, kullanıcılara daha performant bir deneyim sağladığından Bağlantı Çeviri Ayarı'na tercih edilir.

> [!NOTE]
> Seçenek 2 veya 3 kullanıyorsanız, bunlardan yalnızca biri aynı anda etkinleştirilmelidir.

## <a name="how-link-translation-works"></a>Bağlantı çevirisi nasıl çalışır?

Kimlik doğrulamadan sonra, proxy sunucusu uygulama verilerini kullanıcıya aktardığında, Application Proxy uygulamayı sabit kodlu bağlantılar için tarar ve bunları ilgili, yayınlanmış harici URL'leri ile değiştirir.

Application Proxy, uygulamaların UTF-8'de kodlanmış olduğunu varsayar. Bu durumda değilse, http yanıt üstbilgisinde kodlama türünü belirtin, gibi `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Hangi bağlantılar etkilenir?

Bağlantı çevirisi özelliği yalnızca bir uygulamanın gövdesinde kod etiketlerinde bulunan bağlantıları arar. Uygulama Proxy'nin üstbilgideki tanımlama bilgilerini veya URL'leri çevirmek için ayrı bir özelliği vardır. 

Şirket içi uygulamalarda iki yaygın dahili bağlantı türü vardır:

- Gibi `/claims/claims.html`yerel bir dosya yapısında paylaşılan bir kaynağa işaret **eden göreli iç bağlantılar** Bu bağlantılar, Application Proxy aracılığıyla yayınlanan uygulamalarda otomatik olarak çalışır ve bağlantı çevirisi yle veya bağlantı çevirisi olmadan çalışmaya devam eder. 
- Gibi diğer şirket içi uygulamalara sabit `http://expenses` **kodlanmış dahili bağlantılar.** `http://expenses/logo.jpg` Bağlantı çevirisi özelliği, kodlanmış dahili bağlantılar üzerinde çalışır ve bunları uzak kullanıcıların geçmesi gereken dış URL'lere işaret etmek üzere değiştirir.

Application Proxy'nin bağlantı çevirisini desteklediği HTML kod etiketlerinin tam listesi şunlardır:
* a
* ses
* base
* düğmesini seçin
* div
* Katıştır
* Form
* çerçeve
* Kafa
* html
* iframe
* görüntü
* giriş
* bağlantı
* Menuıtem
* Meta
* object
* betiğini çalıştırın
* source
* izle
* video

Ayrıca, CSS içinde URL özniteliği de tercüme edilir.

### <a name="how-do-apps-link-to-each-other"></a>Uygulamalar birbirine nasıl bağlanır?

Bağlantı çevirisi her uygulama için etkinleştirilir, böylece uygulama başına kullanıcı deneyimi üzerinde denetim elabilirsiniz. Bir uygulamanın bağlantı çevirisini, o uygulamadaki bağlantıların değil, *to* o *uygulamanın* bağlantılarının çevrilmesini istediğinizde açın. 

Örneğin, Uygulama Proxy aracılığıyla yayınlanan ve hepsi birbirine bağlanan üç uygulamanız olduğunu varsayalım: Avantajlar, Giderler ve Seyahat. Uygulama Proxy aracılığıyla yayınlanmayan dördüncü bir uygulama var, Geribildirim.

Avantajlar uygulamasının bağlantı çevirisini etkinleştirdiğinizde, Giderler ve Seyahat bağlantıları bu uygulamaların harici URL'lerine yönlendirilir, ancak harici URL olmadığı için Geri Bildirim bağlantısı yönlendirilmez. Bu iki uygulama için bağlantı çevirisi etkinleştirilemediği için Giderler ve Avantajlara Geri Seyahat'ten bağlantılar çalışmaz.

![Bağlantı çevirisi etkinleştirildiğinde Avantajlardan diğer uygulamalara bağlantılar](./media/application-proxy-configure-hard-coded-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Hangi bağlantılar çevrilmiyor?

Performansı ve güvenliği artırmak için bazı bağlantılar çevrilmemiştir:

- Kod etiketlerinin içindeki bağlantılar. 
- HTML veya CSS'de olmayan bağlantılar. 
- URL kodlu biçimdeki bağlantılar.
- Diğer programlardan açılan iç bağlantılar. E-posta veya anlık ileti yoluyla gönderilen veya diğer belgelere eklenen bağlantılar çevrilmez. Kullanıcıların harici URL'ye gitmelerini bilmeleri gerekir.

Bu iki senaryodan birini desteklemeniz gerekiyorsa, bağlantı çevirisi yerine aynı iç ve dış URL'leri kullanın.  

## <a name="enable-link-translation"></a>Bağlantı çevirisini etkinleştir

Bağlantı çevirisi ile başlamak bir düğmeye tıklamak kadar kolaydır:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Azure **Active Directory** > **Enterprise uygulamalarına** > gidin**Tüm uygulamalar** > yönetmek istediğiniz uygulamayı seçin > Uygulama proxy'si. **Application proxy**
3. **Uygulama gövdesindeki URL'leri** **Evet'e**çevirin.

   ![URL'leri uygulama gövdesinde çevirmek için Evet'i seçin](./media/application-proxy-configure-hard-coded-link-translation/select_yes.png)
4. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

Şimdi, kullanıcılarınız bu uygulamaya eriştiklerinde, proxy otomatik olarak kiracınızda Application Proxy aracılığıyla yayınlanmış dahili URL'leri tarar.

## <a name="send-feedback"></a>Geri bildirim gönder

Bu özelliğin tüm uygulamalarınız için çalışması için yardımınızı istiyoruz. HTML ve CSS'de 30'dan fazla etiket tesbit ediyoruz. Çevrilmeyen oluşturulan bağlantılara bir örnek varsa, [Uygulama Proxy Geri Bildirimi'ne](mailto:aadapfeedback@microsoft.com)bir kod parçacığı gönderin. 

## <a name="next-steps"></a>Sonraki adımlar
Aynı dahili ve harici URL'ye sahip olmak için [Azure AD Application Proxy ile özel etki alanlarını kullanma](application-proxy-configure-custom-domain.md)

[SharePoint 2013 için alternatif erişim eşlemelerini yapılandırma](https://technet.microsoft.com/library/cc263208.aspx)
