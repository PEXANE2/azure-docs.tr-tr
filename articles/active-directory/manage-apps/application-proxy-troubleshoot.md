---
title: Sorun Giderme Uygulama Proxy | Microsoft Dokümanlar
description: Azure AD Application Proxy'deki hataların nasıl giderilenini kapsar.
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
ms.date: 06/24/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: H1Hack27Feb2017; it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7be9a17bed2a39d16f813332c2d6effc03393264
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244231"
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Sorun Giderme Uygulama Proxy sorunları ve hata iletileri

Uygulama Proxy sorunları giderme, uygulama proxy bağlayıcıları doğru yapılandırılmış olup olmadığını belirlemek için sorun giderme akışını gözden geçirme ile başlamanızı öneririz, [Hata Ayıklama Uygulama Proxy Bağlayıcı sorunları.](application-proxy-debug-connectors.md) Uygulamaya bağlanmada sorun yaşıyorsanız, [Hata Ayıklama Uygulama Proxy uygulama sorunlarındaki](application-proxy-debug-apps.md)sorun giderme akışını izleyin.

Yayımlanmış bir uygulamaya erişimi veya uygulamaları yayımlamada hatalar oluşuyorsa, Microsoft Azure AD Application Proxy'nin doğru çalışıp çalışmamasını görmek için aşağıdaki seçenekleri denetleyin:

* Windows Hizmetleri konsolunu açın. **Microsoft AAD Application Proxy Bağlayıcısı** hizmetinin etkin ve çalışır durumda olduğunu doğrulayın. Aşağıdaki resimde gösterildiği gibi, Uygulama Proxy hizmet özellikleri sayfasına da bakmak isteyebilirsiniz:  
  ![Microsoft AAD Application Proxy Connector Properties penceresi ekran görüntüsü](./media/application-proxy-troubleshoot/connectorproperties.png)
* Olay Görüntüleyici'yi açın ve **Uygulamalar ve Hizmetler Günlükleri** > **Microsoft** > **AadApplicationProxy** > **Bağlayıcı** > **Yöneticisi'nde**Uygulama Proxy bağlayıcısı olaylarını arayın.
* Gerekirse, [Uygulama Proxy bağlayıcı oturum günlükleri açarak](application-proxy-connectors.md#under-the-hood)daha ayrıntılı günlükleri kullanılabilir.

## <a name="the-page-is-not-rendered-correctly"></a>Sayfa doğru oluşturulmadı
Belirli hata iletileri almadan uygulamanızın işlenmesinde veya yanlış çalışmasında sorun yaşayabilirsiniz. Bu, makale yolunu yayımladıysanız oluşabilir, ancak uygulama bu yolun dışında var olan içeriği gerektirir.

Örneğin, yolu `https://yourapp/app` yayımlarsanız ancak uygulama görüntüleri `https://yourapp/media`çağırırsa, bunlar işlenmez. İlgili tüm içeriği eklemek için gereken en üst düzey yolu kullanarak uygulamayı yayımladığınızdan emin olun. Bu örnekte, bu `http://yourapp/`.

Başvurulan içeriği içerecek şekilde yolunuzu değiştirirseniz, ancak yine de kullanıcıların yoldaki daha derin bir bağlantıya inmelerine ihtiyaç duyarsanız, [Azure AD erişim panelinde ve Office 365 uygulama başlatıcısında Uygulama Proxy uygulamaları için doğru bağlantıyı ayarlayan](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/)blog gönderisine bakın.

## <a name="connector-errors"></a>Bağlayıcı hataları

Bağlayıcı sihirbaz yükleme sırasında kayıt başarısız olursa, hata nedenini görüntülemek için iki yolu vardır. **Uygulamalar ve Hizmetler Günlükleri\Microsoft\AadApplicationProxy\Connector\Admin**altında olay günlüğüne bakın veya aşağıdaki Windows PowerShell komutunu çalıştırın:

    Get-EventLog application –source "Microsoft AAD Application Proxy Connector" –EntryType "Error" –Newest 1

Olay günlüğünden Bağlayıcı hatasını bulduktan sonra, sorunu çözmek için bu yaygın hatalar tablosunu kullanın:

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| Bağlayıcı kaydı başarısız oldu: Azure Yönetim Portalı'nda Uygulama Proxy'sini etkinleştirdiğinizden ve Active Directory kullanıcı adınızı ve parolanızı doğru girdiğinizden emin olun. Hata: 'Bir veya daha fazla hata oluştu.' | Kayıt penceresini Azure AD'de oturum açmadan kapattıysanız, Bağlayıcı sihirbazını yeniden çalıştırın ve Bağlayıcıyı kaydedin. <br><br> Kayıt penceresi açılır ve oturum açmanıza izin vermeden hemen kapanırsa, büyük olasılıkla bu hatayı alırsınız. Bu hata, sisteminizde bir ağ hatası olduğunda oluşur. Bir tarayıcıdan genel bir web sitesine bağlanmanın mümkün olduğundan ve bağlantı noktalarının [Uygulama Proxy önkoşullarında](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)belirtildiği gibi açık olduğundan emin olun. |
| Açık hata kayıt penceresinde sunulur. Devam edemez | Bu hatayı görürseniz ve pencere kapanırsa, yanlış kullanıcı adı veya parola girersiniz. Yeniden Deneyin. |
| Bağlayıcı kaydı başarısız oldu: Azure Yönetim Portalı'nda Uygulama Proxy'sini etkinleştirdiğinizden ve Active Directory kullanıcı adınızı ve parolanızı doğru girdiğinizden emin olun. Hata: 'AADSTS50059: İstekte bulunan veya sağlanan kimlik bilgileri ve hizmet müdürü URI tarafından yapılan aramada kiracı tanımlayıcı hiçbir bilgi başarısız olmadı. | Erişmeye çalıştığınız dizinin kuruluş kimliğinin bir parçası olan bir etki alanı değil, bir Microsoft Hesabı kullanarak oturum açmaya çalışıyorsunuz. Yöneticinin kiracı etki alanıyla aynı etki alanı adının parçası olduğundan emin olun ( örneğin, Azure admin@contoso.comAD etki alanı contoso.com ise, yönetici . |
| PowerShell komut dosyalarını çalıştırmak için geçerli yürütme ilkesi ni almak için başarısız oldu. | Bağlayıcı yükleme başarısız olursa, PowerShell yürütme ilkesinin devre dışı bırakılmadığından emin olun. <br><br>1. Grup İlkesi Düzenleyicisini açın.<br>2. **Bilgisayar Yapılandırma** > **Yönetim Şablonları** > **Windows Components** > **Windows PowerShell** gidin ve **Komut Dosyası Yürütme aç'ı**çift tıklatın.<br>3. Yürütme **ilkesi, Yapılandırılmamış** veya **Etkinleştirilmiş**olarak ayarlanabilir. **Etkin**olarak ayarlanmışsa, Seçenekler altında Yürütme İlkesi'nin **yerel komut dosyalarına ve uzaktan imzalı komut dosyalarına izin** verecek veya tüm komut **dosyalarını İzin**verecek şekilde ayarlandıklarından emin olun. |
| Bağlayıcı yapılandırmayı karşıdan yükleyemedi. | Kimlik doğrulaması için kullanılan Bağlayıcı'nın istemci sertifikasının süresi doldu. Bağlayıcı bir proxy'nin arkasına yüklüyse bu da oluşabilir. Bu durumda, Bağlayıcı Internet'e erişemez ve uzak kullanıcılara uygulama sağlayamaz. Windows PowerShell'deki `Register-AppProxyConnector` cmdlet'i kullanarak güveni el ile yenileyin. Bağlayıcınız bir proxy'nin arkasındaysa, Bağlayıcı hesaplarına "ağ hizmetleri" ve "yerel sistem" internet erişimi vermek gerekir. Bu, proxy'ye erişim izni vererek veya proxy'yi atlayabilmeleri için ayarlayarak gerçekleştirilebilir. |
| Bağlayıcı kaydı başarısız oldu: Bağlayıcıyı kaydetmek için Aktif Dizininizin Uygulama Yöneticisi olduğunuzdan emin olun. Hata: 'Kayıt isteği reddedildi.' | Oturum açmaya çalıştığınız diğer ad bu etki alanında bir yönetici değildir. Bağlayıcınız her zaman kullanıcının etki alanına sahip dizin için yüklenir. Oturum açmaya çalıştığınız yönetici hesabının en azından uygulama yöneticisinin Azure AD kiracısına izin verdiğinden emin olun. |
| Bağlayıcı, ağ sorunları nedeniyle hizmete bağlanamadı. Bağlayıcı aşağıdaki URL'ye erişmeye çalıştı. | Bağlayıcı, Uygulama Proxy bulut hizmetine bağlanamıyor. Bağlantıyı engelleyen bir güvenlik duvarı kuralınız varsa bu durum oluşabilir. [Uygulama Proxy önkoşullarılistelenen](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)doğru bağlantı noktaları na ve URL'lere erişime izin verdiğinden emin olun. |

## <a name="kerberos-errors"></a>Kerberos hataları

Bu tablo, Kerberos kurulumu ve yapılandırmasından gelen daha yaygın hataları kapsar ve çözüm önerileri içerir.

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| PowerShell komut dosyalarını çalıştırmak için geçerli yürütme ilkesi ni almak için başarısız oldu. | Bağlayıcı yükleme başarısız olursa, PowerShell yürütme ilkesinin devre dışı bırakılmadığından emin olun.<br><br>1. Grup İlkesi Düzenleyicisini açın.<br>2. **Bilgisayar Yapılandırma** > **Yönetim Şablonları** > **Windows Components** > **Windows PowerShell** gidin ve **Komut Dosyası Yürütme aç'ı**çift tıklatın.<br>3. Yürütme **ilkesi, Yapılandırılmamış** veya **Etkinleştirilmiş**olarak ayarlanabilir. **Etkin**olarak ayarlanmışsa, Seçenekler altında Yürütme İlkesi'nin **yerel komut dosyalarına ve uzaktan imzalı komut dosyalarına izin** verecek veya tüm komut **dosyalarını İzin**verecek şekilde ayarlandıklarından emin olun. |
| 12008 - Azure AD, arka uç sunucusuna izin verilen maksimum Kerberos kimlik doğrulama denemesi sayısını aştı. | Bu hata, Azure AD ile arka uç uygulama sunucusu arasında yanlış yapılandırma veya her iki makinede de zaman ve tarih yapılandırmasında ki bir sorun gösterebilir. Arka uç sunucusu Azure AD tarafından oluşturulan Kerberos biletini reddetti. Azure AD ve arka uç uygulama sunucusunun doğru şekilde yapılandırıldığından doğrulayın. Azure AD ve arka uç uygulama sunucusundaki saat ve tarih yapılandırmasının eşitlenmiş olduğundan emin olun. |
| 13016 - Azure AD, kenar belirtecinde veya erişim çerezinde UPN bulunmadığından kullanıcı adına Kerberos bileti alamaz. | STS yapılandırmasıyla ilgili bir sorun var. STS'deki UPN talep yapılandırmasını düzeltin. |
| 13019 - Azure AD, aşağıdaki genel API hatası nedeniyle kullanıcı adına Kerberos bileti alamaz. | Bu olay, Azure AD ile etki alanı denetleyicisi sunucusu arasında yanlış yapılandırma veya her iki makinede de zaman ve tarih yapılandırmasında bir sorun gösterebilir. Etki alanı denetleyicisi, Azure AD tarafından oluşturulan Kerberos biletini reddetti. Azure AD ve arka uç uygulama sunucusunun özellikle SPN yapılandırması olmak üzere doğru şekilde yapılandırıldığıdoğrulayın. Etki alanı denetleyicisinin Azure AD ile güven oluşturduğundan emin olmak için Azure AD etki alanının etki alanıyla aynı etki alanına katıldığından emin olun. Azure AD ve etki alanı denetleyicisindeki saat ve tarih yapılandırmasının eşitlenmiş olduğundan emin olun. |
| 13020 - Arka uç sunucusu SPN tanımlı olmadığından Azure AD kullanıcı adına Kerberos bileti alamaz. | Bu olay, Azure AD ile etki alanı denetleyicisi sunucusu arasında yanlış yapılandırma veya her iki makinede de zaman ve tarih yapılandırmasında bir sorun gösterebilir. Etki alanı denetleyicisi, Azure AD tarafından oluşturulan Kerberos biletini reddetti. Azure AD ve arka uç uygulama sunucusunun özellikle SPN yapılandırması olmak üzere doğru şekilde yapılandırıldığıdoğrulayın. Etki alanı denetleyicisinin Azure AD ile güven oluşturduğundan emin olmak için Azure AD etki alanının etki alanıyla aynı etki alanına katıldığından emin olun. Azure AD ve etki alanı denetleyicisindeki saat ve tarih yapılandırmasının eşitlenmiş olduğundan emin olun. |
| 13022 - Arka uç sunucusu Kerberos kimlik doğrulama girişimlerine HTTP 401 hatasıyla yanıt verdiği için Azure AD kullanıcının kimliğini doğrulayamaz. | Bu olay, Azure AD ile arka uç uygulama sunucusu arasında yanlış yapılandırma veya her iki makinede de zaman ve tarih yapılandırmasında bir sorun olduğunu gösterebilir. Arka uç sunucusu Azure AD tarafından oluşturulan Kerberos biletini reddetti. Azure AD ve arka uç uygulama sunucusunun doğru şekilde yapılandırıldığından doğrulayın. Azure AD ve arka uç uygulama sunucusundaki saat ve tarih yapılandırmasının eşitlenmiş olduğundan emin olun. Daha fazla bilgi için, [Uygulama Proxy için Sorun Giderme Kerberos Kısıtlı Temsilci Yapılandırmaları](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)bakın.  |

## <a name="end-user-errors"></a>Son kullanıcı hataları

Bu liste, son kullanıcılarınızın uygulamaya erişip başarısız olduklarında karşılaşabilecekleri hataları kapsar. 

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| Web sitesi sayfayı görüntüleyemez. | Uygulama bir IWA uygulamasıysa, kullanıcınız yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. Bu uygulama için tanımlanan SPN yanlış olabilir. IWA uygulamaları için, bu uygulama için yapılandırılan SPN'nin doğru olduğundan emin olun. |
| Web sitesi sayfayı görüntüleyemez. | Uygulama bir OWA uygulamasıysa, kullanıcınız yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. Bu, aşağıdakilerden biri tarafından neden olabilir:<br><li>Bu uygulama için tanımlanan SPN yanlıştır. Bu uygulama için yapılandırılan SPN'nin doğru olduğundan emin olun.</li><li>Uygulamaya erişmeye çalışan kullanıcı oturum açmaiçin uygun şirket hesabı yerine bir Microsoft hesabı kullanıyor veya kullanıcı konuk kullanıcıdır. Kullanıcının, yayınlanan uygulamanın etki alanıyla eşleşen şirket hesabını kullanarak imzaladığından emin olun. Microsoft Hesabı kullanıcıları ve misafirleri IWA uygulamalarına erişemez.</li><li>Uygulamaya erişmeye çalışan kullanıcı, şirket içi tarafta bu uygulama için düzgün bir şekilde tanımlanmamıştır. Bu kullanıcının, şirket içi makinedeki bu arka uç uygulaması için tanımlandığı şekilde uygun izinlere sahip olduğundan emin olun. |
| Bu kurumsal uygulamaya erişilenemiyor. Bu uygulamaya erişme yetkiniz yok. Yetkilendirme başarısız oldu. Bu uygulamaya erişimi olan kullanıcıyı atadığından emin olun. | Kullanıcınız, oturum açmaları için şirket hesabı yerine Microsoft hesaplarını kullanıyorsa, yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. Konuk kullanıcılar da bu hatayı alabilir. Microsoft Hesabı kullanıcıları ve konukları IWA uygulamalarına erişemez. Kullanıcının, yayınlanan uygulamanın etki alanıyla eşleşen şirket hesabını kullanarak imzaladığından emin olun.<br><br>Bu uygulama için kullanıcıyı atamamış olabilirsiniz. **Uygulama** sekmesine gidin ve **Kullanıcılar ve Gruplar**altında bu kullanıcı yı veya kullanıcı grubunu bu uygulamaya atayın. |
| Bu kurumsal uygulamaya şu anda erişilenemiyor. Lütfen daha sonra tekrar deneyin ... Konektör zaman doldu. | Kullanıcınız, şirket içi tarafta bu uygulama için düzgün bir şekilde tanımlanmamışsa, yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. Kullanıcılarınızın şirket içi makinedeki bu arka uç uygulaması için tanımlandığı şekilde uygun izinlere sahip olduğundan emin olun. |
| Bu kurumsal uygulamaya erişilenemiyor. Bu uygulamaya erişme yetkiniz yok. Yetkilendirme başarısız oldu. Kullanıcının Azure Active Directory Premium lisansına sahip olduğundan emin olun. | Kullanıcınız, abonenin yöneticisi tarafından premium lisansla açıkça atanmamışsa, yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. Abonenin Active Directory **Lisanss** sekmesine gidin ve bu kullanıcıya veya kullanıcı grubuna Premium lisans atandığından emin olun. |
| Belirtilen ana bilgisayar ada sahip bir sunucu bulunamadı. | Uygulamanın özel etki alanı doğru yapılandırılmamışsa, kullanıcınız yayınladığınız uygulamaya erişmeye çalışırken bu hatayı alabilir. [Azure AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md) adımlarını izleyerek etki alanı için bir sertifika yüklediğinizden ve DNS kaydını doğru şekilde yapılandırdığınızdan emin olun |

## <a name="my-error-wasnt-listed-here"></a>Hatam burada listelenmedi

Azure AD Uygulama Proxy'sinde bu sorun giderme kılavuzunda listelenmemiş bir hata veya sorunla karşılaşırsanız, bu konuda bilgi almak isteriz. Karşılaştığınız hatanın ayrıntılarını içeren [geri bildirim ekibimize](mailto:aadapfeedback@microsoft.com) bir e-posta gönderin.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory için Uygulama Proxy'sini etkinleştirme](application-proxy-add-on-premises-application.md)
* [Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
* [Tek oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
* [Koşullu Erişimi Etkinleştir](application-proxy-integrate-with-sharepoint-server.md)


<!--Image references-->
[1]: ./media/application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
