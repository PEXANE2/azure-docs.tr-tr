---
title: Şirket içi Azure AD Parola Koruması SSS
description: Şirket içi Active Directory Etki Alanı Hizmetleri ortamında Azure AD Parola Koruması için sık sorulan soruları gözden geçirme
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: abef6e52e37cfa2faeb426bc59bb0de5a52a6658
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671674"
---
# <a name="azure-ad-password-protection-on-premises-frequently-asked-questions"></a>Azure AD Parola Koruması şirket içinde sık sorulan sorular

Bu bölümde, Azure AD Parola Koruması hakkında sık sorulan birçok sorunun yanıtları verilmektedir.

## <a name="general-questions"></a>Genel sorular

**S: Güvenli parola seçmek için kullanıcılara hangi kılavuz verilmelidir?**

Microsoft'un bu konudaki güncel kılavuzunu aşağıdaki bağlantıdan bulabilirsiniz:

[Microsoft Şifre Kılavuzu](https://www.microsoft.com/research/publication/password-guidance)

**S: Şirket içi Azure AD Parola Koruması genel olmayan bulutlarda mı desteklenir?**

Hayır - şirket içi Azure AD Parola Koruması yalnızca genel bulutta desteklenir. Genel olmayan bulut kullanılabilirliği için tarih açıklanmadı.

Azure AD portalı, genel olmayan bulutlarda bile şirket içi özel "Windows Server Active Directory için Parola koruması" yapılandırmasında değişiklik yapılmasına izin verir; bu tür değişiklikler devam edecek, ancak aksi takdirde yürürlüğe asla. Genel olmayan bulut kimlik bilgileri kullanıldığında şirket içi proxy aracılarının veya ormanların kaydı desteklenmez ve bu tür kayıt girişimleri her zaman başarısız olur.

**S: Azure AD Parola Koruması avantajlarını şirket içi kullanıcılarımın bir alt kümesine nasıl uygulayabilirim?**

Desteklenmiyor. Azure AD Parola Koruması dağıtıldıktan ve etkinleştirildikten sonra ayrımcılık yapmaz - tüm kullanıcılar eşit güvenlik avantajlarından yararlanır.

**S: Parola değişikliği ile parola kümesi (veya sıfırlama) arasındaki fark nedir?**

Parola değişikliği, kullanıcının eski parola hakkında bilgisi olduğunu kanıtladıktan sonra yeni bir parola seçmesidir. Örneğin, bir kullanıcı Windows'da oturum açtığında ve ardından yeni bir parola seçmesi istendiğinde parola değişikliği gerçekleşir.

Parola kümesi (bazen parola sıfırlama olarak adlandırılır), yöneticinin bir hesaptaki parolayı yeni bir parolayla değiştirmesidir, örneğin Etkin Dizin Kullanıcıları ve Bilgisayarlar yönetim aracını kullanarak. Bu işlem yüksek düzeyde ayrıcalık gerektirir (genellikle Etki Alanı Yöneticisi) ve işlemi gerçekleştiren kişinin genellikle eski parola hakkında bilgisi yoktur. Yardım masası senaryoları genellikle parolalarını unutmuş bir kullanıcıya yardımcı olurken parola kümeleri gerçekleştirir. Ayrıca, yepyeni bir kullanıcı hesabı ilk kez bir parolayla oluşturulurken parola ayarlı olayları da görürsünüz.

Parola doğrulama ilkesi, parola değişikliği veya ayar yapılmadan bağımsız olarak aynı şekilde olur. Azure AD Parola Koruma DC Aracısı hizmeti, parola değişikliği veya ayar işlemi nin yapılıp yapılmadığını size bildirmek için farklı olayları kaydeder.  Bkz. [Azure AD Parola Koruması izleme ve günlüğe kaydetme.](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-monitor)

**S: Etkin Dizin Kullanıcıları ve Bilgisayar yönetimi snap-in kullanarak zayıf bir parola ayarlamaya çalışırken yinelenen parola reddi olayları neden günlüğe kaydedilir?**

Active Directory Users and Computers yönetimi snap-in ilk Kerberos protokolü kullanarak yeni şifre ayarlamak için çalışacağız. Başarısız olunması üzerine, snap-in bir eski (SAM RPC) protokolü kullanarak parolayı ayarlamak için ikinci bir girişimde bulunacaktır (kullanılan belirli protokoller önemli değildir). Yeni parola Azure AD Parola Koruması tarafından zayıf kabul edilirse, bu anlık giriş davranışı iki parola sıfırlama reddi olayı kümesinin günlüğe kaydedilmesine neden olur.

**S: Azure AD Parola Koruması parola doğrulama olayları neden boş bir kullanıcı adı ile günlüğe kaydediliyor?**

Active Directory, örneğin [NetValidatePasswordPolicy](https://docs.microsoft.com/windows/win32/api/lmaccess/nf-lmaccess-netvalidatepasswordpolicy) api'sini kullanarak etki alanının geçerli parola karmaşıklığı gereksinimlerini geçip geçip geçmeip geçirip geçirip geçirip geçirgenolmadığını görmek için parolayı sınamayı destekler. Bir parola bu şekilde doğrulandığında, sınama, Azure AD Parola Koruması gibi parola filtresi dll tabanlı ürünlertarafından da doğrulama içerir , ancak belirli bir parola filtresi dll'ye geçirilen kullanıcı adları boş olur. Bu senaryoda, Azure AD Parola Koruması, şu anda etkin olan parola ilkesini kullanarak parolayı doğrulamaya devam eder ve sonucu yakalamak için bir olay günlüğü iletisi yayımlar, ancak olay günlüğü iletisinde boş kullanıcı adı alanları olacaktır.

**S: Azure AD Parola Koruması'nı diğer parola filtresi tabanlı ürünlerle yan yana yüklemek desteklenir mi?**

Evet. Birden çok kayıtlı parola filtresi dlls desteği, Azure AD Parola Koruması'na özgü olmayan temel bir Windows özelliğidir. Bir parola kabul edilmeden önce tüm kayıtlı parola filtresi dlls kabul etmelidir.

**S: Azure'u kullanmadan Active Directory ortamımda Azure AD Parola Koruması'nı nasıl dağıtabilir ve yapılandırabilirim?**

Desteklenmiyor. Azure AD Parola Koruması, şirket içi Active Directory ortamına genişletilmesini destekleyen bir Azure özelliğidir.

**S: Etkin Dizin düzeyinde ilkenin içeriğini nasıl değiştirebilirim?**

Desteklenmiyor. İlke yalnızca Azure AD portalı kullanılarak yönetilebilir. Ayrıca önceki soruya bakın.

**S: Sysvol replikasyonu için neden DFSR gereklidir?**

FRS (DFSR için selefi teknoloji) birçok bilinen sorunları vardır ve Windows Server Active Directory yeni sürümlerinde tamamen desteklenmez. FRS tarafından yapılandırılan etki alanlarında Azure AD Parola Koruması'nın sıfır testi yapılacaktır.

Daha fazla bilgi için lütfen aşağıdaki makalelere bakın:

[Sysvol çoğaltmayı DFSR'ye Geçirme Örneği](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Son FRS için Nigh olduğunu](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

Etki alanınız zaten DFSR kullanmıyorsa, Azure AD Parola Koruması'nı yüklemeden önce DFSR'yi kullanmak üzere değiştirmeniz gerekir. Daha fazla bilgi için aşağıdaki bağlantıya bakın:

[SYSVOL Çoğaltma Geçiş Kılavuzu: FRS DFS Çoğaltma için](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019(v=ws.10))

> [!WARNING]
> Azure AD Parola Koruma DC Aracısı yazılımı şu anda sysvol çoğaltma için FRS kullanmaya devam eden etki alanlarındaki etki alanı denetleyicilerine yüklenir, ancak yazılım bu ortamda düzgün çalışmaz. Ek olumsuz yan etkiler, tek tek dosyaların çoğaltılamaz ve sysvol geri yükleme yordamları başarılı görünüyor ama sessizce tüm dosyaları çoğaltmak için başarısız içerir. Etki alanınızı, hem DFSR'nin doğal yararları hem de Azure AD Parola Koruması dağıtımının engelini kaldırmak için mümkün olan en kısa sürede DFSR'yi kullanmak üzere geçirmelisiniz. Yazılımın gelecekteki sürümleri, hala FRS kullanan bir etki alanında çalışırken otomatik olarak devre dışı bırakılır.

**S: Özellik, sysvol paylaşımı etki alanında ne kadar disk alanı gerektirir?**

Microsoft genel yasaklı listesindeki yasaklı jetonların sayısı ve uzunluğu ve kiracı başına özel liste ve şifreleme ek yükü gibi etkenlere bağlı olduğundan, kesin alan kullanımı değişir. Bu listelerin içeriği gelecekte büyüyebilir. Bunu göz önünde bulundurarak, makul bir beklenti özelliği etki alanı sysvol payı üzerinde alan en az beş (5) megabayt gerekir olmasıdır.

**S: DC aracı yazılımının yüklenmesi veya yükseltilmesi için neden yeniden başlatma gerekiyor?**

Bu gereksinim, temel Windows davranışından kaynaklanır.

**S: Belirli bir proxy sunucusu kullanmak için bir DC aracısını yapılandırmanın bir yolu var mı?**

Hayır. Proxy sunucusu durum ları olmadığı için, hangi proxy sunucusunun kullanıldığı önemli değildir.

**S: Azure AD Koruması Proxy hizmetini Azure AD Connect gibi diğer hizmetlerle yan yana dağıtmak sorun olur mu?**

Evet. Azure AD Parola Koruma Proxy hizmeti ve Azure AD Connect hiçbir zaman birbirleriyle doğrudan çakışmamalıdır.

Ne yazık ki, Azure AD Parola Koruma Proxy yazılımı tarafından yüklenen Microsoft Azure AD Connect Agent Updater hizmetinin sürümü yle [Azure Active Directory Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) yazılımı tarafından yüklenen hizmet sürümü arasında bir uyumsuzluk bulundu. Bu uyumsuzluk, Aracı Updater hizmetinin yazılım güncelleştirmeleri için Azure'a başvuramamasına neden olabilir. Azure AD Parola Koruma Proxy'sini ve Azure Active Directory Application Proxy'yi aynı makineye yüklemeniz önerilmez.

**S: DC ajanları ve vekilleri hangi sırayla kurulmalı ve kaydedilmelidir?**

Proxy aracı yükleme, DC aracı yükleme, orman kaydı ve Proxy kaydı herhangi bir sipariş desteklenir.

**S: Etki alanı denetleyicilerimde bu özelliğin dağıtılmasından kaynaklanan performans isabeti konusunda endişelenmeli miyim?**

Azure AD Parola Koruması DC Aracısı hizmeti, varolan sağlıklı bir Etkin Dizin dağıtımında etki alanı denetleyicisi performansını önemli ölçüde etkilememelidir.

Çoğu Etkin Dizin dağıtımları için parola değiştirme işlemleri, belirli bir etki alanı denetleyicisindeki genel iş yükünün küçük bir oranıdır. Örnek olarak, 10000 kullanıcı hesabı ve 30 gün olarak ayarlanmış bir MaxPasswordAge ilkesine sahip bir Active Directory etki alanı düşünün. Ortalama olarak, bu etki alanı her gün 10000/30=~~333 parola değiştirme işlemi görür, bu da tek bir etki alanı denetleyicisi için bile küçük bir işlem sayısıdır. Olası bir en kötü durum senaryosu düşünün: tek bir DC'deki ~333 parola değişikliklerinin tek bir saat içinde yapıldığını varsayalım. Örneğin, bu senaryo, birçok çalışanın tümü Pazartesi sabahı işe geldiğinde oluşabilir. Bu durumda bile, biz hala ~ 333/60 dakika = dakikada altı şifre değişiklikleri, yine önemli bir yük değildir bakıyoruz.

Ancak, geçerli etki alanı denetleyicileriniz zaten performans sınırlı düzeylerde çalışıyorsa (örneğin, CPU, disk alanı, disk G/Ç, vb. ile ilgili olarak limitler doluyorsa), bu özelliği dağıtmadan önce ek etki alanı denetleyicileri eklemeniz veya kullanılabilir disk alanını genişletmeniz tavsiye edilir. Ayrıca yukarıdaki sysvol disk alanı kullanımı hakkında yukarıdaki soruya bakın.

**S: Azure AD Parola Koruması'nı etki alanımdaki yalnızca birkaç DC'de test etmek istiyorum. Kullanıcı parola değişikliklerini bu özel DC'leri kullanmaya zorlamak mümkün mü?**

Hayır. Windows istemci işletim sistemi, kullanıcı parolasını değiştirdiğinde hangi etki alanı denetleyicisinin kullanıldığını denetler. Etki alanı denetleyicisi Active Directory sitesi ve alt ağ atamaları, ortama özel ağ yapılandırması gibi etkenlere göre seçilir. Azure AD Parola Koruması bu faktörleri denetlemez ve kullanıcının parolasını değiştirmek için hangi etki alanı denetleyicisi seçili ise etkileyemez.

Bu hedefe kısmen ulaşmanın bir yolu, belirli bir Etkin Dizin sitesindeki tüm etki alanı denetleyicilerinde Azure AD Parola Koruması'nı dağıtmaktır. Bu yaklaşım, bu siteye atanan Windows istemcileri için makul bir kapsam sağlar ve bu nedenle de bu istemcilere giriş yapan ve parolalarını değiştiren kullanıcılar için de geçerlidir.

**S: Azure AD Parola Koruması DC Aracı hizmetini yalnızca Birincil Etki Alanı Denetleyicisi'ne (PDC) yüklersem, etki alanındaki diğer tüm etki alanı denetleyicileri de korunur mu?**

Hayır. Bir kullanıcının parolası pdc olmayan bir etki alanı denetleyicisi üzerinde değiştirildiğinde, açık metin parolası Hiçbir zaman PDC'ye gönderilmez (bu fikir yaygın bir yanlış algılamadır). Belirli bir DC'de yeni bir parola kabul edildikten sonra, DC bu parolayı bu parolanın çeşitli kimlik doğrulama protokolüne özgü iş hatalarını oluşturmak için kullanır ve ardından dizindeki bu kalıpları devam eder. Açık metin parolası kalıcı değil. Güncelleştirilmiş hashes sonra PDC çoğaltılır. Kullanıcı parolaları, ağ topolojisi ve Active Directory site tasarımı gibi çeşitli etkenlere bağlı olarak, bazı durumlarda doğrudan PDC üzerinde değiştirilebilir. (Önceki soruya bakın.)

Özetle, Azure AD Parola Koruması DC Aracı hizmetinin PDC'de dağıtılması, özelliğin etki alanı genelinde %100 güvenlik kapsamına ulaşması için gereklidir. Özelliği PDC'de dağıtmak yalnızca etki alanında bulunan diğer DC'ler için Azure AD Parola Koruması güvenlik avantajları sağlamaz.

**S: Neden özel akıllı lokavt aracılar benim şirket içi Active Directory ortamına yüklendikten sonra bile çalışmıyor?**

Özel akıllı kilitleme yalnızca Azure AD'de desteklenir. Azure AD portalındaki özel akıllı kilitleme ayarlarındaki değişikliklerin, aracılar yüklü olsa bile şirket içi Active Directory ortamı üzerinde hiçbir etkisi yoktur.

**S: Azure AD Parola Koruması için bir Sistem Merkezi Operasyon Yöneticisi yönetim paketi kullanılabilir mi?**

Hayır.

**S: İlkeyi Denetim modunda olarak yapılandırmama rağmen Azure AD neden zayıf parolaları hala reddediyor?**

Denetim modu yalnızca şirket içi Active Directory ortamında desteklenir. Azure AD, parolaları değerlendirirken dolaylı olarak her zaman "zorlama" modundadır.

**S: Bir parola Azure AD Parola Koruması tarafından reddedildiğinde kullanıcılarım geleneksel Windows hata iletisini görür. Kullanıcıların gerçekte ne olduğunu bilmeleri için bu hata iletisini özelleştirmek mümkün mü?**

Hayır. Bir parola etki alanı denetleyicisi tarafından reddedildiğinde kullanıcılar tarafından görülen hata iletisi etki alanı denetleyicisi tarafından değil, istemci makinesi tarafından denetlenir. Bu davranış, parolanın varsayılan Active Directory parola ilkeleri veya Azure AD Parola Koruması gibi parola filtresi tabanlı bir çözüm tarafından reddedilmesi yle olur.

## <a name="additional-content"></a>Ek içerik

Aşağıdaki bağlantılar temel Azure AD Parola Koruması belgelerinin bir parçası değildir, ancak özellik hakkında yararlı bir ek bilgi kaynağı olabilir.

[Azure AD Parola Koruması artık genel olarak kullanılabilir!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-is-now-generally-available/ba-p/377487)

[Kimlik Avı Koruma Kılavuzu'nu e-postayla gönder – Bölüm 15: Microsoft Azure AD Şifre Koruma Hizmetini uygulayın (Şirket içi de!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD Parola Koruması ve Akıllı Kilitleme artık Genel Önizleme'de!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="microsoft-premierunified-support-training-available"></a>Microsoft Premier\Unified destek eğitimi kullanılabilir

Azure AD Parola Koruması hakkında daha fazla bilgi edinmek ve ortamınızda dağıtmak istiyorsanız, Premier veya Birleşik destek sözleşmesi olan müşterilerin kullanabileceği Microsoft proaktif hizmetinden yararlanabilirsiniz. Hizmete Azure Etkin Dizini: Parola Koruması adı verilir. Daha fazla bilgi için Teknik Hesap Yöneticinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Burada yanıtlanmamış bir şirket içi Azure AD Parola Koruması sorunuz varsa, aşağıdaki Geri Bildirim öğesi gönderin - teşekkürler!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
