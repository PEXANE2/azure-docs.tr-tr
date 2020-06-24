---
title: Azure DevTest Labs kullanarak bir laboratuvar oluşturma | Microsoft Docs
description: Bu öğreticide, Azure portal kullanarak Azure DevTest Labs bir laboratuvar oluşturacaksınız. Laboratuvar Yöneticisi laboratuvar yapar, laboratuvarda VM 'Ler oluşturur ve ilkeleri yapılandırır.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 44539c6779afaece6d955a907819ef82d8cd7d5a
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84899507"
---
# <a name="tutorial-set-up-a-lab-by-using-azure-devtest-labs"></a>Öğretici: Azure DevTest Labs kullanarak bir laboratuvar ayarlama
Bu öğreticide, Azure portalı kullanarak bir laboratuvar oluşturursunuz. Laboratuvar yöneticisi bir kuruluşta laboratuvar ayarlar, laboratuvarda sanal makineler oluşturur ve ilkeler yapılandırır. Laboratuvar kullanıcıları (örneğin: geliştirici ve test ediciler), laboratuvarda sanal makineler talep eder, sanal makinelere bağlanır ve sanal makineleri kullanır. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar oluşturma
> * Laboratuvara sanal makineler (VM) ekleme
> * Laboratuvar Kullanıcısı rolüne kullanıcı ekleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab"></a>Laboratuvar oluşturma
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u (listenin en üstünde) seçin, **Geliştirici araçları**’nın üzerine gelin ve **DevTest Labs** seçeneğine tıklayın. 

    ![Yeni DevTest Laboratuvarı menüsü](./media/tutorial-create-custom-lab/new-custom-lab-menu.png)
1. **DevTest Laboratuvarı Oluştur** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar adı** alanına laboratuvar için bir ad girin. 
    2. **Abonelik** alanına, laboratuvarı oluşturmak istediğiniz aboneliği seçin. 
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin. 
    4. **Konum** alanında, laboratuvarın oluşturulmasını istediğiniz konumu/bölgeyi seçin. 
    5. **Oluştur**'u seçin. 
    6. **Panoya sabitle**’yi seçin. Laboratuvarı oluşturduktan sonra laboratuvar, panoda gösterilir. 

        ![DevTest Labs laboratuvar bölümü oluşturma](./media/tutorial-create-custom-lab/create-custom-lab-blade.png)
2. Bildirimlere bakarak laboratuvarın başarıyla oluşturulduğunu doğrulayın. **Kaynağa Git**' i seçin.  

    ![Bildirim](./media/tutorial-create-custom-lab/creation-notification.png)
3. Laboratuvarınız için **DevTest Lab** sayfasını görtığınızdan emin olun. 

    ![Laboratuvarınız için ana sayfa](./media/tutorial-create-custom-lab/lab-home-page.png)

## <a name="add-a-vm-to-the-lab"></a>Laboratuvara bir sanal makine ekleme

1. **DevTest Lab** sayfasında, araç çubuğunda **+Ekle**’yi seçin. 

    ![Ekle düğmesi](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. **Temel seçin** sayfasında, anahtar sözcüğüyle (örn.: Windows, Ubuntu) arama yapın ve listedeki temel görüntülerden birini seçin. 
1. **Sanal makine** sayfasında aşağıdaki eylemleri gerçekleştirin: 
    1. **Sanal makine adı** alanına, sanal makine için bir ad girin. 
    2. **Kullanıcı adı** alanına, sanal makineye erişimi olan kullanıcı için bir ad girin. 
    3. **Parola** alanına kullanıcının parolasını girin. 

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-virtual-machine.png)
1. **Gelişmiş ayarlar** sekmesini seçin.
    1. **Bu makineyi talep edilebilir yap** için **Evet**’i seçin.
    2. **Örnek sayısı** değerinin **1** olarak ayarlandığını onaylayın. **2** olarak ayarlarsanız `<base image name>00' and <base image name>01` adlarıyla 2 sanal makine oluşturulur. Örneğin: `win10vm00` ve `win10vm01`.     
    3. **Gönder**’i seçin. 

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-vm-advanced-settings.png)
    9. **Talep edilebilir sanal makineler** listesinde sanal makinenin durumunu görürsünüz. Sanal makine oluşturulması yaklaşık 25 dakika sürebilir. Adı, laboratuvarı içeren geçerli kaynak grubunun adıyla başlayan sanal makine ayrı bir Azure kaynak grubunda oluşturulur. Örneğin, laboratuvar `labrg` ise sanal makine, `labrg3988722144002` kaynak grubunda oluşturulabilir. 

        ![Sanal makine oluşturma durumu](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Sanal makine oluşturulduktan sonra, **Talep edilebilir sanal makineler** listesinde bu sanal makineyi görürsünüz. 

    > [!NOTE] 
    > **Gelişmiş ayarlar** SAYFASıNDA, VM için ortak, özel veya PAYLAŞıLAN bir IP adresi yapılandırabilirsiniz. **PAYLAŞıLAN IP** etkinleştirildiğinde Azure DevTest Labs otomatik olarak Windows VM 'LERI için RDP 'Yi ve Linux sanal MAKINELERI için SSH 'yi otomatik olarak etkinleştirilir. **Ortak IP** adresleriyle VM 'ler OLUŞTURURSANıZ, RDP ve SSH, DevTest Labs 'den herhangi bir değişiklik yapılmadan etkinleştirilir.  

## <a name="add-a-user-to-the-lab-user-role"></a>Laboratuvar Kullanıcısı rolüne kullanıcı ekleme

1. Soldaki menüden **Yapılandırma ve ilkeler**’i seçin. 

    ![Yapılandırma ve ilkeler](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Menüden **erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ rol ataması Ekle** ' yi seçin. 

    ![Rol ataması Ekle-düğme](./media/tutorial-create-custom-lab/add-role-assignment-button.png)
1. **İzin ekle** sayfasında aşağıdaki eylemleri gerçekleştirin:
    1. **Rol** için **DevTest Labs Kullanıcısı**’nı seçin. 
    2. Eklemek istediğiniz **kullanıcıyı** seçin. 
    3. **Kaydet**’i seçin.

        ![Kullanıcı ekle](./media/tutorial-create-custom-lab/add-user.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Sonraki öğreticide, bir laboratuvar kullanıcısının laboratuvardaki bir sanal makineyi nasıl talep edebileceği ve bu sanal makineye nasıl bağlanabileceği gösterilmektedir. Bu öğreticiyi uygulamak istemiyorsanız ve bu öğreticinin parçası olarak oluşturulan kaynakları temizlemek istiyorsanız aşağıdaki adımları izleyin: 

1. Azure portalında, menüden **Kaynak grupları**’nı seçin. 

    ![Kaynak grupları](./media/tutorial-create-custom-lab/resource-groups.png)
1. Laboratuvarı oluşturduğunuz kaynak grubunuzu seçin. 
1. Araç çubuğundan **Kaynak grubunu sil** seçeneğini belirleyin. Bir kaynak grubu silindiğinde, laboratuvar dahil olmak üzere gruptaki tüm kaynaklar silinir. 

    ![Laboratuvar kaynak grubu](./media/tutorial-create-custom-lab/lab-resource-group.png)
1. `<your resource group name><random numbers>` adıyla sizin için oluşturulan ek kaynak grubunu silmek için bu adımları yineleyin. Örneğin: `splab3988722144001`. Sanal makineler, laboratuvarın bulunduğu kaynak grubunda değil, bu kaynak grubunda oluşturulur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sanal makine ile laboratuvar oluşturdunuz ve bir kullanıcıya bu laboratuvara erişme izni verdiniz. Laboratuvar kullanıcısı olarak laboratuvara erişme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Öğretici: Laboratuvara erişme](tutorial-use-custom-lab.md)

