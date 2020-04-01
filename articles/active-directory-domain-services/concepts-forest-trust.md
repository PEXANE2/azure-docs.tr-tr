---
title: Azure AD Etki Alanı Hizmetleri için güvenler nasıl çalışır? Microsoft Dokümanlar
description: Azure AD Etki Alanı Hizmetleri ile orman güveninin nasıl çalıştığı hakkında daha fazla bilgi edinin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 903881a1d15c1f043e381f50e5b69d661cd08192
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476442"
---
# <a name="how-trust-relationships-work-for-resource-forests-in-azure-active-directory-domain-services"></a>Azure Etkin Dizin Etki Alanı Hizmetleri'ndeki kaynak ormanları için güven ilişkileri nasıl çalışır?

Etkin Dizin Etki Alanı Hizmetleri (AD DS), etki alanı ve orman güven ilişkileri aracılığıyla birden çok etki alanı veya orman genelinde güvenlik sağlar. Güvenler arasında kimlik doğrulama sıcanmadan önce, Windows'un öncelikle bir kullanıcı, bilgisayar veya hizmet tarafından talep edilen etki alanının istenen hesabın etki alanıyla güven ilişkisi olup olmadığını denetlemesi gerekir.

Bu güven ilişkisini denetlemek için, Windows güvenlik sistemi isteği alan sunucu için etki alanı denetleyicisi (DC) ile isteyen hesabın etki alanında bir DC arasında bir güven yolu hesaplar.

AD DS ve Windows dağıtılmış güvenlik modeli tarafından sağlanan erişim denetim mekanizmaları etki alanı ve orman güvenlerinin çalışması için bir ortam sağlar. Bu güvenlerin düzgün çalışabilmesi için, her kaynağın veya bilgisayarın bulunduğu etki alanında dc'ye doğrudan bir güven yolu olması gerekir.

Güven yolu, güvenilen etki alanı yetkilisine doğrulanmış bir uzaktan yordam çağrısı (RPC) bağlantısı kullanılarak Net Logon hizmeti tarafından uygulanır. Güvenli bir kanal, etki alanları arası güven ilişkileri aracılığıyla diğer AD DS etki alanlarını da genişletir. Bu güvenli kanal, kullanıcılar ve gruplar için güvenlik tanımlayıcıları (SIT'ler) dahil olmak üzere güvenlik bilgilerini elde etmek ve doğrulamak için kullanılır.

## <a name="trust-relationship-flows"></a>Güven ilişkisi akışları

Güvenler üzerinden güvenli iletişim akışı bir güvenin esnekliğini belirler. Bir güveni nasıl oluşturduğunuz veya yapılandırdığınız, iletişimin ormanlar içinde veya arasında ne kadar genişler olduğunu belirler.

Güvenler üzerinden iletişim akışı güven in yönü tarafından belirlenir. Güvenler tek yönlü veya çift yönlü olabilir ve geçişli veya geçişsiz olabilir.

Aşağıdaki diyagram, *Ağaç 1* ve *Ağaç 2'deki* tüm etki alanlarının varsayılan olarak geçişli güven ilişkileri olduğunu gösterir. Sonuç olarak, *Ağaç 1'deki* kullanıcılar Ağaç *2'deki* etki alanlarındaki kaynaklara erişebilir ve *Ağaç 1'deki* kullanıcılar kaynakta uygun izinler atandığında *Ağaç 2'deki*kaynaklara erişebilir.

![İki orman arasındaki güven ilişkilerinin diyagramı](./media/concepts-forest-trust/trust-relationships.png)

### <a name="one-way-and-two-way-trusts"></a>Tek yönlü ve çift yönlü güvenler

Güven ilişkileri, kaynaklara erişimin tek yönlü veya çift yönlü olmasını sağlar.

Tek yönlü güven, iki etki alanı arasında oluşturulan tek yönlü bir kimlik doğrulama yoludur. *Etki Alanı A* ve Etki *Alanı B*arasında tek yönlü güven, Etki Alanı *A* kullanıcıları Etki *Alanı B'deki*kaynaklara erişebilir. Ancak, *Etki Alanı B'deki* kullanıcılar Etki *Alanı A'daki*kaynaklara erişemez.

Bazı tek yönlü güvenler, oluşturulan güven türüne bağlı olarak geçişsiz veya geçişli olabilir.

İki yönlü bir güvende, *Etki Alanı A* Etki Alanı *B'ye* güvenir ve Etki *Alanı B* Etki *Alanı A'ya*güvenir. Bu yapılandırma, kimlik doğrulama isteklerinin iki etki alanı arasında her iki yönde de geçirilebileceği anlamına gelir. Bazı iki yönlü ilişkiler, oluşturulan güven türüne bağlı olarak geçişsiz veya geçişli olabilir.

AD DS ormanındaki tüm etki alanı güvenleri çift yönlü, geçişli güvenlerdir. Yeni bir alt etki alanı oluşturulduğunda, yeni alt etki alanı ile üst etki alanı arasında otomatik olarak iki yönlü, geçişli bir güven oluşturulur.

### <a name="transitive-and-non-transitive-trusts"></a>Geçişli ve geçişsiz güvenler

Geçiş, bir güvenin oluşturulduğu iki etki alanının dışına genişletilip genişletilemeyeceğini belirler.

* Geçişli güven, diğer etki alanlarıyla güven ilişkilerini genişletmek için kullanılabilir.
* Geçişsiz bir güven, diğer etki alanlarıyla olan güven ilişkilerini reddetmek için kullanılabilir.

Bir ormanda yeni bir etki alanı oluşturduğunuzda, yeni etki alanı ve üst etki alanı arasında otomatik olarak iki yönlü, geçişli bir güven ilişkisi oluşturulur. Alt etki alanları yeni etki alanına eklenirse, güven yolu etki alanı hiyerarşisi boyunca yukarı doğru akar ve yeni etki alanı ile ana etki alanı arasında oluşturulan ilk güven yolunu genişletir. Geçişli güven ilişkileri oluşturulduğu sırada bir etki alanı ağacından yukarı doğru akar ve etki alanı ağacındaki tüm etki alanları arasında geçişli güvenler oluşturur.

Kimlik doğrulama istekleri bu güven yollarını izler, böylece ormandaki herhangi bir etki alanından gelen hesapların ormandaki herhangi bir etki alanı tarafından kimlik doğrulaması yapılabilir. Tek bir oturum açma işlemiyle, uygun izinlere sahip hesaplar ormandaki herhangi bir etki alanında kaynaklara erişebilir.

## <a name="forest-trusts"></a>Ormanlar güvenleri

Orman güvenleri, bölümlenmiş bir AD DS altyapılarını yönetmenize ve birden çok ormandaki kaynaklara ve diğer nesnelere erişimi desteklemenize yardımcı olur. Orman güvenleri, hizmet sağlayıcılar, birleşme veya satın almalar geçiren şirketler, işbirlikçi iş extranet'leri ve idari özerklik için çözüm arayan şirketler için yararlıdır.

Orman güvenlerini kullanarak, iki farklı ormanı tek yönlü veya çift yönlü geçişli güven ilişkisi oluşturmak için bağlayabilirsiniz. Orman güveni, yöneticilerin iki AD DS ormanını tek bir güven ilişkisiyle birbirine bağlayarak ormanlar arasında sorunsuz bir kimlik doğrulama ve yetkilendirme deneyimi sağlamasına olanak tanır.

Bir orman güveni yalnızca bir ormandaki orman kökü etki alanı ile başka bir ormandaki orman kökü etki alanı arasında oluşturulabilir. Orman güvenleri yalnızca iki orman arasında oluşturulabilir ve örtülü olarak üçüncü bir ormana genişletilemez. Bu davranış, *Orman 1* ve *Orman 2*arasında bir orman güveni oluşturulursa ve Orman *2* ile *Orman 3*arasında başka bir orman güveni oluşturulursa, Orman *1'in Orman 3*ile örtülü bir güveni olmadığı anlamına gelir. *Forest 1*

Aşağıdaki diyagram, tek bir kuruluştaki üç AD DS ormanı arasındaki iki ayrı orman güven ilişkisi gösterir.

![Orman diyagramı, tek bir kuruluş içindeki ilişkilere güvenir](./media/concepts-forest-trust/forest-trusts.png)

Bu örnek yapılandırma aşağıdaki erişimi sağlar:

* Orman *2'deki* *kullanıcılar, Orman 1* veya *Orman 3'teki* herhangi bir etki alanındaki kaynaklara erişebilir
* Orman *3'teki* *kullanıcılar, Orman 2'deki* herhangi bir etki alanındakaynaklara erişebilir
* Orman *1'deki* *kullanıcılar, Orman 2'deki* herhangi bir etki alanındakaynaklara erişebilir

Bu yapılandırma, *Orman 1'deki* kullanıcıların *Orman 3'teki* kaynaklara erişmesine veya bunun tersi ne kadar önemli değildir. *Hem Orman 1* hem de Orman *3'teki* kullanıcıların kaynakları paylaşmasına izin vermek için, iki orman arasında iki yönlü geçişli güven oluşturulması gerekir.

İki orman arasında tek yönlü bir orman güveni oluşturulursa, güvenilen ormanın üyeleri güven ormanında bulunan kaynakları kullanabilir. Ancak, güven yalnızca bir yönde çalışır.

Örneğin, tek yönlü bir orman güveni *Orman 1* (güvenilen orman) ve Orman *2* (güvenilen orman) arasında oluşturulur:

* Orman *1* üyeleri *Orman 2'de*bulunan kaynaklara erişebilir.
* Forest *2* üyeleri, aynı güveni kullanarak *Orman 1'de* bulunan kaynaklara erişemez.

> [!IMPORTANT]
> Azure AD Etki Alanı Hizmetleri kaynak ormanı, şirket içi Active Directory'ye yalnızca tek yönlü orman güvenini destekler.

### <a name="forest-trust-requirements"></a>Orman güven gereksinimleri

Bir orman güveni oluşturmadan önce, doğru Etki Alanı Adı Sistemi (DNS) altyapısına sahip olduğunuzu doğrulamanız gerekir. Orman güvenleri yalnızca aşağıdaki DNS yapılandırmalarından biri kullanılabilir olduğunda oluşturulabilir:

* Tek kök DNS sunucusu her iki orman DNS ad alanları için kök DNS sunucusudur - kök bölge DNS ad alanlarının her biri için delegasyonlar içerir ve tüm DNS sunucularının kök ipuçları kök DNS sunucusuiçerir.
* Paylaşılan kök DNS sunucusunun olmadığı ve her orman DNS ad alanının kök DNS sunucularının bulunduğu yerlerde, diğer ad alanındaki adların sorgularını yönlendirmek için her DNS ad alanı için DNS koşullu iletmeler kullanılır.

    > [!IMPORTANT]
    > Azure AD Etki Alanı Hizmetleri kaynak ormanı bu DNS yapılandırması kullanmalıdır. Kaynak ormanı DNS ad alanı dışında bir DNS ad alanı barındırma, Azure AD Etki Alanı Hizmetlerinin bir özelliği değildir. Koşullu iletmeler uygun yapılandırmadır.

* Paylaşılan kök DNS sunucusunun olmadığı ve her orman DNS ad alanının kök DNS sunucularının kullanıldığı yerlerde DNS ikincil bölgeleri, diğer ad alanındaki adların sorgularını yönlendirmek için her DNS ad alanında yapılandırılır.

Orman güveni oluşturmak için Etki Alanı Yöneticileri grubunun (orman kökü etki alanında) veya Active Directory'deki Kurumsal Yöneticiler grubunun bir üyesi olmalısınız. Her güvene, her iki ormandaki yöneticilerin bilmesi gereken bir parola atanır. Her iki ormandaki Kurumsal Yöneticilerin üyeleri aynı anda her iki ormanda da güven oluşturabilir ve bu senaryoda, şifreleme olarak rasgele olan bir parola otomatik olarak oluşturulur ve her iki orman için de yazılır.

Azure AD Etki Alanı Hizmetleri için giden orman güveni Azure portalında oluşturulur. Yönetilen etki alanının kendisiyle güveni el ile oluşturamazsınız. Gelen orman güveni, şirket içi Active Directory'de daha önce belirtilen ayrıcalıklara sahip bir kullanıcı tarafından yapılandırılmalıdır.

## <a name="trust-processes-and-interactions"></a>Güven süreçleri ve etkileşimleri

Birçok etki alanı ve ormanlar arası işlemler, çeşitli görevleri tamamlamak için etki alanı veya orman güvenlerine bağlıdır. Bu bölümde, güvenler arasında kaynaklara erişilirken ve kimlik doğrulama başvuruları değerlendirilirken oluşan süreçler ve etkileşimler açıklanmaktadır.

### <a name="overview-of-authentication-referral-processing"></a>Kimlik doğrulama sevk işleme genel bakış

Kimlik doğrulama isteği bir etki alanına başlandığında, söz konusu etki alanında etki alanı denetleyicisinin isteğin geldiği etki alanıyla bir güven ilişkisi nin bulunup bulunmadığını belirlemesi gerekir. Güvenin yönü ve güvenin geçişli veya geçişsiz olup olmadığı da kullanıcının etki alanındakaynaklara erişmesi için kimlik doğrulamadan önce belirlenmelidir. Güvenilen etki alanları arasında oluşan kimlik doğrulama işlemi, kullanılan kimlik doğrulama protokolüne göre değişir. Kerberos V5 ve NTLM protokolleri, kimlik doğrulaması için başvuruları farklı bir etki alanına işleme

### <a name="kerberos-v5-referral-processing"></a>Kerberos V5 tavsiye işleme

Kerberos V5 kimlik doğrulama protokolü, istemci kimlik doğrulaması ve yetkilendirme bilgileri için etki alanı denetleyicilerinde Net Logon hizmetine bağlıdır. Kerberos protokolü, oturum biletleri için çevrimiçi Anahtar Dağıtım Merkezi'ne (KDC) ve Active Directory hesap mağazasına bağlanır.

Kerberos protokolü ayrıca, adi bilet verme hizmetleri (TGS) ve güvenli bir kanal da Ayrıcalık Öznitelik Sertifikaları (PC) doğrulamak için güvenler kullanır. Kerberos protokolü, yalnızca MIT Kerberos alemi gibi Windows markası olmayan işletim sistemi Kerberos alemleri ile alemler arası kimlik doğrulama gerçekleştirir ve Net Logon hizmeti ile etkileşime geçmesi gerekmez.

İstemci kimlik doğrulaması için Kerberos V5 kullanıyorsa, hesap etki alanında bir etki alanı denetleyicisinden hedef etki alanında sunucuya bir bilet ister. Kerberos KDC istemci ve sunucu arasında güvenilir bir aracı olarak hareket eder ve iki tarafın birbirini doğrulamasını sağlayan bir oturum anahtarı sağlar. Hedef etki alanı geçerli etki alanından farklıysa, KDC kimlik doğrulama isteğinin sevk edilip edilmeyeceğini belirlemek için mantıklı bir işlem izler:

1. Geçerli etki alanı doğrudan istenen sunucunun etki alanı tarafından güvenilir mi?
    * Evet se, istemciye istenen etki alanına bir havale gönderin.
    * Hayır ise, bir sonraki adıma geçin.

2. Geçerli etki alanı ile güven yolundaki sonraki etki alanı arasında geçişli bir güven ilişkisi var mı?
    * Evet se, istemciye güven yolundaki bir sonraki etki alanına bir havale gönderin.
    * Hayır ise, istemciye oturum açma reddedilen bir ileti gönderin.

### <a name="ntlm-referral-processing"></a>NTLM sevk işleme

NTLM kimlik doğrulama protokolü, istemci kimlik doğrulaması ve yetkilendirme bilgileri için etki alanı denetleyicilerinde net oturum açma hizmetine bağlıdır. Bu protokol, Kerberos kimlik doğrulaması kullanmayan istemcilerin kimliğini doğrular. NTLM, etki alanları arasında kimlik doğrulama isteklerini geçirmek için güvenleri kullanır.

İstemci kimlik doğrulaması için NTLM kullanıyorsa, kimlik doğrulama için ilk istek doğrudan istemciden hedef etki alanında kaynak sunucusuna gider. Bu sunucu, istemcinin yanıtladığı bir sorun oluşturur. Sunucu daha sonra kullanıcının yanıtını bilgisayar hesabı etki alanında bir etki alanı denetleyicisine gönderir. Bu etki alanı denetleyicisi, kullanıcı hesabını güvenlik hesapları veritabanıyla karşıla karşılar.

Hesap veritabanında yoksa, etki alanı denetleyicisi geçiş kimlik doğrulaması gerçekleştirmek, isteği iletmek veya aşağıdaki mantığı kullanarak isteği reddetmek için belirler:

1. Geçerli etki alanının kullanıcının etki alanıyla doğrudan güven ilişkisi var mı?
    * Evet ise, etki alanı denetleyicisi geçiş kimlik doğrulaması için kullanıcının etki alanında bir etki alanı denetleyicisine istemcinin kimlik bilgilerini gönderir.
    * Hayır ise, bir sonraki adıma geçin.

2. Geçerli etki alanının kullanıcının etki alanıyla geçişli bir güven ilişkisi var mı?
    * Evet ise, kimlik doğrulama isteğini güven yolundaki bir sonraki etki alanına geçirin. Bu etki alanı denetleyicisi, kullanıcının kimlik bilgilerini kendi güvenlik hesapları veritabanıyla karşılaştırarak işlemi yineler.
    * Hayır ise, istemciye oturum açma reddedilen bir ileti gönderin.

### <a name="kerberos-based-processing-of-authentication-requests-over-forest-trusts"></a>Orman güvenleri üzerinden kimlik doğrulama isteklerinin Kerberos tabanlı işleme

İki orman bir orman güveniyle bağlandığında, Kerberos V5 veya NTLM protokolleri kullanılarak yapılan kimlik doğrulama istekleri, her iki ormandaki kaynaklara erişim sağlamak için ormanlar arasında yönlendirilebilir.

Bir orman güveni ilk kurulduğunda, her orman iş ortağı ormanındaki tüm güvenilen ad alanlarını toplar ve bilgileri güvenilir bir [etki alanı nesnesinde](#trusted-domain-object)depolar. Güvenilen ad alanları, etki alanı ağacı adları, kullanıcı adı (UPN) sonekleri, hizmet anaadı (SPN) soneklerini ve diğer ormanda kullanılan güvenlik kimliği (SID) ad alanlarını içerir. TDO nesneleri genel katalogda çoğaltılır.

Kimlik doğrulama protokolleri orman güven yolunu izleyebilmek için kaynak bilgisayarın hizmet ana adı (SPN) diğer ormandaki bir konuma çözülmelidir. Bir SPN aşağıdakilerden biri olabilir:

* Bir ana bilgisayar DNS adı.
* Etki alanının DNS adı.
* Hizmet bağlantı noktası nesnesinin ayırt edici adı.

Bir ormandaki bir iş istasyonu başka bir ormandaki bir kaynak bilgisayardaki verilere erişmeye çalıştığında, Kerberos kimlik doğrulama işlemi kaynak bilgisayarın SPN'sine bir hizmet bileti için etki alanı denetleyicisi ile bağlantı yatarım. Etki alanı denetleyicisi genel kataloğu sorgulayıp SPN'nin etki alanı denetleyicisi ile aynı ormanda olmadığını belirledikten sonra, etki alanı denetleyicisi ana etki alanı için iş istasyonuna bir başvuru gönderir. Bu noktada, iş istasyonu servis bileti için ana etki alanını sorgular ve kaynağın bulunduğu etki alanına ulaşana kadar başvuru zincirini izlemeye devam eder.

Aşağıdaki diyagram ve adımlar, Windows çalıştıran bilgisayarlar başka bir ormanda bulunan bir bilgisayardan kaynaklara erişmeye çalışırken kullanılan Kerberos kimlik doğrulama işleminin ayrıntılı bir açıklamasını sağlar.

![Bir orman güven üzerinde Kerberos sürecinin Diyagramı](media/concepts-forest-trust/kerberos-over-forest-trust-process.png)

1. *User1,* *europe.tailspintoys.com* etki alanından kimlik bilgilerini kullanarak *Workstation1'de* oturum açar. Kullanıcı daha sonra *usa.wingtiptoys.com* ormanında bulunan *FileServer1'de* paylaşılan bir kaynağa erişmeye çalışır.

2. *Workstation1,* Kendi etki alanı, *ChildDC1*bir etki alanı denetleyicisi üzerinde Kerberos KDC başvurur ve *FileServer1* SPN için bir servis bileti ister.

3. *ChildDC1,* etki alanı veritabanında SPN'yi bulamaz ve *tailspintoys.com* ormanındaki etki alanlarının bu SPN'yi içerip içermediğini görmek için genel kataloğu sorgular. Küresel katalog kendi ormanıyla sınırlı olduğundan, SPN bulunamadı.

    Genel katalog daha sonra, ormanıyla birlikte kurulan orman güvenleri hakkında bilgi için veritabanını denetler. Bulunursa, eşleşebilir bulmak için orman güvenilen etki alanı nesnesi (TDO) ad sonekleri ile hedef SPN'nin soneklerini karşılaştırır. Bir eşleşme bulunduğunda, genel katalog *ChildDC1'e*yönlendirme ipucu sağlar.

    Yönlendirme ipuçları, doğrudan kimlik doğrulama isteklerini hedef ormana yönlendirmeye yardımcı olur. İpuçları yalnızca yerel etki alanı denetleyicisi ve ardından genel katalog gibi tüm geleneksel kimlik doğrulama kanalları bir SPN'yi bulamadığında kullanılır.

4. *ChildDC1,* ana etki alanı için *Workstation1'e*bir başvuru gönderir.

5. *Workstation1,* *wingtiptoys.com* ormanının orman kökü etki alanında bir etki alanı denetleyicisine *(ForestRootDC2)* sevk etmek için *ForestRootDC1'de* (ana etki alanı) bir etki alanı denetleyicisi ile bağlantı kurmaktadır.

6. *Workstation1,* istenen servise servis bileti almak için *wingtiptoys.com* ormanındaki *ForestRootDC2* ile iletişime geyleer.

7. *ForestRootDC2,* SPN'yi bulmak için küresel kataloğuyla bağlantı kurur ve genel katalog SPN için bir eşleşme bulur ve *onu ForestRootDC2'ye*geri gönderir.

8. *ForestRootDC2* daha sonra *usa.wingtiptoys.com'a* geri *Workstation1'e*yönlendirme gönderir.

9. *Workstation1* *ChildDC2'de* KDC ile bağlantı kurun ve *User1'in* *FileServer1'e*erişim elde etmesi için bileti görüşür.

10. *Workstation1'de* servis bileti olduğunda, servis biletini *User1'in*güvenlik kimlik bilgilerini okuyan ve buna göre bir erişim belirteci oluşturan *FileServer1'e*gönderir.

## <a name="trusted-domain-object"></a>Güvenilen etki alanı nesnesi

Kuruluştaki her etki alanı veya orman güveni, etki alanı içinde *Sistem* kapsayıcısında depolanan güvenilir bir Etki Alanı Nesnesi (TDO) tarafından temsil edilir.

### <a name="tdo-contents"></a>TDO içeriği

Bir TDO'da yer alan bilgiler, bir TDO'nun bir etki alanı güveni veya orman güveni tarafından oluşturulup oluşturulmadığına bağlı olarak değişir.

Bir etki alanı güveni oluşturulduğunda, DNS etki alanı adı, etki alanı SID, güven türü, güven geçişi ve karşılıklı etki alanı adı gibi öznitelikler TDO'da temsil edilir. Orman güven TDO'ları, ortak ormandaki tüm güvenilen ad alanlarını tanımlamak için ek öznitelikler depolar. Bu öznitelikler etki alanı ağacı adlarını, kullanıcı anaadı (UPN) soneklerini, hizmet anaadı (SPN) soneklerini ve güvenlik kimliği (SID) ad alanlarını içerir.

Güvenler Etkin Dizin'de TDO olarak depolandığı için, ormandaki tüm etki alanları orman boyunca bulunan güven ilişkileri hakkında bilgiye sahiptir. Benzer şekilde, orman güvenleri aracılığıyla iki veya daha fazla orman birleştiğinde, her ormandaki orman kökü etki alanları, güvenilen ormanlarda bulunan tüm etki alanlarında yürürlükte olan güven ilişkileri hakkında bilgi sahibidir.

### <a name="tdo-password-changes"></a>TDO şifre değişiklikleri

Güven ilişkisindeki her iki etki alanı da, Etkin Dizin'deki TDO nesnesinde depolanan bir parolayı paylaşır. Hesap bakım sürecinin bir parçası olarak, güvenen etki alanı denetleyicisi Her 30 günde bir TDO'da depolanan parolayı değiştirir. Tüm iki yönlü güvenler aslında zıt yönlere giden iki tek yönlü güven olduğundan, işlem iki yönlü güvenler için iki kez gerçekleşir.

Bir güvenin güvenilir ve güvenilir bir tarafı vardır. Güvenilir tarafta, herhangi bir yazılabilir etki alanı denetleyicisi işlem için kullanılabilir. Güvenen tarafta, PDC emülatörü parola değişikliğini gerçekleştirir.

Parolayı değiştirmek için etki alanı denetleyicileri aşağıdaki işlemi tamamlar:

1. Güvenilen etki alanında birincil etki alanı denetleyicisi (PDC) emülatörü yeni bir parola oluşturur. Güvenilen etki alanında bir etki alanı denetleyicisi parola değişikliğini asla başlatmaz. Her zaman güvenen etki alanı PDC emülatörü tarafından başlatılır.

2. Güvenilen etki alanındaki PDC emülatörü, TDO nesnesinin *OldPassword* alanını geçerli *NewPassword* alanına ayarlar.

3. Güvenilen etki alanındaki PDC emülatörü, TDO nesnesinin *NewPassword* alanını yeni parolaya ayarlar. Önceki parolanın bir kopyasının tutulması, güvenilen etki alanında etki alanı denetleyicisi değişikliği almazsa veya değişiklik yeni güven parolasını kullanan bir istek edilmeden önce çoğaltılmazsa eski parolaya geri dönmek mümkün olur.

4. Güvenilen etki alanında ki PDC emülatörü, güven hesabındaki parolayı yeni parolaya ayarlamasını isteyen güvenilir etki alanındaki bir etki alanı denetleyicisine uzaktan arama yapar.

5. Güvenilen etki alanında etki alanı denetleyicisi güven parolasını yeni parolayla değiştirir.

6. Güvenin her iki tarafında, güncelleştirmeler etki alanında diğer etki alanı denetleyicilerine çoğaltılır. Güvenilen etki alanında, değişiklik güvenilen etki alanı nesnesinin acil bir çoğaltmasını tetikler.

Parola artık her iki etki alanı denetleyicisi üzerinde değiştirilir. Normal çoğaltma, TDO nesnelerini etki alanındaki diğer etki alanı denetleyicilerine dağıtır. Ancak, güvenilen etki alanında etki alanı denetleyicisinin, güvenilen etki alanında bir etki alanı denetleyicisini başarıyla güncelleştirmeden parolayı değiştirmesi mümkündür. Parola değişikliğini işlemek için gereken güvenli bir kanal oluşturulamadığından bu senaryo oluşabilir. Ayrıca, güvenilen etki alanında etki alanı denetleyicisi işlem sırasında bir noktada kullanılamaz ve güncelleştirilmiş parolayı alamayabilir de mümkündür.

Parola değişikliğinin başarıyla iletilemediği durumlarla başa çıkmak için, güvenalan etki alanındaki etki alanı denetleyicisi, yeni parolayı kullanarak başarılı bir şekilde kimlik doğrulaması yapmadıkça (güvenli bir kanal kurma) yeni parolayı asla değiştirmez. Bu davranış, hem eski hem de yeni parolaların güvenalan etki alanının TDO nesnesinde tutulmasının nedenidir.

Parolayı kullanarak kimlik doğrulaması başarılı olana kadar parola değişikliği tamamlanmaz. Eski, depolanan parola, güvenilen etki alanında etki alanı denetleyicisi yeni parolayı alana kadar güvenli kanal üzerinden kullanılabilir ve böylece kesintisiz hizmet sağlar.

Parola geçersiz olduğu için yeni parolayı kullanarak kimlik doğrulaması başarısız olursa, güvenilen etki alanı denetleyicisi eski parolayı kullanarak kimlik doğrulaması yapmaya çalışır. Eski parolayla başarılı bir şekilde kimlik doğrulaması yaparsa, parola değiştirme işlemini 15 dakika içinde devam ettirer.

Güven parola güncelleştirmelerinin 30 gün içinde güvenin her iki tarafının etki alanı denetleyicilerine çoğaltılması gerekir. Güven parolası 30 gün sonra değiştirilirse ve etki alanı denetleyicisi yalnızca N-2 parolasına sahipse, güven tarafında güven kullanamaz ve güvenilen tarafta güvenli bir kanal oluşturamaz.

## <a name="network-ports-used-by-trusts"></a>Güvenler tarafından kullanılan ağ bağlantı noktaları

Güvenlerin çeşitli ağ sınırları içinde dağıtılması gerektiğinden, bir veya daha fazla güvenlik duvarLarını aşması gerekebilir. Bu durumda, trafiğin geçmesine izin vermek için güvenlik duvarındaki güven trafiğini tünelleyebilir veya güvenlik duvarındaki belirli bağlantı noktalarını açabilirsiniz.

> [!IMPORTANT]
> Etkin Dizin Etki Alanı Hizmetleri, Active Directory RPC trafiğinin belirli bağlantı noktalarıyla sınırlandırMasını desteklemez.

Windows **Server 2008 ve daha sonraki sürümleri** Microsoft Destek Makalesi bölümünü okuyun Nasıl Etkin [Dizin etki alanları ve güvenler için bir güvenlik duvarı yapılandırmak](https://support.microsoft.com/help/179442/how-to-configure-a-firewall-for-domains-and-trusts) için bir orman güven için gerekli bağlantı noktaları hakkında bilgi edinmek için.

## <a name="supporting-services-and-tools"></a>Destek hizmetleri ve araçları

Güvenleri ve kimlik doğrulamayı desteklemek için bazı ek özellikler ve yönetim araçları kullanılır.

### <a name="net-logon"></a>Net Oturum Açma

Net Logon hizmeti, Windows tabanlı bir bilgisayardan DC'ye kadar güvenli bir kanalı tutar. Ayrıca, güvenle ilgili aşağıdaki işlemlerde de kullanılır:

* Güven kurulumu ve yönetimi - Net Logon, güven parolalarının korunmasına yardımcı olur, güven bilgilerini toplar ve LSA süreci ve TDO ile etkileşim kurarak güvenleri doğrular.

    Orman güvenleri için güven bilgileri, güvenilir bir ormanın yönetmek için iddia ettiği ad alanları kümesini içeren ve her talebin güvenilen orman tarafından güvenilip güvenilmediğini gösteren bir alanla ek açıklama içeren Orman Güven Bilgileri *(FTInfo)* kaydını içerir.

* Kimlik doğrulama - Güvenli bir kanal üzerinden kullanıcı kimlik bilgilerini bir etki alanı denetleyicisine sağlar ve kullanıcı için etki alanı SIT'lerini ve kullanıcı haklarını döndürür.

* Etki alanı denetleyicisi konumu – Etki alanı denetleyicilerini bir etki alanında veya etki alanlarında bulmada yardımcı olur.

* Geçiş doğrulama - Diğer etki alanlarındaki kullanıcıların kimlik bilgileri Net Logon tarafından işlenir. Güvenilen bir etki alanının bir kullanıcının kimliğini doğrulaması gerektiğinde, kullanıcının kimlik bilgilerini Net Logon aracılığıyla doğrulama için güvenilir etki alanına geçirir.

* Ayrıcalık Öznitelik Sertifikası (PAC) doğrulaması – Kimlik doğrulama için Kerberos protokolünü kullanan bir sunucunun PAC'i bir hizmet biletinde doğrulaması gerektiğinde, PAC'yi güvenli kanal üzerinden doğrulama için etki alanı denetleyicisine gönderir.

### <a name="local-security-authority"></a>Yerel Güvenlik Yetkilisi

Yerel Güvenlik Otoritesi (LSA), bir sistemdeki yerel güvenliğin tüm yönleri hakkında bilgi tutan korumalı bir alt sistemdir. Topluolarak yerel güvenlik politikası olarak bilinen LSA, adlar ve tanımlayıcılar arasında çeviri için çeşitli hizmetler sunmaktadır.

LSA güvenlik alt sistemi, nesnelere erişimi doğrulamak, kullanıcı ayrıcalıklarını denetlemek ve denetim iletileri oluşturmak için hem çekirdek modunda hem de kullanıcı modunda hizmetler sağlar. LSA, hizmetler tarafından güvenilen veya güvenilmeyen etki alanlarında sunulan tüm oturum biletlerinin geçerliliğini denetlemekten sorumludur.

### <a name="management-tools"></a>Yönetim araçları

Yöneticiler, güvenleri ortaya çıkarmak, oluşturmak, kaldırmak veya değiştirmek için *Active Directory Etki Alanları ve Güvenler,* *Netdom* ve *Nltest'i* kullanabilir.

* *Etkin Dizin Etki Alanları ve Güvenleri,* etki alanı güvenlerini, etki alanı ve orman işlevsel düzeylerini ve kullanıcı ana adı soneklerini yönetmek için kullanılan Microsoft Yönetim Konsolu'dur (MMC).
* *Netdom* ve *Nltest* komut satırı araçları, güvenleri bulmak, görüntülemek, oluşturmak ve yönetmek için kullanılabilir. Bu araçlar, etki alanı denetleyicisi üzerindeki LSA yetkilisiyle doğrudan iletişim kurar.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak ormanları hakkında daha fazla bilgi edinmek için [Azure AD DS'de orman güvenleri nasıl çalışır?][concepts-trust]

Kaynak ormanı olan bir Azure AD DS yönetilen etki alanı oluşturmaya başlamak için [bkz.][tutorial-create-advanced] Daha sonra [şirket içi etki alanına giden orman güveni (önizleme) oluşturabilirsiniz.][create-forest-trust]

<!-- LINKS - INTERNAL -->
[concepts-trust]: concepts-forest-trust.md
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-forest-trust]: tutorial-create-forest-trust.md
