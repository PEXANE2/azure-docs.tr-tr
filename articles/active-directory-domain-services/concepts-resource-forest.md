---
title: Azure AD Etki Alanı Hizmetleri için kaynak ormanı kavramları | Microsoft Dokümanlar
description: Azure Etkin Dizin Etki Alanı Hizmetlerinde kaynak ormanının ne olduğunu ve sınırlı kullanıcı kimlik doğrulama seçenekleri veya güvenlik sorunlarıyla karma ortamda kuruluşunuza nasıl fayda sağladığını öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: iainfou
ms.openlocfilehash: a583e32cbc3d58d5dfc5616335b2f38ad20fac14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74233616"
---
# <a name="resource-forest-concepts-and-features-for-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetleri için kaynak ormanı kavramları ve özellikleri

Azure Active Directory Domain Services (AD DS), eski, şirket içi, iş yeri uygulamaları için oturum açma deneyimi sağlar. Şirket içi ve bulut kullanıcılarının kullanıcıları, grupları ve parola grupları Azure AD DS yönetilen etki alanına eşitlenir. Bu eşitlenmiş parola yongaları, kullanıcılara şirket içi AD DS, Office 365 ve Azure Etkin Dizini için kullanabilecekleri tek bir kimlik bilgileri kümesi sağlar.

Güvenli ve ek güvenlik avantajları sağlamasına rağmen, bazı kuruluşlar bu kullanıcı parolalarını Azure AD veya Azure AD DS ile eşitleemez. Kuruluştaki kullanıcılar parolalarını bilmiyor olabilir, çünkü yalnızca akıllı kart kimlik doğrulaması kullanıyorlar. Bu sınırlamalar, bazı kuruluşların şirket içi klasik uygulamaları kaldırmak ve Azure'a kaydırmak için Azure AD DS'yi kullanmasını engeller.

Bu gereksinimleri ve kısıtlamaları gidermek için, kaynak ormanı kullanan bir Azure AD DS yönetilen etki alanı oluşturabilirsiniz. Bu kavramsal makalede, ormanların ne olduğu ve güvenli bir kimlik doğrulama yöntemi sağlamak için diğer kaynaklara nasıl güvendikleri açıklanmaktadır. Azure AD DS kaynak ormanları şu anda önizlemede.

> [!IMPORTANT]
> Azure AD DS kaynak ormanları şu anda Azure HDInsight veya Azure Dosyalarını desteklemiyor. Varsayılan Azure AD DS kullanıcı ormanları bu ek hizmetlerin her ikisini de destekler.

## <a name="what-are-forests"></a>Ormanlar nedir?

*Orman,* Active Directory Domain Services (AD DS) tarafından bir veya daha fazla *etki alanını*gruplandırmak için kullanılan mantıksal bir yapıdır. Etki alanları daha sonra nesneleri kullanıcı veya gruplar için depolar ve kimlik doğrulama hizmetleri sağlar.

Azure AD DS'de orman yalnızca bir etki alanı içerir. Şirket içi AD DS ormanları genellikle birçok etki alanı içerir. Büyük kuruluşlarda, özellikle birleşme ve satın almalardan sonra, her biri birden çok etki alanı içeren birden çok şirket içi ormanla karşılaşabilirsiniz.

Varsayılan olarak, bir Azure AD DS yönetilen etki alanı *kullanıcı* ormanı olarak oluşturulur. Bu tür orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD'deki tüm nesneleri eşitler. Kullanıcı hesapları, etki alanına katılan bir VM'de oturum açma gibi Azure AD DS yönetilen etki alanına karşı doğrudan kimlik doğrulaması yapabilir. Parola kalıpları eşitlenediğinde ve kullanıcılar akıllı kart kimlik doğrulaması gibi özel oturum açma yöntemleri kullanmadığında kullanıcı ormanı çalışır.

Azure AD DS *kaynak* ormanında, kullanıcılar şirket içi AD DS'lerinden tek yönlü orman *güveni* üzerinden kimlik doğrulaması sağlar. Bu yaklaşımla, kullanıcı nesneleri ve parola kalıpları Azure AD DS ile eşitlenmez. Kullanıcı nesneleri ve kimlik bilgileri yalnızca şirket içi AD DS'de bulunur. Bu yaklaşım, işletmelerin Azure'da LDAPS, Kerberos veya NTLM gibi klasik kimlik doğrulamasına bağlı kaynakları ve uygulama platformlarını barındırmasına olanak tanır, ancak kimlik doğrulama sorunları veya endişeleri kaldırılır. Azure AD DS kaynak ormanları şu anda önizlemede.

Kaynak ormanları, uygulamalarınızı her seferinde bir bileşenden kaldırma ve kaydırma olanağı da sağlar. Birçok eski şirket içi uygulamalar, genellikle bir web sunucusu veya ön uç ve veritabanı ile ilgili birçok bileşen kullanılarak çok katmanlıdır. Bu katmanlar, tüm uygulamayı tek bir adımda buluta kaldırmayı ve kaydırmayı zorlar. Kaynak ormanları ile uygulamanızı aşamalı bir yaklaşımla buluta taşıyabilirsiniz, bu da uygulamanızı Azure'a taşımanızı kolaylaştırır.

## <a name="what-are-trusts"></a>Güven nedir?

Birden fazla etki alanına sahip kuruluşlar genellikle kullanıcıların farklı bir etki alanında paylaşılan kaynaklara erişmelerine ihtiyaç duyar. Bu paylaşılan kaynaklara erişim, bir etki alanındakullanıcıların başka bir etki alanına kimlik doğrulamasını gerektirir. Farklı etki alanlarındaki istemciler ve sunucular arasında bu kimlik doğrulama ve yetkilendirme özelliklerini sağlamak için, iki etki alanı arasında bir *güven* olması gerekir.

Etki alanı güvenleri ile, her etki alanı için kimlik doğrulama mekanizmaları diğer etki alanından gelen kimlik doğrulamaları güven. Güvenler, gelen kimlik doğrulama isteklerinin güvenilir bir yetkiliden *(güvenilir* etki alanından) geldiğini doğrulayarak kaynak etki alanında *(güvenilen* etki alanı) paylaşılan kaynaklara kontrollü erişim sağlanmasına yardımcı olur. Güvenler, yalnızca doğrulanmış kimlik doğrulama isteklerinin etki alanları arasında seyahat etmesine izin veren köprüler görevi görede dir.

Bir güvenin kimlik doğrulama isteklerini nasıl geçtiği, nasıl yapılandırıldığına bağlıdır. Güvenler aşağıdaki yollardan biriyle yapılandırılabilir:

* **Tek yönlü** - güvenilen etki alanından güvenilen etki alanından kaynaklara erişim sağlar.
* **İki yönlü** - her etki alanından diğer etki alanında kaynaklara erişim sağlar.

Güvenler, ek güven ilişkilerini aşağıdaki yollardan birinde işlenecek şekilde de yapılandırılır:

* **Geçişsiz** - Güven yalnızca iki güven ortağı etki alanı arasında bulunur.
* **Geçişli** - Güven, iş ortaklarından herhangi birinin güvendiği diğer etki alanlarını otomatik olarak genişletir.

Bazı durumlarda, etki alanları oluşturulduğunda güven ilişkileri otomatik olarak kurulur. Diğer zamanlarda, bir güven türünü seçmeniz ve uygun ilişkileri açıkça kurmanız gerekir. Kullanılan belirli güven türleri ve bu güven ilişkilerinin yapısı, Etkin Dizin dizini hizmetinin nasıl düzenlendiğine ve Windows'un farklı sürümlerinin ağda bir arada bulunup bulunmadığına bağlıdır.

## <a name="trusts-between-two-forests"></a>İki orman arasındaki güvenler

Tek yönlü veya iki yönlü orman güveni oluşturarak, tek bir ormandaki etki alanı güvenlerini başka bir ormana genişletebilirsiniz. Orman güveni, yalnızca bir orman kökü etki alanı ile ikinci bir orman kökü etki alanı arasında bulunan geçişli bir güvendir.

* Tek yönlü orman güveni, bir ormandaki tüm kullanıcıların diğer ormandaki tüm etki alanlarında güvenmesine olanak tanır.
* İki yönlü orman güveni, her iki ormandaki her etki alanı arasında geçişli bir güven ilişkisi oluşturur.

Orman tröstlerinin geçişleri iki orman ortağıyla sınırlıdır. Orman güveni, ortakların her ikisinin de güvendiği ek ormanları kapsamaz.

![Azure AD DS'den şirket içi AD DS'ye orman güveni diyagramı](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

Kuruluşun Etkin Dizin yapısına bağlı olarak farklı etki alanı ve orman güven yapılandırmaları oluşturabilirsiniz. Azure AD DS yalnızca tek yönlü orman güvenini destekler. Bu yapılandırmada, Azure AD DS'deki kaynaklar şirket içi bir ormandaki tüm etki alanlarına güvenebilir.

## <a name="supporting-technology-for-trusts"></a>Güvenler için teknolojiyi destekleme

Güvenler, ortak ormanlarda etki alanı denetleyicilerini bulmak için DNS gibi çeşitli hizmetler ve özellikler kullanır. Güvenler ayrıca NTLM ve Kerberos kimlik doğrulama protokollerine ve Etkin Dizin etki alanları ve ormanlar arasında güvenli bir iletişim altyapısı sağlamaya yardımcı olmak için Windows tabanlı yetkilendirme ve erişim denetim mekanizmalarına da bağlıdır. Aşağıdaki hizmetler ve özellikler başarılı güven ilişkilerini desteklemeye yardımcı olur.

### <a name="dns"></a>DNS

AD DS etki alanı denetleyicisi (DC) konumu ve adlandırma için DNS gerekir. AD DS'nin başarılı bir şekilde çalışması için DNS'den aşağıdaki destek sağlanır:

* Ağ ana bilgisayarlarının ve hizmetlerin DC'leri bulmasını sağlayan bir ad çözümleme hizmeti.
* Bir işletmenin kuruluş yapısını dizin hizmeti etki alanlarının adlarına yansıtmasını sağlayan bir adlandırma yapısı.

Ad DS etki alanı ad alanını yansıtan bir DNS etki alanı genellikle dağıtılır. AD DS dağıtımından önce varolan bir DNS ad alanı varsa, DNS ad alanı genellikle Active Directory için bölümlere ayrılır ve Active Directory orman kökü için bir DNS alt etki alanı ve delegasyonu oluşturulur. Daha sonra her Active Directory alt etki alanı için ek DNS etki alanı adları eklenir.

DNS, Active Directory DCs'nin konumunu desteklemek için de kullanılır. DNS bölgeleri, ağ ana bilgisayarlarının ve hizmetlerinin Active Directory DC'leri bulmasını sağlayan DNS kaynak kayıtlarıyla doldurulur.

### <a name="applications-and-net-logon"></a>Uygulamalar ve Net Oturum Açma

Hem uygulamalar hem de Net Logon hizmeti, Windows dağıtılmış güvenlik kanalı modelinin bileşenleridir. Windows Server ve Active Directory ile tümleşik uygulamalar, kimlik doğrulamanın gerçekleşebileceği güvenli bir yol kurulabilmesi için Net Logon hizmetiyle iletişim kurmak için kimlik doğrulama protokollerini kullanır.

### <a name="authentication-protocols"></a>Kimlik Doğrulama Protokolleri

Etkin Dizin DC'leri, aşağıdaki protokollerden birini kullanarak kullanıcıların ve uygulamaların kimliğini doğrular:

* **Kerberos sürüm 5 kimlik doğrulama protokolü**
    * Kerberos sürüm 5 protokolü, Windows çalıştıran ve üçüncü taraf işletim sistemlerini destekleyen şirket içi bilgisayarlar tarafından kullanılan varsayılan kimlik doğrulama protokolüdür. Bu protokol RFC 1510'da belirtilir ve Active Directory, sunucu ileti bloğu (SMB), HTTP ve uzak yordam çağrısı (RPC) ile bu protokolleri kullanan istemci ve sunucu uygulamalarıyla tam olarak entegre edilmiştir.
    * Kerberos protokolü kullanıldığında, sunucunun DC ile iletişim kurması gerekmez. Bunun yerine, istemci sunucu hesabı etki alanında bir DC bir istekte bulunarak bir sunucu için bir bilet alır. Sunucu daha sonra başka bir yetkiliye danışmadan bileti doğrular.
    * Bir işlemle ilgili herhangi bir bilgisayar Kerberos sürüm 5 protokolünü desteklemiyorsa, NTLM protokolü kullanılır.

* **NTLM kimlik doğrulama protokolü**
    * NTLM protokolü, eski işletim sistemleri tarafından kullanılan klasik bir ağ kimlik doğrulama protokolüdür. Uyumluluk nedenleriyle, önceki Windows tabanlı istemciler ve sunucular ve üçüncü taraf işletim sistemleri için tasarlanmış uygulamalardan gelen ağ kimlik doğrulama isteklerini işlemek için Active Directory etki alanları tarafından kullanılır.
    * NTLM protokolü bir istemci ve sunucu arasında kullanıldığında, sunucu istemci kimlik bilgilerini doğrulamak için DC'deki bir etki alanı kimlik doğrulama hizmetine başvurmalıdır. Sunucu, istemci kimlik bilgilerini istemci hesabı etki alanında bir DC'ye ileterek istemcinin kimliğini doğrular.
    * İki Active Directory etki alanı veya orman bir güven tarafından bağlandığında, bu protokoller kullanılarak yapılan kimlik doğrulama istekleri her iki ormandaki kaynaklara erişim sağlamak için yönlendirilebilir.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim kontrolü

Yetkilendirme ve güven teknolojileri, Active Directory etki alanları veya ormanlar arasında güvenli bir iletişim altyapısı sağlamak için birlikte çalışır. Yetkilendirme, bir kullanıcının etki alanında kaynaklara hangi erişim düzeyine sahip olduğunu belirler. Güvenler, kullanıcıların diğer etki alanlarındaki kimlik doğrulamalarını doğrulamak için bir yol sağlayarak kullanıcıların etki alanları arası yetkilendirmesini kolaylaştırır, böylece bu etki alanlarındaki paylaşılan kaynaklara yönelik istekleri yetkilendirilebilir.

Güvenilen bir etki alanında yapılan kimlik doğrulama isteği güvenilen etki alanı tarafından doğrulandığında, hedef kaynağa aktarılır. Hedef kaynak daha sonra, kullanıcı, hizmet veya bilgisayarın güvenilen etki alanında yaptığı belirli isteği erişim denetimi yapılandırmasına göre yetkilendirilip yetkilendirmeyeceğini belirler.

Güvenler, güvenilen bir etki alanına geçirilen kimlik doğrulama isteklerini doğrulamak için bu mekanizmayı sağlar. Kaynak bilgisayarındaki erişim denetim mekanizmaları, güvenilen etki alanında istekte bulunana verilen son erişim düzeyini belirler.

## <a name="next-steps"></a>Sonraki adımlar

Güvenler hakkında daha fazla bilgi edinmek için [Azure AD DS'de orman güvenleri nasıl çalışır?][concepts-trust]

Kaynak ormanı olan bir Azure AD DS yönetilen etki alanı oluşturmaya başlamak için [bkz.][tutorial-create-advanced] Daha sonra [şirket içi etki alanına giden orman güveni (önizleme) oluşturabilirsiniz.][create-forest-trust]

<!-- LINKS - INTERNAL -->
[concepts-trust]: concepts-forest-trust.md
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-forest-trust]: tutorial-create-forest-trust.md
