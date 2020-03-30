---
title: Azure Active Directory Connect SSS - | Microsoft Dokümanlar
description: Bu makalede, Azure AD Connect hakkında sık sorulan sorular yanıtlanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/23/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5aa30bc819531ee8cc9cd337648a6cbc661bb29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77149822"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

## <a name="general-installation"></a>Genel kurulum

**S: Güvenlik saldırısı yüzeyini azaltmak için Azure AD Connect sunucumu nasıl sertleştiririm?**

Microsoft, BT ortamınızın bu kritik bileşeni için güvenlik saldırı yüzeyini azaltmak için Azure AD Connect sunucunuzun sertleştirmesini önerir.  Aşağıdaki önerileri izleyerek kuruluşunuz için güvenlik riskleri azalacaktır.

* Azure AD Connect'i bir etki alanı bağlantılı sunucuda dağıtın ve etki alanı yöneticilerine veya sıkı denetimaltındaki diğer güvenlik gruplarına yönetim erişimi kısıtlama

Daha fazla bilgi için bkz: 

* [Yönetici gruplarını güvence altına alma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Yerleşik yönetici hesaplarının güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Saldırı yüzeylerini azaltarak güvenlik iyileştirme ve sürdürme](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Active Directory saldırı yüzeyini azaltma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

**S: Azure Etkin Dizin (Azure AD) Global Yöneticisi'nde iki faktörlü kimlik doğrulama (2FA) etkinleştirilmişse yükleme çalışır mı?**  
Şubat 2016'dan itibaren bu senaryo desteklenir.

**S: Azure AD Connect'i gözetimsiz yüklemenin bir yolu var mı?**  
Azure AD Connect yüklemesi yalnızca yükleme sihirbazını kullandığınızda desteklenir. Gözetimsiz, sessiz bir yükleme desteklenmez.

**S: Bir etki alanına ulaşılamayan bir ormanım var. Azure AD Connect'i nasıl yüklerim?**  
Şubat 2016'dan itibaren bu senaryo desteklenir.

**S: Azure Active Directory Domain Services (Azure AD DS) sağlık aracısı sunucu çekirdeğinde çalışıyor mu?**  
Evet. Aracıyı yükledikten sonra aşağıdaki PowerShell cmdlet'i kullanarak kayıt işlemini tamamlayabilirsiniz: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: Azure AD Connect, iki etki alanından Azure AD'ye eşitlenmeyi destekler mi?**  
Evet, bu senaryo desteklenir. Birden [Çok Etki Alanına](how-to-connect-install-multiple-domains.md)bakın.
 
**S: Azure AD Connect'te aynı Active Directory etki alanı için birden çok bağlayıcınız olabilir mi?**  
Hayır, aynı AD etki alanı için birden çok bağlayıcı desteklenmez. 

**S: Azure AD Connect veritabanını yerel veritabanından uzak bir SQL Server örneğine taşıyabilir miyim?**   
Evet, aşağıdaki adımlar bunun nasıl yapılacılailgili genel yürütülüyor Şu anda daha ayrıntılı bir belge üzerinde çalışıyoruz.
1. LocalDB ADSync veritabanını yedekleyin.
Bunu yapmanın en basit yolu, Azure AD Connect ile aynı makinede yüklü olan SQL Server Management Studio'yu kullanmaktır. Bağlanın *(LocalDb).\ADSync*ve ardından ADSync veritabanını yedekleyin.

2. ADSync veritabanını uzak SQL Server örneğinize geri yükleyin.

3. Azure AD Connect'i varolan [uzak SQL veritabanına](how-to-connect-install-existing-database.md)yükleyin.
   Makale, yerel bir SQL veritabanı kullanarak nasıl geçirilmeye nasıl yol açtığını gösterir. Uzak bir SQL veritabanı nı kullanmaya geçiş yapıyorsunuzsa, işlemin adım 5'inde Windows Sync hizmetinin çalışacağı varolan bir hizmet hesabı da girmeniz gerekir. Bu eşitleme motoru hizmet hesabı burada açıklanmıştır:
   
      **Varolan bir hizmet hesabı kullanın**: Varsayılan olarak, Azure AD Connect kullanılacak eşitleme hizmetleri için sanal bir hizmet hesabı kullanır. Uzak bir SQL Server örneğini kullanıyorsanız veya kimlik doğrulaması gerektiren bir proxy kullanıyorsanız, etki alanında yönetilen bir hizmet hesabı veya hizmet hesabı kullanın ve parolayı bilin. Bu gibi durumlarda kullanılacak olan hesabı girin. Yüklemeyi çalıştıran kullanıcıların, hizmet hesabı için giriş kimlik bilgilerinin oluşturulabilmesi için SQL'de sistem yöneticileri olduğundan emin olun. Daha fazla bilgi için Azure [AD Connect hesaplarını ve izinlerine](reference-connect-accounts-permissions.md#adsync-service-account)bakın. 
   
      En son sürümle, veritabanını sağlama, artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve ardından veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir. Daha fazla bilgi için bkz: [SQL temsilciyönetici izinlerini kullanarak Azure AD Bağlantısını Yükle.](how-to-connect-install-sql-delegation.md)

İşleri basit tutmak için Azure AD Connect'i yükleyen kullanıcıların SQL'de sistem yöneticisi olmasını öneririz. Ancak, son yapılarda artık, [Sql temsilci yönetici izinlerini kullanarak Azure AD Connect'i yükleyin'de](how-to-connect-install-sql-delegation.md)açıklandığı gibi, devralınan SQL yöneticilerini kullanabilirsiniz.

**S: Alanında en iyi uygulamalardan bazıları nelerdir?**  

Aşağıda, mühendislik, destek ve danışmanlarımızın yıllar içinde geliştirdiği en iyi uygulamalardan bazılarını sunan bir bilgilendirme belgesi veremilmektedir.  Bu hızlı bir şekilde başvurulabilir bir madde işareti listesinde sunulur.  Bu liste kapsamlı olmaya çalışsa da, henüz listede yer alamayabilecek ek en iyi uygulamalar olabilir.

- Tam SQL kullanıyorsanız, o zaman yerel vs uzak kalmalıdır
    - Daha az atlama
    - Sorun giderme daha kolay
    - Daha az karmaşıklık
    - Kaynakları SQL'e atamalı ve Azure AD Connect ve OS için ek ek ek intiaça izin vermeli
- Proxy'yi mümkünse atlayın, proxy'yi atlayamıyorsanız zaman atlama değerinin 5 dakikadan büyük olduğundan emin olmanız gerekir.
- Proxy gerekiyorsa, proxy'yi machine.config dosyasına eklemeniz gerekir
- Yerel SQL işleri ve bakım ve bunların Azure AD Connect'i nasıl etkileyeceğini ve özellikle yeniden dizine alma hakkında bilgi edinin
- DNS'nin harici olarak çözebileceğinden emin olun
- Fiziksel veya sanal sunucular kullanıyor olun sunucu [belirtimlerinin](how-to-connect-install-prerequisites.md#hardware-requirements-for-azure-ad-connect) öneri başına olduğundan emin olun
- Sanal bir sunucu kullanıyorsanız, gerekli kaynakların özel olduğundan emin olun
- Disk ve disk yapılandırmasının SQL Server için En İyi Uygulamalarla buluştuğundan emin olun
- İzleme için Azure AD Connect Health'i yükleme ve yapılandırma
- Azure AD Connect'te yerleşik olan Silme Eşiğini kullanın.
- Eklenebilecek tüm değişiklikler ve yeni öznitelikler için hazırlanmak üzere sürüm güncelleştirmelerini dikkatle gözden geçirin
- Her şeyi yedekleme
    - Yedek Anahtarlar
    - Yedekleme Eşitleme Kuralları
    - Yedek Sunucu Yapılandırması
    - Yedekleme SQL Veritabanı
- SQL VSS Writer olmadan SQL'i yedekleyen üçüncü taraf yedekleme aracıları olmadığından emin olun (üçüncü taraf anlık görüntüleri olan sanal sunucularda yaygındır)
- Karmaşıklık ekledikçe kullanılan özel eşitleme kurallarının miktarını sınırlama
- Azure AD Connect Sunucularını Tier 0 Sunucuları olarak ele
- Etki ve doğru iş sürücüleri büyük anlamadan bulut senkronizasyon kurallarını değiştirme leery olun
- Azure AD Connect ve Azure AD Connect Health desteği için doğru URL ve Güvenlik Duvarı bağlantı noktalarının açık olduğundan emin olun
- Sorun giderme ve hayalet nesneleri önlemek için bulut filtreli öznitelikten yararlanın
- Evreleme Sunucusu ile, sunucular arasında tutarlılık sağlamak için Azure AD Connect Yapılandırma Belgeleyicisini kullandığınızdan emin olun
- Evreleme Sunucuları ayrı veri merkezlerinde olmalıdır (Fiziksel Konumlar
- Evreleme sunucuları Yüksek Kullanılabilirlik çözümü olması amaçlanmaz, ancak birden çok evreleme sunucunuz olabilir
- "Gecikme" Evreleme Sunucuları nın tanıtılması, hata durumunda bazı olası kapalı kalma sürelerini azaltabilir
- Önce Evreleme Sunucusu'ndaki tüm yükseltmeleri test edin ve doğrulayın
- Evreleme sunucusuna geçmeden önce her zaman dışa aktarmayı doğrulayın.  İş etkisini azaltmak için Tam İthalat ve Tam Eşitlemeler için hazırlama sunucusundan yararlanın
- Azure AD Bağlantı Sunucuları arasında sürüm tutarlılığını mümkün olduğunca koruyun 

**S: Azure AD Connect'in Çalışma Grubu makinesinde Azure AD Bağlayıcısı hesabını oluşturmasına izin verebilir miyim?**
Hayır.  Azure AD Connect'in Azure AD Bağlayıcısı hesabını otomatik olarak oluşturmasına izin vermek için makinenin etki alanına katılması gerekir.  

## <a name="network"></a>Ağ
**S: Bir güvenlik duvarım, ağ aygıtım veya bağlantıların ağımda açık kalabilecekleri süreyi sınırlayan başka bir şeyim var. Azure AD Connect'i kullandığımda istemci tarafı zaman ödeme eşiğim ne olmalıdır?**  
Tüm ağ yazılımları, fiziksel aygıtlar veya bağlantıların açık kalabileceği maksimum süreyi sınırlayan herhangi bir şey, Azure AD Connect istemcisinin yüklü olduğu sunucu arasındaki bağlantı için en az beş dakika (300 saniye) bir eşik kullanmalıdır ve Azure Etkin Dizini. Bu öneri, daha önce yayımlanmış tüm Microsoft Identity eşitleme araçları için de geçerlidir.

**S: Tek etiketli etki alanları (SLD'ler) desteklendi mi?**  
Bu ağ yapılandırmasına[(makaleye bakınız)](https://support.microsoft.com/help/2269810/microsoft-support-for-single-label-domains)karşı şiddetle tavsiye etsek de, tek bir düzey etki alanı için ağ yapılandırması düzgün çalıştığı sürece Azure AD Connect eşitlemeyi tek bir etiket etki alanıyla birlikte desteklenir.

**S: Bağımsız AD alan ad alanadlarına sahip ormanlar desteklenir mi?**  
Hayır, Azure AD Connect, ayrı ayrı ad alanları içeren şirket içi ormanları desteklemez.

**S: "Noktalı" NetBIOS adları desteklendi mi?**  
Hayır, Azure AD Connect, NetBIOS adının nokta içerdiği şirket içi ormanları veya etki alanlarını desteklemez (.).

**S: Saf IPv6 ortamı desteklendi mi?**  
Hayır, Azure AD Connect saf bir IPv6 ortamını desteklemez.

**S:I çok ormanlı bir ortama sahip olmak ve iki orman arasındaki ağ NAT (Ağ Adresi Çevirisi) kullanıyor. Bu iki orman arasında Azure AD Bağlantısı kullanmak desteklendi mi?**</br>
Hayır, NAT üzerinden Azure AD Connect kullanmak desteklenmez. 

## <a name="federation"></a>Federasyon
**S: Office 365 sertifikamı yenilememi isteyen bir e-posta alırsam ne yapmalıyım?**  
Sertifikayı yenileme hakkında rehberlik için [sertifikaları yenileme'ye](how-to-connect-fed-o365-certs.md)bakın.

**S: Office 365'e güvenen taraf için "Güvenen tarafı otomatik olarak güncelleştir" ayarladım. Belirteç imzalama sertifikam otomatik olarak devrildiğinde herhangi bir işlem yapmak zorunda mıyım?**  
Makalede özetlenen sertifikaları [yenileme](how-to-connect-fed-o365-certs.md)kılavuzunu kullanın.

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucunun yeniden adını almak desteklendi mi?**  
Hayır. Sunucu adının değiştirilmesi, eşitleme altyapısının SQL veritabanı örneğine bağlanamamasını sağlar ve hizmet başlatılamaz.

**S: Yeni Nesil Şifreleme (NGC) eşitleme kuralları FIPS özellikli bir makinede desteklenir mi?**  
Hayır.  Bu, desteklenmeyen bir durumdur.

**S. Azure portalında senkronize edilmiş bir aygıtı (örneğin: HAADJ) devre dışı bıraktıysam, neden yeniden etkinleştirilir?**<br>
Senkronize aygıtlar şirket içinde yazılabilir veya kullanılabilir. İşyerinde eşitlenmiş bir aygıt etkinleştirilirse, daha önce bir yönetici tarafından devre dışı bırakılmış olsa bile Azure portalında yeniden etkinleştirilebilir. Eşitlenmiş bir aygıtı devre dışı bırakın, bilgisayar hesabını devre dışı katmak için şirket içi Active Directory'yi kullanın.

**S. Senkronize edilmiş kullanıcılar için Office 365 veya Azure AD portalında kullanıcı oturum açmaoturumlarını engellersem, oturum açıldıktan sonra neden engellenir?**<br>
Senkronize kullanıcılar şirket içinde yazılabilir veya üzerinde ustalanabilir. Hesap şirket içinde etkinleştirilirse, yönetici tarafından yerleştirilen oturum açma engelini kaldırabilir.

## <a name="identity-data"></a>Kimlik verileri
**S: Azure AD'deki userPrincipalName (UPN) özniteliği neden şirket içi UPN ile eşleşmiyor?**  
Bilgi için şu makalelere bakın:

* [Office 365, Azure veya Intune'daki kullanıcı adları şirket içi UPN veya alternatif giriş kimliğiyle eşleşmiyor](https://support.microsoft.com/kb/2523192)
* [Farklı bir federe etki alanı kullanmak için bir kullanıcı hesabının UPN'sini değiştirdikten sonra değişiklikler Azure Etkin Dizin eşitleme aracı tarafından eşitlenmez](https://support.microsoft.com/kb/2669550)

Azure [AD Connect eşitleme hizmeti özelliklerinde](how-to-connect-syncservice-features.md)açıklandığı gibi, eşitleme altyapısının UPN'yi güncelleştirmesine izin verecek şekilde Azure AD'yi de yapılandırabilirsiniz.

**S: Şirket içinde bir Azure REKLAM grubu yla veya varolan bir Azure REKLAM grubu veya ilgili kişi nesnesi ile ilgili nesneyi yumuşak eşleştirmek için desteklenir mi?**  
Evet, bu yumuşak eşleşme proxyAddress dayanmaktadır. Yumuşak eşleştirme, posta etkin olmayan gruplar için desteklenmez.

**S: ImmutableId özniteliğini varolan bir Azure REKLAM grubunda el ile ayarlamak veya ilgili kişi nesnesini şirket içi Bir Azure REKLAM grubu veya ilgili kişi nesnesi ile sıkı eşleştirmek için desteklenir mi?**  
Hayır, ImmutableId özniteliğini varolan bir Azure REKLAM grubuna veya kişi nesnesine şu anda desteklenmiyor olan zor eşleşmeye göre el ile ayarlıyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: Azure AD Connect için PowerShell cmdlets nerede belgelenmiştir?**  
Bu sitede belgelenen cmdletler dışında, Azure AD Connect'te bulunan diğer PowerShell cmdlet'ler müşteri kullanımı için desteklenmez.

**S: Yapılandırmayı sunucular arasında taşımak için Synchronization Service Manager'da bulunan "Sunucu dışa aktarma/sunucu alma" seçeneğini kullanabilir miyim?**  
Hayır. Bu seçenek tüm yapılandırma ayarlarını almaz ve kullanılmamalıdır. Bunun yerine, ikinci sunucuda temel yapılandırmayı oluşturmak için sihirbazı kullanın ve sunucular arasında herhangi bir özel kuralı taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı düzenleyicisini kullanın. Daha fazla bilgi [için, Bkz. Swing geçiş.](how-to-upgrade-previous-version.md#swing-migration)

**S: Parolalar Azure oturum açma sayfası için önbelleğe alınabilir ve *otomatik tamamlama = "false"* özniteliğiiçeren bir parola giriş öğesi içerdiği için bu önbelleğe alma önlenebilir mi?**  
Şu anda, otomatik tamamlama etiketi de dahil olmak üzere **Parola** alanının HTML özniteliklerinin değiştirilmesi desteklenmez. Şu **anda, Parola** alanına herhangi bir öznitelik eklemenize olanak tanıyan özel JavaScript'e izin veren bir özellik üzerinde çalışıyoruz.

**S: Azure oturum açma sayfası, daha önce başarılı bir şekilde oturum açmış olan kullanıcıların kullanıcı adlarını görüntüler. Bu davranış kapatılabilir mi?**  
Şu anda, otomatik tamamlama etiketi de dahil olmak üzere **Parola** giriş alanının HTML özniteliklerinin değiştirilmesi desteklenmez. Şu **anda, Parola** alanına herhangi bir öznitelik eklemenize olanak tanıyan özel JavaScript'e izin veren bir özellik üzerinde çalışıyoruz.

**S: Eşzamanlı oturumları önlemenin bir yolu var mı?**  
Hayır.

## <a name="auto-upgrade"></a>Otomatik yükseltme

**S: Otomatik yükseltme kullanmanın avantajları ve sonuçları nelerdir?**  
Azure AD Connect yüklemeleri için tüm müşterilere otomatik yükseltmeyi etkinleştirmelerini tavsiye ediyoruz. Bunun avantajı, Azure AD Connect'te bulunan güvenlik açıklarına yönelik güvenlik güncelleştirmeleri de dahil olmak üzere her zaman en son düzeltme ekilerini almanızdır. Yükseltme işlemi acısızdır ve yeni bir sürüm kullanılabilir olur bulunmaz otomatik olarak gerçekleşir. Binlerce Azure AD Connect müşterisi, her yeni sürümde otomatik yükseltme yi kullanır.

Otomatik yükseltme işlemi her zaman ilk yüklemeotomatik yükseltme için uygun olup olmadığını belirler. Uygunsa, yükseltme gerçekleştirilir ve test edilir. Süreç aynı zamanda kurallara ve belirli çevresel faktörlere özel değişiklikler aramayı da içerir. Testler yükseltmenin başarısız olduğunu gösterirse, önceki sürüm otomatik olarak geri yüklenir.

Ortamın boyutuna bağlı olarak, işlem birkaç saat sürebilir. Yükseltme devam ederken, Windows Server Active Directory ile Azure AD arasında eşitleme olmaz.

**S: Benim otomatik yükseltme artık çalıştığını ve yeni bir sürümünü yüklemem gerektiğini söyleyen bir e-posta aldım. Bunu neden yapmam gerekiyor?**  
Geçen yıl, belirli koşullar altında sunucunuzdaki otomatik yükseltme özelliğini devre dışı bırakabilecek bir Azure AD Connect sürümünü yayımladık. Azure AD Connect sürüm 1.1.750.0 sürümünde sorunu giderdik. Sorundan etkilendiyseniz, onarmak için bir PowerShell komut dosyası çalıştırarak veya Azure AD Connect'in en son sürümüne el ile yükselterek bu komut dosyasını azaltabilirsiniz. 

PowerShell komut dosyasını çalıştırmak için [komut dosyasını indirin](https://aka.ms/repairaadconnect) ve yönetimpowershell penceresinde Azure AD Connect sunucunuzda çalıştırın. Komut dosyasının nasıl çalıştırılsüreceğini öğrenmek için [bu kısa videoyu görüntüleyin.](https://aka.ms/repairaadcau)

El ile yükseltmek için AADConnect.msi dosyasının en son sürümünü indirmeniz ve çalıştırmanız gerekir.
 
-  Geçerli sürümünüz 1.1.750.0'dan eskiyse, [en son sürüme indirin ve yükseltin.](https://www.microsoft.com/download/details.aspx?id=47594)
- Azure AD Connect sürümünüz 1.1.750.0 veya sonraki sürümse, başka bir işlem gerekmez. Otomatik yükseltme düzeltmesini içeren sürümü zaten kullanıyorsunuz. 

**S: Otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmemi söyleyen bir e-posta aldım. Sürüm 1.1.654.0 kullanıyorum. Yükseltmeyapmam gerekiyor mu?**  
Evet, otomatik yükseltmeyi yeniden etkinleştirmek için sürüm 1.1.750.0 veya daha sonra yükseltmeniz gerekir. [En son sürüme indirin ve yükseltin.](https://www.microsoft.com/download/details.aspx?id=47594)

**S: Otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmemi söyleyen bir e-posta aldım. Otomatik yükseltmeyi etkinleştirmek için PowerShell'i kullandıysam, yine de en son sürümü yüklemem gerekir mi?**  
Evet, yine de sürüm 1.1.750.0 veya daha sonra yükseltmeniz gerekir. PowerShell ile otomatik yükseltme hizmetini etkinleştirmek, 1.1.750.0'dan önceki sürümlerde bulunan otomatik yükseltme sorununu azaltmaz.

**S: Daha yeni bir sürüme yükseltmek istiyorum ancak Azure AD Connect'i kimin yüklediğinden emin değilim ve kullanıcı adı ve parolamız yok. Buna ihtiyacımız var mı?**
Azure AD Connect'i yükseltmek için başlangıçta kullanılan kullanıcı adını ve parolayı bilmeniz gerekmez. Genel Yönetici rolüne sahip herhangi bir Azure REKLAM hesabını kullanın.

**S: Azure AD Connect'in hangi sürümünü kullandığımı nasıl bulabilirim?**  
Sunucunuzda Azure AD Connect'in hangi sürümünün yüklü olduğunu doğrulamak için Denetim Masası'na gidin ve **Burada** > gösterildiği gibi**Programlar ve Özellikler'i**seçerek Microsoft Azure AD Connect'in yüklü sürümünü arayın:

![Denetim Masası'nda Azure AD Connect sürümü](./media/reference-connect-faq/faq1.png)

**S: Azure AD Connect'in en son sürümüne nasıl yükseltebilirim?**  
En son sürüme nasıl yükseltilen öğrenmek için [Azure AD Connect: Önceki sürümden en son sürümüne yükseltme](how-to-upgrade-previous-version.md)konusuna bakın. 

**S: Geçen yıl Azure AD Connect'in en son sürümüne yükselttik. Tekrar yükseltmemiz gerekiyor mu?**  
Azure AD Connect ekibi, hizmete sık sık güncellemeler yapar. Hata düzeltmeleri ve güvenlik güncelleştirmelerinin yanı sıra yeni özelliklerden yararlanmak için sunucunuzu en son sürümden haberdar etmek önemlidir. Otomatik yükseltmeyi etkinleştirirseniz, yazılım sürümünüz otomatik olarak güncelleştirilir. Azure AD Connect'in sürüm çıkış geçmişini bulmak için [bkz.](reference-connect-version-history.md)

**S: Yükseltmenin gerçeklemesi ne kadar sürer ve kullanıcılarım üzerindeki etkisi nedir?**  
Yükseltme için gereken süre kiracınızın boyutuna bağlıdır. Daha büyük kuruluşlar için yükseltmeyi akşam veya hafta sonu gerçekleştirmek en iyisi olabilir. Yükseltme sırasında eşitleme etkinliği gerçekleşmez.

**S: Azure AD Connect'e yükselttiğine inanıyorum, ancak Office portalı hala DirSync'den bahsediyor. Bu neden?**  
Office ekibi, Office portal güncelleştirmelerini geçerli ürün adını yansıtacak şekilde almak için çalışıyor. Hangi eşitleme aracını kullandığınızı yansıtmaz.

**S: Otomatik yükseltme durumumda "Askıya alındı" yazıyor. Neden askıya alındı? Etkinleştirmeli miyim?**  
Bir hata, belirli koşullar altında otomatik yükseltme durumunu "Askıya Alındı" olarak ayarlayan önceki bir sürümde tanıtıldı. Teknik olarak etkinleştirmek teknik olarak mümkündür, ancak birkaç karmaşık adım gerektirir. Yapabileceğiniz en iyi şey Azure AD Connect'in en son sürümünü yüklemektir.

**S: Şirketimin katı değişim yönetimi gereksinimleri vardır ve ne zaman dışarı itildiğini kontrol etmek istiyorum. Otomatik yükseltmenin ne zaman başlatıldığını kontrol edebilir miyim?**  
Hayır, bugün böyle bir özellik yok. Özellik gelecekteki bir sürüm için değerlendirilmektedir.

**S: Otomatik yükseltme başarısız olursa bir e-posta alır mıyım? Başarılı olduğunu nasıl bileceğim?**  
Yükseltme nin sonucu size bildirilmez. Özellik gelecekteki bir sürüm için değerlendirilmektedir.

**S: Otomatik yükseltmeleri ne zaman dışarı itme yi planladığınız için bir zaman çizelgesi yayımlıyor musunuz?**  
Otomatik yükseltme, daha yeni bir sürümün sürüm sürecinin ilk adımıdır. Yeni bir sürüm olduğunda, yükseltmeler otomatik olarak itilir. Azure AD Connect'in daha yeni sürümleri [Azure AD Yol Haritası'nda](../fundamentals/whats-new.md)önceden duyurulur.

**S: Otomatik yükseltme, Azure AD Connect Health'i de yükseltiyor mu?**  
Evet, otomatik yükseltme, Azure AD Connect Health'i de yükseltir.

**S: Azure AD Connect sunucularını evreleme modunda otomatik olarak da yükseltiyor musunuz?**  
Evet, evreleme modunda olan bir Azure AD Connect sunucusunu otomatik olarak yükseltebilirsiniz.

**S: Otomatik yükseltme başarısız olursa ve Azure AD Connect sunucum başlamazsa ne yapmalıyım?**  
Nadiren, Azure AD Connect hizmeti siz yükseltmeyi yaptıktan sonra başlamaz. Bu gibi durumlarda, sunucunun yeniden başlatılması genellikle sorunu giderir. Azure AD Connect hizmeti hala başlamazsa, bir destek bileti açın. Daha fazla bilgi için [bkz.](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) 

**S: Azure AD Connect'in daha yeni bir sürümüne yükselttiğinde risklerin ne olduğundan emin değilim. Yükseltme konusunda bana yardım etmem için beni arayabilir misin?**  
Azure AD Connect'in daha yeni bir sürümüne yükseltme konusunda yardıma ihtiyacınız varsa, [Office 365 desteğine başvurmak için bir hizmet isteği oluştur'da bir](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/)destek bileti açın.

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili nasıl yardım alabilirim?**

[Microsoft Bilgi Bankası'nda arama (KB)](https://www.microsoft.com/en-us/search/result.aspx?q=azure+active+directory+connect)

* Azure AD Connect desteğiyle ilgili yaygın çözüm çözme sorunlarına teknik çözümler bulmak için KB'de arama yapın.

[Azure Aktif Dizin Forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* [Azure AD topluluğuna](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)giderek teknik soruları ve yanıtları arayın veya kendi sorularınızı sorun.

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

**S: Neden Eşitleme Adım Hataları sonra Olaylar 6311 ve 6401 oluşur görüyorum?**

Olaylar 6311 - **Sunucu bir geri arama yaparken beklenmeyen bir hata ile karşılaştı** ve 6401 - Yönetim **aracısı denetleyicisi beklenmeyen bir hatayla karşılaştı** - her zaman bir eşitleme adımı hatasından sonra günlüğe kaydedilir. Bu hataları gidermek için eşitleme adımı hatalarını temizlemeniz gerekir.  Daha fazla bilgi için, azure AD Connect eşitleme ile eşitleme ve [Sorun Giderme nesne senkronizasyonu](tshoot-connect-objectsync.md) [sırasında Sorun Giderme hatalarına](tshoot-connect-sync-errors.md) bakın
