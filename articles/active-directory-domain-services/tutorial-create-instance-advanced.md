---
title: Öğretici - Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturma | Microsoft Dokümanlar
description: Bu eğitimde, Bir Azure Etkin Dizin Etki Alanı Hizmetleri örneğini nasıl oluşturup yapılandırabileceğinizi ve Azure portalını kullanarak gelişmiş yapılandırma seçenekleri belirtmeyi öğreneceksiniz.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: a7ab79fe7ccfbf6d292b6ba7be58026955a06516
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519284"
---
# <a name="tutorial-create-and-configure-an-azure-active-directory-domain-services-instance-with-advanced-configuration-options"></a>Öğretici: Gelişmiş yapılandırma seçenekleriyle Azure Active Directory Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), Windows Server Active Directory ile tam uyumlu etki alanı birleştirme, grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Etki alanı denetleyicilerini dağıtmadan, yönetmeden ve düzeltmeden bu etki alanı hizmetlerini tüketirsiniz. Azure AD DS, mevcut Azure AD kiracınızla tümleşir. Bu tümleştirme, kullanıcıların şirket kimlik bilgilerini kullanarak oturum açmalarına olanak tanır ve kaynaklara erişimi güvence altına almak için varolan grupları ve kullanıcı hesaplarını kullanabilirsiniz.

Ağ ve eşitleme için [varsayılan yapılandırma seçeneklerini kullanarak yönetilen bir etki alanı oluşturabilir][tutorial-create-instance] veya bu ayarları el ile tanımlayabilirsiniz. Bu öğretici, Azure portalını kullanarak bir Azure AD DS örneği oluşturmak ve yapılandırmak için bu gelişmiş yapılandırma seçeneklerini nasıl tanımlayabileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yönetilen bir etki alanı için DNS ve sanal ağ ayarlarını yapılandırma
> * Azure AD DS örneği oluşturma
> * Etki alanı yönetimine yönetim kullanıcıları ekleme
> * Parola karması eşitlemeyi etkinleştirme

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure AD DS'yi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ihtiyacınız vardır.
* Gerekli Azure AD DS kaynaklarını oluşturmak için Azure aboneliğinizde *Katılımcı* ayrıcalıklarına ihtiyacınız vardır.

Azure AD DS için gerekli olmasa da, Azure AD kiracısı için [self servis parola sıfırlama (SSPR) yapılandırmanız][configure-sspr] önerilir. Kullanıcılar parolalarını SSPR olmadan değiştirebilir, ancak SSPR parolalarını unutup sıfırlamaları gerektiğinde yardımcı olur.

> [!IMPORTANT]
> Azure AD DS yönetilen bir etki alanı oluşturduktan sonra, örneği farklı bir kaynak grubuna, sanal ağa, aboneye vb. taşıyamazsınız. Azure AD DS örneğini dağıttığınızda en uygun aboneliği, kaynak grubunu, bölgeyi ve sanal ağı seçmeye özen.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticide, Azure portalını kullanarak Azure AD DS örneğini oluşturur ve yapılandırırsınız. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-an-instance-and-configure-basic-settings"></a>Örnek oluşturma ve temel ayarları yapılandırma

**Azure AD Etki Alanı Hizmetlerini Etkinleştir** sihirbazını başlatmak için aşağıdaki adımları tamamlayın:

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
1. Arama çubuğuna *Etki Alanı Hizmetleri* girin ve ardından arama önerilerinden Azure AD Etki Alanı *Hizmetlerini* seçin.
1. Azure AD Etki Alanı Hizmetleri sayfasında **Oluştur'u**seçin. **Azure AD Etki Alanı Hizmetlerini Etkinleştir** sihirbazı başlatıldı.
1. Yönetilen etki alanını oluşturmak istediğiniz Azure **Aboneliği'ni** seçin.
1. Yönetilen etki alanının ait olması gereken **Kaynak grubunu** seçin. Yeni **oluşturmayı** veya varolan bir kaynak grubu seçmeyi seçin.

Bir Azure AD DS örneği oluşturduğunuzda, bir DNS adı belirtirsiniz. Bu DNS adını seçerken bazı hususlar vardır:

* **Yerleşik etki alanı adı:** Varsayılan olarak, dizinin yerleşik etki alanı adı kullanılır *(.onmicrosoft.com* soneki). Yönetilen etki alanına Internet üzerinden güvenli LDAP erişimi sağlamak istiyorsanız, bu varsayılan etki alanıyla bağlantıyı güvence altına almak için dijital bir sertifika oluşturamazsınız. Microsoft *.onmicrosoft.com* etki alanının sahibidir, bu nedenle Sertifika Yetkilisi (CA) sertifika vermez.
* **Özel alan adları:** En yaygın yaklaşım, genellikle zaten sahip olduğunuz ve routable olan özel bir etki alanı adı belirtmektir. Bir routable, özel etki alanı kullandığınızda, trafik doğru uygulamalarınızı desteklemek için gerektiği gibi akabilir.
* **Routable olmayan etki alanı sonekleri:** Genellikle *contoso.local*gibi routable olmayan bir etki alanı adı soneki nden kaçınmanızı öneririz. *.yerel* sonek çözülebilir değildir ve DNS çözümüyle ilgili sorunlara neden olabilir.

> [!TIP]
> Özel bir etki alanı adı oluşturursanız, varolan DNS ad alanlarına dikkat edin. Varolan herhangi bir Azure veya şirket içi DNS ad alanından ayrı bir etki alanı adı kullanılması önerilir.
>
> Örneğin, *contoso.com*varolan bir DNS ad alanınız varsa, *aaddscontoso.com*özel etki alanı adı içeren bir Azure AD DS yönetilen etki alanı oluşturun. Güvenli LDAP kullanmanız gerekiyorsa, gerekli sertifikaları oluşturmak için bu özel alan adını kaydetmeniz ve sahip olmanız gerekir.
>
> Ortamınızdaki diğer hizmetler için bazı ek DNS kayıtları veya ortamınızdaki varolan DNS ad alanları arasında koşullu DNS iletmeleri oluşturmanız gerekebilir. Örneğin, kök DNS adını kullanarak bir siteyi barındıran bir web sunucusu çalıştırıyorsanız, ek DNS girişleri gerektiren ad çakışmaları olabilir.
>
> Bu öğreticilerde ve nasıl yapılır makalelerinde, *özel aaddscontoso.com* etki alanı kısa bir örnek olarak kullanılır. Tüm komutlarda kendi etki alanı adınızı belirtin.

Aşağıdaki DNS ad kısıtlamaları da geçerlidir:

* **Etki alanı öneki kısıtlamaları:** Önek 15 karakterden daha uzun olan yönetilen bir etki alanı oluşturamazsınız. Belirtilen etki alanı adınızın öneki *(aaddscontoso.com* etki alanı adındaki *aaddscontoso* gibi) 15 veya daha az karakter içermelidir.
* **Ağ adı çakışmaları:** Yönetilen etki alanınızın DNS etki alanı adı sanal ağda zaten bulunmamalıdır. Özellikle, bir ad çakışması neden olacak aşağıdaki senaryoları denetleyin:
    * Azure sanal ağında aynı DNS etki alanı adı olan bir Active Directory etki alanınız zaten varsa.
    * Yönetilen etki alanını etkinleştirmeyi planladığınız sanal ağın şirket içi ağınızla VPN bağlantısı varsa. Bu senaryoda, şirket içi ağınızda aynı DNS etki alanı adı olan bir etki alanınız olmadığından emin olun.
    * Azure sanal ağında bu ada sahip mevcut bir Azure bulut hizmetiniz varsa.

Azure AD DS örneği oluşturmak için Azure portalının *Temeller* penceresindeki alanları tamamlayın:

1. Önceki noktaları göz önünde bulundurarak yönetilen etki alanınız için bir **DNS etki alanı adı** girin.
1. Yönetilen etki alanının oluşturulması gereken Azure **Konumu'nu** seçin. Kullanılabilirlik Bölgelerini destekleyen bir bölge seçerseniz, Azure AD DS kaynakları ek fazlalık için bölgeler e göre dağıtılır.

    Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılığı güvence altına almak için etkinleştirilmiş tüm bölgelerde en az üç ayrı alan vardır.

    Azure AD DS'nin bölgelere dağıtılması için yapılandırabileceğiniz hiçbir şey yoktur. Azure platformu, kaynakların bölge dağıtımını otomatik olarak işler. Daha fazla bilgi ve bölge kullanılabilirliğini görmek için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?][availability-zones]

1. **SKU,** oluşturabileceğiniz maksimum orman güvenlerinin performansını, yedekleme sıklığını ve maksimum sayısını belirler. İşletmenizin talepleri veya gereksinimleriniz değişirse, yönetilen etki alanı oluşturulduktan sonra SKU'yu değiştirebilirsiniz. Daha fazla bilgi için [Azure AD DS SKU kavramlarına][concepts-sku]bakın.

    Bu öğretici için *Standart* SKU'yu seçin.
1. *Orman,* Active Directory Domain Services tarafından bir veya daha fazla etki alanını gruplandırmak için kullanılan mantıksal bir yapıdır. Varsayılan olarak, *Kullanıcı* ormanı olarak Azure AD DS yönetilen bir etki alanı oluşturulur. Bu tür orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD'deki tüm nesneleri eşitler. *Kaynak* ormanı yalnızca doğrudan Azure AD'de oluşturulan kullanıcıları ve grupları eşitler. Kaynak ormanları şu anda önizlemede. *Kaynak* ormanları hakkında daha fazla bilgi için, neden kullanabileceğiniz ve şirket içi AD DS etki alanlarında orman güvenlerinin nasıl oluşturulabileceği de dahil olmak üzere, [Azure AD DS kaynak ormanlarına genel bakış][resource-forests]alabilirsiniz.

    Bu öğretici için bir *Kullanıcı* ormanı oluşturmayı seçin.

    ![Azure AD Etki Alanı Hizmetleri örneği için temel ayarları yapılandırma](./media/tutorial-create-instance-advanced/basics-window.png)

1. Ek seçenekleri el ile yapılandırmak için **İleri - Ağ'ı**seçin. Aksi takdirde, varsayılan yapılandırma seçeneklerini kabul etmek için **Gözden Geçir + oluştur'u** seçin ve ardından yönetilen [etki alanınızı dağıtmak](#deploy-the-managed-domain)için bölüme atlayın. Bu oluşturma seçeneğini seçtiğinizde aşağıdaki varsayılanlar yapılandırılır:

    * *10.0.1.0/24*IP adresi aralığını kullanan *aadds-vnet* adlı sanal bir ağ oluşturur.
    * *10.0.1.0/24*IP adresi aralığını kullanarak *aadds-subnet* adlı bir alt ağ oluşturur.
    * Azure AD'deki *tüm* kullanıcıları Azure AD DS yönetilen etki alanına senkronize eder.

## <a name="create-and-configure-the-virtual-network"></a>Sanal ağı oluşturma ve yapılandırma

Bağlantı sağlamak için bir Azure sanal ağı ve özel bir alt ağ gereklidir. Azure AD DS bu sanal ağ alt ağında etkinleştirilir. Bu öğreticide, varolan bir sanal ağı kullanmayı seçebilmekle birlikte sanal bir ağ oluşturursunuz. Her iki yaklaşımda da Azure AD DS tarafından kullanılmak üzere özel bir alt ağ oluşturmanız gerekir.

Bu özel sanal ağ alt ağı için bazı hususlar aşağıdaki alanları içerir:

* Azure AD DS kaynaklarını desteklemek için alt ağ adres aralığında en az 3-5 kullanılabilir IP adresiolmalıdır.
* Azure AD DS'yi dağıtmak için *Ağ Geçidi* alt netini seçmeyin. Azure AD DS'yi ağ *geçidi* alt ağına dağıtmak desteklenmez.
* Alt ağa başka sanal makineler dağıtmayın. Uygulamalar ve VM'ler genellikle bağlantıyı güvence altına almak için ağ güvenlik gruplarını kullanır. Bu iş yüklerini ayrı bir alt ağda çalıştırmak, yönetilen etki alanınıza bağlantıyı bozmadan bu ağ güvenlik gruplarını uygulamanıza olanak tanır.
* Azure AD DS'yi etkinleştirdikten sonra yönetilen etki alanınızı farklı bir sanal ağa taşıyamazsınız.

Sanal ağı planlama ve yapılandırma hakkında daha fazla bilgi için [Azure Active Directory Domain Services için ağ la][network-considerations]ilgili hususlara bakın.

*Ağ* penceresindeki alanları aşağıdaki gibi tamamlayın:

1. **Ağ** sayfasında, açılan menüden Azure AD DS'yi dağıtmak için sanal bir ağ seçin veya **yeni oluştur'u**seçin.
    1. Sanal ağ oluşturmayı seçerseniz, *myVnet*gibi sanal ağ için bir ad girin, ardından *10.0.1.0/24*gibi bir adres aralığı sağlayın.
    1. *DomainServices*gibi açık bir ada sahip özel bir alt ağ oluşturun. *10.0.1.0/24*gibi bir adres aralığı sağlayın.

    [![](./media/tutorial-create-instance-advanced/create-vnet.png "Create a virtual network and subnet for use with Azure AD Domain Services")](./media/tutorial-create-instance-advanced/create-vnet-expanded.png#lightbox)

    Özel IP adresi aralığınızda bulunan bir adres aralığı seçtiğinizden emin olun. Ortak adres alanında bulunan sahip olmadığınız IP adresi aralıkları Azure AD DS'de hatalara neden olur.

1. *DomainServices*gibi bir sanal ağ alt ağı seçin.
1. Hazır **olduğunda, Sonraki seçin - Yönetim**.

## <a name="configure-an-administrative-group"></a>Yönetim grubunu yapılandırma

Azure AD DS etki alanının yönetimi için *AAD DC Yöneticileri* adlı özel bir yönetim grubu kullanılır. Bu grubun üyelerine, yönetilen etki alanına etki alanına katılan VM'lerde yönetim izinleri verilir. Etki alanına katılan VM'lerde bu grup yerel yöneticiler grubuna eklenir. Bu grubun üyeleri, etki alanına katılan VM'lere uzaktan bağlanmak için Uzak Masaüstü'nü de kullanabilir.

Azure AD DS'yi kullanarak yönetilen bir etki alanında *Etki Alanı Yöneticisi* veya Kurumsal *Yönetici* izinleri yoktur. Bu izinler hizmet tarafından ayrılmıştır ve kiracı içindeki kullanıcılar tarafından kullanılamaz. Bunun yerine, *AAD DC Yöneticileri* grubu bazı ayrıcalıklı işlemleri gerçekleştirmenize olanak tanır. Bu işlemler, etki alanı birleştirilmiş VM'lerde yönetim grubuna ait olmayı ve Grup İlkesi'ni yapılandırmayı içerir.

Sihirbaz, Azure AD dizininizde Otomatik olarak *AAD DC Yöneticileri* grubunu oluşturur. Azure REKLAM dizininizde bu ada sahip varolan bir grubunuzun varsa, sihirbaz bu grubu seçer. Dağıtım işlemi sırasında bu *AAD DC Yöneticileri* grubuna isteğe bağlı olarak ek kullanıcılar eklemeyi seçebilirsiniz. Bu adımlar daha sonra tamamlanabilir.

1. Bu *AAD DC Yöneticileri* grubuna ek kullanıcılar eklemek için **grup üyeliğini yönet'i**seçin.

    ![AAD DC Yöneticileri grubunun grup üyeliğini yapılandırma](./media/tutorial-create-instance-advanced/admin-group.png)

1. Üye **Ekle** düğmesini seçin, ardından Azure AD dizininizdeki kullanıcıları arayın ve seçin. Örneğin, kendi hesabınızı arayın ve *AAD DC Yöneticileri* grubuna ekleyin.
1. İstenirseniz, Azure AD DS yönetilen etki alanında dikkat gerektiren uyarılar olduğunda bildirimler için ek alıcılar değiştirin veya ekleyin.
1. Hazır **olduğunda, İleri - Eşitleme'yi**seçin.

## <a name="configure-synchronization"></a>Eşitlemeyi yapılandırma

Azure AD DS, Azure AD'de kullanılabilen *tüm* kullanıcıları ve grupları veya yalnızca belirli grupların *kapsamlı* eşitlemesini eşitlemenize olanak tanır. *Tüm* kullanıcıları ve grupları eşitlemeyi seçerseniz, daha sonra yalnızca kapsamlı bir eşitleme gerçekleştirmeyi seçemezsiniz. Kapsamlı eşitleme hakkında daha fazla bilgi için [Azure AD Etki Alanı Hizmetleri kapsamlı eşitleme][scoped-sync]bilgisine bakın.

1. Bu öğretici **için, tüm** kullanıcıları ve grupları eşitlemeyi seçin. Bu eşitleme seçeneği varsayılan seçenektir.

    ![Azure AD'deki kullanıcıların ve grupların tam senkronizasyonunu gerçekleştirin](./media/tutorial-create-instance-advanced/sync-all.png)

1. **İncele ve oluştur**’u seçin.

## <a name="deploy-the-managed-domain"></a>Yönetilen etki alanını dağıtma

Sihirbazın **Özet** sayfasında, yönetilen etki alanının yapılandırma ayarlarını gözden geçirin. Değişiklik yapmak için sihirbazın herhangi bir adımına geri dönebilirsiniz. Azure AD DS yönetilen bir etki alanını, bu yapılandırma seçeneklerini kullanarak tutarlı bir şekilde farklı bir Azure AD kiracısına yeniden dağıtmak **için, otomasyon için bir şablon da indirebilirsiniz.**

1. Yönetilen etki alanını oluşturmak için **Oluştur'u**seçin. Azure AD DS yönetimi oluşturulduktan sonra DNS adı veya sanal ağ gibi belirli yapılandırma seçeneklerinin değiştirilemeyecek şekilde bir not görüntülenir. Devam etmek için **Tamam'ı**seçin.
1. Yönetilen etki alanınızı sağlama işlemi bir saat kadar sürebilir. Portalda, Azure AD DS dağıtımınızın ilerlemesini gösteren bir bildirim görüntülenir. Dağıtım için ayrıntılı ilerleme yi görmek için bildirimi seçin.

    ![Dağıtımın Azure portalında bildirim devam ediyor](./media/tutorial-create-instance-advanced/deployment-in-progress.png)

1. *MyResourceGroup*gibi kaynak grubunuzu seçin ve *ardından aaddscontoso.com*gibi Azure kaynakları listesinden Azure AD DS örneğinizi seçin. **Genel Bakış** sekmesi, yönetilen etki alanının şu anda *dağıtmakta*olduğunu gösterir. Yönetilen etki alanını tam olarak sağlanana kadar yapılandıramazsınız.

    ![Sağlama durumunda Etki Alanı Hizmetleri durumu](./media/tutorial-create-instance-advanced/provisioning-in-progress.png)

1. Yönetilen etki alanı tam olarak sağlandığında, **Genel Bakış** sekmesi etki alanı durumunu *Çalıştırma*olarak gösterir.

    ![Başarıyla sağlandıktan sonra Alan Adı Hizmetleri durumu](./media/tutorial-create-instance-advanced/successfully-provisioned.png)

Yönetilen etki alanı, Azure AD kiracınızla ilişkilidir. Sağlama işlemi sırasında Azure AD DS, Azure AD kiracısında *Etki Alanı Denetleyici Hizmetleri* ve *AzureActiveDirectoryDomainControllerServices* adlı iki Kurumsal Uygulama oluşturur. Yönetilen etki alanınıza hizmet etmek için bu Kurumsal Uygulamalar gereklidir. Bu uygulamaları silmeyin.

## <a name="update-dns-settings-for-the-azure-virtual-network"></a>Azure sanal ağı için DNS ayarlarını güncelleştirme

Azure AD DS'nin başarıyla dağıtılmasıyla, artık sanal ağı diğer bağlı Sanal Aygıtların ve uygulamaların yönetilen etki alanını kullanmasına izin verecek şekilde yapılandırın. Bu bağlantıyı sağlamak için, sanal ağınızın DNS sunucu ayarlarını azure AD DS'nin dağıtıldığı iki IP adresini işaret etmek için güncelleştirin.

1. Yönetilen etki alanınız için **Genel Bakış** sekmesi bazı Gerekli **yapılandırma adımlarını**gösterir. İlk yapılandırma adımı, sanal ağınız için DNS sunucu ayarlarını güncelleştirmektir. DNS ayarları doğru şekilde yapılandırıldıktan sonra bu adım artık gösterilmez.

    Listelenen adresler, sanal ağda kullanılmak üzere etki alanı denetleyicileridir. Bu örnekte, bu adresler *10.1.0.4* ve *10.1.0.5'tir.* Bu IP adreslerini daha sonra **Özellikler** sekmesinde bulabilirsiniz.

    ![Azure AD Etki Alanı Hizmetleri IP adresleriyle sanal ağınız için DNS ayarlarını yapılandırın](./media/tutorial-create-instance-advanced/configure-dns.png)

1. Sanal ağ için DNS sunucu ayarlarını güncelleştirmek için **Yapıl** düğmesini seçin. DNS ayarları sanal ağınız için otomatik olarak yapılandırılır.

> [!TIP]
> Önceki adımlarda varolan bir sanal ağı seçtiyseniz, ağa bağlı tüm VM'ler yalnızca yeniden başlatmadan sonra yeni DNS ayarlarını alır. Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanarak VM'leri yeniden başlatabilirsiniz.

## <a name="enable-user-accounts-for-azure-ad-ds"></a>Azure AD DS için kullanıcı hesaplarını etkinleştirme

Azure AD DS, yönetilen etki alanında kullanıcıların kimliğini doğrulamak için NT LAN Yöneticisi (NTLM) ve Kerberos kimlik doğrulaması için uygun bir biçimde parola hashes'e ihtiyaç duyar. Azure AD, kiracınız için Azure AD DS'yi etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gerekli biçimde parola açıkları oluşturmaz veya depolamaz. Azure AD, güvenlik nedeniyle parola kimlik bilgilerini açık metin biçiminde de depolamaz. Bu nedenle, Azure AD bu NTLM veya Kerberos parola hatalarını kullanıcıların varolan kimlik bilgilerini temel alarak otomatik olarak oluşturamaz.

> [!NOTE]
> Uygun şekilde yapılandırıldıktan sonra, kullanılabilir parola varoşları Azure AD DS yönetilen etki alanında depolanır. Azure AD DS yönetilen etki alanını silerseniz, bu noktada depolanan parola varoşları da silinir. Azure AD'de senkronize edilmiş kimlik bilgisi bilgileri daha sonra bir Azure AD DS yönetilen etki alanı oluşturursanız yeniden kullanılamaz - parola karma eşitlemeyi yeniden yapılandırmanız gerekir. Daha önce etki alanına katılan VM'ler veya kullanıcılar hemen kimlik doğrulaması yapamaz - Azure AD'nin parola hatalarını oluşturması ve yeni Azure AD DS yönetilen etki alanında depolaması gerekir. Daha fazla bilgi için Azure [AD DS ve Azure AD Connect için Parola karma eşitleme işlemine][password-hash-sync-process]bakın.

Bu parola kalıplarını oluşturma ve depolama adımları, Azure AD Connect'i kullanarak şirket içi dizininizden senkronize edilen yalnızca Azure AD'da oluşturulan yalnızca bulut kullanıcı hesapları için farklıdır. Yalnızca bulutta yer alan bir kullanıcı hesabı, Azure portal veya Azure AD PowerShell cmdlet’leri kullanılarak Azure AD dizininizde oluşturulmuş bir hesaptır. Bu kullanıcı hesapları şirket içi dizinden eşitlenmez. Bu öğreticide, yalnızca buluta özel temel bir kullanıcı hesabıyla çalışalım. Azure AD Connect'i kullanmak için gereken ek adımlar hakkında daha fazla bilgi için, [şirket içi AD'nizden yönetilen etki alanınızla senkronize edilen kullanıcı hesapları için parola gruplarını eşitle'ye][on-prem-sync]bakın.

> [!TIP]
> Azure AD kiracınızda şirket içi REKLAMınızdan yalnızca bulut kullanıcıları ve kullanıcıların bir birleşimi varsa, her iki adımı da tamamlamanız gerekir.

Yalnızca bulutkullanıcıları hesapları için kullanıcıların Azure AD DS'yi kullanabilmeleri için parolalarını değiştirmeleri gerekir. Bu parola değiştirme işlemi, Kerberos ve NTLM kimlik doğrulamasının parola işlenmelerinin Azure AD'de oluşturulmasına ve depolanmasına neden olur. Bir sonraki oturum açmada parola değişikliğini zorlayan Azure AD DS'yi kullanması gereken kiracıdaki tüm kullanıcıların parolalarının süresi dolabilir veya parolalarını el ile değiştirmelerini emredebilirsiniz. Bu öğretici için, bir kullanıcı parolası el ile değiştirelim.

Bir kullanıcı parolasını sıfırlamadan önce, Azure AD kiracısının [self servis parola sıfırlama için yapılandırılması][configure-sspr]gerekir.

Yalnızca bulut kullanıcının parolasını değiştirmek için, kullanıcının aşağıdaki adımları tamamlaması gerekir:

1. 'deki Azure AD Erişim [https://myapps.microsoft.com](https://myapps.microsoft.com)Paneli sayfasına gidin.
1. Sağ üst köşede adınızı seçin ve açılan menüden **Profil'i** seçin.

    ![Profil seçme](./media/tutorial-create-instance-advanced/select-profile.png)

1. **Profil** sayfasında **parolayı değiştir'i**seçin.
1. **Parolayı Değiştir** sayfasında, mevcut (eski) parolanızı girin ve ardından yeni bir parola girin ve onaylayın.
1. **Gönder**’i seçin.

Yeni parolanın Azure AD DS'de kullanılabilir olması ve yönetilen etki alanına katılan bilgisayarlarda başarılı bir şekilde oturum açabilmesi için parolanızı değiştirmeniz birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönetilen bir etki alanı için DNS ve sanal ağ ayarlarını yapılandırma
> * Azure AD DS örneği oluşturma
> * Etki alanı yönetimine yönetim kullanıcıları ekleme
> * Azure AD DS için kullanıcı hesaplarını etkinleştirin ve parola kalıpları oluşturun

Yönetilen bu etki alanını iş başında görmek için, etki alanına sanal bir makine oluşturun ve katılın.

> [!div class="nextstepaction"]
> [Yönetilen etki alanınızda bir Windows Server sanal makinesine katılma](join-windows-vm.md)

<!-- INTERNAL LINKS -->
[tutorial-create-instance]: tutorial-create-instance.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[network-considerations]: network-considerations.md
[create-dedicated-subnet]: ../virtual-network/virtual-network-manage-subnet.md#add-a-subnet
[scoped-sync]: scoped-synchronization.md
[on-prem-sync]: tutorial-configure-password-hash-sync.md
[configure-sspr]: ../active-directory/authentication/quickstart-sspr.md
[password-hash-sync-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[resource-forests]: concepts-resource-forest.md
[availability-zones]: ../availability-zones/az-overview.md
[concepts-sku]: administration-concepts.md#azure-ad-ds-skus

<!-- EXTERNAL LINKS -->
