---
title: 'Azure Active Directory Domain Services: Yönet grup ilkesi | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarında grup ilkesi yönetme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: iainfou
ms.openlocfilehash: c7b32885fdb3cf4f3e584c916d6b234fff54bfc4
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234036"
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen bir etki alanında grup ilkesi yönetme
Azure Active Directory Domain Services, ' AADDC Users ' ve ' AADDC Computers ' kapsayıcıları için yerleşik grup ilkesi nesneleri (GPO 'Lar) içerir. Yönetilen etki alanında grup ilkesi yapılandırmak için bu yerleşik GPO 'Ları özelleştirebilirsiniz. Ayrıca, ' AAD DC Administrators ' grubunun üyeleri yönetilen etki alanında kendi özel OU 'Ları oluşturabilir. Ayrıca özel GPO 'Lar oluşturabilir ve bu özel OU 'Lara bağlanabilir. ' AAD DC Administrators ' grubuna ait kullanıcılara, yönetilen etki alanında yönetim ayrıcalıkları grup ilkesi verilir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](create-instance.md)özetlenen tüm görevleri izleyin.
4. Azure AD Domain Services yönetilen etki alanını yönettiğiniz, **etki alanına katılmış bir sanal makine** . Böyle bir sanal makineniz yoksa, [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)başlıklı makalede özetlenen tüm görevleri izleyin.
5. Yönetilen etki alanınız için grup ilkesi yönetmek için dizininizdeki **' AAD DC yöneticileri ' grubuna ait olan bir kullanıcı hesabının** kimlik bilgilerine ihtiyacınız vardır.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Görev 1-yönetilen etki alanı için grup ilkesi uzaktan yönetmek için etki alanına katılmış bir sanal makine sağlama
Azure AD Domain Services yönetilen etki alanları, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi tanıdık Active Directory yönetim araçları kullanılarak uzaktan yönetilebilir. Benzer şekilde, yönetilen etki alanı için grup ilkesi, grup ilkesi yönetim araçları kullanılarak uzaktan yönetilebilir.

Azure AD dizininizde bulunan Yöneticiler, Uzak Masaüstü aracılığıyla yönetilen etki alanındaki etki alanı denetleyicilerine bağlanma ayrıcalıklarına sahip değildir. ' AAD DC yöneticileri ' grubunun üyeleri, yönetilen etki alanları için grup ilkesi uzaktan yönetebilir. Yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayarında grup ilkesi araçları kullanabilirler. Grup ilkesi araçları, Windows Server 'da ve yönetilen etki alanına katılmış istemci makinelerde grup ilkesi yönetimi isteğe bağlı özelliğinin bir parçası olarak yüklenebilir.

İlk görev, yönetilen etki alanına katılmış bir Windows Server sanal makinesi sağlamak için kullanılır. Yönergeler için [Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)başlıklı makaleye bakın.

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Görev 2-sanal makinede grup ilkesi araçları 'nı yükler
Etki alanına katılmış sanal makineye grup ilkesi yönetim araçlarını yüklemek için aşağıdaki adımları gerçekleştirin.

1. Azure portal gidin. Sol bölmedeki **tüm kaynaklar** ' a tıklayın. Görev 1 ' de oluşturduğunuz sanal makineyi bulun ve tıklayın.
2. Genel Bakış sekmesinde **Bağlan** düğmesine tıklayın. Bir Uzak Masaüstü Protokolü (. rdp) dosyası oluşturulup indirilir.

    ![Windows sanal makinesine Bağlan](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Oturum açma isteminde, ' AAD DC Administrators ' grubuna ait bir kullanıcının kimlik bilgilerini kullanın. Örneğin, bizim örneğimizde 'bob@domainservicespreview.onmicrosoft.com' kullanıyoruz. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için Evet ' i veya devam et ' i tıklatın.
4. Başlangıç ekranından **Sunucu Yöneticisi**açın. Sunucu Yöneticisi penceresinin merkezi bölmesinde **rol ve Özellik Ekle** ' ye tıklayın.

    ![Sanal makinede Sunucu Yöneticisi Başlat](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. **Rol ve Özellik Ekleme Sihirbazı**' nın **başlamadan önce** sayfasında **İleri**' ye tıklayın.

    ![Başlamadan önce sayfasına](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. **Yükleme türü** sayfasında, **rol tabanlı veya özellik tabanlı yükleme** seçeneğini işaretli bırakın ve **İleri**' ye tıklayın.

    ![Yükleme türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. **Sunucu seçimi** sayfasında, Sunucu havuzundan geçerli sanal makineyi seçin ve **İleri**' ye tıklayın.

    ![Sunucu seçim sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. **Sunucu rolleri** sayfasında, **İleri**' ye tıklayın. Sunucuda herhangi bir rol yüklemediğimiz için bu sayfayı atlıyoruz.
9. **Özellikler** sayfasında **Grup İlkesi Yönetimi** özelliğini seçin.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. **Onay** sayfasında, sanal makineye Grup İlkesi Yönetimi özelliğini yüklemek Için, **yükler** ' e tıklayın. Özellik yüklemesi başarıyla tamamlandığında, **rol ve özellik ekleme** Sihirbazı 'ndan çıkmak için **Kapat** ' a tıklayın.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Görev 3-yönetmek için grup ilkesi Management konsolunu başlatın grup ilkesi
Yönetilen etki alanındaki grup ilkesi yönetmek için etki alanına katılmış sanal makinede grup ilkesi yönetim konsolunu kullanabilirsiniz.

> [!NOTE]
> Yönetilen etki alanındaki grup ilkesi yönetmek için ' AAD DC Administrators ' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından **Yönetim Araçları**' na tıklayın. **Grup İlkesi Yönetim** konsolunun sanal makinede yüklü olduğunu görmeniz gerekir.

    ![grup ilkesi yönetimini Başlat](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. **Grup İlkesi Yönetimi** ' ne tıklayarak Grup İlkesi Yönetim konsolunu başlatın.

    ![grup ilkesi konsolu](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Görev 4-yerleşik grup ilkesi nesnelerini özelleştirme
Yönetilen etki alanındaki ' AADDC Computers ' ve ' AADDC Users ' kapsayıcıları için bir tane olmak üzere iki yerleşik grup ilkesi nesnesi (GPO) vardır. Bu GPO 'Ları, yönetilen etki alanında Grup ilkesini yapılandırmak için özelleştirebilirsiniz.

1. Yönetilen etki alanınız için grup ilkelerini görmek üzere **Grup İlkesi Yönetim** konsolunda ormanı genişletmek için tıklatın **: contoso100.com** ve **Domains** düğümleri.

    ![Yerleşik GPO 'Lar](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Bu yerleşik GPO 'Ları, yönetilen etki alanında Grup ilkelerini yapılandırmak için özelleştirebilirsiniz. GPO 'ya sağ tıklayın ve yerleşik GPO 'YU özelleştirmek için **Düzenle...** ' ye tıklayın. Grup ilkesi yapılandırma Düzenleyicisi aracı, GPO 'YU özelleştirmenize olanak sağlar.

    ![Yerleşik GPO 'YU düzenleme](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Artık yerleşik GPO 'YU düzenlemek için **Grup İlkesi Yönetimi Düzenleyicisi** konsolunu kullanabilirsiniz. Örneğin, aşağıdaki ekran görüntüsünde, yerleşik ' AADDC Computers ' GPO 'sunu nasıl özelleştireceğiniz gösterilmektedir.

    ![GPO 'YU özelleştirme](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Görev 5-özel bir grup ilkesi nesnesi oluşturma (GPO)
Kendi özel Grup İlkesi nesnelerinizi oluşturabilir veya içeri aktarabilirsiniz. Özel GPO 'Ları, yönetilen etki alanında oluşturduğunuz özel bir OU 'ya de bağlayabilirsiniz. Özel kuruluş birimleri oluşturma hakkında daha fazla bilgi için bkz. [yönetilen bir etki alanında özel bır OU oluşturma](create-ou.md).

> [!NOTE]
> Yönetilen etki alanındaki grup ilkesi yönetmek için ' AAD DC Administrators ' grubunun bir üyesi olmanız gerekir.
>
>

1. **Grup İlkesi Yönetim** konsolunda, özel kuruluş BIRIMINIZ (OU) seçeneğini belirleyin. OU 'ya sağ tıklayın ve **Bu etki alanında GPO oluştur ve buraya bağla...** seçeneğine tıklayın.

    ![Özel bir GPO oluşturma](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Yeni GPO için bir ad belirtip **Tamam**' a tıklayın.

    ![GPO için bir ad belirtin](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Yeni bir GPO oluşturulup özel OU 'nuzun bağlanır. GPO 'ya sağ tıklayın ve menüden **Düzenle...** ' ye tıklayın.

    ![Yeni oluşturulan GPO](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Yeni oluşturulan GPO 'YU **Grup İlkesi Yönetimi Düzenleyicisi**kullanarak özelleştirebilirsiniz.

    ![Yeni GPO 'YU özelleştirme](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


[Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx) kullanma hakkında daha fazla bilgi TechNet 'te bulunabilir.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](create-instance.md)
* [Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD Domain Services etki alanını yönetme](manage-domain.md)
* [Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx)
