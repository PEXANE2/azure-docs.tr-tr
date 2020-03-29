---
title: Uygulamanızı Azure'da nasıl test edin? | Microsoft Dokümanlar
description: Bir dosya paylaşımını bir laboratuvarda nasıl oluşturup yerel makinenize ve laboratuardaki sanal makineye nasıl monte edeceğiz ve ardından masaüstü/web uygulamalarını dosya paylaşımına nasıl dağıtıp test edebilirsiniz.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: spelluru
ms.openlocfilehash: f8c57b9e1fabbd04a7d9c92484b0f52f074c2577
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65872500"
---
# <a name="test-your-app-in-azure"></a>Uygulamanızı Azure’da test edin 
Bu makalede, DevTest Labs kullanarak Azure'da uygulamanızı sınayatmak için adımlar sağlanmaktadır. İlk olarak, bir laboratuar içinde bir dosya paylaşımı ayarlayın ve yerel geliştirme makinesi ve bir laboratuvar içinde bir VM bir sürücü olarak monte. Ardından Visual Studio 2019'u kullanarak uygulamanızı dosya paylaşımına dağıtır sınız, böylece uygulamayı laboratuvarda VM'de çalıştırabilirsiniz.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar 
1. Zaten bir azure [aboneliğiniz](https://azure.microsoft.com/free/) yoksa bir Azure aboneliği oluşturun ve [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure DevTest Labs'ı kullanarak bir laboratuvar oluşturmak için [bu makaledeki](devtest-lab-create-lab.md) yönergeleri izleyin. Bir sonraki oturum açğınızda kolayca bulabilmeniz için laboratuarı panonuza sabitle. Azure DevTest Labs, atıkları en aza indirerek ve maliyeti kontrol ederek Azure içinde hızlı bir şekilde kaynak oluşturmanıza olanak tanır. DevTest Labs hakkında daha fazla bilgi edinmek için [genel bakışa](devtest-lab-overview.md)bakın. 
3. Depolama hesabı oluştur makalesindeki yönergeleri izleyerek laboratuvarın kaynak grubunda bir Azure Depolama [hesabı oluşturun.](../storage/common/storage-create-storage-account.md) Depolama **hesabı oluştur** sayfasında, Kaynak **grubu**için **varolan kullan'ı** seçin ve **laboratuvarın kaynak grubunu**seçin. 
4. Azure Dosyaları makalesinde [dosya paylaşımı oluştur'daki](../storage/files/storage-how-to-create-file-share.md) yönergeleri izleyerek Azure depolama alanınızda bir dosya paylaşımı oluşturun. 

## <a name="mount-the-file-share-on-your-local-machine"></a>Dosya paylaşımını yerel makinenize monte edin
1. Yerel makinenizde, Windows makalesi [ile Azure dosya paylaşımı nı kullanma'nın](../storage/files/storage-how-to-use-files-windows.md) Windows bölümünde kalıcı Azure dosya paylaşımı kimlik [bilgilerini](../storage/files/storage-how-to-use-files-windows.md#persisting-azure-file-share-credentials-in-windows) kullanın. 
2. Ardından, `net use` dosya paylaşımını makinenize monte etmek için komutu kullanın. Örnek komut aşağıda vereme: Komutu çalıştırmadan önce Azure depolama adınızı ve dosya paylaşım adınızı belirtin. 

    `net use Z: \\<YOUR AZURE STORAGE NAME>.file.core.windows.net\<YOUR FILE SHARE NAME> /persistent:yes`

## <a name="create-a-vm-in-the-lab"></a>Laboratuvarda VM oluşturma
1. Dosya **paylaşım** sayfasında, üstteki kırıntı menüsündeki **kaynak grubunu** seçin. **Kaynak grup** sayfasını görürsünüz. 
    
    ![Kırıntı menüsünden kaynak grubu seçin](media/test-app-in-azure/select-resource-group-bread-crump.png)
2. Kaynak **grup** sayfasında, DevTest Labs'da oluşturduğunuz **laboratuarı** seçin.

    ![Laboratuvarı seçme](media/test-app-in-azure/select-devtest-lab-in-resource-group.png)
3. Laboratuvarınızın **DevTest Lab** sayfasında araç çubuğuna **+ Ekle'yi** seçin. 

    ![Laboratuvar için düğme ekle](media/test-app-in-azure/add-button-in-lab.png)
4. Bir temel sayfa **seçin,** **smalldisk'i**arayın ve **[smalldisk] Windows Server 2016 Veri Merkezi'ni**seçin. 

    ![Küçük disk Windows sunucusu seçin](media/test-app-in-azure/choose-small-disk-windows-server.png)
5. Sanal **makine** sayfasında, **sanal makine adı,** **kullanıcı adı,** **şifre**belirtin ve **Oluştur'u**seçin.    
    
    ![Sanal makine sayfası oluşturma](media/test-app-in-azure/create-virtual-machine-page.png)    

## <a name="mount-the-file-share-on-your-vm"></a>VM'nizde dosya paylaşımını montaj
1. Sanal makine başarıyla oluşturulduktan sonra, listeden **sanal makineyi** seçin.    

    ![Laboratuvar VM'yi seçin](media/test-app-in-azure/select-lab-vm.png)
2. VM'ye bağlanmak için araç çubuğunda **Bağlan'ı** seçin. 
3. [Azure PowerShell'i yükleyin.](/powershell/azure/install-az-ps)
4. Dosya paylaşımı bölümünde Mount yönergeleri izleyin. 

## <a name="publish-your-app-from-visual-studio"></a>Uygulamanızı Visual Studio'dan yayınlayın
Bu bölümde, uygulamanızı Visual Studio'dan buluttaki bir test VM'sine yayınlarsınız.

1. Visual Studio 2019'u kullanarak bir masaüstü/web uygulaması oluşturun.
2. Uygulamanızı oluşturun.
3. Uygulamanızı yayınlamak için **Çözüm Gezgini'nde**projenizi sağ tıklatın ve **Yayımla'yı**seçin. 
4. **Yayımla**sihirbazına, dosya paylaşımınıza eşlenen **sürücüyü** girin.

    **Masaüstü uygulaması:**

    ![Masaüstü uygulaması](media/test-app-in-azure/desktop-app.png)

    **Web uygulaması:**

    ![Web uygulaması](media/test-app-in-azure/web-app.png)

1. Yayımlama iş akışını tamamlamak için **İleri'yi** seçin ve **Finish'i**seçin. Sihirbaz adımlarını bitirdiğinizde, Visual Studio uygulamanızı oluşturur ve dosya paylaşımınıza yayınlar. 


## <a name="test-the-app-on-your-test-vm-in-the-lab"></a>Uygulamayı test VM'nizde laboratuarda test edin

1. Laboratuvarda VM'niz için sanal makine sayfasına gidin. 
2. Durdurulmuş durumdaysa VM'yi başlatmak için araç çubuğunda **Başlat'ı** seçin. Her seferinde başlatmave durdurmayı önlemek için VM'iniz için otomatik başlatma ve otomatik kapatma ilkeleri ayarlayabilirsiniz. 
3. **Bağlan**’ı seçin.

    ![Sanal makine sayfası](media/test-app-in-azure/virtual-machine-page.png)
4. Sanal makine içinde **Dosya Gezgini'ni başlatın**ve dosya paylaşımınızı bulmak için **Bu Bilgisayarı** seçin.

    ![VM'de paylaşım bul](media/test-app-in-azure/find-share-on-vm.png)

    > [!NOTE]
    > Herhangi bir nedenle, sanal makinenizde veya yerel makinenizde dosya paylaşımınızı bulamıyorsanız, komutu `net use` çalıştırarak dosyayı yeniden monte edebilirsiniz. Azure portalında `net use` **Dosya Paylaşımınızın** **Bağlantı** Sihirbazı'nda komutu bulabilirsiniz.
1. Dosya paylaşımını açın ve Visual Studio'dan dağıttığınız uygulamayı gördüğünüzden onaylayın. 

    ![VM'de açık pay](media/test-app-in-azure/open-file-share.png)

    Artık Azure'da oluşturduğunuz test VM'de uygulamanıza erişebilir ve test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarda VM'lerin nasıl kullanılacağını öğrenmek için aşağıdaki makalelere bakın. 

- [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)
- [Bir laboratuvar VM'yi yeniden başlatın](devtest-lab-restart-vm.md)
- [Bir laboratuvar VM'ini yeniden boyutlandırma](devtest-lab-resize-vm.md)
