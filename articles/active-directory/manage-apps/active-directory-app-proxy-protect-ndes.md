---
title: BIR NDES sunucusunda AD Application Proxy ile tümleştirme
titleSuffix: Azure Active Directory
description: NDES sunucunuzu korumak için Bir Azure Active Directory Application Proxy dağıtma kılavuzu.
services: active-directory
author: CelesteDG
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2020
ms.author: baselden
ms.reviewer: mimart
ms.openlocfilehash: 4ccd8834671725ace72497391090f81eb197ad6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77032263"
---
# <a name="integrate-with-azure-ad-application-proxy-on-a-network-device-enrollment-service-ndes-server"></a>Ağ Aygıtı Kayıt Hizmeti (NDES) sunucusunda Azure AD Application Proxy ile tümleştirme

Azure Active Directory (AD) Application Proxy, ağınızdaki uygulamaları yayımlamanızı sağlar. Bu uygulamalar SharePoint siteleri, Microsoft Outlook Web Uygulaması ve diğer web uygulamaları gibi uygulamalardır. Ayrıca Azure üzerinden ağınızın dışındaki kullanıcılara güvenli erişim sağlar.

Azure AD Application Proxy'de yeniyseniz ve daha fazla bilgi edinmek istiyorsanız, [Azure AD Application Proxy aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md)e bakın.

Azure AD Application Proxy, Azure'da yerleşiktir. Dağıtılmış hizmet reddi (DDOS) saldırılarına ve mükemmel kullanılabilirliğe karşı daha iyi koruma için büyük miktarda ağ bant genişliği ve sunucu altyapısı sağlar. Ayrıca, şirket içi ağınıza harici güvenlik duvarı bağlantı noktaları açmaya gerek yoktur ve DMZ sunucusu gerekmez. Tüm trafik gelen kaynaklanır. Giden bağlantı noktalarının tam listesi için [Bkz. Öğretici: Azure Etkin Dizini'nde Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#prepare-your-on-premises-environment)

> Azure AD Application Proxy, yalnızca Azure Active Directory'nin Premium veya Temel sürümlerini kullanıyorsanız kullanılabilen bir özelliktir. Daha fazla bilgi için [Azure Etkin Dizin fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)'na bakın. 
> Enterprise Mobility Suite (EMS) lisanslarınız varsa, bu çözümü kullanabilirsiniz.
> Azure AD Application Proxy bağlayıcısı yalnızca Windows Server 2012 R2 veya sonraki bir tarihte yüklenir. Bu da NDES sunucusunun bir gereksinimidir.

## <a name="install-and-register-the-connector-on-the-ndes-server"></a>Bağlayıcıyı NDES sunucusuna yükleyin ve kaydettirin

1. Uygulama Proxy'sini kullanan dizinin uygulama yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın. Örneğin, kiracı etki alanı contoso.com, yönetici admin@contoso.com veya bu etki alanında başka bir yönetici takma olmalıdır.
1. Sağ üst köşede kullanıcı adınızı seçin. Uygulama Proxy'sini kullanan bir dizinde oturum unuzu imzaladığınızı doğrulayın. Dizin değiştirmeniz gerekiyorsa, **Dizin Değiştir'i** seçin ve Uygulama Proxy'sini kullanan bir dizin seçin.
1. Sol gezinti panelinde **Azure Etkin Dizin'i**seçin.
1. **Yönet**altında, **Uygulama proxy'si**seçin.
1. **İndir bağlayıcı hizmetini**seçin.

    ![Hizmet Koşullarını görmek için konektör hizmetini indirin](./media/active-directory-app-proxy-protect-ndes/application-proxy-download-connector-service.png)

1. Hizmet Koşullarını okuyun. Hazır olduğunuzda, Kabul **koşullarını seçin & İndir.**
1. Azure AD Application Proxy bağlayıcısı kurulum dosyasını NDES sunucunuza kopyalayın. 
   > Des erişimi olan şirket ağınızdaki herhangi bir sunucuya bağlayıcıyı yükleyebilirsiniz. NDES sunucusuna yüklemeniz gerekmez.
1. *AADApplicationProxyConnectorInstaller.exe*gibi kurulum dosyasını çalıştırın. Yazılım lisans koşullarını kabul edin.
1. Yükleme sırasında, bağlayıcıyı Azure AD dizininizde Uygulama Proxy'sine kaydetmeniz istenir.
   * Azure AD dizininizde bir genel veya uygulama yöneticisinin kimlik bilgilerini sağlayın. Azure AD genel veya uygulama yöneticisi kimlik bilgileri, portaldaki Azure kimlik bilgilerinizden farklı olabilir.

        > [!NOTE]
        > Bağlayıcıyı kaydetmek için kullanılan genel veya uygulama yöneticisi hesabı, Uygulama Proxy hizmetini etkinleştirdiğiniz aynı dizine ait olmalıdır.
        >
        > Örneğin, Azure AD etki alanı *contoso.com*ise, genel/uygulama yöneticisi nin bu etki alanında geçerli bir diğer ad `admin@contoso.com` veya olması gerekir.

   * Bağlayıcıyı yüklediğiniz sunucu için Internet Explorer Gelişmiş Güvenlik Yapılandırması açıksa, kayıt ekranı engellenebilir. Erişime izin vermek için, hata iletisindeki yönergeleri izleyin veya yükleme işlemi sırasında Internet Explorer Gelişmiş Güvenliği kapatın.
   * Bağlayıcı kaydı başarısız olursa, [Sorun Giderme Uygulama](application-proxy-troubleshoot.md)Proxy'si'ne bakın.
1. Kurulumun sonunda, giden proxy'si olan ortamlar için bir not gösterilir. Azure AD Application Proxy bağlayıcısını giden proxy üzerinden çalışacak şekilde yapılandırmak için `C:\Program Files\Microsoft AAD App Proxy connector\ConfigureOutBoundProxy.ps1`sağlanan komut dosyasını çalıştırın.
1. Azure portalındaki Uygulama proxy sayfasında, yeni bağlayıcı aşağıdaki örnekte gösterildiği gibi *Etkin*durumuyla listelenir:

    ![Azure portalında etkin olarak gösterilen yeni Azure AD Uygulama Proxy bağlayıcısı](./media/active-directory-app-proxy-protect-ndes/connected-app-proxy.png)

    > [!NOTE]
    > Azure AD Application Proxy üzerinden kimlik doğrulaması yapılan uygulamalar için yüksek kullanılabilirlik sağlamak için, birden çok VM'ye bağlayıcı yükleyebilirsiniz. Azure AD DS yönetilen etki alanına katılan diğer sunuculara konektör yüklemek için önceki bölümde listelenen aynı adımları yineleyin.

1. Başarılı yüklemeden sonra Azure portalına geri dön.

1. **Kurumsal uygulamaları**seçin.

   ![doğru paydaşlarla etkileşimde bulunduğunuzdan emin olun](./media/active-directory-app-proxy-protect-ndes/azure-active-directory-enterprise-applications.png)

1. **+Yeni Uygulama'yı**seçin ve ardından **şirket içi uygulamayı**seçin. 

1. Kendi **şirket içi uygulamanızı**ekle'de, aşağıdaki alanları yapılandırın:

   * **Adı**: Uygulama için bir ad girin.
   * **Dahili Url**: Konektörü yüklediğiniz NDES sunucunuzun dahili URL'sini/FQDN'sini girin.
   * **Ön Kimlik Doğrulama**: **Geçiş'i**seçin. Herhangi bir ön kimlik doğrulama biçimi kullanmak mümkün değildir. Sertifika İstekleri (SCEP) için kullanılan protokol bu seçeneği sağlamaz.
   * Sağlanan **Harici URL'yi** panonuza kopyalayın.

1. Uygulamanızı kaydetmek için **+Ekle'yi** seçin.

1. 10. adımda kopyaladığınız bağlantıyı tarayıcıya yapıştırarak NDES sunucunuza Azure AD Uygulaması proxy'si üzerinden erişip erişemeyeceğiniz konusunda test edin. Varsayılan bir IIS karşılama sayfası görmeniz gerekir.

1. Son test olarak, önceki adımda yapıştırdığınız varolan URL'ye *mscep.dll* yolunu ekleyin:

   https://scep-test93635307549127448334.msappproxy.net/certsrv/mscep/mscep.dll

1. Bir HTTP **Hata 403** görmelisiniz - Yasak yanıt.

1. Sağlanan NDES URL'sini (Microsoft Intune aracılığıyla) aygıtlara değiştirin, bu değişiklik Microsoft Endpoint Configuration Center'da veya Intune Cloud'da olabilir.

   * Yapılandırma Merkezi için Sertifika Kayıt Noktası'na (CRP) gidin ve URL'yi ayarlayın. Bu URL, aygıtların meydan okumalarına çağrıda bulunduğu ve sundukları URL'dir.
   * Yalnızca Intune Cloud Alone olarak da bilinen Intune Cloud için, yeni bir SCEP ilkesini edin veya oluşturun ve yeni URL ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

NDES ile tümleşik Azure AD Uygulama Proxy ile kullanıcıların erişebilmek için uygulamalar yayımlayın. Daha fazla bilgi için Azure [AD Application Proxy kullanarak uygulama yayımlama'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)bakın.
