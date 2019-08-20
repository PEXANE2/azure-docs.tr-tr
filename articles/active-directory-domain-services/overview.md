---
title: Azure Active Directory Domain Services genel bakış | Microsoft Docs
description: Bu genel bakışta, bulutta uygulamalara ve hizmetlere kimlik hizmetleri sağlamak için Azure Active Directory Domain Services neler sağladığını ve kuruluşunuzda nasıl kullanacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: overview
ms.date: 08/14/2019
ms.author: iainfou
ms.openlocfilehash: 921266b78b82364b4610dcd74b6ee16ee44cb060
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617273"
---
# <a name="what-is-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services nedir?

Azure Active Directory Domain Services (Azure AD DS), Windows Server Active Directory ile tamamen uyumlu etki alanına katılması, Grup ilkesi, LDAP ve Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı Hizmetleri sağlar. Bulutta etki alanı denetleyicileri dağıtma, yönetme ve düzeltme eki uygulama gerekmeden bu etki alanı hizmetlerini kullanırsınız. Azure AD DS, mevcut Azure AD kiracınızla tümleştirilir, bu da kullanıcıların mevcut kimlik bilgilerini kullanarak oturum açmasını olanaklı kılar. Ayrıca, mevcut grupları ve Kullanıcı hesaplarını, kaynaklara erişimi güvenli hale getirmek için de kullanabilirsiniz. Bu, şirket içi kaynakların daha yumuşak bir şekilde daha iyi bir şekilde daha iyi bir şekilde daha iyi bir şekilde

Azure AD DS, Azure AD 'den kimlik bilgilerini çoğalttığından, bu nedenle yalnızca bulutta bulunan veya şirket içi Active Directory Domain Services (AD DS) ortamıyla eşitlenen Azure AD kiracılarıyla birlikte çalışmaktadır. Mevcut bir şirket içi AD DS ortamınız varsa, kullanıcılar için tutarlı bir kimlik sağlamak üzere Kullanıcı hesabı bilgilerini eşzamanlı hale getirebilirsiniz. Yalnızca bulut ortamlarında, Azure AD DS merkezi kimlik hizmetlerini kullanmak için geleneksel bir şirket içi AD DS ortamına ihtiyacınız yoktur. Her iki ortam için de aynı Azure AD DS özellikleri kümesi vardır.

Aşağıdaki videoda, Bulutta kimlik hizmetleri sağlamak için Azure AD DS uygulamalarınızın ve iş yüklerinizde nasıl tümleştirildiğini gösteren bir genel bakış sunulmaktadır:

<br />

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="common-ways-to-provide-identity-solutions-in-the-cloud"></a>Bulutta kimlik çözümleri sağlamanın yaygın yolları

Mevcut iş yüklerini buluta geçirdiğinizde, dizin durumunu algılayan uygulamalar, bir şirket içi AD DS dizinine okuma veya yazma erişimi için LDAP kullanabilir. Windows Server 'da çalışan uygulamalar genellikle etki alanına katılmış olarak dağıtılır ve bu sayede grup ilkesi kullanarak güvenli bir şekilde yönetilebilir. Uygulamalar, son kullanıcıların kimliğini doğrulamak için Kerberos veya NTLM kimlik doğrulaması gibi Windows tümleşik kimlik doğrulamasına de bağlı olabilir.

BT yöneticileri, Azure 'da çalışan uygulamalara bir kimlik hizmeti sağlamak için genellikle aşağıdaki çözümlerden birini kullanır:

* Azure 'da ve şirket içi AD DS ortamında çalışan iş yükleri arasında siteden siteye VPN bağlantısı yapılandırın.
* AD DS etki alanı/ormanı genişletmek için Azure sanal makinelerini (VM 'Ler) kullanarak çoğaltma etki alanı denetleyicileri oluşturun.
* Azure VM 'lerde çalışan etki alanı denetleyicilerini kullanarak Azure 'da tek başına AD DS ortamı dağıtın.

Bu yaklaşımlar sayesinde, şirket içi dizine yönelik VPN bağlantıları, uygulamaları geçici ağ kesintileri veya kesintiler için savunmasız hale getirir. Azure 'da VM 'Leri kullanarak etki alanı denetleyicileri dağıtırsanız, BT ekibi VM 'Leri bunları yönetmeli, güvenli, düzeltme eki, izleme, yedekleme ve sorunlarını gidermelidir.

Azure AD DS, kimlik hizmetleri sağlamak için şirket içi AD DS ortamına VPN bağlantıları oluşturma veya Azure 'da VM 'Leri çalıştırma ve yönetme gereksinimlerine yönelik alternatifler sunmaktadır. Yönetilen bir hizmet olarak Azure AD DS, hem karma hem de yalnızca bulut ortamları için tümleşik bir kimlik çözümü oluşturma karmaşıklığını azaltır.

## <a name="azure-ad-ds-features-and-benefits"></a>Azure AD DS özellikleri ve avantajları

Bulutta uygulamalara ve sanal makinelere kimlik hizmetleri sağlamak için Azure AD DS, etki alanına katılması, Güvenli LDAP (LDAPS), grup ilkesi ve DNS yönetimi ve LDAP bağlama ve okuma desteği gibi işlemler için geleneksel bir AD DS ortamıyla tamamen uyumludur. LDAP yazma desteği, Azure AD DS yönetilen etki alanında oluşturulan nesneler için kullanılabilir, ancak Azure AD 'den eşitlenen kaynaklar için kullanılabilir. Azure AD DS dağıtım ve yönetim işlemlerini basitleştirecek aşağıdaki özellikler:

* **Basitleştirilmiş dağıtım deneyimi:** Azure AD DS, Azure portal tek bir sihirbaz kullanılarak Azure AD kiracınız için etkinleştirilmiştir.
* **Azure AD ile tümleşik:** Kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri Azure AD kiracınızdan otomatik olarak kullanılabilir. Azure AD kiracınızdan veya şirket içi AD DS ortamınızda bulunan yeni kullanıcılar, gruplar veya özniteliklerde yapılan değişiklikler otomatik olarak Azure AD DS eşitlenir.
* **Şirket kimlik bilgilerinizi/parolalarınızı kullanın:** Azure AD kiracınızdaki kullanıcıların parolaları Azure AD DS ile çalışır. Kullanıcılar, Şirket kimlik bilgilerini etki alanına katmak, etkileşimli olarak veya uzak masaüstü üzerinden oturum açmak ve Azure AD DS yönetilen etki alanında kimlik doğrulaması yapmak için kullanabilir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması desteğiyle, Windows ile tümleşik kimlik doğrulamasına dayanan uygulamalar dağıtabilirsiniz.
* **Yüksek kullanılabilirlik:** Azure AD DS, yönetilen etki alanınız için yüksek kullanılabilirlik sağlayan birden çok etki alanı denetleyicisi içerir. Bu yüksek kullanılabilirlik, hizmet çalışma süresini ve esnekliği hatalara karşı garanti eder.

Azure AD DS yönetilen bir etki alanının bazı önemli yönleri şunlardır:

* Azure AD DS yönetilen etki alanı tek başına bir etki alanıdır. Şirket içi bir etki alanının uzantısı değildir.
* BT takımınızın bu Azure AD DS yönetilen etki alanı için etki alanı denetleyicilerini yönetmesi, düzeltme eki uygulama veya izleme yapması gerekmez.

Şirket içinde AD DS çalıştıran hibrit ortamlarda, AD çoğaltmasını Azure AD DS yönetilen etki alanına yönetmeniz gerekmez. Şirket içi dizininizdeki Kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri Azure AD Connect aracılığıyla Azure AD ile eşitlenir. Bu Kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri, Azure AD DS yönetilen etki alanı içinde otomatik olarak kullanılabilir.

## <a name="how-does-azure-ad-ds-work"></a>Azure AD DS nasıl çalışır?

Azure, kimlik hizmetleri sağlamak için seçtiğiniz bir sanal ağda AD DS yönetilen bir etki alanı oluşturur. Arka planda ve yönetmeniz, güvenli hale getirmeniz veya güncelleştirmeniz gerekmeden yedekliliğe sahip bir Windows Server etki alanı denetleyicileri çifti sağlanır. Azure AD DS yönetilen etki alanı, merkezi bir grup kullanıcıya, gruba ve kimlik bilgilerine erişim sağlamak için Azure AD 'den tek yönlü bir eşitleme gerçekleştirecek şekilde yapılandırılmıştır. Kaynakları doğrudan Azure AD DS yönetilen etki alanında oluşturabilirsiniz, ancak Azure AD 'ye geri eşitlenmez. Azure 'daki bu sanal ağa bağlanan uygulamalar, hizmetler ve VM 'Ler, etki alanına katılması, Grup ilkesi, LDAP ve Kerberos/NTLM kimlik doğrulaması gibi ortak AD DS özellikleri kullanabilir. Şirket içi AD DS ortamı olan bir karma ortamda, [Azure AD Connect][azure-ad-connect] kimlik BILGILERINI Azure AD ile eşitler.

![AD Connect kullanarak Azure AD ve şirket içi Active Directory Domain Services Azure AD Domain Services eşitleme](./media/active-directory-domain-services-design-guide/sync-topology.png)

Azure AD DS 'yi eylemde görmek için birkaç örneğe göz atalım:

* [Hibrit kuruluşlar için Azure AD DS](#azure-ad-ds-for-hybrid-organizations)
* [Yalnızca bulutta kuruluşlar için Azure AD DS](#azure-ad-ds-for-cloud-only-organizations)

### <a name="azure-ad-ds-for-hybrid-organizations"></a>Hibrit kuruluşlar için Azure AD DS

Birçok kuruluş, hem bulut hem de şirket içi uygulama iş yüklerini içeren karma bir altyapı çalıştırır. Yükseltme ve kaydırma stratejisinin bir parçası olarak Azure 'a geçirilen eski uygulamalar, kimlik bilgilerini sağlamak için geleneksel LDAP bağlantıları kullanmaya devam edebilir. Bu karma altyapıyı desteklemek için, şirket içi Active Directory Domain Services (AD DS) ortamından kimlik bilgileri bir Azure AD kiracısıyla eşitlenebilir. Azure AD DS, Azure 'da bu eski uygulamaları bir kimlik kaynağıyla, şirket içi dizin hizmetlerine geri uygulama bağlantısını yapılandırmaya ve yönetmeye gerek kalmadan sağlayabilir.

Hem şirket içinde hem de Azure kaynaklarını çalıştıran bir karma kuruluş olan Litwa Corporation için bir örneğe bakalım:

![Şirket içi eşitleme içeren karma kuruluş için Azure Active Directory Domain Services](./media/overview/synced-tenant.png)

* Etki alanı Hizmetleri gerektiren uygulamalar ve sunucu iş yükleri, Azure 'daki bir sanal ağa dağıtılır.
    * Bu, yükseltme ve kaydırma stratejisinin bir parçası olarak Azure 'a geçirilen eski uygulamaları içerebilir.
* Şirket içi dizinlerinden kimlik bilgilerini Azure AD kiracısıyla eşleştirmek için, Litwa şirketi [Azure AD Connect][azure-ad-connect]dağıtır.
    * Eşitlenen kimlik bilgileri, Kullanıcı hesaplarını ve grup üyeliklerini içerir.
* Litwonların BT ekibi, bu veya eşlenmiş bir sanal ağ üzerinden Azure AD kiracısı için Azure AD DS sunar.
* Azure sanal ağında dağıtılan uygulamalar ve VM 'Ler, etki alanına katılması, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve grup ilkesi gibi Azure AD DS özelliklerini kullanabilir.

### <a name="azure-ad-ds-for-cloud-only-organizations"></a>Yalnızca bulutta kuruluşlar için Azure AD DS

Yalnızca bulutta yer alan bir Azure AD kiracısı, şirket içi kimlik kaynağına sahip değildir. Kullanıcı hesapları ve grup üyelikleri, örneğin, Azure AD 'de oluşturulur ve yönetilir.

Şimdi yalnızca, kimlik için Azure AD kullanan bir yalnızca bulut organizasyonu olan contoso örneğine göz atalım. Tüm Kullanıcı kimlikleri, kimlik bilgileri ve grup üyelikleri, Azure AD 'de oluşturulur ve yönetilir. Şirket içi bir dizinden herhangi bir kimlik bilgisini eşitlemeye yönelik Azure AD Connect ek bir yapılandırması yoktur.

![Şirket içi eşitleme olmadan yalnızca bulutta yer alan bir kuruluş için Azure Active Directory Domain Services](./media/overview/cloud-only-tenant.png)

* Etki alanı Hizmetleri gerektiren uygulamalar ve sunucu iş yükleri, Azure 'daki bir sanal ağa dağıtılır.
* Contoso BT ekibi, Azure AD kiracısının bu veya eşlenmiş bir sanal ağ için Azure AD DS sunar.
* Azure sanal ağında dağıtılan uygulamalar ve VM 'Ler, etki alanına katılması, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve grup ilkesi gibi Azure AD DS özelliklerini kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS diğer kimlik çözümleriyle ve eşitlemenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure AD DS Azure AD ile karşılaştırın, Azure VM 'lerinde Active Directory Domain Services ve şirket içi Active Directory Domain Services][compare]
* [Azure AD Domain Services Azure AD dizininizle nasıl eşitleneceğini öğrenin][synchronization]

Başlamak için [Azure Portal kullanarak Azure AD DS yönetilen bir etki alanı oluşturun][tutorial-create].

<!-- INTERNAL LINKS -->
[compare]: compare-identity-solutions.md
[synchronization]: synchronization.md
[tutorial-create]: tutorial-create-instance.md
[azure-ad-connect]: ../active-directory/hybrid/whatis-hybrid-identity.md
[password-hash-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md
