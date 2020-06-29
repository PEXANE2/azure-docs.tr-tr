---
title: Azure Lab Services (betik) içinde bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme | Microsoft Docs
description: İçinde birden çok VM içeren bir şablon VM 'si oluşturmayı öğrenin.  Diğer bir deyişle, Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: a45257dffffc98c3f650c5b1753158edd9bfbede
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445755"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services-using-a-script"></a>Betik kullanarak Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesi içinde çoklu VM ortamı oluşturmanızı sağlar. Şablonu yayımlamak, her kullanıcıya, içindeki birden çok VM ile bir sanal makine oluşturacak şekilde laboratuvar sağlar.  İç içe sanallaştırma ve Azure Lab Services hakkında daha fazla bilgi için bkz. [Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme](how-to-enable-nested-virtualization-template-vm.md).

Bu makaledeki adımlar, Windows Server 2016, Windows Server 2019 veya Windows 10 için iç içe sanallaştırmayı ayarlamaya odaklanmaktadır. Hyper-V ile şablon makinesi ayarlamak için bir komut dosyası kullanacaksınız.  Aşağıdaki adımlar, [Laboratuvar Hizmetleri Hyper-V betiklerini](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV)kullanma konusunda size rehberlik edecektir.

>[!IMPORTANT]
>Laboratuvar oluştururken sanal makine boyutu için **büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma, aksi takdirde çalışmaz.  

## <a name="run-script"></a>Betik çalıştırma

1. Internet Explorer kullanıyorsanız, `https://github.com` Güvenilen siteler listesine eklemeniz gerekebilir.
    1. Internet Explorer'ı açın.
    1. Dişli simgesini seçin ve **Internet seçenekleri**' ni seçin.  
    1. **Internet seçenekleri** iletişim kutusu göründüğünde, **güvenlik**' i seçin, **Güvenilen siteler**' i seçin, **siteler** ' i tıklatın.
    1. **Güvenilen siteler** iletişim kutusu göründüğünde, `https://github.com` güvenilir web siteleri listesine ekleyin ve **Kapat**' ı seçin.

        ![Güvenilen siteler](./media/how-to-enable-nested-virtualization-template-vm-using-script/trusted-sites-dialog.png)
1. Git deposu dosyalarını aşağıdaki adımlarda özetlenen şekilde indirin.
    1. Adresine gidin [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/) .
    1. **Kopyala veya indir** düğmesine tıklayın.
    1. **ZIP 'ı indir**' e tıklayın.
    1. ZIP dosyasını Ayıkla

    >[!TIP]
    >Git deposunu üzerinde de kopyalayabilirsiniz [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git) .

1. **PowerShell** 'i **yönetici** modunda başlatın.
1. PowerShell penceresinde, indirilen betiği içeren klasöre gidin. Depo dosyalarının en üst klasöründen gezindiğinizde, betiği adresinde bulunur `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\` .
1. Betiği başarıyla çalıştırmak için yürütme ilkesini değiştirmeniz gerekebilir. Şu komutu çalıştırın:

    ```powershell
    Set-ExecutionPolicy bypass -force
    ```

1. Betiği çalıştırın:

    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > Betik için makinenin yeniden başlatılması gerekebilir. Betikteki yönergeleri izleyin ve komut dosyası **tamamlanana** kadar çıkışta betiği yeniden çalıştırın.
1. Yürütme ilkesini sıfırlamayı unutmayın. Şu komutu çalıştırın:

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Sonuç

Şablon makineniz artık Hyper-V sanal makineleri oluşturmak için hazırdır. Bkz. Hyper-V sanal makineleri oluşturma hakkında yönergeler için bkz. [Hyper-v ' d e sanal makine oluşturma](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) . Ayrıca, kullanılabilir işletim sistemlerini ve yazılımlarını kullanıma almak için bkz. [Microsoft değerlendirme merkezi](https://www.microsoft.com/evalcenter/) .  

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)