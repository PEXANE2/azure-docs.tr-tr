---
title: Azure Laboratuvar Hizmetleri'nde (Script) bir şablon VM'de iç içe sanallaştırmayı etkinleştirme | Microsoft Dokümanlar
description: İçinde birden çok VM içeren bir şablon VM'yi nasıl oluşturabilirsiniz öğrenin.  Başka bir deyişle, Azure Lab Hizmetlerinde bir şablon VM'de iç içe sanallaştırmayı etkinleştirin.
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
ms.openlocfilehash: 56e5ad21f94521565b4df193b2450a1c994b66f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503042"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services-using-a-script"></a>Azure Lab Hizmetleri'ndeki şablon sanal makinede bir komut dosyası kullanarak iç içe sanallaştırmayı etkinleştirme

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesinin içinde çoklu VM ortamı oluşturmanıza olanak tanır. Şablonun yayımı, laboratuardaki her kullanıcıya içinde birden çok VM'nin yer verdiği sanal bir makine sağlar.  İç içe sanallaştırma ve Azure Laboratuvar Hizmetleri hakkında daha fazla bilgi için azure [Lab Hizmetlerinde şablon sanal makinede iç içe sanallaştırmayı etkinleştir'e](how-to-enable-nested-virtualization-template-vm.md)bakın.

Bu makaledeki adımlar, Windows Server 2016 veya Windows Server 2019 için iç içe sanallaştırma ayarlamaya odaklanmaktadır. Hyper-V ile şablon makinesi ni ayarlamak için bir komut dosyası kullanırsınız.  Aşağıdaki [adımlar, Lab Services Hyper-V komut dosyalarını](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV)nasıl kullanacağınız konusunda size yol gösterecektir.

>[!IMPORTANT]
>Laboratuarı oluştururken sanal makine boyutu için **Büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma aksi takdirde çalışmaz.  

## <a name="run-script"></a>Betik çalıştırma

1. Internet Explorer kullanıyorsanız, güvenilen siteler `https://github.com` listesine eklemeniz gerekebilir.
    1. Internet Explorer'ı açın.
    1. Vites simgesini seçin ve **Internet seçeneklerini**seçin.  
    1. Internet **Seçenekleri** iletişim kutusu görüntülendiğinde, **Güvenlik,** **Güvenilen Siteler'** i seçin, **Siteler** düğmesini tıklatın.
    1. **Güvenilen siteler** iletişim kutusu görüntülendiğinde, güvenilen web siteleri listesine ekleyin `https://github.com` ve **Kapat'ı**seçin.

        ![Güvenilir siteler](../media/how-to-enable-nested-virtualization-template-vm-using-script/trusted-sites-dialog.png)
1. Aşağıdaki adımlarda belirtildiği gibi Git depo dosyalarını indirin.
    1. [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/)Git.
    1. Klon **veya İndir** düğmesini tıklatın.
    1. **ZIP İndir'i**tıklatın.
    1. ZIP dosyasını ayıklama

    >[!TIP]
    >Git deposunu da .'de [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git)klonlayabilirsiniz.

1. **PowerShell'i** **Yönetici** modunda başlatın.
1. PowerShell penceresinde, indirilen komut dosyasının yer alan klasörüne gidin. Depo dosyalarının üst klasöründen geziniyorsanız, komut dosyası `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`.
1. Komut dosyasını başarıyla çalıştırmak için yürütme ilkesini değiştirmeniz gerekebilir. Şu komutu çalıştırın:

    ```powershell
    Set-ExecutionPolicy bypass -force
    ```

1. Betiği çalıştırın:

    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > Komut dosyası, makinenin yeniden başlatılmasını gerektirebilir. Komut dosyasının yönergelerini izleyin ve **Komut Dosyası tamamlanana** kadar komut dosyasını çıktıda görene kadar yeniden çalıştırın.
1. Yürütme ilkesini sıfırlamayı unutmayın. Şu komutu çalıştırın:

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Sonuç

Artık şablon makineniz Hyper-V sanal makineleri oluşturmaya hazır. Hyper-V sanal makinelerin nasıl oluşturulacağına ilişkin talimatlar için [Hyper-V'de Sanal Makine Oluştur'a](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) bakın. Ayrıca, kullanılabilir işletim sistemlerine ve yazılımlara göz atabilmek için [Microsoft Değerlendirme Merkezi'ne](https://www.microsoft.com/evalcenter/) bakın.  

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)