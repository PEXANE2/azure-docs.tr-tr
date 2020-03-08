---
title: Laboratuvar hesaplarını Azure Lab Services yönetme | Microsoft Docs
description: Bir laboratuvar hesabı oluşturmayı, tüm laboratuar hesaplarını görüntülemeyi veya bir Azure aboneliğindeki laboratuvar hesabını silmeyi öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: c82b5d02ab3928eb0472f2a047cdca2238bf0b63
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78382926"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Azure Lab Services laboratuvar hesaplarını yönetme 
Azure Lab Services, laboratuvar hesabı, sınıf laboratuvarları gibi yönetilen laboratuvar türleri için bir kapsayıcıdır. Yönetici Azure Lab Services bir laboratuvar hesabı ayarlar ve hesapta laboratuvarları oluşturabileceğiniz laboratuvar sahiplerine erişim sağlar. Bu makalede laboratuvar hesabı oluşturma, tüm laboratuar hesaplarını görüntüleme veya laboratuvar hesabını silme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin. **DevOps** bölümünde **Laboratuvar hesapları** ' nı seçin. **Laboratuvar hesapları**'nın yanındaki yıldız (`*`) seçeneğini belirlerseniz Sol menüdeki **Sık Kullanılanlar** bölümüne eklenir. Sonraki zamanda, **Sık Kullanılanlar**altında **Laboratuvar hesapları** ' nı seçersiniz.

    ![Tüm Hizmetler-> Laboratuvar hesapları](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. **Laboratuvar hesapları** sayfasında, sayfada **Ekle** ' yi veya **Laboratuvar hesabı oluştur** ' u seçin. 

    ![Laboratuvar hesapları sayfasında Ekle ' yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. **Laboratuvar hesabı oluşturma** sayfasının **temel bilgiler** sekmesinde, aşağıdaki işlemleri yapın: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin.
    5. Laboratuvar **oluşturucunun laboratuvar konumunu seçmesine Izin ver** alanı için, laboratuvar oluşturucularının laboratuvar için bir konum seçebilmesini isteyip istemediğinizi belirtin. Varsayılan olarak, seçeneği devre dışıdır. Laboratuvar oluşturucuları, devre dışı bırakıldığında, oluşturdukları laboratuvar için bir konum belirtemez. Labs, en yakın coğrafi konumda laboratuvar hesabına oluşturulur. Laboratuvar Oluşturucusu etkinleştirildiğinde Laboratuvar oluşturma sırasında bir konum seçebilir. Daha fazla bilgi için bkz. laboratuvar [oluşturucusunun laboratuvar için konum seçmesine Izin verme](allow-lab-creator-pick-lab-location.md). 

        ![Laboratuvar hesabı oluşturma-> temel bilgiler](../media/how-to-manage-lab-accounts/create-lab-account-basics.png)
5. Ileri ' yi seçin: **Gelişmiş** sekmesine gitmek için sayfanın alt kısmındaki **Gelişmiş** ' i seçin ve ardından aşağıdaki adımları uygulayın: 
    1. Var olan bir **paylaşılan görüntü Galerisi** seçin veya bir tane oluşturun. Şablon VM 'yi, başkaları tarafından yeniden kullanılabilmesi için paylaşılan görüntü galerisine kaydedebilirsiniz. Paylaşılan görüntü galerileriyle ilgili ayrıntılı bilgi için, bkz. [Azure Lab Services paylaşılan görüntü Galerisi kullanma](how-to-use-shared-image-gallery.md).
    2. Kullanıcılar bağlantılarını kestikten sonra **Windows sanal makinelerini otomatik olarak kapatmak** isteyip istemediğinizi belirtin. Sanal makinelerin otomatik olarak kapanmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini belirtin. 
    3. **Eş sanal ağ**için, laboratuvar ağı için bir eş sanal ağ (VNet) seçin. Bu hesapta oluşturulan Labs seçili VNet 'e bağlı ve seçilen VNet 'teki kaynaklara erişime sahip. Daha fazla bilgi için bkz. [laboratuvarınızın sanal ağını eş bir sanal ağla bağlama](how-to-connect-peer-virtual-network.md).    
    8. Laboratuvardaki VM 'Ler için bir **adres aralığı** belirtin. Adres aralığı, sınıfsız etki alanları arası yönlendirme (CıDR) gösteriminde olmalıdır (örnek: 10.20.0.0/23). Laboratuvardaki sanal makineler, bu adres aralığında oluşturulacaktır. Daha fazla bilgi için bkz [. laboratuvardaki VM 'ler için bir adres aralığı belirtme](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab)  

        ![Laboratuvar hesabı oluşturma-> Gelişmiş](../media/how-to-manage-lab-accounts/create-lab-account-advanced.png)  
6. **İleri:** **Etiketler** sekmesine geçmek Için sayfanın alt kısmındaki Etiketler ' i seçin. laboratuvar hesabıyla ilişkilendirmek istediğiniz etiketleri ekleyin. Etiketler, birden fazla kaynağa ve kaynak grubuna aynı etiketi uygulayarak kaynakları kategorilere ayırmanıza ve birleştirilmiş faturalandırmayı görüntülemenize olanak tanıyan ad/değer çiftleridir. daha fazla bilgi için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/management/tag-resources.md).

    ![Laboratuvar hesabı > etiketleri oluşturma](../media/how-to-manage-lab-accounts/create-lab-account-tags.png)
7. **Gözden geçir + oluştur** sekmesine geçmek için bu sayfanın altındaki **gözden geçir + oluştur** ' u seçin. 
4. Bu sayfadaki Özet bilgilerini gözden geçirin ve **Oluştur**' u seçin. 

    ![Laboratuvar hesabı > etiketleri oluşturma](../media/how-to-manage-lab-accounts/create-lab-account-review-create.png)
5. Dağıtım tamamlanana kadar bekleyin, **sonraki adımlar**' ı genişletin ve aşağıdaki görüntüde gösterildiği gibi **Kaynağa Git** ' i seçin: 

    Araç çubuğundaki **zil simgesini** de seçebilir (**Bildirimler**), dağıtımın başarılı olduğunu doğrulayın ve ardından **Kaynağa Git**' i seçin. 

    Alternatif olarak, **Laboratuvar hesapları** sayfasında **Yenile** ' yi seçin ve oluşturduğunuz laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Laboratuvar hesaplarını görüntüleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Menüden **tüm kaynaklar** ' ı seçin. 
3. **Tür**Için **Laboratuvar hesapları** ' nı seçin. 
    Abonelik, kaynak grubu, konumlar ve etiketlere göre de filtre uygulayabilirsiniz. 

    ![Tüm Kaynaklar-> Laboratuvar hesapları](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Laboratuvar hesabındaki laboratuvarları görüntüleyin ve yönetin

1. **Laboratuvar hesabı** sayfasında, sol taraftaki menüden **Tüm Labs** ' i seçin.

    ![Hesaptaki laboratuvarlar](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Hesapta aşağıdaki bilgileri içeren bir **laboratuvarın listesini** görürsünüz: 
    1. Laboratuvarın adı.
    2. Laboratuvarın oluşturulduğu tarih. 
    3. Laboratuvarı oluşturan kullanıcının e-posta adresi. 
    4. Laboratuvarda izin verilen en fazla kullanıcı sayısı. 
    5. Laboratuvarın durumu. 
    6. Rol atamaları. 

## <a name="delete-a-lab-in-the-lab-account"></a>Laboratuvar hesabındaki bir laboratuvarı silme
Laboratuvar hesabındaki laboratuvarların listesini görmek için önceki bölümdeki yönergeleri izleyin.

1. Seç **... (üç nokta)** ve **Sil**' i seçin. 

    ![Laboratuvar silme-düğme](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Uyarı iletisinde **Evet** ' i seçin. 

    ![Laboratuvar Silmeyi Onayla](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Laboratuvar hesabını silme
Bir listede laboratuvar hesapları görüntüleyen önceki bölümde yer alan yönergeleri izleyin. Laboratuvar hesabını silmek için aşağıdaki yönergeleri kullanın: 

1. Silmek istediğiniz **Laboratuvar hesabını** seçin. 
2. Araç çubuğundan **Sil** ' i seçin. 

    ![Laboratuvar hesapları-> Sil düğmesi](../media/how-to-manage-lab-accounts/delete-button.png)
1. Onay için **Yes** yazın.
1. **Sil**’i seçin. 

    ![Laboratuvar hesabını silme-onay](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

> [!NOTE]
> Laboratuvar hesaplarını yönetmek için az. LabServices PowerShell modülünü (Önizleme) da kullanabilirsiniz. Daha fazla bilgi için [GitHub 'Daki az. LabServices giriş sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucusunun laboratuvar konumunu seçmesine izin ver](allow-lab-creator-pick-lab-location.md)
- [Laboratuvarınızın ağını eş bir sanal ağla bağlama](how-to-connect-peer-virtual-network.md)
- [Laboratuvara paylaşılan görüntü Galerisi iliştirme](how-to-attach-detach-shared-image-gallery.md)
- [Laboratuvar sahibi olarak Kullanıcı ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)