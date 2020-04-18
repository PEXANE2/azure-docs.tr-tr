---
title: Azure AD Etki Alanı Hizmetleri için DNS'yi yönetme | Microsoft Dokümanlar
description: DNS'yi yönetmek ve Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanı için koşullu iletmeler oluşturmak için DNS Sunucu Araçlarını nasıl yükleyin.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 04/16/2020
ms.author: iainfou
ms.openlocfilehash: f4bd3f75c3246cb11e88dbaae817eba8ac76b394
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603528"
---
# <a name="administer-dns-and-create-conditional-forwarders-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanında DNS'yi yönetin ve koşullu iletmeler oluşturun

Azure Etkin Dizin Etki Alanı Hizmetlerinde (Azure AD DS) önemli bir bileşen DNS (Etki Alanı Adı Çözümü) bileşendir. Azure AD DS, yönetilen etki alanı için ad çözümlemesi sağlayan bir DNS sunucusu içerir. Bu DNS sunucusu, hizmetin çalışmasına izin veren önemli bileşenler için yerleşik DNS kayıtları ve güncelleştirmeleri içerir.

Kendi uygulamalarınızı ve hizmetlerinizi çalıştırırken, etki alanına katılmayan makineler için DNS kayıtları oluşturmanız, yük dengeleyicileri için sanal IP adreslerini yapılandırmanız veya harici DNS iletmeleri ayarlamanız gerekebilir. *AAD DC Yöneticileri* grubuna ait kullanıcılara Azure AD DS yönetilen etki alanında DNS yönetim ayrıcalıkları verilir ve özel DNS kayıtları oluşturabilir ve edinebilir.

Karma bir ortamda, şirket içi AD DS ortamı gibi diğer DNS ad alanlarında yapılandırılan DNS bölgeleri ve kayıtları Azure AD DS ile eşitlenmez. Diğer DNS ad alanlarında ki adlandırılmış kaynakları çözmek için, ortamınızdaki varolan DNS sunucularını işaret eden koşullu iletmeler oluşturun ve kullanın.

Bu makalede, DNS Server araçlarını nasıl yükleyip sonra kayıtları yönetmek ve Azure AD DS'de koşullu iletmeler oluşturmak için DNS konsolunu nasıl kullanacağınızı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
* Azure AD DS sanal ağınızdaki bağlantı, diğer DNS ad alanlarınızın barındırıldığı yere kadar.
    * Bu bağlantı, [Azure ExpressRoute][expressroute] veya [Azure VPN Ağ Geçidi][vpn-gateway] bağlantısıyla sağlanabilir.
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
    * Gerekirse, bir Windows [Server VM oluşturmak ve yönetilen bir etki alanına katılmak için][create-join-windows-vm]öğreticiyi tamamlayın.
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="install-dns-server-tools"></a>DNS Server araçlarını yükleme

Azure AD DS'de DNS kayıtları oluşturmak ve değiştirmek için DNS Server araçlarını yüklemeniz gerekir. Bu araçlar Windows Server'da bir özellik olarak yüklenebilir. Bir Windows istemcisine yönetim araçlarının nasıl yüklenirhakkında daha fazla bilgi için, [uzaktan sunucu yönetim araçlarını (RSAT)][install-rsat]yükle'ye bakın.

1. Yönetim VM'nizde oturum açın. Azure portalını kullanarak bağlanma adımlarını öğrenmek [için windows server vm'ye bağlan'a][connect-windows-server-vm]bakın.
1. VM'de oturum açtığınızda **Sunucu Yöneticisi** varsayılan olarak açılmıyorsa, **Başlat** menüsünü seçin ve ardından **Server Manager'ı**seçin.
1. **Sunucu Yöneticisi** penceresinin *Pano* bölmesinde, Rol ve **Özellikler Ekle'yi**seçin.
1. Roller ve Özellikler Ekle *Sihirbazı'nın*Başlamadan **Önce** sayfasında **İleri'yi**seçin.
1. Yükleme *Türü*için **Rol tabanlı veya özellik tabanlı yükleme** seçeneğini işaretli bırakın ve **İleri'yi**seçin.
1. Sunucu **Seçimi** sayfasında, sunucu havuzundan geçerli VM'yi seçin( *örneğin myvm.aaddscontoso.com*, sonra **İleri'yi**seçin.
1. Sunucu **Rolleri** sayfasında **İleri'yi**tıklatın.
1. **Özellikler** sayfasında, Uzak **Sunucu Yönetim Araçları** düğümlerini genişletin ve ardından Rol **Yönetim Araçları** düğümlerini genişletin. Rol yönetim araçları listesinden **DNS Server Tools** özelliğini seçin.

    ![Kullanılabilir rol yönetim araçları listesinden DNS Sunucu Araçlarını yüklemeyi seçin](./media/manage-dns/install-dns-tools.png)

1. **Onay** sayfasında **Yükle'yi**seçin. Grup İlkesi Yönetimi araçlarını yüklemek bir veya iki dakika sürebilir.
1. Özellik yükleme tamamlandığında, Rol ve **Özellikler Ekle** sihirbazı'ndan çıkmak için **Kapat'ı** seçin.

## <a name="open-the-dns-management-console-to-administer-dns"></a>DNS'yi yönetmek için DNS yönetim konsolu'nu açın

DNS Server araçları yüklüolduğu için, Azure AD DS yönetilen etki alanında DNS kayıtlarını yönetebilirsiniz.

> [!NOTE]
> DNS'yi Azure AD DS yönetilen bir etki alanında yönetmek için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Önceki bölümde yüklü **DNS** de dahil olmak üzere kullanılabilir yönetim araçlarının listesi gösterilir. DNS Yönetim konsolu başlatmak için **DNS'yi** seçin.
1. **DNS Server'a Bağlan** iletişim kutusunda aşağıdaki **bilgisayarı**seçin, ardından yönetilen etki alanının DNS etki alanı adını girin, örneğin *aaddscontoso.com:*

    ![DNS konsolundaki Azure AD DS yönetilen etki alanına bağlanın](./media/manage-dns/connect-dns-server.png)

1. DNS Konsolu belirtilen Azure AD DS yönetilen etki alanına bağlanır. Gerekli DNS girişlerinizi oluşturmak veya gerektiğinde varolan kayıtları yeniden deletmek için **İleri Arama Bölgelerini** veya Ters Arama **Bölgelerini** genişletin.

    ![DNS Konsolu - etki alanını yönet](./media/manage-dns/dns-manager.png)

> [!WARNING]
> DNS Server araçlarını kullanarak kayıtları yönetirken, Azure AD DS tarafından kullanılan yerleşik DNS kayıtlarını silmediğinizden veya değiştirmediğinizden emin olun. Yerleşik DNS kayıtları etki alanı DNS kayıtları, ad sunucusu kayıtları ve DC konumu için kullanılan diğer kayıtları içerir. Bu kayıtları değiştirirseniz, etki alanı hizmetleri sanal ağda kesintiye uğrar.

## <a name="create-conditional-forwarders"></a>Koşullu iletmeler oluşturma

Azure AD DS DNS bölgesi yalnızca yönetilen etki alanının bulunduğu bölge ve kayıtları içermelidir. Diğer DNS ad alanlarındaki adlandırılmış kaynakları gidermek için Azure AD DS'de ek bölgeler oluşturmayın. Bunun yerine, Azure AD DS yönetilen etki alanında koşullu iletmeciler ikullanın ve dns sunucusuna bu kaynakların adreslerini çözmek için nereye gideceğini söyleyin.

Koşullu iletme, DNS sunucusunda sorguları iletmek için *contoso.com*gibi bir DNS etki alanı tanımlamanıza olanak tanıyan bir yapılandırma seçeneğidir. Yerel DNS sunucusu nun bu etki alanındaki kayıtların sorgularını çözmeye çalışması yerine, DNS sorguları bu etki alanı için yapılandırılan DNS'ye iletilir. Bu yapılandırma, azure AD DS yönetilen etki alanında yinelenen kayıtların bulunduğu yerel bir DNS bölgesi oluşturmadığınız dan bu kaynakları yansıtacak şekilde doğru DNS kayıtlarının döndürülmesini sağlar.

Azure AD DS yönetilen etki alanınızda koşullu bir iletme oluşturmak için aşağıdaki adımları tamamlayın:

1. *aaddscontoso.com*.vb gibi Azure AD DS DNS bölgenizi seçin
1. **Koşullu İletmeciler'i**seçin, ardından sağdan seçin ve **Yeni Koşullu İletmeci'yi seçin...**
1. *contoso.com*gibi diğer **DNS Etki Alanınızı**girin ve aşağıdaki örnekte gösterildiği gibi, bu ad alanı için DNS sunucularının IP adreslerini girin:

    ![DNS sunucusu için koşullu bir iletme ekleme ve yapılandırma](./media/manage-dns/create-conditional-forwarder.png)

1. **Bu koşullu iletmeyi Etkin Dizini'nde Depola**kutusunu işaretleyin ve aşağıdaki gibi çoğaltın, ardından aşağıdaki örnekte gösterildiği gibi *bu etki alanındaki Tüm DNS sunucuları*için seçeneği seçin:

    ![DNS Konsolu - etki alanını yönet](./media/manage-dns/store-in-domain.png)

    > [!IMPORTANT]
    > Koşullu iletme etki *alanı*yerine *ormanda* depolanırsa, koşullu iletme başarısız olur.

1. Koşullu iletme oluşturmak için **Tamam'ı**seçin.

Azure AD DS yönetilen etki alanına bağlı VM'lerden diğer ad alanlarındaki kaynakların ad çözümlemesi artık doğru şekilde çözülmelidir. Koşullu iletmede yapılandırılan DNS etki alanı sorguları ilgili DNS sunucularına aktarılır.

## <a name="next-steps"></a>Sonraki adımlar

DNS yönetimi hakkında daha fazla bilgi için [Technet'teki DNS araçları makalesine](https://technet.microsoft.com/library/cc753579.aspx)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh
