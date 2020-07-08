---
title: Uygulamanızı Azure 'da test etme | Microsoft Docs
description: Laboratuvara bir dosya paylaşımının nasıl oluşturulduğunu ve bunu yerel makinenize ve bir sanal makineye nasıl bağlayacağınızı öğrenin ve ardından Masaüstü/Web uygulamalarını dosya paylaşımında dağıtıp test edin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 30e77e67a1674b2170c1d1782f1389274afa4c35
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85476504"
---
# <a name="test-your-app-in-azure"></a>Uygulamanızı Azure’da test edin 
Bu makalede, DevTest Labs kullanarak Azure 'da uygulamanızı test etme adımları sağlanmaktadır. İlk olarak, bir laboratuvarda bir dosya paylaşma ayarlarsınız ve bunu yerel geliştirme makinenizde ve bir laboratuvardaki VM 'de bir sürücü olarak bağlarsınız. Daha sonra, uygulamayı laboratuvardaki VM üzerinde çalıştırabilmeniz için uygulamanızı dosya paylaşımında dağıtmak üzere Visual Studio 2019 ' i kullanırsınız.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar 
1. Henüz bir hesabınız yoksa [Azure aboneliği oluşturun](https://azure.microsoft.com/free/) ve [Azure Portal](https://portal.azure.com)oturum açın.
2. Azure DevTest Labs kullanarak bir laboratuvar oluşturmak için [Bu makaledeki](devtest-lab-create-lab.md) yönergeleri izleyin. Bir sonraki oturum açışınızda kolayca bulabilmeniz için Laboratuvarı panonuza sabitleyin. Azure DevTest Labs, atık ve denetim maliyetini en aza indirerek Azure 'da hızlı bir şekilde kaynak oluşturmanızı sağlar. DevTest Labs hakkında daha fazla bilgi edinmek için bkz. [genel bakış](devtest-lab-overview.md). 
3. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) makalesindeki yönergeleri izleyerek laboratuvarın kaynak grubunda bir Azure depolama hesabı oluşturun. **Depolama hesabı oluştur** sayfasında, **var olan** **kaynak grubunu**kullan ' ı seçin ve **laboratuvarın kaynak grubunu**seçin. 
4. Azure [dosyalarında dosya paylaşma oluşturma](../storage/files/storage-how-to-create-file-share.md) makalesindeki yönergeleri izleyerek Azure depolamada bir dosya paylaşma oluşturun. 

## <a name="mount-the-file-share-on-your-local-machine"></a>Dosya paylaşımından yerel makinenize bağlama
1. Yerel makinenizde, Azure dosya paylaşma ' nın [Azure dosya paylaşımından takma](../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share) bölümünde yer alan betiği kullanarak [Windows](../storage/files/storage-how-to-use-files-windows.md) makalesini kullanın. 
2. Sonra, `net use` dosya paylaşımından makinenize bağlamak için komutunu kullanın. Örnek komut şu şekildedir: komutunu çalıştırmadan önce Azure depolama adınızı ve dosya paylaşımının adını belirtin. 

    `net use Z: \\<YOUR AZURE STORAGE NAME>.file.core.windows.net\<YOUR FILE SHARE NAME> /persistent:yes`

## <a name="create-a-vm-in-the-lab"></a>Laboratuvarda VM oluşturma
1. **Dosya paylaşma** sayfasında, üstteki içerik haritası menüsünde **kaynak grubunu** seçin. **Kaynak grubu** sayfasını görürsünüz. 
    
    ![İçerik haritası menüsünden kaynak grubu seç](media/test-app-in-azure/select-resource-group-bread-crump.png)
2. **Kaynak grubu** sayfasında, DevTest Labs 'de oluşturduğunuz **Laboratuvarı** seçin.

    ![Laboratuvarı seçme](media/test-app-in-azure/select-devtest-lab-in-resource-group.png)
3. Laboratuvarınızın **DevTest Lab** sayfasında, araç çubuğunda **+ Ekle** ' yi seçin. 

    ![Laboratuvar için Düğme Ekle](media/test-app-in-azure/add-button-in-lab.png)
4. **Temel seçin** sayfasında, **smalldisk**' i arayın ve **[smalldisk] Windows Server 2016 veri merkezini**seçin. 

    ![Küçük disk Windows Server seçin](media/test-app-in-azure/choose-small-disk-windows-server.png)
5. **Sanal makine** sayfasında, **sanal makine adı**, **Kullanıcı adı**, **parola**belirtin ve **Oluştur**' u seçin.    
    
    ![Sanal makine oluştur sayfası](media/test-app-in-azure/create-virtual-machine-page.png)    

## <a name="mount-the-file-share-on-your-vm"></a>Sanal makinenize dosya paylaşımından bağlama
1. Sanal makine başarıyla oluşturulduktan sonra, listeden **sanal makineyi** seçin.    

    ![Laboratuvar VM 'sini seçin](media/test-app-in-azure/select-lab-vm.png)
2. SANAL makineye bağlanmak için araç çubuğunda **Bağlan** ' ı seçin. 
3. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps).
4. Dosya paylaşımının bağlama bölümündeki yönergeleri izleyin. 

## <a name="publish-your-app-from-visual-studio"></a>Uygulamanızı Visual Studio 'dan yayımlayın
Bu bölümde, uygulamanızı Visual Studio 'dan buluttaki bir test sanal makinesine yayımlarsınız.

1. Visual Studio 2019 kullanarak bir Masaüstü/Web uygulaması oluşturun.
2. Uygulamanızı oluşturun.
3. Uygulamanızı yayımlamak için **Çözüm Gezgini**projenize sağ tıklayın ve **Yayımla**' yı seçin. 
4. **Yayımla sihirbazında**, dosya paylaşımınızla eşleştirilmiş olan **sürücüyü** girin.

    **Masaüstü uygulaması:**

    ![Masaüstü uygulaması](media/test-app-in-azure/desktop-app.png)

    **Web uygulaması:**

    ![Web uygulaması](media/test-app-in-azure/web-app.png)

1. Yayımla iş akışını tamamlayıp **İleri** ' yi seçin ve **son**' u seçin. Sihirbaz adımlarını tamamladığınızda, Visual Studio uygulamanızı oluşturur ve dosya paylaşımınızda yayımlar. 


## <a name="test-the-app-on-your-test-vm-in-the-lab"></a>Uygulamayı laboratuvardaki test sanal makinenizde test etme

1. Laboratuvardaki VM 'niz için sanal makine sayfasına gidin. 
2. Durdurulmuş durumdaysa VM 'yi başlatmak için araç çubuğundan **Başlat** ' ı seçin. Her seferinde başlatma ve durdurma yapmaktan kaçınmak için sanal makinenizin otomatik başlatma ve otomatik kapatmalar ilkelerini ayarlayabilirsiniz. 
3. **Bağlan**'ı seçin.

    ![Sanal makine sayfası](media/test-app-in-azure/virtual-machine-page.png)
4. Sanal makine içinde **dosya gezginini**başlatın ve dosya paylaşımınızı bulmak IÇIN **Bu bilgisayarı** seçin.

    ![VM 'de paylaşma bul](media/test-app-in-azure/find-share-on-vm.png)

    > [!NOTE]
    > Herhangi bir nedenle, sanal makinenizde veya yerel makinenizde dosya paylaşımınızı bulamıyorsanız, komutunu çalıştırarak onu yeniden bağlayabilirsiniz `net use` . `net use`Komutu, Azure Portal **Dosya paylaşımınızın** **bağlanma** sihirbazında bulabilirsiniz.
1. Dosya paylaşımından açın ve Visual Studio 'dan dağıttığınız uygulamayı görtığınızdan emin olun. 

    ![VM 'de paylaşma aç](media/test-app-in-azure/open-file-share.png)

    Artık, Azure 'da oluşturduğunuz test VM 'si içinde uygulamanıza erişebilir ve test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarda VM 'Leri nasıl kullanacağınızı öğrenmek için aşağıdaki makalelere bakın. 

- [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)
- [Laboratuvar VM 'yi yeniden başlatma](devtest-lab-restart-vm.md)
- [Laboratuvar VM 'yi yeniden boyutlandırma](devtest-lab-resize-vm.md)
