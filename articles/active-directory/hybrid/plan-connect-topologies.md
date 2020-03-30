---
title: 'Azure AD Connect: Desteklenen topolojiler | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect için desteklenen ve desteklenmeyen topolojileri ayrıntılarıyla
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: conceptual
ms.date: 11/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9618e02f54fbb2a3b92771761c5fcf700d126b5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253838"
---
# <a name="topologies-for-azure-ad-connect"></a>Azure AD Connect için topolojiler
Bu makalede, önemli tümleştirme çözümü olarak Azure AD Connect eşitlemi kullanan çeşitli şirket içi ve Azure Etkin Dizin (Azure AD) topolojileri açıklanmaktadır. Bu makalede, desteklenen ve desteklenmeyen yapılandırmaları içerir.


İşte makalede resimler için efsane:

| Açıklama | Sembol |
| --- | --- |
| Şirket Içi Aktif Dizin ormanı |![Şirket Içi Aktif Dizin ormanı](./media/plan-connect-topologies/LegendAD1.png) |
| Filtreli alma ile şirket içi Active Directory |![Filtreli alma ile Etkin Dizin](./media/plan-connect-topologies/LegendAD2.png) |
| Azure AD Connect eşitleme sunucusu |![Azure AD Connect eşitleme sunucusu](./media/plan-connect-topologies/LegendSync1.png) |
| Azure AD Connect eşitleme sunucusu "evreleme modu" |![Azure AD Connect eşitleme sunucusu "evreleme modu"](./media/plan-connect-topologies/LegendSync2.png) |
| Ön Planda Kimlik Yöneticisi (FIM) 2010 veya Microsoft Identity Manager (MIM) 2016 ile GALSync |![FIM 2010 veya MIM 2016 ile GALSync](./media/plan-connect-topologies/LegendSync3.png) |
| Azure AD Connect eşitleme sunucusu, ayrıntılı |![Azure AD Connect eşitleme sunucusu, ayrıntılı](./media/plan-connect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/plan-connect-topologies/LegendAAD.png) |
| Desteklenmeyen senaryo |![Desteklenmeyen senaryo](./media/plan-connect-topologies/LegendUnsupported.png) |


> [!IMPORTANT]
> Microsoft, Azure AD Connect eşitlemesi değiştirmeyi veya resmi olarak belgelenen eylemler dışında çalıştırmayı desteklemez. Bu yapılandırmalardan veya eylemlerden herhangi biri, tutarsız veya desteklenmeyen bir Azure AD Connect eşitleme durumuna neden olabilir. Sonuç olarak, Microsoft bu tür dağıtımlar için teknik destek sağlayamaz.


## <a name="single-forest-single-azure-ad-tenant"></a>Tek orman, tek Azure AD kiracı
![Tek bir orman ve tek bir kiracı için topoloji](./media/plan-connect-topologies/SingleForestSingleDirectory.png)

En yaygın topoloji, bir veya birden çok etki alanı olan tek bir şirket içi orman ve tek bir Azure AD kiracısıdır. Azure AD kimlik doğrulaması için parola karma eşitleme kullanılır. Azure AD Connect'in hızlı yüklemesi yalnızca bu topolojiyi destekler.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Tek orman, birden çok eşitleme sunucusu tek bir Azure AD kiracısına
![Tek bir orman için desteklenmeyen, filtrelenmiş topoloji](./media/plan-connect-topologies/SingleForestFilteredUnsupported.png)

Aynı Azure AD kiracısına bağlı birden çok Azure AD Connect eşitleme sunucusuna sahip olmak, bir [evreleme sunucusu](#staging-server)dışında desteklenmez. Bu sunucular birbirini dışlayan nesneler kümesiyle eşitlemek üzere yapılandırılacak olsa bile desteklenmez. Tek bir sunucudan ormandaki tüm etki alanlarına ulaşamıyorsanız veya yükü birkaç sunucuya dağıtmak istiyorsanız bu topolojiyi düşünebilirsiniz.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Birden çok orman, tek Azure AD kiracı
![Birden fazla orman ve tek bir kiracı için topoloji](./media/plan-connect-topologies/MultiForestSingleDirectory.png)

Birçok kuruluş, birden çok şirket içi Active Directory ormanı olan ortamlara sahiptir. Birden fazla şirket içi Active Directory ormanına sahip olmanın çeşitli nedenleri vardır. Tipik örnekler, hesap kaynağı ormanlı tasarımlar ve bir birleşme veya satın alma sonucuverilebilir.

Birden çok ormanA sahipolduğunuzda, tüm ormanlartek bir Azure AD Connect eşitleme sunucusu tarafından erişilebilmelidir. Sunucu nun bir etki alanına katılması gerekir. Tüm ormanlara ulaşmak için gerekirse, sunucuyu bir çevre ağına (DMZ, arındırılmış bölge ve ekranlı alt ağ olarak da bilinir) yerleştirebilirsiniz.

Azure AD Connect yükleme sihirbazı, birden çok ormanda temsil edilen kullanıcıları birleştirmek için çeşitli seçenekler sunar. Amaç, bir kullanıcının Azure AD'de yalnızca bir kez temsil edilmesidir. Yükleme sihirbazında özel yükleme yolunda yapılandırabileceğiniz bazı yaygın topolojiler vardır. **Kullanıcılarınızı Benzersiz olarak tanımlayan** sayfada, topolojinizi temsil eden ilgili seçeneği seçin. Konsolidasyon yalnızca kullanıcılar için yapılandırılır. Yinelenen gruplar varsayılan yapılandırmaile konsolide edilmez.

Ortak topolojiler ayrı topolojiler, [tam mesh](#multiple-forests-full-mesh-with-optional-galsync)ve [hesap-kaynak topolojisi](#multiple-forests-account-resource-forest)ile ilgili bölümlerde ele alınmıştır.

Azure AD Connect eşitlemesinde varsayılan yapılandırma varsayar:

* Her kullanıcının yalnızca bir etkin hesabı vardır ve bu hesabın bulunduğu orman kullanıcının kimliğini doğrulamak için kullanılır. Bu varsayım, parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon içindir. UserPrincipalName ve sourceAnchor/immutableID bu ormandan gelir.
* Her kullanıcının yalnızca bir posta kutusu vardır.
* Bir kullanıcı için posta kutusunu barındıran orman, Exchange Global Adres Listesi'nde (GAL) görünen öznitelikler için en iyi veri kalitesine sahiptir. Kullanıcı için posta kutusu yoksa, bu öznitelik değerlerine katkıda bulunmak için herhangi bir orman kullanılabilir.
* Bağlantılı bir posta kutunuz varsa, oturum açma için kullanılan farklı bir ormanda da bir hesap vardır.

Ortamınız bu varsayımlarla eşleşmiyorsa, aşağıdaki şeyler olur:

* Birden fazla etkin hesabınız veya birden fazla posta kutunuz varsa, eşitleme altyapısı birini seçer ve diğerini yok sayar.
* Azure AD'ye başka etkin hesabı olmayan bağlantılı bir posta kutusu dışa aktarılmaz. Kullanıcı hesabı hiçbir grupta üye olarak temsil edilmez. DirSync'deki bağlantılı posta kutusu her zaman normal bir posta kutusu olarak temsil edilir. Bu değişiklik, birden çok orman senaryolarını daha iyi desteklemek için kasıtlı olarak farklı bir davranıştır.

[Varsayılan yapılandırmayı anlamada](concept-azure-ad-connect-sync-default-configuration.md)daha fazla ayrıntı bulabilirsiniz.

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Birden çok orman, birden çok eşitleme sunucusu bir Azure AD kiracısına
![Birden çok orman ve birden çok eşitleme sunucusu için desteklenmeyen topoloji](./media/plan-connect-topologies/MultiForestMultiSyncUnsupported.png)

Tek bir Azure AD kiracısına bağlı birden fazla Azure AD Connect eşitleme sunucusuna sahip olmak desteklenmez. Bunun istisnası bir [evreleme sunucusunun](#staging-server)kullanılmasıdır.

Bu topoloji, tek bir Azure AD kiracısına bağlı **birden çok eşitleme sunucusunun** desteklenmemesi durumunda aşağıdakilerden farklıdır.

### <a name="multiple-forests-single-sync-server-users-are-represented-in-only-one-directory"></a>Birden çok orman, tek eşitleme sunucusu, kullanıcılar yalnızca bir dizinde temsil edilir
![Tüm dizinlerde kullanıcıları yalnızca bir kez temsil etme seçeneği](./media/plan-connect-topologies/MultiForestUsersOnce.png)

![Birden fazla orman ve ayrı topolojinin tasviri](./media/plan-connect-topologies/MultiForestSeparateTopologies.png)

Bu ortamda, tüm şirket içi ormanlar ayrı varlıklar olarak kabul edilir. Başka hiçbir ormanda kullanıcı bulunmaz. Her ormanın kendi Exchange organizasyonu vardır ve ormanlar arasında GALSync yoktur. Bu topoloji, birleşme/satın alma dan sonra veya her iş biriminin bağımsız olarak çalıştığı bir kuruluştaki durum olabilir. Bu ormanlar Azure AD'de aynı kuruluştadır ve birleşik bir GAL ile görünür. Önceki resimde, her ormandaki her nesne metaverse'de bir kez temsil edilir ve hedef Azure AD kiracısında toplanır.

### <a name="multiple-forests-match-users"></a>Birden çok orman: kullanıcıları eşleştirin
Tüm bu senaryolarda yaygın olarak, dağıtım ve güvenlik gruplarının kullanıcıların, kişilerin ve Yabancı Güvenlik Ilkelerinin (FSP) bir karışımını içerebildiğidir. FSP'ler, etkin dizin etki alanı hizmetlerinde (AD DS) bir güvenlik grubundaki diğer ormanların üyelerini temsil etmek için kullanılır. Tüm FSP'ler Azure AD'deki gerçek nesneye göre çözülür.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Birden fazla orman: isteğe bağlı GALSync ile tam örgü
![Birden çok dizinde kullanıcı kimlikleri varsa eşleştirme için posta özniteliğini kullanma seçeneği](./media/plan-connect-topologies/MultiForestUsersMail.png)

![Birden fazla orman için tam örgü topolojisi](./media/plan-connect-topologies/MultiForestFullMesh.png)

Tam bir örgü topolojisi, kullanıcıların ve kaynakların herhangi bir ormanda bulunmasını sağlar. Genellikle, ormanlar arasında iki yönlü güvenler vardır.

Exchange birden fazla ormanda mevcutsa, (isteğe bağlı olarak) şirket içi GALSync çözümü olabilir. Her kullanıcı daha sonra diğer tüm ormanlarda bir kişi olarak temsil edilir. GALSync yaygın olarak FIM 2010 veya MIM 2016 ile uygulanmaktadır. Azure AD Connect şirket içi GALSync için kullanılamaz.

Bu senaryoda, kimlik nesneleri posta özniteliği üzerinden birleştirilir. Bir ormanda posta kutusu olan bir kullanıcı, diğer ormanlarındaki kişilerle birleştirilir.

### <a name="multiple-forests-account-resource-forest"></a>Birden çok orman: hesap kaynağı ormanı
![Birden çok dizin arasında kimlikler bulunduğunda eşleştirme için ObjectSID ve msExchMasterAccountSID özniteliklerini kullanma seçeneği](./media/plan-connect-topologies/MultiForestUsersObjectSID.png)

![Birden fazla orman için hesap kaynağı orman topolojisi](./media/plan-connect-topologies/MultiForestAccountResource.png)

Hesap kaynağı orman topolojisinde, etkin kullanıcı hesaplarına sahip bir veya daha fazla *hesap* ormanın vardır. Ayrıca, devre dışı bırakılmış hesapları olan bir veya daha fazla *kaynak* ormanınız da vardır.

Bu senaryoda, bir (veya daha fazla) kaynak ormanı tüm hesap ormanlarına güvenir. Kaynak ormanı genellikle Exchange ve Lync ile genişletilmiş bir Active Directory şemasına sahiptir. Tüm Exchange ve Lync hizmetleri, diğer ortak hizmetlerle birlikte, bu ormanda yer almaktadır. Kullanıcıların bu ormanda devre dışı bırakılmış bir kullanıcı hesabı vardır ve posta kutusu hesap ormanına bağlıdır.

## <a name="office-365-and-topology-considerations"></a>Office 365 ve topoloji konuları
Bazı Office 365 iş yüklerinin desteklenen topolojiler üzerinde belirli kısıtlamaları vardır:

| İş yükü | Kısıtlamalar |
| --------- | --------- |
| Exchange Online | Exchange Online tarafından desteklenen hibrit topolojiler hakkında daha fazla bilgi için [birden çok Active Directory ormanı olan Karma dağıtımlara](https://technet.microsoft.com/library/jj873754.aspx)bakın. |
| Skype Kurumsal | Birden çok şirket içi orman kullanıyorsanız, yalnızca hesap kaynağı orman topolojisi desteklenir. Daha fazla bilgi için [Skype for Business Server 2015 için Çevresel gereksinimlere](https://technet.microsoft.com/library/dn933910.aspx)bakın. |

Daha büyük bir kuruluşsanız, [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md) özelliğini kullanmayı düşünmelisiniz. Kullanıcıkaynaklarının hangi veri merkezi bölgesinde bulunduğunu tanımlamanıza olanak tanır.

## <a name="staging-server"></a>Evreleme sunucusu
![Bir topolojide sunucu evreleme](./media/plan-connect-topologies/MultiForestStaging.png)

Azure AD Connect, ikinci bir sunucunun *evreleme modunda*yüklenmesini destekler. Bu moddaki bir sunucu bağlı tüm dizinlerden gelen verileri okur, ancak bağlı dizinlere hiçbir şey yazmaz. Normal eşitleme döngüsünü kullanır ve bu nedenle kimlik verilerinin güncelleştirilmiş bir kopyasına sahiptir.

Birincil sunucunun başarısız olduğu bir felakette, evreleme sunucusunda başarısız olabilirsiniz. Bunu Azure AD Connect sihirbazında yaparsınız. Birincil sunucuyla altyapı paylaşılmaydığından, bu ikinci sunucu farklı bir veri merkezinde bulunabilir. Birincil sunucuda yapılan yapılandırma değişikliğini el ile ikinci sunucuya kopyalamanız gerekir.

Yeni bir özel yapılandırmayı ve bunun verileriniz üzerindeki etkisini sınamak için bir hazırlama sunucusu kullanabilirsiniz. Değişiklikleri önizleyebilir ve yapılandırmayı ayarlayabilirsiniz. Yeni yapılandırmadan memnun olduğunuzda, evreleme sunucusunu etkin sunucu yapabilir ve eski etkin sunucuyu hazırlama moduna ayarlayabilirsiniz.

Etkin eşitleme sunucusunu değiştirmek için de bu yöntemi kullanabilirsiniz. Yeni sunucuyu hazırlayın ve hazırlama moduna ayarlayın. İyi durumda olduğundan emin olun, hazırlama modunu devre dışı kaldırın (etkin hale getirin) ve şu anda etkin olan sunucuyu kapatın.

Farklı veri merkezlerinde birden fazla yedekleme yapmak istediğinizde birden fazla hazırlama sunucusuna sahip olmak mümkündür.

## <a name="multiple-azure-ad-tenants"></a>Birden çok Azure AD kiracı
Bir kuruluş için Azure AD'de tek bir kiracıya sahip olmanızı öneririz.
Birden çok Azure AD kiracısı kullanmayı planlamadan önce, [Azure AD'deki makale Yönetim birimleri yönetimine](../users-groups-roles/directory-administrative-units.md)bakın. Tek bir kiracı kullanabileceğiniz yaygın senaryoları kapsar.

![Birden fazla orman ve birden fazla kiracı için topoloji](./media/plan-connect-topologies/MultiForestMultiDirectory.png)

Azure AD Connect eşitleme sunucusu ile Azure AD kiracıarasında 1:1 ilişkisi vardır. Her Azure AD kiracısı için bir Azure AD Connect eşitleme sunucusu yüklemesi gerekir. Azure AD kiracı örnekleri tasarım tarafından yalıtılır. Diğer bir deyişle, bir kiracıdaki kullanıcılar diğer kiracıdaki kullanıcıları göremiyor. Bu ayrımı istiyorsanız, bu desteklenen bir yapılandırmadır. Aksi takdirde, tek bir Azure AD kiracı modelini kullanmanız gerekir.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Azure AD kiracısında her nesne yalnızca bir kez
![Tek bir orman için filtrelenmiş topoloji](./media/plan-connect-topologies/SingleForestFiltered.png)

Bu topolojide, her Azure AD kiracısına bir Azure AD Connect eşitleme sunucusu bağlanır. Azure AD Connect eşitleme sunucularının filtreleme için yapılandırılması gerekir, böylece her birinin üzerinde çalışacak her biri birbirini dışlayan nesneler kümesi vardır. Örneğin, her sunucuya belirli bir etki alanı veya kuruluş birimi kapsamını açabilirsiniz.

Bir DNS etki alanı yalnızca tek bir Azure AD kiracısında kaydedilebilir. Şirket içi Active Directory örneğindeki kullanıcıların UPN'leri de ayrı ad alanları kullanmalıdır. Örneğin, önceki resimde, şirket içi Active Directory örneğinde üç ayrı UPN soneki kaydedilir: contoso.com, fabrikam.com ve wingtiptoys.com. Her şirket içi Active Directory etki alanındaki kullanıcılar farklı bir ad alanı kullanır.

>[!NOTE]
>Global Adres Listesi Senkronizasyonu (GalSync) bu topolojide otomatik olarak yapılmaz ve her kiracının Exchange Online ve Skype for Business Online'da tam bir Küresel Adres Listesi (GAL) olduğundan emin olmak için ek bir özel MIM uygulaması gerektirir.


Bu topoloji, aksi desteklenen senaryolar üzerinde aşağıdaki kısıtlamalara sahiptir:

* Azure AD kiracılarından yalnızca biri, şirket içi Etkin Dizin örneğiyle bir Exchange hibritini etkinleştirebilir.
* Windows 10 aygıtları yalnızca bir Azure AD kiracısıyla ilişkilendirilebilir.
* Parola karma eşitleme ve geçiş kimlik doğrulaması için tek oturum açma (SSO) seçeneği yalnızca bir Azure AD kiracısıyla kullanılabilir.

Nesneleri birbirini dışlayan bir küme için gereksinim ilerler, writeback için de geçerlidir. Bazı geri yazma özellikleri, şirket içinde tek bir yapılandırma varsayalım, çünkü bu topoloji ile desteklenmez. Bu özellikler şunlardır:

* Varsayılan yapılandırma ile grup yazma geri.
* Cihaz geri yazma.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Azure AD kiracısında her nesne birden çok kez
![Tek bir orman ve birden çok kiracı için desteklenmeyen topoloji](./media/plan-connect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Tek bir orman ve birden çok bağlayıcı için desteklenmeyen topoloji](./media/plan-connect-topologies/SingleForestMultiConnectorsUnsupported.png)

Bu görevler desteklenmez:

* Aynı kullanıcıyı birden çok Azure AD kiracısıyla eşitle.
* Bir Azure AD kiracısındaki kullanıcıların başka bir Azure AD kiracısında kişi olarak görünmesi için yapılandırma değişikliği yapın.
* Birden çok Azure AD kiracısına bağlanmak için Azure AD Connect eşitlesini değiştirin.

### <a name="galsync-by-using-writeback"></a>Writeback kullanarak GALSync
![GALSync'in Azure AD'ye odaklanarak birden çok orman ve birden çok dizin için desteklenmeyen topolojisi](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Birden fazla orman ve birden fazla dizin için desteklenmeyen topoloji, GALSync şirket içi Active Directory'ye odaklanarak](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD kiracıları tasarım tarafından yalıtılır. Bu görevler desteklenmez:

* Başka bir Azure AD kiracısının verilerini okumak için Azure AD Connect eşitleme yapılandırmasını değiştirin.
* Azure AD Connect eşitlemeyi kullanarak kullanıcıları şirket içi başka bir Etkin Dizin örneğine kişi olarak dışa aktarın.

### <a name="galsync-with-on-premises-sync-server"></a>Şirket içi eşitleme sunucusuyla GALSync
![Birden fazla orman ve birden fazla dizin için bir topolojide GALSync](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM 2010 veya MIM 2016'yı iki Exchange kuruluşu arasında kullanıcıları senkronize etmek için (GALSync üzerinden) şirket içinde kullanabilirsiniz. Bir kuruluştaki kullanıcılar diğer kuruluşta yabancı kullanıcı/ilgili kişi olarak görünür. Bu farklı şirket içi Active Directory örnekleri daha sonra kendi Azure AD kiracılarıyla eşitlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu senaryolar için Azure AD Connect'i nasıl yükleyirdiğinizi öğrenmek için [Azure AD Connect'in Özel yüklemesine](how-to-connect-install-custom.md)bakın.

Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
