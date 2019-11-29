---
title: Azure Lab Services | bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme | Microsoft Docs
description: İçinde birden çok VM içeren bir şablon VM 'si oluşturmayı öğrenin.  Diğer bir deyişle, Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirin.
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
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 64097a5b3b62bcd5a84f4472a844bb95cf24cd6f
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555080"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştir

Şu anda Azure Lab Services, bir laboratuvarda bir şablon sanal makinesi ayarlamanıza ve tek bir kopyayı kullanıcılarınızın her biri için kullanılabilir hale getirmenizi sağlar. Bir Profesör eğitim ağı, güvenlik veya BT sınıflarındaysanız, öğrencilerinizi her birini, birden fazla sanal makinenin bir ağ üzerinden birbirleriyle iletişim kurabilecek bir ortamla birlikte sağlamanız gerekebilir.

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesi içinde çoklu VM ortamı oluşturmanızı sağlar. Şablonu yayımlamak, her kullanıcıya, içindeki birden çok VM ile bir sanal makine oluşturacak şekilde laboratuvar sağlar.  Bu makalede, Azure Lab Services bir şablon makinesinde iç içe sanallaştırmanın nasıl ayarlanacağı ele alınmaktadır.

## <a name="what-is-nested-virtualization"></a>İç içe sanallaştırma nedir?

İç içe sanallaştırma, sanal makine içinde sanal makineler oluşturmanızı sağlar. İç içe sanallaştırma Hyper-V aracılığıyla yapılır ve yalnızca Windows VM 'lerinde kullanılabilir.

İç içe sanallaştırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure 'da iç içe sanallaştırma](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Azure VM 'de iç içe sanallaştırmayı etkinleştirme](../../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

İç içe sanallaştırmaya sahip bir laboratuvar ayarlamadan önce göz önünde bulundurmanız gereken birkaç nokta vardır.

- Yeni bir laboratuvar oluştururken, sanal makine boyutu için **Orta (Iç içe sanallaştırma)** veya **büyük (iç içe sanallaştırma)** boyutlarını seçin. Bu sanal makine boyutları, iç içe sanallaştırmayı destekler.
- Hem konak hem de istemci sanal makineleri için iyi performans sağlayacak bir boyut seçin.  Sanallaştırmayı kullanırken, seçtiğiniz boyutun yalnızca bir makine değil, ana bilgisayarın yanı sıra aynı anda çalıştırılması gereken tüm istemci makineler için yeterli olması gerektiğini unutmayın.
- İstemci sanal makinelerinin Azure sanal ağındaki DNS sunucuları gibi Azure kaynaklarına erişimi olmayacaktır.
- Konak sanal makinesi, istemci makinenin internet bağlantısına izin vermek için kurulum gerektirir.
- İstemci sanal makineleri bağımsız makineler olarak lisanslanır. Microsoft Operasyon sistemleri ve ürünleri için lisanslama hakkında bilgi için bkz. [Microsoft lisanslama](https://www.microsoft.com/licensing/default) . Şablon makinesini ayarlamadan önce kullanılan diğer yazılımlar için lisans sözleşmelerini denetleyin.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Şablon VM üzerinde iç içe yerleştirilmiş sanallaştırmayı etkinleştirme

Bu makalede, bir laboratuar hesabı ve Laboratuvarı oluşturmuş olduğunuz varsayılmaktadır.  Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [öğretici, laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md). Laboratuvar oluşturma hakkında daha fazla bilgi için bkz. [sınıf Laboratuvarı ayarlama öğreticisi](tutorial-setup-classroom-lab.md).

>[!IMPORTANT]
>Laboratuvar oluştururken sanal makine boyutu için **büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma, aksi takdirde çalışmaz.  

Şablon makinesine bağlanmak için bkz. [sınıf şablonu oluşturma ve yönetme](how-to-create-manage-template.md). 

Bu bölümdeki adımlar, Windows Server 2016 veya Windows Server 2019 için iç içe sanallaştırmayı ayarlamaya odaklanmaktadır. Hyper-V ile şablon makinesi ayarlamak için bir komut dosyası kullanacaksınız.  Aşağıdaki adımlar, [Laboratuvar Hizmetleri Hyper-V betiklerini](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV)kullanma konusunda size rehberlik edecektir.

1. Internet Explorer kullanıyorsanız, güvenilen siteler listesine `https://github.com` eklemeniz gerekebilir.
    1. Internet Explorer'ı açın.
    1. Dişli simgesini seçin ve **Internet seçenekleri**' ni seçin.  
    1. **Internet seçenekleri** iletişim kutusu göründüğünde, **güvenlik**' i seçin, **Güvenilen siteler**' i seçin, **siteler** ' i tıklatın.
    1. **Güvenilen siteler** iletişim kutusu göründüğünde, güvenilir web siteleri listesine `https://github.com` ekleyin ve **Kapat**' ı seçin.

        ![Güvenilen siteler](../media/how-to-enable-nested-virtualization-template-vm/trusted-sites-dialog.png)
1. Git deposu dosyalarını aşağıdaki adımlarda özetlenen şekilde indirin.
    1. [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/)gidin.
    1. **Kopyala veya indir** düğmesine tıklayın.
    1. **ZIP 'ı indir**' e tıklayın.
    1. ZIP dosyasını Ayıkla

    >[!TIP]
    >Git deposunu [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git)de kopyalayabilirsiniz.

1. **PowerShell** 'i **yönetici** modunda başlatın.
1. PowerShell penceresinde, indirilen betiği içeren klasöre gidin. Depo dosyalarının en üst klasöründen gezindiğinizde, komut dosyası `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`konumunda bulunur.
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
