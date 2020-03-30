---
title: Azure AD Etki Alanı Hizmetlerinde grup ilkesi oluşturma ve yönetme | Microsoft Dokümanlar
description: Yerleşik grup ilkesi nesnelerini (GDO' lar) nasıl dizeedin ve Azure Active Directory Domain Services yönetilen etki alanında kendi özel ilkelerinizi nasıl oluşturabileceğinizi öğrenin.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: iainfou
ms.openlocfilehash: bce71355eef19ec3cc85525033274f57b1a3e0b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78946417"
---
# <a name="administer-group-policy-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanında Grup İlkesi'ni yönetme

Azure Etkin Dizin Etki Alanı Hizmetleri'ndeki (Azure AD DS) kullanıcı ve bilgisayar nesnelerinin ayarları genellikle Grup İlkesi Nesneleri (GPO) kullanılarak yönetilir. Azure AD DS, *AADDC Kullanıcıları* ve *AADDC Bilgisayar* kapsayıcıları için yerleşik GPO'lar içerir. Bu yerleşik GPO'ları, ortamınız için gerektiği gibi Grup İlkesi'ni yapılandırmak için özelleştirebilirsiniz. *Azure AD DC yöneticileri* grubunun üyeleri, Azure AD DS etki alanında Grup İlkesi yönetim ayrıcalıklarına sahiptir ve özel GPO'lar ve kuruluş birimleri (OSB) oluşturabilir. Grup İlkesi'nin ne olduğu ve nasıl çalıştığı hakkında daha fazla bilgi için [Bkz. Grup İlkesi genel bakış.][group-policy-overview]

Karma bir ortamda, şirket içi AD DS ortamında yapılandırılan grup ilkeleri Azure AD DS ile eşitlenmez. Azure AD DS'deki kullanıcılar veya bilgisayarlar için yapılandırma ayarlarını tanımlamak için varsayılan GPO'lardan birini düzenleme veya özel bir GPO oluşturun.

Bu makalede, Grup İlkesi Yönetimi araçlarını nasıl yükleyip, yerleşik GPO'ları nasıl yükleyip özel GPO'lar oluşturabileceğinizi gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
    * Gerekirse, bir Windows [Server VM oluşturmak ve yönetilen bir etki alanına katılmak için][create-join-windows-vm]öğreticiyi tamamlayın.
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

> [!NOTE]
> Yeni şablonları yönetim iş istasyonuna kopyalayarak Grup İlkesi Yönetim Şablonlarını kullanabilirsiniz. *.admx* `%SYSTEMROOT%\PolicyDefinitions` dosyalarını yereldosyalara özel *.adml* dosyalarını `%SYSTEMROOT%\PolicyDefinitions\[Language-CountryRegion]` `Language-CountryRegion` *.adml* dosyalarının dili ve bölgesiyle eşleştiği şekilde kopyalayın.
>
> Örneğin, *.adml* dosyalarının İngilizce, Abd sürümünü klasöre kopyalayın. `\en-us`
>
> Alternatif olarak, Grup İlkesi Yönetim Şablonunuzu Azure AD DS yönetilen etki alanının bir parçası olan etki alanı denetleyicilerinde merkezi olarak depolayabilirsiniz. Daha fazla bilgi [için, Windows'da Grup İlkesi Yönetim Şablonları için Merkez Mağaza'yı nasıl oluşturup yönetebilirsiniz'e](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra)bakın.

## <a name="install-group-policy-management-tools"></a>Grup İlke Yönetimi araçlarını yükleme

Grup İlkesi Nesnesi (GPO) oluşturmak ve yapılandırmak için Grup İlkesi Yönetimi araçlarını yüklemeniz gerekir. Bu araçlar Windows Server'da bir özellik olarak yüklenebilir. Bir Windows istemcisine yönetim araçlarının nasıl yüklenirhakkında daha fazla bilgi için, [uzaktan sunucu yönetim araçlarını (RSAT)][install-rsat]yükle'ye bakın.

1. Yönetim VM'nizde oturum açın. Azure portalını kullanarak bağlanma adımlarını öğrenmek [için windows server vm'ye bağlan'a][connect-windows-server-vm]bakın.
1. VM'de oturum açtığınızda **Sunucu Yöneticisi** varsayılan olarak açılmalıdır. Değilse, **Başlat** menüsünde **Server Manager'ı**seçin.
1. **Sunucu Yöneticisi** penceresinin *Pano* bölmesinde, Rol ve **Özellikler Ekle'yi**seçin.
1. Roller ve Özellikler Ekle *Sihirbazı'nın*Başlamadan **Önce** sayfasında **İleri'yi**seçin.
1. Yükleme *Türü*için **Rol tabanlı veya özellik tabanlı yükleme** seçeneğini işaretli bırakın ve **İleri'yi**seçin.
1. Sunucu **Seçimi** sayfasında, sunucu havuzundan geçerli VM'yi seçin( *örneğin myvm.aaddscontoso.com*, sonra **İleri'yi**seçin.
1. Sunucu **Rolleri** sayfasında **İleri'yi**tıklatın.
1. **Özellikler** sayfasında **Grup İlkesi Yönetimi** özelliğini seçin.

    ![Özellikler sayfasından 'Grup İlkesi Yönetimi'ni yükleme](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)

1. **Onay** sayfasında **Yükle'yi**seçin. Grup İlkesi Yönetimi araçlarını yüklemek bir veya iki dakika sürebilir.
1. Özellik yükleme tamamlandığında, Rol ve **Özellikler Ekle** sihirbazı'ndan çıkmak için **Kapat'ı** seçin.

## <a name="open-the-group-policy-management-console-and-edit-an-object"></a>Grup İlkesi Yönetim Konsolu'nu açın ve bir nesneyi

Varsayılan grup ilkesi nesneleri (GPO'lar), Azure AD DS yönetilen etki alanında kullanıcılar ve bilgisayarlar için vardır. Önceki bölümden yüklenen Grup İlkesi Yönetimi özelliğiyle, varolan bir GPO'yu görüntüleyelim ve güncelleyelim. Sonraki bölümde, özel bir GPO oluşturursunuz.

> [!NOTE]
> Grup ilkesini Azure AD DS yönetilen bir etki alanında yönetmek için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Önceki bölümde yüklü **Grup İlkesi Yönetimi** de dahil olmak üzere kullanılabilir yönetim araçlarının listesi gösterilir.
1. Grup İlkesi Yönetim Konsolu'nu (GPMC) açmak için **Grup İlkesi Yönetimi'ni**seçin.

    ![Grup İlke Yönetimi Konsolu, grup ilkesi nesnelerini yönetmeye hazır açılır](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

Azure AD DS yönetilen etki alanında iki yerleşik Grup İlkesi İlkesi Nesnesi (GPO) vardır - biri *AADDC Computers* kapsayıcısı için, diğeri de *AADDC Kullanıcıları* kapsayıcısı için. Bu GPO'ları, Azure AD DS yönetilen etki alanınızda gerektiğinde grup ilkesini yapılandırmak için özelleştirebilirsiniz.

1. Grup **İlkesi Yönetimi** konsolunda, **Orman: aaddscontoso.com** düğümü genişletin. Ardından, **Etki Alanları** düğümlerini genişletin.

    *AADDC Bilgisayarlar* ve *AADDC Kullanıcıları*için iki yerleşik kapsayıcı bulunmaktadır. Bu kapsayıcıların her biri, bunlara uygulanan varsayılan bir GPO'ya sahiptir.

    ![Varsayılan 'AADDC Bilgisayar' ve 'AADDC Kullanıcıları' kapsayıcılarına uygulanan yerleşik GPO'lar](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)

1. Bu yerleşik GDO'lar, Azure AD DS yönetilen etki alanınızda belirli grup ilkelerini yapılandırmak için özelleştirilebilir. *AADDC Computers GPO*gibi GPO'lardan birini sağ seçin, ardından **Edit'i seçin...**

    ![Yerleşik GPO'lardan birini 'Edit' seçeneğini seçin](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)

1. Grup İlkesi Yönetimi Düzenleyicisi aracı, *Hesap İlkeleri*gibi GPO'yu özelleştirmenize izin vermek için açılır:

    ![Ayarları gerektiği gibi yapılandırmak için GPO'u özelleştirme](./media/active-directory-domain-services-admin-guide/gp-editor.png)

    Tamamlandığında, ilkeyi kaydetmek için **Dosya > Kaydet'i** seçin. Bilgisayarlar varsayılan olarak her 90 dakikada bir Grup İlkesini yeniler ve yaptığınız değişiklikleri uygular.

## <a name="create-a-custom-group-policy-object"></a>Özel bir Grup İlkesi Nesnesi Oluşturma

Benzer ilke ayarlarını gruplandırmak için, genellikle tek, varsayılan GPO'da gerekli tüm ayarları uygulamak yerine ek GPO'lar oluşturursunuz. Azure AD DS ile kendi özel grup ilke nesnelerinizi oluşturabilir veya içe aktarabilir ve bunları özel bir OU'ya bağlayabilirsiniz. Önce özel bir OU oluşturmanız gerekiyorsa, [Azure AD DS yönetilen etki alanında özel bir OU oluşturma'ya](create-ou.md)bakın.

1. Grup **İlkesi Yönetimi** konsolunda, *MyCustomOU*gibi özel kuruluş biriminizi (OU) seçin. Ou'yu sağ seçin ve bu etki alanında bir GPO oluştur'u seçin **ve buraya bağla...**:

    ![Grup İlkesi Yönetimi konsolunda özel bir GPO oluşturma](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)

1. *Özel GPO'm*gibi yeni GPO için bir ad belirtin, ardından **Tamam'ı**seçin. İsteğe bağlı olarak bu özel GPO'ya varolan bir GPO ve ilke seçenekleri kümesini dayandırabilirsiniz.

    ![Yeni özel GPO için bir ad belirtin](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)

1. Özel GPO oluşturulur ve özel OU'nuza bağlanır. Şimdi ilke ayarlarını yapılandırmak için, özel GPO'yu sağ seçin ve **Edit'i seçin...**:

    ![Özel GPO'nuzu 'Edit' seçeneğini seçin](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)

1. **Grup İlkesi Yönetimi** Düzenleyicisi, GPO'yu özelleştirmenize izin vermek için açılır:

    ![Ayarları gerektiği gibi yapılandırmak için GPO'u özelleştirme](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)

    Tamamlandığında, ilkeyi kaydetmek için **Dosya > Kaydet'i** seçin. Bilgisayarlar varsayılan olarak her 90 dakikada bir Grup İlkesini yeniler ve yaptığınız değişiklikleri uygular.

## <a name="next-steps"></a>Sonraki adımlar

Grup İlkesi Yönetim Konsolu'nu kullanarak yapılandırabileceğiniz kullanılabilir Grup İlkesi ayarları hakkında daha fazla bilgi için [Grup İlkesi tercih öğeleriyle Çalışma'ya][group-policy-console]bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[group-policy-overview]: /previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831791(v=ws.11)
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh
[group-policy-console]: /previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn789194(v=ws.11)
