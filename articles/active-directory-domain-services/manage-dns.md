---
title: Azure AD Etki Alanı Hizmetleri için DNS'yi yönetme | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri yönetilen bir etki alanı için DNS'yi yönetmek için DNS Sunucu Araçlarını nasıl yükleyin.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 2694a5f250b746748a1b42ac4d211aa28ef1ebad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613700"
---
# <a name="administer-dns-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanında DNS'yi yönetme

Azure Etkin Dizin Etki Alanı Hizmetlerinde (Azure AD DS) önemli bir bileşen DNS (Etki Alanı Adı Çözümü) bileşendir. Azure AD DS, yönetilen etki alanı için ad çözümlemesi sağlayan bir DNS sunucusu içerir. Bu DNS sunucusu, hizmetin çalışmasına izin veren önemli bileşenler için yerleşik DNS kayıtları ve güncelleştirmeleri içerir.

Kendi uygulamalarınızı ve hizmetlerinizi çalıştırırken, etki alanına katılmayan makineler için DNS kayıtları oluşturmanız, yük dengeleyicileri için sanal IP adreslerini yapılandırmanız veya harici DNS iletmeleri ayarlamanız gerekebilir. *AAD DC Yöneticileri* grubuna ait kullanıcılara Azure AD DS yönetilen etki alanında DNS yönetim ayrıcalıkları verilir ve özel DNS kayıtları oluşturabilir ve edinebilir.

Karma bir ortamda, şirket içi AD DS ortamında yapılandırılan DNS bölgeleri ve kayıtları Azure AD DS ile eşitlenmez. Kendi DNS girişlerinizi tanımlamak ve kullanmak için Azure AD DS DNS sunucusunda kayıtlar oluşturun veya ortamınızdaki varolan DNS sunucularını gösteren koşullu iletmeler kullanın.

Bu makalede, DNS Server araçlarını nasıl yükleyip sonra Azure AD DS'deki kayıtları yönetmek için DNS konsoluna nasıl kullanacağınızı gösterilmektedir.

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

    ![Kullanılabilir rol yönetim araçları listesinden DNS Sunucu Araçlarını yüklemeyi seçin](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

1. **Onay** sayfasında **Yükle'yi**seçin. Grup İlkesi Yönetimi araçlarını yüklemek bir veya iki dakika sürebilir.
1. Özellik yükleme tamamlandığında, Rol ve **Özellikler Ekle** sihirbazı'ndan çıkmak için **Kapat'ı** seçin.

## <a name="open-the-dns-management-console-to-administer-dns"></a>DNS'yi yönetmek için DNS yönetim konsolu'nu açın

DNS Server araçları yüklüolduğu için, Azure AD DS yönetilen etki alanında DNS kayıtlarını yönetebilirsiniz.

> [!NOTE]
> DNS'yi Azure AD DS yönetilen bir etki alanında yönetmek için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Önceki bölümde yüklü **DNS** de dahil olmak üzere kullanılabilir yönetim araçlarının listesi gösterilir. DNS Yönetim konsolu başlatmak için **DNS'yi** seçin.
1. **DNS Server'a Bağlan** iletişim kutusunda aşağıdaki **bilgisayarı**seçin, ardından yönetilen etki alanının DNS etki alanı adını girin, örneğin *aaddscontoso.com:*

    ![DNS konsolundaki Azure AD DS yönetilen etki alanına bağlanın](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

1. DNS Konsolu belirtilen Azure AD DS yönetilen etki alanına bağlanır. Gerekli DNS girişlerinizi oluşturmak veya gerektiğinde varolan kayıtları yeniden deletmek için **İleri Arama Bölgelerini** veya Ters Arama **Bölgelerini** genişletin.

    ![DNS Konsolu - etki alanını yönet](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

> [!WARNING]
> DNS Server araçlarını kullanarak kayıtları yönetirken, Azure AD DS tarafından kullanılan yerleşik DNS kayıtlarını silmediğinizden veya değiştirmediğinizden emin olun. Yerleşik DNS kayıtları etki alanı DNS kayıtları, ad sunucusu kayıtları ve DC konumu için kullanılan diğer kayıtları içerir. Bu kayıtları değiştirirseniz, etki alanı hizmetleri sanal ağda kesintiye uğrar.

## <a name="next-steps"></a>Sonraki adımlar

DNS yönetimi hakkında daha fazla bilgi için [Technet'teki DNS araçları makalesine](https://technet.microsoft.com/library/cc753579.aspx)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh
