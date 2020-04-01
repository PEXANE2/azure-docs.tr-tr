---
title: Öğretici - Azure Active Directory Etki Alanı Hizmetleri için yönetim VM'si oluşturun | Microsoft Dokümanlar
description: Bu eğitimde, Azure Active Directory Domain Services örneğini yönetmek için kullandığınız bir Windows sanal makinesini nasıl oluşturup yapılandırabileceğinizi öğrenirsiniz.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 09fcf88c6dfe90380f387c6d72c751634f5b1606
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475771"
---
# <a name="tutorial-create-a-management-vm-to-configure-and-administer-an-azure-active-directory-domain-services-managed-domain"></a>Öğretici: Azure Active Directory Etki Alanı Hizmetleri yönetilen bir etki alanını yapılandırmak ve yönetmek için bir yönetim VM'si oluşturun

Azure Active Directory Etki Alanı Hizmetleri (AD DS), etki alanı birleştirme, grup ilkesi, LDAP ve Windows Server Active Directory ile tam uyumlu Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Bu yönetilen etki alanını, şirket içi Active Directory Domain Services etki alanıyla aynı Uzak Sunucu Yönetim Araçları (RSAT) kullanarak yönetirsiniz. Azure AD DS yönetilen bir hizmet olduğundan, etki alanı denetleyicilerine bağlanmak için uzak masaüstü protokolü (RDP) kullanmak gibi gerçekleştiremediğiniz bazı yönetim görevleri vardır.

Bu öğretici, Azure'da bir Windows Server VM oluşturmayı ve Azure AD DS yönetilen bir etki alanını yönetmek için gerekli araçları nasıl yükleydiğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD DS yönetilen etki alanında kullanılabilir yönetim görevlerini anlama
> * Etkin Dizin yönetim araçlarını Windows Server VM'ye yükleme
> * Ortak görevleri gerçekleştirmek için Active Directory Administrative Center'ı kullanın

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için ilk öğreticiye bakın.
* Azure AD DS yönetilen etki alanına katılan bir Windows Server VM.
    * Gerekirse, [bir Windows Server VM oluşturmak ve yönetilen bir etki alanına katılmak için][create-join-windows-vm]önceki öğretici bakın.
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.
* Azure AD DS sanal ağınızda dağıtılan bir Azure Bastion ana bilgisayarı.
    * Gerekirse, [bir Azure Bastion ana bilgisayarı oluşturun.][azure-bastion]

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu eğitimde, Azure portalını kullanarak bir yönetim VM'si oluşturur ve yapılandırırsınız. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="available-administrative-tasks-in-azure-ad-ds"></a>Azure AD DS'de kullanılabilir yönetim görevleri

Azure AD DS, kullanıcılarınızın, uygulamalarınızın ve hizmetlerin tüketilmesi için yönetilen bir etki alanı sağlar. Bu yaklaşım, yapabileceğiniz bazı kullanılabilir yönetim görevlerini ve yönetilen etki alanında hangi ayrıcalıklara sahip olduğunuzu değiştirir. Bu görevler ve izinler, düzenli bir şirket içi Aktif Dizin Etki Alanı Hizmetleri ortamında deneyimlediğinizden farklı olabilir. Ayrıca Uzak Masaüstü'nü kullanarak Azure AD DS yönetilen etki alanında etki alanı denetleyicilerine bağlanamazsınız.

### <a name="administrative-tasks-you-can-perform-on-an-azure-ad-ds-managed-domain"></a>Azure AD DS yönetilen etki alanında gerçekleştirebileceğiniz yönetim görevleri

*AAD DC Yöneticileri* grubunun üyelerine Azure AD DS yönetilen etki alanında aşağıdakiler gibi görevleri yapmalarına olanak tanıyan ayrıcalıklar verilir:

* Yönetilen etki alanında *AADDC Bilgisayarlar* ve *AADDC Kullanıcıları* kapsayıcıları için yerleşik grup ilkesi nesnesini (GPO) yapılandırın.
* Yönetilen etki alanında DNS’yi yönetin.
* Yönetilen etki alanında özel kuruluş birimleri (OSB) oluşturun ve yönetin.
* Yönetilen etki alanına katılan bilgisayarlara yönetici erişimi edinin.

### <a name="administrative-privileges-you-dont-have-on-an-azure-ad-ds-managed-domain"></a>Azure AD DS yönetilen etki alanında sahip olmadığınız yönetim ayrıcalıkları

Azure AD DS yönetilen etki alanı kilitlenir, böylece etki alanında belirli yönetim görevlerini yapmak için ayrıcalıklarınız yoktur. Aşağıdaki örneklerden bazıları yapamayacağınız görevlerdir:

* Yönetilen etki alanının şemasını genişletin.
* Uzak Masaüstü'nü kullanarak yönetilen etki alanı için etki alanı denetleyicilerine bağlanın.
* Yönetilen etki alanına etki alanı denetleyicileri ekleyin.
* Yönetilen etki alanı için *Etki Alanı Yöneticisi* veya Kurumsal *Yönetici* ayrıcalıklarınız yoktur.

## <a name="sign-in-to-the-windows-server-vm"></a>Windows Server VM'de oturum açın

Önceki öğreticide, bir Windows Server VM oluşturuldu ve Azure AD DS yönetilen etki alanına katıldı. Yönetim araçlarını yüklemek için vm'yi kullanalım. Gerekirse, [yönetilen bir etki alanına Windows Server VM oluşturmak ve katılmak için öğreticideki adımları izleyin.][create-join-windows-vm]

> [!NOTE]
> Bu öğreticide, Azure'da Azure AD DS yönetilen etki alanına katılan bir Windows Server VM kullanırsınız. Yönetilen etki alanına birleştirilmiş Windows 10 gibi bir Windows istemcisi de kullanabilirsiniz.
>
> Bir Windows istemcisine yönetim araçlarının nasıl yüklenir hakkında daha fazla bilgi için uzaktan [sunucu yönetim araçlarını (RSAT) yükle'ye](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) bakın

Başlamak için Windows Server VM'ye aşağıdaki gibi bağlanın:

1. Azure portalında, sol **taraftaki Kaynak gruplarını** seçin. *MyResourceGroup*gibi VM'nizin oluşturulduğu kaynak grubunu seçin ve ardından *myVM*gibi VM'yi seçin.
1. VM'niz için **Genel Bakış** bölmesinde **Connect**ve ardından **Bastion'u**seçin.

    ![Azure portalında Bastion'u kullanarak Windows sanal makinesine bağlanma](./media/join-windows-vm/connect-to-vm.png)

1. VM'nizin kimlik bilgilerini girin ve ardından **Bağlan'ı**seçin.

   ![Azure portalındaki Bastion ana bilgisayar aracılığıyla bağlantı kurun](./media/join-windows-vm/connect-to-bastion.png)

Gerekirse, web tarayıcınızın Bastion bağlantısının görüntülenmesi için açılır pencereler açmasına izin verin. VM'inize bağlantı sağlamak birkaç saniye sürer.

## <a name="install-active-directory-administrative-tools"></a>Etkin Dizin yönetim araçlarını yükleme

Azure AD DS yönetilen etki alanları, Etkin Dizin Yönetim Merkezi (ADAC) veya AD PowerShell gibi şirket içi AD DS ortamlarıyla aynı yönetim araçları kullanılarak yönetilir. Bu araçlar, Windows Server ve istemci bilgisayarlardaki Uzak Sunucu Yönetim Araçları (RSAT) özelliğinin bir parçası olarak yüklenebilir. *AAD DC Yöneticileri* grubunun üyeleri, yönetilen etki alanına katılan bir bilgisayardan bu AD yönetim araçlarını uzaktan kullanarak Azure AD DS yönetilen etki alanlarını uzaktan yönetebilir.

Etkin Dizin Yönetimi araçlarını etki alanı birleştirilmiş vm'ye yüklemek için aşağıdaki adımları tamamlayın:

1. VM'de oturum açtığınızda **Sunucu Yöneticisi** varsayılan olarak açılmıyorsa, **Başlat** menüsünü seçin ve ardından **Server Manager'ı**seçin.
1. **Sunucu Yöneticisi** penceresinin *Pano* bölmesinde, Rol ve **Özellikler Ekle'yi**seçin.
1. Roller ve Özellikler Ekle *Sihirbazı'nın*Başlamadan **Önce** sayfasında **İleri'yi**seçin.
1. Yükleme *Türü*için **Rol tabanlı veya özellik tabanlı yükleme** seçeneğini işaretli bırakın ve **İleri'yi**seçin.
1. Sunucu **Seçimi** sayfasında, sunucu havuzundan geçerli VM'yi seçin( *örneğin myvm.aaddscontoso.com*, sonra **İleri'yi**seçin.
1. Sunucu **Rolleri** sayfasında **İleri'yi**tıklatın.
1. **Özellikler** sayfasında, Uzak **Sunucu Yönetim Araçları** düğümlerini genişletin ve ardından Rol **Yönetim Araçları** düğümlerini genişletin.

    Rol yönetim araçları listesinden **AD DS ve AD LDS Araçları** özelliğini seçin ve **ardından İleri'yi**seçin.

    ![Özellikler sayfasından 'AD DS ve AD LDS Araçları'nı yükleyin](./media/tutorial-create-management-vm/install-features.png)

1. **Onay** sayfasında **Yükle'yi**seçin. Yönetim araçlarını yüklemek bir veya iki dakika sürebilir.
1. Özellik yükleme tamamlandığında, Rol ve **Özellikler Ekle** sihirbazı'ndan çıkmak için **Kapat'ı** seçin.

## <a name="use-active-directory-administrative-tools"></a>Active Directory yönetim araçlarını kullanma

Yönetim araçları yüklüolduğu için, Azure AD DS yönetilen etki alanını yönetmek için bunları nasıl kullanacağız görelim. *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabıyla VM'de oturum açmış olduğunuzdan emin olun.

1. **Başlat** menüsünden **Windows Yönetim Araçları'nı**seçin. Önceki adımda yüklenen AD yönetim araçları listelenir.

    ![Sunucuya yüklenen Yönetim Araçları listesi](./media/tutorial-create-management-vm/list-admin-tools.png)

1. **Active Directory Administrative Center'ı**seçin.
1. Azure AD DS yönetilen etki alanını keşfetmek için sol bölmedeki etki alanı adını *(aaddscontoso.com.* *AADDC Computers* ve *AADDC Kullanıcıları* adlı iki kapsayıcı listenin en üstünde yer almaktadır.

    ![Kullanılabilir kapsayıcıları Azure AD DS yönetilen etki alanının bir bölümünü listele](./media/tutorial-create-management-vm/active-directory-administrative-center.png)

1. Azure AD DS yönetilen etki alanına ait kullanıcıları ve grupları görmek için **AADDC Users** konteynerini seçin. Azure AD kiracınızdaki kullanıcı hesapları ve gruplar bu kapsayıcıda listelenir.

    Aşağıdaki örnek çıktıda, *Contoso Admin* adında bir kullanıcı hesabı ve *AAD DC Yöneticileri* grubu bu kapsayıcıda gösterilir.

    ![Etkin Dizin Yönetim Merkezi'ndeki Azure AD DS etki alanı kullanıcılarının listesini görüntüleyin](./media/tutorial-create-management-vm/list-azure-ad-users.png)

1. Azure AD DS yönetilen etki alanına katılan bilgisayarları görmek için **AADDC Computers** kapsayıcısını seçin. *myVM*gibi geçerli sanal makineiçin bir giriş listelenir. Azure AD DS yönetilen etki alanına katılan tüm bilgisayarların bilgisayar hesapları bu *AADDC Computers* kapsayıcısında depolanır.

Kullanıcı hesabı parolalarını sıfırlama veya grup üyeliğini yönetme gibi Ortak Etkin Dizin Yönetim Merkezi eylemleri kullanılabilir. Bu eylemler yalnızca Doğrudan Azure AD DS yönetilen etki alanında oluşturulan kullanıcılar ve gruplar için çalışır. Kimlik bilgileri yalnızca Azure *AD'den* Azure AD DS'ye eşitlenir. Azure AD DS'den Azure AD'ye geri yazma yok. Azure AD'den senkronize edilen kullanıcılar için parolaları veya yönetilen grup üyeliğini değiştiremezsiniz ve bu değişiklikleri yeniden senkronize edemezsiniz.

Azure AD DS yönetilen etki alanınızdaki yaygın eylemleri yönetmek için yönetim araçlarının bir parçası olarak yüklenen *Windows PowerShell için Active Directory Module'i*de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD DS yönetilen etki alanında kullanılabilir yönetim görevlerini anlama
> * Etkin Dizin yönetim araçlarını Windows Server VM'ye yükleme
> * Ortak görevleri gerçekleştirmek için Active Directory Administrative Center'ı kullanın

Azure AD DS yönetilen etki alanınızla güvenli bir şekilde etkileşimde kalmak için güvenli Hafif Dizin Erişim Protokolü 'nü (LDAPS) etkinleştirin.

> [!div class="nextstepaction"]
> [Yönetilen etki alanınız için güvenli LDAP'yi yapılandırma](tutorial-configure-ldaps.md)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
