---
title: Azure Çok Faktörlü Kimlik Doğrulamayı Yapılandırma - Azure Etkin Dizini
description: Bu makalede, Azure portalında Azure Çok Faktörlü Kimlik Doğrulama ayarlarının nasıl yapılandırılabildiğini açıklanmaktadır
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 077032e4fe3886d5bf9a678dffdffca1a5802091
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263809"
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure Çok Faktörlü Kimlik Doğrulama ayarlarını yapılandırma

Bu makale, Azure portalında Çok Faktörlü Kimlik Doğrulama ayarlarını yönetmenize yardımcı olur. Azure Çok Faktörlü Kimlik Doğrulama'dan en iyi şekilde kurtulmanıza yardımcı olan çeşitli konuları kapsar. Özelliklerin tümü Azure Çok Faktörlü Kimlik Doğrulama'nın her sürümünde kullanılamaz.

**Azure Active Directory** > **Security** > **MFA'ya**göz atarak Azure Çok Faktörlü Kimlik Doğrulaması ile ilgili ayarlara Azure portalından erişebilirsiniz.

![Azure portalı - Azure AD Çok Faktörlü Kimlik Doğrulama ayarları](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-portal.png)

## <a name="settings"></a>Ayarlar

Bu ayarlardan bazıları MFA Server, Azure MFA veya her ikisi için de geçerlidir.

| Özellik | Açıklama |
| ------- | ----------- |
| Hesap kilitleme | Satırda çok fazla reddedilen kimlik doğrulama denemesi varsa, hesapları çok faktörlü kimlik doğrulama hizmetinde geçici olarak kilitleyin. Bu özellik yalnızca kimlik doğrulaması için PIN giren kullanıcılar için geçerlidir. (MFA Sunucusu) |
| [Kullanıcıları engelleme/engellemeyi kaldırma](#block-and-unblock-users) | Belirli kullanıcıların Çok Faktörlü Kimlik Doğrulama isteklerini alabilmelerini engellemek için kullanılır. Engellenen kullanıcılar için tüm kimlik doğrulaması denemeleri otomatik olarak reddedilir. Kullanıcılar engellendikleri tarihten 90 gün sonrasına kadar engellenmiş olarak kalır. |
| [Sahtekarlık uyarısı](#fraud-alert) | Kullanıcıların sahte doğrulama isteklerini bildirme becerisiyle ilgili ayarları yapılandırın |
| [Bildirimler](#notifications) | MFA Server'dan olay bildirimlerini etkinleştirin. |
| [OATH belirteçleri](concept-authentication-methods.md#oath-hardware-tokens-public-preview) | Kullanıcılar için OATH belirteçlerini yönetmek için bulut tabanlı Azure MFA ortamlarında kullanılır. |
| [Telefon görüşmesi ayarları](#phone-call-settings) | Bulut ve şirket içi ortamlar için telefon aramaları ve karşılamalarla ilgili ayarları yapılandırın. |
| Sağlayıcılar | Bu, hesabınızla ilişkili olabileceğiniz varolan kimlik doğrulama sağlayıcılarını gösterir. 1 Eylül 2018 itibariyle yeni kimlik doğrulama sağlayıcıları oluşturulamaz |

## <a name="manage-mfa-server"></a>MFA Server'ı Yönet

Bu bölümdeki ayarlar yalnızca MFA Server içindir.

| Özellik | Açıklama |
| ------- | ----------- |
| Sunucu ayarlar | MFA Server'ı indirin ve ortamınızı başlatmanız için etkinleştirme kimlik bilgilerini oluşturun |
| [Tek seferlik baypas](#one-time-bypass) | Bir kullanıcının sınırlı bir süre için iki aşamalı doğrulama gerçekleştirmeden kimlik doğrulaması gerçekleştirmesine izin verin. |
| [Önbelleğe alma kuralları](#caching-rules) |  Önbelleğe alma, ilk istek hala devam ederken VPN gibi şirket içi sistemler birden çok doğrulama isteği gönderdiğinde kullanılır. Bu özellik, kullanıcı devam eden ilk doğrulamayı başarıya kaydettikten sonra sonraki isteklerin otomatik olarak başarılı olmasını sağlar. |
| Sunucu durumu | Sürüm, durum, IP ve son iletişim saati ve tarihi de dahil olmak üzere şirket içi MFA sunucularınızın durumunu görün. |

## <a name="activity-report"></a>Faaliyet raporu

Burada bulunan raporlama MFA Server 'a (şirket içi) özgüdür. Azure MFA (bulut) raporları için oturum açma raporuna Azure AD'de bakın.

## <a name="block-and-unblock-users"></a>Kullanıcıları engelleme ve engellemeyi kaldırma

Kullanıcıların kimlik doğrulama isteklerialmasını önlemek için _blok ve engellemeyi kaldırın._ Engellenen kullanıcılar için tüm kimlik doğrulaması denemeleri otomatik olarak reddedilir. Kullanıcılar engellendikleri tarihten 90 gün sonrasına kadar engellenmiş olarak kalır.

### <a name="block-a-user"></a>Kullanıcıyı engelleme

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **Blok/engellemesini kaldır kullanıcılarına**göz atın.
3. Kullanıcıyı engellemek için **Ekle'yi** seçin.
4. Çoğaltma **Grubu'nu**seçin. Engellenen kullanıcının kullanıcı adını **kullanıcı\@adı olarak**domain.com. **Neden** alanına bir açıklama girin.
5. Kullanıcıyı engellemeyi bitirmek için **Ekle'yi** seçin.

### <a name="unblock-a-user"></a>Kullanıcının engelini kaldırma

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **Blok/engellemesini kaldır kullanıcılarına**göz atın.
3. Engellemeyi kaldırmak için kullanıcının yanındaki **Eylem** sütunundaki **Engellemeyi Kaldır'ı** seçin.
4. **Engellemeyi kaldırma nedenine** bir açıklama girin.
5. Kullanıcının engelini kaldırmayı bitirmek için **Engellemeyi Kaldır'ı** seçin.

## <a name="fraud-alert"></a>Sahtekarlık uyarısı

Kullanıcılarınızın kaynaklarına erişmek için sahte girişimleri bildirebileceği şekilde _dolandırıcılık uyarısı_ özelliğini yapılandırın. Kullanıcılar, mobil uygulamayı kullanarak veya telefonları aracılığıyla dolandırıcılık girişimlerini bildirebilir.

### <a name="turn-on-fraud-alerts"></a>Dolandırıcılık uyarılarını açma

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **Dolandırıcılık uyarısına**göz atın.
3. Kullanıcıların **A'ya** **ayarlanan sahtekarlık uyarıları göndermelerine izin** verme'yi ayarlayın.
4. **Kaydet'i**seçin.

### <a name="configuration-options"></a>Yapılandırma seçenekleri

* **Sahtekarlığın bildirilen kullanıcıyı engellemesi**: Bir kullanıcı dolandırıcılığı rapor ederse, hesabı 90 gün boyunca veya bir yönetici hesabının engelini kaldırana kadar engellenir. Yönetici, oturum açma raporunu kullanarak oturum açma ları gözden geçirebilir ve gelecekteki dolandırıcılığı önlemek için uygun eylemi gerçekleştirebilir. Yönetici daha sonra kullanıcının hesabının [engelini kaldırabilir.](#unblock-a-user)
* **İlk karşılama sırasında sahtekarlığı bildiren kod**: Kullanıcılar iki aşamalı doğrulama yapmak için **#** bir telefon görüşmesi aldığında, normalde oturum açma larını onaylamak için bastırır. Dolandırıcılığı bildirmek için, kullanıcı basmadan **#** önce bir kod girer. Bu kod **0** varsayılan olarak 0'dır, ancak özelleştirebilirsiniz.

   >[!NOTE]
   >Microsoft'un varsayılan sesli selamlamaları, kullanıcılara bir sahtekarlık uyarısı göndermeleri için **0#** tuşuna basmalarını bildirir. **0'dan**başka bir kod kullanmak istiyorsanız, kullanıcılarınız için uygun talimatlarla kendi özel sesli tebriklerinizi kaydedin ve yükleyin.
   >

### <a name="view-fraud-reports"></a>Dolandırıcılık raporlarını görüntüleme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Azure Etkin Dizin** > **Oturum Açma'yı**seçin. Dolandırıcılık raporu artık standart Azure AD Oturum Açma raporunun bir parçasıdır.

## <a name="notifications"></a>Bildirimler

Dolandırıcılık uyarısı e-postaları alacak kullanıcılar için e-posta adreslerini burada yapılandırın.

![Bildirim sahtekarlığı uyarısı e-posta örneği](./media/howto-mfa-mfasettings/multi-factor-authentication-fraud-alert-email.png)

## <a name="phone-call-settings"></a>Telefon görüşmesi ayarları

### <a name="caller-id"></a>Arayan Kimliği

**MFA arayan kimlik numarası** - Bu, kullanıcılarınızın telefonlarında göreceği numaradır. Yalnızca ABD tabanlı numaralar kullanılabilir.

>[!NOTE]
>Çok Faktörlü Kimlik Doğrulama aramaları ortak telefon ağı üzerinden yerleştirildiğinde, bazen arayan kimliğini desteklemeyen bir operatör aracılığıyla yönlendirilir. Bu nedenle, Çok Faktörlü Kimlik Doğrulama sistemi her zaman gönderse de arayan kimliği garanti edilmez.

Amerika Birleşik Devletleri'nde, MFA Caller KIMLIĞINI yapılandırmadıysanız, Microsoft'tan gelen sesli aramalar aşağıdaki numaralardan gelir: +1 (866) 539 4191, +1 (855) 330 8653 ve +1 (877) 668 6536. Spam filtreleri kullanıyorsanız, bu numaraları hariç tutadığından emin olun.

### <a name="custom-voice-messages"></a>Özel sesli mesajlar

_Özel sesli mesajlar_ özelliğiyle iki aşamalı doğrulama için kendi kayıtlarınızı veya selamlamalarınızı kullanabilirsiniz. Bu iletiler, Microsoft kayıtlarına ek olarak veya yerine kullanılabilir.

Başlamadan önce aşağıdaki kısıtlamalara dikkat edin:

* Desteklenen dosya biçimleri .wav ve .mp3'tür.
* Dosya boyutu sınırı 1 MB'dır.
* Kimlik doğrulama iletileri 20 saniyeden kısa olmalıdır. 20 saniyeden uzun iletiler doğrulamanın başarısız olması yla ilgili olabilir. Kullanıcı, ileti bitmeden ve doğrulama süreleri dolmadan yanıt vermeyebilir.

### <a name="custom-message-language-behavior"></a>Özel ileti dili davranışı

Kullanıcıya özel bir sesli ileti çalındığında, iletinin dili aşağıdaki etkenlere bağlıdır:

* Geçerli kullanıcının dili.
  * Kullanıcının tarayıcısı tarafından algılanan dil.
  * Diğer kimlik doğrulama senaryoları farklı davranabilir.
* Kullanılabilir özel iletilerin dili.
  * Bu dil, özel bir ileti eklendiğinde yönetici tarafından seçilir.

Örneğin, Almanca bir dili olan tek bir özel ileti varsa:

* Almanca kimlik doğrulayan bir kullanıcı özel Almanca mesajını duyar.
* İngilizce kimlik doğrulayan bir kullanıcı standart İngilizce iletiyi duyar.

### <a name="set-up-a-custom-message"></a>Özel bir ileti ayarlama

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. **Azure Active Directory** > **Security** > **MFA** > Telefon arama**ayarlarına**göz atın.
1. **Tebrik Ekle'yi**seçin.
1. Karşılama türünü seçin.
1. Dili seçin.
1. Yüklemek için bir .mp3 veya .wav ses dosyası seçin.
1. **Ekle'yi**seçin.

### <a name="custom-voice-message-defaults"></a>Özel sesli ileti varsayılanları

Özel iletiler oluşturmak için örnek komut dosyaları.

| İleti adı | Betik |
| --- | --- |
| Kimlik doğrulama başarılı | Oturum açma nız başarıyla doğrulandı. Hoşça kal. |
| Uzantı istemi | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Devam etmek için lütfen pound tuşuna basın. |
| Dolandırıcılık Onayı | Bir sahtekarlık uyarısı gönderildi. Hesabınızın engelini kaldırmak için lütfen şirketinizin BT yardım masasına başvurun. |
| Dolandırıcılık karşılama (Standart) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. Bu doğrulamayı başlatmadıysanız, birisi hesabınıza erişmeye çalışıyor olabilir. Lütfen bir sahtekarlık uyarısı göndermek için sıfır pound tuşuna basın. Bu, şirketinizin BT ekibini bilgilendirecek ve daha fazla doğrulama girişimini engelleyecektir. |
| Dolandırıcılık ihbarı bir sahtekarlık uyarısı gönderildi. | Hesabınızın engelini kaldırmak için lütfen şirketinizin BT yardım masasına başvurun. |
| Etkinleştirme | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. |
| Kimlik doğrulama yeniden reddedildi | Doğrulama reddedildi. |
| Yeniden Deneme (Standart) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. |
| Tebrik (Standart) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. |
| Tebrik (PIN) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Lütfen PIN'inizi girin ve ardından doğrulamanızı bitirmek için pound tuşuna basın. |
| Dolandırıcılık karşılama (PIN) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz.  Lütfen PIN'inizi girin ve ardından doğrulamanızı bitirmek için pound tuşuna basın. Bu doğrulamayı başlatmadıysanız, birisi hesabınıza erişmeye çalışıyor olabilir. Lütfen bir sahtekarlık uyarısı göndermek için sıfır pound tuşuna basın. Bu, şirketinizin BT ekibini bilgilendirecek ve daha fazla doğrulama girişimini engelleyecektir. |
| Yeniden Deneme(PIN) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Lütfen PIN'inizi girin ve ardından doğrulamanızı bitirmek için pound tuşuna basın. |
| Basamaklardan sonra uzantı istemi | Bu uzantıda zaten varsa, devam etmek için pound tuşuna basın. |
| Kimlik doğrulama reddedildi | Üzgünüm, şu anda sizi imzalayamayız. Lütfen daha sonra tekrar deneyin. |
| Etkinleştirme karşılama (Standart) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. |
| Etkinleştirme yeniden denemesi (Standart) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Doğrulamanızı bitirmek için lütfen pound tuşuna basın. |
| Etkinleştirme karşılama (PIN) | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Lütfen PIN'inizi girin ve ardından doğrulamanızı bitirmek için pound tuşuna basın. |
| Basamaklardan önce uzantı istemi | Microsoft'un oturum açma doğrulama sistemini kullandığınız için teşekkür ederiz. Lütfen bu aramayı uzantıya aktarın ... |

## <a name="one-time-bypass"></a>Tek seferlik baypas

_Tek seferlik atlama_ özelliği, kullanıcının iki aşamalı doğrulama gerçekleştirmeden tek bir kez kimlik doğrulaması yapmasına olanak tanır. Baypas geçicidir ve belirtilen sayıda saniyeden sonra sona erer. Mobil uygulamanın veya telefonun bildirim veya telefon görüşmesi almadığı durumlarda, kullanıcının istenen kaynağa erişebilmesi için tek seferlik bir bypass'a izin verebilirsiniz.

### <a name="create-a-one-time-bypass"></a>Tek seferlik baypas oluşturma

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **One-time bypass'a**göz atın.
3. **Ekle'yi**seçin.
4. Gerekirse, bypass için çoğaltma grubunu seçin.
5. **Kullanıcı adını kullanıcı adı\@olarak domain.com.** Baypasın sürmesi gereken saniye sayısını girin. Baypas nedenini girin.
6. **Ekle'yi**seçin. Zaman sınırı hemen yürürlüğe girer. Kullanıcının bir kerelik bypass süresi dolmadan önce oturum açması gerekir.

### <a name="view-the-one-time-bypass-report"></a>Tek seferlik atlama raporunu görüntüleme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **One-time bypass'a**göz atın.

## <a name="caching-rules"></a>Önbelleğe alma kuralları

_Önbelleğe alma_ özelliğini kullanarak bir kullanıcı kimlik doğrulama denemesine izin verecek bir süre ayarlayabilirsiniz. Belirtilen süre içinde kullanıcı için sonraki kimlik doğrulama denemeleri otomatik olarak başarılı olur. Önbelleğe alma, ilk istek hala devam ederken VPN gibi şirket içi sistemler birden çok doğrulama isteği gönderdiğinde kullanılır. Bu özellik, kullanıcı devam eden ilk doğrulamayı başarıya kaydettikten sonra sonraki isteklerin otomatik olarak başarılı olmasını sağlar.

>[!NOTE]
>Önbelleğe alma özelliği, Azure Etkin Dizini'nde (Azure AD) oturum açmaiçin kullanılmak üzere tasarlanmamıştır.

### <a name="set-up-caching"></a>Önbelleğe alma

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizin** > **Güvenliği** > **MFA** > **Önbelleğe Alma kurallarına**göz atın.
3. **Ekle'yi**seçin.
4. Açılan listeden **önbellek türünü** seçin. En fazla **önbellek saniyesayısını**girin.
5. Gerekirse, kimlik doğrulama türünü seçin ve bir uygulama belirtin.
6. **Ekle'yi**seçin.

## <a name="mfa-service-settings"></a>MFA hizmet ayarları

Uygulama parolaları, güvenilir IP'ler, doğrulama seçenekleri ve Azure Çok Faktörlü Kimlik Doğrulaması için çok faktörlü kimlik doğrulamayı hatırlama ayarları hizmet ayarlarında bulunabilir. Hizmet ayarlarına Azure Portalı'ndan Azure **Active Directory** > **Security** > **MFA'ya** > göz atarak erişebilirsiniz**Ek bulut tabanlı MFA ayarlarını****Yapılandırmaya** > **başlar.** > 

![Azure Çok Faktörlü Kimlik Doğrulama hizmeti ayarları](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-service-settings.png)

Güvenilen IP adresi aralıkları özel veya herkese açık olabilir.

## <a name="app-passwords"></a>Uygulama parolaları

Office 2010 veya daha önceki uygulamalar ve iOS 11'den önceki Apple Mail gibi bazı uygulamalar iki aşamalı doğrulamayı desteklemez. Uygulamalar ikinci bir doğrulamayı kabul etmek üzere yapılandırılmamıştır. Bu uygulamaları kullanmak için uygulama _parolaları_ özelliğinden yararlanın. Bir uygulamanın iki aşamalı doğrulamayı atlayıp çalışmaya devam etmesine izin vermek için geleneksel parolanızın yerine bir uygulama parolası kullanabilirsiniz.

Microsoft Office 2013 istemcileri ve sonrası için modern kimlik doğrulaması desteklenir. Outlook dahil Office 2013 istemcileri, modern kimlik doğrulama protokollerini destekler ve iki aşamalı doğrulamayla çalışması etkinleştirilebilir. İstemci etkinleştirildikten sonra istemci için uygulama parolaları gerekmez.

>[!NOTE]
>Uygulama parolaları Koşullu Erişim tabanlı çok faktörlü kimlik doğrulama ilkeleri ve modern kimlik doğrulama ile çalışmaz.

### <a name="considerations-about-app-passwords"></a>Uygulama parolaları ile ilgili dikkat edilmesi gerekenler

Uygulama parolalarını kullanırken aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulama parolaları uygulama başına yalnızca bir kez girilir. Kullanıcıların parolaları takip etmeleri veya her seferinde girmeleri gerekmez.
* Gerçek parola otomatik olarak oluşturulur ve kullanıcı tarafından sağlanmaz. Otomatik olarak oluşturulan parola, saldırganın tahmin etmesi daha zordur ve daha güvenlidir.
* Kullanıcı başına 40 parola sınırı vardır.
* Parolaları önbelleğe alan ve bunları şirket içi senaryolarda kullanan uygulamalar, uygulama parolası iş veya okul hesabı dışında bilinmediği için başarısız olmaya başlayabilir. Bu senaryoya örnek olarak, şirket içinde bulunan, ancak arşivlenmiş posta bulutta olan Exchange e-postaları örnektir. Bu senaryoda, aynı parola çalışmıyor.
* Çok Faktörlü Kimlik Doğrulama bir kullanıcının hesabında etkinleştirildikten sonra, uygulama parolaları Outlook ve Microsoft Skype for Business gibi tarayıcı olmayan istemcilerin çoğunda kullanılabilir. Windows PowerShell gibi tarayıcı dışı uygulamalar aracılığıyla uygulama parolaları kullanılarak yönetim işlemleri gerçekleştirimez. Kullanıcının yönetim hesabı olsa bile eylemler gerçekleştirilemez. PowerShell komut dosyalarını çalıştırmak için güçlü bir parolaya sahip bir hizmet hesabı oluşturun ve iki aşamalı doğrulama için hesabı etkinleştirme.

>[!WARNING]
>Uygulama parolaları, istemcilerin hem şirket içinde hem de bulutotomatik olarak keşfedilen uç noktalarıyla iletişim kurduğu karma ortamlarda çalışmaz. Şirket içinde kimlik doğrulaması yapmak için etki alanı parolaları gereklidir. Uygulama parolalarının bulutla kimlik doğrulaması yapılması gerekir.

### <a name="guidance-for-app-password-names"></a>Uygulama şifre adları için kılavuz

Uygulama parola adları kullanıldıkları cihazı yansıtmalıdır. Outlook, Word ve Excel gibi tarayıcı dışı uygulamaları olan bir dizüstü bilgisayarınız varsa, bu uygulamalar için **Laptop** adında bir uygulama parolası oluşturun. Masaüstü bilgisayarınızda çalışan aynı uygulamalar için **Masaüstü** adlı başka bir uygulama parolası oluşturun.

>[!NOTE]
>Uygulama başına bir uygulama parolası yerine cihaz başına bir uygulama parolası oluşturmanızı öneririz.

### <a name="federated-or-single-sign-on-app-passwords"></a>Federe veya tek oturum açma uygulaması parolaları

Azure AD, şirket içi Windows Server Active Directory Domain Services (AD DS) ile federasyonu veya tek oturum açmayı (SSO) destekler. Kuruluşunuz Azure AD ile bıkmışsa ve Azure Çok Faktörlü Kimlik Doğrulaması kullanıyorsanız, uygulama parolalarıyla ilgili aşağıdaki noktaları göz önünde bulundurun.

>[!NOTE]
>Aşağıdaki puanlar yalnızca federe (SSO) müşterileri için geçerlidir.

* Uygulama parolaları Azure AD tarafından doğrulanır ve bu nedenle federasyonu atlar. Federasyon yalnızca uygulama parolalarını ayarlarken etkin olarak kullanılır.
* Kimlik Sağlayıcı (IdP) pasif akışı aksine federe (SSO) kullanıcıları için temas edilmez. Uygulama parolaları iş veya okul hesabında depolanır. Bir kullanıcı şirketten ayrılırsa, kullanıcının bilgileri **DirSync'i** gerçek zamanlı olarak kullanarak iş veya okul hesabına akar. Hesabın devre dışı sayılması/silinmesi eşitleme işlemini üç saat kadar sürebilir ve bu da Uygulama Parolası'nın Azure AD'de devre dışı kaldırılmasını/silinmesini geciktirebilir.
* Şirket içi istemci Erişim Denetimi ayarları uygulama parolaları özelliği tarafından onurlandırılmaz.
* Uygulama parolaları özelliğiyle kullanım için şirket içinde kimlik doğrulama günlüğü/denetim özelliği yoktur.
* Bazı gelişmiş mimariler, istemcilerle iki aşamalı doğrulama için kimlik bilgilerinin bir birleşimini gerektirir. Bu kimlik bilgileri, bir iş veya okul hesabı kullanıcı adı ve parolaları ve uygulama parolalarını içerebilir. Gereksinimler kimlik doğrulamanın nasıl gerçekleştirililene bağlıdır. Şirket içi altyapıya, iş veya okul hesabı kullanıcı adı ve parolasına karşı kimlik doğrulaması yapan istemciler için gereklidir. Azure AD'ye karşı kimlik doğrulaması yapan istemciler için bir uygulama parolası gereklidir.

  Örneğin, aşağıdaki mimariye sahip olduğunuzu varsayalım:

  * Şirket içi Active Directory örneğiniz Azure AD ile beslenir.
  * Exchange'i çevrimiçi olarak kullanıyorsun.
  * Skype for Business'ı şirket içinde kullanıyorsunuz.
  * Azure Çok Faktörlü Kimlik Doğrulaması kullanıyorsunuz.

  Bu senaryoda, aşağıdaki kimlik bilgilerini kullanırsınız:

  * Skype for Business'ta oturum açabilmek için iş veya okul hesabı kullanıcı adınızı ve şifrenizi kullanın.
  * Adres defterine çevrimiçi Exchange'e bağlanan bir Outlook istemcisinden erişmek için bir uygulama parolası kullanın.

### <a name="allow-users-to-create-app-passwords"></a>Kullanıcıların uygulama parolaları oluşturmasına izin verme

Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Uygulama parolaları özelliği etkinleştirilmelidir. Kullanıcılara uygulama parolaları oluşturma olanağı sağlamak için aşağıdaki yordamı kullanın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Kullanıcıları'nı**seçin.
3. **Çok Faktörlü Kimlik Doğrulama'yı**seçin.
4. Çok Faktörlü Kimlik Doğrulama **altında, hizmet ayarlarını**seçin.
5. Hizmet **Ayarları** sayfasında, **tarayıcı olmayan uygulamalarda oturum açmaları için kullanıcıların uygulama parolaları oluşturmasına izin** ver'i seçin.

### <a name="create-app-passwords"></a>Uygulama parolaları oluşturma

Kullanıcılar ilk kayıt sırasında uygulama parolaları oluşturabilir. Kullanıcı, kayıt işleminin sonunda uygulama parolaları oluşturma seçeneğine sahiptir.

Kullanıcılar kayıt tan sonra uygulama parolaları da oluşturabilir. Kullanıcılarınız için daha fazla bilgi ve ayrıntılı adımlar için Azure [Çok Faktörlü Kimlik Doğrulama'da uygulama parolaları nedir?](../user-help/multi-factor-authentication-end-user-app-passwords.md)

## <a name="trusted-ips"></a>Güvenilen IP'ler

Azure Çok Faktörlü Kimlik Doğrulama'nın _Güvenilir IP'ler_ özelliği, yönetilen veya federe bir kiracının yöneticileri tarafından kullanılır. Bu özellik, şirket intranetinden oturum kuran kullanıcılar için iki aşamalı doğrulamayı atlar. Bu özellik, yöneticiler için ücretsiz sürüm le değil, Azure Çok Faktörlü Kimlik Doğrulama'nın tam sürümüyle kullanılabilir. Azure Çok Faktörlü Kimlik Doğrulama'nın tam sürümünü nasıl alacağınız hakkında ayrıntılı bilgi için Azure [Çok Faktörlü Kimlik Doğrulama](multi-factor-authentication.md)bölümüne bakın.

> [!NOTE]
> MFA, IP'lere ve Koşullu Erişim adlı konumlara yalnızca IPV4 adresleriyle güvenir.

Kuruluşunuz şirket içi uygulamalara MFA sağlamak için NPS uzantısını dağıtırsa, kaynak IP adresinin her zaman NPS sunucusu gibi görüneceğini unutmayın.

| Azure AD kiracı türü | Güvenilir IP özellik seçenekleri |
|:--- |:--- |
| Yönetilen |**Belirli IP adresleri aralığı**: Yöneticiler, şirket intranetinden oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilen bir ip adresi aralığı belirtir. En fazla 50 Güvenilir IP aralığı yapılandırılabilir.|
| Federe |**Tüm Federe Kullanıcılar**: Kuruluşun içinden oturum kuran tüm federe kullanıcılar iki aşamalı doğrulamayı atlayabilir. Kullanıcılar, Active Directory Federation Services (AD FS) tarafından verilen bir talebi kullanarak doğrulamayı atlar.<br/>**Belirli IP adresleri aralığı**: Yöneticiler, şirket intranetinden oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilen bir ip adresi aralığı belirtir. |

Güvenilir IP'ler bypass yalnızca şirket intranetinin içinden çalışır. **Tüm Federe Kullanıcılar** seçeneğini ve şirket intraneti dışından bir kullanıcı işareti seçerseniz, kullanıcının iki aşamalı doğrulama kullanarak kimliğini doğrulaması zorundadır. Kullanıcı bir AD FS talebi sunsa bile işlem aynıdır. 

### <a name="end-user-experience-inside-of-corpnet"></a>Corpnet içinde son kullanıcı deneyimi

Güvenilen IP'ler özelliği devre dışı bırakıldığında, tarayıcı akışları için iki aşamalı doğrulama gerekir. Uygulama parolaları eski zengin istemci uygulamaları için gereklidir.

Güvenilen IP özelliği etkinleştirildiğinde, tarayıcı akışları için iki aşamalı doğrulama *gerekmez.* Kullanıcı nın bir uygulama parolası oluşturmamış olması koşuluyla, eski zengin istemci uygulamaları için uygulama parolaları *gerekmez.* Uygulama parolası kullanımda kaldıktan sonra parola gerekli kalır. 

### <a name="end-user-experience-outside-corpnet"></a>Corpnet dışında son kullanıcı deneyimi

Güvenilen IP özelliğinin etkin olup olmadığına bakılmaksızın, tarayıcı akışları için iki aşamalı doğrulama gereklidir. Uygulama parolaları eski zengin istemci uygulamaları için gereklidir.

### <a name="enable-named-locations-by-using-conditional-access"></a>Koşullu Erişim'i kullanarak adlandırılmış konumları etkinleştirme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Güvenliği** > **Koşullu Erişim** > Adlı**konumları**seçin.
3. **Yeni konumu**seçin.
4. Konum için bir ad girin.
5. **Güvenilir konum olarak İşaretle'yi**seçin.
6. **192.168.1.1/24**gibi CIDR gösteriminde IP Aralığını girin.
7. **Oluştur'u**seçin.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>Koşullu Erişim'i kullanarak Güvenilir IP özelliğini etkinleştirin

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Güvenliği** >  **Koşullu Erişim** > Adlı**konumları**seçin.
3. **MFA güvenilen IP'leri Yapılandır'ı**seçin.
4. Hizmet **Ayarları** sayfasında, **Güvenilir IP'ler**altında aşağıdaki iki seçenekten birini seçin:

   * **Intranetimden kaynaklanan federe kullanıcılardan gelen istekler için**: Bu seçeneği seçmek için onay kutusunu seçin. Şirket ağında oturum açan tüm federe kullanıcılar, AD FS tarafından verilen bir talebi kullanarak iki aşamalı doğrulamayı atlar. AD FS'nin intranet talebini uygun trafiğe eklemek için bir kuralı olduğundan emin olun. Kural yoksa, AD FS'de aşağıdaki kuralı oluşturun:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Belirli bir genel IP aralığından gelen istekler için**: Bu seçeneği seçmek için, CIDR gösterimini kullanarak metin kutusuna IP adreslerini girin.
      * xxx.xxx.xxx.1 aralığında olan IP adresleri için xxx.xxx.xxx.254, **xxx.xxx.xxx.0/24**gibi gösterim kullanın.
      * Tek bir IP adresi için **xxx.xxx.xxx.xxx/32**gibi gösterim kullanın.
      * En fazla 50 IP adresi aralığı girin. Bu IP adreslerinden oturum kuran kullanıcılar iki aşamalı doğrulamayı atlar.

5. **Kaydet'i**seçin.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>Hizmet ayarlarını kullanarak Güvenilir IP özelliğini etkinleştirme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Kullanıcıları'nı**seçin.
3. **Çok Faktörlü Kimlik Doğrulama'yı**seçin.
4. Çok Faktörlü Kimlik Doğrulama **altında, hizmet ayarlarını**seçin.
5. Hizmet **Ayarları** sayfasında, **Güvenilir IP'ler**altında aşağıdaki iki seçenekten birini (veya her ikisini) seçin:

   * **Intranetimdeki federe kullanıcılardan gelen istekler için**: Bu seçeneği seçmek için onay kutusunu seçin. Şirket ağında oturum açan tüm federe kullanıcılar, AD FS tarafından verilen bir talebi kullanarak iki aşamalı doğrulamayı atlar. AD FS'nin intranet talebini uygun trafiğe eklemek için bir kuralı olduğundan emin olun. Kural yoksa, AD FS'de aşağıdaki kuralı oluşturun:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Belirli bir IP adresi alt ağlarından gelen istekler için**: Bu seçeneği seçmek için, CIDR gösterimini kullanarak metin kutusuna IP adreslerini girin.
      * xxx.xxx.xxx.1 aralığında olan IP adresleri için xxx.xxx.xxx.254, **xxx.xxx.xxx.0/24**gibi gösterim kullanın.
      * Tek bir IP adresi için **xxx.xxx.xxx.xxx/32**gibi gösterim kullanın.
      * En fazla 50 IP adresi aralığı girin. Bu IP adreslerinden oturum kuran kullanıcılar iki aşamalı doğrulamayı atlar.

6. **Kaydet'i**seçin.

## <a name="verification-methods"></a>Doğrulama yöntemleri

Kullanıcılarınız için kullanılabilen doğrulama yöntemlerini seçebilirsiniz. Aşağıdaki tablo, yöntemlere kısa bir genel bakış sağlar.

Kullanıcılarınız hesaplarını Azure Çok Faktörlü Kimlik Doğrulamasına kaydettiklerinde, etkinleştirdiğiniz seçeneklerden tercih ettikleri doğrulama yöntemini seçerler. İki [aşamalı doğrulama için hesabımı ayarla'da](../user-help/multi-factor-authentication-end-user-first-time.md)kullanıcı kayıt işlemi için kılavuz sağlanır.

| Yöntem | Açıklama |
|:--- |:--- |
| Telefona çağrı |Otomatik sesli arama yerleştirir. Kullanıcı aramaya yanıt verir ve kimliğini doğrulamak için telefonda # tuşuna basar. Telefon numarası şirket içi Active Directory ile eşitlenmez. |
| Telefona kısa mesaj |Doğrulama kodu içeren bir metin iletisi gönderir. Kullanıcıdan oturum açma arabirimine doğrulama kodunu girmesi istenir. Bu işleme tek yönlü SMS denir. Çift yönlü SMS, kullanıcının belirli bir kodu geri alması gerektiği anlamına gelir. Çift yönlü SMS 14 Kasım 2018 tarihinden sonra küçümsülmez ve desteklenmez. Yöneticiler, daha önce çift yönlü SMS kullanan kullanıcılar için başka bir yöntem etkinleştirmelidir.|
| Mobil uygulama üzerinden bildirim |Telefonunuza veya kayıtlı cihazınıza bir anında iletme bildirimi gönderir. Kullanıcı bildirimi görüntüler ve doğrulamayı tamamlamak için **Doğrula'yı** seçer. Microsoft Authenticator uygulaması [Windows Phone,](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6) [Android](https://go.microsoft.com/fwlink/?Linkid=825072)ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073)için kullanılabilir. |
| Mobil uygulama veya donanım belirteci doğrulama kodu |Microsoft Authenticator uygulaması her 30 saniyede bir yeni bir OATH doğrulama kodu oluşturur. Kullanıcı doğrulama kodunu oturum açma arabirimine girer. Microsoft Authenticator uygulaması [Windows Phone,](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6) [Android](https://go.microsoft.com/fwlink/?Linkid=825072)ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073)için kullanılabilir. |

### <a name="enable-and-disable-verification-methods"></a>Doğrulama yöntemlerini etkinleştirme ve devre dışı

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Kullanıcıları'nı**seçin.
3. **Çok Faktörlü Kimlik Doğrulama'yı**seçin.
4. Çok Faktörlü Kimlik Doğrulama **altında, hizmet ayarlarını**seçin.
5. Hizmet **Ayarları** sayfasında, **doğrulama seçenekleri**altında, kullanıcılarınıza sağlayabileceğiniz yöntemleri seçin/seçin.
6. **Kaydet**'e tıklayın.

Kimlik doğrulama yöntemlerinin kullanımı ile ilgili ek ayrıntılar makalede kimlik [doğrulama yöntemleri nelerdir](concept-authentication-methods.md)bulunabilir.

## <a name="remember-multi-factor-authentication"></a>Çok Faktörlü Kimlik Doğrulamayı Hatırlayın

Kullanıcı tarafından güvenilen aygıtlar ve tarayıcılar için _hatırla Çok Faktörlü Kimlik_ Doğrulama özelliği, tüm Multi-Factor Kimlik Doğrulama kullanıcıları için ücretsiz bir özelliktir. Multi-Factor Authentication kullanarak bir cihazda başarıyla oturum açan kullanıcılar, belirtilen sayıda gün boyunca izleyen doğrulamaları atlayabilir. Bu özellik, bir kullanıcının aynı cihazda iki aşamalı doğrulama gerçekleştirme sini en aza indirme sayısını en aza indirerek kullanılabilirliği artırır.

>[!IMPORTANT]
>Bir hesap veya aygıtın gizliliği ihlal edilirse, güvenilen aygıtlar için Çok Faktörlü Kimlik Doğrulaması'nı hatırlamak güvenliği etkileyebilir. Bir şirket hesabı ele geçirilirse veya güvenilir bir aygıt kaybolursa veya çalınırsa, [MFA Oturumlarını Iptal](howto-mfa-userdevicesettings.md)Etmelisiniz.
>
>Geri yükleme eylemi tüm aygıtlardan güvenilen durumu iptal eder ve kullanıcının iki aşamalı doğrulamayı yeniden gerçekleştirmesi gerekir. Ayrıca, [iki aşamalı doğrulama için ayarlarınızı yönet'teki](../user-help/multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device)yönergelerle kullanıcılarınıza Kendi cihazlarında Çok Faktörlü Kimlik Doğrulamayı geri yüklemeleri için talimat verebilirsiniz.

### <a name="how-the-feature-works"></a>Özellik nasıl çalışır?

Çok Faktörlü Kimlik Doğrulamayı hatırla özelliği, kullanıcı oturum açma sırasında X gün seçeneğini **tekrar sormayın** seçeneğini seçtiğinde tarayıcıda kalıcı bir çerez ayarlar. Kullanıcı, çerezin süresi dolana kadar aynı tarayıcıdan Çoklu Faktör Kimlik Doğrulaması için tekrar istenmez. Kullanıcı aynı cihazda farklı bir tarayıcı açarsa veya çerezlerini temizlerse, doğrulamaları istenir.

**X gün için tekrar sormayın** seçeneği, uygulamanın modern kimlik doğrulamayı destekleyip desteklemediğine bakılmaksızın tarayıcı dışı uygulamalarda gösterilmez. Bu uygulamalar, her saat yeni erişim belirteçleri sağlayan _yeni belirteçleri_ kullanır. Yenileme belirteci doğrulandığında, Azure AD son iki adımlı doğrulamanın belirtilen gün sayısı içinde gerçekleştiğini denetler.

Bu özellik, normalde her seferinde istenen web uygulamalarındaki kimlik doğrulamalarının sayısını azaltır. Özellik, normalde her 90 günde bir hızlı olan modern kimlik doğrulama istemcileri için kimlik doğrulama larının sayısını artırır. Koşullu Erişim ilkeleriyle birleştirildiğinde kimlik doğrulama sayısını da artırabilir.

>[!IMPORTANT]
>**Hatırla Çok Faktörlü Kimlik Doğrulama** özelliği, kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama Sunucusu veya üçüncü taraf çok faktörlü kimlik doğrulama çözümü aracılığıyla AD FS için iki aşamalı doğrulama gerçekleştirdiği AD FS özelliğinde beni oturum **açtma** özelliğiyle uyumlu değildir.
>
>Kullanıcılarınız beni AD **FS'de oturum** alabildiğimi ve ayrıca cihazlarını Çok Faktörlü Kimlik Doğrulama için güvenilir olarak işaretlemeyi seçerse, **kullanıcı hatırlanan çok faktörlü kimlik doğrulama** gün sayısı dolduktan sonra otomatik olarak doğrulanmaz. Azure AD yeni bir iki aşamalı doğrulama ister, ancak AD FS yeniden iki aşamalı doğrulama gerçekleştirmek yerine özgün Çok Faktörlü Kimlik Doğrulama iddiası ve tarihiyle birlikte bir belirteç döndürür. **Bu tepki, Azure AD ve AD FS arasında bir doğrulama döngüsü oluşturur.**
>
>**Hatırla Çok Faktörlü Kimlik Doğrulama** özelliği B2B kullanıcılarıyla uyumlu değildir ve davet edilen kiracılarla oturum açtığınızda B2B kullanıcıları için görünmez.
>

### <a name="enable-remember-multi-factor-authentication"></a>Çok Faktörlü Kimlik Doğrulamayı hatırlamayı etkinleştirme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Kullanıcıları'nı**seçin.
3. **Çok Faktörlü Kimlik Doğrulama'yı**seçin.
4. Çok Faktörlü Kimlik Doğrulama **altında, hizmet ayarlarını**seçin.
5. Hizmet **Ayarları** sayfasında, **çok faktörlü kimlik doğrulamayı hatırla seçeneğini yönetin,** kullanıcıların **güvendikleri cihazlarda çok faktörlü kimlik doğrulamayı hatırlamalarına izin** ver'i seçin.
6. Güvenilen aygıtların iki aşamalı doğrulamayı atlayabilmeleri için gün sayısını ayarlayın. Varsayılan değer 14 gündür.
7. **Kaydet'i**seçin.

### <a name="mark-a-device-as-trusted"></a>Aygıtı güvenilir olarak işaretleme

Hatırla Çok Faktörlü Kimlik Doğrulama özelliğini etkinleştirdikten sonra, kullanıcılar **yeniden sorma**seçeneğini seçerek oturum açtıklarında aygıtı güvenilir olarak işaretleyebilirler.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD oturum açma sayfası markasını değiştirme](../fundamentals/customize-branding.md)
