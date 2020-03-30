---
title: Galeri dışı uygulamalar için parola SSO'su yapılandırma sorunları
description: Azure AD uygulama galerisinde olmayan özel uygulamalar için parola tek oturum açma (SSO) yapılandırdığınızda sık karşılaşılan sorunlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274142"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola tek oturum açma yapılandırma sorunları

Bu makalede, galeri dışı bir uygulama için *parola tek oturum açma* (SSO) yapılandırdığınızda oluşabilecek sık karşılaşılan sorunlar açıklanmaktadır.

## <a name="capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını yakalama

Oturum açma alanı yakalama yalnızca HTML özellikli oturum açma sayfaları için desteklenir. Adobe Flash veya HTML etkin olmayan diğer teknolojileri kullananlar gibi standart olmayan oturum açma sayfaları için desteklenmez.

Özel uygulamalarınız için oturum açma alanlarını yakalamanın iki yolu vardır:

- **Otomatik oturum açma alanı yakalama,** kullanıcı adı ve parola alanları için *iyi bilinen DIV aygıtları kullanıyorsa,* çoğu HTML özellikli oturum açma sayfalarında iyi çalışır. Sayfadaki HTML, belirli ölçütlerle eşleşen DIV i'lerini bulmak için kazınmıştır. Bu meta veriler, daha sonra uygulamaya yeniden oynatılması için kaydedilir.

- Uygulama satıcısı oturum açma giriş alanlarını *etiketlemezse,* **el ile oturum açma alanı yakalama** kullanılır. Satıcı *otomatik olarak algılanamaz birden çok alan işlerse*el ile yakalama da kullanılır. Azure Etkin Dizin (Azure AD), bu alanların sayfada nerede olduğunu söylerseniz, oturum açma sayfasında olduğu kadar çok alan için veri depolayabilir.

Genel olarak, otomatik oturum açma alanı yakalama çalışmıyorsa, manuel seçeneği deneyin.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını otomatik olarak yakalama

Parola tabanlı SSO'yı otomatik oturum açma alanı yakalama yı kullanarak yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com/)açın. Genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Sol taraftaki gezinti bölmesinde, Azure AD uzantısını açmak için **Tüm hizmetler'i** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar** listesinin en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, sol taraftaki gezinti bölmesinde **Tek oturum** açma'yı seçin.

8. **Parola tabanlı Oturum Açma** modunu seçin.

9. Kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri sayfanın URL'si olan **Oturum Açma URL'sini**girin. *Oturum açma alanlarının, sağladığınız URL'nin sayfasında görünür olduğundan emin olun.*

10. **Kaydet'i**seçin.

    Sayfa otomatik olarak kullanıcı adı ve şifre giriş kutuları için kazınmış. Artık Access Panel tarayıcı uzantısını kullanarak parolaları bu uygulamaya güvenli bir şekilde iletmek için Azure AD'yi kullanabilirsiniz.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını el ile yakalama

Oturum açma alanlarını el ile yakalamak için Access Panel tarayıcı uzantısı yüklü olmalıdır. Ayrıca, tarayıcınız *inPrivate,* *incognito*veya *özel* modda çalışıyor olamaz.

Uzantıyı yüklemek için bu [makalenin Access Panel Tarayıcı uzantısını yükle bölümüne](#install-the-access-panel-browser-extension) bakın.

El ile oturum açma alanı yakalamayı kullanarak bir uygulama için parola tabanlı SSO'yı yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com/)açın. Genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Sol taraftaki gezinti bölmesinde, Azure AD uzantısını açmak için **Tüm hizmetler'i** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE] 
   > İstediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar** listesinin en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, sol taraftaki gezinti bölmesinde **Tek oturum** açma'yı seçin.

8. **Parola tabanlı Oturum Açma** modunu seçin.

9. Kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri sayfa olan **Oturum Açma URL'sini**girin. *Oturum açma alanlarının, sağladığınız URL'nin sayfasında görünür olduğundan emin olun.*

10. **Uygulama * &lt;Adı&gt; * Parola Tek Oturum Açma Ayarlarını Yapılandır'ı**seçin.

11. **Oturum açma alanlarını El ile algıla'yı**seçin.

14. **Tamam**’ı seçin.

15. **Kaydet'i**seçin.

16. Access Panel'i kullanmak için yönergeleri izleyin.

## <a name="troubleshoot-problems"></a>Sorun giderme sorunları

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>"Bu URL'de oturum açma alanları bulamadık" hatası alıyorum

Oturum açma alanlarının otomatik olarak algılanması başarısız olduğunda bu hata iletisini alırsınız. Sorunu çözmek için el ile oturum açma alanı algılamayı deneyin. Bu makalenin bir uygulama bölümü [için El ile yakalama oturum açma alanlarına](#manually-capture-sign-in-fields-for-an-app) bakın.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>"Tek oturum açma yapılandırmasını kaydedemiyoruz" hatası alıyorum

Nadiren, SSO yapılandırmasını güncelleştirmek başarısız olur. Bu sorunu gidermek için yapılandırmayı yeniden kaydetmeyi deneyin.

Hatayı almaya devam ederseniz, bir destek örneği açın. Bu makalenin yardım bölümlerini almak için [portal bildirim ayrıntılarını görüntüle](#view-portal-notification-details) ve [bildirim ayrıntılarını destek mühendisine gönder'](#send-notification-details-to-a-support-engineer-to-get-help) de açıklanan bilgileri ekleyin.

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>Uygulamam için oturum açma alanlarını el ile algılamıyorum

El ile algılama çalışmadığında aşağıdaki davranışları gözlemleyebilirsiniz:

- El ile yakalama işlemi işe yarayabilir, ancak yakalanan alanlar doğru değil.

- Yakalama işlemi çalıştığında doğru alanlar vurgulanmıyor.

- Yakalama işlemi sizi beklendiği gibi uygulamanın oturum açma sayfasına götürür, ancak hiçbir şey olmaz.

- El ile yakalama çalışıyor gibi görünse de, kullanıcılar Access Panel'den uygulamaya gittiğinde SSO gerçekleşmez.

Bu sorunlardan herhangi birini yaşıyorsanız, aşağıdaki leri yapın:

- Access Panel tarayıcı uzantısının en son sürümünün *yüklü ve etkin*olduğundan emin olun. Bu [makalenin Access Paneltarayıcı uzantısını yükle bölümüne](#install-the-access-panel-browser-extension) bakın.

- Yakalama işlemi sırasında tarayıcınızın *gizli*, *özel*veya *Özel* modda olmadığından emin olun. Erişim Masası uzantısı bu modlarda desteklenmez.

- Kullanıcılarınızın Access Panel'den uygulamada gizli *,* *özel*veya *özel modda*oturum açmaya çalışmadığından emin olun.

- El ile yakalama işlemini yeniden deneyin. Kırmızı işaretçilerin doğru alanların üzerinde olduğundan emin olun.

- El ile yakalama işlemi yanıt vermeyi durdurmuş gibi görünüyorsa veya oturum açma sayfası yanıt vermiyorsa, el ile yakalama işlemini yeniden deneyin. Ancak bu kez, işlemi tamamladıktan sonra tarayıcınızın geliştirici konsolunu açmak için F12 tuşuna basın. **Konsol** sekmesini seçin. **Window.location yazın="*&lt;uygulamayı&gt;yapılandırırken belirttiğiniz oturum açma URL'si*" "**, sonra Enter tuşuna basın. Bu, yakalama işlemini sona erdirebilen ve yakalanan alanları depolayan bir sayfayı yeniden yönlendirmeye zorlar.

### <a name="contact-support"></a>Desteğe başvurun

Hala sorun yaşıyorsanız, Microsoft Destek ile bir servis talebi açın. Ne denediğinizi açıklayın. Bu makalenin yardım bölümlerini almak için Portal bildirim [ayrıntılarını görüntüle](#view-portal-notification-details) ve [bildirim ayrıntılarını destek mühendisine gönder](#send-notification-details-to-a-support-engineer-to-get-help) (varsa) açıklanan ayrıntıları ekleyin.

## <a name="install-the-access-panel-browser-extension"></a>Access Panel tarayıcı uzantısını yükleme

Şu adımları uygulayın:

1. Desteklenen bir tarayıcıda [Erişim Paneli'ni](https://myapps.microsoft.com) açın. *Kullanıcı*olarak Azure AD'de oturum açın.

2. Access Panel'de **parola-SSO uygulamasını** seçin.

3. Yazılımı yüklemeniz istendiğinde, **Şimdi Yükle'yi**seçin.

4. Tarayıcınız için bir indirme sayfasına yönlendirilirsiniz. Uzantıyı **eklemeyi** seçin.

5. Sizden istenirse, **Etkinleştir** veya **İzin Ver'i**seçin.

6. Yüklemeden sonra tarayıcınızı yeniden başlatın.

7. Access Paneli'nde oturum açın. Parola SSO özellikli uygulamalarınızı açıp açamayacağınızı görün.

Ayrıca doğrudan bu bağlantılar aracılığıyla Chrome ve Firefox için tarayıcı uzantısı indirebilirsiniz:

-   [Chrome Erişim Paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox Erişim Paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Portal bildirim ayrıntılarını görüntüleme

Herhangi bir portal bildiriminin ayrıntılarını görmek için aşağıdaki adımları izleyin:

1. Azure portalının sağ üst köşesindeki **Bildirimler** simgesini (çan) seçin.

2. *Hata* durumunu gösteren herhangi bir bildirimi seçin. (Onlar kırmızı var "!.)

   > [!NOTE]
   > *Başarılı* veya *Devam Eden* durumundaki bildirimleri seçemezsiniz.

3. **Bildirim Ayrıntıları** bölmesi açılır. Sorun hakkında bilgi edinmek için bilgileri okuyun.

5. Hala yardıma ihtiyacınız varsa, bilgileri bir destek mühendisi veya ürün grubuyla paylaşın. Paylaşmak için bildirim ayrıntılarını kopyalamak için **Kopyala hata** kutusunun sağındaki **kopya** simgesini seçin.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Yardım almak için bildirim ayrıntılarını destek mühendisine gönderme

Size hızlı bir şekilde yardımcı olmaları için bu bölümde listelenen *tüm* ayrıntıları destekle paylaşmanız önemlidir. Kaydetmek için ekran görüntüsü alabilir veya **Kopyala hatasını**seçebilirsiniz.

Aşağıdaki bilgiler, her bildirim öğesinin ne anlama geldiğini açıklar ve örnekler sağlar.

### <a name="essential-notification-items"></a>Temel bildirim öğeleri

- **Başlık**: bildirimin açıklayıcı başlığı.

   Örnek: *Uygulama proxy ayarları*

- **Açıklama**: operasyon sonucunda meydana gelen.

   Örnek: *Girilen dahili URL zaten başka bir uygulama tarafından kullanılıyor.*

- **Bildirim Kimliği**: bildirimin benzersiz kimliği.

    Örnek: *istemciBildirim-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **İstemci İstek Kimliği:** Tarayıcınızın yaptığı özel istek kimliğidir.

    Örnek: *302fd775-3329-4670-a9f3-bea37004f0bc*

- **Zaman Damgası UTC**: UTC'de bildirimin gerçekleştiği zaman dilimi.

    Örnek: *2017-03-23T19:50:43.7583681Z*

- **Dahili İşlem Kimliği**: Sistemlerimizdeki hatayı aramak için kullanılan dahili kimlik.

    Örnek: **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN**: İşlemi çalıştıran kullanıcı.

    Örnek: *tperkins\@f128.info*

- **Kiracı Kimliği**: İşi yürüten kullanıcının üyesi olduğu kiracının benzersiz kimliğidir.

    Örnek: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Kullanıcı nesnekimliği**: İşlemi yürüten kullanıcının benzersiz kimliği.

    Örnek: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

- **Ekran Adı**: (boş olabilir) hata için daha ayrıntılı bir ekran adı.

    Örnek: *Uygulama proxy ayarları*

- **Durum**: bildirimin özel durumu.

    Örnek: *Başarısız oldu*

- **Object ID**: (boş olabilir) işlemin çalıştırıldığı nesne kimliği.

   Örnek: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Ayrıntılar**: operasyon sonucunda meydana gelen şeyin ayrıntılı açıklaması.

    Örnek: *İç<https://bing.com/>url ' ' ' zaten kullanımda olduğu için geçersizdir.*

- **Kopyalama hatası**: Destek le yardımcı olmak için bildirim ayrıntılarını kopyalamak için **Kopyala hatası** textbox'ın sağındaki **kopya simgesini** seçmenize olanak tanır.

    Örnek:```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
