---
title: Azure Laboratuvar Hizmetleri'ndeki laboratuvar hesaplarını yönetme | Microsoft Dokümanlar
description: Azure aboneliğinde nasıl bir laboratuvar hesabı oluşturarak, tüm laboratuvar hesaplarını nasıl görüntüleyebilirsiniz veya bir laboratuvar hesabını silmeyi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284297"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki laboratuvar hesaplarını yönetme 
Azure Laboratuvar Hizmetleri'nde, laboratuvar hesabı sınıf laboratuarları gibi yönetilen laboratuvar türleri için bir kapsayıcıdır. Yönetici, Azure Laboratuvar Hizmetleri ile bir laboratuvar hesabı oluşturur ve hesapta laboratuvar oluşturabilen laboratuvar sahiplerine erişim sağlar. Bu makalede, bir laboratuvar hesabı oluşturma, tüm laboratuvar hesaplarını görüntüleme veya bir laboratuvar hesabını silme nasıl açıklanmaktadır.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden **Tüm Hizmetler'i** seçin. **DevOps** bölümünde **Laboratuvar Hesapları'nı** seçin. Laboratuvar`*` **Hesapları'nın**yanındaki yıldız ( ) seçeneğini seçerseniz, sol menüdeki SıK **Kullanılanlar** bölümüne eklenir. Bir sonraki andan itibaren, **Sık Kullanılanlar**altında Laboratuvar **Hesapları'nı** seçersiniz.

    ![Tüm Hizmetler -> Laboratuvar Hesapları](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Laboratuvar **Hesapları** sayfasında, araç çubuğuna **Ekle'yi** veya sayfada **laboratuvar hesabı oluştur'u** seçin. 

    ![Laboratuvar Hesapları sayfasında Ekle'yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. **Laboratuvar hesabı oluştur** sayfasının **Temeller** sekmesinde aşağıdaki işlemleri yapın: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin.
    5. Laboratuvar **oluşturucusunlaboratuvar konum alanını seçmesine izin ver,** laboratuvar oluşturucularının laboratuvar için bir konum seçip seçemeyeceğini belirtin. Varsayılan olarak, seçenek devre dışı bırakılır. Devre dışı bırakıldığında, laboratuvar oluşturucuları oluşturdukları laboratuvar için bir konum belirleyemez. Laboratuvarlar laboratuvar hesabına en yakın coğrafi konumda oluşturulur. Etkinleştirildiğinde, bir laboratuvar oluşturucusu laboratuvar oluşturma sırasında bir konum seçebilir. Daha fazla bilgi için laboratuvar [oluşturucusu laboratuar için yer seçmek için izin](allow-lab-creator-pick-lab-location.md)ver'e bakın. 

        ![Laboratuvar hesabı oluşturma -> Temelleri](../media/how-to-manage-lab-accounts/create-lab-account-basics.png)
5. **İleri'yi seçin:** **Gelişmiş** sekmesine gitmek için sayfanın alt kısmında gelişmiş ve ardından aşağıdaki adımları yapın: 
    1. Varolan paylaşılan bir **resim galerisiseçin** veya bir tane oluşturun. Başkaları tarafından yeniden kullanılabilsi için vm şablonu paylaşılan resim galerisine kaydedebilirsiniz. Paylaşılan resim galerileri hakkında ayrıntılı bilgi için azure [lab hizmetlerinde paylaşılan bir resim galerisi kullan'a](how-to-use-shared-image-gallery.md)bakın.
    2. Kullanıcılar bağlantılarını kestiğinde **Windows sanal makinelerini otomatik olarak kapatmak** isteyip istemediğinizi belirtin. Otomatik olarak kapanmadan önce sanal makinelerin kullanıcının yeniden bağlanmasını ne kadar beklemesi gerektiğini belirtin. 
    3. **Peer sanal ağı**için, laboratuar ağı için eş sanal ağ (VNet) seçin. Bu hesapta oluşturulan laboratuvarlar seçili VNet'e bağlıdır ve seçili VNet'teki kaynaklara erişebilir. Daha fazla bilgi için bkz: [Laboratuvarınızın sanal ağını bir eş sanal ağıyla bağlayın.](how-to-connect-peer-virtual-network.md)    
    8. Laboratuardaki VM'ler için bir **adres aralığı** belirtin. Adres aralığı sınıfsız etki alanları arası yönlendirme (CIDR) gösteriminde olmalıdır (örnek: 10.20.0.0/23). Bu adres aralığında laboratuvardaki sanal makineler oluşturulacaktır. Daha fazla bilgi için bkz: [Laboratuvardaki VM'ler için bir adres aralığı belirtin](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab)  

        ![Laboratuvar hesabı oluşturma -> Gelişmiş](../media/how-to-manage-lab-accounts/create-lab-account-advanced.png)  
6. **Sonrakini Seçin:** Etiketler sekmesine geçmek için sayfanın altındaki **etiketler.** Laboratuvar hesabıyla ilişkilendirmek istediğiniz etiketleri ekleyin. Etiketler, aynı etiketi birden çok kaynak ve kaynak grubuna uygulayarak kaynakları kategorilere ayırmanızı ve birleştirilmiş faturalandırmayı görüntülemenizi sağlayan ad/değer çiftleridir. Daha fazla bilgi için [Azure kaynaklarınızı düzenlemek için Etiketleri Kullan'a](../../azure-resource-manager/management/tag-resources.md)bakın.

    ![Laboratuvar hesabı oluşturma -> Etiketleri](../media/how-to-manage-lab-accounts/create-lab-account-tags.png)
7. Gözden Geçir + oluştur sekmesine geçmek için bu sayfanın altındaki **Gözden Geçir + oluştur'u** seçin. **Review + create** 
4. Bu sayfadaki özet bilgileri gözden geçirin ve **Oluştur'u**seçin. 

    ![Laboratuvar hesabı oluşturma -> Etiketleri](../media/how-to-manage-lab-accounts/create-lab-account-review-create.png)
5. Dağıtım tamamlanana kadar bekleyin, **Sonraki adımları**genişletin ve aşağıdaki resimde gösterildiği gibi **kaynağa git'i** seçin: 

    Ayrıca araç çubuğundaki **çan simgesini** **(Bildirimler),** dağıtımın başarılı olduğunu onaylayabilir ve sonra **kaynağa git'i**seçebilirsiniz. 

    Alternatif olarak, Laboratuvar **Hesapları** sayfasında **Yenile'yi** seçin ve oluşturduğunuz laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Laboratuvar hesaplarını görüntüleme
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Menüden **tüm kaynakları** seçin. 
3. Türü için Lab **Hesapları'nı**seçin. **Lab Accounts** 
    Abonene, kaynak grubuna, konumlara ve etiketlere göre de filtre uygulayabilirsiniz. 

    ![Tüm kaynaklar -> Lab Hesapları](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Laboratuvar hesabındaki laboratuvarları görüntüleme ve yönetme

1. Laboratuvar **Hesabı** sayfasında, sol menüdeki **Tüm laboratuarları** seçin.

    ![Hesaptaki laboratuarlar](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Hesapta aşağıdaki bilgileri içeren **bir laboratuvar listesi** görürsünüz: 
    1. Laboratuvarın adı.
    2. Laboratuvarın oluşturulduğu tarih. 
    3. Laboratuarı oluşturan kullanıcının e-posta adresi. 
    4. Laboratuvara girilen maksimum kullanıcı sayısı. 
    5. Laboratuvarın durumu. 
    6. Rol atamaları. 

## <a name="delete-a-lab-in-the-lab-account"></a>Laboratuvar hesabındaki bir laboratuarı silme
Laboratuvar hesabındaki laboratuvarların listesini görmek için önceki bölümdeki yönergeleri izleyin.

1. Seçin **... (elips)** ve **Sil'i**seçin. 

    ![Laboratuarı silme - düğmesi](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Uyarı iletisinde **Evet'i** seçin. 

    ![Laboratuvar silme işlemini onaylama](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Laboratuvar hesabını silme
Bir listede laboratuvar hesaplarını görüntüleyen önceki bölümdeki yönergeleri izleyin. Bir laboratuvar hesabını silmek için aşağıdaki yönergeleri kullanın: 

1. Silmek istediğiniz **laboratuvar hesabını** seçin. 
2. Araç çubuğundan **Sil'i** seçin. 

    ![Laboratuvar Hesapları -> Sil düğmesi](../media/how-to-manage-lab-accounts/delete-button.png)
1. Onay için **Evet** yazın.
1. **Sil**’i seçin. 

    ![Laboratuvar hesabını silme - onay](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)

> [!NOTE]
> Laboratuvar hesaplarını yönetmek için Az.LabServices PowerShell modüllerini (önizleme) de kullanabilirsiniz. Daha fazla bilgi için [GitHub'daki Az.LabServices ana sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Laboratuvarağınızı bir eş sanal ağıyla bağlayın](how-to-connect-peer-virtual-network.md)
- [Paylaşılan bir resim galerisini laboratuvara ekleme](how-to-attach-detach-shared-image-gallery.md)
- [Kullanıcıyı laboratuvar sahibi olarak ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)