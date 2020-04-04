---
title: Azure Active Directory Etki Alanı Hizmetlerine Genel Bakış | Microsoft Dokümanlar
description: Bu genel bakışta, Azure Active Directory Domain Services'ın buluttaki uygulamalara ve hizmetlere kimlik hizmetleri sağlamak için kuruluşunuzda nasıl kullanacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: overview
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 0f5f890b4f32961e00fb30316a1dc7c88ef93a45
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654835"
---
# <a name="what-is-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetleri nedir?

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), etki alanı birleştirme, grup ilkesi, hafif dizin erişim protokolü (LDAP) ve Windows Server Active Directory ile tam uyumlu Kerberos / NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Bu etki alanı hizmetlerini bulutta etki alanı denetleyicilerini dağıtmaya, yönetmeye ve düzeltmee gerek kalmadan kullanırsınız. Azure AD DS, kullanıcıların mevcut kimlik bilgilerini kullanarak oturum açmalarını mümkün kılan mevcut Azure AD kiracınızla tümleşir. Kaynaklara erişimi güvence altına almak için varolan grupları ve kullanıcı hesaplarını da kullanabilirsiniz ve bu da şirket içi kaynakların Azure'a daha sorunsuz bir şekilde kaldırılmasını ve kaydırmasını sağlar.

Başlamak için, [Azure portalını kullanarak Azure AD DS yönetilen bir etki alanı oluşturun.][tutorial-create]

Azure AD DS, Azure AD'deki kimlik bilgilerini çoğaltır, bu nedenle yalnızca bulut erişimiolan veya şirket içi Active Directory Domain Services (AD DS) ortamıyla eşitlenmiş Azure AD kiracılarıyla çalışır. Her iki ortam için de aynı Azure AD DS özellikleri kümesi bulunur.

* Varolan bir şirket içi AD DS ortamınız varsa, kullanıcılar için tutarlı bir kimlik sağlamak için kullanıcı hesabı bilgilerini eşitleyebilirsiniz.
* Yalnızca bulutortamları için, Azure AD DS'nin merkezi kimlik hizmetlerini kullanmak için geleneksel şirket içi AD DS ortamına ihtiyacınız yoktur.

Aşağıdaki video, Azure AD DS'nin bulutta kimlik hizmetleri sağlamak için uygulamalarınızla ve iş yüklerinizle nasıl entegre olduğunu özetler:

<br />

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="common-ways-to-provide-identity-solutions-in-the-cloud"></a>Bulutta kimlik çözümleri sunmanın yaygın yolları

Varolan iş yüklerini buluta geçirdiğinizde, dizin bilincine bağlı uygulamalar LDAP'yi şirket içi BIR AD DS dizinine erişim okumak veya yazmak için kullanabilir. Windows Server'da çalışan uygulamalar genellikle etki alanı birleştirilmiş sanal makinelerde (VM) dağıtılır, böylece Grup İlkesi kullanılarak güvenli bir şekilde yönetilebilirler. Son kullanıcıların kimliğini doğrulamak için uygulamalar, Kerberos veya NTLM kimlik doğrulaması gibi Windows'a entegre kimlik doğrulamasına da güvenebilir.

BT yöneticileri, Azure'da çalışan uygulamalara kimlik hizmeti sağlamak için genellikle aşağıdaki çözümlerden birini kullanır:

* Azure'da çalışan iş yükleri ile şirket içi AD DS ortamı arasında siteden siteye VPN bağlantısı yapılandırın.
    * Şirket içi etki alanı denetleyicileri daha sonra VPN bağlantısı üzerinden kimlik doğrulaması sağlar.
* AD DS etki alanını / ormanı şirket içinde genişletmek için Azure sanal makineleri (VM) kullanarak çoğaltma etki alanı denetleyicileri oluşturun.
    * Azure VM'lerde çalışan etki alanı denetleyicileri kimlik doğrulaması sağlar ve şirket içi AD DS ortamı arasında dizin bilgilerini çoğaltır.
* Azure VM'lerde çalışan etki alanı denetleyicilerini kullanarak Azure'da bağımsız bir AD DS ortamı dağıtın.
    * Azure VM'lerde çalışan etki alanı denetleyicileri kimlik doğrulaması sağlar, ancak şirket içi AD DS ortamından çoğaltılan dizin bilgisi yoktur.

Bu yaklaşımlarla, şirket içi dizine VPN bağlantıları uygulamaları geçici ağ aksaklıklarına veya kesintilerine karşı savunmasız hale getirir. Etki alanı denetleyicilerini Azure'da VM'ler kullanarak dağıtığınızda, BT ekibinin VM'leri yönetmesi, ardından bunları güvenli hale alması, yama, izleme, yedekleme ve sorun gidermesi gerekir.

Azure AD DS, şirket içi BIR AD DS ortamına VPN bağlantıları oluşturma veya kimlik hizmetleri sağlamak için Azure'da VM'leri çalıştırma ve yönetme gereksinimine alternatifler sunar. Azure AD DS, yönetilen bir hizmet olarak, hem karma hem de yalnızca bulut ortamları için tümleşik bir kimlik çözümü oluşturmak için karmaşıklığı azaltır.

## <a name="azure-ad-ds-features-and-benefits"></a>Azure AD DS özellikleri ve avantajları

Azure AD DS, buluttaki uygulamalara ve VM'lere kimlik hizmetleri sağlamak için etki alanına katılma, güvenli LDAP (LDAPS), Grup İlkesi, DNS yönetimi ve LDAP bağlama ve okuma desteği gibi işlemler için geleneksel AD DS ortamıyla tamamen uyumludur. LDAP yazma desteği, Azure AD DS yönetilen etki alanında oluşturulan nesneler için kullanılabilir, ancak Azure AD'den senkronize edilen kaynaklar için kullanılamaz.

Kimlik seçenekleriniz hakkında daha fazla bilgi edinmek için [Azure AD DS'sini Azure AD ile, Azure VM'lerde Active Directory Etki Alanı Hizmetleri ve şirket içinde Active Directory Etki Alanı Hizmetleri ile karşılaştırın.][compare]

Azure AD DS'nin aşağıdaki özellikleri dağıtım ve yönetim işlemlerini basitleştirir:

* **Basitleştirilmiş dağıtım deneyimi:** Azure REKLAM DS, Azure portalında tek bir sihirbaz kullanarak Azure AD kiracınız için etkinleştirilir.
* **Azure AD ile entegre:** Kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri Azure AD kiracınızdan otomatik olarak kullanılabilir. Azure AD kiracınızdan veya şirket içi AD DS ortamınızdan yeni kullanıcılar, gruplar veya özniteliklerdeki değişiklikler otomatik olarak Azure AD DS ile senkronize edilir.
    * Azure AD'ınıza bağlı dış dizinlerde bulunan hesaplar Azure AD DS'de kullanılamaz. Kimlik bilgileri bu dış dizinler için kullanılamaz, bu nedenle Azure AD DS yönetilen etki alanına eşitlenemez.
* **Kurumsal kimlik bilgilerinizi/parolalarınızı kullanın:** Azure AD DS'deki kullanıcıların parolaları Azure AD kiracınızdakiyle aynıdır. Kullanıcılar, şirket kimlik bilgilerini etki alanına katılmak, etkileşimli olarak veya uzak masaüstü üzerinden oturum açmak ve Azure AD DS yönetilen etki alanına karşı kimlik doğrulaması yapmak için kullanabilir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması desteğiyle, Windows tümleşik kimlik doğrulamaya dayanan uygulamaları dağıtabilirsiniz.
* **Yüksek kullanılabilirlik:** Azure AD DS, yönetilen etki alanınız için yüksek kullanılabilirlik sağlayan birden çok etki alanı denetleyicisi içerir. Bu yüksek kullanılabilirlik, servis çalışma süresini ve arızalara karşı dayanıklılığı garanti eder.
    * [Azure Kullanılabilirlik Bölgelerini][availability-zones]destekleyen bölgelerde, bu etki alanı denetleyicileri ek esneklik için bölgelere de dağıtılır.

Azure AD DS yönetilen etki alanının bazı önemli yönleri şunlardır:

* Azure AD DS yönetilen etki alanı tek başına bir etki alanıdır. Şirket içi alan adının bir uzantısı değildir.
    * Gerekirse, Azure AD DS'den şirket içi AD DS ortamına tek yönlü giden orman güvenleri oluşturabilirsiniz. Daha fazla bilgi için Azure [AD DS için Kaynak ormanı kavramları ve özelliklerine][ forest-trusts]bakın.
* BT ekibinizin bu Azure AD DS yönetilen etki alanı için etki alanı denetleyicilerini yönetmesi, yamaması veya izlemesi gerekmez.

AD DS'yi şirket içinde çalıştıran karma ortamlar için, AD çoğaltmaişlemini Azure AD DS yönetilen etki alanında yönetmeniz gerekmez. Şirket içi dizininizdeki kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri [Azure AD Connect][azure-ad-connect]aracılığıyla Azure AD'ye senkronize edilir. Bu kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri Azure AD DS yönetilen etki alanında otomatik olarak kullanılabilir.

## <a name="how-does-azure-ad-ds-work"></a>Azure AD DS nasıl çalışır?

Azure, kimlik hizmetleri sağlamak için seçtiğiniz sanal ağda bir AD DS örneği oluşturur. Sahne arkasında, Azure VM'lerde çalışan bir çift Windows Server etki alanı denetleyicisi oluşturulur. Bu etki alanı denetleyicilerini yönetmeniz, yapılandırmanız veya güncelleştirmeniz gerekmez. Azure platformu, etki alanı denetleyicilerini Azure AD DS hizmetinin bir parçası olarak yönetir.

Azure AD DS yönetilen etki alanı, merkezi bir kullanıcı, grup ve kimlik bilgileri kümesine erişim sağlamak için Azure AD'den tek yönlü eşitleme gerçekleştirecek şekilde yapılandırılmıştır. Kaynakları doğrudan Azure AD DS yönetilen etki alanında oluşturabilirsiniz, ancak bunlar Azure AD ile eşitlenmez. Azure'da bu sanal ağa bağlanan uygulamalar, hizmetler ve Sanal Kullanımalanları, etki alanı birleştirme, grup ilkesi, LDAP ve Kerberos / NTLM kimlik doğrulaması gibi yaygın AD DS özelliklerini kullanabilir.

Azure [AD Connect,][azure-ad-connect] şirket içi AD DS ortamına sahip karma bir ortamda kimlik bilgilerini Azure AD ile senkronize eder ve daha sonra Azure AD DS ile senkronize edilir.

![Azure AD Etki Alanı Hizmetlerinde Azure AD ve şirket içi Active Directory Etki Alanı Hizmetleri ile AD Connect kullanarak senkronizasyon](./media/active-directory-domain-services-design-guide/sync-topology.png)

Azure AD DS'yi iş başında görmek için birkaç örnek görelim:

* [Karma kuruluşlar için Azure AD DS](#azure-ad-ds-for-hybrid-organizations)
* [Yalnızca bulut kuruluşları için Azure AD DS](#azure-ad-ds-for-cloud-only-organizations)

### <a name="azure-ad-ds-for-hybrid-organizations"></a>Karma kuruluşlar için Azure AD DS

Birçok kuruluş, hem bulut hem de şirket içi uygulama iş yüklerini içeren karma bir altyapı çalıştırıyor. Kaldırma ve kaydırma stratejisinin bir parçası olarak Azure'a geçirilen eski uygulamalar, kimlik bilgilerini sağlamak için geleneksel LDAP bağlantılarını kullanabilir. Bu karma altyapıyı desteklemek için, şirket içi AD DS ortamındaki kimlik bilgileri bir Azure AD kiracısına eşitlenebilir. Azure AD DS, daha sonra bu eski uygulamaları Azure'da, uygulama bağlantılarını şirket içi dizin hizmetlerine yapılandırmaya ve yönetmeye gerek kalmadan bir kimlik kaynağıyla sağlar.

Hem şirket içi hem de Azure kaynaklarını çalıştıran karma bir kuruluş olan Litware Corporation için bir örneğe bakalım:

![Şirket içi eşitleme içeren karma bir kuruluş için Azure Active Directory Etki Alanı Hizmetleri](./media/overview/synced-tenant.png)

* Etki alanı hizmetleri gerektiren uygulamalar ve sunucu iş yükleri Azure'da sanal bir ağda dağıtılır.
    * Bu, kaldırma ve kaydırma stratejisinin bir parçası olarak Azure'a geçirilen eski uygulamaları içerebilir.
* Litware Corporation, kimlik bilgilerini şirket içi dizinlerinden Azure AD kiracılarına senkronize etmek için [Azure AD Connect'i][azure-ad-connect]dağıtır.
    * Senkronize edilen kimlik bilgileri, kullanıcı hesaplarını ve grup üyeliklerini içerir.
* Litware'in BT ekibi, Azure AD kiracıları veya bir eşlenen sanal ağ için Azure AD DS'ye olanak tanır.
* Azure sanal ağında dağıtılan uygulamalar ve SANAL'lar daha sonra etki alanı birleştirme, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi Azure AD DS özelliklerini kullanabilir.

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamlarıyla senkronizasyon için yüklenmeli ve yapılandırılmalıdır. Azure AD Connect'i Azure AD DS yönetilen bir etki alanına yüklemek için desteklenmez.

### <a name="azure-ad-ds-for-cloud-only-organizations"></a>Yalnızca bulut kuruluşları için Azure AD DS

Yalnızca buluta dayalı Azure AD kiracının şirket içi kimlik kaynağı yoktur. Örneğin, kullanıcı hesapları ve grup üyelikleri doğrudan Azure AD'de oluşturulur ve yönetilir.

Şimdi, kimlik için Azure AD kullanan yalnızca bulut kuruluşu Contoso için bir örneğe bakalım. Tüm kullanıcı kimlikleri, kimlik bilgileri ve grup üyelikleri Azure AD'de oluşturulur ve yönetilir. Azure AD Connect'in şirket içi dizindeki kimlik bilgilerini eşitlemek için ek bir yapılandırması yoktur.

![Şirket içi senkronizasyon olmadan yalnızca bulut ayarı için Azure Active Directory Etki Alanı Hizmetleri](./media/overview/cloud-only-tenant.png)

* Etki alanı hizmetleri gerektiren uygulamalar ve sunucu iş yükleri Azure'da sanal bir ağda dağıtılır.
* Contoso'nun BT ekibi, Azure AD kiracıları için Azure AD DS'yi bu veya gözlenen sanal ağ için etkinleştirir.
* Azure sanal ağında dağıtılan uygulamalar ve SANAL'lar daha sonra etki alanı birleştirme, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi Azure AD DS özelliklerini kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS diğer kimlik çözümleri ve eşitlemenin nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure AD DS'yi Azure AD, Azure VM'lerde Etkin Dizin Etki Alanı Hizmetleri ve şirket içinde Etkin Dizin Etki Alanı Hizmetleri ile karşılaştırın][compare]
* [Azure AD Etki Alanı Hizmetlerinin Azure REKLAM dizininizdekiyle nasıl eşitlebildiğini öğrenin][synchronization]

Başlamak için, [Azure portalını kullanarak Azure AD DS yönetilen bir etki alanı oluşturun.][tutorial-create]

<!-- INTERNAL LINKS -->
[compare]: compare-identity-solutions.md
[synchronization]: synchronization.md
[tutorial-create]: tutorial-create-instance.md
[azure-ad-connect]: ../active-directory/hybrid/whatis-azure-ad-connect.md
[password-hash-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md
[availability-zones]: ../availability-zones/az-overview.md
[forest-trusts]: concepts-resource-forest.md
