---
title: Galeri olmayan uygulamalar için parola SSO yapılandırma sorunları
description: Azure AD uygulama galerisinde olmayan özel uygulamalar için parola çoklu oturum açma (SSO) yapılandırdığınızda oluşan yaygın sorunlar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed8bafe7f5bc28cf37205107f8ab6dd5cdb4907c
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74274142"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola çoklu oturum açmayı yapılandırma sorunları

Bu makalede, Galeri dışı bir uygulama için *parola çoklu oturum açma* (SSO) yapılandırdığınızda oluşabilecek yaygın sorunlar açıklanmaktadır.

## <a name="capture-sign-in-fields-for-an-app"></a>Uygulama için oturum açma alanlarını yakalama

Oturum açma alanı yakalama yalnızca HTML özellikli oturum açma sayfaları için desteklenir. Standart olmayan oturum açma sayfalarında, Adobe Flash veya HTML olmayan diğer teknolojileri kullananlar gibi desteklenmez.

Özel uygulamalarınız için oturum açma alanlarını yakalamanın iki yolu vardır:

- **Otomatik oturum açma alanı yakalama** , Kullanıcı adı ve parola alanları için *Iyi bilinen DIV kimliklerini kullanıyorsa* , en fazla HTML etkin oturum açma sayfası ile iyi çalışacaktır. Sayfadaki HTML, belirli ölçütlerle eşleşen DIV kimliklerini bulmak için korlanır. Bu meta veriler, daha sonra uygulamaya yeniden oynatılabilecek şekilde kaydedilir.

- Uygulama satıcısı *oturum açma giriş alanlarını etiketleyip*, **el ile oturum açma alanı yakalama** kullanılır. Satıcı *otomatik olarak algılanamayan birden çok alan işliyorsa*el ile yakalama da kullanılır. Azure Active Directory (Azure AD), oturum açma sayfasında, bu alanların sayfada olduğu yerde olduğunu söylemiş kadar çok sayıda alan için veri depolayabilirler.

Genel olarak, otomatik oturum açma alanı yakalama işe yaramazsa, el ile seçeneğini deneyin.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını otomatik olarak yakala

Parola tabanlı SSO 'yu otomatik oturum açma alanı yakalamayı kullanarak yapılandırmak için şu adımları izleyin:

1. [Azure portalını](https://portal.azure.com/) açın. Genel yönetici veya ortak yönetici olarak oturum açın.

2. Sol taraftaki Gezinti bölmesinde, **tüm hizmetler** ' i seçerek Azure ad uzantısını açın.

3. Filtre arama kutusuna **Azure Active Directory** yazın ve **Azure Active Directory**' ı seçin.

4. Azure AD gezinti bölmesinde **Kurumsal uygulamalar** ' ı seçin.

5. Uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar** listesinin en üstündeki **filtre** denetimini kullanın. **Göster** seçeneğini "tüm uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, sol taraftaki Gezinti bölmesinde **Çoklu oturum açma** seçeneğini belirleyin.

8. **Parola tabanlı oturum açma** modunu seçin.

9. Kullanıcıların oturum açmak için Kullanıcı adını ve parolasını girmesi gereken sayfanın URL 'SI olan **oturum açma URL**'sini girin. *SAĞLADıĞıNıZ URL 'nin sayfada oturum açma alanlarının göründüğünden emin olun*.

10. **Kaydet**’i seçin.

    Sayfa, Kullanıcı adı ve parola giriş kutuları için otomatik olarak indirgendi. Artık erişim paneli tarayıcı uzantısını kullanarak bu uygulamaya parolaları güvenli bir şekilde aktarmak için Azure AD 'yi kullanabilirsiniz.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını el ile yakalama

Oturum açma alanlarını el ile yakalamak için, erişim paneli tarayıcı uzantısının yüklü olması gerekir. Ayrıca, tarayıcınız *InPrivate*, *ınbilito*veya *özel* modda çalıştırılabilir.

Uzantıyı yüklemek için, bu makalenin [erişim paneli tarayıcı uzantısını yüklemek](#install-the-access-panel-browser-extension) bölümüne bakın.

El ile oturum açma alanı yakalamayı kullanarak bir uygulama için parola tabanlı SSO yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure portalını](https://portal.azure.com/) açın. Genel yönetici veya ortak yönetici olarak oturum açın.

2. Sol taraftaki Gezinti bölmesinde, **tüm hizmetler** ' i seçerek Azure ad uzantısını açın.

3. Filtre arama kutusuna **Azure Active Directory** yazın ve **Azure Active Directory**' ı seçin.

4. Azure AD gezinti bölmesinde **Kurumsal uygulamalar** ' ı seçin.

5. Uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

   > [!NOTE] 
   > İstediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar** listesinin en üstündeki **filtre** denetimini kullanın. **Göster** seçeneğini "tüm uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, sol taraftaki Gezinti bölmesinde **Çoklu oturum açma** seçeneğini belirleyin.

8. **Parola tabanlı oturum açma** modunu seçin.

9. Kullanıcıların oturum açmak için Kullanıcı adını ve parolasını girmesi gereken sayfa olan **oturum açma URL 'sini**girin. *SAĞLADıĞıNıZ URL 'nin sayfada oturum açma alanlarının göründüğünden emin olun*.

10. ***&lt;AppName&gt;* Password çoklu oturum açma ayarlarını yapılandır '** ı seçin.

11. **Oturum açma alanlarını el ile Algıla**' yı seçin.

14. **Tamam**’ı seçin.

15. **Kaydet**’i seçin.

16. Erişim panelini kullanmak için yönergeleri izleyin.

## <a name="troubleshoot-problems"></a>Sorunları giderme

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>"Bu URL 'de oturum açma alanları bulamadık" hatası alıyorum

Bu hata iletisini, oturum açma alanlarının otomatik algılanması başarısız olduğunda alırsınız. Sorunu çözmek için el ile oturum açma alanı algılamayı deneyin. Bu makalenin [bir uygulaması için, oturum açma alanlarını el ile yakala](#manually-capture-sign-in-fields-for-an-app) bölümüne bakın.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>"Çoklu oturum açma yapılandırması kaydedilemedi" hatası alıyorum

Nadiren, SSO yapılandırmasının güncelleştirilmesi başarısız olur. Bu sorunu çözmek için yapılandırmayı tekrar kaydetmeyi deneyin.

Hatayı almaya devam ederseniz bir destek talebi açın. Bu makalenin yardım bölümüne ulaşmak için [Portal bildirim ayrıntılarına bakın](#view-portal-notification-details) ve [bildirim ayrıntılarını bir destek mühendisine gönderin](#send-notification-details-to-a-support-engineer-to-get-help) .

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>Uygulamam için oturum açma alanlarını el ile algılayamıyorum

El ile algılama çalışmadığı zaman aşağıdaki davranışları gözlemleyebilirsiniz:

- El ile yakalama işlemi çalışıyor görünüyor, ancak yakalanan alanlar doğru değil.

- Yakalama işlemi çalıştırıldığında doğru alanlar vurgulanmaz.

- Yakalama işlemi sizi uygulamanın oturum açma sayfasına beklenen şekilde götürür, ancak hiçbir şey olmaz.

- El ile yakalama çalışır, ancak kullanıcılar erişim panelinden uygulamaya gittikten sonra SSO gerçekleşmez.

Bu sorunlardan herhangi biriyle karşılaşırsanız şunları yapın:

- Access panel tarayıcı uzantısının en son sürümünün *yüklü ve etkin*olduğundan emin olun. Bu makalenin [erişim paneli tarayıcı uzantısını yükler](#install-the-access-panel-browser-extension) bölümüne bakın.

- Yakalama işlemi sırasında tarayıcınızın, *InPrivate*veya *özel* *modda olmadığından emin*olun. Bu modlarda erişim paneli uzantısı desteklenmez.

- Kullanıcılarınızın erişim panelinden uygulamada oturum açmaya çalışmalarından *,* *InPrivate*veya *özel moddayken*emin olun.

- El ile yakalama işlemini yeniden deneyin. Kırmızı işaretçilerin doğru alanların üzerinde olduğundan emin olun.

- El ile yakalama işlemi yanıt vermeyi durdurmuş gibi görünüyorsa veya oturum açma sayfası yanıt vermezse, el ile yakalama işlemini yeniden deneyin. Ancak, bu kez işlemi tamamladıktan sonra gözatıcının geliştirici konsolunu açmak için F12 tuşuna basın. **Konsol** sekmesini seçin. **Window. Location = "yazın ve *Uygulamayı yapılandırırken belirttiğiniz oturum açma URL 'sini&lt;&gt;*** ve ardından ENTER tuşuna basın. Bu, yakalama işlemini sonlandıran ve yakalanan alanları depolayan bir sayfa yeniden yönlendirmeyi zorlar.

### <a name="contact-support"></a>Desteğe başvurun

Hala sorun yaşıyorsanız Microsoft Desteği bir servis talebi açın. Ne denediklerinizi tanıtın. Bu makalenin yardım bölümlerini almak için, [Portal bildirim ayrıntılarına bakın](#view-portal-notification-details) ve [bildirim ayrıntılarını bir destek mühendisine gönderin](#send-notification-details-to-a-support-engineer-to-get-help) (varsa).

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükler

Şu adımları uygulayın:

1. [Erişim panelini](https://myapps.microsoft.com) desteklenen bir tarayıcıda açın. Azure AD 'de *Kullanıcı*olarak oturum açın.

2. Access panel 'de **Password-SSO uygulaması** ' nı seçin.

3. Yazılımı yüklemek isteyip istemediğiniz sorulduğunda **Şimdi yüklensin**' i seçin.

4. Tarayıcınız için bir indirme sayfasına yönlendirilirsiniz. Uzantıyı **eklemeyi** seçin.

5. İstenirse **Etkinleştir** veya **izin ver**' i seçin.

6. Yükleme sonrasında tarayıcınızı yeniden başlatın.

7. Erişim panelinde oturum açın. Password-SSO özellikli uygulamalarınızı açabilme konusunda bilgi için bkz.

Ayrıca, Chrome ve Firefox tarayıcı uzantısını bu bağlantılar aracılığıyla doğrudan indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Portal bildirim ayrıntılarını görüntüleme

Herhangi bir portal bildiriminin ayrıntılarını görmek için şu adımları izleyin:

1. Azure portal sağ üst köşesindeki **Bildirimler** simgesini (zil) seçin.

2. Bir *hata* durumu gösteren herhangi bir bildirim seçin. (Kırmızı bir "!" vardır.)

   > [!NOTE]
   > *Başarılı* veya *sürüyor* durumunda olan bildirimleri seçemezsiniz.

3. **Bildirim ayrıntıları** bölmesi açılır. Sorun hakkında bilgi edinmek için bilgileri okuyun.

5. Hala yardıma ihtiyacınız varsa, bilgileri bir destek mühendisiyle veya ürün grubuyla paylaşabilirsiniz. Bildirim ayrıntılarını paylaşmak üzere kopyalamak için **kopyalama hatası** kutusunun sağ tarafındaki **Kopyala** simgesini seçin.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Yardım almak için bir destek mühendisine bildirim ayrıntıları gönderin

Bu bölümde listelenen *Tüm* ayrıntıları, hızlı bir şekilde yardımcı olmaları için destek ile paylaşmanız önemlidir. Kaydetmek için, bir ekran görüntüsü alabilir veya **kopyalama hatası**' nı seçebilirsiniz.

Aşağıdaki bilgiler, her bildirim öğesinin ne anlama geldiğini açıklar ve örnekler sağlar.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

- **Başlık**: bildirimin açıklayıcı başlığı.

   Örnek: *uygulama proxy 'si ayarları*

- **Açıklama**: işlemin sonucu olarak ne oldu.

   Örnek: *girilen Iç URL zaten başka bir uygulama tarafından kullanılıyor.*

- **BILDIRIM kimliği**: BILDIRIMIN benzersiz kimliği.

    Örnek: *clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **Istemci Istek kimliği**: tarayıcınızın yaptığı özel istek kimliği.

    Örnek: *302fd775-3329-4670-a9f3-kir, 37004f0bc*

- **Zaman DAMGASı UTC**: bildirimin gerçekleştiği zaman DAMGASı (UTC).

    Örnek: *2017-03-23T19:50:43.7583681 z*

- **Iç Işlem kimliği**: sistemlerimizde hatayı aramak için kullanılan iç kimlik.

    Örnek: **71a2f329-ca29-402f-AA72-bc00a7aca603**

- **UPN**: işlemi çalıştıran kullanıcı.

    Örnek: *tperkins\@f128.info*

- **KIRACı kimliği**: işlemi çalıştıran kullanıcının üyesi olduğu kiracının benzersiz kimliği.

    Örnek: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Kullanıcı nesne kimliği**: işlemi çalıştıran kullanıcının benzersiz kimliği.

    Örnek: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

- **Görünen ad**: (boş olabilir) hata için daha ayrıntılı bir görünen ad.

    Örnek: *uygulama proxy 'si ayarları*

- **Durum**: bildirimin özel durumu.

    Örnek: *başarısız oldu*

- **Nesne kimliği**: (boş olabilir) işlemin ÇALıŞTıRıLDıĞı nesne kimliği.

   Örnek: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Ayrıntılar**: işlemin sonucu olarak neyin ne olduğuna ilişkin ayrıntılı açıklama.

    Örnek: *'<https://bing.com/>' iç URL 'si zaten kullanımda olduğundan geçersiz.*

- **Kopyalama hatası**: destek konusunda yardım almak üzere bildirim ayrıntılarını kopyalamak için **kopyalama hatası** metin kutusunun sağ tarafındaki **Kopyala simgesini** seçmenizi sağlar.

    Örnek: ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama proxy 'Si ile uygulamalarınıza çoklu oturum açma sağlama](application-proxy-configure-single-sign-on-with-kcd.md)
