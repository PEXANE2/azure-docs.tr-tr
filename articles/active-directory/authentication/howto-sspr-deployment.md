---
title: Azure Active Directory self servis parola sıfırlama için dağıtım konuları
description: Azure AD self servis parola sıfırlama işleminin başarılı uygulanması için dağıtım konuları ve stratejileri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: baselden
author: barbaraselden
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9a8f572189afaa726f7e01f5e0bbb73340face8
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657226"
---
# <a name="plan-an-azure-active-directory-self-service-password-reset-deployment"></a>Azure Active Directory self servis parola sıfırlama dağıtımını planlayın

> [!IMPORTANT]
> Bu dağıtım planı, Azure AD self servis parola sıfırlama (SSPR) dağıtımı için rehberlik ve en iyi yöntemleri sunar.
>
> **Son bir Kullanıcı kullanıyorsanız ve hesabınıza geri [https://aka.ms/sspr](https://aka.ms/sspr) **dönmek istiyorsanız adresine gidin.

[Self servis parola sıfırlama (SSPR)](https://www.youtube.com/watch?v=tnb2Qf4hTP8) , kullanıcıların yardım için BT personeline başvurmadan parolalarını sıfırlamalarını sağlayan bir Azure ACTIVE DIRECTORY (ad) özelliğidir. Kullanıcılar kendi kendilerini engellemeyi kaldırabilir ve nerede olurlarsa veya gün içinde çalışmaya devam edebilir. Kuruluşunuz, çalışanların kendi engellemesini engellemelerine izin vererek, en yaygın parolayla ilgili sorunlara yönelik üretken olmayan süreyi ve yüksek destek maliyetlerini azaltabilir.

SSPR aşağıdaki temel yeteneklere sahiptir:

* Self servis, son kullanıcıların destek için yönetici veya yardım masası ile iletişime geçmeksizin, sona erdikleri veya süre dolmayan parolalarını sıfırlamasına olanak sağlar.
* [Parola geri yazma](./concept-sspr-writeback.md) , şirket içi parolaların yönetimine ve bulut olmasına rağmen hesap kilitlemenizin çözümlenmesine izin verir.
* Parola yönetimi etkinlik raporları yöneticilere, kuruluşunuzda gerçekleşen parola sıfırlama ve kayıt etkinliği hakkında bilgi verir.

Bu dağıtım kılavuzu, bir SSPR dağıtımını nasıl planlayıp test etmek istediğinizi gösterir.

SSPR 'yi eylemde hızlıca görmek ve daha sonra ek dağıtım konularını anlamak için geri dönüp:

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı etkinleştir (SSPR)](tutorial-enable-sspr.md)

## <a name="learn-about-sspr"></a>SSPR hakkında bilgi edinin

SSPR hakkında daha fazla bilgi edinin. [Nasıl çalıştığını görün: Azure AD self servis parola sıfırlama](./concept-sspr-howitworks.md).

### <a name="key-benefits"></a>Önemli avantajlar

SSPR 'yi etkinleştirmenin başlıca avantajları şunlardır:

* **Maliyeti yönetin**. SSPR, kullanıcıların kendi parolalarını sıfırlamalarını sağlayarak BT destek maliyetlerini azaltır. Ayrıca kayıp parolalar ve kilitleme işlemleri nedeniyle kaybedilen sürenin maliyetini azaltır. 

* **Sezgisel kullanıcı deneyimi**. Kullanıcıların parolaları sıfırlamasına ve cihazların herhangi bir cihazdan veya konumdan talep üzerine engellenmesini sağlayan, sezgisel bir kerelik Kullanıcı kayıt işlemi sağlar. SSPR kullanıcıların daha hızlı çalışmaya geri almasına ve daha üretken olmasına olanak tanır.

* **Esneklik ve güvenlik**. SSPR, kuruluşların bir bulut platformunun sağladığı güvenliğe ve esnekliğe erişmesini sağlar. Yöneticiler, ayarları yeni güvenlik gereksinimlerine uyum sağlayacak şekilde değiştirebilir ve bu değişiklikleri kullanıcıların oturum açma durumlarını kesintiye uğratmadan kullanıcılara alabilir.

* **Sağlam denetim ve kullanım izleme**. Bir kuruluş, kullanıcıları kendi parolalarını sıfırlarken iş sistemlerinin güvende kalmasını sağlayabilir. Sağlam denetim günlükleri, parola sıfırlama işleminin her bir adımının bilgilerini içerir. Bu günlükler bir API 'den kullanılabilir ve kullanıcının verileri bir güvenlik olayı ve olay Izleme (SıEM) sistemine aktarmasını sağlar.

### <a name="licensing"></a>Lisanslama

Azure Active Directory, Kullanıcı başına lisanslanır, her kullanıcının kullandıkları özellikler için uygun bir lisans gerekir. SSPR için grup tabanlı lisans öneririz. 

Sürümleri ve özellikleri karşılaştırmak ve grup veya Kullanıcı tabanlı lisanslamayı etkinleştirmek için bkz. [Azure AD self servis parola sıfırlama Için lisans gereksinimleri](./concept-sspr-licensing.md).

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="prerequisites"></a>Ön koşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim. Gerekirse, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.


### <a name="training-resources"></a>Eğitim kaynakları

| Kaynaklar| Bağlantı ve açıklama |
| - | - |
| Videolar| [Daha iyi BT ölçeklenebilirliği sayesinde kullanıcılarınıza güç katın](https://youtu.be/g9RpRnylxS8) 
| |[Self servis parola sıfırlama nedir?](https://youtu.be/hc97Yx5PJiM)|
| |[Self servis parola sıfırlama dağıtımı](https://www.youtube.com/watch?v=Pa0eyqjEjvQ&index=18&list=PLLasX02E8BPBm1xNMRdvP6GtA6otQUqp0)|
| |[Azure AD 'de kullanıcılar için self servis parola sıfırlama nasıl yapılandırılır?](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/) |
| |[[Kullanıcıların, Azure Active Directory için güvenlik bilgilerini kaydetmesi için [kullanıcıları hazırlama]](https://youtu.be/gXuh0XS18wA) |
| Çevrimiçi kurslar|[Microsoft Azure Active Directory kimlikleri yönetme](https://www.pluralsight.com/courses/microsoft-azure-active-directory-managing-identities) Kullanıcılarınıza modern ve korumalı bir deneyim sunmak için SSPR 'yi kullanın. Özellikle "[Azure Active Directory Kullanıcıları ve grupları yönetme](https://app.pluralsight.com/library/courses/microsoft-azure-active-directory-managing-identities/table-of-contents)" modülünü inceleyin. |
|Pluralaltim ücretli kurslar |[Kimlik ve erişim yönetimi sorunları](https://www.pluralsight.com/courses/identity-access-management-issues) Kuruluşunuzda bilmeniz için ıAM ve güvenlik sorunları hakkında bilgi edinin. Özellikle "diğer kimlik doğrulama yöntemleri" modülüne bakın.|
| |[Microsoft Enterprise Mobility Suite Ile çalışmaya](https://www.pluralsight.com/courses/microsoft-enterprise-mobility-suite-getting-started) başlama Şirket içi varlıkları, kimlik doğrulama, yetkilendirme, şifreleme ve güvenli bir mobil deneyimle izin verecek şekilde buluta genişletmeye yönelik en iyi yöntemleri öğrenin. Özellikle, "Microsoft Azure Active Directory Premium gelişmiş özelliklerini yapılandırma" modülüne bakın.
|Öğreticiler |[Bir Azure AD self servis parola sıfırlama pilot kullanıma alma](./tutorial-enable-sspr.md) |
| |[Parola geri yazma etkinleştiriliyor](./tutorial-enable-sspr-writeback.md) |
| |[Windows 10 için oturum açma ekranından Azure AD parola sıfırlama](./howto-sspr-windows.md) |
| SSS|[Parola yönetimi hakkında sık sorulan sorular](./active-directory-passwords-faq.md) |


### <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki örnek, yaygın karma ortamlar için parola sıfırlama çözüm mimarisini açıklar.

![Çözüm mimarisi diyagramı](./media/howto-sspr-deployment//solutions-architecture.png)

İş akışının açıklaması

Kullanıcılar parolayı sıfırlamak için [parola sıfırlama portalına](https://aka.ms/sspr)gider. Kimliklerini kanıtlamak için önceden kaydedilmiş kimlik doğrulama yöntemini veya yöntemlerini doğrulamaları gerekir. Parolayı başarıyla sıfırlarsa, sıfırlama işlemine başlarlar.

* Yalnızca bulutta bulunan kullanıcılar için SSPR yeni parolayı Azure AD 'de depolar. 

* Karma kullanıcılar için SSPR, Azure AD Connect hizmeti aracılığıyla parolayı on-Prem Active Directory geri yazar. 

Note: [Parola karması eşitlemesi (PHS)](../hybrid/whatis-phs.md) devre dışı bırakılmış kullanıcılar IÇIN, SSPR parolaları yalnızca şirket içi Active Directory depolar.

### <a name="best-practices"></a>En iyi uygulamalar

Kullanıcıların, kuruluştaki başka bir popüler uygulama veya hizmetle birlikte SSPR 'yi dağıtarak hızla kaydolmaya yardımcı olabilirsiniz. Bu eylem, büyük bir oturum açma hacmi oluşturur ve kayıt işlemini yeniden dener.

SSPR 'yi dağıtılmadan önce, her parola sıfırlama çağrısının numarasını ve ortalama maliyetini belirlemeyi tercih edebilirsiniz. Bu veri gönderi dağıtımını, SSPR 'nin kuruluşa göre göstereceği değeri göstermek için kullanabilirsiniz.

#### <a name="enable-combined-registration-for-sspr-and-mfa"></a>SSPR ve MFA için Birleşik kaydı etkinleştir

Microsoft, kuruluşların SSPR ve Multi-Factor Authentication için Birleşik kayıt deneyimini etkinleştirmesini önerir. Bu Birleşik kayıt deneyimini etkinleştirdiğinizde, kullanıcıların her iki özelliği de etkinleştirmek için kayıt bilgilerini yalnızca bir kez seçmesini gerekir.

Birleşik kayıt deneyimi, kuruluşların hem SSPR hem de Azure Multi-Factor Authentication etkinleştirmesini gerektirmez. Birleşik kayıt, kuruluşlara daha iyi bir kullanıcı deneyimi sağlar. Daha fazla bilgi için bkz. [Birleşik güvenlik bilgileri kaydı](concept-registration-mfa-sspr-combined.md)

## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlayın

Ortamınızda bu dağıtımın stratejisini belirlerken kurumsal ihtiyaçlarınızı göz önünde bulundurun.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlara katılın

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuç ve sorumlulukların eşleşmeyen beklentileri nedeniyle bu, genellikle bu şekilde yapılır. Bu girişlerin oluşmasını önlemek için, [doğru paydaşları](https://aka.ms/deploymentplans) ve proje giriş ve accountatçlarını belgeleyerek projedeki paydaş rollerinin iyi anlaşıldığından emin olun.

#### <a name="required-administrator-roles"></a>Gerekli yönetici rolleri


| İş rolü/kişi| Azure AD rolü (gerekirse) |
| - | - |
| Düzey 1 yardım masası| Parola yöneticisi |
| Düzey 2 yardım masası| Kullanıcı yöneticisi |
| SSPR Yöneticisi| Genel yönetici |


### <a name="plan-communications"></a>İletişimi planlama

İletişim, her yeni hizmetin başarısı için önemlidir. Deneyimlerinizin nasıl değiştirileceği, ne zaman değiştirileceği ve sorunlarla karşılaştıklarında nasıl destek kazanabilecekleri konusunda, kullanıcılarınız ile önceden iletişim kurması gerekir. Son Kullanıcı iletişim stratejinizi nasıl planlayabileceğiniz hakkında fikir edinmek için [Microsoft İndirme Merkezi ' nde self servis parola sıfırlama dağıtım malzemelerini](https://www.microsoft.com/download/details.aspx?id=56768) gözden geçirin.

### <a name="plan-a-pilot"></a>Pilot planı planlayın

SSPR 'nin ilk yapılandırmasının bir test ortamında olması önerilir. Kuruluşunuzdaki kullanıcıların bir alt kümesi için SSPR 'yi etkinleştirerek bir pilot grubu ile başlayın. [Pilot Için en iyi uygulamalar](../fundamentals/active-directory-deployment-plans.md)bölümüne bakın.

Bir grup oluşturmak için, bkz. [Grup oluşturma ve Azure Active Directory üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md). 

## <a name="plan-configuration"></a>Planı yapılandırma

SSPR 'yi önerilen değerlerle birlikte etkinleştirmek için aşağıdaki ayarlar gereklidir.

| Alan | Ayar | Değer |
| --- | --- | --- |
| **SSPR özellikleri** | Self servis parola sıfırlama etkin | Pilot için **Seçili** grup ve **üretim için** |
| **Kimlik doğrulama yöntemleri** | Kaydolmak için gereken kimlik doğrulama yöntemleri | Sıfırlama için her zaman 1 daha fazla |
|   | Sıfırlamak için gereken kimlik doğrulama yöntemleri | Bir veya iki |
| **Kaydını** | Kullanıcılardan oturum açarken kaydolmalarını iste | Yes |
|   | Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı | 90 – 180 gün |
| **Bildirimler** | Parola sıfırlamayı kullanıcılara bildir | Yes |
|   | Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir | Yes |
| **Özelleştirme** | Yardım masası bağlantısını Özelleştir | Yes |
|   | Özel yardım masası e-postası veya URL 'SI | Destek sitesi veya e-posta adresi |
| **Şirket içi tümleştirme** | Parolaları şirket içi AD 'ye geri yazma | Yes |
|   | Kullanıcıların, parolayı sıfırlamadan hesabın kilidini açma izni ver | Yes |

### <a name="sspr-properties"></a>SSPR özellikleri

SSPR 'yi etkinleştirirken, pilot ortamda uygun bir güvenlik grubu seçin.

* Herkes için SSPR kaydını zorlamak için, **All** seçeneğini kullanmanızı öneririz.
* Aksi takdirde, uygun Azure AD veya AD güvenlik grubunu seçin.

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinleştirildiğinde, kullanıcılar yalnızca yöneticinin etkinleştirildiği kimlik doğrulama yöntemlerinde verileri varsa parolalarını sıfırlayabilir. Telefon, kimlik doğrulayıcı uygulama bildirimi, güvenlik soruları vb. yöntemler içerir. Daha fazla bilgi için bkz. [kimlik doğrulama yöntemleri nedir?](./concept-authentication-methods.md).

Aşağıdaki kimlik doğrulama yöntemi ayarlarını yapmanızı öneririz:

* Sıfırlamak için gereken sayıdan en az bir daha fazlasına **kaydolmak için gereken kimlik doğrulama yöntemlerini** ayarlayın. Birden çok kimlik doğrulama izni verilmesi, kullanıcıların sıfırlanması gerektiğinde esneklik sağlar.

* Kuruluşunuza uygun bir düzeye **sıfırlamak için gereken yöntem sayısını** ayarlayın. Biri en az artışla ihtiyaç duyurken, ikisi de güvenlik duruşunuzu artırabilir. 

Note: kullanıcının kimlik doğrulama yöntemlerinin [Azure Active Directory parola ilkelerinde ve kısıtlamalarında](./concept-sspr-policy.md)yapılandırılmış olması gerekir.

### <a name="registration-settings"></a>Kayıt ayarları

Kullanıcıların **Evet** **'de oturum açarken kaydolmasını gerektir** ayarını belirleyin. Bu ayar, kullanıcıların oturum açarken kaydolmalarını gerektirir, tüm kullanıcıların korunmasını sağlar.

Kuruluşunuzun daha kısa bir zaman dilimine yönelik bir iş ihtiyacı yoksa, kullanıcılardan kimlik doğrulama bilgilerini **90** ile **180** gün arasında **yeniden onaylamasını istemeden önce geçen gün sayısını** ayarlayın.

### <a name="notifications-settings"></a>Bildirim ayarları

**Parola sıfırlamaları için kullanıcılara bildir** ' i ve **diğer yöneticiler parolasını Evet olarak sıfırladığında tüm yöneticilere bildir** ' **Yes**i yapılandırın. Her ikisinde de **Evet** ' i seçmek, kullanıcıların parolası sıfırlandığında haberdar olmanızı sağlayarak güvenliği artırır. Ayrıca, bir yönetici bir parolayı değiştirdiğinde tüm yöneticilerin farkında olmasını sağlar. Kullanıcılar veya Yöneticiler bir bildirim alıyorsa ve değişikliği başlatmadıklarında, olası bir güvenlik sorununu anında raporlayabilir.

### <a name="customization-settings"></a>Özelleştirme ayarları

Sorun yaşayan kullanıcıların yardım almak için yardım masasına e-posta veya URL 'YI özelleştirmek kritik öneme sahiptir. Bu seçeneği, kullanıcılarınızın tanıdık olduğu ortak bir yardım masası e-posta adresi veya Web sayfası olarak ayarlayın. 

Daha fazla bilgi için bkz. [self servis parola sıfırlama Için Azure AD Işlevselliğini özelleştirme](./howto-sspr-customization.md).

### <a name="password-writeback"></a>Parola geri yazma

**Parola geri yazma** özelliği, [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) ile etkinleştirilir ve bulutta parola sıfırlamaları gerçek zamanlı olarak mevcut bir şirket içi dizine geri gönderilir. Daha fazla bilgi için bkz. [parola geri yazma nedir?](./concept-sspr-writeback.md)

Aşağıdaki ayarları öneririz:

* Şirket **ıçı ad 'ye geri yazma parolalarının** **Evet**olarak ayarlandığından emin olun. 
* **Parolayı sıfırlamadan kullanıcıların hesabı kilitlemeden Izin ver** ' i **Evet**olarak ayarlayın.

Varsayılan olarak, Azure AD, parola sıfırlama gerçekleştirdiğinde hesapların kilidini açar.

### <a name="administrator-password-setting"></a>Yönetici parolası ayarı

Yönetici hesaplarının izinleri yükseltilmiş. Şirket içi kuruluş veya etki alanı yöneticileri, SSPR üzerinden parolalarını sıfırlayamaz. Şirket içi yönetici hesaplarında aşağıdaki kısıtlamalar vardır:

* yalnızca kendi şirket içi ortamında parolalarını değiştirebilir.
* gizli soruları ve yanıtları hiçbir şekilde, parolasını sıfırlamak için bir yöntem olarak kullanamaz.

Şirket içi Active Directory yönetici hesaplarınızı Azure AD ile eşitmemenizi öneririz.

### <a name="environments-with-multiple-identity-management-systems"></a>Birden çok kimlik yönetimi sistemine sahip ortamlar

Bazı ortamlarda birden çok kimlik yönetim sistemi vardır. Oracle ve Sitedefteri gibi şirket içi kimlik yöneticileri, parolalar için AD ile eşitleme gerektirir. Bunu, parola değiştirme bildirimi hizmeti (PCNS) gibi Microsoft Identity Manager (MıM) içeren bir araç kullanarak yapabilirsiniz. Bu daha karmaşık senaryoda daha fazla bilgi bulmak için, [bir etki alanı denetleyicisinde MIM parola değiştirme bildirimi hizmetini dağıtma](/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller)makalesine bakın.

## <a name="plan-testing-and-support"></a>Test ve destek planlayın

Dağıtımın her aşamasında, ilk pilot gruplardan kuruluş genelinde, sonuçların beklendiğinden emin olun.

### <a name="plan-testing"></a>Test planı

Dağıtımınızın beklendiği gibi çalıştığından emin olmak için, uygulamayı doğrulamak üzere bir dizi test çalışması planlayın. Test çalışmalarını değerlendirmek için, parolası olan yönetici olmayan bir test kullanıcısına ihtiyacınız vardır. Bir kullanıcı oluşturmanız gerekiyorsa, [Azure Active Directory yeni kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md)bölümüne bakın.

Aşağıdaki tablo, ilkelerinize göre beklenen sonuçları belgelemek için kullanabileceğiniz yararlı test senaryolarını içerir.
<br>


| İş örneği| Beklenen sonuçlar |
| - | - |
| SSPR portalına kurumsal ağ içinden erişilebilir| Kuruluşunuz tarafından belirlenir |
| SSPR portalına şirket ağı dışından erişilebilir| Kuruluşunuz tarafından belirlenir |
| Kullanıcı parola sıfırlama için etkin olmadığında Kullanıcı parolasını tarayıcıdan sıfırlayın| Kullanıcı parola sıfırlama akışına erişemiyor |
| Kullanıcı parola sıfırlama için kaydolmadıysanız Kullanıcı parolasını tarayıcıdan sıfırlayın| Kullanıcı parola sıfırlama akışına erişemiyor |
| Parola sıfırlama kaydı uygulandığında kullanıcı oturum açar| Kullanıcıdan güvenlik bilgilerini kaydetmesini ister |
| Parola sıfırlama kaydı tamamlandığında Kullanıcı oturum açar| Kullanıcıdan güvenlik bilgilerini kaydetmesini ister |
| Kullanıcının lisansı olmadığında SSPR portalına erişilebilir| Erişilebilir |
| Windows 10 Azure AD 'ye katılmış veya karma Azure AD 'ye katılmış cihaz kilit ekranından Kullanıcı parolasını sıfırlama| Kullanıcı, parolayı sıfırlayabilir |
| SSPR kaydı ve kullanım verileri, neredeyse gerçek zamanlı olarak yöneticiler tarafından kullanılabilir| Denetim günlükleri aracılığıyla kullanılabilir |

Ayrıca, [Azure AD self servis parola sıfırlama pilot hizmeti 'Ni tamamlamaya](./tutorial-enable-sspr.md)da başvurabilirsiniz. Bu öğreticide, kuruluşunuzda SSPR 'den bir pilot sürümü kullanıma sunarak yönetici olmayan bir hesap kullanarak test edersiniz.

### <a name="plan-support"></a>Destek planlayın

SSPR genellikle kullanıcı sorunları oluşturmadığından, oluşabilecek sorunları gidermek için destek personeli hazırlamak önemlidir. Bir yönetici, Azure AD portalı aracılığıyla son kullanıcıların parolasını sıfırlayabileceğinden, sorunu bir self servis destek süreci aracılığıyla çözmeye yardımcı olmak daha iyidir.

Destek takımınızın başarısını etkinleştirmek için, kullanıcılarınızın aldığınız sorulara dayalı bir SSS oluşturabilirsiniz. İşte birkaç örnek:

| Senaryolar| Description |
| - | - |
| Kullanıcının kayıtlı bir kimlik doğrulama yöntemi yok| Kullanıcı parolasını sıfırlamaya çalışıyor ancak kaydoldukları kimlik doğrulama yöntemlerinden hiçbirine sahip değil (örnek: kendi cep telefonlarını evde ve e-postaya erişemez) |
| Kullanıcı Office veya cep telefonunda bir metin veya çağrı almıyor| Kullanıcı, kimliğini metin veya çağrı yoluyla doğrulamaya çalışıyor ancak bir metin/çağrı almıyor. |
| Kullanıcı parola sıfırlama portalına erişemiyor| Kullanıcı parolasını sıfırlamak istiyor ancak parola sıfırlama için etkin değil, parolaları güncelleştirmek için sayfaya erişemez. |
| Kullanıcı yeni bir parola ayarlayamıyorum| Kullanıcı parola sıfırlama akışı sırasında doğrulamayı tamamlar, ancak yeni bir parola ayarlayamaz. |
| Kullanıcı bir Windows 10 cihazında parola sıfırlama bağlantısını görmez| Kullanıcı Windows 10 kilit ekranından parolayı sıfırlamaya çalışıyor, ancak cihaz Azure AD 'ye katılmadı ya da Intune cihaz ilkesi etkinleştirilmemiş |

### <a name="plan-rollback"></a>Planı geri alma

Dağıtımı geri almak için:

* tek bir kullanıcı için, kullanıcıyı güvenlik grubundan kaldırın 

* bir grup için, SSPR yapılandırmasından grubu kaldırın

* Herkes için, Azure AD kiracısı için SSPR 'yi devre dışı bırakın

## <a name="deploy-sspr"></a>SSPR dağıtma

Dağıtılmadan önce, aşağıdakileri yapmadiğinizden emin olun:

1. [İletişim planınızı](#plan-communications)oluşturma ve yürütme başladı.

1. Uygun [yapılandırma ayarları](#plan-configuration)belirlendi.

1. [Pilot](#plan-a-pilot) ve üretim ortamları için kullanıcılar ve gruplar tanımlanır.

1. Kayıt ve self servis için [yapılandırma ayarları belirlendi](#plan-configuration) .

1. Karma ortamınız varsa, [parola geri yazma özelliği yapılandırıldı](#password-writeback) .


**Artık SSPR 'yi dağıtmaya hazırsınız!**

Aşağıdaki alanların yapılandırılmasına ilişkin adım adım yönergeler için bkz. [self servis parola sıfırlamayı etkinleştirme](./tutorial-enable-sspr.md#enable-self-service-password-reset) .

1. [Kimlik doğrulama yöntemleri](./concept-authentication-methods.md)

1. [Kayıt ayarları](./concept-registration-mfa-sspr-combined.md)

1. [Bildirim ayarları](#notifications-settings)

1. [Özelleştirme ayarları](./howto-sspr-customization.md)

1. [Şirket içi tümleştirme](./tutorial-enable-sspr-writeback.md)

### <a name="enable-sspr-in-windows"></a>Windows 'da SSPR 'yi etkinleştirme
Windows 7, 8, 8,1 ve 10 çalıştıran makineler için, [Windows oturum açma ekranında kullanıcıların parolalarını sıfırlamalarını](./howto-sspr-windows.md) sağlayabilirsiniz

## <a name="manage-sspr"></a>SSPR 'yi Yönet

Azure AD, denetimler ve raporlar aracılığıyla SSPR performanstan ilgili ek bilgiler sağlayabilir.

### <a name="password-management-activity-reports"></a>Parola yönetimi etkinlik raporları 

SSPR performansını ölçmek için Azure portal önceden oluşturulmuş raporları kullanabilirsiniz. Uygun şekilde lisanslandıysanız özel sorgular da oluşturabilirsiniz. Daha fazla bilgi için bkz. [Azure AD parola yönetimi Için raporlama seçenekleri](./howto-sspr-reporting.md)

> [!NOTE]
>  [Genel yönetici](../users-groups-roles/directory-assign-admin-roles.md)olmanız ve bu verilerin kuruluşunuz için toplanması için kabul etmeniz gerekir. Kabul etmek için, Raporlama sekmesini veya Azure portalındaki denetim günlüklerini en az bir kez ziyaret etmeniz gerekir. Bu durumda, veriler kuruluşunuz için toplanmaz.

Kayıt ve parola sıfırlama için denetim günlükleri 30 gün boyunca kullanılabilir. Kuruluşunuzun içindeki güvenlik denetimi daha uzun bekletme gerektiriyorsa, günlüklerin [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), splunk veya arctıma gıbı bır SIEM aracına aktarılması ve kullanılması gerekir.

![SSPR raporlama ekran görüntüsü](./media/howto-sspr-deployment/sspr-reporting.png)

### <a name="authentication-methods--usage-and-insights"></a>Kimlik doğrulama yöntemleri-kullanım ve Öngörüler

[Kullanım ve Öngörüler](./howto-authentication-methods-usage-insights.md) , Azure MFA ve SSPR gibi özellikler için kimlik doğrulama yöntemlerinin kuruluşunuzda nasıl çalıştığını anlamanıza olanak tanır. Bu raporlama özelliği, kuruluşunuzun hangi yöntemlerin kayıt yaptığını ve bunların nasıl kullanılacağını anlamak için gerekenleri sağlar.

### <a name="troubleshoot"></a>Sorun giderme

* [Self servis parola sıfırlama sorunlarını giderme](./active-directory-passwords-troubleshoot.md) bölümüne bakın 

* [Parola yönetimi hakkında sık sorulan soruları](./active-directory-passwords-faq.md) izleyin 

### <a name="helpful-documentation"></a>Faydalı belgeler

* [Kimlik doğrulaması yöntemleri nelerdir?](./concept-authentication-methods.md)

* [Nasıl çalışıyor: Azure AD self servis parola sıfırlama](./concept-sspr-howitworks.md)

* [Self servis parola sıfırlama için Azure AD işlevselliğini özelleştirme](./howto-sspr-customization.md)

* [Azure Active Directory'deki parola ilkeleri ve kısıtlamaları](./concept-sspr-policy.md)

* [Parola geri yazma nedir?](./concept-sspr-writeback.md)

## <a name="next-steps"></a>Sonraki adımlar

* SSPR 'yi dağıtmaya başlamak için bkz. [Azure AD self servis parola sıfırlamayı etkinleştirme](tutorial-enable-sspr.md)

* [Azure AD parola korumasını uygulamayı deneyin](./concept-password-ban-bad.md)

* [Azure AD akıllı kilitleme 'yi uygulamayı düşünün](./howto-password-smart-lockout.md)