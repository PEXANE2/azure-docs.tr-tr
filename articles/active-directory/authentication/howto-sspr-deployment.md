---
title: Self servis parola sıfırlama dağıtımı - Azure Etkin Dizini
description: Azure AD self servis parola sıfırlamanın başarılı bir şekilde uygulanması için strateji
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: baselden
author: barbaraselden
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 785a8a031a10232a37b235711ba919fdc1df35d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061444"
---
# <a name="plan-an-azure-active-directory-self-service-password-reset"></a>Azure Active Directory self servis parola sıfırlama planı

> [!NOTE]
> Bu dağıtım planı, Azure AD self servis parola sıfırlama (SSPR) dağıtmaya yönelik planlama kılavuzu ve en iyi uygulamaları sunar. <br>**Hesabınıza geri [https://aka.ms/sspr](https://aka.ms/sspr)dönmek için SSPR aracı arıyorsanız, **gidin.

[Self Servis Parola Sıfırlama (SSPR),](https://www.youtube.com/watch?v=tnb2Qf4hTP8) kullanıcıların yardım almak için BT personeliyle iletişim ebaşvurmadan parolalarını sıfırlamalarını sağlayan bir Azure Etkin Dizin (AD) özelliğidir. Kullanıcılar kendilerini hızlı bir şekilde kaldırabilir ve nerede olurlarsa olsunlar veya günün hangi saatinde olursa olsun çalışmaya devam edebilirler. Kuruluşunuzun, çalışanların kendilerinin engelini kaldırmalarına izin vererek, parolayla ilgili çoğu sorun için üretken olmayan süreyi ve yüksek destek maliyetlerini azaltabilir. 

SSPR aşağıdaki temel özelliklere sahiptir:

* Self servis, son kullanıcıların süresi dolmuş veya süresi dolmamış parolalarını bir yöneticiye veya destek için yardım masasına başvurmadan sıfırlamalarına olanak tanır.

* [Parola Yazma Geri Ödeme,](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback) şirket içi parolaların yönetimine ve buluta rağmen hesap kilitlemesinin çözülmesine olanak tanır.

* Parola yönetimi etkinlik raporları, yöneticilere kuruluşlarında gerçekleşen parola sıfırlama ve kayıt etkinliği hakkında bilgi verir.

## <a name="learn-about-sspr"></a>SSPR hakkında bilgi edinin

SSPR hakkında daha fazla bilgi edinin. [Bkz. Nasıl çalışır: Azure AD self servis parola sıfırlama.](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks)

### <a name="key-benefits"></a>Önemli avantajlar

SSPR'yi etkinleştirmenin temel avantajları şunlardır:

* **Maliyeti yönetin.** SSPR, kullanıcıların parolaları kendi başlarına sıfırlamalarını sağlayarak BT destek maliyetlerini azaltır. Ayrıca, kaybolan parolalar ve kilitlenmeler nedeniyle kaybedilen zaman maliyetini de azaltır. 

* **Sezgisel kullanıcı deneyimi.** Kullanıcıların parolaları sıfırlamasına ve herhangi bir cihazdan veya konumdan isteğe bağlı olarak hesapların engelini kaldırmasına olanak tanıyan sezgisel bir tek seferlik kullanıcı kaydı işlemi sağlar. SSPR, kullanıcıların daha hızlı çalışmaya ve daha üretken hale dönmelerine olanak tanır.

* **Esneklik ve güvenlik.** SSPR, işletmelerin bir bulut platformunun sağladığı güvenlik ve esnekliğe erişmesini sağlar. Yöneticiler, yeni güvenlik gereksinimlerini karşılamak için ayarları değiştirebilir ve bu değişiklikleri oturum açmalarını bozmadan kullanıcılara dağıtabilir.

* **Sağlam denetim ve kullanım izleme.** Kuruluş, kullanıcıları kendi parolalarını sıfırlarken iş sistemlerinin güvenli kalmasını sağlayabilir. Sağlam denetim günlükleri, parola sıfırlama işleminin her adımına ait bilgileri içerir. Bu günlükler bir API'den edinilebilir ve kullanıcının verileri tercih edilen bir Güvenlik Olayı ve Olay İzleme (SIEM) sistemine aktarmasını sağlar.

### <a name="licensing"></a>Lisanslama

Azure Active Directory, kullanıcı başına lisanslıdır, bu da her kullanıcının kullandığı özellikler için uygun bir lisansa ihtiyaç duyduğu anlamına gelir. SSPR için grup tabanlı lisanslama öneririz. 

Sürümleri ve özellikleri karşılaştırmak ve grup veya kullanıcı tabanlı lisanslamayı etkinleştirmek için Azure [AD self servis parola sıfırlama gereksinimlerine](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)bakın.

Fiyatlandırma hakkında daha fazla bilgi için [Azure Etkin Dizin fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)'na bakın.

### <a name="prerequisites"></a>Ön koşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim. Gerekirse, [ücretsiz bir oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.


### <a name="training-resources"></a>Eğitim kaynakları

| Kaynaklar| Bağlantı ve Açıklama |
| - | - |
| Videolar| [Kullanıcılarınızı daha iyi BT ölçeklenebilirliğiyle güçlendirin](https://youtu.be/g9RpRnylxS8) 
| |[Self servis parola sıfırlama nedir?](https://youtu.be/hc97Yx5PJiM)|
| |[Self servis parola sıfırlama dağıtma](https://www.youtube.com/watch?v=Pa0eyqjEjvQ&index=18&list=PLLasX02E8BPBm1xNMRdvP6GtA6otQUqp0)|
| |[Azure AD'deki kullanıcılar için self servis parola sıfırlama nasıl yapılandırılasınız?](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/) |
| |[Kullanıcıları Azure Etkin Dizini için [] güvenlik bilgilerini kaydetmeye hazırlama](https://youtu.be/gXuh0XS18wA) |
| Online kurslar|[Microsoft Azure Etkin Dizini'nde Kimlikleri Yönetme](https://www.pluralsight.com/courses/microsoft-azure-active-directory-managing-identities) Kullanıcılarınıza modern ve korumalı bir deneyim sunmak için SSPR'yi kullanın. Özellikle "[Azure Etkin Dizin Kullanıcıları ve Gruplarını Yönetme](https://app.pluralsight.com/library/courses/microsoft-azure-active-directory-managing-identities/table-of-contents)" modülüne bakın. |
|Çoğul görüşlü ücretli kurslar |[Kimlik ve Erişim Yönetimi Sorunları](https://www.pluralsight.com/courses/identity-access-management-issues) Kuruluşunuzdaki IAM ve güvenlik sorunları hakkında bilgi edinin. Özellikle "Diğer Kimlik Doğrulama Yöntemleri" modülüne bakın.|
| |[Microsoft Enterprise Mobility Suite ile başlarken](https://www.pluralsight.com/courses/microsoft-enterprise-mobility-suite-getting-started) Şirket içi varlıkları buluta kimlik doğrulama, yetkilendirme, şifreleme ve güvenli bir mobil deneyim sağlayacak şekilde genişletmeye yönelik en iyi uygulamaları öğrenin. Özellikle "Microsoft Azure Active Directory Premium'un Gelişmiş Özelliklerini Yapılandırma" modülüne bakın.
|Öğreticiler |[Azure AD self servis parola sıfırlama pilot unakaydetmeyi tamamlayın](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-pilot) |
| |[Parola yazma yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-writeback) |
| |[Windows 10 için giriş ekranından Azure AD parolası sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows) |
| SSS|[Parola yönetimi sık sorulan sorular](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-faq) |


### <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki örnekte, ortak karma ortamlar için parola sıfırlama çözüm mimarisi açıklanmaktadır.

![çözüm mimarisi diyagramı](./media/howto-sspr-deployment//solutions-architecture.png)

İş akışının açıklaması

Parolayı sıfırlamak için kullanıcılar [parola sıfırlama portalına](https://aka.ms/sspr)gider. Kimliklerini kanıtlamak için daha önce kaydedilmiş kimlik doğrulama yöntemini veya yöntemlerini doğrulamaları gerekir. Parolayı başarıyla sıyrıklarsa, sıfırlama işlemini başlatırlar.

* Yalnızca bulut kullanıcıları için SSPR yeni parolayı Azure AD'de depolar. 

* Karma kullanıcılar için SSPR, Azure AD Connect hizmeti aracılığıyla ön m'deki Active Directory'nin parolasını geri yazar. 

Not: [Parola karma senkronizasyonu (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) devre dışı bırakılan kullanıcılar için SSPR parolaları yalnızca ön m'deki Active Directory'de depolar.

### <a name="best-practices"></a>En iyi uygulamalar

SSPR'yi kuruluştaki başka bir popüler uygulama veya hizmetin yanında dağıtarak kullanıcıların hızlı bir şekilde kaydolmasına yardımcı olabilirsiniz. Bu eylem, büyük miktarda oturum açma sağlar ve kaydı yönlendirecektir.

SSPR'yi dağıtmadan önce, her parola sıfırlama çağrısının sayısını ve ortalama maliyetini belirlemeyi tercih edebilirsiniz. SSPR'nin kuruluşa getirdiği değeri göstermek için bu veri sonrası dağıtımı kullanabilirsiniz.

#### <a name="enable-combined-registration-for-sspr-and-mfa"></a>SSPR ve MFA için kombine kaydı etkinleştirme

Microsoft, kuruluşların SSPR ve çok faktörlü kimlik doğrulama için birleştirilmiş kayıt deneyimini etkinleştirmesini önerir. Bu birleştirilmiş kayıt deneyimini etkinleştirdiğinizde, kullanıcıların her iki özelliği de etkinleştirmek için kayıt bilgilerini yalnızca bir kez seçmeleri gerekir.

Birleştirilmiş kayıt deneyimi, kuruluşların hem SSPR hem de Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmesini gerektirmez. Birleşik kayıt, kuruluşlara daha iyi bir kullanıcı deneyimi sağlar. Daha fazla bilgi için [bkz.](concept-registration-mfa-sspr-combined.md)

## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlama

Ortamınızda bu dağıtım için strateji belirlerken kuruluş ihtiyaçlarınızı göz önünde bulundurun.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuçlar ve sorumluluklar hakkındaki uyumsuz beklentiler nedeniyle bunu yaparlar. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan](https://aka.ms/deploymentplans) ve projedeki paydaş rollerinin paydaşları ve bunların proje girdilerini ve hesap yeteneklerini belgeleyerek iyi anlaşıldığından emin olun.

#### <a name="required-administrator-roles"></a>Gerekli yönetici rolleri


| İş Rolü/Kişiliği| Azure AD Rolü (gerekirse) |
| - | - |
| Seviye 1 yardım masası| Parola yöneticisi |
| Düzey 2 yardım masası| Kullanıcı yöneticisi |
| SSPR yöneticisi| Genel yönetici |


### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcılarınızla deneyimlerinin nasıl değişeceğini, ne zaman değişeceğini ve sorunlarla karşılaşırsa nasıl destek kazanabileceğinizi proaktif bir şekilde iletmelisiniz. Son kullanıcı iletişim stratejinizi nasıl planlayasınız hakkında fikirler için Microsoft indirme merkezinde Self servis parola sıfırlama kullanıma hazır kullanıma [alma materyallerini](https://www.microsoft.com/download/details.aspx?id=56768) inceleyin.

### <a name="plan-a-pilot"></a>Bir pilot planlayın

SSPR'nin ilk yapılandırmasının bir test ortamında olmasını öneririz. Kuruluşunuzdaki bir kullanıcı alt kümesi için SSPR'yi etkinleştirerek bir pilot grupla başlayın. [Bir pilot için en iyi uygulamalara](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans)bakın.

Bir grup oluşturmak için, [bir grup oluşturma ve Azure Etkin Dizini'ne nasıl üye ekleyiş](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal)gerektiğini görün. 

## <a name="plan-configuration"></a>Yapılandırmayı planla

Önerilen değerlerle birlikte SSPR'yi etkinleştirmek için aşağıdaki ayarlar gereklidir.

| Alan | Ayar | Değer |
| --- | --- | --- |
| **SSPR Özellikleri** | Self servis parola sıfırlama etkin | Pilot için **seçilen** grup / **Tüm** üretim için |
| **Kimlik doğrulama yöntemleri** | Kayıt için gerekli kimlik doğrulama yöntemleri | Sıfırlama için gerekenden her zaman 1 fazla |
|   | Sıfırlamak için gereken kimlik doğrulama yöntemleri | Bir ya da iki |
| **Kayıt** | Kullanıcılardan oturum açarken kaydolmalarını iste | Evet |
|   | Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı | 90 - 180 gün |
| **Bildirimler** | Parola sıfırlamayı kullanıcılara bildir | Evet |
|   | Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir | Evet |
| **Özelleştirme** | Yardım masası bağlantısını özelleştirme | Evet |
|   | Özel yardım masası e-postası veya URL | Destek sitesi veya e-posta adresi |
| **Şirket içi tümleştirme** | Şirket içi AD'ye parolaları geri yazma | Evet |
|   | Parolayı sıfırlamadan kullanıcıların hesap kilidini açmasına izin verme | Evet |

### <a name="sspr-properties"></a>SSPR özellikleri

SSPR'yi etkinleştirirken, pilot ortamda uygun bir güvenlik grubu seçin.

* Herkes için SSPR kaydını uygulamak için **Tümü** seçeneğini kullanmanızı öneririz.
* Aksi takdirde, uygun Azure AD veya AD güvenlik grubunu seçin.

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinleştirildiğinde, kullanıcılar parolalarını yalnızca yöneticinin etkinleştirdiği kimlik doğrulama yöntemlerinde veri varsa sıfırlayabilirler. Yöntemler arasında telefon, Kimlik Doğrulaması uygulaması bildirimi, güvenlik soruları vb. yer alır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods)

Aşağıdaki kimlik doğrulama yöntemi ayarlarını öneririz:

* **Kaydolmak için gereken Kimlik Doğrulama yöntemlerini** sıfırlamak için gereken sayıdan en az bir daha fazlasına ayarlayın. Birden çok kimlik doğrulamasına izin vermek, kullanıcılara sıfırlamaları gerektiğinde esneklik sağlar.

* Kuruluşunuza uygun bir düzeye **sıfırlamak için gereken yöntem sayısını** ayarlayın. Biri en az sürtünme gerektirir, iki güvenlik duruşu artırabilir ise. 

Not: Kullanıcı, [Azure Etkin Dizini'ndeki Parola ilkeleri nde ve kısıtlamalarında](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy)yapılandırılan kimlik doğrulama yöntemlerine sahip olmalıdır.

### <a name="registration-settings"></a>Kayıt ayarları

Ayar **Evet'te oturum açmak için kullanıcıların kaydolmasını zorunlu kılmasını** gerektirin. **Yes** Bu ayar, kullanıcıların oturum verirken kaydolmasını ve tüm kullanıcıların korunmasını sağlamasını gerektirir.

Kuruluşunuzun daha kısa bir zaman dilimi için bir iş gereksinimi yoksa, kullanıcılardan kimlik doğrulama bilgilerini **90** ile **180** gün arasında **yeniden onaylamaları istenmeden önceki gün sayısını** ayarlayın.

### <a name="notifications-settings"></a>Bildirimler ayarları

Diğer yöneticiler parolalarını **Evet**olarak sıfırladığında hem **kullanıcıları parola sıfırlamada notify** hem de tüm yöneticileri **bildir'i** yapılandırın. Her ikide de **Evet'i** seçmek, kullanıcıların parolaları sıfırlandığında haberdar olmalarını sağlayarak güvenliği artırır. Ayrıca, bir yönetici parolayı değiştirdiğinde tüm yöneticilerin farkında olmasını sağlar. Kullanıcılar veya yöneticiler bir bildirim alırsa ve değişikliği başlatmamışlarsa, olası bir güvenlik sorununu hemen bildirebilirler.

### <a name="customization-settings"></a>Özelleştirme ayarları

Sorunla karşılaşan kullanıcıların hemen yardım alabilmesi için yardım masası e-postasını veya URL'sini özelleştirmek çok önemlidir. Bu seçeneği, kullanıcılarınızın aşina olduğu ortak bir yardım masası e-posta adresine veya web sayfasına ayarlayın. 

Daha fazla bilgi için, [self servis parola sıfırlama için Azure AD işlevini özelleştir'e](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-customization)bakın.

### <a name="password-writeback"></a>Şifre Yazma

**Parola Yazma,** [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity) ile etkinleştirilir ve buluttaki parola sıfırlamalarını gerçek zamanlı olarak varolan şirket içi dizine geri yazar. Daha fazla bilgi için [bkz: Password Writeback nedir?](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback)

Aşağıdaki ayarları öneririz:

* Şirket **içi AD'ye parola yazmanın** **Evet**olarak ayarlandığından emin olun. 
* Kullanıcıların **parolayı Evet'e sıfırlamadan hesabın kilidini açmalarına izin** verme'yi ayarlayın. **Yes**

Varsayılan olarak, Azure AD parola sıfırlama gerçekleştirirken hesapların kilidini açar.

### <a name="administrator-password-setting"></a>Yönetici parola ayarı

Yönetici hesaplarının yüksek izinleri vardır. Şirket içi kuruluş veya etki alanı yöneticileri parolalarını SSPR aracılığıyla sıfırlayamamaktadır. Şirket içi yönetici hesaplarıaşağıdaki kısıtlamalara sahiptir:

* parolalarını yalnızca prem ortamlarında değiştirebilirler.
* parolalarını sıfırlamak için gizli soruları ve yanıtları hiçbir zaman bir yöntem olarak kullanamazsınız.

Prem Active Directory yönetici hesaplarınızı Azure AD ile senkronize etmemenizi öneririz.

### <a name="environments-with-multiple-identity-management-systems"></a>Birden fazla kimlik yönetim sistemine sahip ortamlar

Bazı ortamlarda birden çok kimlik yönetim sistemi vardır. Oracle ve SiteMinder gibi on-premesis kimlik yöneticileri, parolalar için AD ile senkronizasyon gerektirir. Bunu, Microsoft Identity Manager (MIM) ile Parola Değişikliği Bildirim Hizmeti (PCNS) gibi bir aracı kullanarak yapabilirsiniz. Bu daha karmaşık senaryo hakkında bilgi bulmak için, bir [etki alanı denetleyicisi üzerinde MIM Şifre Değişikliği Bildirim Hizmeti dağıtın](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller)makalebakın.

## <a name="plan-testing-and-support"></a>Plan Test ve Destek

İlk pilot gruplardan kuruluş genelinde dağıtımınızın her aşamasında, sonuçların beklendiği gibi olduğundan emin olun.

### <a name="plan-testing"></a>Planı testi

Dağıtımınızın beklendiği gibi çalıştığından emin olmak için, uygulamayı doğrulamak için bir dizi test çalışması planlayın. Test servis taleplerini değerlendirmek için, parolası olan yönetici olmayan bir test kullanıcısına ihtiyacınız vardır. Bir kullanıcı oluşturmanız gerekiyorsa, bkz. [Azure Etkin Dizin'e yeni kullanıcılar ekle.](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)

Aşağıdaki tablo, kuruluşlarınızın beklenen sonuçları ilkelerinize göre belgelemek için kullanabileceğiniz yararlı test senaryolarını içerir.
<br>


| İş durumu| Beklenen sonuçlar |
| - | - |
| SSPR portalına kurumsal ağ içinden erişilebilir| Kuruluşunuz tarafından belirlenir |
| SSPR portalına şirket ağı dışından erişilebilir| Kuruluşunuz tarafından belirlenir |
| Kullanıcı parola sıfırlama için etkinleştirildiğinde kullanıcı parolasını tarayıcıdan sıfırlama| Kullanıcı parola sıfırlama akışına erişemez |
| Kullanıcı parola sıfırlama için kaydolmadıklarında kullanıcı parolayı tarayıcıdan sıfırlama| Kullanıcı parola sıfırlama akışına erişemez |
| Parola sıfırlama kaydı yapmak için zorunlu kılındığında kullanıcı girişleri| Kullanıcıdan güvenlik bilgilerini kaydetmesini ister |
| Parola sıfırlama kaydı tamamlandığında kullanıcı girişleri| Kullanıcıdan güvenlik bilgilerini kaydetmesini ister |
| Kullanıcının lisansı olmadığında SSPR portalına erişilebilir| Erişilebilir mi |
| Windows 10 Azure AD'den kullanıcı parolası sıfırlama veya karma Azure AD'ın aygıt kilidi ekranına katılması| Kullanıcı parolayı sıfırlayabilir |
| SSPR kayıt ve kullanım verileri yöneticilerin neredeyse gerçek zamanlı olarak kullanabileceği| Denetim günlükleri aracılığıyla kullanılabilir |

Ayrıca, Azure [AD self servis parola sıfırlama pilot rulosu](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-pilot)tamamla'ya da bakabilirsiniz. Bu eğitimde, kuruluşunuzdaki SSPR'den pilot bir şekilde kaydedilmesini ve yönetici olmayan bir hesabı kullanarak test edilmesini sağlayacaksınız.

### <a name="plan-support"></a>Planı destek

SSPR genellikle kullanıcı sorunları oluşturmaz, ancak ortaya çıkabilecek sorunlarla başa çıkmak için destek personeli hazırlamak önemlidir. Bir yönetici Azure AD portalı aracılığıyla son kullanıcılar için parolayı sıfırlayabilir, ancak bir self servis destek işlemi yle sorunun giderilmesine yardımcı olmak daha iyidir.

Destek ekibinizin başarısını sağlamak için, kullanıcılarınızdan aldığınız soruları temel alan bir SSS oluşturabilirsiniz. İşte birkaç örnek:

| Senaryolar| Açıklama |
| - | - |
| Kullanıcının kullanılabilir kayıtlı kimlik doğrulama yöntemleri yok| Bir kullanıcı parolasını sıfırlamaya çalışıyor, ancak mevcut olarak kaydettiği kimlik doğrulama yöntemlerinden hiçbirine sahip değil (Örnek: cep telefonunu evde bıraktılar ve e-postalara erişemiyor) |
| Kullanıcı, ofisinden veya cep telefonunda bir metin veya arama almıyor| Kullanıcı metin veya arama yoluyla kimliğini doğrulamaya çalışır, ancak bir metin/çağrı almıyor. |
| Kullanıcı parola sıfırlama portalına erişemez| Bir kullanıcı parolasını sıfırlamak istiyor, ancak parola sıfırlama için etkin değil ve parolaları güncelleştirmek için sayfaya erişemez. |
| Kullanıcı yeni bir parola ayarlayamıyor| Kullanıcı parola sıfırlama akışı sırasında doğrulamayı tamamlar, ancak yeni bir parola ayarlayamaz. |
| Kullanıcı, Windows 10 aygıtında Parolayı Sıfırla bağlantısını görmüyor| Bir kullanıcı Windows 10 kilit ekranından parolayı sıfırlamaya çalışıyor, ancak aygıt Azure AD'ye katılmıyor veya Intune aygıt ilkesi etkinleştirildi |

### <a name="plan-roll-back"></a>Geri alma planı

Dağıtımı geri almak için:

* tek bir kullanıcı için, kullanıcıyı güvenlik grubundan kaldırın 

* bir grup için, grubu SSPR yapılandırmasından kaldırın

* Azure AD kiracısı için SSPR'yi herkes devre dışı

## <a name="deploy-sspr"></a>SSPR dağıtma

Dağıtmadan önce aşağıdakileri yaptığınızdan emin olun:

1. [İletişim planınızı](#plan-communications)oluşturdu ve yürütmeye başladı.

1. Uygun [yapılandırma ayarlarını](#plan-configuration)belirledi.

1. [Pilot](#plan-a-pilot) ve üretim ortamları için kullanıcı ve grupları belirletir.

1. Kayıt ve self servis için [kararlı yapılandırma ayarları.](#plan-configuration)

1. Karma bir ortamınız [varsa, yapılandırılan parola yazma.](#password-writeback)


**Artık SSPR'yi dağıtmaya hazırsınız!**

Bkz. Aşağıdaki alanların yapılandırılmasıyla ilgili adım adım yol tarifleriiçin [self servis parola sıfırlamayı etkinleştir.](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-pilot#enable-self-service-password-reset)

1. [Kimlik doğrulama yöntemleri](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods)

1. [Kayıt ayarları](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined)

1. [Bildirimler ayarları](#notifications-settings)

1. [Özelleştirme ayarları](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-customization)

1. [Şirket içi tümleştirme](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-writeback)

### <a name="enable-sspr-in-windows"></a>Windows'da SSPR'yi etkinleştirme
Windows 7, 8, 8.1 ve 10 çalıştıran makinelerde [kullanıcıların Windows oturum açma ekranında parolalarını sıfırlamalarını sağlayabilirsiniz](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-windows)

## <a name="manage-sspr"></a>SSPR'yi yönet

Azure AD, denetimler ve raporlar aracılığıyla SSPR performansınız hakkında ek bilgiler sağlayabilir.

### <a name="password-management-activity-reports"></a>Parola yönetimi etkinlik raporları 

SSPR performansını ölçmek için Azure portalında önceden oluşturulmuş raporları kullanabilirsiniz. Uygun şekilde lisansınız varsa, özel sorgular da oluşturabilirsiniz. Daha fazla bilgi için Azure [AD parola yönetimi için Raporlama seçeneklerine](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-reporting) bakın

> [!NOTE]
>  [Genel bir yönetici](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)olmalısınız ve bu verilerin kuruluşunuz için toplandığını kabul etmeniz gerekir. Oturum açmak için, Azure Portalı'ndaki Raporlama sekmesini veya denetim günlüklerini en az bir kez ziyaret etmelisiniz. O zamana kadar, veriler kuruluşunuz için toplanmaz.

Kayıt ve parola sıfırlama için denetim günlükleri 30 gün boyunca kullanılabilir. Şirketinizdeki güvenlik denetimi daha uzun süre bekletilmesi gerektiriyorsa, günlüklerin dışa aktarılması ve [Azure Sentinel,](https://docs.microsoft.com/azure/sentinel/connect-azure-active-directory)Splunk veya ArcSight gibi bir SIEM aracında tüketilmesi gerekir.

![SSPR Raporlama ekran görüntüsü](./media/howto-sspr-deployment/sspr-reporting.png)

### <a name="authentication-methods--usage-and-insights"></a>Kimlik doğrulama yöntemleri- Kullanım ve Öngörüler

[Kullanım ve öngörüler,](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights) Azure MFA ve SSPR gibi özelliklerin kimlik doğrulama yöntemlerinin kuruluşunuzda nasıl çalıştığını anlamanızı sağlar. Bu raporlama özelliği, kuruluşunuza hangi yöntemlerin kaydolduğunu ve bunları nasıl kullanacağınızı anlama olanağı sağlar.

### <a name="troubleshoot"></a>Sorun giderme

* Sorun [Giderme self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-troubleshoot) 

* [Parola yönetimini sık sorulan soruları](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-faq) takip edin 

### <a name="helpful-documentation"></a>Yararlı belgeler

* [Kimlik doğrulaması yöntemleri nelerdir?](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods)

* [Nasıl çalışır: Azure AD self servis parola sıfırlama?](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks)

* [Self servis parola sıfırlama için Azure AD işlevini özelleştirme](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-customization)

* [Azure Active Directory'deki parola ilkeleri ve kısıtlamaları](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy)

* [Parola yazma nedir?](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback)

## <a name="next-steps"></a>Sonraki adımlar

* SSPR'yi dağıtmaya başlamak için Azure [AD self servis parola sıfırlama pilot una](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-pilot) bürünme

* [Azure AD parola koruması uygulamayı düşünün](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad)

* [Azure AD Akıllı Kilitleme'yi uygulamayı düşünün](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout)