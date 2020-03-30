---
title: 'Azure AD Connect: Hesaplar ve izinler | Microsoft Dokümanlar'
description: Bu konu, kullanılan ve oluşturulan hesapları ve gerekli izinleri açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 10/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6071e6553fb1275fea63a37b4897aef2685bd509
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253773"
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Hesaplar ve izinler

## <a name="accounts-used-for-azure-ad-connect"></a>Azure AD Connect için kullanılan hesaplar

![hesaplara genel bakış](media/reference-connect-accounts-permissions/account5.png)

Azure AD Connect, şirket içi veya Windows Server Etkin Dizini'nden Azure Active Directory'ye gelen bilgileri senkronize etmek için 3 hesap kullanır.  Bu hesaplar şunlardır:

- **AD DS Bağlayıcı hesabı**: Windows Server Active Directory'ye bilgi okumak/yazmak için kullanılır

- **ADSync hizmet hesabı**: senkronizasyon hizmetini çalıştırmak ve SQL veritabanına erişmek için kullanılır

- **Azure AD Bağlayıcısı hesabı**: Azure AD'ye bilgi yazmak için kullanılır

Azure AD Connect'i çalıştırmak için kullanılan bu üç hesaba ek olarak, Azure AD Connect'i yüklemek için aşağıdaki ek hesaplara da ihtiyacınız olacaktır.  Bunlar:

- **Yerel Yönetici hesabı**: Azure AD Connect'i yükleyen ve makinede yerel Yönetici izinleri olan yönetici.

- **AD DS Kurumsal Yönetici hesabı**: İsteğe bağlı olarak yukarıdaki "AD DS Bağlayıcı hesabı" oluşturmak için kullanılır.

- **Azure AD Global Administrator hesabı**: Azure AD Bağlayıcısı hesabını oluşturmak ve Azure AD'yi yapılandırmak için kullanılır.

- **SQL SA hesabı (isteğe bağlı)**: SQL Server'ın tam sürümünü kullanırken ADSync veritabanını oluşturmak için kullanılır.  Bu SQL Server, Azure AD Connect yüklemesi için yerel veya uzak olabilir.  Bu hesap, Kurumsal Yönetici ile aynı hesap olabilir.  Veritabanının sağlanması artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve daha sonra veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir.  Bu konuda bilgi için sql [temsilci yönetici izinlerini kullanarak Azure AD Connect yükle'ye](how-to-connect-install-sql-delegation.md) bakın


>[!IMPORTANT]
> Build 1.4.####.# itibariyle artık ad DS Bağlayıcı hesabı olarak bir kuruluş yöneticisi veya etki alanı yöneticisi hesabı kullanmak desteklenmez.  **Varolan hesabı kullanırken**kurumsal yönetici veya etki alanı yöneticisi olan bir hesap girmeye çalışırsanız, bir hata alırsınız.

> [!NOTE]
> Azure AD Connect'te kullanılan yönetim hesaplarının bir ESAE Yönetim Ormanı'ndan ("Kırmızı orman" olarak da bilinir) yönetilmesi desteklenir.
> Adanmış yönetim ormanları kuruluşların yönetim hesaplarını, iş istasyonlarını ve grupları üretim ortamından daha güçlü güvenlik denetimleri olan bir ortamda barındırmasına imkan tanır.
> Özel idari ormanlar hakkında daha fazla bilgi edinmek için lütfen [ESAE İdari Orman Tasarım Yaklaşımı'na](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material#esae-administrative-forest-design-approach)bakın.

> [!NOTE]
> Genel Yönetici rolü ilk kurulumdan sonra gerekli değildir ve gerekli tek hesap **Dizin Eşitleme Hesapları** rol hesabı olacaktır. Bu, hesabı Genel Yönetici rolüyle kaldırmak isteyeceğiniz anlamına gelmez. Bu daha az güçlü bir rol için rol değiştirmek için daha iyidir, tamamen hesabı kaldırma hiç tekrar sihirbazı yeniden çalıştırmak gerekiyorsa sorunlara neden olabilir gibi. Rolün ayrıcalığını azaltarak, Azure AD Bağlantı sihirbazını yeniden kullanmanız gerekiyorsa ayrıcalıkları her zaman yeniden yükseltebilirsiniz. 

## <a name="installing-azure-ad-connect"></a>Azure AD Connect'i yükleme
Azure AD Connect yükleme sihirbazı iki farklı yol sunar:

* Ekspres Ayarlar'da sihirbaz daha fazla ayrıcalık gerektirir.  Bu, kullanıcı oluşturmanıza veya izinleri yapılandırmanıza gerek kalmadan yapılandırmanızı kolayca ayarlayabilmesi içindir.
* Özel Ayarlar'da sihirbaz size daha fazla seçenek ve seçenek sunar. Ancak, doğru izinlere kendiniz sahip olduğundan emin olmanız gereken bazı durumlar vardır.



## <a name="express-settings-installation"></a>Ekspres ayarlar yüklemesi
Express ayarlarında yükleme sihirbazı aşağıdakileri ister:

  - AD DS Kurumsal Yönetici kimlik bilgileri
  - Azure AD Global Administrator kimlik bilgileri

### <a name="ad-ds-enterprise-admin-credentials"></a>AD DS Kurumsal Yönetici kimlik bilgileri
AD DS Kurumsal Yönetici hesabı, şirket içi Active Directory'nizi yapılandırmak için kullanılır. Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Etki Alanı Yöneticisi değil, Kurumsal Yönetici, Etkin Dizin'deki izinlerin tüm etki alanlarında ayarlanabileceğinden emin olmalıdır.

DirSync'den yükseltme yapıyorsunuzsa, DIRSync tarafından kullanılan hesabın parolasını sıfırlamak için AD DS Enterprise Admins kimlik bilgileri kullanılır. Azure AD Global Administrator kimlik bilgilerine de ihtiyacınız var.

### <a name="azure-ad-global-admin-credentials"></a>Azure AD Global Yönetici kimlik bilgileri
Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Azure AD değişikliklerini eşitlemek için kullanılan Azure AD Bağlayıcısı hesabını oluşturmak için kullanılır. Hesap ayrıca Azure AD'de özellik olarak eşitleme sağlar.

### <a name="ad-ds-connector-account-required-permissions-for-express-settings"></a>AD DS Bağlayıcı hesabı ekspres ayarlar için gerekli izinler
AD DS Bağlayıcısı hesabı, Windows Server AD'ye okuma ve yazma için oluşturulur ve ekspres ayarlar tarafından oluşturulduğunda aşağıdaki izinlere sahiptir:

| İzin | Kullanıldığı yerler |
| --- | --- |
| <li>Dizin Değişikliklerini Çoğaltma</li><li>Dizin Değişikliklerini Tümünü Çoğaltma |Parola karma eşitleme |
| Tüm özellikleri Oku/Yaz Kullanıcı |İthalat ve Exchange hibrid |
| Tüm özellikleri okuma/yazma iNetOrgPerson |İthalat ve Exchange hibrid |
| Tüm özellikleri Oku/Yaz Grubu |İthalat ve Exchange hibrid |
| Tüm özellikleri Oku/Yaz İletişim |İthalat ve Exchange hibrid |
| Parola sıfırlama |Parola yazmayı etkinleştirme için hazırlık |

### <a name="express-installation-wizard-summary"></a>Ekspres yükleme sihirbazı özeti

![Ekspres kurulum](./media/reference-connect-accounts-permissions/express.png)

Aşağıda, ekspres yükleme sihirbazı sayfalarının, toplanan kimlik bilgilerinin ve bunların ne için kullanıldığının bir özeti ve

| Sihirbaz Sayfası | Toplanan Kimlik Bilgileri | Gerekli İzinler | Için Kullanılır |
| --- | --- | --- | --- |
| Yok |Yükleme sihirbazını çalıştıran kullanıcı |Yerel sunucunun yöneticisi |<li>Eşitleme hizmetini çalıştırmak için kullanılan ADSync hizmet hesabını oluşturur. |
| Azure AD'ye Bağlanma |Azure AD dizin kimlik bilgileri |Azure AD'de genel yönetici rolü |<li>Azure REKLAM dizininde eşitleme etkinleştirme.</li>  <li>Azure AD'de devam eden eşitleme işlemleri için kullanılan Azure AD Bağlayıcısı hesabının oluşturulması.</li> |
| AD DS'ye Bağlanma |Şirket Içi Aktif Dizin kimlik bilgileri |Active Directory'deki Kurumsal Yöneticiler (EA) grubunun üyesi |<li>Active Directory'de AD DS Bağlayıcısı hesabını oluşturur ve bu hesaba izin verir. Oluşturulan bu hesap, eşitleme sırasında dizin bilgilerini okumak ve yazmak için kullanılır.</li> |


## <a name="custom-installation-settings"></a>Özel kurulum ayarları

Özel ayarlar yüklemesi ile sihirbaz size daha fazla seçenek ve seçenek sunar. 

### <a name="custom-installation-wizard-summary"></a>Özel yükleme sihirbazı özeti

Aşağıda, özel yükleme sihirbazı sayfalarının, toplanan kimlik bilgilerinin ve bunların ne için kullanıldığının bir özeti ve

![Ekspres kurulum](./media/reference-connect-accounts-permissions/customize.png)

| Sihirbaz Sayfası | Toplanan Kimlik Bilgileri | Gerekli İzinler | Için Kullanılır |
| --- | --- | --- | --- |
| Yok |Yükleme sihirbazını çalıştıran kullanıcı |<li>Yerel sunucunun yöneticisi</li><li>Tam bir SQL Server kullanıyorsanız, kullanıcı SQL'de Sistem Yöneticisi (SA) olmalıdır</li> |Varsayılan olarak, eşitleme motoru hizmeti hesabı olarak kullanılan yerel hesabı oluşturur. Hesap yalnızca yönetici belirli bir hesap belirtmediğinde oluşturulur. |
| Senkronizasyon hizmetleri yükleme, Servis hesabı seçeneği |AD veya yerel kullanıcı hesabı kimlik bilgileri |Kullanıcı, izinleri yükleme sihirbazı tarafından verilir |Yönetici bir hesap belirtirse, bu hesap eşitleme hizmetinin hizmet hesabı olarak kullanılır. |
| Azure AD'ye Bağlanma |Azure AD dizin kimlik bilgileri |Azure AD'de genel yönetici rolü |<li>Azure REKLAM dizininde eşitleme etkinleştirme.</li>  <li>Azure AD'de devam eden eşitleme işlemleri için kullanılan Azure AD Bağlayıcısı hesabının oluşturulması.</li> |
| Dizinlerinizi bağlama |Azure AD'ye bağlı her orman için şirket içi Etkin Dizin kimlik bilgileri |İzinler, etkinleştirdiğiniz özelliklere bağlıdır ve AD DS Bağlayıcısı oluştur hesabında bulunabilir |Bu hesap, eşitleme sırasında dizin bilgilerini okumak ve yazmak için kullanılır. |
| AD FS Sunucuları |Listedeki her sunucu için sihirbaz, sihirbazı çalıştıran kullanıcının oturum açma kimlik bilgileri bağlanmak için yetersiz olduğunda kimlik bilgilerini toplar |Etki Alanı Yöneticisi |AD FS sunucu rolünün yüklenmesi ve yapılandırması. |
| Web uygulaması proxy sunucuları |Listedeki her sunucu için sihirbaz, sihirbazı çalıştıran kullanıcının oturum açma kimlik bilgileri bağlanmak için yetersiz olduğunda kimlik bilgilerini toplar |Hedef makinede yerel yönetici |WAP sunucu rolünün yüklenmesi ve yapılandırması. |
| Proxy güven kimlik bilgileri |Federasyon hizmet güven kimlik bilgileri (proxy'nin FS'den güven sertifikası na kaydolmak için kullandığı kimlik bilgileri |AD FS sunucusunun yerel yöneticisi olan etki alanı hesabı |FS-WAP güven sertifikasının ilk kaydı. |
| AD FS Hizmet Hesabı sayfası, "Etki alanı kullanıcı hesabı seçeneği kullanın" |AD kullanıcı hesabı kimlik bilgileri |Etki alanı kullanıcısı |Kimlik bilgileri sağlanan Azure AD kullanıcı hesabı, AD FS hizmetinin oturum açma hesabı olarak kullanılır. |

### <a name="create-the-ad-ds-connector-account"></a>AD DS Bağlayıcısı hesabını oluşturma

>[!IMPORTANT]
>ADSyncConfig.psm1 adlı yeni bir PowerShell Modülü, Azure AD DS Bağlayıcısı hesabı için doğru Active Directory izinlerini yapılandırmanıza yardımcı olmak için cmdlets koleksiyonunu içeren build **1.1.880.0** (Ağustos 2018'de piyasaya sürüldü) ile tanıtıldı.
>
>Daha fazla bilgi için Azure [AD Connect: AD DS Bağlayıcı Hesap İzni Yapılandırın](how-to-connect-configure-ad-ds-connector-account.md)

**Dizinleri Bağla** sayfasında belirttiğiniz hesabın yüklemeden önce Etkin Dizin'de bulunması gerekir.  Azure AD Connect sürümü 1.1.524.0 ve daha sonra Azure AD Connect sihirbazı Active Directory'ye bağlanmak için kullanılan **AD DS Bağlayıcı sı hesabını** oluşturmasına izin verme seçeneğine sahiptir.  

Ayrıca gerekli izinleri verilmiş olmalıdır. Yükleme sihirbazı izinleri doğrulamaz ve herhangi bir sorun yalnızca eşitleme sırasında bulunur.

Hangi izinlere ihtiyacınız vardır, etkinleştirdiğiniz isteğe bağlı özelliklere bağlıdır. Birden çok etki alanınız varsa, izinlerin ormandaki tüm etki alanları için verilmesi gerekir. Bu özelliklerden herhangi birini etkinleştirmezseniz, varsayılan **Etki Alanı Kullanıcısı** izinleri yeterlidir.

| Özellik | İzinler |
| --- | --- |
| ms-DS-ConsistencyGuid özelliği |Tasarım Kavramları belgelenen ms-DS-TutarlılıkGuid özniteliği için izinler yazın [- kaynakAnchor olarak ms-DS-TutarlılıkGuid kullanma](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). | 
| Parola karma eşitleme |<li>Dizin Değişikliklerini Çoğaltma</li>  <li>Dizin Değişikliklerini Tümünü Çoğaltma |
| Exchange hibrit dağıtım |Kullanıcılar, gruplar ve kişiler için [Exchange karma geri yazma'da](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) belgelenen özniteliklere izinyazın. |
| Exchange Mail Ortak Klasörü |Ortak klasörler için Exchange [Mail Ortak](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder) Klasörü'nde belgelenen özniteliklerin izinlerini okuyun. | 
| Parola geri yazma |Kullanıcılar için [parola yönetimiyle başlarken](../authentication/howto-sspr-writeback.md) belgelenen özniteliklere izinyazın. |
| Cihaz geri yazma |[Aygıt writeback'inde](how-to-connect-device-writeback.md)açıklandığı gibi PowerShell komut dosyasıyla verilen izinler. |
| Grup geri yazma |**Office 365 Gruplarını** Exchange yüklü bir ormana yazmanızı sağlar.  Daha fazla bilgi için [Bkz. Grup Yazma.](how-to-connect-preview.md#group-writeback)|

## <a name="upgrade"></a>Yükseltme
Azure AD Connect'in bir sürümünden yeni bir sürüme yükselttiğinde aşağıdaki izinlere gereksinim duyarsınız:

>[!IMPORTANT]
>Azure AD Connect, build 1.1.484 ile başlayarak, SQL veritabanını yükseltmek için sysadmin izinleri gerektiren bir regresyon hatası tanıttı.  Bu hata build 1.1.647'de düzeltilir.  Bu yapıya yükseltme yapacaksanız, sysadmin izinlerine ihtiyacınız olacaktır.  Dbo izinleri yeterli değildir.  Sysadmin izinleri olmadan Azure AD Connect'i yükseltmeye çalışırsanız, yükseltme başarısız olur ve Azure AD Connect daha sonra düzgün çalışmaz.  Microsoft bunun farkındadır ve bunu düzeltmek için çalışmaktadır.


| Asıl | Gerekli izinler | Kullanıldığı yerler |
| --- | --- | --- |
| Yükleme sihirbazını çalıştıran kullanıcı |Yerel sunucunun yöneticisi |İkilileri güncelleyin. |
| Yükleme sihirbazını çalıştıran kullanıcı |ADSyncAdmins üyesi |Eşitleme Kuralları ve diğer yapılandırmada değişiklik yapın. |
| Yükleme sihirbazını çalıştıran kullanıcı |Tam bir SQL sunucusu kullanıyorsanız: Eşitleme motoru veritabanının DBO (veya benzeri) |Tabloları yeni sütunlarla güncelleştirme küçülme gibi veritabanı düzeyinde değişiklikler yapın. |

## <a name="more-about-the-created-accounts"></a>Oluşturulan hesaplar hakkında daha fazla şey
### <a name="ad-ds-connector-account"></a>AD DS Bağlayıcı hesabı
Ekspres ayarları kullanıyorsanız, Etkin Dizin'de eşitleme için kullanılan bir hesap oluşturulur. Oluşturulan hesap, Kullanıcılar kapsayıcısındaki orman kökü etki alanında bulunur ve adı **MSOL_.** Hesap, süresi dolmayan uzun karmaşık bir parolayla oluşturulur. Etki alanınızda bir parola ilkesi varsa, bu hesap için uzun ve karmaşık parolalara izin verileceğinden emin olun.

![AD hesabı](./media/reference-connect-accounts-permissions/adsyncserviceaccount.png)

Özel ayarlar kullanıyorsanız, yüklemeyi başlatmadan önce hesabı oluşturmaktan siz sorumlusunuz.  Bkz. AD DS Bağlayıcısı hesabı oluştur.

### <a name="adsync-service-account"></a>AD Eşitleme hizmeti hesabı
Eşitleme hizmeti farklı hesaplar altında çalıştırılabilir. **Sanal Hizmet Hesabı** (VSA), Grup **Yönetilen Hizmet Hesabı** (gMSA/sMSA) veya normal bir kullanıcı hesabı altında çalıştırılabilir. Desteklenen seçenekler, yeni bir yükleme yaptığınızda Connect'in 2017 Nisan sürümüyle değiştirildi. Azure AD Connect'in daha önceki bir sürümünden yükseltme yaparsanız, bu ek seçenekler kullanılamaz.

| Hesap türü | Yükleme seçeneği | Açıklama |
| --- | --- | --- |
| [Sanal Hizmet Hesabı](#virtual-service-account) | Ekspres ve özel, 2017 Nisan ve sonrası | Bu seçenek, Etki Alanı Denetleyicisi'ndeki yüklemeler dışında tüm ekspres yüklemeler için kullanılan seçenektir. Özel için, başka bir seçenek kullanılmadığı sürece varsayılan seçenektir. |
| [Grup Yönetilen Hizmet Hesabı](#group-managed-service-account) | Özel, 2017 Nisan ve sonrası | Uzak bir SQL sunucusu kullanıyorsanız, grup yönetilen bir hizmet hesabı kullanmanızı öneririz. |
| [Kullanıcı hesabı](#user-account) | Ekspres ve özel, 2017 Nisan ve sonrası | AAD_ önceden belirlenmiş bir kullanıcı hesabı yalnızca Windows Server 2008'e yüklendiğinde ve etki alanı denetleyicisi üzerine yüklendiğinde yükleme sırasında oluşturulur. |
| [Kullanıcı hesabı](#user-account) | Ekspres ve özel, 2017 Mart ve daha önce | Yükleme sırasında AAD_ önceden belirlenmiş yerel bir hesap oluşturulur. Özel yükleme kullanırken, başka bir hesap belirtilebilir. |

2017 Mart veya daha önceki bir yapıyla Bağlan'ı kullanıyorsanız, Windows güvenlik nedenleriyle şifreleme anahtarlarını yok ettiği için hizmet hesabındaki parolayı sıfırlamamalısınız. Azure AD Connect'i yeniden yüklemeden hesabı başka bir hesapla değiştiremezsiniz. 2017 Nisan veya daha sonra bir yapıya yükseltme yaptıysanız, hizmet hesabındaki parolayı değiştirmek desteklenir, ancak kullanılan hesabı değiştiremezsiniz.

> [!Important]
> Servis hesabını yalnızca ilk yüklemede ayarlayabilirsiniz. Yükleme tamamlandıktan sonra servis hesabının değiştirilmesi desteklenmez.

Bu, eşitleme hizmeti hesabı için varsayılan, önerilen ve desteklenen seçeneklertablosudur.

Açıklama:

- **Kalın** varsayılan seçeneği ve çoğu durumda önerilen seçeneği gösterir.
- *Varsayılan* seçenek olmadığında önerilen seçeneği gösterir.
- 2008 - Windows Server 2008'e yüklendiğinde varsayılan seçenek
- Kalın olmayan - Desteklenen seçenek
- Yerel hesap - Sunucudaki yerel kullanıcı hesabı
- Etki alanı hesabı - Etki alanı kullanıcı hesabı
- sMSA - [bağımsız Yönetilen Hizmet hesabı](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [grup Yönetilen Hizmet hesabı](https://technet.microsoft.com/library/hh831782.aspx)

| | Yerel veritabanı</br>Express | LocalDB/LocalSQL</br>Özel | Uzak SQL</br>Özel |
| --- | --- | --- | --- |
| **bağımsız/çalışma grubu makinesi** | Desteklenmiyor | **Vsa**</br>Yerel hesap (2008)</br>Yerel hesap |  Desteklenmiyor |
| **etki alanı birleştirilmiş makine** | **Vsa**</br>Yerel hesap (2008) | **Vsa**</br>Yerel hesap (2008)</br>Yerel hesap</br>Etki alanı hesabı</br>sMSA,gMSA | **gMSA**</br>Etki alanı hesabı |
| **Etki Alanı Denetleyicisi** | **Etki alanı hesabı** | *gMSA*</br>**Etki alanı hesabı**</br>sMSA| *gMSA*</br>**Etki alanı hesabı**|

#### <a name="virtual-service-account"></a>Sanal hizmet hesabı
Sanal hizmet hesabı, parolası olmayan ve Windows tarafından yönetilen özel bir hesap türüdür.

![Vsa](./media/reference-connect-accounts-permissions/aadsyncvsa.png)

VSA eşitleme altyapısı ve SQL aynı sunucuda olduğu senaryolar ile kullanılmak üzere tasarlanmıştır. Uzak SQL kullanıyorsanız, bunun yerine Grup Yönetilen Hizmet Hesabı kullanmanızı öneririz.

Bu özellik, Windows Server 2008 R2 veya daha sonra gerektirir. Windows Server 2008'e Azure AD Connect yüklerseniz, yükleme bunun yerine bir [kullanıcı hesabı](#user-account) kullanmaya geri döner.

#### <a name="group-managed-service-account"></a>Grup yönetilen hizmet hesabı
Uzak bir SQL sunucusu kullanıyorsanız, grup **yönetilen**bir hizmet hesabı kullanmanızı öneririz. Aktif Dizinizi Grup Yönetilen Hizmet hesabı için nasıl hazırlayacağınız hakkında daha fazla bilgi için [Grup Yönetilen Hizmet Hesaplarına Genel Bakış'a](https://technet.microsoft.com/library/hh831782.aspx)bakın.

Bu seçeneği kullanmak için [gerekli bileşenleri Yükle](how-to-connect-install-custom.md#install-required-components) **sayfasında, varolan bir hizmet hesabını kullan'ı**ve **Yönetilen Hizmet Hesabı'nı**seçin.  
![Vsa](./media/reference-connect-accounts-permissions/serviceaccount.png)  
Ayrıca [tek başına yönetilen](https://technet.microsoft.com/library/dd548356.aspx)bir hizmet hesabı kullanmak için desteklenir. Ancak, bunlar yalnızca yerel makinede kullanılabilir ve bunları varsayılan sanal hizmet hesabı üzerinden kullanmanın hiçbir yararı yoktur.

Bu özellik Windows Server 2012 veya daha sonra gerektirir. Eski bir işletim sistemi kullanmanız ve uzak SQL kullanmanız gerekiyorsa, bir [kullanıcı hesabı](#user-account)kullanmanız gerekir.

#### <a name="user-account"></a>Kullanıcı hesabı
Yükleme sihirbazı tarafından yerel bir hizmet hesabı oluşturulur (özel ayarlarda kullanılacak hesabı belirtmediğiniz sürece). Hesap önceden belirlenmiş **AAD_** ve gerçek eşitleme hizmeti nin olarak çalışması için kullanılır. Bir Etki Alanı Denetleyicisi'ne Azure AD Connect yüklerseniz, hesap etki alanında oluşturulur. Aşağıdaki durumlarda **AAD_** hizmet hesabı etki alanında bulunmalıdır:
   - SQL sunucusu çalıştıran uzak bir sunucu kullanıyorsunuz
   - kimlik doğrulaması gerektiren bir proxy kullanıyorsunuz

![Eşitleme Hizmeti Hesabı](./media/reference-connect-accounts-permissions/syncserviceaccount.png)

Hesap, süresi dolmayan uzun karmaşık bir parolayla oluşturulur.

Bu hesap, diğer hesapların parolalarını güvenli bir şekilde depolamak için kullanılır. Bu diğer hesaplar parolaları veritabanında şifrelenmiş olarak depolanır. Şifreleme anahtarlarının özel anahtarları, Windows Veri Koruma API 'si (DPAPI) kullanılarak şifreleme hizmetleri gizli anahtar şifrelemesi ile korunur.

Tam bir SQL Server kullanıyorsanız, hizmet hesabı eşitleme altyapısı için oluşturulan veritabanının DBO'sudur. Hizmet, diğer izinlerde beklendiği gibi çalışmaz. Bir SQL girişi de oluşturulur.

Hesap ayrıca dosya, kayıt defteri anahtarları ve Eşitleme Motoru ile ilgili diğer nesneleriçin izin verilir.

### <a name="azure-ad-connector-account"></a>Azure AD Bağlayıcısı hesabı
Eşitleme hizmetinin kullanımı için Azure AD'de bir hesap oluşturulur. Bu hesap, görüntü adıyla tanımlanabilir.

![AD hesabı](./media/reference-connect-accounts-permissions/aadsyncserviceaccount2.png)

Hesabın kullanıldığı sunucunun adı kullanıcı adının ikinci bölümünde tanımlanabilir. Resimde, sunucu adı DC1'dir. Evreleme sunucularınız varsa, her sunucunun kendi hesabı vardır.

Hesap, süresi dolmayan uzun karmaşık bir parolayla oluşturulur. Yalnızca dizin eşitleme görevlerini gerçekleştirmek için izinleri olan özel bir rol **Dizin Eşitleme Hesapları** verilir. Bu özel dahili rol Azure AD Connect sihirbazı dışında kullanılamaz. Azure portalı bu hesabı **Kullanıcı**rolüyle gösterir.

Azure AD'de 20 eşitleme hizmeti hesabı sınırı vardır. Azure REKLAM'ınızda mevcut Azure AD hizmet hesaplarının listesini almak için aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın:`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

Kullanılmayan Azure REKLAM hizmet hesaplarını kaldırmak için aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın:`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

>[!NOTE]
>Yukarıdaki PowerShell komutlarını kullanabilmeniz için Azure [Active Directory PowerShell'i Grafik modülü ne](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0#installing-the-azure-ad-module) bağlamanız ve [Connect-AzureAD'ı](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0) kullanarak Azure AD örneğinize bağlanmanız gerekir

Azure AD Bağlayıcısı hesabının parolasını yönetme veya sıfırlama hakkında daha fazla bilgi için Azure [AD Bağlantı hesabını yönet'e](how-to-connect-azureadaccount.md) bakın

## <a name="related-documentation"></a>İlgili belgeler
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)yle ilgili belgeleri okumadıysanız, aşağıdaki tablo ilgili konulara bağlantılar sağlar.

|Konu başlığı |Bağlantı|  
| --- | --- |
|Azure AD Connect'i indirme | [Azure AD Connect'i indirme](https://go.microsoft.com/fwlink/?LinkId=615771)|
|Hızlı ayarları kullanarak yükleme | [Azure AD Connect'i hızlı yükleme](how-to-connect-install-express.md)|
|Özelleştirilmiş ayarları kullanarak yükleme | [Azure AD Connect özel yüklemesi](./how-to-connect-install-custom.md)|
|DirSync'ten yükseltme | [Azure AD eşitleme aracından (DirSync) yükseltme](how-to-dirsync-upgrade-get-started.md)|
|Yükleme işleminden sonra | [Yüklemeyi doğrulama ve lisans atama](how-to-connect-post-installation.md)|

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
