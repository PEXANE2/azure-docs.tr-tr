---
title: Şirket içi Azure AD parola koruması hakkında SSS-Azure Active Directory
description: Şirket içi Azure AD parola koruması hakkında SSS
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f1f2e06eb6b5f8d402515ff1c07a4163174495d
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68666353"
---
# <a name="azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Şirket içi Azure AD parola koruması-sık sorulan sorular

Bu bölümde, Azure AD parola koruması hakkında sık sorulan birçok soruya yanıtlar verilmektedir.

## <a name="general-questions"></a>Genel sorular

**S: Güvenli parola seçme konusunda kullanıcılara hangi yönergeler verilmelidir?**

Microsoft 'un bu konuyla ilgili geçerli Kılavuzu aşağıdaki bağlantıda bulunabilir:

[Microsoft parola Kılavuzu](https://www.microsoft.com/research/publication/password-guidance)

**S: Şirket içi Azure AD parola koruması, genel olmayan bulutlarda destekleniyor mu?**

Şirket içi Azure AD parola koruması yalnızca genel bulutta desteklenir. Genel olmayan bulut kullanılabilirliği için hiçbir tarih duyurulmamıştır.

**S: Azure AD parola koruma avantajlarını şirket içi kullanıcılarınızın bir alt kümesine nasıl uygulayabilirim?**

Desteklenmiyor. Dağıtım ve etkinleştirildikten sonra Azure AD parola koruması, tüm kullanıcılar eşit güvenlik avantajları elde etmez.

**S: Parola değiştirme ve parola ayarlama (veya sıfırlama) arasındaki fark nedir?**

Parola değişikliği, bir Kullanıcı eski parola hakkında bilgi sahibi olduktan sonra yeni bir parola seçtiğinde. Örneğin, bir Kullanıcı Windows 'da oturum açtığında ve ardından yeni bir parola seçmesi istendiğinde bu durum gerçekleşir.

Bir parola kümesi (bazen parola sıfırlama olarak adlandırılır), bir yönetici bir hesaptaki parolayı yeni bir parolayla Değiştir, örneğin Active Directory Kullanıcılar ve bilgisayarlar Yönetim Aracı ' nı kullanmaktır. Bu işlem, yüksek düzeyde ayrıcalık (genellikle etki alanı Yöneticisi) gerektirir ve işlemi gerçekleştiren kişi genellikle eski parola hakkında bilgi sahibi değildir. Yardım Masası senaryoları genellikle bu işlemi, parolasını unuttueden bir kullanıcıyı öğrenerek, örneğin. Ayrıca, yeni bir kullanıcı hesabı bir parola ile ilk kez oluşturulduğunda parola ayarlama olaylarını da görürsünüz.

Parola doğrulama ilkesi, parola değiştirme veya ayarlama işleminin yapılıp yapılmadığına bakılmaksızın aynı şekilde davranır. Azure AD parola koruması DC Aracısı hizmeti, parola değiştirme veya ayarlama işleminin yapılıp yapılmadığını bildirmek için farklı olayları günlüğe kaydeder.  Bkz. [Azure AD parola koruması izleme ve günlüğe kaydetme](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-monitor).

**S: Active Directory Kullanıcıları ve bilgisayarları Yönetimi ek bileşenini kullanarak zayıf bir parola ayarlamaya çalışırken neden yinelenen parola reddetme olayları günlüğe kaydedilir?**

Active Directory Kullanıcıları ve Bilgisayarları yönetim ek bileşeni, önce Kerberos protokolünü kullanarak yeni parolayı ayarlamaya çalışır. Hata durumunda, ek bileşen eski (SAM RPC) protokolü kullanarak parolayı ayarlamaya yönelik ikinci bir deneme yapar (kullanılan protokoller önemli değildir). Yeni parola Azure AD parola koruması tarafından zayıf kabul edildiğinde, bu iki parola sıfırlama reddetme olayı kümesine neden olur.

**S: Azure AD parola koruma parolası doğrulama olayları neden boş bir kullanıcı adıyla günlüğe kaydediliyor?**

Active Directory, örneğin [NetValidatePasswordPolicy](https://docs.microsoft.com/windows/win32/api/lmaccess/nf-lmaccess-netvalidatepasswordpolicy) API 'sini kullanarak etki alanının geçerli parola karmaşıklığı gereksinimlerini geçirmesinin başarılı olup olmadığını görmek için bir parolayı test etme özelliğini destekler. Bu şekilde bir parola doğrulandığında, test, Azure AD parola koruması gibi parola filtresi DLL tabanlı ürünlerin doğrulanmasını de içerir, ancak belirli bir parola filtresi dll 'sine geçirilen kullanıcı adları boş olur. Bu senaryoda, Azure AD parola koruması Şu anda etkin olan parola ilkesini kullanarak parolayı doğrular ve sonucu yakalamak için bir olay günlüğü iletisi verir, ancak olay günlüğü iletisinde boş Kullanıcı adı alanları olacaktır.

**S: Azure AD parola korumasını diğer parola filtresi tabanlı ürünlerle yan yana yüklemek mi istiyorsunuz?**

Evet. Birden çok kayıtlı parola filtresi dll 'leri için destek, Azure AD parola korumasına özgü olmayan bir temel Windows özelliğidir. Bir parola kabul edilmeden önce tüm kayıtlı parola filtresi dll 'leri kabul etmelidir.

**S: Azure AD parola korumasını Azure 'ı kullanmadan Active Directory ortammda nasıl dağıtırım ve yapılandırabilirim?**

Desteklenmiyor. Azure AD parola koruması, şirket içi Active Directory ortamına genişletilmesini destekleyen bir Azure özelliğidir.

**S: Active Directory düzeyinde ilkenin içeriğini nasıl değiştirebilirim?**

Desteklenmiyor. İlke yalnızca Azure AD Yönetim Portalı kullanılarak yönetilebilir. Ayrıca önceki soruya bakın.

**S: SYSVOL çoğaltma için neden DFSR gereklidir?**

FRS (DFSR 'nin öncül teknolojisi) birçok bilinen soruna sahiptir ve Windows Server Active Directory 'in daha yeni sürümlerinde tümüyle desteklenmez. Azure AD parola korumasının sıfır testi, FRS tarafından yapılandırılan etki alanlarında yapılır.

Daha fazla bilgi için lütfen aşağıdaki makalelere bakın:

[SYSVOL çoğaltmasını DFSR 'ye geçirme durumu](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Son, FRS için Nigh](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**S: Özellik, etki alanı SYSVOL paylaşımında ne kadar disk alanı gerektiriyor?**

Kesin alan kullanımı, Microsoft Global yasaklanmış listesindeki yasaklanmış belirteçlerin sayısı ve uzunluğu, kiracı başına özel liste ve şifreleme ek yükü gibi etkenlere bağlı olduğundan farklılık gösterir. Bu listelerin içeriği gelecekte büyümek için büyük olasılıkla. Göz önünde bulundurularak, özelliğin etki alanı SYSVOL paylaşımında en az beş (5) megabayt alana ihtiyacı olacak şekilde makul bir beklentidir.

**S: DC Aracısı yazılımını yüklemek veya yükseltmek için neden yeniden başlatma gerekiyor?**

Bu gereksinim, çekirdek Windows davranışının oluşmasına neden olur.

**S: Belirli bir proxy sunucusunu kullanmak için DC aracısını yapılandırmak için herhangi bir yol var mı?**

Hayır. Proxy sunucusu durum bilgisiz olduğundan, belirli bir proxy sunucusunun kullanıldığı önemli değildir.

**S: Azure AD parola koruma proxy hizmetini Azure AD Connect gibi diğer hizmetlerle yan yana dağıtmayı sorunsuz mı?**

Evet. Azure AD parola koruma proxy hizmeti ve Azure AD Connect hiçbir şekilde doğrudan çakışmalıdır.

**S: DC aracıları ve proxy 'ler hangi sırada yüklenir ve kaydedilir?**

Proxy aracısı yüklemesi, DC Aracısı yüklemesi, orman kaydı ve proxy kaydı için herhangi bir sıralama desteklenir.

**S: Bu özelliği dağıtmaktan etki alanı denetleyicilerimin performans isabetinden endişe almam gerekir mi?**

Azure AD parola koruması DC Aracısı hizmeti, mevcut bir sağlıklı Active Directory dağıtımında etki alanı denetleyicisi performansını önemli ölçüde etkilememelidir.

Çoğu Active Directory dağıtım parolası değiştirme işlemleri, belirli bir etki alanı denetleyicisindeki genel iş yükünün küçük bir oransıdır. Örnek olarak, 10000 Kullanıcı hesabı ve bir Maxpasswordadge ilkesi içeren Active Directory bir etki alanını 30 gün olarak düşünün. Ortalama olarak, bu etki alanı, tek bir etki alanı denetleyicisi için çok az sayıda işlem olan her gün 10000/30 = ~ bir parola değiştirme işlemi görür. Olası bir en kötü durum senaryosunu göz önünde bulundurun: tek bir DC 'de ~ 333 parola değişikliklerinin tek bir saat içinde yapıldığını varsayalım. Örneğin, bu senaryo birçok çalışanın bir Pazartesi sabah günü üzerinde çalışması halinde gerçekleşebilir. Bu durumda bile, hala önemli bir yük olmayan ~ 333/60 dakika = altı parola değişikliğine bakıyoruz.

Ancak, geçerli etki alanı denetleyicileriniz zaten performans sınırlı düzeylerde çalışıyorsa (örneğin, CPU, disk alanı, disk g/ç gibi), daha önce ek etki alanı denetleyicileri eklemeniz veya kullanılabilir disk alanını genişletmeniz önerilir Bu özellik dağıtılıyor. Yukarıdaki SYSVOL disk alanı kullanımı hakkında yukarıdaki soruya de bakın.

**S: Azure AD parola korumasını etki alanım 'da yalnızca birkaç DC 'de test etmek istiyorum. Kullanıcı parolası değişikliklerini söz konusu DC 'leri kullanacak şekilde zorlamak mümkün midir?**

Hayır. Windows istemci işletim sistemi, Kullanıcı parolasını değiştirdiğinde kullanılacak etki alanı denetleyicisini denetler. Etki alanı denetleyicisi, Active Directory site ve alt ağ atamaları, ortama özgü ağ yapılandırması vb. gibi etkenlere göre belirlenir. Azure AD parola koruması bu faktörleri denetlemez ve kullanıcının parolasını değiştirmek için hangi etki alanı denetleyicisinin seçili olduğunu etkilemez.

Bu hedefe kısmen ulaşmak için bir yol, Azure AD parola korumasını belirli bir Active Directory sitesindeki tüm etki alanı denetleyicilerine dağıtmaktır. Bu yaklaşım, bu siteye atanan Windows istemcileri için ve ayrıca bu istemcilere oturum açan ve parolalarını değiştiren kullanıcılar için makul bir kapsam sağlar.

**S: Azure AD parola koruması DC Aracısı hizmetini yalnızca birincil etki alanı denetleyicisine (PDC) yüklediğimde, etki alanındaki diğer tüm etki alanı denetleyicileri de korunacaktır mi?**

Hayır. Bir kullanıcının parolası, belirli bir PDC olmayan etki alanı denetleyicisinde değiştirildiğinde, şifresiz metin parolası hiçbir zaman PDC 'ye gönderilmez (Bu fikir, yaygın olarak karşılaşılan bir açıklıkdır). Belirli bir DC 'de yeni bir parola kabul edildikten sonra bu DC, söz konusu parolanın kimlik doğrulama protokolüne özgü karmalarını oluşturmak için bu parolayı kullanır ve sonra bu karmaların dizinde devam eder. Şifresiz metin parolası kalıcı değil. Güncelleştirilmiş karmalar daha sonra PDC 'ye çoğaltılır. Kullanıcı parolaları, bazı durumlarda ağ topolojisi ve Active Directory site tasarımı gibi çeşitli faktörlere bağlı olarak doğrudan PDC üzerinde değiştirilebilir. (Önceki soruya bakın.)

Özet olarak, etki alanı genelinde özelliğin% 100 güvenlik kapsamına ulaşmak için PDC 'de Azure AD parola koruması DC Aracısı hizmetinin dağıtımı gerekir. Özelliği PDC 'ye dağıtmak, etki alanındaki diğer DC 'Ler için Azure AD parola koruması güvenlik avantajları sağlamaz.

**S: Azure AD parola koruması için kullanılabilir bir System Center Operations Manager yönetim paketi var mı?**

Hayır.

**S: İlkeyi denetim modunda olacak şekilde yapılandırdığım halde Azure neden zayıf parolaların reddettiği halde kalıyor?**

Denetim modu yalnızca şirket içi Active Directory ortamında desteklenir. Azure, parolaları değerlendirdiğinde "zorla" modunda her zaman örtülü olarak gerçekleştirilir.

## <a name="additional-content"></a>Ek içerik

Aşağıdaki bağlantılar çekirdek Azure AD parola koruma belgelerinin bir parçası değildir, ancak bu özellik hakkında yararlı bir ek bilgi kaynağı olabilir.

[Azure AD parola koruması genel kullanıma sunuldu!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-is-now-generally-available/ba-p/377487)

[E-posta sızdırma koruma Kılavuzu – 15. Bölüm: Microsoft Azure AD parola koruma hizmetini uygulayın (Şirket Içi için çok fazla!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD parola koruması ve akıllı kilitleme artık genel önizlemede!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="microsoft-premierunified-support-training-available"></a>Microsoft Premier\Unified destek eğitimi kullanılabilir

Azure AD parola koruması hakkında daha fazla bilgi edinmek ve ortamınıza dağıtmak istiyorsanız, Premier veya Birleşik destek sözleşimiyle bu müşterilere sunulan Microsoft proaktif bir hizmetten yararlanabilirsiniz. Hizmet Azure Active Directory çağrılır: Parola koruması. Daha fazla bilgi için teknik hesap yöneticinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Burada yanıtlanmayan bir şirket içi Azure AD parola koruma sorunuz varsa, bir geri bildirim öğesi gönderin; teşekkürler!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
