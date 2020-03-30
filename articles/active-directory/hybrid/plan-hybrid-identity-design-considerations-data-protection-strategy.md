---
title: Karma kimlik tasarımı - veri koruma stratejisi Azure | Microsoft Dokümanlar
description: Tanımladığınız iş gereksinimlerini karşılamak için karma kimlik çözümünüz için veri koruma stratejisini tanımlarsınız.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0186d862968259aae73071cfecd7d62443d0256
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109365"
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için veri koruma stratejisini tanımlayın
Bu görevde, tanımladığınız iş gereksinimlerini karşılamak için karma kimlik çözümünüz için veri koruma stratejisini tanımlarsınız:

* [Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [İçerik yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Erişim denetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Veri koruma seçeneklerini tanımlama
[Dizin eşitleme gereksinimlerini](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)belirleyin'de açıklandığı gibi, Microsoft Azure AD şirket içi Active Directory Domain Services (AD DS) ile senkronize olabilir. Bu tümleştirme, kuruluşların kurumsal kaynaklara erişmeye çalışırken kullanıcıların kimlik bilgilerini doğrulamak için Azure AD'yi kullanmasına olanak tanır. Bunu her iki senaryo için de yapabilirsiniz: şirket içinde ve bulutta veri. Azure AD'deki verilere erişim, bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir.

Kimlik doğrulaması alındıktan sonra, kullanıcı ana adı (UPN) kimlik doğrulama belirtecinden okunur. Daha sonra, yetkilendirme sistemi çoğaltılan bölümü ve kullanıcının etki alanına karşılık gelen kapsayıcıyı belirler. Kullanıcının varlığı, etkin durumu ve rolü hakkındaki bilgiler, yetkilendirme sisteminin bu oturumda kullanıcı için hedef kiracıya erişimin izin verilip verilmediğini belirlemesine yardımcı olur. Belirli yetkili eylemler (özellikle kullanıcı ve parola sıfırlama oluşturma) kiracı yöneticinin uyumluluk çabalarını veya soruşturmalarını yönetmek için kullandığı bir denetim izi oluşturur.

Veri hacmi, bant genişliği kullanılabilirliği veya diğer hususlar nedeniyle, şirket içi veri merkezinizden Internet bağlantısı üzerinden verileri Azure Depolama'ya taşımak her zaman mümkün olmayabilir. [Azure Depolama İçe Alma/Dışa Aktarma Hizmeti,](../../storage/common/storage-import-export-service.md) blob depolamasına büyük hacimli verileri yerleştirmek/almak için donanım tabanlı bir seçenek sağlar. [BitLocker şifreli](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) sabit disk sürücülerini doğrudan bulut operatörlerinin içeriği depolama hesabınıza yüklediği veya size geri dönmek için Azure verilerinizi sürücülerinize indirebilecekleri bir Azure veri merkezine göndermenize olanak tanır. Bu işlem için yalnızca şifreli diskler kabul edilir (iş kurulumu sırasında hizmetin kendisi tarafından oluşturulan BitLocker anahtarı kullanılarak). BitLocker tuşu Azure'a ayrı olarak sağlanır ve böylece bant anahtarı paylaşımı sağlanır.

Aktarım sırasındaki veriler farklı senaryolarda gerçekleşebildiğinden, Microsoft Azure'un kiracıların trafiğini birbirinden ayırmak için [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) kullandığını ve ana bilgisayar ve konuk düzeyindegüvenlik duvarları, IP paket filtreleme, bağlantı noktası engelleme ve HTTPS bitiş noktaları gibi ölçüleri kullandığını bilmek de önemlidir. Ancak, Azure'un altyapıdan altyapıya ve altyapıdan müşteriye (şirket içi) dahil olduğu dahili iletişimlerinin çoğu da şifrelenir. Bir diğer önemli senaryo da Azure veri merkezleri içindeki iletişimler; Microsoft, hiçbir VM'nin başka bir sinin IP adresini taklit edemeyebileceğinden veya kulak misafiri olamayacağımdan emin olmak için ağları yönetir. TLS/SSL, Azure Depolama veya SQL Veritabanlarına erişirken veya Bulut Hizmetlerine bağlanırken kullanılır. Bu durumda, müşteri yöneticisi bir TLS/SSL sertifikası almak ve kiracı altyapısına dağıtmakla yükümlüdür. Aynı dağıtımda Sanal Makineler arasında veya Microsoft Azure Sanal Ağı üzerinden tek bir dağıtımda kiracılar arasında hareket eden veri trafiği, HTTPS, SSL/TLS veya diğerleri gibi şifreli iletişim protokolleri aracılığıyla korunabilir.

[Veri koruma gereksinimlerini belirley'deki](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)soruları nasıl yanıtladığınıza bağlı olarak, verilerinizi nasıl korumak istediğinizi ve karma kimlik çözümünüzün bu işlemde size nasıl yardımcı olabileceğini belirleyebilmelisiniz. Aşağıdaki tablo, her veri koruma senaryosu için kullanılabilen Azure tarafından desteklenen seçenekleri gösterir.

| Veri koruma seçenekleri | Bulutta istirahat te | Tesis içinde istirahat | Transit halinde |
| --- | --- | --- | --- |
| BitLocker Sürücü Şifrelemesi |X |X | |
| Sql Server veritabanlarını şifrelemek için |X |X | |
| VM-vm Şifreleme | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Her Azure hizmetinin uyumlu olduğu sertifikalar hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi'nde](https://azure.microsoft.com/support/trust-center/) [Uyumluluk özelliğini](https://azure.microsoft.com/support/trust-center/services/) okuyun.
> Veri koruma seçenekleri çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma bu görev için geçerli değildir. Verilerin her durumu için kullanılabilen tüm seçeneklerden yararlandığınızdan emin olun.
>
>

## <a name="define-content-management-options"></a>İçerik yönetimi seçeneklerini tanımlama

Karma kimlik altyapısını yönetmek için Azure AD'yi kullanmanın bir avantajı da, işlemin son kullanıcıaçısından tamamen saydam olmasıdır. Kullanıcı paylaşılan bir kaynağa erişmeye çalışır, kaynak kimlik doğrulaması gerektirir, kullanıcı belirteci elde etmek ve kaynağa erişmek için Azure AD'ye bir kimlik doğrulama isteği göndermesi zorundadır. Tüm bu işlem, kullanıcı etkileşimi olmadan arka planda gerçekleşir. 

Veri gizliliği yle ilgili olan kuruluşlar genellikle çözümleri için veri sınıflandırması gerektirir. Mevcut şirket içi altyapıları zaten veri sınıflandırmasını kullanıyorsa, Kullanıcının kimliği için ana depo olarak Azure AD'yi kullanmak mümkündür. Veri sınıflandırması için şirket içinde kullanıldığı yaygın bir araç, Windows Server 2012 R2 için [Veri Sınıflandırma Araç Seti](https://msdn.microsoft.com/library/Hh204743.aspx) olarak adlandırılır. Bu araç, özel bulutunuzdaki dosya sunucularında verileri tanımlamaya, sınıflandırmaya ve korumaya yardımcı olabilir. Bu görevi gerçekleştirmek için Windows Server 2012'de [Otomatik Dosya Sınıflandırmasını](https://technet.microsoft.com/library/hh831672.aspx) kullanmak da mümkündür.

Kuruluşunuzun yerinde veri sınıflandırması yoksa, ancak şirket içinde yeni Sunucular eklemeden hassas dosyaları koruması gerekiyorsa, Microsoft [Azure Hakları Yönetimi Hizmetini](https://technet.microsoft.com/library/JJ585026.aspx)kullanabilirler.  Azure RMS, dosyalarınızın ve e-postalarınızın güvenliğini sağlamaya yardımcı olmak için şifreleme, kimlik ve yetkilendirme ilkeleri kullanır ve telefonlar, tabletler ve bilgisayarlar gibi birden çok cihazda çalışır. Azure RMS bir bulut hizmeti olduğundan, korumalı içeriği onlarla paylaşmadan önce güvenleri diğer kuruluşlarla açıkça yapılandırmanız gerekmez. Zaten bir Office 365 veya bir Azure AD dizinine sahiplerse, kuruluşlar arası işbirliği otomatik olarak desteklenir. Azure Active Directory Senkronizasyon Hizmetleri (AAD Syncnization) veya Azure AD Connect'i kullanarak, Azure RMS'nin şirket içi Etkin Dizin hesaplarınız için ortak bir kimliği desteklemesi için gereken dizin özniteliklerini yalnızca eşitleyebilirsiniz.

İçerik yönetiminin önemli bir parçası, kimin hangi kaynağa erişene kadar erişene kadar erişene kadar, kimlik yönetimi çözümü için zengin bir günlük leme yeteneğinin önemli olduğunu anlamaktır. Azure AD, şular dahil olmak üzere 30 gün boyunca günlük sağlar:

* Rol üyeliğindeki değişiklikler (ör. kullanıcı Global Admin rolüne eklendi)
* Kimlik bilgisi güncelleştirmeleri (ör. parola değişiklikleri)
* Etki alanı yönetimi (ör. özel bir etki alanını doğrulama, etki alanını kaldırma)
* Uygulamaları ekleme veya kaldırma
* Kullanıcı yönetimi (ör. kullanıcı ekleme, kaldırma, güncelleme)
* Lisans ekleme veya kaldırma

> [!NOTE]
> Azure'da günlük açma özellikleri hakkında daha fazla bilgi için [Microsoft Azure Güvenlik ve Denetim Günlüğü Yönetimi'ni](https://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) okuyun.
> [İçerik yönetimi gereksinimlerini belirley'deki](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)soruları nasıl yanıtladığınıza bağlı olarak, içeriğin karma kimlik çözümünde nasıl yönetilmesini istediğinizi belirleyebilmelisiniz. Tablo 6'da açıklanan tüm seçenekler Azure AD ile tümleştirme yeteneğine sahip olsa da, hangisinin işletme gereksinimlerinize daha uygun olduğunu tanımlamak önemlidir.
>
>

| İçerik yönetimi seçenekleri | Yararları | Dezavantajlar |
| --- | --- | --- |
| Merkezi şirket içi (Active Directory Rights Management Server) |Verilerin sınıflandırılmasından sorumlu sunucu altyapısı üzerinde tam denetim <br> Windows Server'da yerleşik yetenek, ekstra lisans veya abonelik gerekmez <br> Karma bir senaryoda Azure AD ile tümleştirilebilir <br> Exchange Online ve SharePoint Online gibi Microsoft Çevrimiçi hizmetlerinde bilgi hakları yönetimi (IRM) yeteneklerinin yanı sıra Office 365'i destekler <br> Exchange Server, SharePoint Server ve Windows Server ve Dosya Sınıflandırma Altyapısı (FCI) çalıştıran dosya sunucuları gibi şirket içi Microsoft sunucu ürünlerini destekler. |BT Sunucuya sahip olduğundan, daha yüksek bakım (güncelleştirmeleri, yapılandırmayı ve olası yükseltmeleri takip edin <br> Şirket içinde sunucu altyapısı gerektirme<br> Azure özelliklerinden yerel olarak yararlanmaz |
| Bulutta merkezileştirilmiş (Azure RMS) |Şirket içi çözüme göre yönetilmesi daha kolay <br> Hibrit bir senaryoda AD DS ile entegre edilebilir <br>  Azure AD ile tam entegre <br> Hizmeti dağıtmak için şirket içinde sunucu gerektirmez <br> Exchange Server, SharePoint, Server gibi şirket içi Microsoft sunucu ürünlerini ve Windows Server ve Dosya Sınıflandırması, Altyapı (FCI) çalıştıran dosya sunucularını destekler <br> BT, BYOK yeteneği ile kiracı anahtarı üzerinde tam kontrole sahip olabilir. |Kuruluşunuzun RMS'yi destekleyen bir bulut aboneliği olmalıdır <br> Kuruluşunuzun RMS için kullanıcı kimlik doğrulamasını desteklemek için bir Azure REKLAM dizinine sahip olması gerekir |
| Karma (Azure RMS ile entegre, Şirket Içi Aktif Dizin Hakları Yönetimi Sunucusu) |Bu senaryo, hem şirket içinde hem de bulutta merkezi leştirilmiş avantajları biriktirir. |Kuruluşunuzun RMS'yi destekleyen bir bulut aboneliği olmalıdır <br> Kuruluşunuzun RMS için kullanıcı kimlik doğrulamasını desteklemek için bir Azure REKLAM dizinine sahip olması gerekir, <br> Azure bulut hizmeti ile şirket içi altyapı arasında bağlantı gerektirir |

## <a name="define-access-control-options"></a>Erişim denetimi seçeneklerini tanımlama
Azure AD'de bulunan kimlik doğrulama, yetkilendirme ve erişim denetimi özelliklerinden yararlanarak, şirketinizin merkezi bir kimlik deposu kullanmasına olanak sağlarken, kullanıcıların ve iş ortaklarının aşağıdaki şekilde gösterildiği gibi tek oturum açma (SSO) kullanmasına izin verebilirsiniz:

![merkezi yönetim](./media/plan-hybrid-identity-design-considerations/centralized-management.png)

Merkezi yönetim ve diğer dizinlerle tam entegrasyon

Azure Active Directory, binlerce SaaS uygulamasına ve şirket içi web uygulamalarına tek oturum açma sağlar. Azure [Etkin Dizin federasyonu uyumluluk listesine bakın:](how-to-connect-fed-compatibility.md) Microsoft tarafından sınanan SSO üçüncü taraf hakkında daha fazla ayrıntı için tek oturum açma makalesi uygulamak için kullanılabilecek üçüncü taraf kimlik sağlayıcıları. Bu özellik, kuruluşun kimlik ve erişim yönetimini denetlerken çeşitli B2B senaryoları uygulamasına olanak tanır. Ancak, B2B tasarım işlemi sırasında, iş ortağı tarafından kullanılan kimlik doğrulama yöntemini anlamak ve bu yöntemin Azure tarafından destekleniyorsa doğrulamak önemlidir. Şu anda, aşağıdaki yöntemler Azure AD tarafından desteklenir:

* Güvenlik İddiası Biçimlendirme Dili (SAML)
* OAuth
* Kerberos
* Belirteçler
* Sertifikalar

> [!NOTE]
> Azure'daki her protokol ve yetenekleri hakkında daha fazla bilgi için [Azure Active Directory Authentication Protocols'u](https://msdn.microsoft.com/library/azure/dn151124.aspx) okuyun.
>
>

Mobil iş uygulamaları, Azure AD desteğini kullanarak, çalışanların kurumsal Active Directory kimlik bilgileriyle mobil uygulamalarında oturum açmalarına olanak sağlamak için aynı kolay Mobil Hizmetler kimlik doğrulama deneyimini kullanabilir. Bu özellik sayesinde Azure AD, Mobil Hizmetler'de zaten desteklenen diğer kimlik sağlayıcılarıyla birlikte (Microsoft Hesapları, Facebook Kimliği, Google Kimliği ve Twitter Kimliği dahil) bir kimlik sağlayıcısı olarak desteklenir. Şirket içi uygulamalar, şirketin AD DS'inde bulunan kullanıcının kimlik bilgilerini kullanıyorsa, buluttan gelen iş ortakları ve kullanıcılardan erişim saydam olmalıdır. Kullanıcının Koşullu Erişim denetimini (bulut tabanlı) web uygulamalarına, web API'sına, Microsoft bulut hizmetlerine, üçüncü taraf SaaS uygulamalarına ve yerel (mobil) istemci uygulamalarına yönetebilir ve güvenlik, denetim ve raporlama avantajlarından tek bir Yer. Ancak, uygulamanın üretim dışı bir ortamda veya sınırlı sayıda kullanıcıyla doğrulanması önerilir.

> [!TIP]
> Azure AD'nin AD DS'nin sahip olduğu gibi Grup İlkesi olmadığını belirtmek önemlidir. Aygıtlar için ilkeyi uygulamak için [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx)gibi bir mobil aygıt yönetimi çözümüne ihtiyacınız vardır.
>
>

Kullanıcının kimliği Azure AD kullanılarak doğrulandıktan sonra, kullanıcının sahip olduğu erişim düzeyini değerlendirmek önemlidir. Kullanıcının bir kaynak üzerinde sahip olduğu erişim düzeyi değişebilir. Azure AD, bazı kaynaklara erişimi denetleyerek ek bir güvenlik katmanı ekleyebilirken, kaynağın kendisinin dosya sunucusunda bulunan dosyalar için erişim denetimi gibi ayrı ayrı kendi erişim denetim listesine sahip olabileceğini unutmayın. Aşağıdaki şekil, karma bir senaryoda sahip olabileceğiniz erişim denetimi düzeylerini özetler:

![erişim kontrolü](./media/plan-hybrid-identity-design-considerations/accesscontrol.png)

Şekil X'te gösterilen diyagramdaki her etkileşim, Azure AD tarafından karşılanabilecek bir erişim denetimi senaryosunu temsil eder. Aşağıda her senaryonun bir açıklaması var:

1. Şirket içinde barındırılan uygulamalara Koşullu Erişim: Windows Server 2012 R2 ile AD FS kullanacak şekilde yapılandırılan uygulamalar için erişim ilkelerine sahip kayıtlı aygıtlar kullanabilirsiniz.

2. Azure portalına Erişim Denetimi: Azure, rol tabanlı erişim denetimi (RBAC)'ı kullanarak portala erişimi denetlemenizi de sağlar. Bu yöntem, şirketin azure portalında bir kişinin yapabileceği işlem sayısını kısıtlamasına olanak tanır. PORTALa erişimi denetlemek için RBAC'ı kullanarak BT Yöneticileri aşağıdaki erişim yönetimi yaklaşımlarını kullanarak erişimi devredebilir:

   - Grup tabanlı rol ataması: Yerel Etkin Dizininizden senkronize edilebilen Azure REKLAM gruplarına erişim atayabilirsiniz. Bu, kuruluşunuzun grupları yönetmek için takım lama ve işlemlerde yaptığı mevcut yatırımlardan yararlanmanızı sağlar. Azure AD Premium'un temsilci grup yönetimi özelliğini de kullanabilirsiniz.
   - Azure'da yerleşik rolleri kullanın: Kullanıcıların ve grupların yalnızca işlerini yapmak için gereken görevleri yapma iznine sahip olmalarını sağlamak için üç rol (Sahip, Katılımcı ve Okuyucu) kullanabilirsiniz.
   -  Kaynaklara parçalı erişim: Belirli bir abonelik, kaynak grubu veya web sitesi veya veritabanı gibi tek bir Azure kaynağı için kullanıcılara ve gruplara roller atayabilirsiniz. Bu şekilde, kullanıcıların gereksinim duydukları tüm kaynaklara erişebilmesini ve yönetmeleri gerekmeyen kaynaklara erişmemelerini sağlayabilirsiniz.

   > [!NOTE]
   > Uygulamalar oluşturuyorsanız ve bunlar için erişim denetimini özelleştirmek istiyorsanız, yetkilendirme için Azure AD Uygulama Rolleri'ni de kullanabilirsiniz. Bu özelliği kullanmak için uygulamanızı nasıl oluşturkullanacağınıza ilişkin bu [WebApp-RoleClaims-DotNet örneğini](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) inceleyin.


3. Microsoft Intune ile Office 365 uygulamaları için Koşullu Erişim: BT yöneticileri, şirket kaynaklarını güvence altına almak için Koşullu Erişim aygıt ilkeleri sağlarken, uyumlu aygıtlarda bilgi çalışanlarının hizmetlere erişmesine izin verebilir. 
  
4. Saas uygulamaları için Koşullu Erişim: [Bu özellik,](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work/) uygulama başına çok faktörlü kimlik doğrulama erişim kurallarını ve güvenilir bir ağda olmayan kullanıcılar için erişimi engelleme olanağını yapılandırmanızı sağlar. Çok faktörlü kimlik doğrulama kurallarını uygulamaya atanan tüm kullanıcılara veya yalnızca belirtilen güvenlik grupları içindeki kullanıcılara uygulayabilirsiniz. Kullanıcılar, uygulamaya kuruluşun ağındaki bir IP adresinden erişiyorsa, çok faktörlü kimlik doğrulama gereksiniminin dışında tutulabilir.

Erişim denetimi seçenekleri çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma bu görev için geçerli değildir. Kaynaklarınıza erişimi denetlemenizi gerektiren her senaryo için kullanılabilen tüm seçeneklerden yararlandığınızdan emin olun.

## <a name="define-incident-response-options"></a>Olay yanıt seçeneklerini tanımlama
Azure AD, kullanıcının etkinliğini izleyerek BT'ye ortamdaki olası güvenlik risklerini kimlikle mesken deyardımcı olabilir. BT, kuruluşunuzun dizininin bütünlüğü ve güvenliği hakkında görünürlük kazanmak için Azure AD Access ve Kullanım raporlarını kullanabilir. Bu bilgilerle, bir BT yöneticisi olası güvenlik risklerinin nerede olabileceğini daha iyi belirleyebilir, böylece bu riskleri azaltmayı yeterince planlayabilirler.  [Azure AD Premium aboneliğinde,](../fundamentals/active-directory-get-started-premium.md) BT'nin bu bilgileri almasına olanak tanıyan bir dizi güvenlik raporu vardır. [Azure AD raporları](../reports-monitoring/overview-reports.md) aşağıdaki gibi sınıflandırılır:

* **Anomali raporları**: Anormal olduğu tespit edilen oturum açma olaylarını içerir. Amaç, bu tür etkinliklerden haberdar olmanızı sağlamak ve bir olayın şüpheli olup olmadığını belirlemenizi sağlamaktır.
* **Tümleşik Uygulama raporu**: Bulut uygulamalarının kuruluşunuzda nasıl kullanıldığına ilişkin öngörüler sağlar. Azure Active Directory, binlerce bulut uygulamasıyla tümleştirme sunar.
* **Hata raporları**: Dış uygulamalara hesap verirken oluşabilecek hataları belirtin.
* **Kullanıcıya özel raporlar**: Belirli bir kullanıcı için cihaz/oturum açma etkinlik verilerini görüntüleyin.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 gün içinde denetlenen tüm olayların yanı sıra grup etkinliği değişiklikleri, parola sıfırlama ve kayıt etkinliği nin kaydını içerir.

> [!TIP]
> Bir servis talebi üzerinde çalışan Olay Yanıtı ekibine de yardımcı olabilecek bir diğer rapor da [sızdırılmış kimlik bilgileri](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials/) raporu olan kullanıcıdır. Bu rapor, sızdırılan kimlik bilgileri listesi ile kiracınız arasındaki eşleşmeleri yüzleşir.
>


Azure AD'de bir olay yanıtı soruşturması sırasında kullanılabilecek diğer önemli yerleşik raporlar şunlardır:

* **Parola sıfırlama etkinliği**: yöneticiye kuruluşta parola sıfırlamanın ne kadar etkin bir şekilde kullanıldığına ilişkin bilgiler sağlar.
* **Parola sıfırlama kaydı etkinliği**: kullanıcıların parola sıfırlama yöntemlerini hangi yöntemleri kaydettiklerini ve hangi yöntemleri seçtiklerini anlamalarını sağlar.
* **Grup etkinliği**: Access Paneli'nde başlatılan grup (ör. eklenen veya kaldırılan kullanıcılar) değişikliklerin geçmişini sağlar.

Bir Olay Yanıtı araştırma sürecinde kullanabileceğiniz Azure AD Premium'un temel raporlama özelliğine ek olarak, BT aşağıdakiler gibi bilgileri elde etmek için Denetim Raporu'ndan da yararlanabilir:

* Rol üyeliğindeki değişiklikler (örneğin, kullanıcı Global Yönetici rolüne eklendi)
* Kimlik bilgisi güncelleştirmeleri (örneğin, parola değişiklikleri)
* Etki alanı yönetimi (örneğin, özel bir etki alanını doğrulama, etki alanını kaldırma)
* Uygulamaları ekleme veya kaldırma
* Kullanıcı yönetimi (örneğin, bir kullanıcıyı ekleme, kaldırma, güncelleme)
* Lisans ekleme veya kaldırma

Olay yanıtı seçenekleri çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma bu görev için geçerli değildir. Şirketinizin olay yanıt sürecinin bir parçası olarak Azure AD raporlama özelliğini kullanmanızı gerektiren her senaryo için kullanılabilen tüm seçeneklerden yararlandığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimi görevlerini belirleme](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)
