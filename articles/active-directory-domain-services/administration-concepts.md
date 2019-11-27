---
title: Azure AD Domain Services için yönetim kavramları | Microsoft Docs
description: Azure Active Directory Domain Services yönetilen bir etki alanını ve Kullanıcı hesaplarının ve parolaların davranışını yönetme hakkında bilgi edinin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: iainfou
ms.openlocfilehash: f239bab48e732755361fe734fdc24b37d3823c63
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481015"
---
# <a name="management-concepts-for-user-accounts-passwords-and-administration-in-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services 'de Kullanıcı hesapları, parolalar ve yönetim için yönetim kavramları

Azure Active Directory Domain Services (AD DS) yönetilen bir etki alanı oluşturup çalıştırdığınızda, bazı durumlarda geleneksel bir şirket içi AD DS ortamıyla karşılaştırıldığında bazı farklılıklar vardır. Azure AD DS 'de otomatik olarak yönetilen bir etki alanı olarak aynı yönetim araçlarını kullanırsınız, ancak etki alanı denetleyicilerine (DC) doğrudan erişemezsiniz. Ayrıca, Kullanıcı hesabı oluşturma kaynağına bağlı olarak parola ilkeleri ve parola karmalarının davranışında bazı farklılıklar da vardır.

Bu kavramsal makalede, bir Azure AD DS yönetilen etki alanını ve bunların oluşturulma şekline bağlı olarak Kullanıcı hesaplarının farklı davranışlarını yönetme hakkında bilgi yer alır.

## <a name="domain-management"></a>Etki alanı yönetimi

Azure AD DS 'de, kullanıcılar ve gruplar, kimlik bilgileri ve ilkeler gibi tüm kaynakları içeren etki alanı denetleyicileri (DC 'Ler), yönetilen hizmetin bir parçasıdır. Yedeklilik için, Azure AD DS yönetilen bir etki alanının parçası olarak iki DC oluşturulur. Yönetim görevlerini gerçekleştirmek için bu DC 'lerde oturum açamazsınız. Bunun yerine, Azure AD DS yönetilen etki alanına katılmış bir yönetim sanal makinesi oluşturun ve ardından düzenli AD DS yönetim araçlarınızı yükleyebilirsiniz. Örneğin, DNS veya grup ilkesi nesneleri gibi Active Directory Yönetim Merkezi veya Microsoft Yönetim Konsolu (MMC) ek bileşenlerini kullanabilirsiniz.

## <a name="user-account-creation"></a>Kullanıcı hesabı oluşturma

Kullanıcı hesapları, Azure AD DS 'de birden çok şekilde oluşturulabilir. Çoğu kullanıcı hesabı Azure AD 'den eşitlenir ve bu da şirket içi AD DS ortamından eşitlenen Kullanıcı hesabını da içerebilir. Ayrıca, hesapları doğrudan Azure AD DS 'da da el ile oluşturabilirsiniz. İlk parola eşitleme veya parola ilkesi gibi bazı özellikler, Kullanıcı hesaplarının nasıl ve nerede oluşturulduğuna bağlı olarak farklı davranır.

* Kullanıcı hesabı Azure AD 'den eşitlenebilir. Bu, doğrudan Azure AD 'de oluşturulan bulut Kullanıcı hesaplarını ve Azure AD Connect kullanarak şirket içi AD DS ortamından eşitlenen karma Kullanıcı hesaplarını içerir.
    * Azure AD DS 'daki Kullanıcı hesaplarının çoğu Azure AD 'den eşitleme işlemi aracılığıyla oluşturulur.
* Kullanıcı hesabı Azure AD DS yönetilen bir etki alanında el ile oluşturulabilir ve Azure AD 'de mevcut değildir.
    * Yalnızca Azure AD DS çalışan uygulamalar için hizmet hesapları oluşturmanız gerekiyorsa, bunları yönetilen etki alanında el ile oluşturabilirsiniz. Eşitleme, Azure AD 'den tek yönlü olduğundan Azure AD DS 'de oluşturulan kullanıcı hesapları Azure AD 'ye geri aktarılmaz.

## <a name="password-policy"></a>Parola ilkesi

Azure AD DS, hesap kilitleme, en fazla parola yaşı ve parola karmaşıklığı gibi şeyler için ayarları tanımlayan bir varsayılan parola ilkesi içerir. Hesap kilitleme ilkesi gibi ayarlar, kullanıcının önceki bölümde özetlendiği şekilde nasıl oluşturulduğuna bakılmaksızın Azure AD DS 'deki tüm kullanıcılara uygulanır. Minimum parola uzunluğu ve parola karmaşıklığı gibi bazı ayarlar, yalnızca doğrudan Azure AD DS oluşturulan kullanıcılara uygulanır.

Azure AD DS 'de varsayılan ilkeyi geçersiz kılmak için kendi özel parola ilkelerinizi oluşturabilirsiniz. Bu özel ilkeler, gerektiğinde belirli kullanıcı gruplarına uygulanabilir.

Parola ilkelerinin Kullanıcı oluşturma kaynağına göre uygulanma farkları hakkında daha fazla bilgi için, bkz. [yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri][password-policy].

## <a name="password-hashes"></a>Parola karmaları

Yönetilen etki alanındaki kullanıcıların kimliğini doğrulamak için Azure AD DS 'nin, NT LAN Manager (NTLM) ve Kerberos kimlik doğrulaması için uygun bir biçimde parola karmaları olması gerekir. Azure AD, kiracınız için Azure AD DS etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gereken biçimde parola karmaları oluşturmaz veya depolamaz. Azure AD, güvenlik nedenleriyle şifresiz metin biçiminde hiçbir parola kimlik bilgisi depolamaz. Bu nedenle, Azure AD kullanıcıların mevcut kimlik bilgilerini temel alarak bu NTLM veya Kerberos parola karmalarını otomatik olarak üretemiyor.

Yalnızca bulut Kullanıcı hesapları için, kullanıcıların Azure AD DS kullanabilmesi için parolalarını değiştirmesi gerekir. Bu parola değiştirme işlemi, Kerberos ve NTLM kimlik doğrulamasının parola karmalarının Azure AD 'de oluşturulmasına ve depolanmasına neden olur.

Azure AD Connect kullanarak şirket içi AD DS ortamından eşitlenen kullanıcılar için [parola karmalarının eşitlenmesini etkinleştirin][hybrid-phs].

> [!IMPORTANT]
> Azure AD Connect yalnızca Azure AD kiracınız için Azure AD DS etkinleştirdiğinizde eski parola karmalarını eşitler. Yalnızca Azure AD ile şirket içi AD DS ortamını eşleştirmek için Azure AD Connect kullanıyorsanız eski parola karmaları kullanılmaz.
>
> Eski uygulamalarınız NTLM kimlik doğrulaması veya LDAP basit bağlamalar kullanmıyorsanız, Azure AD DS için NTLM parola karma eşitlemesini devre dışı bırakmanızı öneririz. Daha fazla bilgi için bkz. [zayıf şifre paketlerini ve NTLM kimlik bilgisi karma eşitlemesini devre dışı bırakma][secure-domain].

Uygun şekilde yapılandırıldıktan sonra, kullanılabilir parola karmaları Azure AD DS yönetilen etki alanında depolanır. Azure AD DS yönetilen etki alanını silerseniz, o noktada depolanan tüm parola karmaları da silinir. Azure AD 'deki eşitlenmiş kimlik bilgileri, daha sonra Azure AD DS yönetilen bir etki alanı oluşturursanız yeniden kullanılamaz. parola karmalarını yeniden depolamak için Parola karması eşitlemesini yeniden yapılandırmanız gerekir. Daha önce etki alanına katılmış VM 'Ler veya kullanıcılar, parola karmalarını yeni Azure AD DS yönetilen etki alanında oluşturmak ve depolamak için Azure AD ihtiyaçlarına hemen kimlik doğrulaması yapamaz. Daha fazla bilgi için bkz. [Azure AD DS Için Parola karması eşitleme işlemi ve Azure AD Connect][azure-ad-password-sync].

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamları ile eşitleme için yüklenmeli ve yapılandırılmalıdır. Nesneleri Azure AD 'ye geri eşitlemeniz için Azure AD DS tarafından yönetilen bir etki alanına Azure AD Connect yüklemek desteklenmez.

## <a name="forests-and-trusts"></a>Ormanlar ve güvenler

*Orman* , bir veya daha fazla *etki alanını*gruplamak için Active Directory Domain Services (AD DS) tarafından kullanılan mantıksal bir yapıdır. Etki alanları daha sonra Kullanıcı veya grup için nesneleri depolar ve kimlik doğrulama hizmetleri sağlar.

Azure AD DS, orman yalnızca bir etki alanı içerir. Şirket içi AD DS ormanları genellikle birçok etki alanı içerir. Büyük kuruluşlarda, özellikle Birleşmeler ve alımlar sonrasında, her biri birden çok etki alanı içeren birden çok şirket içi ormana sahip olabilirsiniz.

Varsayılan olarak, Azure AD DS yönetilen bir etki alanı bir *Kullanıcı* Ormanı olarak oluşturulur. Bu tür bir orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD 'deki tüm nesneleri eşitler. Kullanıcı hesapları, etki alanına katılmış bir VM 'de oturum açmak gibi Azure AD DS yönetilen etki alanında doğrudan kimlik doğrulaması yapabilir. Parola karmaları eşitlenecekse ve kullanıcılar akıllı kart kimlik doğrulaması gibi özel oturum açma yöntemlerini kullanmadığı zaman bir Kullanıcı ormanı işe yarar.

Azure AD DS *kaynak* ormanında, kullanıcılar şirket içi AD DS tek yönlü bir orman *güveni* üzerinden kimlik doğrular. Bu yaklaşımda, Kullanıcı nesneleri ve parola karmaları Azure AD DS ile eşitlenmez. Kullanıcı nesneleri ve kimlik bilgileri yalnızca şirket içi AD DS bulunur. Bu yaklaşım, kuruluşların Azure 'da, LDAPS, Kerberos veya NTLM gibi klasik kimlik doğrulamasına bağlı olan kaynakları ve uygulama platformlarını barındırmasına, ancak tüm kimlik doğrulama sorunları veya kaygıları kaldırılmasına olanak sağlar. Azure AD DS kaynak ormanları Şu anda önizleme aşamasındadır.

Azure AD DS orman türleri hakkında daha fazla bilgi için bkz. [kaynak ormanları nelerdir?][concepts-forest] ve [orman güvenleri Azure AD DS nasıl çalışır?][concepts-trust]

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [Azure AD DS yönetilen bir etki alanı oluşturun][create-instance].

<!-- INTERNAL LINKS -->
[password-policy]: password-policy.md
[hybrid-phs]: tutorial-configure-password-hash-sync.md#enable-synchronization-of-password-hashes
[secure-domain]: secure-your-domain.md
[azure-ad-password-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[create-instance]: tutorial-create-instance.md
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
