---
title: 'Azure AD Connect: Sürüm yayın geçmişi | Microsoft Dokümanlar'
description: Bu makalede, Azure AD Connect ve Azure AD Eşitlemi'nin tüm sürümleri listelanıyor
services: active-directory
author: billmath
manager: daveba
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 04/03/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d2e3f8da4a05feedb8c1ab585fabcc74edbc71a
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998740"
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Sürüm yayın geçmişi
Azure Etkin Dizin (Azure AD) ekibi, Azure AD Connect'i yeni özellikler ve işlevlerle düzenli olarak güncelleştirir. Tüm eklemeler tüm izleyiciler için geçerli değildir.

Bu makale, yayımlanan sürümleri izlemenize ve değişikliklerin en son sürümde ne olduğunu anlamanıza yardımcı olmak için tasarlanmıştır.

Bu tablo ilgili konuların bir listesidir:

Konu başlığı |  Ayrıntılar
--------- | --------- |
Azure AD Connect'ten yükseltme adımları | [Önceki sürümden en son](how-to-upgrade-previous-version.md) Azure AD Connect sürümüne yükseltme yapmak için farklı yöntemler.
Gerekli izinler | Güncelleştirme uygulamak için gereken izinler için [hesaplara ve izinlere](reference-connect-accounts-permissions.md#upgrade)bakın.
İndirme| [Azure AD Connect'i indirin.](https://go.microsoft.com/fwlink/?LinkId=615771)

>[!NOTE]
>Azure AD Connect'in yeni bir sürümünü yayınlamak, hizmetin çalışma işlevselliğini sağlamak için birkaç kalite denetim adımı gerektiren bir işlemdir ve bu işlemden geçerken yeni bir sürümün sürüm sayısı ve sürüm durumu en son durumu yansıtacak şekilde güncellenecektir.
Bu işlemden geçerken, sürüm numarası "1.3.X.0" olarak küçük sürüm numarası konumunda "X" ile gösterilir - bu, bu belgedeki sürüm notlarının "1.3" ile başlayan tüm sürümler için geçerli olduğunu gösterir. En kısa sürede biz sürüm sürümü numarası en son yayımlanan sürümü güncellenir ve sürüm durumu "İndirme ve otomatik yükseltme için yayımlanan" olarak güncellenecektir serbest bırakma işlemi sonuçlandırMaz.
Azure AD Connect'in tüm sürümleri otomatik yükseltme için kullanılamaz. Sürüm durumu, bir sürümün otomatik yükseltme için mi yoksa yalnızca karşıdan yükleme için mi sunulduğunu gösterir. Azure AD Connect sunucunuzda otomatik yükseltme etkinleştirilmişse, bu sunucu otomatik yükseltme için yayımlanan Azure AD Connect'in en son sürümüne otomatik olarak yükseltilir. Tüm Azure AD Connect yapılandırmalarının otomatik yükseltme için uygun olmadığını unutmayın. [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) hakkında daha fazla bilgi için lütfen bu bağlantıyı izleyin

>[!IMPORTANT]
> 1 Kasım 2020'den itibaren, 18 aydan uzun bir süre önce piyasaya sürülen Azure AD Connect sürümlerinin amortismana alınacağı bir amortisman işlemi uygulamaya başlayacağız. O zaman, Azure AD Connect'in tüm sürümlerini 24/4/2019'da yayımlanan sürüm 1.3.20.0 ve daha eski sürümlerle küçümseyerek bu işleme başlayacağız ve yeni bir sürüm her yayımlayınca Azure AD Connect'in eski sürümlerinin amortismanını değerlendirmeye devam edeceğiz.
>
> En iyi destek deneyimini elde etmek için Azure AD Connect'in en yeni sürümünü çalıştırdığınızdan emin olmanız gerekir. 
>
>Azure AD Connect'in amortismana tabi bir sürümünü çalıştırırsanız, en son güvenlik düzeltmelerine, performans iyileştirmelerine, sorun giderme ve tanılama araçlarına ve hizmet geliştirmelerine sahip olmayabilirsiniz ve desteğe ihtiyacınız varsa, kuruluşunuzun gereksinim duyduğu hizmet düzeyini size sağlayamayabiliriz.
>
>Azure AD Connect'i eşitleme için etkinleştirdiyseniz, eski sürümlerden birini çalıştırdığınızda sizi yaklaşan amortismanlar hakkında uyaran Sistem Durumu bildirimleri otomatik olarak almaya başlarsınız.
>
>Azure AD Connect'i en son sürüme nasıl yükseltilir hakkında daha fazla bilgi edinmek için lütfen [bu makaleye](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version) bakın.

## <a name="15200"></a>1.5.20.0

### <a name="release-status"></a>Yayın durumu
04/09/2020: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar
Bu düzeltme yapısı, Grup Filtreleme özelliği etkinse ve kaynak çapa olarak mS-DS-ConsistencyGuid kullanıyorsanız, build 1.5.18.0 ile ilgili bir sorunu giderir.

## <a name="15180"></a>1.5.18.0

### <a name="release-status"></a>Yayın durumu
04/02/2020: İndirmek için yayınlandı

### <a name="functional-changes-adsyncautoupgrade"></a>Fonksiyonel değişiklikler ADSyncAutoUpgrade 

- Grup nesneleri için mS-DS-ConsistencyGuid özelliği için destek eklendi. Bu, ad grubu objectID'nin değiştiği , örneğin bir REKLAM sunucusu bir felaketten sonra yeniden oluşturulduğunda, AD'deki gruplar arası grupları ormanlar veya AD'deki gruplar arasında yeniden bağlamanızı sağlar. Daha fazla bilgi için [ormanlar arasında taşıma gruplarına](how-to-connect-migrate-groups.md)bakın.
- mS-DS-ConsistencyGuid özniteliği otomatik olarak eşitlenen tüm gruplarüzerinde ayarlanır ve bu özelliği etkinleştirmek için hiçbir şey yapmanız gerekmez. 
- Artık kullanımda olmadığı için Get-ADSyncRunProfile kaldırıldı. 
- Daha fazla bağlam sağlamak için AD DS bağlayıcı hesabı için Kurumsal Yönetici veya Etki Alanı Yöneticisi hesabı kullanmaya çalışırken gördüğünüz uyarıyı değiştirdiniz. 
- Eski CSDelete.exe aracı kaldırılır ve yeni Remove-ADSyncCSObject cmdlet ile değiştirilir bağlayıcı alan nesneleri kaldırmak için yeni bir cmdlet eklendi. Remove-ADSyncCSObject cmdlet giriş olarak bir CsObject alır. Bu nesne Get-ADSyncCSObject cmdlet kullanılarak alınabilir.

>[!NOTE]
>Eski CSDelete.exe aracı kaldırıldı ve yeni Remove-ADSyncCSObject cmdlet ile değiştirildi 

### <a name="fixed-issues"></a>Düzeltilen sorunlar

- Özelliği devre dışı bırakarak Azure AD Connect sihirbazını yeniden çalıştırma konusunda grup yazma orman/OU seçicisi olan bir hata düzeltildi. 
- Gerekli DCOM kayıt defteri değerleri yeni bir yardım bağlantısı ile eksikse görüntülenecek yeni bir hata sayfası tanıtıldı. Bilgiler de günlük dosyaları için yazılır. 
- Azure Etkin Dizin eşitleme hesabının oluşturulmasıyla ilgili bir sorun giderildi, çünkü hesap kullanılmaya çalışılmadan önce tüm hizmet yinelemelerinde yayılmadı. 
- Vekil karakterleri doğru şekilde işlemeyan eşitleme hataları sıkıştırma yardımcı yardımcı lığına bir hata düzeltildi. 
- Otomatik yükseltmede sunucuyu zamanlayıcı askıya alınan durumda bırakan bir hata düzeltildi. 

## <a name="14380"></a>1.4.38.0
### <a name="release-status"></a>Yayın durumu
12/9/2019: İndiriçin yayın. Otomatik yükseltme yoluyla kullanılamaz.
### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
- Azure AD Etki Alanı Hizmetleri için Password Hash Sync'i Kerberos karmalarında dolgu için doğru şekilde hesaplamak için güncelledik.  Bu, AAD'den Azure AD Etki Alanı Hizmetlerine parola eşitleme sırasında bir performans artışı sağlar.
- Kimlik doğrulama aracısı ve servis veri kurumu arasında güvenilir oturumlar için destek ekledik.
- Bu sürüm, kimlik doğrulama aracısı ve bulut hizmetleri arasındaki iletişim için TLS 1.2'yi zorlar.
- Kimlik doğrulama aracısı ve bulut hizmetleri arasındaki websocket bağlantıları için bir DNS önbelleği ekledik.
- Aracı bağlantısını test etmek için buluttan belirli aracıyı hedefleme özelliğini ekledik.

### <a name="fixed-issues"></a>Düzeltilen sorunlar
- Sürüm 1.4.18.0 DSSO için PowerShell cmdlet ps çalışırken sağlanan yönetici kimlik bilgileri yerine giriş pencereleri kimlik bilgilerini kullanarak bir hata vardı. Bunun sonucunda DSSO'yu AADConnect kullanıcı arabirimi üzerinden birden fazla ormanda etkinleştirmek mümkün değildi. 
- AADConnect kullanıcı arabirimi üzerinden tüm ormanda aynı anda DSSO'yu etkinleştirmek için bir düzeltme yapıldı

## <a name="14320"></a>1.4.32.0
### <a name="release-status"></a>Yayın durumu
11/08/2019: İndirilme için yayınlandı. Otomatik yükseltme yoluyla kullanılamaz.

>[!IMPORTANT]
>Azure AD Connect'in bu sürümündeki dahili şema değişikliği nedeniyle, MSOnline PowerShell kullanarak AD FS güven ilişkisi yapılandırma ayarlarını yönetiyorsanız, MSOnline PowerShell modülünüzü 1.1.183.57 veya daha yüksek sürümde güncellemeniz gerekir

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu sürüm, varolan Karma Azure AD'nin birleştirilmiş aygıtlarıyla ilgili bir sorunu giderir. Bu sürüm, bu sorunu düzelten yeni bir aygıt eşitleme kuralı içerir.
Bu kural değişikliğinin eski aygıtların Azure AD'den silinmesine neden olabileceğini unutmayın. Bu aygıt nesneleri Koşullu Erişim yetkilendirmesi sırasında Azure AD tarafından kullanılmadığı için bu endişe verici bir neden değildir. Bazı müşteriler için, bu kural değişikliği yle silinecek aygıt sayısı silme eşiğini aşabilir. Azure AD'de aygıt nesnelerinin Silme Silme Eşiğini aştığını görürseniz, silme işleminin geçmesine izin vermeniz önerilir. [Silme eşiğini aştıklarında silmelerin akmasına nasıl izin vereneniz](how-to-connect-sync-feature-prevent-accidental-deletes.md)

## <a name="14250"></a>1.4.25.0

### <a name="release-status"></a>Yayın durumu
28.09.2019: Kiracı seçmek için otomatik yükseltme için serbest bırakıldı. İndirilmeye hazır değil.

Bu sürüm, önceki sürümden 1.4.18.0'a otomatik olarak yükseltilen ve Self servis parola sıfırlama (SSPR) ve Parola Yazma ile ilgili sorunlarla karşılaşan bazı sunucuların bulunduğu bir hatayı giderir.

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Belirli koşullar altında, sürüm 1.4.18.0'a otomatik olarak yükseltilen sunucular, yükseltme tamamlandıktan sonra Self servis parola sıfırlama ve Parola Yazma'yı yeniden etkinleştirmedi. Bu otomatik yükseltme sürümü bu sorunu giderir ve Self servis parola sıfırlama ve Parola Yazma'yı yeniden etkinleştirir.

Vekil karakterleri doğru şekilde işlemeyan eşitleme hataları sıkıştırma yardımcı yardımcı lığına bir hata düzelttik.

## <a name="14180"></a>1.4.18.0

>[!WARNING]
>Azure AD Connect'in bu sürümüne yükselttikten sonra bazı müşterilerin mevcut Karma Azure AD'si bağlanan aygıtlarla ilgili bir sorun yaşadığı bir olayı araştırıyoruz. Karma Azure AD'yi dağıtan müşterilerin, bu sorunların temel nedeni tam olarak anlaşılıp hafifletilene kadar bu sürüme yükseltmeyi ertelemelerini öneriyoruz. Daha fazla bilgi en kısa sürede sağlanacaktır.

>[!IMPORTANT]
>Azure AD Connect'in bu sürümüyle bazı müşteriler Windows aygıtlarının bir kısmının veya tamamının Azure AD'den kaybolduğunu görebilir. Bu aygıt kimlikleri Koşullu Erişim yetkilendirmesi sırasında Azure AD tarafından kullanılmadığı için bu endişe verici bir neden değildir. Daha fazla bilgi için bkz: [Azure AD Connect 1.4.xx.x aygıtının kaybolmasını anlama](reference-connect-device-disappearance.md)


### <a name="release-status"></a>Yayın durumu
25.09.2019: Yalnızca otomatik yükseltme için piyasaya sürüldü.

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
- Yeni sorun giderme aracı, "kullanıcı eşitleme", "grup eşitleme" veya "grup üyesi eşitleme değil" senaryoları sorun giderme yardımcı olur.
- AAD Connect sorun giderme komut dosyasında ulusal bulutlar için destek ekleme 
- Müşteriler, MIIS_Service için amortismana alınan WMI uç noktalarının artık kaldırıldığı konusunda bilgilendirilmelidir. Herhangi bir WMI işlemleri artık PS cmdlets ile yapılmalıdır.
- AZUREADSSOACC nesnesi üzerinde kısıtlı delegasyonu sıfırlayarak güvenlik geliştirme
- Eşitleme kuralı eklerken/düzenlerken, konektör şemasında bulunan ancak bağlayıcıya eklenmemiş bir öznitelik varsa, öznitelikler otomatik olarak bağlayıcıya eklenir. Aynı kural etkiler nesne türü için de geçerlidir. Bağlayıcıya bir şey eklenirse, bağlayıcı bir sonraki eşitleme döngüsünde tam alma için işaretlenir.
- Yeni Azure AD Connect Dağıtımlarında artık bağlayıcı hesap olarak bir Kuruluş veya Etki Alanı yöneticisinin kullanılması desteklenmez. Bağlayıcı hesap olarak bir Enterprise veya Domain yöneticisini kullanan geçerli AAD Connect dağıtımları bu sürümden etkilenmez.
- Eşitleme Yöneticisi'nde kural oluşturma/edit/silme üzerinde tam bir eşitleme çalıştırılır. Tam alma veya tam eşitleme çalıştırılacak olup olmadığını kullanıcıya bildiren herhangi bir kural değişikliğinde açılır pencere görünür.
- 'Bağlayıcılar > özellikleri > bağlantı' sayfasına parola hataları için azaltma adımları eklendi
- Bağlayıcı özellikleri sayfasında eşitleme hizmeti yöneticisi için bir amortisman uyarısı eklendi. Bu uyarı, kullanıcıya Azure AD Connect sihirbazı aracılığıyla değişiklik yapılması gerektiğini belirtir.
- Kullanıcının parola ilkesiyle ilgili sorunlar için yeni hata eklendi.
- Grup filtrelemenin etki alanı ve OU filtreleri tarafından yanlış yapılandırılmasını önleyin. Grup filtreleme, girilen grubun etki alanı/OU'su zaten filtrelendiğinde bir hata gösterir ve sorun çözülene kadar kullanıcının ilerlemesini engellenir.
- Kullanıcılar artık Eşitleme Hizmet Yöneticisi Kullanıcı Arabirimi'nde Active Directory Etki Alanı Hizmetleri veya Windows Azure Etkin Dizini için bir bağlayıcı oluşturamaz.
- Eşitleme Hizmet Yöneticisi'nde özel kullanıcı bir iki kullanıcı ayrılık çalışma denetimlerinin sağlanabilirliği.
- Azure AD Connect'te tüm oturum açma yöntemleri için altı federasyon yönetimi görevi etkinleştirildi.  (Daha önce, tüm oturum açmalar için yalnızca "AD FS TLS/SSL sertifikasını güncelleştir" görevi mevcuttu.)
- Oturum açma yöntemini federasyondan PHS veya PTA'ya değiştirirken, tüm Azure AD etki alanlarının ve kullanıcılarının yönetilen kimlik doğrulamasına dönüştürüleceği konusunda bir uyarı eklendi.
- "Azure AD ve AD FS güvenini sıfırla" görevinden belirteç imzalama sertifikaları kaldırıldı ve bu sertifikaları güncelleştirmek için ayrı bir alt görev eklendi.
- AD FS çiftliği için TLS veya belirteç imzalama sertifikalarını güncelleştirmek için alt görevleri olan "Sertifikaları yönetme" adı verilen yeni bir federasyon yönetimi görevi eklendi.
- Yöneticilerin AD FS çiftliği için yeni bir birincil sunucu belirtmesine olanak tanıyan "Birincil sunucu belirt" adlı yeni bir federasyon yönetimi alt görevi eklendi.
- AD FS sunucusu dağıtmak, Bir Web Application Proxy sunucusu dağıtmak ve birincil sunucubelirtmek için alt görevleri olan "Sunucuları yönet" adlı yeni bir federasyon yönetimi görevi eklendi.
- Geçerli AD FS ayarlarını görüntüleyen "Federasyon yapılandırmasını görüntüle" adlı yeni bir federasyon yönetimi görevi eklendi.  (Bu ek nedeniyle, AD FS ayarları "Çözümünüzü gözden geçir" sayfasından kaldırılmıştır.)

### <a name="fixed-issues"></a>Düzeltilen sorunlar
- İlgili kişi nesnesini devralan bir kullanıcı nesnesinin kendi kendine referansı olduğu (örn. kullanıcı kendi yöneticisidir) senaryosu için çözülmüş eşitleme hatası sorunu.
- Yardım açılır şimdi klavye odak göstermek.
- Otomatik yükseltme için, herhangi bir çakışan uygulama 6 saat çalışıyorsa, öldürmek ve yükseltme ile devam edin.
- Dizin uzantıları seçerken müşterinin nesne başına seçebileceği öznitelik sayısını 100 ile sınırlayın. Azure nesne başına en fazla 100 uzantı özniteliğine sahip olduğu için, bu hatanın dışa aktarma sırasında oluşmasını önler.
- AD Bağlantısı komut dosyasını daha sağlam hale getirmek için bir hata düzeltildi
- AADConnect'in varolan bir Adlandırılmış Borular WCF hizmetini kullanarak bir makineye yüklemesini sağlamak için bir hata düzeltildi.
- ADSync hizmetinin başlangıçta yüklendiğinde başlamasına izin vermeyen grup ilkeleri etrafında geliştirilmiş tanılama ve sorun giderme.
- Windows bilgisayarının görüntü adının yanlış yazıldığı bir hata düzeltildi.
- Windows bilgisayarı için işletim sistemi türünün yanlış yazıldığı bir hatayı düzeltin.
- Windows olmayan 10 bilgisayarların beklenmedik şekilde eşitlendiği bir hata düzeltildi. Bu değişikliğin etkisinin, daha önce eşitlenmiş Windows-10 olmayan bilgisayarların artık silineceğini unutmayın. Windows bilgisayarlarının eşitleme yalnızca Windows-10 aygıtları için çalışan Karma Azure AD etki alanı birleştirme için kullanıldığından, bu herhangi bir özelliği etkilemez.
- ADSync PowerShell modülüne birkaç yeni (dahili) cmdlet eklendi.


## <a name="13210"></a>1.3.21.0
>[!IMPORTANT]
>Azure AD Connect'i önceki bir sürümden 1.3.21.0'a yükseltmeyle ilgili bilinen bir sorun vardır ve Azure AD Connect, Azure AD Connect başarıyla yükseltilmiş olsa bile güncelleştirilmiş sürümü yansıtmamaktadır.
>
> Bunu çözmek için **AdSync** modülünü içe aktarmanız ve ardından Azure AD Connect sunucusunda`Set-ADSyncDirSyncConfiguration` PowerShell cmdlet'i çalıştırmanız gerekir.  Aşağıdaki adımları kullanabilirsiniz:
>
>1. PowerShell'i administator modunda açın.
>2. `Import-Module "ADSync"` öğesini çalıştırın.
>3. `Set-ADSyncDirSyncConfiguration -AnchorAttribute ""` öğesini çalıştırın.
 
### <a name="release-status"></a>Yayın durumu 

14.05.2019: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- Microsoft Azure Active Directory Connect derleme1.3.20.0'da bulunan ayrıcalık güvenlik açığı yüksekliği düzeltildi.  Bu güvenlik açığı, belirli koşullar altında, saldırganın ayrıcalıklı bir hesap bağlamında iki PowerShell cmdlet yürütmesine ve ayrıcalıklı eylemler gerçekleştirmesine izin verebilir.  Bu güvenlik güncelleştirmesi, bu cmdlets devre dışı bırakarak sorunu giderir. Daha fazla bilgi için [güvenlik güncelleştirmesi](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1000)bakın.

## <a name="13200"></a>1.3.20.0 

### <a name="release-status"></a>Yayın durumu 

24.04.2019: İndirmek için yayınlandı

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 

- Etki Alanı Yenileme için destek ekleyin 
- Exchange Mail Ortak Klasörler özelliği GA gider 
- Hizmet hataları için sihirbaz hata işlemeyi geliştirme 
- Bağlayıcı özellikleri sayfasında Eşitleme Servis Yöneticisi UI'de uyarı bağlantısı eklendi. 
- Birleşik Gruplar Writeback özelliği artık GA 
- DC bir LDAP denetimi eksik olduğunda Geliştirilmiş SSPR hata iletisi 
- Yükleme sırasında DCOM kayıt defteri hataları için tanılama eklendi  
- PHS RPC hatalarının daha iyi izlenmesi 
- Bir alt etki alanından EA'nın eA dinlerine izin verme 
- Yükleme sırasında veritabanı adının girilmesine izin ver (varsayılan ad ADSync)
- Ping için WS-Trust düzeltmesi almak ve yeni Azure örnekleri için destek eklemek için ADAL 3.19.8'e yükseltin 
- SamAccountName, DomainNetbios ve DomainFQDN'yi buluta akacak şekilde Grup Eşitleme Kurallarını değiştirin - talepler için gerekli 
- Modifiye Varsayılan Eşitleme Kural Taşıma – [burada](how-to-connect-fix-default-rules.md)daha fazla bilgi edinin .
- Windows hizmeti olarak çalışan yeni bir aracı eklendi. "Yönetici Aracı" adlı bu aracı, bir destek servis talebi açtığınızda Microsoft Engineers sorun gidermesine yardımcı olmak için Azure AD Connect sunucusunun daha derin uzaktan tanılamasağlar. Bu aracı yüklenmez ve varsayılan olarak etkinleştirilir.  Aracının nasıl yüklenir ve etkinleştirilir hakkında daha fazla bilgi için Azure [AD Connect Yönetici Aracısı nedir?](whatis-aadc-admin-agent.md) 
- Son Kullanıcı Lisans Sözleşmesi (EULA) güncellendi 
- Giriş türü olarak AD FS kullanan dağıtımlar için otomatik yükseltme desteği eklendi.  Bu, yükseltme işleminin bir parçası olarak AD FS Azure AD Relying Party Trust'ı güncelleştirme gereksinimini de ortadan kaldırdı. 
- İki seçenek sunan bir Azure AD güven yönetimi görevi eklendi: güveni çözümle/güncelleştirin ve güveni sıfırla. 
- AD FS Azure AD Relying Party güven davranışını her zaman -SupportMultipleDomain anahtarını (güven ve Azure AD etki alanı güncelleştirmeleri içerir) kullanabilmesi için değiştirdi. 
- Önceden yüklenmiş bir sertifika kullanma seçeneğini kaldırarak bir .pfx sertifikası gerektirmesi için yeni AD FS farm davranışını yükle değiştirildi.
- Yükleme yeni AD FS çiftlik iş akışını yalnızca 1 AD FS ve 1 WAP sunucusunun dağıtılmasına izin sağlayacak şekilde güncelleştirin.  Tüm ek sunucular ilk yüklemeden sonra yapılacaktır. 

### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- ADSync hizmeti için SQL yeniden bağlantı mantığını düzeltme 
- Boş bir SQL AOA DB kullanarak temiz Yükleme izin vermek için düzeltme 
- GWB izinlerini hassaslaştırmak için PS İzinleri komut dosyasını düzeltme 
- LocalDB ile VSS Hatalarını Düzeltme  
- Nesne türü kapsam içinde değilken yanıltıcı hata iletisini düzeltme 
- Azure AD PowerShell'in bir sunucuya yüklenmesinin Azure AD Connect ile bir derleme çakışmasına neden olabileceği bir sorun düzeltildi. 
- Bağlayıcı Kimlik Bilgileri Eşitleme Hizmet Yöneticisi Kullanıcı Arabirimi'nde güncelleştirildiğinde Evreleme Sunucusu'ndaki PHS hatası düzeltildi. 
- Bazı bellek sızıntıları düzeltildi 
- Çeşitli Autoupgrade düzeltmeleri 
- İhracat ve Onaylanmamış İthalat İşleme için Çeşitli düzeltmeler 
- Etki Alanı ve OU filtreleme bir ters eğik çizgi işleme ile bir hata düzeltildi 
- ADSync hizmetinin durdurulmasının 2 dakikadan fazla sürdüğünü ve yükseltme sırasında soruna neden olduğu bir sorun giderildi. 




## <a name="12700"></a>1.2.70.0

### <a name="release-status"></a>Yayın durumu

12.18.2018: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu yapı, Azure AD Connect ile gönderilen standart dışı bağlayıcıları (örneğin, Genel LDAP Bağlayıcısı ve Genel SQL Bağlayıcısı) güncelleştirir. İlgili bağlayıcılar hakkında daha fazla bilgi için Bağlayıcı Sürüm Yayın [Geçmişi'ndeki](/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-version-history)sürüm 1.1.911.0'a bakın.


## <a name="12690"></a>1.2.69.0

### <a name="release-status"></a>Yayın durumu
12/11/2018: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar
Bu düzeltme yapısı, kullanıcının aygıt geri yazılmasını etkinleştirirken Kayıtlı Aygıtlar kapsayıcısı için belirtilen orman içinde bir hedef etki alanı seçmesine olanak tanır.  Yeni Aygıt Seçenekleri işlevini (1.1.819.0 – 1.2.68.0) içeren önceki sürümlerde, Kayıtlı Aygıtlar konteyner konumu orman köküyle sınırlıydı ve alt etki adlarına izin vermedi.  Bu sınırlama yalnızca yeni dağıtımlarda kendini gösterdi – yerinde yükseltmeler etkilenmedi.  

Güncelleştirilmiş Aygıt Seçenekleri işlevini içeren herhangi bir yapı yeni bir sunucuya dağıtıldıysa ve aygıt yazma özelliği etkinleştirildiyse, orman kökünde istemiyorsanız kapsayıcının konumunu el ile belirtmeniz gerekir.  Bunu yapmak için, aygıtın geri yazılmasını devre dışı bırakmanız ve "Writeback forest" sayfasında kapsayıcı konumunu belirtmenize olanak tanıyan yeniden etkinleştirmeniz gerekir.



## <a name="12680"></a>1.2.68.0

### <a name="release-status"></a>Yayın durumu 

30.11.2018: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu düzeltme yapısı, senkronizasyon sunucusundaki MSOnline PowerShell Gallery modülünün bağımsız varlığı nedeniyle kimlik doğrulama hatasının oluşabileceği bir çakışmayı giderir.



## <a name="12670"></a>1.2.67.0

### <a name="release-status"></a>Yayın durumu 

11.11.2018: İndirmek için yayınlandı

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu düzeltme yapısı, Windows Server 2008/R2'de ADDS Etki Alanı Denetleyicisi kullanırken Parola Yazma'nın başarısız olduğu önceki yapıdaki bir gerilemeyi giderir.

## <a name="12650"></a>1.2.65.0 

### <a name="release-status"></a>Yayın durumu 

25.10.2018: indirmek için yayınlandı

 
### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 


- Barındırılan sesli postanın beklendiği gibi çalıştığından emin olmak için öznitelik yazma nın işlevselliği değiştirildi.  Belirli senaryolarda, Azure AD geçersiz bir değerle geri yazma sırasında msExchUcVoicemailSettings özniteliğinin üzerine yazıyordu.  Bulut değeri ayarlanmadığı takdirde Azure AD artık bu özniteliğin şirket içi değerini temizlemez.
- Azure AD'ye Bağlantı sorunlarını araştırmak ve tanımlamak için Azure AD Connect sihirbazında tanılama eklendi. Bu aynı tanılama, Test- AdSyncAzureServiceConnectivity Cmdlet kullanılarak doğrudan PowerShell üzerinden de çalıştırılabilir. 
- AD'ye Bağlantı sorunlarını araştırmak ve tanımlamak için Azure AD Connect sihirbazında tanılama eklendi. Bu aynı tanılama, ADConnectivityTools PowerShell modülündeki Start-ConnectivityValidation işlevini kullanarak doğrudan PowerShell üzerinden de çalıştırılabilir.  Daha fazla bilgi için [bkz: ADConnectivityTool PowerShell Modülü nedir?](how-to-connect-adconnectivitytools.md)
- Karma Azure Active Directory Join ve aygıt geri yazma için AD şema sürümü ön denetimi eklendi 
- Dizin Uzantısı sayfası özniteliği aramasını büyük/küçük harf duyarlı olmayacak şekilde değiştirdi.
-   TLS 1.2 için tam destek eklendi. Bu sürüm, diğer tüm protokollerin devre dışı bırakılmasını ve Azure AD Connect'in yüklendiği makinede yalnızca TLS 1.2'nin etkinleştirildiğini destekler.  Daha fazla bilgi [için Azure AD Connect için TLS 1.2 uygulamasına](reference-connect-tls-enforcement.md) bakın

 

### <a name="fixed-issues"></a>Düzeltilen sorunlar   

- SQL Always On kullanılıyorsa Azure AD Connect Yükseltmesi'nin başarısız olacağı bir hata düzeltildi. 
- İleri eğik çizgi içeren OU adlarını doğru şekilde ayrışdırmak için hata düzeltildi. 
- Geçiş Kimlik Doğrulama'nın evreleme modunda temiz bir yükleme için devre dışı bırakıldığı bir sorun düzeltildi. 
- Sorun giderme araçlarını çalıştırırken PowerShell modülünün yüklenmesini engelleyen bir hata düzeltildi 
- Müşterilerin ana bilgisayar adının ilk karakterinde sayısal değerleri kullanmasını engelleyen bir hata düzeltildi. 
- Azure AD Connect'in geçersiz bölümlere ve kapsayıcı seçimine izin vereceği bir hata düzeltildi 
- Desktop SSO etkinleştirildiğinde "Geçersiz Parola" hata iletisi düzeltildi. 
- AD FS Güven Yönetimi için çeşitli Hata düzeltmeleri  
- Device Writeback yapılandırırken - msD'ler-DeviceContainer nesne sınıfını aramak için şema denetimi düzeltildi (WS2012 R2'de tanıtıldı)

 
## <a name="118820"></a>1.1.882.0  

07.09.2018: indirmek için yayımlanan, otomatik yükseltme için serbest olmayacak 

### <a name="fixed-issues"></a>Düzeltilen sorunlar  

SQL Her Zaman Kullanılabilirlik'te ADSync DB için yapılandırılırsa Azure AD Connect Yükseltmesi başarısız olur. Bu düzeltme bu sorunu giderir ve Yükseltme'nin başarılı olmasını sağlar. 

## <a name="118800"></a>1.1.880.0

### <a name="release-status"></a>Yayın durumu

21.08.2018: İndirme ve otomatik yükseltme için yayımlandı. 

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- Azure AD Connect'teki Ping Fetorat tümleştirmesi artık Genel Kullanılabilirlik için kullanılabilir. [Ping Federate ile Azure AD federe nasıl hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#federation-with-pingfederate)
- Azure AD Connect artık her güncelleştirme yapıldığında AD FS'de Azure AD güveninin yedeğini oluşturur ve gerekirse kolay geri yükleme için ayrı bir dosyada saklar. [Azure AD Connect'te yeni işlevsellik ve Azure AD güven yönetimi hakkında daha fazla bilgi edinin.](https://aka.ms/fedtrustinaadconnect)
- Yeni sorun giderme aracı, değişen birincil e-posta adresi ve hesabı genel adres listesinden gizleme sorunu gidermenize yardımcı olur
- Azure AD Connect en son SQL Server 2012 Yerel İstemciyi içerecek şekilde güncellendi
- "Kullanıcı oturum açma"da kullanıcı oturum açma veya Geçiş Kimlik Doğrulama'ya geçiş yaptığınızda, Sorunsuz Tek Oturum Açma onay kutusu varsayılan olarak etkinleştirilir.
- Windows Server Essentials 2019 için destek eklendi
- Azure AD Connect Health aracısı en son 3.1.7.0 sürümüne güncelleştirildi
- Yükseltme sırasında, yükleyici varsayılan eşitleme kurallarında yapılan değişiklikleri algılarsa, değiştirilen kuralları üzerine yazmadan önce yöneticiye bir uyarı istenir. Bu, kullanıcının düzeltici eylemlerde bulunup daha sonra devam etmesine olanak sağlar. Eski Davranış: Değiştirilen kutu dışı kural varsa, el ile yükseltme kullanıcıya herhangi bir uyarı vermeden bu kuralların üzerine yazıldı ve eşitleme zamanlayıcısı kullanıcıya bilgi vermeden devre dışı bırakıldı. Yeni Davranış: Değiştirilen kutudan çıkış eşitleme kurallarının üzerine yazmadan önce kullanıcıya uyarı istenir. Kullanıcı, düzeltici eylemde bulunduktan sonra yükseltme işlemini durdurma ve daha sonra devam etme seçeneğine sahip olacaktır.
- FIPS uyumlu bir ortamda MD5 karma oluşturma için bir hata iletisi ve bu sorun için geçici bir çözüm sağlayan belgelere bağlantı sağlayarak, bir FIPS uyumluluk sorununun daha iyi işlenmesini sağlayın.
- Kullanıcı Arabirimi güncelleştirmesi, şimdi federasyon için ayrı bir alt grup altında olan sihirbazdaki federasyon görevlerini geliştirmek için güncelleştirme. 
- Tüm federasyon ek görevleri artık kullanım kolaylığı için tek bir alt menü altında gruplandırılır.
- Yeni adSyncConfig Posh Modülü (AdSyncConfig.psm1) yeni AD İzinleri fonksiyonları ile eski ADSyncPrep.psm1 (kısa bir süre içinde deprecated olabilir) taşındı

### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- .NET 4.7.2'ye yükselttikten sonra AAD Connect sunucusunun yüksek CPU kullanımını göstereceği bir hata düzeltildi
- Otomatik çözümlenmiş bir SQL kilitlemi sorunu için zaman zaman hata iletisi üretecek bir hata düzeltildi
- Eşitleme Kuralları Düzenleyicisi ve Eşitleme Hizmeti Yöneticisi için çeşitli erişilebilirlik sorunları düzeltildi  
- Azure AD Connect'in kayıt defteri ayarı bilgilerini alabildiği bir hata düzeltildi
- Kullanıcı sihirbaza ileri/geri gittiğinde sorun yaratan bir hata düzeltildi
- Sihirbazda yanlış çoklu iş parçacığı teslimi nedeniyle bir hata oluşmasını önlemek için bir hata düzeltildi
- Grup Eşitleme Filtreleme sayfası güvenlik gruplarını çözerken bir LDAP hatasıyla karşılaştığında, Azure AD Connect artık tam doğrulukla özel durumu döndürür.  Başvuru özel durum için temel nedeni hala bilinmiyor ve farklı bir hata tarafından ele alınacaktır.
-  STK ve NGC tuşlarıiçin izinlerin (WHfB için Kullanıcı/Aygıt nesnelerindeki ms-DS-KeyCredentialLink özniteliği) doğru ayarlanmadığı bir hata düzeltildi.     
- 'Set-ADSyncRestrictedPermissions' doğru çağrılmadığını bir hata düzeltildi
-  AADConnect'in yükleme sihirbazında Grup Yazma Geri Yüklemesi'nde izin verme desteği ekleme
- Parola Karma Eşitleme'den AD FS'ye oturum açma yöntemini değiştirirken, Parola Hash Eşitlemesi devre dışı bırakılmadı.
- AD FS yapılandırmasında IPv6 adresleri için ek doğrulama
- Varolan bir yapılandırmanın var olduğunu bildirmek için bildirim iletisini güncelleştirildi.
- Aygıt geri yazımı, güvenilmeyen ormandaki kapsayıcıyı algılamaz. Bu daha iyi bir hata iletisi ve uygun belgelere bir bağlantı sağlamak için güncellendi
- Ou'nun seçiminin ve ardından bu OU'ya karşılık gelen eşitleme/yazma geri alma nın seçilmesi genel bir eşitleme hatası verir. Bu, daha anlaşılır bir hata iletisi oluşturmak için değiştirildi.

## <a name="118190"></a>1.1.819.0

### <a name="release-status"></a>Yayın durumu

14.05.2018: Otomatik yükseltme ve indirme için yayınlandı.

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

Yeni özellikler ve geliştirmeler

- Bu sürüm, PingFederate'nin Azure AD Connect'e entegrasyonunun genel önizlemesini içerir. Bu sürümle müşteriler, Federasyon sağlayıcıları olarak PingFederate'den yararlanmak için Azure Active Directory ortamlarını kolayca ve güvenilir bir şekilde yapılandırabilirler. Bu yeni özelliği nasıl kullanacağınız hakkında daha fazla bilgi edinmek için lütfen [çevrimiçi belgelerimizi](plan-connect-user-signin.md#federation-with-pingfederate)ziyaret edin. 
- Artık Bağlantılı Posta Kutuları ve AD Dinamik Grupları gibi daha fazla hata senaryosunu analiz ettiği Azure AD Connect Sihirbazı Sorun Giderme Yardımcı Programı güncelleştirildi. Sorun giderme yardımcı programı hakkında daha fazla bilgi [için burada.](tshoot-connect-objectsync.md)
- Aygıt Yazma Yapılandırması artık yalnızca Azure AD Bağlantı Sihirbazı içinde yönetildi.
- SQL Bağlantı sorunlarını ve diğer çeşitli sorun giderme yardımcı yardımcılarını gidermek için kullanılabilecek ADSyncTools.psm1 adlı yeni bir PowerShell Modülü eklendi. [BURADA](tshoot-connect-tshoot-sql-connectivity.md)ADSyncTools modülü hakkında daha fazla bilgi edinin. 
- Yeni bir ek görev "Aygıt seçeneklerini yapılandırmak" eklendi. Aşağıdaki iki işlemi yapılandırmak için görevi kullanabilirsiniz: 
  - **Karma Azure AD join**: Ortamınızda şirket içi AD ayak izi varsa ve Azure Active Directory tarafından sağlanan özelliklerden de yararlanmak istiyorsanız, karma Azure AD birleştirilmiş aygıtlar uygulayabilirsiniz. Bunlar hem şirket içi Active Directory alanınıza hem de Azure Active Directory alanınıza katılmış cihazlardır.
  - **Aygıt geri yazma**: Aygıt geri yazımı, aygıtlara dayalı Koşullu Erişimi AD FS (2012 R2 veya daha yüksek) korumalı aygıtlara etkinleştirmek için kullanılır

    >[!NOTE] 
    > - Özelleştir eşitleme seçeneklerinden aygıt geri yazılmasını etkinleştirme seçeneği soluk hale getirilir. 
    > -  ADPrep için PowerShell modülü bu sürümle amortismana hazırdır.



### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- Bu sürüm, SQL Server Express yüklemesini SQL Server 2012 SP4'e güncelleştirir ve bu güncelleştirmeler, diğerlerinin yanı sıra çeşitli güvenlik açıkları için düzeltmeler sağlar.  SQL Server 2012 SP4 hakkında daha fazla bilgi için [lütfen buraya](https://support.microsoft.com/help/4018073/sql-server-2012-service-pack-4-release-information) bakın.
- Eşitleme Kuralı İşleme: üst eşitleme kuralı artık geçerli değilse, Birleştirme Koşulu olmayan giden Birleştirme eşitleme kuralları uygulanmamalıdır
- Eşitleme Hizmeti Yöneticisi Kullanıcı Arabirimi ve Eşitleme Kuralları Düzenleyicisi'ne çeşitli erişilebilirlik düzeltmeleri uygulandı
- Azure AD Bağlantı Sihirbazı: Azure AD Connect bir çalışma grubundayken AD Bağlayıcıhesabı oluşturma hatası
- Azure AD Bağlantı Sihirbazı: Azure AD Oturum Açma sayfasında, AD etki alanlarında ve Azure AD Doğrulanmış etki alanlarında herhangi bir uyumsuzluk olduğunda doğrulama onay kutusunu görüntüler
- Otomatik yükseltme girişiminden sonra belirli durumlarda otomatik yükseltme durumunu doğru ayarlamak için PowerShell düzeltmesini otomatik yükseltme.
- Azure AD Bağlantı Sihirbazı: Daha önce eksik olan bilgileri yakalamak için telemetri güncelleştirildi
- Azure AD Bağlantı Sihirbazı: AD FS'den Geçiş Kimlik Doğrulaması'na geçmek için **Kullanıcı oturum açma** görevini değiştir'i kullandığınızda aşağıdaki değişiklikler yapılmıştır:
    - Geçiş Kimlik Doğrulama Aracısı Azure AD Connect sunucusuna yüklenir ve federeden yönetilene etki alanı(lar) dönüştürmeden önce Geçiş Kimlik Doğrulama özelliği etkinleştirilir.
    - Kullanıcılar artık federe den yönetilen dönüştürülür. Yalnızca etki alanı(lar) dönüştürülür.
- Azure AD Connect Sihirbazı: AD FS Multi Domain Regex, kullanıcı UPN'de özel karakterleri desteklemek için özel karakter Regex güncellemesi olduğunda doğru değildir
- Azure AD Bağlantı Sihirbazı: Sahte "Kaynak bağlantı özelliğini yapılandırma" iletisi değiştirilmediğinde kaldırın 
- Azure AD Connect Sihirbazı: Çift federasyon senaryosu için AD FS desteği
- Azure AD Bağlantı Sihirbazı: Yönetilen bir etki alanını federe ye dönüştürürken AD FS Talepleri ek etki alanı için güncelleştirilmez
- Azure AD Bağlantı Sihirbazı: Yüklü paketlerin algılanması sırasında, eski Dirsync/Azure AD Sync/Azure AD Connect ile ilgili ürünleri buluruz. Şimdi bayat ürünleri kaldırmaya çalışacağız.
- Azure AD Bağlantı Sihirbazı: Geçiş kimlik doğrulama aracısının yüklenmesi başarısız olduğunda Hata İletisi Eşlemesi'ni düzeltin
- Azure AD Bağlantı Sihirbazı: Alan Adı OU Filtreleme sayfasından kaldırılan "Yapılandırma" kapsayıcısı
- Sync Engine yüklemesi: Zaman zaman Sync Engine yüklemek msi başarısız gereksiz eski mantık kaldırmak
- Azure AD Bağlantı Sihirbazı: Parola Karma Eşitlemi için İsteğe Bağlı Özellikler sayfasındaki açılır pencere yardım metnini düzeltme
- Eşitleme Motoru çalışma süresi: CS nesnesinin içe aktarıldığı ve Eşitleme Kuralları'nın nesneyi yeniden sağlama girişiminde bulunduğu senaryoyu düzeltin.
- Eşitleme Motoru çalışma süresi: Alma Hatası için olay günlüğüne Çevrimiçi bağlantı sorun giderme kılavuzu için yardım bağlantısı ekleyin
- Eşitleme Motoru çalışma süresi: Konektörleri sayısallandırırken Eşitleme Zamanlayıcısının azaltılmış bellek kullanımı
- Azure AD Bağlantı Sihirbazı: AD Okuma ayrıcalığı olmayan özel bir Eşitleme Hizmeti Hesabı'nı çözen bir sorunu düzeltme
- Azure AD Bağlantı Sihirbazı: Etki Alanı ve OU filtreleme seçimlerinin günlüğe kaydetmesini geliştirin
- Azure AD Bağlantı Sihirbazı: AD FS MFA senaryosu için oluşturulan federasyon güvenine varsayılan talepler ekleme
- Azure AD Connect Sihirbazı: AD FS Deploy WAP: Sunucu ekleme yeni sertifika kullanamıyor
- Azure AD Bağlantı Sihirbazı: PremCredentials bir etki alanı için başlatılmadığınızda DSSO özel durumu 
- Tercihen Etkin Kullanıcı nesnesinden AD distinguishedName özniteliği akışı.
- Bir kozmetik hata ilk OOB Sync Kuralı Önceliği 100 yerine 99 olarak ayarlandı sabit



## <a name="117510"></a>1.1.751.0
Durum 4/12/2018: Yalnızca indirmek için yayımlandı

>[!NOTE]
>Bu sürüm, Azure AD Connect için bir düzeltmedir

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Eşitleme
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
Düzeltilen bir sorun, Çin kiracılar için otomatik Azure örnek bulma zaman zaman başarısız oldu.  

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issues"></a>Düzeltilen sorunlar

Yapılandırma yeniden deneme mantığında bir argumentexception belirten bir sorun vardı "aynı anahtara sahip bir öğe zaten eklendi."  Bu, tüm yeniden deneme işlemlerinin başarısız lığa neden olur.

## <a name="117500"></a>1.1.750.0
Durum 22/3/2018: Otomatik yükseltme ve indirme için yayınlandı.
>[!NOTE]
>Bu yeni sürüme yükseltme tamamlandığında, Azure AD bağlayıcısı için tam eşitleme ve tam alma ve AD bağlayıcısı için tam eşitleme otomatik olarak tetikler. Azure AD Connect ortamınızın boyutuna bağlı olarak bu işlem biraz zaman alabilir, bunu desteklemek için gerekli adımları attığınızdan veya bunu yapmak için uygun bir an bulana kadar yükseltmeyi erteledinizden emin olun.

>[!NOTE]
>"Otomatik Yükseltme işlevi, 1.1.524.0'dan sonra yapı dağıtan bazı kiracılar için yanlış devre dışı bırakıldı. Azure AD Connect örneğinizin Otomatik Yükseltme için hala uygun olduğundan emin olmak için aşağıdaki PowerShell cmdlet'i çalıştırın: "Set-ADSyncAutoUpgrade -AutoupGradeState Etkin"


### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Set-ADSyncAutoUpgrade cmdlet, otomatik yükseltme durumu Askıya Alındı olarak ayarlanmışsa, daha önce Otomatik Yükseltme'yi engellerdi. Bu işlev şimdi değiştirildi, bu nedenle gelecekteki yapılar Otomatik Yükseltme engellemez.
* Kullanıcı **Oturum Açma** sayfası seçeneği "Parola Eşitleme" "Password Hash Synchronization" olarak değiştirildi.  Azure AD Connect parolaları değil parola varrıları eşitler, bu nedenle bu durum gerçekte meydana gelen lerle hizalanır.  Daha fazla bilgi için bkz: [Azure AD Connect eşitlemesi ile parola karma senkronizasyonu uygula](how-to-connect-password-hash-synchronization.md)

## <a name="117490"></a>1.1.749.0
Durum: Belirli müşteriler için serbest bırakıldı

>[!NOTE]
>Bu yeni sürüme yükseltme tamamlandığında, Azure AD bağlayıcısı için tam eşitleme ve tam alma ve AD bağlayıcısı için tam eşitleme otomatik olarak tetikler. Azure AD Connect ortamınızın boyutuna bağlı olarak bu işlem biraz zaman alabilecek olduğundan, lütfen bunu desteklemek için gerekli adımları attığınızdan veya bunu yapmak için uygun bir an bulana kadar yükseltmeyi erteledinizden emin olun.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Bir sonraki sayfaya geçerken Bölüm Filtreleme sayfası için arka plan görevlerinde zamanlama penceresini düzeltin.

* ConfigDB özel eylemi sırasında Access ihlaline neden olan bir hata düzeltildi.

* SQL bağlantı zaman amıkurtarmak için bir hata düzeltildi.

* SAN joker karakterli sertifikaların ön koşul denetiminde başarısız olduğu bir hata düzeltildi.

* Azure AD bağlayıcısı dışa aktarMası sırasında miiserver.exe'nin çökmesine neden olan bir hata düzeltildi.

* Yapılandırmayı değiştirmek için Azure AD Connect sihirbazını çalıştırırken DC'de hatalı parola denemesi günlüğe kaydedilmiş bir hata düzeltildi.


#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Genel Veri Koruma Yönetmeliği (GDPR) için Gizlilik Ayarları ekleme.  Daha fazla bilgi için [buraya](reference-connect-user-privacy.md)makaleye bakın.

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]  

* uygulama telemetri - admin bu veri sınıfını açık/kapalı olarak

* Azure AD Sağlık verileri - yönetici, sistem durumu ayarlarını kontrol etmek için sistem durumu portalını ziyaret etmelidir.
   Hizmet ilkesi değiştirildikten sonra, aracılar bunu okur ve uygular.

* Eklenen aygıt geri yazma yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu

* ZIP-Text / HTML Raporunda HTML raporu ve tam veri toplama ile geliştirilmiş Genel Tanılama

* Otomatik yükseltme nin güvenilirliği artırıldı ve sunucunun durumunun belirlenebilmesini sağlamak için ek telemetri eklendi

* AD Bağlayıcısı hesabında ayrıcalıklı hesaplariçin kullanılabilen izinleri kısıtlama

  * Yeni yüklemeler için sihirbaz, MSOL hesabını oluşturduktan sonra ayrıcalıklı hesapların MSOL hesabında sahip olduğu izinleri kısıtlar.

Değişiklikler aşağıdakileri halledecektir:
1. Ekspres Kurulumlar
2. Otomatik Oluşturma hesabı ile Özel Yüklemeler
3. Azure AD Connect'in temiz yüklemesi için SA ayrıcalığı gerektirmeyeceğiniz şekilde yükleyici değiştirildi

* Belirli bir nesne için eşitleme sorunlarını gidermek için yeni bir yardımcı program eklendi. Azure AD Connect Sihirbazı Sorun Giderme Ek Görevi'nin 'Sorun Giderme Nesne Eşitleme' seçeneği altında kullanılabilir. Şu anda, yardımcı program aşağıdakileri denetler:

  * UserPrincipalName, senkronize edilmiş kullanıcı nesnesi ile Azure AD Kiracı'daki kullanıcı hesabı arasındaki uyuşmazlık.
  * Nesne etki alanı filtreleme nedeniyle eşitleme den filtrelenirse
  * Kuruluş birimi (OU) filtreleme nedeniyle nesne eşitlemeden filtrelenirse

* Belirli bir kullanıcı hesabı için şirket içi Active Directory'de depolanan geçerli parola karmasını eşitlemek için yeni bir yardımcı program eklendi.

Yardımcı program parola değişikliği gerektirmez. Azure AD Connect Sihirbazı Sorun Giderme Ek Görevi'nin 'Parola Karma Senkronizasyonu' seçeneği altında kullanılabilir.






## <a name="116540"></a>1.1.654.0
Durum: 12 Aralık 2017

>[!NOTE]
>Bu sürüm, Azure AD Connect için güvenlikle ilgili bir düzeltmedir

### <a name="azure-ad-connect"></a>Azure AD Connect
Azure AD Connect sürümü 1.1.654.0 (ve sonrası) için, Azure AD Connect AD DS hesabını oluşturduğunda [AD DS hesabına erişimi kilitle](#lock) bölümünde açıklanan önerilen izin değişikliklerinin otomatik olarak uygulandığından emin olmak için bir geliştirme eklendi. 

- Azure AD Connect'i ayarlarken, yükleme yöneticisi varolan bir AD DS hesabı sağlayabilir veya Hesabı otomatik olarak Azure AD Connect'in oluşturmasına izin verebilir. İzin değişiklikleri, kurulum sırasında Azure AD Connect tarafından oluşturulan AD DS hesabına otomatik olarak uygulanır. Bunlar yükleme yöneticisi tarafından sağlanan varolan AD DS hesabına uygulanmaz.
- Azure AD Connect'in eski bir sürümünden 1.1.654.0'a (veya sonra) yükseltilen müşteriler için, izin değişiklikleri yükseltmeden önce oluşturulan mevcut AD DS hesaplarına geriye dönük olarak uygulanmaz. Bunlar yalnızca yükseltmeden sonra oluşturulan yeni AD DS hesaplarına uygulanır. Bu, Azure AD'ye eşitlenecek yeni AD ormanları eklediğinizde oluşur.

>[!NOTE]
>Bu sürüm, yalnızca hizmet hesabının yükleme işlemi tarafından oluşturulduğu Azure AD Connect'in yeni yüklemelerinin güvenlik açığını kaldırır. Varolan yüklemeler veya hesabı kendiniz sağladığınız durumlarda, bu güvenlik açığının bulunmadığından emin olmalısınız.

#### <a name="lock-down-access-to-the-ad-ds-account"></a><a name="lock"></a>AD DS hesabına erişimi kilitleme
Şirket içi AD'de aşağıdaki izin değişikliklerini uygulayarak AD DS hesabına erişimi kilitleyin:  

*   Belirtilen nesnede devralmayı devre dışı
*   SELF'e özgü ACE'ler dışında, belirli bir nesnedeki tüm ACE'leri kaldırın. SELF söz konusu olduğunda varsayılan izinleri sağlam tutmak istiyoruz.
*   Bu özel izinleri atama:

Tür     | Adı                          | Erişim               | Uygulanan Öğe
---------|-------------------------------|----------------------|--------------|
İzin Ver    | SİSTEM                        | Tam Denetim         | Bu nesne  |
İzin Ver    | Enterprise Admins             | Tam Denetim         | Bu nesne  |
İzin Ver    | Etki Alanı Yöneticileri                 | Tam Denetim         | Bu nesne  |
İzin Ver    | Yöneticiler                | Tam Denetim         | Bu nesne  |
İzin Ver    | Kurumsal Etki Alanı Denetleyicileri | Liste İçerikleri        | Bu nesne  |
İzin Ver    | Kurumsal Etki Alanı Denetleyicileri | Tüm Özellikleri Oku  | Bu nesne  |
İzin Ver    | Kurumsal Etki Alanı Denetleyicileri | İzinleri Okuma     | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | Liste İçerikleri        | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | Tüm Özellikleri Oku  | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | İzinleri Okuma     | Bu nesne  |

AD DS hesabının ayarlarını sıkılaştırmak için [bu PowerShell komut dosyasını](https://gallery.technet.microsoft.com/Prepare-Active-Directory-ef20d978)çalıştırabilirsiniz. PowerShell komut dosyası, yukarıda belirtilen izinleri AD DS hesabına atar.

#### <a name="powershell-script-to-tighten-a-pre-existing-service-account"></a>Önceden varolan bir hizmet hesabını sıkılaştırmak için PowerShell komut dosyası

PowerShell komut dosyasını kullanmak, bu ayarları uygulamak için önceden varolan bir AD DS hesabına (kuruluşunuz tarafından sağlanan veya Azure AD Connect'in önceki bir yüklemesi tarafından oluşturulan eter) lütfen komut dosyasını yukarıdaki bağlantıdan indirin.

##### <a name="usage"></a>Kullanım:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN <$ObjectDN> -Credential <$Credential>
```

Konum 

**$ObjectDN** = İzinleri sıkılaştırılması gereken Active Directory hesabı.

**$Credential** = $ObjectDN hesabındaki izinleri kısıtlamak için gerekli ayrıcalıklara sahip yönetici kimlik bilgileri. Bu ayrıcalıklar genellikle Enterprise veya Domain yöneticisi tarafından tutulur. Hesap arama hatalarını önlemek için yönetici hesabının tam nitelikli alan adını kullanın. Örnek: contoso.com\admin.

>[!NOTE] 
>$credential. Kullanıcı Adı FQDN\username biçiminde olmalıdır. Örnek: contoso.com\admin 

##### <a name="example"></a>Örnek:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN "CN=TestAccount1,CN=Users,DC=bvtadwbackdc,DC=com" -Credential $credential 
```
### <a name="was-this-vulnerability-used-to-gain-unauthorized-access"></a>Bu güvenlik açığı yetkisiz erişim elde etmek için kullanıldı mı?

Bu güvenlik açığının Azure AD Connect yapılandırmanızı tehlikeye atmak için kullanIlip kullanılmadığından görmek için hizmet hesabının son parola sıfırlama tarihini doğrulamanız gerekir.  Beklenmeyen zaman damgası ise, daha fazla araştırma, olay günlüğü üzerinden, bu parola sıfırlama olayı için, üstlenilmelidir.

Daha fazla bilgi için Microsoft [Güvenlik Danışma 4056318](https://technet.microsoft.com/library/security/4056318)

## <a name="116490"></a>1.1.649.0
Durum: 27 Ekim 2017

>[!NOTE]
>Bu yapı, Azure AD Connect Otomatik Yükseltme özelliği aracılığıyla müşteriler tarafından kullanılamaz.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issue"></a>[https://github.com/Azure/azure-powershell/issues/6814]() sorunu düzeltildi
* Azure AD Connect ile Azure AD Connect Sistem Durumu Aracısı (eşitleme için) arasındaki sürüm uyumluluğu sorunu giderildi. Bu sorun, Sürüm 1.1.647.0'a yerinde yükseltme yapan müşterileri etkiler, ancak şu anda Sağlık Aracısı sürümü 3.0.127.0'a sahiptir. Yükseltmeden sonra, Sistem Durumu Aracısı artık Azure AD Bağlantı Senkronizasyon Hizmeti hakkındaki sistem durumu verilerini Azure AD Sağlık Hizmeti'ne gönderemez. Bu düzeltmeyle, Azure AD Connect yerinde yükseltme sırasında Sağlık Aracısı sürümü 3.0.129.0 yüklenir. Health Agent sürümü 3.0.129.0, Azure AD Connect sürümü 1.1.649.0 ile uyumluluk sorunu içermez.


## <a name="116470"></a>1.1.647.0
Durum: 19 Ekim 2017

> [!IMPORTANT]
> Azure AD Connect sürüm 1.1.647.0 ile Azure AD Connect Sağlık Aracısı (eşitleme için) sürüm 3.0.127.0 arasında bilinen bir uyumluluk sorunu vardır. Bu sorun, Sistem Durumu Aracısı'nın Azure AD Bağlantı Eşitleme Hizmeti (nesne eşitleme hataları ve geçmiş verilerini çalıştırma dahil) hakkında sistem durumu verilerini Azure AD Sistem Durumu Hizmeti'ne göndermesini engeller. Azure AD Connect dağıtımınızı sürüm 1.1.647.0'a el ile yükseltmeden önce, lütfen Azure AD Connect sunucunuzda yüklü olan Azure AD Connect Health Agent'ın geçerli sürümünü doğrulayın. Bunu *Denetim Masası → Programları Kaldır Ekle'ye* giderek yapabilir ve *Eşitleme için Microsoft Azure AD Connect Health Agent*uygulamasını arayabilirsiniz. Sürümü 3.0.127.0 ise, yükseltmeden önce bir sonraki Azure AD Connect sürümünün kullanılabilir olmasını beklemeniz önerilir. Sağlık Aracısı sürümü 3.0.127.0 değilse, kılavuz, yerinde yükseltme ile devam etmek iyidir. Bu sorunun salıncak yükseltmesini veya Azure AD Connect'in yeni yüklemesini gerçekleştiren müşterileri etkilemediğini unutmayın.
>
>

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect sihirbazında *kullanıcı oturum açma* görevini değiştir ile ilgili bir sorun giderildi:

  * Sorun, Parola Eşitleme **etkinleştirilmiş**mevcut bir Azure AD Connect dağıtımı varsa ve kullanıcı oturum açma yöntemini *Geçiş Kimlik Doğrulaması*olarak ayarlamaya çalıştığınızda oluşur. Değişiklik uygulanmadan önce sihirbaz , "*Parola Eşitlemesi Devre Dışı"* istemini yanlış gösterir. Ancak, değişiklik uygulandıktan sonra Parola Eşitlemesi etkin kalır. Bu düzeltmeyle sihirbaz artık komut istemini göstermez.

  * Tasarım gereği sihirbaz, kullanıcı oturum açma yöntemini *Değiştir kullanıcı oturum açma* görevini kullanarak kullanıcı oturum açma yöntemini güncelleştirdiğinizde Parola Eşitlemesi'ni devre dışı etmez. Bu, parola eşitlemesini tutmak isteyen müşterilerin, birincil kullanıcı oturum açma yöntemi olarak Geçiş Kimlik Doğrulamasını veya federasyona olanak sağlamasına rağmen kesintiye uğramasını önlemek içindir.
  
  * Kullanıcı oturum açma yöntemini güncelleştirdikten sonra Parola Eşitlemesi'ni devre dışı kılmış olmak istiyorsanız, sihirbazda *Eşitleme Yapılandırması* görevini özelleştirmeniz gerekir. *İsteğe Bağlı özellikler* sayfasına gidince, *Parola Eşitleme* seçeneğinin işaretlerini kaldırın.
  
  * Sorunsuz Tek Oturum Açma'yı etkinleştirmeye/devre dışı etmeye çalışırsanız, aynı sorunun da oluştuğunu unutmayın. Özellikle, Parola Eşitleme etkin leştirilmiş mevcut bir Azure AD Connect dağıtımınız vardır ve kullanıcı oturum açma yöntemi zaten *Geçiş Kimlik Doğrulaması*olarak yapılandırıldı. Kullanıcı *oturum açma* görevini değiştir seçeneğini kullanarak, Kullanıcı oturum açma yöntemi "Geçiş Kimlik Doğrulaması" olarak yapılandırılırken *Sorunsuz Tek Oturum Açma* seçeneğini etkinleştir seçeneğini denetlemeyi/denetlemeyi denersiniz. Değişiklik uygulanmadan önce sihirbaz , "*Parola Eşitlemesi Devre Dışı"* istemini yanlış gösterir. Ancak, değişiklik uygulandıktan sonra Parola Eşitlemesi etkin kalır. Bu düzeltmeyle sihirbaz artık komut istemini göstermez.

* Azure AD Connect sihirbazında *kullanıcı oturum açma* görevini değiştir ile ilgili bir sorun giderildi:

  * Sorun, Parola Eşitleme **devre dışı**bırakılmış varolan bir Azure AD Connect dağıtımınız olduğunda ve kullanıcı oturum açma yöntemini Geçiş *Kimlik Doğrulaması*olarak ayarlamaya çalıştığınızda oluşur. Değişiklik uygulandığında, sihirbaz hem Geçiş Kimlik Doğrulaması hem de Parola Eşitleme'yi sağlar. Bu düzeltmeyle sihirbaz artık Parola Eşitlemesini etkinleştirmiyor.

  * Önceden, Parola Eşitleme Pass-through Kimlik Doğrulaması etkinleştirmek için bir ön koşuldu. Kullanıcı oturum açma yöntemini *Geçiş Kimlik Doğrulaması*olarak ayarladığınızda, sihirbaz hem Geçiş Kimlik Doğrulamasını hem de Parola Eşitlemesini etkinleştiriniz. Son zamanlarda, Parola Eşitleme bir ön koşul olarak kaldırıldı. Azure AD Connect sürüm 1.1.557.0'ın bir parçası olarak, kullanıcı oturum açma yöntemini *Geçiş Kimlik Doğrulaması*olarak ayarladığınızda Parola Senkronizasyonunu etkinleştirmemek için Azure AD Connect'te değişiklik yapıldı. Ancak, değişiklik yalnızca Azure AD Connect yüklemesine uygulandı. Bu düzeltmeyle, aynı değişiklik kullanıcı *oturum açma* görevine de uygulanır.
  
  * Sorunsuz Tek Oturum Açma'yı etkinleştirmeye/devre dışı etmeye çalışırsanız, aynı sorunun da oluştuğunu unutmayın. Özellikle, Parola Eşitleme devre dışı bırakılmış mevcut bir Azure AD Connect dağıtımınız var ve kullanıcı oturum açma yöntemi zaten *Geçiş Kimlik Doğrulaması*olarak yapılandırıldı. Kullanıcı *oturum açma* görevini değiştir seçeneğini kullanarak, Kullanıcı oturum açma yöntemi "Geçiş Kimlik Doğrulaması" olarak yapılandırılırken *Sorunsuz Tek Oturum Açma* seçeneğini etkinleştir seçeneğini denetlemeyi/denetlemeyi denersiniz. Değişiklik uygulandığında, sihirbaz Parola Eşitlemesini sağlar. Bu düzeltmeyle sihirbaz artık Parola Eşitlemesini etkinleştirmiyor. 

* Azure AD Connect yükseltmesinin hatayla başarısız olması "*Eşitleme Hizmeti'ni yükseltemeyen*bir sorun giderildi. Ayrıca, Eşitleme Hizmeti artık olay hatası ile başlatamaz "*Veritabanı sürümü yüklenen ikili sürümüdaha yeni olduğu için hizmet başlatılamadı*". Sorun, yükseltmeyi gerçekleştiren yöneticinin Azure AD Connect tarafından kullanılan SQL sunucusunda sysadmin ayrıcalığına sahip olmaması yla oluşur. Bu düzeltmeyle Azure AD Connect, yöneticinin yükseltme sırasında ADSync veritabanına db_owner ayrıcalığına sahip olmasını gerektirir.

* [Sorunsuz Tek Oturum Açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)özelliğini etkinleştiren müşterileri etkileyen bir Azure AD Connect Yükseltme sorunu düzeltildi. Azure AD Connect yükseltildikten sonra, özellik etkin ve tam olarak işlevsel kalsa bile, Azure AD Connect sihirbazında Sorunsuz Tek Oturum Açma yanlış olarak görünür. Bu düzeltmeyle, özellik artık sihirbazda etkinleştirilen doğru şekilde görünür.

* Kaynak Çapa ile ilgili hiçbir değişiklik yapılmamış olsa bile, Azure AD Connect sihirbazı'nın *Yapılandırın* Kaynak Çapayı Yapılandırmaya Hazır sayfasındaher zaman "*Yapılandır Kaynak Çapa*" istemini göstermesine neden olan bir sorun düzeltildi.

* Azure AD Connect'in yerinde olarak yeniden yükseltmesini gerçekleştirirken, müşterinin ilgili Azure AD kiracısının Genel Yönetici kimlik bilgilerini sağlaması gerekir. Daha önce, Genel Yönetici'nin kimlik bilgileri farklı bir Azure AD kiracısına ait olsa bile yükseltme devam edebilir. Yükseltme başarıyla tamamlanmış gibi görünse de, yükseltme yle ilgili bazı yapılandırmalar doğru şekilde kalıcı değildir. Bu değişiklikle sihirbaz, sağlanan kimlik bilgileri Azure AD kiracısıyla eşleşmiyorsa yükseltmenin devam etmesini engeller.

* Zorunlu olarak yeniden başlatılan Gereksiz mantık, el ile yükseltmenin başlangıcında Azure AD Connect Health hizmetini yeniden başlattı.


#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Microsoft Germany Cloud ile Azure AD Connect'i ayarlamak için gereken adımları basitleştirmek için mantık eklendi. Daha önce, bu makalede açıklandığı gibi Microsoft Germany Cloud ile doğru çalışması için Azure AD Connect sunucusundaki belirli kayıt defteri anahtarlarını güncelleştirmeniz gerekir. Azure AD Connect, kurulum sırasında sağlanan genel yönetici kimlik bilgilerini temel alarak kiracınızın Microsoft Almanya Bulut'ta olup olmadığını otomatik olarak algılayabilir.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
> [!NOTE]
> Not: Eşitleme Hizmeti, kendi özel zamanlayıcınızı geliştirmenize olanak tanıyan bir WMI arabirimine sahiptir. Bu arabirim artık amortismana hazır ve 30 Haziran 2018 tarihinden sonra gönderilen Azure AD Connect'in gelecekteki sürümlerinden kaldırılacaktır. Eşitleme zamanlamasını özelleştirmek isteyen müşteriler [yerleşik zamanlayıcıyı](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler)kullanmalıdır.

#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect sihirbazı şirket içi Active Directory'deki değişiklikleri eşitlemek için gereken AD Bağlayıcısı hesabını oluşturduğunda, hesabı PublicFolder nesnelerini okumak için gereken izni doğru şekilde atamaz. Bu sorun hem Express yüklemeve Özel yükleme etkiler. Bu değişiklik sorunu giderir.

* Windows Server 2016'dan çalışan yöneticiler için Azure AD Connect Sihirbazı sorun giderme sayfasının doğru şekilde işlemene neden olan bir sorun düzeltildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Azure AD Connect sihirbazı sorun giderme sayfasını kullanarak Parola Eşitleme sorunu giderme de sorun giderme sayfası artık etki alanına özgü durumu döndürür.

* Daha önce, Parola Karma Eşitlemesini etkinleştirmeye çalıştıysanız, Azure AD Connect, AD Bağlayıcısı hesabının şirket içi AD'den parola karmalarını eşitlemek için izin gerekip gerekmediğini doğrulamaz. Artık, Azure AD Connect sihirbazı, AD Bağlayıcısı hesabının yeterli izinleri yoksa sizi doğrular ve uyarır.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issue"></a>[https://github.com/Azure/azure-powershell/issues/6814]() sorunu düzeltildi
* Kaynak Bağlantı özelliği olarak [ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) kullanımı ile ilgili bir sorun giderildi. Bu sorun, kullanıcı oturum açma yöntemi olarak *Federasyon'u AD FS ile* yapılandıran müşterileri etkiler. Sihirbazda *Kaynak Bağlantı Yapılandırma* görevini yürüttüğünde, Azure AD Connect değişmezId için kaynak özniteliği olarak *ms-DS-ConsistencyGuid kullanmaya geçer. Bu değişikliğin bir parçası olarak Azure AD Connect, AD FS'de ImmutableId için talep kurallarını güncelleştirmeye çalışır. Ancak, Azure AD Connect AD FS yapılandırmak için gerekli yönetici kimlik bilgilerine sahip olmadığından bu adım başarısız oldu. Bu düzeltmeyle Azure AD Connect, *Yapılandırılan Kaynak Bağlantı* görevini yürüttüğünde AD FS için yönetici kimlik bilgilerini girmenizi ister.



## <a name="116140"></a>1.1.614.0
Durum: 05 Eylül 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bilinen sorunlar
* Azure AD Connect yükseltmesinin "*Eşitleme Hizmetini yükseltilemiyor*" hatasıyla başarısız olmasına neden olan bilinen bir sorun vardır. Ayrıca, Eşitleme Hizmeti artık olay hatası ile başlatamaz "*Veritabanı sürümü yüklenen ikili sürümüdaha yeni olduğu için hizmet başlatılamadı*". Sorun, yükseltmeyi gerçekleştiren yöneticinin Azure AD Connect tarafından kullanılan SQL sunucusunda sysadmin ayrıcalığına sahip olmaması yla oluşur. Dbo izinleri yeterli değildir.

* Azure AD Connect Yükseltmesi ile ilgili olarak, [Sorunsuz Tek Oturum Açma](how-to-connect-sso.md)özelliğini etkinleştiren müşterileri etkileyen bilinen bir sorun vardır. Azure AD Connect yükseltildikten sonra, özellik etkin kalsa bile özellik sihirbazda devre dışı bırakılmış olarak görünür. Bu sorun için bir düzeltme gelecekteki sürümde sağlanacaktır. Bu ekran sorunu yla ilgili endişeleriniz olan müşteriler, sihirbazda Sorunsuz Tek Oturum Açma'yı etkinleştirerek bu sorunu el ile düzeltebilir.

#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect'in şirket içi AD FS'deki talep kurallarını güncelleştirmesini engelleyen ve [kaynak bağlantı özelliği olarak ms-DS-ConsistencyGuid'i](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) etkinleştiren bir sorun giderildi. Ad FS'yi oturum açma yöntemi olarak yapılandıran varolan bir Azure AD Connect dağıtımının özelliğini etkinleştirmeye çalışırsanız sorun oluşur. Sihirbaz, AD FS'deki talep kurallarını güncelleştirmeye çalışmadan önce ADFS kimlik bilgilerini istenmediği için sorun oluşur.
* Şirket içi AD ormanı NTLM'yi devre dışı bıraktıysa, Azure AD Connect'in yüklemede başarısız olmasına neden olan bir sorun giderildi. Sorun, Azure AD Connect sihirbazının Kerberos kimlik doğrulaması için gerekli güvenlik bağlamlarını oluştururken tam nitelikli kimlik bilgileri sağlamamasıdır. Bu, Kerberos kimlik doğrulamasına ve Azure AD Connect sihirbazına ait olmayan bir hatanın NTLM kullanmaya geri dönmesine neden olur.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Etiket özniteliği doldurulmuyorsa yeni eşitleme kuralının oluşturulamayacağı bir sorun düzeltildi.
* Azure AD Connect'in, Kerberos kullanılabilir olmasına rağmen NTLM kullanarak şirket içi Parola Eşitleme REKLAMı'na bağlanmasına neden olan bir sorun giderildi. Şirket içi AD topolojisi yedeklemeden geri yüklenen bir veya daha fazla etki alanı denetleyicisi varsa, bu sorun oluşur.
* Yükseltmeden sonra tam eşitleme adımlarını gereksiz yere gerçekleştirmesine neden olan bir sorun giderildi. Genel olarak, kutudan çıkma eşitleme kurallarında değişiklikler varsa, yükseltmeden sonra tam eşitleme adımlarının çalıştırılması gerekir. Sorun, change detection mantığında yeni satır karakterleri ile eşitleme kuralı ifadesiyle karşılaştığında bir değişikliği yanlış algılayan bir hatadan kaynaklanıyordu. Yeni satır karakterleri okunabilirliği artırmak için eşitleme kuralı ifadesine eklenir.
* Otomatik Yükseltme'den sonra Azure AD Connect sunucusunun düzgün çalışmamasına neden olabilecek bir sorun düzeltildi. Bu sorun, sürüm 1.1.443.0 (veya daha önceki) Azure AD Connect sunucularını etkiler. Sorunla ilgili ayrıntılar için Azure AD Connect makalesine bakın [otomatik yükseltmeden sonra düzgün çalışmıyor.](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade)
* Hatalarla karşılaşıldığında Otomatik Yükseltme'nin her 5 dakikada bir yeniden denenmesine neden olabilecek bir sorun giderildi. Düzeltme ile, hatalarla karşılaşıldığında otomatik yükseltme üstel geri dönüşle yeniden çalışır.
* Parola eşitleme olayı 611'in Windows Application Event günlüklerinde **hata**yerine **bilgilendirme** olarak gösterildiği bir sorun düzeltildi. Parola eşitleme bir sorunla karşılaştığında olay 611 oluşturulur. 
* Azure AD Connect sihirbazında Grup yazma geri leme özelliğinin Grup geri lemesi için gerekli bir OU seçmeden etkinleştirilmesine olanak tanıyan bir sorun giderildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Ek Görevler altında Azure AD Connect sihirbazına bir Sorun Giderme görevi eklendi. Müşteriler, parola eşitlemeyle ilgili sorunları gidermek ve genel tanılama toplamak için bu görevden yararlanabilir. Gelecekte, Sorun Giderme görevi diğer dizin eşitleme ile ilgili sorunları içerecek şekilde genişletilecektir.
* Azure AD Connect artık **Varolan Veritabanını Kullan**adlı yeni bir yükleme modunu destekliyor. Bu yükleme modu, müşterilerin varolan bir ADSync veritabanını belirten Azure AD Connect'i yüklemesine olanak tanır. Bu özellik hakkında daha fazla bilgi için [varolan bir veritabanını kullan](how-to-connect-install-existing-database.md)makalesine bakın.
* Azure AD Connect, gelişmiş güvenlik için artık dizin eşitleme için Azure AD'ye bağlanmak için TLS1.2'yi kullanmaya varsayılan olarak sunulur. Daha önce, varsayılan TLS1.0 oldu.
* Azure AD Connect Parola Eşitleme Aracısı başlatıldığında, parola eşitleme için Azure AD'nin tanınmış bitiş noktasına bağlanmaya çalışır. Başarılı bir bağlantı olduğunda, bölgeye özgü bir bitiş noktasına yönlendirilir. Daha önce, Parola Eşitleme Aracısı yeniden başlatılıncaya kadar bölgeye özgü bitiş noktasını önbelleğe aldatır. Şimdi, aracı önbelleği temizler ve bölgeye özgü bitiş noktasıyla bağlantı sorunuyla karşılaşırsa iyi bilinen bitiş noktasıyla yeniden dener. Bu değişiklik, önbelleğe alınmış bölgeye özgü bitiş noktası artık kullanılamadığında parola eşitlemesi farklı bir bölgeye özgü bitiş noktasına geçebilmesini sağlar.
* Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. (i) AD DS hesabını kendiniz oluşturabilir ve kimlik bilgilerini Azure AD Connect'e verebilir veya (ii) bir Kurumsal Yöneticinin kimlik bilgilerini sağlayabilir ve Azure AD Connect'in sizin için AD DS hesabını oluşturmasına izin verebilirsiniz. Daha önce ( i) Azure AD Connect sihirbazındaki varsayılan seçenektir. Şimdi, (ii) varsayılan seçenektir.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Microsoft Azure Kamu Bulutu ve Microsoft Cloud Germany için ek destek.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* AD prep PowerShell modülündeki Initialize-ADSyncNGCKeysWriteBack cmdlet, aygıt kayıt kabına yanlış bir şekilde ALA'lar uyguluyordu ve bu nedenle yalnızca varolan izinleri devralacak.  Bu, eşitleme hizmeti hesabının doğru izinlere sahip olması için güncelleştirildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* AAD Connect Verify ADFS Giriş görevi, yalnızca ADFS'den giriş belirteç alımını değil, Microsoft Online'a karşı girişleri doğrulayacak şekilde güncelleştirildi.
* AAD Connect'i kullanarak yeni bir ADFS çiftliği kurarken, ADFS kimlik bilgilerini isteyen sayfa taşınmış ve kullanıcıdan ADFS ve WAP sunucuları sağlaması istenmeden önce bu sayfa nın oluşması sağlanmıştır.  Bu, AAD Connect'in belirtilen hesabın doğru izinlere sahip olup olmadığını denetlemesine olanak tanır.
* AAD Connect yükseltmesi sırasında, ADFS AAD Trust güncelleştirmezse artık bir yükseltmede başarısız olmayacağız.  Bu durumda, kullanıcıya uygun bir uyarı iletisi gösterilir ve AAD Connect ek görevi aracılığıyla güveni sıfırlamaya devam etmelidir.

### <a name="seamless-single-sign-on"></a>Sorunsuz Tek Oturum Açma
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* [Sorunsuz Tek Oturum Açma'yı](how-to-connect-sso.md)etkinleştirmeye çalışırsanız Azure AD Connect sihirbazı'nın hata döndürmesine neden olan bir sorun giderildi. Hata iletisi *"Microsoft Azure AD Connect Kimlik Doğrulama Aracısı Yapılandırması başarısız oldu."* Bu sorun, bu [makalede](how-to-connect-pta-upgrade-preview-authentication-agents.md)açıklanan adımlara bağlı olarak [Geçiş Kimlik Doğrulama Kimlik Doğrulaması](how-to-connect-sso.md) Için Kimlik Doğrulama Aracıları önizleme sürümünü el ile yükseltmiş olan varolan müşterileri etkiler.


## <a name="115610"></a>1.1.561.0
Durum: 23 Temmuz 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>[https://github.com/Azure/azure-powershell/issues/6814]() sorunu düzeltildi

* Kutudan çıkan eşitleme kuralının kaldırılmasına neden olan "Out to AD - User ImmutableId" kuralının kaldırılmasına neden olan bir sorun düzeltildi:

  * Sorun, Azure AD Connect yükseltildiğinde veya Azure AD Connect sihirbazındaki görev seçeneği Güncelleştirme *Eşitleme Yapılandırması* kullanıldığında Oluşur.
  
  * Bu eşitleme kuralı, [kaynak bağlantı özelliği olarak ms-DS-ConsistencyGuid'i](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor)etkinleştiren müşteriler için geçerlidir. Bu özellik sürüm 1.1.524.0 ve sonrası tanıtıldı. Eşitleme kuralı kaldırıldığında, Azure AD Connect artık ObjectGuid öznitelik değeriyle şirket içi AD ms-DS-TutarlılıkGuid özniteliği dolduramaz. Yeni kullanıcıların Azure AD'ye dahil olmasını engellemez.
  
  * Düzeltme, özellik etkinleştirilme den sonra, eşitleme kuralının yükseltme sırasında veya yapılandırma değişikliği sırasında artık kaldırılmamasını sağlar. Bu sorundan etkilenen varolan müşteriler için düzeltme, Azure AD Connect'in bu sürümüne yükseltme yapıldıktan sonra eşitleme kuralının yeniden eklenmesini de sağlar.

* Kutu dan yoksun eşitleme kurallarının 100'den az öncelik değerine sahip olması yla ilgili bir sorun düzeltildi:

  * Genel olarak, öncelik değerleri 0 - 99 özel eşitleme kuralları için ayrılmıştır. Yükseltme sırasında, kutu dan sıyrık eşitleme kurallarının öncelik değerleri eşitleme kuralı değişikliklerini karşılamak üzere güncelleştirilir. Bu sorun nedeniyle, kutudan çıkma eşitleme kuralları 100'den az öncelik değeri atanabilir.
  
  * Düzeltme, yükseltme sırasında sorunun oluşmasını önler. Ancak, sorundan etkilenen varolan müşteriler için öncelik değerlerini geri yüklemez. Gelecekte restorasyona yardımcı olmak için ayrı bir düzeltme sağlanacaktır.

* Azure AD Connect sihirbazındaki [Etki Alanı ve OU Filtreleme ekranının,](how-to-connect-install-custom.md#domain-and-ou-filtering) OU tabanlı filtreleme etkin olsa bile tüm etki alanlarını ve *OU'ları* seçili olarak Eşitle seçeneğini gösterdiği bir sorun giderildi.

*   Eşitleme Hizmet Yöneticisi'ndeki [Yapılandırma Dizin Bölümleri ekranının](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) *Yenileme* düğmesi tıklatıldığında bir hatayı döndürmesine neden olan bir sorun düzeltildi. Hata iletisi *"Etki alanlarını yenilerken bir hatayla karşılaşıldı: 'System.Collections.ArrayList' türünden bir nesneyi 'Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject' yazmak için atamayan bir hatadır.* Hata, varolan bir AD ormanına yeni AD etki alanı eklendiğinde ve Azure AD Connect'i Yenile düğmesini kullanarak güncelleştirmeye çalıştığınızda oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* [Otomatik Yükseltme özelliği,](how-to-connect-install-automatic-upgrade.md) müşterileri aşağıdaki yapılandırmalarla desteklemek için genişletildi:
  * Aygıt geri yazma özelliğini etkinleştirdin.
  * Grup geri yazma özelliğini etkinleştirdin.
  * Yükleme Express ayarları veya DirSync yükseltmesi değildir.
  * Metaverse'de 100.000'den fazla nesne var.
  * Birden fazla ormana bağlanıyorsun. Ekspres kurulum yalnızca bir ormana bağlanır.
  * AD Bağlayıcıhesabı artık varsayılan MSOL_ hesabı değildir.
  * Sunucu evreleme modunda olacak şekilde ayarlanmıştır.
  * Kullanıcı geri yazma özelliğini etkinleştirdin.
  
  >[!NOTE]
  >Otomatik Yükseltme özelliğinin kapsam genişletmesi, Azure AD Connect build 1.1.105.0 ve sonrası müşterileri etkiler. Azure AD Connect sunucunuzun otomatik olarak yükseltilmesini istemiyorsanız, Azure AD Connect sunucunuzda aşağıdaki cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Otomatik Yükseltmeyi etkinleştirme/devre dışı bırakma hakkında daha fazla bilgi için [Azure AD Connect: Automatic upgrade](how-to-connect-install-automatic-upgrade.md)makalesine bakın.

## <a name="115580"></a>1.1.558.0
Durum: Yayınlanmayacak. Bu yapıdaki değişiklikler sürüm 1.1.561.0'a dahildir.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>[https://github.com/Azure/azure-powershell/issues/6814]() sorunu düzeltildi

* Ou tabanlı filtreleme yapılandırması güncelleştirildiğinde kaldırılacak "Out to-ad - User ImmutableId" kutusunun dışında eşitleme kuralının kaldırılmasına neden olan bir sorun düzeltildi. Bu eşitleme kuralı kaynak [bağlantı özelliği olarak ms-DS-ConsistencyGuid](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor)için gereklidir.

* Azure AD Connect sihirbazındaki [Etki Alanı ve OU Filtreleme ekranının,](how-to-connect-install-custom.md#domain-and-ou-filtering) OU tabanlı filtreleme etkin olsa bile tüm etki alanlarını ve *OU'ları* seçili olarak Eşitle seçeneğini gösterdiği bir sorun giderildi.

*   Eşitleme Hizmet Yöneticisi'ndeki [Yapılandırma Dizin Bölümleri ekranının](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) *Yenileme* düğmesi tıklatıldığında bir hatayı döndürmesine neden olan bir sorun düzeltildi. Hata iletisi *"Etki alanlarını yenilerken bir hatayla karşılaşıldı: 'System.Collections.ArrayList' türünden bir nesneyi 'Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject' yazmak için atamayan bir hatadır.* Hata, varolan bir AD ormanına yeni AD etki alanı eklendiğinde ve Azure AD Connect'i Yenile düğmesini kullanarak güncelleştirmeye çalıştığınızda oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* [Otomatik Yükseltme özelliği,](how-to-connect-install-automatic-upgrade.md) müşterileri aşağıdaki yapılandırmalarla desteklemek için genişletildi:
  * Aygıt geri yazma özelliğini etkinleştirdin.
  * Grup geri yazma özelliğini etkinleştirdin.
  * Yükleme Express ayarları veya DirSync yükseltmesi değildir.
  * Metaverse'de 100.000'den fazla nesne var.
  * Birden fazla ormana bağlanıyorsun. Ekspres kurulum yalnızca bir ormana bağlanır.
  * AD Bağlayıcıhesabı artık varsayılan MSOL_ hesabı değildir.
  * Sunucu evreleme modunda olacak şekilde ayarlanmıştır.
  * Kullanıcı geri yazma özelliğini etkinleştirdin.
  
  >[!NOTE]
  >Otomatik Yükseltme özelliğinin kapsam genişletmesi, Azure AD Connect build 1.1.105.0 ve sonrası müşterileri etkiler. Azure AD Connect sunucunuzun otomatik olarak yükseltilmesini istemiyorsanız, Azure AD Connect sunucunuzda aşağıdaki cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Otomatik Yükseltmeyi etkinleştirme/devre dışı bırakma hakkında daha fazla bilgi için [Azure AD Connect: Automatic upgrade](how-to-connect-install-automatic-upgrade.md)makalesine bakın.

## <a name="115570"></a>1.1.557.0
Durum: Temmuz 2017

>[!NOTE]
>Bu yapı, Azure AD Connect Otomatik Yükseltme özelliği aracılığıyla müşteriler tarafından kullanılamaz.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>[https://github.com/Azure/azure-powershell/issues/6814]() sorunu düzeltildi
* Varolan hizmet bağlantı noktası nesnesi üzerinde yapılandırılan doğrulanmış etki alanının hala geçerli bir etki alanı olsa bile değiştirilmesine neden olan Initialize-ADSyncDomainJoinedComputerSync cmdlet ile ilgili bir sorun giderildi. Bu sorun, Azure AD kiracınızda hizmet bağlantı noktasını yapılandırmak için kullanılabilecek birden fazla doğrulanmış etki alanı olduğunda oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Parola geri ödeme artık Microsoft Azure Devlet bulutu ve Microsoft Cloud Germany ile önizleme için kullanılabilir. Farklı hizmet örnekleri için Azure AD Connect desteği hakkında daha fazla bilgi için Azure AD Connect makalesine [bakın: Örneğin özel hususlar.](reference-connect-instances.md)

* Initialize-ADSyncDomainJoinedComputerSync cmdlet artık AzureADDomain adında yeni bir isteğe bağlı parametreye sahiptir. Bu parametre, hizmet bağlantı noktasını yapılandırmak için hangi doğrulanmış etki alanının kullanılacağını belirtmenize olanak tanır.

### <a name="pass-through-authentication"></a>Doğrudan Kimlik Doğrulama

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Geçiş Kimlik Doğrulaması için gerekli aracının adı *Microsoft Azure AD Application Proxy Proxy'sinden* *Microsoft Azure AD Connect Authentication Agent*olarak değiştirildi.

* Geçiş Kimlik Doğrulaması'nı etkinleştirmek artık varsayılan olarak Parola Hash Eşitlemesini etkinleştirmez.


## <a name="115530"></a>1.1.553.0
Durum: Haziran 2017

> [!IMPORTANT]
> Bu yapıda şema ve eşitleme kuralı değişiklikleri getirilmiştir. Azure AD Connect Eşitleme Hizmeti, yükseltmeden sonra Tam Alma ve Tam Eşitleme adımlarını tetikler. Değişikliklerin ayrıntıları aşağıda açıklanmıştır. Yükseltmeden sonra Tam Alma ve Tam Eşitleme adımlarını geçici olarak ertelemek için, [yükseltmeden sonra tam eşitlemenin nasıl ertelenebildiğini](how-to-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade)makaleye bakın.
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Bilinen sorun
* Azure AD Connect eşitleme ile [OU tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) kullanan müşterileri etkileyen bir sorun vardır. Azure AD Connect sihirbazındaki [Etki Alanı ve OU Filtreleme sayfasına](how-to-connect-install-custom.md#domain-and-ou-filtering) gidince aşağıdaki davranış lar beklenmektedir:
  * OU tabanlı filtreleme etkinleştirilmişse, **seçili etki alanlarını eşitleme ve OU** seçeneği seçilir.
  * Aksi takdirde, **tüm etki alanlarını ve OS'leri eşitle** seçeneği seçilir.

Ortaya çıkan sorun, Sihirbazı çalıştırdığınızda **tüm etki alanlarını ve DIĞER Olası rö'leri eşitleme seçeneğinin** her zaman seçilmesidir.  Bu, OU tabanlı filtreleme daha önce yapılandırılmış olsa bile oluşur. Herhangi bir AAD Connect yapılandırma değişikliğini kaydetmeden önce, **Eşitleme seçili etki alanları ve OS'ler seçeneğinin seçildiğinden** emin olun ve eşitleme yapması gereken tüm OS'lerin yeniden etkinleştirildiğinden emin olun. Aksi takdirde, OU tabanlı filtreleme devre dışı bırakılır.

#### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Azure AD Yöneticisi'nin şirket içi AD ayrıcalıklı bir kullanıcı hesabının parolasını sıfırlamasına olanak tanıyan Parola yazma yla ilgili bir sorun giderildi. Sorun, Azure AD Connect'e ayrıcalıklı hesap üzerinden Parolayı Sıfırlama izni verildiğinde oluşur. Bu sorun, azure AD Yöneticisi'nin, yönetici nin sahibi olmadığı sürece, şirket içinde bulunan bir AD ayrıcalıklı kullanıcı hesabının parolasını sıfırlamasına izin vermeyerak Azure AD Connect'in bu sürümünde giderilir. Daha fazla bilgi için [Güvenlik Danışma 4033453](https://technet.microsoft.com/library/security/4033453)bakın.

* Azure AD Connect'in şirket içi AD ms-DS-ConsistencyGuid özniteliğine geri yazılmadığını [Kaynak Bağlantı özelliği olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) ile ilgili bir sorun düzeltildi. Sorun, Azure AD Connect'e birden çok şirket içi AD ormanı eklendiğinde ve *birden çok dizin seçeneğinde Kullanıcı kimlikleri bulunduğunda* oluşur. Bu tür yapılandırma kullanıldığında, ortaya çıkan eşitleme kuralları Metaverse kaynakAnchorBinary özniteliği doldurmak yok. SourceAnchorBinary özniteliği ms-DS-ConsistencyGuid özniteliği için kaynak özniteliği olarak kullanılır. Sonuç olarak, ms-DSConsistencyGuid özniteliğine geri yazma oluşmaz. Sorunu gidermek için, Metaverse kaynakAnchorBinary özniteliği her zaman doldurulur olduğundan emin olmak için aşağıdaki eşitleme kuralları güncelleştirildi:
  * AD'den gelen - InetOrgPerson AccountEnabled.xml
  * AD'den gelen - InetOrgPerson Common.xml
  * AD'den - Kullanıcı AccountEnabled.xml
  * AD'den - Kullanıcı Common.xml
  * AD'den - Kullanıcı Join SOAInAAD.xml

* Daha önce, [Kaynak Bağlantı özelliği olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) etkin olmasa bile, Azure AD Connect'e "Out to AD – User ImmutableId" eşitleme kuralı eklendi. Etkisi iyi huyludur ve ms-DS-ConsistencyGuid özniteliğinin silinmesine neden olmaz. Karışıklığı önlemek için, eşitleme kuralının yalnızca özellik etkinleştirildiğinde eklendiğinden emin olmak için mantık eklendi.

* Hata olayı 611 ile başarısız parola karma eşitleme neden olan bir sorun düzeltildi. Bu sorun, bir veya daha fazla etki alanı denetleyicisi şirket içi AD kaldırıldıktan sonra oluşur. Her parola eşitleme döngüsünün sonunda, şirket içi AD tarafından verilen eşitleme çerezi, kaldırılan etki alanı denetleyicilerinin 0 USN (Sıra Numarasını Güncelleştirme) değerine sahip Çağırma ID'lerini içerir. Parola Eşitleme Yöneticisi, 0 USN değerini içeren eşitleme çerezini kalıcı olarak süreyemez ve hata olayı 611'de başarısız olur. Bir sonraki eşitleme döngüsü sırasında, Parola Eşitleme Yöneticisi, 0 USN değeri içermeyen kalıcı son eşitleme çerezini yeniden kullanır. Bu, aynı parola değişikliklerinin yeniden eşitlenmesine neden olur. Bu düzeltmeyle, Parola Eşitleme Yöneticisi eşitleme çerezini doğru şekilde devam ettir.

* Daha önce, Otomatik Yükseltme Set-ADSyncAutoUpgrade cmdlet kullanılarak devre dışı bırakılmış olsa bile, Otomatik Yükseltme işlemi düzenli olarak yükseltme için denetlemeye devam ediyor ve devre dışı bırakma onuruna indirilen yükleyici güveniyor. Bu düzeltmeyle, Otomatik Yükseltme işlemi artık yükseltmeyi düzenli aralıklarla denetlemeyecek. Düzeltme, bu Azure AD Connect sürümü için yükseltme yükleyicisi bir kez yürütüldüğünde otomatik olarak uygulanır.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha önce, kaynak bağlantı özelliği [olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) yalnızca yeni dağıtımlar için kullanılabilir. Şimdi, varolan dağıtımlar için kullanılabilir. Daha ayrıntılı belirtmek gerekirse:
  * Özelliğe erişmek için Azure AD Connect sihirbazını başlatın ve Kaynak Bağlantı Kaynağını *Güncelleştir* seçeneğini belirleyin.
  * Bu seçenek yalnızca sourceG özniteliği olarak objectGuid kullanan varolan dağıtımlar tarafından görülebilir.
  * Sihirbaz, seçeneği yapılandırırken, şirket içi Active Directory'nizde ms-DS-ConsistencyGuid özniteliğinin durumunu doğrular. Öznitelik dizindeki herhangi bir kullanıcı nesnesi üzerinde yapılandırılmamışsa, sihirbaz kaynakÇapa özniteliği olarak ms-DS-ConsistencyGuid kullanır. Öznitelik dizindeki bir veya daha fazla kullanıcı nesnesi üzerinde yapılandırılırsa, sihirbaz özniteliğin diğer uygulamalar tarafından kullanıldığı sonucuna varır ve kaynak Çapa özniteliği olarak uygun değildir ve Kaynak Bağlantı değişikliğinin devam ına izin vermez. Özniteliğin varolan uygulamalar tarafından kullanılmadığından eminseniz, hatayı nasıl bastırabileceğinizhakkında bilgi almak için Destek'e başvurmanız gerekir.

* Azure AD Connect, Aygıt nesnelerinde **kullanıcıSertifikası** özelliğine özel olarak, [artık etki alanı yla birleştirilmiş aygıtları Windows 10 deneyimi için Azure AD'ye bağlamak için](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) gereken sertifika değerlerini arar ve Azure AD ile eşitlemeden önce geri kalanını filtreler. Bu davranışı etkinleştirmek için, kutudan çıkan eşitleme kuralı "Out to AAD - Device Join SOAInAD" güncelleştirildi.

* Azure AD Connect artık Exchange Online **cloudPublicDelegates'in** şirket içi AD **publicDelegates** özniteliğine yeniden yazılmasını destekler. Bu, exchange online posta kutusunun sendonbehalfto hakları ile şirket içi Exchange posta kutusu olan kullanıcılara verilebileceği senaryoyu sağlar. Bu özelliği desteklemek için, yeni bir out-of-box eşitleme kuralı "Out to AD – User Exchange Hybrid PublicDelegates writeback" eklendi. Bu eşitleme kuralı yalnızca Exchange Karma özelliği etkinleştirildiğinde Azure AD Connect'e eklenir.

* Azure AD Connect artık Azure AD'den **altRecipient** özniteliğini eşitlemeyi destekler. Bu değişikliği desteklemek için, aşağıdaki kutu dan uzak eşitleme kuralları gerekli öznitelik akışını içerecek şekilde güncelleştirildi:
  * AD'den – Kullanıcı Değişimi
  * Out AAD için - Kullanıcı ExchangeOnline
  
* Metaverse'deki **cloudSOAExchMailbox** özelliği, belirli bir kullanıcının Exchange Online posta kutusuna sahip olup olmadığını gösterir. Tanımı, bu Tür Ekipman ve Konferans Odası posta kutuları gibi ek Exchange Online RecipientDisplayTypes içerecek şekilde güncelleştirildi. Bu değişikliği etkinleştirmek için, "AAD'den Giriş – Kullanıcı Değişimi Hibriti" olarak kutudan çıkma eşitleme kuralı altında bulunan cloudSOAExchMailbox özniteliğinin tanımı aşağıdakiadreslerden güncelleştirildi:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... aşağıdakilere:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* KullanıcıSertifikası özyündeki sertifika değerlerini işlemek için eşitleme kuralı ifadeleri oluşturmak için aşağıdaki X509Certificate2 uyumlu işlevler kümesi eklendi:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|CertThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Şunu seçin:|
    |CertKeyAlgorithmParams|CertHashString|Konum|
    |||With|

* Aşağıdaki şema değişiklikleri, müşterilerin grup nesneleri için sAMAccountName, domainNetBios ve domainFQDN'nin yanı sıra Kullanıcı nesneleri için ayırt edici Ad akışını sağlamak için özel senkronizasyon kuralları oluşturmalarına olanak sağlamak için sunulmuştur:

  * MV şemasına aşağıdaki öznitelikler eklenmiştir:
    * Grup: Hesap Adı
    * Grup: domainNetBios
    * Grup: domainFQDN
    * Kişi: seçkinName

  * Azure AD Bağlayıcı şemasına aşağıdaki öznitelikler eklendi:
    * Grup: OnpremisesSamAccountName
    * Grup: NetBiosName
    * Grup: DnsDomainName
    * Kullanıcı: OnPremisesDistinguishedName

* ADSyncDomainJoinedComputerSync cmdlet komut dosyası artık AzureEnvironment adında yeni bir isteğe bağlı parametreye sahiptir. Parametre, ilgili Azure Etkin Dizin idenin hangi bölgede barındırılan olduğunu belirtmek için kullanılır. Geçerli değerler şunlardır:
  * AzureCloud (varsayılan)
  * AzureChinaCloud
  * AzureGermanyCloud
  * ABD Hükümeti
 
* Eşitleme kuralı oluşturma sırasında bağlantı türünün varsayılan değeri olarak Join (Provision yerine) kullanmak için Eşitleme Kuralı Düzenleyicisi güncelleştirildi.

### <a name="ad-fs-management"></a>AD FS yönetimi

#### <a name="issues-fixed"></a>Sorunlar düzeltildi

* Aşağıdaki URL'ler, Azure AD tarafından kimlik doğrulama kesintisine karşı dayanıklılığı artırmak için sunulan yeni WS-Federation uç noktalarıdır ve şirket içi AD FS yanıtlayan parti güven yapılandırmasına eklenecektir:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://ccs.login.microsoftonline.com/login.srf
  * https://ccs-sdf.login.microsoftonline.com/login.srf
  
* AD FS'nin Veren Kimliği için yanlış talep değeri oluşturmasına neden olan bir sorun düzeltildi. Sorun, Azure AD kiracısında birden çok doğrulanmış etki alanı varsa ve IssuerID iddiasını oluşturmak için kullanılan userPrincipalName özniteliğinin etki alanı johndoe@us.contoso.comsoneki en az 3 düzey derinse (örneğin,) oluşur. Sorun, talep kuralları tarafından kullanılan regex güncelleştirilerek çözülür.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Önceden, Azure AD Connect tarafından sağlanan ADFS Sertifika Yönetimi özelliği yalnızca Azure AD Connect aracılığıyla yönetilen ADFS çiftliklerinde kullanılabilir. Artık, Azure AD Connect kullanılarak yönetilmeyen ADFS çiftliklerinde bu özelliği kullanabilirsiniz.

## <a name="115240"></a>1.1.524.0
Yayınlanma Tarihi: Mayıs 2017

> [!IMPORTANT]
> Bu yapıda şema ve eşitleme kuralı değişiklikleri getirilmiştir. Azure AD Connect Eşitleme Hizmeti, yükseltmeden sonra Tam Alma ve Tam Eşitleme adımlarını tetikler. Değişikliklerin ayrıntıları aşağıda açıklanmıştır.
>
>

**Düzeltilen sorunlar:**

Azure AD Connect Eşitleme

* Müşteri, Set-ADSyncAutoUpgrade cmdlet'i kullanarak özelliği devre dışı bıraksa bile Azure AD Connect sunucusunda Otomatik Yükseltme'nin oluşmasına neden olan bir sorun giderildi. Bu düzeltmeyle, sunucudaki Otomatik Yükseltme işlemi hala düzenli olarak yükseltmeyi denetler, ancak indirilen yükleyici Otomatik Yükseltme yapılandırmasını onurlandırıyor.
* Azure AD Connect, DirSync yerinde yükseltme sırasında Azure AD bağlayıcısı tarafından Azure AD ile senkronize olmak için kullanılmak üzere bir Azure AD hizmet hesabı oluşturur. Hesap oluşturulduktan sonra Azure AD Connect, hesabı kullanarak Azure AD ile kimlik doğrulaması kullanır. Bazen, kimlik doğrulama, DirSync yerinde yükseltmehata ile başarısız lığa neden olan geçici sorunlar nedeniyle başarısız olur *"Bir hata Yapılandırma AAD Eşitleme görev yürütme oluştu: AADSTS50034: Bu uygulamada oturum açmak için, hesap xxx.onmicrosoft.com dizinine eklenmelidir."* Azure AD Connect, DirSync yükseltmesinin esnekliğini artırmak için artık kimlik doğrulama adımını yeniden dener.
* Yapı 443 ile ilgili bir sorun vardı, DirSync yerinde yükseltmenin başarılı olmasına neden oldu, ancak dizin eşitlemesi için gereken profilleri çalıştırma oluşturuldu. İyileştirme mantığı, Azure AD Connect'in bu yapısına dahildir. Müşteri bu yapıya yükseltme yaptığında, Azure AD Connect eksik çalışan profilleri algılar ve bunları oluşturur.
* Parola Eşitleme işleminin Event ID 6900 ile başlamamasına neden olan bir sorun ve *"Aynı anahtara sahip bir öğe zaten eklenmiştir" hatası düzeltildi.* BU sorun, AD yapılandırması bölümü içerecek şekilde OU filtreleme yapılandırmasını güncellerseniz oluşur. Bu sorunu gidermek için Parola Eşitleme işlemi artık yalnızca AD etki alanı bölümlerinden parola değişikliklerini eşitler. Yapılandırma bölümü gibi etki alanı olmayan bölümler atlanır.
* Azure AD Connect, Express yüklemesi sırasında, şirket içi AD ile iletişim kurmak için AD bağlayıcısı tarafından kullanılmak üzere şirket içi bir AD DS hesabı oluşturur. Daha önce, hesap, kullanıcı-Hesap Denetimi özniteliği üzerinde ayarlanan PASSWD_NOTREQD bayrağıyla oluşturulur ve hesapta rasgele bir parola ayarlanır. Azure AD Connect, parola hesapta ayarlandıktan sonra PASSWD_NOTREQD bayrağını açıkça kaldırır.
* MailNickname özniteliği şirket içi AD şemasında bulunduğunda DirSync yükseltmesinin hata yla başarısız olmasına neden olan bir sorun *düzeltildi,* ancak AD Kullanıcı nesne sınıfına bağlı değil.
* Bir yönetici Azure AD Connect sihirbazı kullanarak Azure AD Connect eşitleme yapılandırmasını güncellerken Aygıt geri yazma özelliğinin otomatik olarak devre dışı edilmesine neden olan bir sorun düzeltildi. Bu sorun, sihirbazın şirket içi AD'de varolan Aygıt geri yazma yapılandırması için önceden gerekli bir denetim gerçekleştirmesi nedeniyle kaynaklanır ve denetim başarısız olur. Düzeltme, Aygıt yazma zaten daha önce etkin olup olmadığını denetimi atlamaktır.
* OU filtrelemesini yapılandırmak için Azure AD Connect sihirbazını veya Eşitleme Hizmet Yöneticisi'ni kullanabilirsiniz. Daha önce, OU filtreleme yapılandırmak için Azure AD Connect sihirbazını kullanırsanız, daha sonra oluşturulan yeni OU'lar dizin eşitlemesi için dahil edilir. Yeni OU'ların eklenmesini istemiyorsanız, Eşitleme Hizmet Yöneticisi'ni kullanarak OU filtrelemesini yapılandırmanız gerekir. Artık, Azure AD Connect sihirbazını kullanarak aynı davranışı elde edebilirsiniz.
* Azure AD Connect tarafından gerekli depolanan yordamların dbo şeması altında değil, yükleme yöneticisinin şeması altında oluşturulmasına neden olan bir sorun düzeltildi.
* Azure AD tarafından döndürülen TrackingId özniteliğinin AAD Connect Server Olay Günlükleri'nde atlanmasına neden olan bir sorun düzeltildi. Bu sorun, Azure AD Connect'ten yeniden yönlendirme iletisi alırsa ve Azure AD Connect sağlanan bitiş noktasına bağlanamazsa sorun oluşur. TrackingId, Destek Mühendisleri tarafından sorun giderme sırasında servis yan günlükleriyle ilişkilendirmek için kullanılır.
* Azure AD Connect Azure AD'den Büyük Nesne hatası aldığında, Azure AD Connect EventID 6941 ile bir olay oluşturur ve *"Sağlanan nesne çok büyüktür. Bu nesnedeki öznitelik değerlerinin sayısını kırpın."* Aynı zamanda Azure AD Connect, EventID 6900 ile yanıltıcı bir olay oluşturur ve *"Microsoft.Online.Coexistence.ProvisionRetryException: Windows Azure Active Directory hizmetiyle iletişim kuramaz."* iletisi de oluşturur. Karışıklığı en aza indirmek için Azure AD Connect, Büyük Nesne hatası geldiğinde ikinci olayı artık oluşturmaz.
* Genel LDAP bağlayıcısı yapılandırmasını güncelleştirmeye çalışırken Eşitleme Hizmet Yöneticisi'nin yanıt vermemesine neden olan bir sorun giderildi.

**Yeni özellikler/geliştirmeler:**

Azure AD Connect Eşitleme
* Eşitleme Kuralı Değişiklikleri – Aşağıdaki eşitleme kuralı değişiklikleri uygulanmıştır:
  * Özniteliklerin 15'ten fazla değeri **varsa, kullanıcıSertifikası** ve **userSMIMECertificate** özniteliklerini dışa aktarmamak için güncelleştirilmiş varsayılan eşitleme kuralı.
  * AD öznitelikleri **employeeID** ve **msExchBypassModerationLink** şimdi varsayılan eşitleme kuralı kümesine dahildir.
  * AD öznitelik **fotoğrafı** varsayılan eşitleme kuralı kümesinden kaldırıldı.
  * Metaverse şemasına ve AAD Konektör şemasına **tercihli DataLocation** eklendi. Azure AD'deki öznitelikleri güncelleştirmek isteyen müşteriler, bunu yapmak için özel eşitleme kuralları uygulayabilir. 
  * Metaverse şemasına ve AAD Konektör şemasına **userType** eklendi. Azure AD'deki öznitelikleri güncelleştirmek isteyen müşteriler, bunu yapmak için özel eşitleme kuralları uygulayabilir.

* Azure AD Connect artık şirket içi AD nesneleri için Kaynak Çapa özniteliği olarak TutarlılıkGuid özniteliğinin kullanımını otomatik olarak sağlar. Ayrıca, Azure AD Connect boşsa ObjectGuid özniteliği değeriyle Tutarlıguid özniteliğini doldurur. Bu özellik yalnızca yeni dağıtım için geçerlidir. Bu özellik hakkında daha fazla bilgi edinmek için Azure [AD Connect: Design concepts - sourceAnchor olarak ms-DS-ConsistencyGuid](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor)kullanma makalesibölümüne bakın.
* Parola Hash Senkronizasyonu ile ilgili sorunları tanılamaya yardımcı olmak için cmdlet Invoke-ADSyncDiagnostics ile ilgili yeni sorun giderme eklendi. Cmdlet'i kullanma hakkında bilgi için Azure AD Connect eşitleme ile sorun [giderme parola karma senkronizasyonumakalesine](tshoot-connect-password-hash-synchronization.md)bakın.
* Azure AD Connect artık şirket içi AD'den Azure AD'ye Posta Özellikli Ortak Klasör nesnelerini eşitlemeyi destekler. İsteğe Bağlı Özellikler altında Azure AD Connect sihirbazını kullanarak özelliği etkinleştirebilirsiniz. Bu özellik hakkında daha fazla bilgi edinmek [için, şirket içi Posta Etkin Ortak Klasörler için Office 365 Dizin Tabanlı Kenar Engelleme desteğine](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders)bakın.
* Azure AD Connect, şirket içi AD'den eşitlemek için bir AD DS hesabı gerektirir. Daha önce, Express modunu kullanarak Azure AD Connect'i yüklediyseniz, bir Kurumsal Yönetici hesabının kimlik bilgilerini sağlayabilirsiniz ve Azure AD Connect gerekli AD DS hesabını oluşturur. Ancak, özel bir yükleme ve varolan bir dağıtıma orman eklemek için, bunun yerine AD DS hesabını sağlamanız gerekiyordu. Artık, özel bir yükleme sırasında kurumsal yönetici hesabının kimlik bilgilerini sağlama ve Azure AD Connect'in gerekli AD DS hesabını oluşturmasına izin verme seçeneğiniz de vardır.
* Azure AD Connect artık SQL AOA'yı destekliyor. Azure AD Connect'i yüklemeden önce SQL AOA'yı etkinleştirmelisiniz. Yükleme sırasında Azure AD Connect, sağlanan SQL örneğinin SQL AOA için etkin olup olmadığını algılar. SQL AOA etkinse, Azure AD Connect, SQL AOA'nın senkron çoğaltma veya eşzamanlı çoğaltma kullanacak şekilde yapılandırıp yapılandırılmadığını daha iyi anlayabilir. Kullanılabilirlik Grubu Dinleyicisi'ni ayarlarken RegisterAllProvidersIP özelliğini 0 olarak ayarlamanız önerilir. Bu öneri, Azure AD Connect'in SQL'e bağlanmak için şu anda SQL Native Client'ı kullanması ve SQL Native Client'ın MultiSubNetFailover özelliğinin kullanımını desteklememesidir.
* Azure AD Connect sunucunuz için veritabanı olarak LocalDB kullanıyorsanız ve 10 GB boyut sınırına ulaştıysanız, Eşitleme Hizmeti artık başlamazsa. Daha önce, Eşitleme Hizmeti'nin başlaması için yeterli DB alanını geri almak için LocalDB'de ShrinkDatabase işlemini gerçekleştirmeniz gerekir. Bundan sonra, daha fazla DB alanı geri almak için çalışma geçmişini silmek için Eşitleme Hizmet Yöneticisi'ni kullanabilirsiniz. Şimdi, DB alanını geri almak için YerelDB'den çalışma geçmişi verilerini temizlemek için Start-ADSyncPurgeRunHistory cmdlet'i kullanabilirsiniz. Ayrıca, bu cmdlet, Eşitleme Hizmeti çalışmadığında kullanılabilecek çevrimdışı modu (-çevrimdışı parametre yi belirterek) destekler. Not: Çevrimdışı mod yalnızca Eşitleme Hizmeti çalışmıyorsa ve kullanılan veritabanı LocalDB ise kullanılabilir.
* Azure AD Connect, gereken depolama alanı miktarını azaltmak için, bunları LocalDB/SQL veritabanlarında depolamadan önce eşitleme hata ayrıntılarını sıkıştırır. Azure AD Connect'in eski bir sürümünden bu sürüme yükseltme yaparken, Azure AD Connect varolan eşitleme hata ayrıntılarıüzerinde bir kerelik sıkıştırma gerçekleştirir.
* Daha önce, OU filtreleme yapılandırmasını güncelleştirdikten sonra, varolan nesnelerin dizin eşitlemesi dışında düzgün bir şekilde dahil edildiğinden/dışlandığından emin olmak için Tam içeri aktarmayı el ile çalıştırmanız gerekir. Azure AD Connect, bir sonraki eşitleme döngüsü nde tam alma işlemi otomatik olarak tetikler. Ayrıca, Tam alma yalnızca güncelleştirmeden etkilenen AD bağlayıcılarına uygulanır. Not: Bu iyileştirme, yalnızca Azure AD Connect sihirbazı kullanılarak yapılan OU filtreleme güncelleştirmeleri için geçerlidir. Eşitleme Hizmet Yöneticisi kullanılarak yapılan OU filtreleme güncelleştirmesi için geçerli değildir.
* Daha önce, Grup tabanlı filtreleme yalnızca Kullanıcıları, Grupları ve İlgili Kişi nesnelerini destekler. Şimdi, Grup tabanlı filtreleme de Bilgisayar nesnelerini destekler.
* Daha önce, Azure AD Connect eşitleme zamanlayıcısıdevre dışı bırakmadan Bağlayıcı Alanı verilerini silebilirsiniz. Şimdi, Eşitleme Hizmet Yöneticisi, zamanlayıcının etkin olduğunu algılarsa Bağlayıcı Uzay verilerinin silinmesini engeller. Ayrıca, Bağlayıcı alan verileri silinirse, müşterileri olası veri kaybı hakkında bilgilendirmek için bir uyarı döndürülür.
* Daha önce, düzgün çalışması için Azure AD Connect sihirbazı için PowerShell transkripsiyonuna devre dışı bırakmalısınız. Bu sorun kısmen çözüldü. Eşitleme yapılandırmasını yönetmek için Azure AD Connect sihirbazı kullanıyorsanız PowerShell transkripsiyonünü etkinleştirebilirsiniz. ADFS yapılandırmasını yönetmek için Azure AD Connect sihirbazı kullanıyorsanız PowerShell transkripsiyonünü devre dışı bmelisiniz.



## <a name="114860"></a>1.1.486.0
Yayınlanma Tarihi: Nisan 2017

**Düzeltilen sorunlar:**
* Azure AD Connect'in Windows Server'ın yerelleştirilmiş sürümüne başarıyla yüklenmeyeceğiniz sorun giderildi.

## <a name="114840"></a>1.1.484.0
Yayınlanma Tarihi: Nisan 2017

**Bilinen sorunlar:**

* Aşağıdaki koşulların tümü doğruysa Azure AD Connect'in bu sürümü başarıyla yüklenmez:
   1. DirSync yerinde yükseltme veya Azure AD Connect'in yeni yüklemesini gerçekleştiresiniz.
   2. Windows Server'ın sunucudaki yerleşik Yönetici grubunun adının "Yöneticiler" olmadığı yerelleştirilmiş bir sürümünü kullanıyorsunuz.
   3. Kendi tam SQL'inizi sağlamak yerine Azure AD Connect yüklü varsayılan SQL Server 2012 Express LocalDB'yi kullanıyorsunuz.

**Düzeltilen sorunlar:**

Azure AD Connect Eşitleme
* Eşitleme zamanlayıcısının, söz konusu eşitleme adımı için bir veya daha fazla bağlayıcı nın çalışma profili eksikse eşitleme adımının tamamını atladığı bir sorun düzeltildi. Örneğin, bir Delta İçe Aktarma çalışması profili oluşturmadan Eşitleme Hizmet Yöneticisi'ni kullanarak el ile bir bağlayıcı eklediniz. Bu düzeltme, eşitleme zamanlayıcısının diğer bağlayıcılar için Delta İçe Akt'ı çalıştırmaya devam etmesini sağlar.
* Eşitleme Hizmeti'nin çalışma adımlarından biriyle ilgili bir sorunla karşılaştığında çalışan profili işlemeyi hemen durdurduğu bir sorun düzeltildi. Bu düzeltme, Eşitleme Hizmeti'nin bu adımı atlamasını ve geri kalanını işlemeye devam etmesini sağlar. Örneğin, birden çok çalıştırma adımı olan AD bağlayıcınız için bir Delta Alma çalıştırma profiliniz vardır (her şirket içi AD etki alanı için bir tane). Eşitleme Hizmeti, bunlardan biri ağ bağlantısı sorunları olsa bile, delta alma diğer AD etki alanları ile çalıştırın.
* Otomatik Yükseltme sırasında Azure AD Bağlayıcısı güncelleştirmesinin atlanmasına neden olan bir sorun giderildi.
* Azure AD Connect'in sunucunun kurulum sırasında etki alanı denetleyicisi olup olmadığını yanlış belirlemesine neden olan ve bunun sonucunda DirSync yükseltmesinin başarısız olması yla ilgili bir sorun düzeltildi.
* Azure AD Bağlayıcısı için herhangi bir çalışma profili oluşturmamak için DirSync yerinde yükseltmesine neden olan bir sorun düzeltildi.
* Genel LDAP Bağlayıcısı'nı yapılandırmaya çalışırken Eşitleme Hizmet Yöneticisi kullanıcı arabiriminin yanıt vermiyor olduğu bir sorun giderildi.

AD FS yönetimi
* AD FS birincil düğümü başka bir sunucuya taşınmışsa Azure AD Connect sihirbazı başarısız oldu.

Masaüstü SSO
* Yeni yükleme sırasında Oturum Açma seçeneğiniz olarak Parola Senkronizasyonu'nu seçtiyseniz, Oturum Açma ekranının Masaüstü SSO özelliğini etkinleştirmenize izin vermediği Azure AD Connect sihirbazındaki bir sorun giderildi.

**Yeni özellikler/geliştirmeler:**

Azure AD Connect Eşitleme
* Azure AD Connect Sync artık Sanal Hizmet Hesabı, Yönetilen Hizmet Hesabı ve Grup Yönetilen Hizmet Hesabı'nın hizmet hesabı olarak kullanımını destekler. Bu, yalnızca Azure AD Connect'in yeni yüklemesi için geçerlidir. Azure AD Connect'i yüklerken:
    * Varsayılan olarak, Azure AD Connect sihirbazı bir Sanal Hizmet Hesabı oluşturur ve onu hizmet hesabı olarak kullanır.
    * Bir etki alanı denetleyicisine yüklüyorsanız, Azure AD Connect, bir etki alanı kullanıcı hesabı oluşturacağı ve bunun yerine hizmet hesabı olarak kullandığı önceki davranışa geri döner.
    * Aşağıdakilerden birini sağlayarak varsayılan davranışı geçersiz kılabilirsiniz:
      * Grup Yönetilen Hizmet Hesabı
      * Yönetilen Hizmet Hesabı
      * Etki alanı kullanıcı hesabı
      * Yerel bir kullanıcı hesabı
* Daha önce, bağlayıcılar güncelleştirme veya eşitleme kuralı değişiklikleri içeren yeni bir Azure AD Connect yapısına yükseltme yaptığınızda, Azure AD Connect tam eşitleme döngüsünü tetikler. Azure AD Connect, yalnızca güncelleştirmeli bağlayıcılar için Tam Alma adımLarını ve yalnızca eşitleme kuralı değişikliklerine sahip bağlayıcılar için Tam Eşitleme adımLarını seçici olarak tetikler.
* Daha önce, Dışa Aktarma Silme Eşiği yalnızca eşitleme zamanlayıcısı aracılığıyla tetiklenen dışa aktarımlar için geçerlidir. Şimdi, özellik Eşitleme Hizmet Yöneticisi kullanarak müşteri tarafından el ile tetiklenen dışa aktarma yı içerecek şekilde genişletilir.
* Azure AD kiracınızda, kiracınız için Parola Eşitleme özelliğinin etkin olup olmadığını gösteren bir hizmet yapılandırması vardır. Önceden, etkin ve bir hazırlama sunucunuz olduğunda hizmet yapılandırmasının Azure AD Connect tarafından yanlış yapılandırılması kolaydır. Azure AD Connect artık hizmet yapılandırmasını yalnızca etkin Azure AD Connect sunucunuzla tutarlı tutmaya çalışır.
* Azure AD Connect sihirbazı, şirket içi AD Geri Dönüşüm Kutusu etkin değilse, artık bir uyarı algılar ve döndürür.
* Daha önce, toplu iş teki nesnelerin birleşik boyutu belirli eşiği aşarsa Azure AD'ye dışa aktarma süresi ve başarısız olur. Şimdi, Eşitleme Hizmeti, sorunla karşılaşılırsa nesneleri ayrı, daha küçük gruplar halinde yeniden göndermeyi yeniden dener.
* Eşitleme Hizmeti Anahtar Yönetimi uygulaması Windows Başlat Menüsünden kaldırıldı. Şifreleme anahtarının yönetimi miiskmu.exe kullanılarak komut satırı arabirimi üzerinden desteklenmeye devam edecektir. Şifreleme anahtarını yönetme hakkında bilgi için Azure [AD Bağlantı Eşitleme şifreleme anahtarını terk etme makalesine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-adsync-service-account-encryption-key)bakın.
* Daha önce, Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirirseniz, şifreleme anahtarını bırakıp Azure AD Connect eşitleme hizmeti hesabı parolasını yeniden başlatana kadar Eşitleme Hizmeti doğru şekilde başlatılamaz. Şimdi, bu süreç artık gerekli değildir.

Masaüstü SSO

* Azure AD Connect sihirbazı artık Geçiş Kimlik Doğrulama ve Masaüstü SSO'yu yapılandırırken 9090 numaralı bağlantı noktasının ağda açılmasını gerektirmez. Sadece port 443 gereklidir. 

## <a name="114430"></a>1.1.443.0
Yayınlanma: Mart 2017

**Düzeltilen sorunlar:**

Azure AD Connect Eşitleme
* Azure AD Bağlayıcısı'nın görüntü adı Azure AD Bağlayıcısı'na atanan ilk onmicrosoft.com etki alanını içermiyorsa, Azure AD Connect sihirbazının başarısız olması yla ilgili bir sorun düzeltildi.
* Eşitleme Hizmeti Hesabı'nın parolası apostrophe, üst üste ve boşluk gibi özel karakterler içerdiğinde, Sql veritabanına bağlantı kurarken Azure AD Connect sihirbazının başarısız olmasına neden olan bir sorun giderildi.
* Bir şirket içi AD nesnesini geçici olarak eşitlemeden çıkardıktan ve sonra eşitleme için yeniden ekledikten sonra, "Görüntünün görüntüden farklı bir çapaya sahip olması" hatasının, bir Azure AD Connect sunucusunda oluşmasına neden olan bir sorun düzeltildi.
* Şirket içi bir AD nesnesini geçici olarak eşitlemeden çıkardıktan ve sonra eşitleme için yeniden ekledikten sonra, "DN tarafından bulunan nesne bir hayalettir" hatasının, bir Azure AD Connect sunucusunda oluşmasına neden olan bir sorun düzeltildi.

AD FS yönetimi
* Azure AD Connect sihirbazı AD FS yapılandırmasını güncelleştirmez ve Alternatif Giriş Kimliği yapılandırıldıktan sonra güvenilen taraf güvenine doğru talepleri ayarladığı bir sorun düzeltildi.
* Azure AD Connect sihirbazının hizmet hesapları sAMAccountName biçimi yerine userPrincipalName biçimi kullanılarak yapılandırılan AD FS sunucularını doğru şekilde işleyemediği bir sorun giderildi.

Doğrudan Kimlik Doğrulama
* Pass Through Authentication seçilirse ancak bağlayıcısının kaydı başarısız olursa Azure AD Connect sihirbazının başarısız olmasına neden olan bir sorun giderildi.
* Desktop SSO özelliği etkinleştirildiğinde seçilen oturum açma yönteminde Doğrulama denetimlerini atlamasına neden olan bir sorun düzeltildi.

Parola Sıfırlama
* Bağlantı bir güvenlik duvarı veya proxy tarafından öldürüldüyse, Azure AAD Connect sunucusunun yeniden bağlanmaya çalışmamasına neden olabilecek bir sorun giderildi.

**Yeni özellikler/geliştirmeler:**

Azure AD Connect Eşitleme
* Get-ADSyncScheduler cmdlet şimdi SyncCycleInProgress adlı yeni bir Boolean özelliği döndürür. Döndürülen değer doğruysa, zamanlanmış bir eşitleme döngüsü devam ediyor demektir.
* Azure AD Connect yüklemesini ve kurulum günlüklerini depolamak için hedef klasörü, günlük dosyalarına erişilebilirliği artırmak için %localappdata%\AADConnect'ten %programdata%\AADConnect'e taşındı.

AD FS yönetimi
* AD FS Farm TLS/SSL Sertifikasını güncellemek için destek eklendi.
* AD FS 2016'nın yönetimi için destek eklendi.
* Artık AD FS yüklemesi sırasında varolan gMSA (Grup Yönetilen Hizmet Hesabı) belirtebilirsiniz.
* Artık SHA-256'yı Azure AD güvenerek parti güveni için imza karma algoritması olarak yapılandırabilirsiniz.

Parola Sıfırlama
* Ürünün daha sıkı güvenlik duvarı kurallarına sahip ortamlarda çalışmasına izin vermek için geliştirmeler tanıtıldı.
* Azure Hizmet Veri Servisi için geliştirilmiş bağlantı güvenilirliği.

## <a name="113800"></a>1.1.380.0
Yayınlanma Tarihi: Aralık 2016

**Sorun giderildi:**

* Active Directory Federation Services (AD FS) için sorunlu talep kuralının bu yapıda eksik olduğu sorun giderildi.

>[!NOTE]
>Bu yapı, Azure AD Connect Otomatik Yükseltme özelliği aracılığıyla müşteriler tarafından kullanılamaz.

## <a name="113710"></a>1.1.371.0
Yayınlanma Tarihi: Aralık 2016

**Bilinen sorun:**

* AD FS için sorunlu talep kuralı bu yapıda eksiktir. Azure Etkin Dizini (Azure AD) ile birden çok etki alanını fecürünle aktarıyorsanız, sorunlu talep kuralı gereklidir. Şirket içi AD FS dağıtımınızı yönetmek için Azure AD Connect kullanıyorsanız, bu yapıya yükseltme, AD FS yapılandırmanızdan varolan sorunlu talep kuralını kaldırır. Yükleme/yükseltmeden sonra sorunlu talep kuralını ekleyerek sorunu çözebilirsiniz. İhraç edilen talep kuralını ekleme yle ilgili ayrıntılar için, [Azure AD ile federating için Birden Çok etki alanı desteği](how-to-connect-install-multiple-domains.md)hakkındaki bu makaleye bakın.

**Sorun giderildi:**

* Bağlantı Noktası 9090 giden bağlantı için açılmazsa, Azure AD Connect yüklemesi veya yükseltmesi başarısız olur.

>[!NOTE]
>Bu yapı, Azure AD Connect Otomatik Yükseltme özelliği aracılığıyla müşteriler tarafından kullanılamaz.

## <a name="113700"></a>1.1.370.0
Yayınlanma Tarihi: Aralık 2016

**Bilinen sorunlar:**

* AD FS için sorunlu talep kuralı bu yapıda eksiktir. Azure AD ile birden çok etki alanını fecürünle aktarıyorsanız, sorunlu talep kuralı gereklidir. Şirket içi AD FS dağıtımınızı yönetmek için Azure AD Connect kullanıyorsanız, bu yapıya yükseltme, AD FS yapılandırmanızdan varolan sorunlu talep kuralını kaldırır. Yükleme/yükseltmeden sonra sorunlu talep kuralını ekleyerek sorunu çözebilirsiniz. İhraç talebi kuralı ekleme yle ilgili ayrıntılar için, [Azure AD ile federating için Birden çok etki alanı desteği](how-to-connect-install-multiple-domains.md)hakkındaki bu makaleye bakın.
* Bağlantı noktası 9090 kurulumu tamamlamak için giden açık olmalıdır.

**Yeni özellikler:**

* Geçiş Kimlik Doğrulama (Önizleme).

>[!NOTE]
>Bu yapı, Azure AD Connect Otomatik Yükseltme özelliği aracılığıyla müşteriler tarafından kullanılamaz.

## <a name="113430"></a>1.1.343.0
Yayınlanma Tarihi: Kasım 2016

**Bilinen sorun:**

* AD FS için sorunlu talep kuralı bu yapıda eksiktir. Azure AD ile birden çok etki alanını fecürünle aktarıyorsanız, sorunlu talep kuralı gereklidir. Şirket içi AD FS dağıtımınızı yönetmek için Azure AD Connect kullanıyorsanız, bu yapıya yükseltme, AD FS yapılandırmanızdan varolan sorunlu talep kuralını kaldırır. Yükleme/yükseltmeden sonra sorunlu talep kuralını ekleyerek sorunu çözebilirsiniz. İhraç talebi kuralı ekleme yle ilgili ayrıntılar için, [Azure AD ile federating için Birden çok etki alanı desteği](how-to-connect-install-multiple-domains.md)hakkındaki bu makaleye bakın.

**Düzeltilen sorunlar:**

* Bazen, Azure AD Connect'i yüklemek, parolası kuruluşun parola ilkesinde belirtilen karmaşıklık düzeyini karşılayan yerel bir hizmet hesabı oluşturamadığından başarısız olur.
* Bağlayıcı alanındaki bir nesne aynı anda bir birleştirme kuralı için kapsam dışına çıktığında ve başka bir birleştirme kuralı için kapsam dışı hale geldiğinde birleştirme kurallarının yeniden değerlendirilmediği bir sorun düzeltildi. Bu, birleştirme koşulları birbirini dışlayan iki veya daha fazla birleştirme kuralınız varsa gerçekleşebilir.
* Birleştirme kuralları içermeyen gelen eşitleme kurallarının (Azure AD'den) birleştirme kuralları içerenlerden daha düşük öncelik değerlerine sahipse işlenmediği bir sorun düzeltildi.

**Geliştirmeler:**

* Windows Server 2016 Standardına veya daha yüksek bir düzeye Azure AD Connect yükleme desteği eklendi.
* SQL Server 2016'yı Azure AD Connect için uzak veritabanı olarak kullanmak için destek eklendi.

## <a name="112810"></a>1.1.281.0
Yayınlanma Tarihi: Ağustos 2016

**Düzeltilen sorunlar:**

* Eşitleme aralığındaki değişiklikler, bir sonraki eşitleme döngüsü tamamlanana kadar gerçekleşmez.
* Azure AD Connect sihirbazı, kullanıcı adı alt puanla başlayan\_bir Azure REKLAM hesabıkabul etmez .
* Azure AD Connect sihirbazı, hesap parolası çok fazla özel karakter içeriyorsa Azure AD hesabının kimliğini doğrulamaz. Hata iletisi "Kimlik bilgilerini doğrulayamıyor. Beklenmeyen bir hata oluştu." döndürülür.
* Evreleme sunucusunun kaldırılması Azure AD kiracısında parola eşitlemesi devre dışı kalır ve parola eşitlemesi etkin sunucuyla başarısız olur.
* Parola eşitleme, kullanıcıda depolanan parola karma sıyrığı olmadığında sık karşılaşılan durumlarda başarısız olur.
* Azure AD Connect sunucusu hazırlama modu için etkinleştirildiğinde, parola yazma geçici olarak devre dışı bırakılmaz.
* Azure AD Connect sihirbazı, sunucu evreleme modundayken gerçek parola eşitleme ve parola yazma yapılandırmasını göstermez. Her zaman onları engelli olarak gösterir.
* Parola eşitleme ve parola yazma yapılandırma değişiklikleri, sunucu evreleme modundayken Azure AD Connect sihirbazı tarafından kalıcı hale getirilmiş değildir.

**Geliştirmeler:**

* Yeni bir eşitleme döngüsünü başarıyla başlatıp başlatamayacağını belirtmek için Start-ADSyncSyncCycle cmdlet'i güncelleştirin.
* Şu anda devam etmekte olan eşitleme döngüsünü ve işlemini sonlandırmak için Stop-ADSyncSyncCycle cmdlet eklendi.
* Şu anda devam etmekte olan eşitleme döngüsünü ve işlemini sonlandırmak için Stop-ADSyncScheduler cmdlet'i güncelleştirildi.
* Azure AD Connect sihirbazında [Dizin uzantılarını](how-to-connect-sync-feature-directory-extensions.md) yapılandırırken, "Teletex dizesi" türünün Azure AD özniteliği artık seçilebilir.

## <a name="111890"></a>1.1.189.0
Yayınlanma Tarihi: Haziran 2016

**Sabit sorunlar ve iyileştirmeler:**

* Azure AD Connect artık FIPS uyumlu bir sunucuya yüklenebilir.
  * Parola eşitleme için [Bkz. Parola karma eşitleme ve FIPS.](how-to-connect-password-hash-synchronization.md#password-hash-synchronization-and-fips)
* Active Directory Connector'da NetBIOS adının FQDN'ye çözümlenemediği bir sorun giderildi.

## <a name="111800"></a>1.1.180.0
Yayınlanma Tarihi: May 2016

**Yeni özellikler:**

* Azure AD Connect'i çalıştırmadan önce yapmadıysanız etki alanlarını uyarır ve doğrulamanıza yardımcı olur.
* [Microsoft Cloud Germany](reference-connect-instances.md#microsoft-cloud-germany)için destek eklendi.
* Yeni URL gereksinimleriyle en son [Microsoft Azure Kamu bulut](reference-connect-instances.md#microsoft-azure-government) altyapısı için destek eklendi.

**Sabit sorunlar ve iyileştirmeler:**

* Eşitleme kurallarını bulmayı kolaylaştırmak için Eşitleme Kuralı Düzenleyicisi'ne filtreleme eklendi.
* Konektör alanını silerken geliştirilmiş performans.
* Aynı nesne hem silindiğinde hem de aynı çalıştırmaya eklendiğinde (sil/ekle olarak adlandırılır) bir sorun giderildi.
* Devre dışı bırakılmış eşitleme kuralı artık yükseltme veya dizin şema yenilemesindeki nesneleri ve öznitelikleri yeniden etkinleştirmez.

## <a name="111300"></a>1.1.130.0
Yayınlanma Tarihi: Nisan 2016

**Yeni özellikler:**

* [Dizin uzantılarına](how-to-connect-sync-feature-directory-extensions.md)çok değerli öznitelikler için destek eklendi.
* Yükseltme için uygun kabul edilecek [otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) için daha fazla yapılandırma varyasyonları için destek eklendi.
* [Özel zamanlayıcı](how-to-connect-sync-feature-scheduler.md#custom-scheduler)için bazı cmdlets eklendi.

## <a name="111190"></a>1.1.119.0
Yayınlanma: Mart 2016

**Düzeltilen sorunlar:**

* Bu işletim sisteminde parola eşitleme desteklenmediğinden, Windows Server 2008'de (pre-R2) Express yüklemesi kullanılamaz.
* Özel bir filtre yapılandırması ile DirSync yükseltme beklendiği gibi çalışmadı.
* Daha yeni bir sürüme yükseltilirken ve yapılandırmada değişiklik olmadığında, tam bir alma/eşitleme zamanlanmamalıdır.

## <a name="111100"></a>1.1.110.0
Yayınlanma Tarihi: Şubat 2016

**Düzeltilen sorunlar:**

* Yükleme varsayılan C:\Program Dosyaları klasöründe değilse, önceki sürümlerden yükseltme çalışmaz.
* Yükleme **sihirbazının sonundaki eşitleme işlemini başlat'ı** yükleyip temizlerseniz, yükleme sihirbazını ikinci kez çalıştırmak zamanlayıcıyı etkinleştirmez.
* Zamanlayıcı, US-tr tarih/saat biçiminin kullanılmadığı sunucularda beklendiği gibi çalışmaz. Ayrıca doğru `Get-ADSyncScheduler` kez dönmek için engelleyecektir.
* Oturum açma seçeneği ve yükseltme olarak AD FS ile Azure AD Connect'in daha önceki bir sürümüne yüklediyseniz, yükleme sihirbazını yeniden çalıştıramazsınız.

## <a name="111050"></a>1.1.105.0
Yayınlanma Tarihi: Şubat 2016

**Yeni özellikler:**

* Express ayarları müşterileri için [otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) özelliği.
* Yükleme sihirbazında Azure Çok Faktörlü Kimlik Doğrulama ve Ayrıcalıklı Kimlik Yönetimi'ni kullanarak global yöneticiyi destekleyin.
  * Çok Faktörlü Kimlik Doğrulama'yı https://secure.aadcdn.microsoftonline-p.com kullanıyorsanız, proxy'nizin trafiğe de izin vermeniz gerekir.
  * Düzgün çalışması https://secure.aadcdn.microsoftonline-p.com için Çoklu Faktör Kimlik Doğrulaması için güvenilir siteler listenize eklemeniz gerekir.
* İlk yüklemeden sonra kullanıcının oturum açma yöntemini değiştirmeye izin verin.
* Yükleme sihirbazında [Etki Alanı ve OU filtrelemesine](how-to-connect-install-custom.md#domain-and-ou-filtering) izin verin. Bu, tüm etki alanlarının kullanılamadığı ormanlara bağlanmanızı da sağlar.
* [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) eşitleme motoruna yerleşiktir.

**Önizlemeden GA'ya yükseltilen özellikler:**

* [Cihaz writeback](how-to-connect-device-writeback.md).
* [Dizin uzantıları](how-to-connect-sync-feature-directory-extensions.md).

**Yeni önizleme özellikleri:**

* Yeni varsayılan eşitleme döngüsü aralığı 30 dakikadır. Önceki tüm sürümler için üç saat kullanılır. [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) davranışını değiştirmek için destek ekler.

**Düzeltilen sorunlar:**

* DNS etki alanlarını doğrula sayfası etki alanlarını her zaman tanımadı.
* AD FS yapılandırırken etki alanı yöneticisi kimlik bilgileri için istemler.
* Şirket içi AD hesapları, kök etki alanından farklı bir DNS ağacına sahip bir etki alanında bulunuyorsa yükleme sihirbazı tarafından tanınmaz.

## <a name="1091310"></a>1.0.9131.0
Yayınlanma: Aralık 2015

**Düzeltilen sorunlar:**

* Etkin Dizin Etki Alanı Hizmetlerinde (AD DS) parolaları değiştirdiğinizde parola eşitleme çalışmayabilir, ancak parola ayarladığınızda çalışır.
* Proxy sunucunuz olduğunda, yükleme sırasında veya yapılandırma sayfasında yükseltme iptal edilirse Azure AD kimlik doğrulaması başarısız olabilir.
* Sql Server sistem yöneticisi (SA) değilseniz, Azure AD Connect'in önceki sürümünden tam bir SQL Server örneğiyle güncelleştirilme başarısız olur.
* Azure AD Connect'in uzak bir SQL Server ile önceki sürümünden güncellenen güncelleme, "ADSync SQL veritabanına erişilemiyor" hatasını gösterir.

## <a name="1091250"></a>1.0.9125.0
Yayınlanma Tarihi: Kasım 2015

**Yeni özellikler:**

* AD FS'yi Azure AD güvenine yeniden yapılandırabilirsiniz.
* Active Directory şeasını yenileyebilir ve eşitleme kurallarını yeniden oluşturabilir.
* Eşitleme kuralını devre dışı kılabilir.
* "AuthoritativeNull"u eşitleme kuralında yeni bir edebi olarak tanımlayabilir.

**Yeni önizleme özellikleri:**

* [Azure AD Senkronizasyon için Sistem'e Bağlanıyor.](how-to-connect-health-sync.md)
* Azure [AD Etki Alanı Hizmetleri](../user-help/active-directory-passwords-update-your-own-password.md) parola eşitleme desteği.

**Yeni desteklenen senaryo:**

* Birden çok şirket içi Exchange kuruluşunu destekler. Daha fazla bilgi için, [birden çok Active Directory ormanı olan Karma dağıtımlara](https://docs.microsoft.com/previous-versions/exchange-server/exchange-150/jj873754(v=exchg.150))bakın.

**Düzeltilen sorunlar:**

* Parola eşitleme sorunları:
  * Kapsam dışından kapsam dışına taşınan bir nesnenin parolası eşitlenmez. Bu hem OU hem de öznitelik filtreleme içerir.
  * Eşitleme eklemek için yeni bir OU seçmek tam parola eşitleme gerektirmez.
  * Devre dışı bırakılmış bir kullanıcı etkinleştirildiğinde parola eşitleme yapmaz.
  * Parola yeniden deneme sırası sonsuzdur ve kullanımdan kaldırılacak 5.000 nesnenin önceki sınırı kaldırıldı.
* Windows Server 2016 orman işlevsel düzeyiile Active Directory'ye bağlanılmaz.
* İlk yüklemeden sonra grup filtreleme için kullanılan grubu değiştiremezsin.
* Artık Azure AD Connect sunucusunda parola yazma etkinken parola değişikliği yapan her kullanıcı için yeni bir kullanıcı profili oluşturmaz.
* Eşitleme kuralları kapsamlarında Uzun İntesiyon değerlerini kullanamaz.
* Erişileemeyen etki alanı denetleyicileri varsa, "aygıt yazma" onay kutusu devre dışı kalır.

## <a name="1086670"></a>1.0.8667.0
Yayınlanma: Ağustos 2015

**Yeni özellikler:**

* Azure AD Connect yükleme sihirbazı artık tüm Windows Server dillerine yerelleştirildi.
* Azure AD parola yönetimini kullanırken hesap kilidini açmak için destek eklendi.

**Düzeltilen sorunlar:**

* Azure AD Connect yükleme sihirbazı, yüklemeyi ilk başlatan kişi yerine başka bir kullanıcı yüklemeye devam ederse çöker.
* Azure AD Connect'in önceki bir yüklemesi Azure AD Connect eşitlemeyi temiz bir şekilde kaldıramazsa, yeniden yüklemek mümkün değildir.
* Kullanıcı ormanın kök etki alanında değilse veya Active Directory'nin İngilizce olmayan bir sürümü kullanılıyorsa, Express yüklemesini kullanarak Azure AD Connect'i yükleyemezsiniz.
* Active Directory kullanıcı hesabının FQDN'si çözülemiyorsa, "Şemayı gerçekleştiremedi" yanıltıcı bir hata iletisi gösterilir.
* Active Directory Connector'da kullanılan hesap sihirbazın dışında değiştirilirse, sihirbaz sonraki çalıştırmalarda başarısız olur.
* Azure AD Connect bazen etki alanı denetleyicisine yüklenmezse.
* Uzantı öznitelikleri eklenmişse "Evreleme modu"nu etkinleştiremez ve devre dışı kılamaz.
* Parola yazma, Etkin Dizin Bağlayıcısı'ndaki kötü parola nedeniyle bazı yapılandırmalarda başarısız olur.
* Öznitelik filtrelemede ayırt edici bir ad (DN) kullanılıyorsa DirSync yükseltilemez.
* Parola sıfırlama kullanırken aşırı CPU kullanımı.

**Kaldırılan önizleme özellikleri:**

* Önizleme özelliği [Kullanıcı yazma](how-to-connect-preview.md#user-writeback) geri geçici olarak önizleme müşterilerimizin geribildirim dayalı kaldırıldı. Sağlanan geri bildirimleri ele aldıktan sonra tekrar eklenecektir.

## <a name="1086410"></a>1.0.8641.0
Yayınlanma: Haziran 2015

**Azure AD Connect'in ilk sürümü.**

Adı Azure AD Eşitleme'den Azure AD Bağlantısı'na değiştirildi.

**Yeni özellikler:**

* [Ekspres ayarlar yüklemesi](how-to-connect-install-express.md)
* [AD FS'yi yapılandırabilir](how-to-connect-install-custom.md#configuring-federation-with-ad-fs)
* [DirSync'den yükseltilebilir](how-to-dirsync-upgrade-get-started.md)
* [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md)
* Tanıtılan [evreleme modu](how-to-connect-sync-staging-server.md)

**Yeni önizleme özellikleri:**

* [Kullanıcı geri yazma](how-to-connect-preview.md#user-writeback)
* [Grup geri yazma](how-to-connect-preview.md#group-writeback)
* [Cihaz geri yazma](how-to-connect-device-writeback.md)
* [Dizin genişletmeleri](how-to-connect-preview.md)

## <a name="104940501"></a>1.0.494.0501
Yayınlanma Tarihi: May 2015

**Yeni Gereksinim:**

* Azure AD Eşitlemesi artık .NET Framework sürümü 4.5.1'in yüklenmesini gerektirir.

**Düzeltilen sorunlar:**

* Azure AD'den parola yazma, Azure Hizmet Veri Servisi bağlantı hatasıyla başarısız oluyor.

## <a name="104910413"></a>1.0.491.0413
Yayınlanma: Nisan 2015

**Sabit sorunlar ve iyileştirmeler:**

* Geri dönüşüm kutusu etkinse ve ormanda birden çok etki alanı varsa, Etkin Dizin Bağlayıcısı silmeişlemini doğru işlemez.
* Azure Active Directory Connector için içe aktarma işlemlerinin performansı artırıldı.
* Bir grup üyelik sınırını aştığında (varsayılan olarak, sınır 50.000 nesne olarak ayarlanır), grup Azure Etkin Dizini'nde silinir. Yeni davranışta, grup silinmez, bir hata atılır ve yeni üyelik değişiklikleri dışa aktarılmaz.
* Bağlayıcı alanında aynı DN ile aşamalı bir silme zaten varsa, yeni bir nesne sağlanamaz.
* Bazı nesneler, nesne üzerinde herhangi bir değişiklik olmamasına rağmen delta eşitleme sırasında eşitleme için işaretlenir.
* Parola eşitlemeyi zorlamak, tercih edilen DC listesini de kaldırır.
* CSExportAnalyzer bazı nesne durumları ile ilgili sorunlar vardır.

**Yeni özellikler:**

* Birleştirme artık MV'deki "ANY" nesne türüne bağlanabilir.

## <a name="104850222"></a>1.0.485.0222
Yayınlanma: Şubat 2015

**Geliştirmeler:**

* Gelişmiş alma performansı.

**Düzeltilen sorunlar:**

* Parola Eşitleme, öznitelik filtreleme tarafından kullanılan cloudFiltered özniteliği onurlandırıyor. Filtre uygulanmış nesneler artık parola eşitleme kapsamında değildir.
* Topolojinin çok sayıda etki alanı denetleyicisi olduğu nadir durumlarda, parola eşitleme çalışmaz.
* Azure AD/Intune'da aygıt yönetimi etkinleştirildikten sonra Azure AD Bağlayıcısı'ndan içe aktarırken "durmuş sunucu".
* Yabancı Güvenlik Müdürlerini (FSP'leri) aynı ormandaki birden çok etki alanından birleştirmek belirsiz birleştirme hatasına neden olur.

## <a name="104751202"></a>1.0.475.1202
Yayınlanma: Aralık 2014

**Yeni özellikler:**

* Öznitelik tabanlı filtreleme ile parola eşitleme şimdi desteklenir. Daha fazla bilgi için [filtreleme ile Parola eşitleme'ye](how-to-connect-sync-configure-filtering.md)bakın.
* ms-DS-ExternalDirectoryObjectID özniteliği Active Directory'ye geri yazılır. Bu özellik, Office 365 uygulamaları için destek ekler. OAuth2'yi, Karma Exchange Dağıtımı'nda çevrimiçi ve şirket içi posta kutularına erişmek için kullanır.

**Yükseltme sorunları düzeltildi:**

* Oturum açma yardımcısının daha yeni bir sürümü sunucuda kullanılabilir.
* Azure AD Eşitlemeyi'ni yüklemek için özel bir yükleme yolu kullanıldı.
* Geçersiz bir özel birleştirme ölçütü yükseltmeyi engeller.

**Diğer düzeltmeler:**

* Office Pro Plus şablonları düzeltildi.
* Tire ile başlayan kullanıcı adlarının neden olduğu yükleme sorunları düzeltildi.
* Yükleme sihirbazını ikinci kez çalıştırırken kaynakÇapa ayarını kaybetme düzeltildi.
* Parola eşitleme için ETW izleme düzeltildi.

## <a name="104701023"></a>1.0.470.1023
Yayınlanma Tarihi: Ekim 2014

**Yeni özellikler:**

* Birden çok şirket içi Active Directory'den Azure AD'ye parola eşitleme.
* Tüm Windows Server dillerine yerelleştirilmiş yükleme ui' si.

**AADSync 1.0 GA'dan yükseltme**

Azure AD Eşitlemesi zaten yüklüyse, kutudan çıkan eşitleme kurallarından herhangi birini değiştirmiş seniz atmanız gereken bir adım daha vardır. 1.0.470.1023 sürümüne yükselttikten sonra, değiştirdiğiniz eşitleme kuralları yinelenir. Değiştirilen her eşitleme kuralı için aşağıdakileri yapın:

1. Değiştirdiğiniz eşitleme kuralını bulun ve değişikliklere dikkat edin.
1. Eşitleme kuralını silin.
1. Azure AD Eşitleme tarafından oluşturulan yeni eşitleme kuralını bulun ve değişiklikleri yeniden uygulayın.

**Active Directory hesabı için izinler**

Active Directory hesabına, Active Directory'den parola yazılarını okuyabilmek için ek izinler verilmelidir. Verilecek izinler "Çoğaltma Dizin Değişiklikleri" ve "Tüm Değişiklikleri Çoğaltma" olarak adlandırılır. Parola açıklarını okuyabilmek için her iki izinde de gereklidir.

## <a name="104190911"></a>1.0.419.0911
Yayınlanma: Eylül 2014

**Azure AD Eşitlemi'nin ilk sürümü.**

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
