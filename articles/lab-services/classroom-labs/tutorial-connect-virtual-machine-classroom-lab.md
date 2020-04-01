---
title: Azure Lab Services’teki bir sınıf laboratuvarına erişme | Microsoft Docs
description: Bu öğreticide, bir profesör tarafından ayarlanan sınıf laboratuvarındaki sanal makinelere erişirsiniz.
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
ms.date: 02/10/2020
ms.author: spelluru
ms.openlocfilehash: 27d79e28a986e929fb71dd77fc50b3c2cd32618f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77134029"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Öğretici: Azure Lab Services’teki bir sınıf laboratuvarına erişme
Bu öğreticide, öğrenci olarak bir sınıf laboratuvarındaki sanal makineye (VM) bağlanırsınız. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvara kaydolun
> * VM’yi başlatma
> * VM’ye bağlanma

## <a name="register-to-the-lab"></a>Laboratuvara kaydolun

1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. Kaydı tamamladıktan sonra kayıt URL'sini kullanmanız gerekmez. Bunun yerine, URL'yi kullanın: [https://labs.azure.com](https://labs.azure.com). Internet Explorer 11 henüz desteklenmedi. 
1. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 

    > [!NOTE]
    > Azure Laboratuvar Hizmetlerini kullanmak için bir Microsoft hesabı gereklidir. Portalda oturum kurmak için Yahoo veya Google hesapları gibi Microsoft hesabı olmayan hesabınızı kullanmaya çalışıyorsanız, Microsoft hesabınızla bağlantılı olacak bir Microsoft hesabı oluşturmak için yönergeleri izleyin. Ardından, kayıt işlemini tamamlamak için adımları izleyin. 
1. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 
1. Sanal makine hazır olana kadar bekleyin. VM döşemesi üzerinde aşağıdaki alanlara dikkat edin:
    1. Döşemenin üst **kısmında, laboratuvarın adını**görüyorsunuz.
    1. Sağında, VM işletim sistemini **(OS)** temsil eden simgeyi görürsünüz. Bu örnekte, Windows os's. 
    1. Döşemedeki ilerleme çubuğu, size atanan [kota saat](how-to-configure-student-usage.md#set-quotas-for-users) sayısına göre kullanılan saat sayısını gösterir. Bu süre, laboratuvar için zamanlanan süreye ek olarak size ayrılan ek süredir. 
    1. VM'yi başlatmak/durdurmak ve VM'ye bağlanmak için döşemenin alt kısmında simgeler/düğmeler görürsünüz. 
    1. Düğmelerin sağında VM'nin durumunu görürsünüz. VM'nin durumunu gördüğünüzü doğrulayın **Durdurulur.** 

        ![Durdurulmuş durumda VM](../media/tutorial-connect-vm-in-classroom-lab/vm-in-stopped-state.png)

## <a name="start-the-vm"></a>VM’yi başlatma
1. Aşağıdaki resimde gösterildiği gibi ilk düğmeyi seçerek VM'yi **başlatın.** Bu işlem biraz zaman alır.  

    ![VM’yi başlatma](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)
4. VM'nin durumunun **Running**olarak ayarlı olduğunu doğrulayın. 

    ![Çalışma durumunda VM](../media/tutorial-connect-vm-in-classroom-lab/vm-running.png)

    İlk düğmenin simgesinin **durdurma** işlemini temsil edecek şekilde değiştirilmeye dikkat edin. VM'yi durdurmak için bu düğmeyi seçebilirsiniz. 

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. Laboratuvarın VM'sine **bağlanmak** için aşağıdaki resimde gösterildiği gibi ikinci düğmeyi seçin. 

    ![VM’ye bağlanma](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Aşağıdaki adımlardan birini yapın: 
    1. **Windows** sanal makineleri için **RDP** dosyasını sabit diske kaydedin. Sanal makineye bağlanmak için RDP dosyasını açın. Makinede oturum açabilmek için eğitimci/profesörünüzden aldığınız **kullanıcı adı** ve **şifreyi** kullanın. 
    3. **Linux** sanal makineleri için, onlara bağlanmak için **SSH** veya **RDP** (etkinse) kullanabilirsiniz. Daha fazla bilgi için bkz: [Linux makineleri için uzak masaüstü bağlantısını etkinleştir.](how-to-enable-remote-desktop-linux.md) 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, eğitimcinizden/profesörünüzden aldığınız kayıt bağlantısını kullanarak bir sınıf laboratuvarına eriştiniz.

Bir laboratuvar sahibi olarak, laboratuarınıza kimlerin kaydolduğunu görüntülemek ve VM kullanımını izlemek istiyorsunuz. Laboratuvarın kullanımını nasıl izleyeceğini öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Laboratuvar kullanımını izleme](tutorial-track-usage.md) 
