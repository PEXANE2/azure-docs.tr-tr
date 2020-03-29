---
title: Sorun giderme self servis parola sıfırlama - Azure Active Directory
description: Azure AD self servis parola sıfırlama sorunu giderme
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a22c0cc922e021edc37dfbb2d89fdd20c77b2c87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848774"
---
# <a name="troubleshoot-self-service-password-reset"></a>Self servis parolasını sıfırlama sorunlarını giderme

Azure Etkin Dizin (Azure AD) self servis parola sıfırlama (SSPR) ile ilgili bir sorun mu yaşıyorsunuz? Aşağıdaki bilgiler, işleri yeniden çalıştırmanıza yardımcı olabilir.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-might-see"></a>Kullanıcının görebileceği self servis parola sıfırlama hatalarını giderme sorunu

| Hata | Ayrıntılar | Teknik ayrıntılar |
| --- | --- | --- |
| KiracısSPRFlagDisabled = 9 | Üzgünüz, yöneticiniz kuruluşunuz için parola sıfırlamayı devre dışı bıraktık çünkü şu anda parolanızı sıfırlayamadınız. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen yöneticinizle iletişime geçin ve bu özelliği etkinleştirmelerini isteyin. Daha fazla bilgi için Yardım'a [bakın, Azure AD şifremi unuttum.](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions) | SSPR_0009: Parola sıfırlamanın yöneticiniz tarafından etkinleştirilemediğini tespit ettik. Lütfen yöneticinizle iletişime geçin ve kuruluşunuz için parola sıfırlamayı etkinleştirmelerini isteyin. |
| WritebackNotEnabled = 10 |Üzgünüz, yöneticiniz kuruluşunuz için gerekli bir hizmeti etkinleştirmediği için şu anda parolanızı sıfırlayamadığınız için üzgünüz. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen yöneticinizle iletişime geçin ve kuruluşunuzun yapılandırmasını kontrol etmelerini isteyin. Bu gerekli hizmet hakkında daha fazla bilgi edinmek için [parola yazma yı yapılandırma'ya](howto-sspr-writeback.md)bakın. | SSPR_0010: Parola yazmanın etkinleştirilemediğini tespit ettik. Lütfen yöneticinize başvurun ve şifre yazmayı etkinleştirmelerini isteyin. |
| SsprNotEnabledInUserPolicy = 11 | Üzgünüz, yöneticiniz kuruluşunuz için parola sıfırlama yı yapılandırmadığı için şu anda parolanızı sıfırlayamadığınız için. Bu durumu çözmek için başka bir işlem yapAmaz. Yöneticinize başvurun ve parola sıfırlama yapılandırmalarını isteyin. Parola sıfırlama yapılandırması hakkında daha fazla bilgi edinmek için hızlı [başlangıç: Azure AD self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Kuruluşunuz parola sıfırlama ilkesi tanımlamadı. Lütfen yöneticinizle iletişime geçin ve onlardan bir parola sıfırlama ilkesi tanımlamalarını isteyin. |
| UserNotLicensed = 12 | Üzgünüz, kuruluşunuziçin gerekli lisanslar eksik olduğundan şu anda parolanızı sıfırlayamadığınız için üzgünüz. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen yöneticinizle iletişime geçin ve lisans atamanızı kontrol etmelerini isteyin. Lisanslama hakkında daha fazla bilgi edinmek [için Azure AD self servis parola sıfırlama gereksinimlerine](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing)bakın. | SSPR_0012: Kuruluşunuz parola sıfırlama gerçekleştirmek için gerekli lisanslara sahip değildir. Lütfen yöneticinizle iletişime geçin ve lisans ödevlerini gözden geçirmelerini isteyin. |
| KullanıcıNotMemberOfScopedAccessGroup = 13 | Üzgünüz, yöneticiniz hesabınızı parola sıfırlama kullanacak şekilde yapılandırmadığı için şu anda parolanızı sıfırlayamadığınız için. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen yöneticinizle iletişime geçin ve parola sıfırlama için hesabınızı yapılandırmalarını isteyin. Parola sıfırlama hesap yapılandırması hakkında daha fazla bilgi edinmek [için kullanıcılar için parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices)yı kaldır'a bakın. | SSPR_0013: Parola sıfırlama için etkin leştirilmiş bir grubun üyesi değilsiniz. Yöneticinize başvurun ve gruba eklenmesini isteyin. |
| UserNotProperlyYapılanmış = 14 | Üzgünüz, hesabınızda gerekli bilgiler eksik olduğundan şu anda parolanızı sıfırlayamıyorsunuz. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen admin iletişim ve sizin için şifrenizi sıfırlamak isteyin. Hesabınıza yeniden erişedikten sonra, gerekli bilgileri kaydetmeniz gerekir. Bilgileri kaydetmek için self servis parola sıfırlama makalesi [için Kaydol'daki](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register) adımları izleyin. | SSPR_0014: Parolanızı sıfırlamak için ek güvenlik bilgileri gereklidir. Devam etmek için yöneticinize başvurun ve parolanızı sıfırlamalarını isteyin. Hesabınıza erişedikten sonra ek güvenlik bilgilerini https://aka.ms/ssprsetup. Yöneticiniz, Set'teki adımları izleyerek hesabınıza ek güvenlik bilgileri ekleyebilir [ve parola sıfırlama için kimlik doğrulama verilerini okuyabilir.](howto-sspr-authenticationdata.md) |
| OnpremisesadminActionGerekli = 29 | Üzgünüz, kuruluşunuzun parola sıfırlama yapılandırmasıyla ilgili bir sorun nedeniyle şu anda parolanızı sıfırlayamayız. Bu durumu çözmek için başka bir işlem yapAmaz. Lütfen yöneticinize başvurun ve araştırmalarını isteyin. Olası sorun hakkında daha fazla bilgi edinmek için Sorun [Giderparolasını geri alma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback)'ya bakın. | SSPR_0029: Şirket içi yapılandırmanızdaki bir hata nedeniyle parolanızı sıfırlayamıyoruz. Lütfen yöneticinize başvurun ve araştırmalarını isteyin. |
| OnpremisesConnectivityError = 30 | Üzgünüz, kuruluşunuzla bağlantı sorunları nedeniyle şu anda parolanızı sıfırlayamayız. Şu anda yapılacak bir işlem yok, ancak daha sonra tekrar denerseniz sorun çözülebilir. Sorun devam ederse, lütfen yöneticinizle iletişime geçin ve araştırmalarını isteyin. Bağlantı sorunları hakkında daha fazla bilgi edinmek için Sorun [Giderparolası geri bağlantı bağlantısına](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity)bakın. | SSPR_0030: Şirket içi ortamınızla bağlantının zayıf olması nedeniyle parolanızı sıfırlayamayız. Yöneticinize başvurun ve araştırmalarını isteyin.|

## <a name="troubleshoot-the-password-reset-configuration-in-the-azure-portal"></a>Azure portalında parola sıfırlama yapılandırmasını giderme

| Hata | Çözüm |
| --- | --- |
| Azure portalında Azure AD altında **Parola sıfırlama** bölümünü göremiyorum. | Bu, işlemi gerçekleştiren yöneticiye atanmış bir Azure AD lisansınız yoksa gerçekleşebilir. <br> <br> Söz konusu yönetici hesabına lisans atayın. [Atama, doğrulama ve lisanslar makalesiyle ilgili sorunları çözme](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) adımlarını izleyebilirsiniz.|
| Belirli bir yapılandırma seçeneği görmüyorum. | UI'nin birçok öğesi ihtiyaç duyulana kadar gizlenir. Görmek istediğiniz tüm seçenekleri etkinleştirmedeneyin. |
| **Şirket içi entegrasyon** sekmesini göremiyorum. | Bu seçenek yalnızca Azure AD Connect'i indirdiyseniz ve parola yazmayı yapılandırmışsanız görünür hale gelir. Daha fazla bilgi için, [ekspres ayarları kullanarak Azure AD Connect ile başlarken](../hybrid/how-to-connect-install-express.md)bakın. |

## <a name="troubleshoot-password-reset-reporting"></a>Parola sıfırlama raporlaması

| Hata | Çözüm |
| --- | --- |
| **Self Servis Şifre Yönetimi** denetim etkinlik kategorisinde herhangi bir parola yönetimi etkinlik türü görmüyorum. | Bu, işlemi gerçekleştiren yöneticiye atanmış bir Azure AD lisansınız yoksa gerçekleşebilir. <br> <br> Söz konusu yönetici hesabına lisans atayarak bu sorunu çözebilirsiniz. Lisanslar makalesiyle [ilgili atama, doğrulama ve sorunları çözme](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) adımlarını izleyin. |
| Kullanıcı kayıtları birden çok kez gösterir. | Şu anda, bir kullanıcı kaydolduğunda, ayrı bir olay olarak kaydedilmiş her bir veri parçasını kaydederiz. <br> <br> Bu verileri bir araya getirmek ve görüntüleme konusunda daha fazla esnekliğe sahip olmak istiyorsanız, raporu karşıdan yükleyebilir ve verileri Excel'de özet tablo olarak açabilirsiniz.

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Parola sıfırlama kayıt portalının sorun gidermesi

| Hata | Çözüm |
| --- | --- |
| Parola sıfırlama için dizin etkin değil. **Yöneticiniz bu özelliği kullanmanıza izin vermedi.** | Self **servis parola sıfırlama özelliğini** **Seçili** veya **Tümü'ne** değiştirin ve ardından **Kaydet'i**seçin. |
| Kullanıcının atanmış bir Azure REKLAM lisansı yok. **Yöneticiniz bu özelliği kullanmanıza izin vermedi.** | Bu, işlemi gerçekleştiren yöneticiye atanmış bir Azure AD lisansınız yoksa gerçekleşebilir. <br> <br> Söz konusu yönetici hesabına lisans atayarak bu sorunu çözebilirsiniz. Lisanslar makalesiyle [ilgili atama, doğrulama ve sorunları çözme](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) adımlarını izleyin.|
| İsteği işleyen bir hata var. | Bu birçok sorundan kaynaklanabilir, ancak genellikle bu hata bir hizmet kesintisi veya yapılandırma sorunu neden olur. Bu hatayı görürseniz ve bu durum işletmenizi etkilerse, ek yardım için Microsoft desteğine başvurun. |

## <a name="troubleshoot-the-password-reset-portal"></a>Parola sıfırlama portalının sorun giderme

| Hata | Çözüm |
| --- | --- |
| Parola sıfırlama için dizin etkin değil. | Self **servis parola sıfırlama özelliğini** **Seçili** veya **Tümü'ne** değiştirin ve ardından **Kaydet'i**seçin. |
| Kullanıcının atanmış bir Azure REKLAM lisansı yok. | Bu, işlemi gerçekleştiren yöneticiye atanmış bir Azure AD lisansınız yoksa gerçekleşebilir. <br> <br> Söz konusu yönetici hesabına lisans atarsanız bu sorunu çözebilirsiniz. Lisanslar makalesiyle [ilgili atama, doğrulama ve sorunları çözme](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) adımlarını izleyin. |
| Parola sıfırlama için dizin etkinleştirildi, ancak kullanıcı eksik veya yanlış biçimlendirilmiş kimlik doğrulama bilgilerine sahip. | İşleme başlamadan önce, kullanıcının dizinde dosyadaki kişi verilerini düzgün bir şekilde oluşturduğundan emin olun. Daha fazla bilgi için Azure [AD self servis parola sıfırlama tarafından kullanılan Verilere](howto-sspr-authenticationdata.md)bakın. |
| Parola sıfırlama için dizin etkindir, ancak ilke iki doğrulama yöntemi gerektirecek şekilde ayarlandığında kullanıcının dosyada yalnızca bir kişi verisi vardır. | Devam etmeden önce, kullanıcının en az iki düzgün yapılandırılmış iletişim yöntemi olduğundan emin olun. Bir örnek hem cep telefonu numarası *hem* de ofis telefon numarası olmasıdır. |
| Parola sıfırlama için dizin etkinleştirilir ve kullanıcı düzgün şekilde yapılandırılır, ancak kullanıcıyla bağlantı kurulamaz. | Bu, geçici bir hizmet hatasının veya doğru şekilde algılayamadığımız yanlış iletişim verilerinin sonucu olabilir. <br> <br> Kullanıcı 10 saniye beklerse, "yeniden deneyin" ve "yöneticinize başvurun" bağlantıları görüntülenir. Kullanıcı "yeniden deneyin" seçeneğini seçerse, aramayı yeniden dener. Kullanıcı "yöneticinizle iletişim kurun" seçeneğini seçerse, yöneticilerine bu kullanıcı hesabı için gerçekleştirilecek parola sıfırlamasını isteyen bir form e-postası gönderir. |
| Kullanıcı şifre sıfırlama SMS veya telefon görüşmesi alır asla. | Bu, dizindeki yanlış bilgilendirilmiş bir telefon numarasının sonucu olabilir. Telefon numarası "+ccc xxxyyyzzzzXeeee" biçiminde olduğundan emin olun. <br> <br> Dizinde bir tane belirtseniz bile, parola sıfırlama uzantılarını desteklemez. Arama gönderilmeden önce uzantılar söküldü. Uzantıolmadan bir numara kullanın veya uzantıyı özel şube değişiminizdeki (PBX) telefon numarasına entegre edin. |
| Kullanıcı parola sıfırlama e-postasını asla almaz. | Bu sorunun en yaygın nedeni, iletinin bir spam filtresi tarafından reddedilmesidir. E-posta için spam, önemsiz veya silinmiş öğeler klasörünüzdür. <br> <br> Ayrıca, ileti için doğru e-posta hesabını kontrol ettiğinizden emin olun. |
| Parola sıfırlama ilkesini belirledim, ancak bir yönetici hesabı parola sıfırlama kullandığında bu ilke uygulanmaz. | Microsoft, en yüksek güvenlik düzeyini sağlamak için yönetici parola sıfırlama ilkesini yönetir ve denetler. |
| Kullanıcının bir parolayı bir günde çok fazla sıfırlama girişiminde bulunmaları engellenir. | Kullanıcıların parolalarını kısa sürede çok fazla sıfırlamaya çalışmalarını engellemek için otomatik bir azaltma mekanizması uyguluyoruz. Azaltma şu anda gerçekleşir: <br><ul><li>Kullanıcı bir telefon numarasını bir saat içinde beş kez doğrulamaya çalışır.</li><li>Kullanıcı güvenlik soru kapısını bir saat içinde beş kez kullanmaya çalışır.</li><li>Kullanıcı, aynı kullanıcı hesabının parolasını bir saat içinde beş kez sıfırlamaya çalışır.</li></ul>Bu sorunu gidermek için, kullanıcıya son denemeden sonra 24 saat beklemesini bildirin. Kullanıcı daha sonra parolasını sıfırlayabilir. |
| Kullanıcı telefon numarasını doğrularken bir hata görür. | Bu hata, girilen telefon numarası dosyadaki telefon numarasıyla eşleşmediğinde oluşur. Kullanıcının parola sıfırlama için telefon tabanlı bir yöntem kullanmaya çalışırken alan ve ülke kodu da dahil olmak üzere tam telefon numarasını girdiğinden emin olun. |
| İsteği işleyen bir hata var. | Bu birçok sorundan kaynaklanabilir, ancak genellikle bu hata bir hizmet kesintisi veya yapılandırma sorunu neden olur. Bu hatayı görürseniz ve bu durum işletmenizi etkilerse, ek yardım için Microsoft desteğine başvurun. |
| Şirket içi politika ihlali | Parola, şirket içi Active Directory parola ilkesini karşılamıyor. |
| Parola bulanık ilke yi yetmiyor | Kullanılan parola [yasaklı parola listesinde](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad#how-are-passwords-evaluated) görünür ve kullanılmayabilir. |

## <a name="troubleshoot-password-writeback"></a>Parola yazma sorunu

| Hata | Çözüm |
| --- | --- |
| Parola sıfırlama hizmeti şirket içinde başlamaz. Hata 6800, Azure AD Connect makinesinin uygulama olay günlüğünde görünür. <br> <br> Bindikten sonra, federe, geçiş kimlik doğrulaması veya parola-karma senkronize kullanıcılar parolalarını sıfırlayamaz. | Parola yazma etkinleştirildiğinde, eşitleme motoru bulut onboarding hizmetiyle iletişim kurarak yapılandırmayı (onboarding) gerçekleştirmek için writeback kitaplığını çağırır. Windows Communication Foundation (WCF) bitiş noktasını başlatırken veya giriş sırasında karşılaşılan hatalar, Azure AD Connect makinenizde olay günlüğünde hatalara neden olur. <br> <br> Azure AD Sync (ADSync) hizmetinin yeniden başlatılması sırasında, writeback yapılandırıldıysa, WCF bitiş noktası açılır. Ancak, bitiş noktasının başlatılması başarısız olursa, olay 6800'ü günlüğe kaydeder ve eşitleme hizmetinin başlatılmasına izin veririz. Bu olayın varlığı, parola yazma bitiş noktasının başlatılmadığını anlamına gelir. Bu olay 6800 için olay günlüğü ayrıntıları, PasswordResetService bileşeni tarafından yapılan olay günlüğü girişleri ile birlikte, bitiş noktasını neden başlatamadığınızı gösterir. Bu olay günlüğü hatalarını gözden geçirin ve parola geri yazma hala çalışmıyorsa Azure AD Bağlantısı'nı yeniden başlatmayı deneyin. Sorun devam ederse, devre dışı bırakıp parola yazmayı yeniden etkinleştirmeye çalışın.
| Bir kullanıcı parolayı sıfırlamaya veya parola yazma etkinleştirilmiş bir hesabın kilidini açmaya çalıştığında, işlem başarısız olur. <br> <br> Ayrıca, Azure AD Connect olay günlüğünde şunları içeren bir olay görürsünüz: "Eşitleme Altyapısı bir hata döndürür hr=800700CE, ileti=Dosya adı veya uzantısı çok uzundur" | Azure AD Connect için Etkin Dizin hesabını bulun ve parolayı en fazla 256 karakter içermeden sıfırlayın. Ardından **Başlat** menüsünden **Eşitleme Hizmetini** açın. **Bağlayıcılara** göz atın ve **Active Directory Bağlayıcısını**bulun. Seçin ve ardından **Özellikler'i**seçin. **Kimlik Bilgileri** sayfasına göz atın ve yeni parolayı girin. Sayfayı kapatmak için **Tamam'ı** seçin. |
| Azure AD Connect yükleme işleminin son aşamasında, parola yazma işleminin yapılandırılamayacağını belirten bir hata görürsünüz. <br> <br> Azure AD Connect uygulama olay günlüğü , "Hata auth belirteci alma" metniyle birlikte 32009 hata içerir. | Bu hata aşağıdaki iki durumda oluşur: <br><ul><li>Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için yanlış bir parola belirlemişsiniz.</li><li>Azure AD Connect yükleme işleminin başında belirtilen küresel yönetici hesabı için federe bir kullanıcı kullanmaya çalıştınız.</li></ul> Bu sorunu gidermek için, yükleme işleminin başında belirttiğiniz küresel yönetici için federe bir hesap kullanmadığınızdan emin olun. Ayrıca belirtilen parolanın doğru olduğundan emin olun. |
| Azure AD Connect makine olay günlüğü, PasswordResetService çalıştırılarak atılan hata 32002'yi içerir. <br> <br> Hata şöyledir: "ServiceBus'a Bağlanma hatası. Belirteç sağlayıcısı bir güvenlik belirteci sağlayamadı." | Şirket ortamınız buluttaki Azure Hizmet Veri Servisi bitiş noktasına bağlanamıyor. Bu hata, normalde belirli bir bağlantı noktasına veya web adresine giden bir bağlantıyı engelleyen bir güvenlik duvarı kuralından kaynaklanır. Daha fazla bilgi için [Bağlantı ön koşulları](../hybrid/how-to-connect-install-prerequisites.md) ile ilgili bilgilere bakın. Bu kuralları güncelledikten sonra Azure AD Connect makinesini yeniden başlatın ve parola yazma yeniden çalışmaya başlamalıdır. |
| Federe, geçiş kimlik doğrulaması veya parola-karma senkronize kullanıcılar bir süre çalıştıktan sonra parolalarını sıfırlayamaz. | Bazı nadir durumlarda, Azure AD Connect yeniden başlatıldığında parola geri yazma hizmeti yeniden başlatılamayabilirsiniz. Bu gibi durumlarda, öncelikle parola yazmanın şirket içinde etkinleştirilip etkinleştirilmediğini kontrol edin. Azure AD Connect sihirbazı veya PowerShell 'i kullanarak kontrol edebilirsiniz (Önceki HowTos bölümüne bakın). Özellik etkin görünüyorsa, UI veya PowerShell aracılığıyla özelliği etkinleştirmeyi veya devre dışı bırakmayı deneyin. Bu işe yaramazsa, Azure AD Connect'in tam bir kaldırVe yeniden yüklemesini deneyin. |
| Parolalarını sıfırlamaya çalışan federe, geçiş kimlik doğrulaması veya parolalarını sıfırlamaya çalışan parola-karma senkronize kullanıcılar, parolalarını göndermeye çalıştıktan sonra bir hata görürler. Hata, bir hizmet sorunu olduğunu gösterir. <br ><br> Bu soruna ek olarak, parola sıfırlama işlemleri sırasında, yönetim aracısının şirket içi olay günlüklerinizde erişiminin engellenmesinde bir hata görebilirsiniz. | Olay günlüğünde bu hataları görürseniz, yapılandırma sırasında sihirbazda belirtilen Active Directory Management Agent (ADMA) hesabının parola yazma için gerekli izinlere sahip olduğunu onaylayın. <br> <br> Bu izin verildikten sonra, izinlerin etki alanı denetleyicisi (DC) `sdprop` üzerindeki arka plan görevi aracılığıyla aşağı inmeleri bir saat kadar sürebilir. <br> <br> Parola sıfırlamanın çalışması için, izin parolası sıfırlanan kullanıcı nesnesinin güvenlik tanımlayıcısına damgalanması gerekir. Bu izin kullanıcı nesnesi üzerinde görünene kadar, erişim reddedilen bir iletiyle parola sıfırlama başarısız olmaya devam ediyor. |
| Parolalarını sıfırlamaya çalışan federe, geçiş kimlik doğrulaması veya parolalarını sıfırlamaya çalışan parola-karma senkronize kullanıcılar, parolalarını gönderdikten sonra bir hata görür. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu soruna ek olarak, parola sıfırlama işlemleri sırasında, Azure AD Connect hizmetindeki etkinlik günlüklerinizde "Nesne bulunamadı" hatası belirten bir hata görebilirsiniz. | Bu hata genellikle eşitleme altyapısının Azure AD bağlayıcı alanında kullanıcı nesnesini veya bağlı metaverse (MV) veya Azure AD bağlayıcı alanı nesnesini bulamadığını gösterir. <br> <br> Bu sorunu gidermek için, kullanıcının azure AD Connect'in geçerli örneği aracılığıyla şirket içinde Azure AD'ye gerçekten senkronize olduğundan emin olun ve bağlayıcı boşluklarındaki ve MV'deki nesnelerin durumunu denetleyin. Active Directory Certificate Services (AD CS) nesnesinin "Microsoft.InfromADUserAccountEnabled.xxx" kuralı yla MV nesnesine bağlı olduğunu doğrulayın.|
| Parolalarını sıfırlamaya çalışan federe, geçiş kimlik doğrulaması veya parolalarını sıfırlamaya çalışan parolaları senkronize edilmiş kullanıcılar, parolalarını gönderdikten sonra bir hata görürler. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu soruna ek olarak, parola sıfırlama işlemleri sırasında, Azure AD Connect hizmetindeki etkinlik günlüklerinizde "Birden çok eşleşme bulundu" hatası olduğunu belirten bir hata görebilirsiniz. | Bu, eşitleme altyapısının MV nesnesinin "Microsoft.InfromADUserAccountEnabled.xxx" üzerinden birden fazla AD CS nesnesine bağlı olduğunu algıladığını gösterir. Bu, kullanıcının birden fazla ormanda etkin bir hesabı olduğu anlamına gelir. Bu senaryo parola yazma için desteklenmez. |
| Parola işlemleri bir yapılandırma hatasıyla başarısız oldu. Uygulama olay günlüğü, "0x8023061f ( Bu Yönetim Aracısı'nda parola eşitlemesi etkinleştirilmediği için işlem başarısız oldu) metniyle birlikte Azure AD Connect 6329 hatasını içerir. | Bu hata, parola yazma özelliği zaten etkinleştirildikten sonra yeni bir Active Directory ormanı (veya varolan bir ormanı kaldırmak ve okumak için) olarak değiştirilirse oluşur. Bu son eklenen ormanlarda kullanıcılar için parola işlemleri başarısız oldu. Sorunu gidermek için, orman yapılandırması değişiklikleri tamamlandıktan sonra parola yazma özelliğini devre dışı bırakıp yeniden etkinleştirin. |

## <a name="password-writeback-event-log-error-codes"></a>Parola yazma olay günlüğü hata kodları

Parola yazma yla ilgili sorunları giderdiğinizde en iyi yöntem, Azure AD Connect makinenizde uygulama olay günlüğünü incelemektir. Bu olay günlüğü, parola yazma için ilgi iki kaynaktan olaylar içerir. PasswordResetService kaynağı, parola yazma işleminin işleyişiyle ilgili işlemleri ve sorunları açıklar. ADSync kaynağı, Etkin Dizin ortamınızda parola ayarlamayla ilgili işlemleri ve sorunları açıklar.

### <a name="if-the-source-of-the-event-is-adsync"></a>Olayın kaynağı ADSync ise

| Kod | Ad veya ileti | Açıklama |
| --- | --- | --- |
| 6329 | KEFALET: MMS(4924) 0x80230619: "Bir kısıtlama, parolanın belirtilen geçerli parolaya değiştirilmesini önler." | Bu olay, parola yazma hizmeti yerel dizininizde etki alanının parola yaşını, geçmişini, karmaşıklığını veya filtreleme gereksinimlerini karşılamayan bir parola ayarlamaya çalıştığında oluşur. <br> <br> Minimum parola yaşınız varsa ve bu süre içinde parolayı yakın zamanda değiştirdiyseniz, etki alanınızda belirtilen yaşa ulaşana kadar parolayı yeniden değiştiremezsiniz. Test amacıyla, minimum yaş 0 olarak ayarlanmalıdır. <br> <br> Parola geçmişi gereksinimleriniz etkinse, *N'nin* parola geçmişi ayarı olduğu son *N* zamanlarında kullanılmayan bir parola seçmeniz gerekir. Son *N* kez kullanılan bir parola seçerseniz, bu durumda bir hata görürsünüz. Sınama amacıyla parola geçmişi 0 olarak ayarlanmalıdır. <br> <br> Parola karmaşıklığı gereksinimleriniz varsa, kullanıcı parolayı değiştirmeye veya sıfırlamaya çalıştığında bunların tümü zorlanır. <br> <br> Parola filtreleriniz etkinse ve kullanıcı filtreleme ölçütlerini karşılamayan bir parola seçerse, sıfırlama veya değiştirme işlemi başarısız olur. |
| 6329 | MMS(3040): admaexport.cpp(2837): Sunucu LDAP parola ilkesi denetimini içermez. | Bu sorun, dcs'de LDAP_SERVER_POLICY_HINTS_OID denetimi (1.2.840.113556.1.4.2066) etkinleştirilmezse oluşur. Parola yazma özelliğini kullanmak için denetimi etkinleştirmeniz gerekir. Bunu yapmak için, DC'lerin Windows Server 2008R2 veya sonraki adresinde olması gerekir. |
| İk 8023042 | Synchronization Engine bir hata hr=80230402 döndü, mesaj=Aynı çapaile yinelenen girişler olduğundan bir nesneyi alma girişimi başarısız oldu. | Bu hata, aynı kullanıcı kimliği birden çok etki alanında etkinleştirildiğinde oluşur. Buna örnek olarak, hesap ve kaynak ormanlarını eşitliyorsanız ve her ormanda aynı kullanıcı kimliği mevcut ve etkinleştirilmişse. <br> <br> Bu hata, takma ad veya UPN gibi benzersiz olmayan bir bağlantı özniteliği kullanırsanız ve iki kullanıcı aynı bağlantı özniteliğini paylaşırsa da oluşabilir. <br> <br> Bu sorunu gidermek için, etki alanlarınızda çoğaltılmış kullanıcı olmadığından ve her kullanıcı için benzersiz bir bağlantı özniteliği kullandığınızdan emin olun. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Olayın kaynağı PasswordResetService ise

| Kod | Ad veya ileti | Açıklama |
| --- | --- | --- |
| 31001 | PasswordResetStart | Bu olay, şirket içi hizmetin, buluttan kaynaklanan federe, geçiş kimlik doğrulaması veya parola-karma senkronize kullanıcı için bir parola sıfırlama isteği algıladığını gösterir. Bu olay, her parola sıfırlama yazma işleminin ilk olayıdır. |
| 31002 | PasswordResetBaşarı | Bu olay, bir kullanıcının parola sıfırlama işlemi sırasında yeni bir parola seçtiğini gösterir. Bu parolanın kurumsal şifre gereksinimlerini karşıladığını belirledik. Parola, yerel Active Directory ortamına başarıyla geri yazılmıştır. |
| 31003 | PasswordResetFail | Bu olay, bir kullanıcının bir parola seçtiğini ve parolanın şirket içi ortama başarıyla geldiğini gösterir. Ancak parolayı yerel Active Directory ortamında ayarlamaya çalışırken bir hata oluştu. Bu hata çeşitli nedenlerle olabilir: <br><ul><li>Kullanıcının parolası etki alanı için yaş, geçmiş, karmaşıklık veya filtre gereksinimlerini karşılamaz. Bu sorunu gidermek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni parolayı ayarlamak için uygun izinlere sahip değildir.</li><li>Kullanıcının hesabı, parola kümesi işlemlerine izin veramayan etki alanı veya kurumsal yönetici grubu gibi korumalı bir gruptadır.</li></ul>|
| 31004 | OnboardingEventStart | Bu olay, Azure AD Connect ile parola yazmayı etkinleştiriseniz ve kuruluşunuzu parola yazma web hizmetine başlatmaya başladığımızda oluşur. |
| 31005 | OnboardingEventBaşarı | Bu olay, onboarding işleminin başarılı olduğunu ve parola yazma özelliğinin kullanıma hazır olduğunu gösterir. |
| 31006 | Parolayı Değiştir | Bu olay, şirket içi hizmetin buluttan kaynaklanan federe, geçiş kimlik doğrulaması veya parola-karma senkronize kullanıcı için bir parola değişikliği isteği algıladığını gösterir. Bu olay, her parola değişikliği yazma işleminin ilk olayıdır. |
| 31007 | ChangePasswordSuccess | Bu olay, bir kullanıcının parola değiştirme işlemi sırasında yeni bir parola seçtiğini, parolanın kurumsal parola gereksinimlerini karşıladığını ve parolanın yerel Active Directory ortamına başarıyla geri yazıldığını belirledik. |
| 31008 | ChangePasswordFail | Bu olay, bir kullanıcının bir parola seçtiğini ve parolanın şirket içi ortama başarıyla geldiğini, ancak parolayı yerel Active Directory ortamında ayarlamaya çalışırken bir hata oluştuğunu gösterir. Bu hata çeşitli nedenlerle olabilir: <br><ul><li>Kullanıcının parolası etki alanı için yaş, geçmiş, karmaşıklık veya filtre gereksinimlerini karşılamaz. Bu sorunu gidermek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni parolayı ayarlamak için uygun izinlere sahip değildir.</li><li>Kullanıcının hesabı, parola kümesi işlemlerine izin veramayan etki alanı veya kuruluş yöneticileri gibi korumalı bir gruptadır.</li></ul> |
| 31009 | SıfırlamaUserPasswordByAdminStart | Şirket içi hizmet, kullanıcı adına yöneticiden gelen federe, geçiş kimlik doğrulaması veya parola-karma senkronize kullanıcı için parola sıfırlama isteği algıladı. Bu olay, bir yönetici tarafından başlatılan her parola sıfırlama yazma işlemindeki ilk olaydır. |
| 31010 | SıfırlamaUserPasswordByAdminSuccess | Yönetici, başlatılan parola sıfırlama işlemi sırasında yeni bir parola seçti. Bu parolanın kurumsal şifre gereksinimlerini karşıladığını belirledik. Parola, yerel Active Directory ortamına başarıyla geri yazılmıştır. |
| 31011 | SıfırlamaUserPasswordByAdminFail | Yönetici bir kullanıcı adına bir parola seçti. Parola şirket içi ortama başarıyla ulaştı. Ancak parolayı yerel Active Directory ortamında ayarlamaya çalışırken bir hata oluştu. Bu hata çeşitli nedenlerle olabilir: <br><ul><li>Kullanıcının parolası etki alanı için yaş, geçmiş, karmaşıklık veya filtre gereksinimlerini karşılamaz. Bu sorunu gidermek için yeni bir parola deneyin.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni parolayı ayarlamak için uygun izinlere sahip değildir.</li><li>Kullanıcının hesabı, parola kümesi işlemlerine izin veramayan etki alanı veya kuruluş yöneticileri gibi korumalı bir gruptadır.</li></ul>  |
| 31012 | OffboardingEventStart | Bu olay, Azure AD Connect ile parola yazmayı devre dışı ederseniz ve kuruluşunuzun parola yazma web hizmetine dahil etmeye başladığımızı gösterirse oluşur. |
| 31013| OffboardingEventBaşarı| Bu olay, offboarding işleminin başarılı olduğunu ve parola yazma özelliğinin başarıyla devre dışı bırakıldığını gösterir. |
| 31014| OffboardingEventFail| Bu olay, offboarding işleminin başarılı olmadığını gösterir. Bunun nedeni, yapılandırma sırasında bulutta veya şirket içi yönetici hesabında bir izin hatası olabilir. Parola yazma yıkıvmayı devre dışı bırakırken federe bir bulut yöneticisi ni kullanmaya çalışıyorsanız hata da oluşabilir. Bu sorunu gidermek için, yönetim izinlerinizi denetleyin ve parola yazma özelliğini yapılandırırken federal hesap kullanmadığınızdan emin olun.|
| 31015| WriteBackServiceStarted| Bu olay, parola yazma hizmetinin başarıyla başladığını gösterir. Buluttan gelen parola yönetimi isteklerini kabul etmeye hazırdır.|
| 31016| WriteBackServiceDurduruldu| Bu olay, parola yazma hizmetinin durduğunu gösterir. Buluttan gelen parola yönetimi istekleri başarılı olmayacaktır.|
| 31017| AuthTokenBaşarı| Bu olay, azure AD Connect kurulumu sırasında belirtilen global yönetici için bir yetkilendirme jetonunu başarıyla aldığımızı ve biniş veya biniş işlemini başlattığımızı gösterir.|
| 31018| KeyPairCreationBaşarı| Bu olay, parola şifreleme anahtarını başarıyla oluşturduğumuzanlamına gelendir. Bu anahtar, şirket içi ortamınıza gönderilmek üzere buluttan gelen parolaları şifrelemek için kullanılır.|
| 32000| UnknownError| Bu olay, parola yönetimi işlemi sırasında oluşan bilinmeyen bir hatayı gösterir. Daha fazla ayrıntı için olaydaki özel durum metnine bakın. Sorun yaşıyorsanız, devre dışı bırakmayı ve ardından parola yazmayı yeniden etkinleştirmeyi deneyin. Bu işe yaramazsa, destek mühendisinize belirtilen izleme kimliğiyle birlikte olay günlüğünüzün bir kopyasını ekleyin.|
| 32001| ServiceError| Bu olay, bulut parola sıfırlama hizmetine bağlanan bir hata olduğunu gösterir. Bu hata genellikle şirket içi hizmet parola sıfırlama web hizmetine bağlanamayınca oluşur.|
| 32002| ServiceBusError| Bu olay, kiracınızın Servis Veri Servisi örneğine bağlanan bir hata olduğunu gösterir. Bu durum, şirket içi ortamınızdaki giden bağlantıları engelliyorsanız gerçekleşebilir. TCP 443 üzerinden bağlantılara izin verdiğinizden emin https://ssprdedicatedsbprodncu.servicebus.windows.netolmak için güvenlik duvarınızı kontrol edin ve sonra yeniden deneyin. Hala sorun yaşıyorsanız, devre dışı bırakmayı ve ardından parola yazmayı yeniden etkinleştirmeyi deneyin.|
| 32003| InPutValidationHatası| Bu olay, web hizmeti API'mize geçen girişin geçersiz olduğunu gösterir. İşlemi tekrar deneyin.|
| 32004| Şifre ÇözmeHatası| Bu olay, buluttan gelen parolanın şifresini çözen bir hata olduğunu gösterir. Bunun nedeni, bulut hizmeti ile şirket ortamınız arasındaki şifre çözme anahtarı uyuşmazlığı olabilir. Bu sorunu gidermek için, şirket içi ortamınızda parola yazmayı devre dışı bırakıp yeniden etkinleştirin.|
| 32005| Yapılandırma Hatası| Biniş sırasında, kiracıya özgü bilgileri şirket içi ortamınızda bir yapılandırma dosyasına kaydederiz. Bu olay, bu dosyayı kaydeden bir hata olduğunu veya hizmet başlatıldığında dosyayı okurken bir hata olduğunu gösterir. Bu sorunu gidermek için, yapılandırma dosyasının yeniden yazılmasını zorlamak için parola yazmayı devre dışı bırakmayı ve yeniden etkinleştirmeyi deneyin.|
| 32007| OnBoardingConfigUpdateError| Biniş sırasında, buluttan şirket içi parola sıfırlama hizmetine veri göndeririz. Bu veriler, diskte güvenli bir şekilde depolanacak eşitleme hizmetine gönderilmeden önce bellek içi bir dosyaya yazılır. Bu olay, bellekte bu verileri yazma veya güncelleştirme ile ilgili bir sorun olduğunu gösterir. Bu sorunu gidermek için, bu yapılandırma dosyasının yeniden yazılmasını zorlamak için parola yazmayı devre dışı bırakmayı ve ardından yeniden etkinleştirmeyi deneyin.|
| 32008| Validationerror| Bu olay, parola sıfırlama web hizmetinden geçersiz bir yanıt aldığımızı gösterir. Bu sorunu gidermek için, parola yazmayı devre dışı bırakmayı ve ardından yeniden etkinleştirmeyi deneyin.|
| 32009| AuthTokenError| Bu olay, Azure AD Connect kurulumu sırasında belirtilen genel yönetici hesabı için yetkilendirme belirteci alamadığımızı gösterir. Bu hata, genel yönetici hesabı için belirtilen kötü bir kullanıcı adı veya paroladan kaynaklanabilir. Belirtilen genel yönetici hesabı federe ise bu hata da oluşabilir. Bu sorunu gidermek için yapılandırmayı doğru kullanıcı adı ve parolayla yeniden çalıştırın ve yöneticinin yönetilen (yalnızca bulut veya parola yla eşitlenmiş) bir hesap olduğundan emin olun.|
| 32010| CryptoError| Bu olay, parola şifreleme anahtarını oluştururken veya bulut hizmetinden gelen bir parolanın şifresini çözmede bir hata olduğunu gösterir. Bu hata büyük olasılıkla ortamınızla ilgili bir sorunu gösterir. Bu sorunu nasıl çözeceğiniz hakkında daha fazla bilgi edinmek için etkinlik günlüğünüzün ayrıntılarına bakın. Ayrıca devre dışı bırakmayı ve ardından parola yazma hizmetini yeniden etkinleştirmeyi de deneyebilirsiniz.|
| 32011| OnboardingServiceError| Bu olay, şirket içi hizmetin, biniş işlemini başlatmak için parola sıfırlama web hizmetiyle düzgün şekilde iletişim kuramayacağını gösterir. Bu, bir güvenlik duvarı kuralının sonucu olarak veya kiracınız için kimlik doğrulama belirteci almada bir sorun varsa ortaya çıkabilir. Bu sorunu gidermek için, TCP 443 ve TCP 9350-9354 üzerinden giden bağlantıları https://ssprdedicatedsbprodncu.servicebus.windows.netengellemediğinizden emin olun. Ayrıca, onboard'da kullanmakta olduğunuz Azure AD yöneticisi hesabının federe olmadığından emin olun.|
| 32013| OffboardingError| Bu olay, şirket içi hizmetin, biniş işlemini başlatmak için parola sıfırlama web hizmetiyle düzgün şekilde iletişim kuramayacağını gösterir. Bu, bir güvenlik duvarı kuralının sonucu olarak veya kiracınız için yetkilendirme belirteci almada bir sorun varsa ortaya çıkabilir. Bu sorunu gidermek için, 443'ün üzerindeki giden bağlantıları veya https://ssprdedicatedsbprodncu.servicebus.windows.netdaha fazla bağlantıyı engellemediğinizden ve yerleşik olarak kullandığınız Azure Etkin Dizin yöneticisi hesabının federe olmadığından emin olun.|
| 32014| ServiceBusWarning| Bu olay, kiracınızın Servis Veri Hizmetleri örneğine bağlanmak için yeniden denememiz gerektiğini gösterir. Normal koşullarda, bu bir sorun olmamalıdır, ancak bu olayı birçok kez görürseniz, özellikle yüksek gecikmeli veya düşük bant genişliği bağlantısı varsa, Servis Veri Yolu'na olan ağ bağlantınızı kontrol etmeyi düşünün.|
| 32015| ReportServiceHealthError| Şifre yazma hizmetinin durumunu izlemek için, her beş dakikada bir şifre sıfırlama web hizmetimize sinyal verilerini göndeririz. Bu olay, bu sistem durumu bilgilerini bulut web hizmetine geri gönderirken bir hata olduğunu gösterir. Bu sistem durumu bilgileri nesne tanımlayıcı bilgiler (OII) veya kişisel olarak tanımlanabilir bilgiler (PII) verilerini içermez ve bulutta hizmet durumu bilgileri sağlayabilmemiz için tamamen bir sinyal ve temel hizmet istatistikleridir.|
| 33001| ADUnKnownError| Bu olay, Active Directory tarafından döndürülen bilinmeyen bir hata olduğunu gösterir. Daha fazla bilgi için ADSync kaynağından etkinlikler için Azure AD Connect sunucu olay günlüğünü kontrol edin.|
| 33002| ADUserNotFoundError| Bu olay, parolayı sıfırlamaya veya değiştirmeye çalışan kullanıcının şirket içi dizinde bulunmadığını gösterir. Bu hata, kullanıcı şirket içinde silindiğinde ancak bulutta silinmediğinde oluşabilir. Eşitleme ile ilgili bir sorun varsa, bu hata da oluşabilir. Daha fazla bilgi için eşitleme günlüklerinizi ve son birkaç eşitleme çalıştırma ayrıntılarını kontrol edin.|
| 33003| ADMutliMatchError| Parola sıfırlama veya değiştirme isteği buluttan kaynaklandığında, bu isteğin şirket içi ortamınızdaki bir kullanıcıya nasıl bağlanabileceğimizi belirlemek için Azure AD Connect'in kurulum işlemi sırasında belirtilen bulut bağlantısını kullanırız. Bu olay, şirket içi dizininizde aynı bulut bağlantı özelliğine sahip iki kullanıcı bulduğumuzu gösterir. Daha fazla bilgi için eşitleme günlüklerinizi ve son birkaç eşitleme çalıştırma ayrıntılarını kontrol edin.|
| 33004| ADPermissionsHata| Bu olay, Etkin Dizin Yönetimi Aracısı (ADMA) hizmet hesabının yeni bir parola ayarlamak için söz konusu hesapta uygun izinlere sahip olmadığını gösterir. Kullanıcının ormanındaki ADMA hesabının ormandaki tüm nesnelerde parola izinlerini sıfırlayıp değiştirdiğinden emin olun. İzinlerin nasıl ayarlanılabilenhakkında daha fazla bilgi için Bkz. Adım 4: Uygun Active Directory izinlerini ayarlayın. Bu hata, kullanıcının özniteliği AdminCount 1 olarak ayarlandığında da oluşabilir.|
| 33005| ADUserAccount Devre Dışı| Bu olay, şirket içinde devre dışı bırakılan bir hesabın parolasını sıfırlamaya veya değiştirmeye çalıştığımızı gösterir. Hesabı etkinleştirin ve işlemi yeniden deneyin.|
| 33006| ADUserAccountLockedOut| Bu olay, şirket içinde kilitlenmiş bir hesabın parolasını sıfırlamaya veya değiştirmeye çalıştığımızı gösterir. Kilitlemeler, bir kullanıcı kısa bir süre içinde parola işlemini çok fazla kez değiştirmeyi veya sıfırlamayı denediğinde oluşabilir. Hesabın kilidini açın ve işlemi yeniden deneyin.|
| 33007| ADUserYanlış Şifre| Bu olay, kullanıcının parola değiştirme işlemi gerçekleştirirken yanlış bir geçerli parola belirttiğini gösterir. Doğru geçerli parolayı belirtin ve yeniden deneyin.|
| 33008| ADPasswordPolicyError| Bu olay, parola yazma hizmeti yerel dizininizde etki alanının parola yaşını, geçmişini, karmaşıklığını veya filtreleme gereksinimlerini karşılamayan bir parola ayarlamaya çalıştığında oluşur. <br> <br> Minimum parola yaşınız varsa ve bu süre içinde parolayı yakın zamanda değiştirdiyseniz, etki alanınızda belirtilen yaşa ulaşana kadar parolayı yeniden değiştiremezsiniz. Test amacıyla, minimum yaş 0 olarak ayarlanmalıdır. <br> <br> Parola geçmişi gereksinimleriniz etkinse, *N'nin* parola geçmişi ayarı olduğu son *N* zamanlarında kullanılmayan bir parola seçmeniz gerekir. Son *N* kez kullanılan bir parola seçerseniz, bu durumda bir hata görürsünüz. Sınama amacıyla parola geçmişi 0 olarak ayarlanmalıdır. <br> <br> Parola karmaşıklığı gereksinimleriniz varsa, kullanıcı parolayı değiştirmeye veya sıfırlamaya çalıştığında bunların tümü zorlanır. <br> <br> Parola filtreleriniz etkinse ve kullanıcı filtreleme ölçütlerini karşılamayan bir parola seçerse, sıfırlama veya değiştirme işlemi başarısız olur.|
| 33009| ADConfigurationError| Bu olay, Active Directory ile ilgili bir yapılandırma sorunu nedeniyle şirket içi dizininize parola yazarken bir sorun olduğunu gösterir. Hangi hatanın oluştuğu hakkında daha fazla bilgi için ADSync hizmetinden gelen iletiler için Azure AD Connect makinesinin uygulama olay günlüğünü denetleyin.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Parola yazma bağlantısı sorunu

Azure AD Connect'in parola yazma bileşeniyle hizmet kesintileri yaşıyorsanız, bu sorunu çözmek için atabileceğiniz bazı hızlı adımlar aşağıda verilmiştir:

* [Ağ bağlantısını onaylama](#confirm-network-connectivity)
* [Azure AD Connect Sync hizmetini yeniden başlatın](#restart-the-azure-ad-connect-sync-service)
* [Parola yazma özelliğini devre dışı bırakıp yeniden etkinleştirin](#disable-and-re-enable-the-password-writeback-feature)
* [En son Azure AD Connect sürümüne yükleme](#install-the-latest-azure-ad-connect-release)
* [Parola yazma sorunu](#troubleshoot-password-writeback)

Genel olarak, hizmetinizi en hızlı şekilde kurtarmak için, bu adımları daha önce tartışılan sırada yürütmenizi öneririz.

### <a name="confirm-network-connectivity"></a>Ağ bağlantısını onaylama

En yaygın hata noktası, güvenlik duvarı ve proxy bağlantı noktaları ve boşta zaman zaman zaman ları yanlış yapılandırılmış olmasıdır. 

Azure AD Connect sürüm 1.1.443.0 ve üzeri için aşağıdakilere giden HTTPS erişimi ne kadar gerekir:

* \*.passwordreset.microsoftonline.com
* \*.servicebus.windows.net

Daha ayrıntılı bilgi için, her Çarşamba güncellenen ve bir sonraki Pazartesi yürürlüğe giren [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) güncelleştirilmiş listesine başvurun.

Daha fazla bilgi için Azure AD Connect makalesi [için Ön](../hybrid/how-to-connect-install-prerequisites.md) koşullardaki bağlantı ön koşulları gözden geçirin.

> [!NOTE]
> "Parolanın süresi hiç dolmaz" veya "Kullanıcı parolayı değiştiremez" ayarları şirket içinde AD DS'deki hesapta yapılandırılırsa SSPR de başarısız olabilir. 

### <a name="restart-the-azure-ad-connect-sync-service"></a>Azure AD Connect Sync hizmetini yeniden başlatın

Hizmetle ilgili bağlantı sorunlarını veya diğer geçici sorunları gidermek için Azure AD Bağlantı Eşitleme hizmetini yeniden başlatın:

1. Yönetici olarak, Azure AD Connect çalıştıran sunucuda **Başlat'ı** seçin.
1. Arama alanına **services.msc** girin ve **Enter'u**seçin.
1. **Microsoft Azure AD Eşitleme** girişini arayın.
1. Hizmet girişine sağ tıklayın, **Yeniden Başlat'ı**seçin ve ardından işlemin tamamlanmasını bekleyin.

   ![GUI'yi kullanarak Azure AD Eşitleme hizmetini yeniden başlatın][Service restart]

Bu adımlar, bulut hizmetiyle bağlantınızı yeniden kurun ve karşılaştığınız kesintileri giderin. ADSync hizmetini yeniden başlatmak sorununuzu çözmüyorsa, devre dışı bırakıp parola yazma özelliğini yeniden etkinleştirmenizi öneririz.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Parola yazma özelliğini devre dışı bırakıp yeniden etkinleştirin

Bağlantı sorunlarını gidermek için parola yazma özelliğini devre dışı bırakıp yeniden etkinleştirin:

   1. Yönetici olarak Azure AD Connect Yapılandırma sihirbazını açın.
   1. **Azure AD'ye Bağlan'da**Azure AD genel yönetici kimlik bilgilerinizi girin.
   1. **AD DS'ye**bağlan'da, AD Etki Alanı Hizmetleri yönetici kimlik bilgilerinizi girin.
   1. **Kullanıcılarınızı benzersiz bir şekilde tanımlarken** **İleri** düğmesini seçin.
   1. **İsteğe bağlı özelliklerde,** **Parola geri yazma** onay kutusunu temizleyin.
   1. **Yapılandırmaya Hazır** sayfasına gelene kadar hiçbir şeyi değiştirmeden kalan iletişim sayfaları aracılığıyla **İleri'yi** seçin.
   1. **Yapılandırmaya Hazır sayfasının** Parola **yazma** seçeneğini **devre dışı bırakıldığından** emin olun ve değişikliklerinizi işlemek için yeşil **Yapıla** düğmesini seçin.
   1. **Bitti'de,** **Şimdi Eşitle** seçeneğini temizleyin ve sihirbazı kapatmak için **Bitir'i** seçin.
   1. Azure AD Connect Yapılandırma sihirbazını yeniden açın.
   1. Hizmeti yeniden etkinleştirmek için **İsteğe Bağlı Özellikler** sayfasında parola **yazma** seçeneğini seçtiğinizden emin olmak dışında 2-8 adımlarını yineleyin.

Bu adımlar, bulut hizmetimizle bağlantınızı yeniden kurun ve karşılaştığınız kesintileri giderin.

Parola yazma özelliğini devre dışı bırakmak ve ardından yeniden etkinleştirmek sorununuzu çözmüyorsa, Azure AD Connect'i yeniden yüklemenizi öneririz.

### <a name="install-the-latest-azure-ad-connect-release"></a>En son Azure AD Connect sürümüne yükleme

Azure AD Connect'i yeniden yüklemek, bulut hizmetlerimizle yerel Active Directory ortamınız arasındaki yapılandırma ve bağlantı sorunlarını çözebilir.

Bu adımı yalnızca daha önce açıklanan ilk iki adımı denedikten sonra gerçekleştirmenizi öneririz.

> [!WARNING]
> Kullanıma hazır eşitleme kurallarını özelleştirmişseniz, *yükseltmeye devam etmeden önce bunları yedekleyin ve bitirdikten sonra bunları el ile yeniden dağıtın.*

1. Azure AD Connect'in en son sürümünü [Microsoft Download Center'dan](https://go.microsoft.com/fwlink/?LinkId=615771)indirin.
1. Azure AD Connect'i zaten yüklediğiniz için, Azure AD Connect yüklemenizi en son sürüme güncellemek için yerinde yükseltme yapmanız gerekir.
1. İndirilen paketi çalıştırın ve Azure AD Connect makinenizi güncellemek için ekrandaki yönergeleri izleyin.

Önceki adımlar bulut hizmetimizle bağlantınızı yeniden kurmalı ve karşılaştığınız kesintileri gidermelidir.

Azure AD Connect sunucusunun en son sürümünü yüklemek sorununuzu çözmüyorsa, en son sürümü yükledikten sonra parola yazmayı son adım olarak devre dışı bırakmayı ve ardından yeniden etkinleştirmenizi öneririz.

## <a name="verify-that-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Azure AD Connect'in parola geri yazma için gerekli izne sahip olduğunu doğrulayın

Azure AD Connect, parola geri yazmayı gerçekleştirmek için Etkin Dizin **Sıfırlama parolasını sıfırlama** izni gerektirir. Azure AD Connect'in belirli bir şirket içi Active Directory kullanıcı hesabı için gerekli izne sahip olup olmadığını öğrenmek için Windows Etkili İzin özelliğini kullanabilirsiniz:

1. Azure AD Connect sunucusunda oturum açın ve**Senkronizasyon Hizmetini Başlat'ı** **Start** > seçerek **Eşitleme Hizmeti Yöneticisi'ni** başlatın.
1. **Bağlayıcılar** sekmesinin altında, şirket içi **Etkin Dizin Etki Alanı Hizmetleri** bağlayıcısını seçin ve ardından **Özellikler'i**seçin.  
   ![Özelliklerin nasıl dolduğunu gösteren Eşitleme Servis Yöneticisi](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
  
1. Açılan pencerede, Etkin **Dizin Ormanı'na Bağlan'ı** seçin ve **Kullanıcı adı** özelliğine dikkat edin. Bu özellik, Azure AD Connect tarafından dizin eşitleme gerçekleştirmek için kullanılan AD DS hesabıdır. Azure AD Connect'in parola yazma yı gerçekleştirabilmesi için AD DS hesabının parola izinin sıfırlanması gerekir.  

   ![Eşitleme hizmeti Active Directory kullanıcı hesabını bulma](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
  
1. Şirket içi etki alanı denetleyicisinde oturum açın ve **Active Directory Users and Computers** uygulamasını başlatın.
1. **Görünüm'i** seçin ve **Gelişmiş Özellikler** seçeneğinin etkin olduğundan emin olun.  

   ![Etkin Dizin Kullanıcıları ve Bilgisayarları Gelişmiş Özellikler Gösterir](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
  
1. Doğrulamak istediğiniz Active Directory kullanıcı hesabını arayın. Hesap adını sağ tıklatın ve **Özellikler'i**seçin.  
1. Açılan pencerede **Güvenlik** sekmesine gidin ve **Gelişmiş'i**seçin.  
1. Yönetici için **Gelişmiş Güvenlik Ayarları** açılır penceresinde Etkili **Erişim** sekmesine gidin.
1. **Bir kullanıcı seçin,** Azure AD Connect tarafından kullanılan AD DS hesabını seçin (bkz. adım 3) ve ardından **etkili erişimi görüntüle'yi**seçin.

   ![Eşitleme Hesabını Gösteren Etkili Erişim sekmesi](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
  
1. Aşağı kaydırın ve **parolayı sıfırla'yı**arayın. Girişte bir onay işareti varsa, AD DS hesabının seçili Active Directory kullanıcı hesabının parolasını sıfırlama izni vardır.  

   ![Eşitleme hesabının Sıfırlama parola sıyrık iznine sahip olduğunu doğrulama](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure REKLAM forumları

Azure AD ve self servis parola sıfırlama hakkında genel bir sorunuz varsa, [Azure REKLAM forumlarında](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD)toplulukten yardım isteyebilirsiniz. Topluluğun üyeleri arasında mühendisler, ürün yöneticileri, MVP'ler ve diğer BT profesyonelleri yer almaktadır.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurma

Bir sorunun cevabını bulamazsanız, destek ekiplerimiz size daha fazla yardımcı olmak için her zaman hazırdır.

Size düzgün bir şekilde yardımcı olmak için, bir servis talebi açarken mümkün olduğunca çok ayrıntı sağlamanızı rica ediyoruz. Bu ayrıntılar şunlardır:

* **Hatanın genel açıklaması**: Hata nedir? Fark edilen davranış neydi? Hatayı nasıl yeniden üretebiliriz? Mümkün olduğunca çok ayrıntı sağlayın.
* **Sayfa**: Hatayı fark ettiğinizde hangi sayfadaydınız? Mümkünse URL'yi ve sayfanın ekran görüntüsünü ekleyin.
* **Destek kodu**: Kullanıcı hatayı gördüğünde oluşturulan destek kodu neydi?
   * Bu kodu bulmak için hatayı yeniden üretin ve ardından ekranın altındaki **Destek kodu** bağlantısını seçin ve destek mühendisine sonuçları gösteren GUID'i gönderin.

   ![Ekranın alt kısmındaki destek kodunu bulma][Support code]

  * Alt tabakada destek kodu olmayan bir sayfadaysanız, F12'yi seçin ve SID ve CID'yi arayın ve bu iki sonucu destek mühendisine gönderin.
* **Tarih, saat ve saat dilimi**: Hatanın oluştuğu saat *dilimiyle* kesin tarih ve saati ekleyin.
* **Kullanıcı Kimliği**: Hatayı gören kullanıcı kimdi? Bir örnek *\@kullanıcı contoso.com.*
   * Bu federe bir kullanıcı mı?
   * Bu bir geçiş kimlik doğrulama kullanıcısı mı?
   * Bu şifre-karma senkronize kullanıcı mı?
   * Bu yalnızca bulut kullanıcımıdır?
* **Lisanslama**: Kullanıcının atanmış bir Azure AD lisansı var mı?
* **Uygulama olay günlüğü**: Parola yazma geri ödemesini kullanıyorsanız ve hata şirket içi altyapınızdaysa, Azure AD Connect sunucusundan uygulama olay günlüğünüzün sıkıştırılmış bir kopyasını ekleyin.

[Service restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Azure AD Eşitleme hizmetini yeniden başlatın"
[Support code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Destek kodu pencerenin sağ alt kısmında yer alır"

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, Azure AD aracılığıyla parola sıfırlama hakkında ek bilgiler sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
