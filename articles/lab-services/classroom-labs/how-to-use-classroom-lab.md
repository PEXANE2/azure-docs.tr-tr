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
ms.topic: how-to
ms.custom: mvc
ms.date: 03/17/2020
ms.author: spelluru
ms.openlocfilehash: db1e60ccd7e05cb8b6923d6b2ec008cdfb76eaa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501919"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Azure Lab Services’teki bir sınıf laboratuvarına erişme
Bu makalede, sınıf laboratuvarına nasıl kaydolunur, erişebileceğiniz tüm laboratuvarları görüntüleyin, laboratuvarda bir VM'yi başlatın/durdurabilirsiniz ve VM'ye bağlanmanız açıklanmaktadır. 

## <a name="register-to-the-lab"></a>Laboratuvara kaydolun

1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. Kaydı tamamladıktan sonra kayıt URL'sini kullanmanız gerekmez. Bunun yerine, URL'yi kullanın: [https://labs.azure.com](https://labs.azure.com). Internet Explorer 11 henüz desteklenmedi. 
1. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 

    > [!NOTE]
    > Azure Laboratuvar Hizmetlerini kullanmak için bir Microsoft hesabı gereklidir. Portalda oturum kurmak için Yahoo veya Google hesapları gibi Microsoft hesabı olmayan hesabınızı kullanmaya çalışıyorsanız, Microsoft hesabınızla bağlantılı olacak bir Microsoft hesabı oluşturmak için yönergeleri izleyin. Ardından, kayıt işlemini tamamlamak için adımları izleyin. 
1. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 
1. Sanal makine hazır olana kadar bekleyin. VM döşemesi üzerinde aşağıdaki alanlara dikkat edin:
    1. Döşemenin üst **kısmında, laboratuvarın adını**görüyorsunuz.
    1. Sağında, VM işletim sistemini **(OS)** temsil eden simgeyi görürsünüz. Bu örnekte, Windows os's. 
    1. VM'yi başlatmak/durdurmak ve VM'ye bağlanmak için döşemenin alt kısmında simgeler/düğmeler görürsünüz. 
    1. Düğmelerin sağında VM'nin durumunu görürsünüz. VM'nin durumunu gördüğünüzü doğrulayın **Durdurulur.**

        ![Durdurulmuş durumda VM](../media/tutorial-connect-vm-in-classroom-lab/vm-in-stopped-state.png)

## <a name="start-or-stop-the-vm"></a>VM'yi başlatın veya durdurun
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
    1. Laboratuvar **VM'ye** bağlanmak için mac kullanıyorsanız, sonraki bölümdeki yönergeleri izleyin. 

## <a name="progress-bar"></a>İlerleme çubuğu 
Döşemedeki ilerleme çubuğu, size atanan [kota saat](how-to-configure-student-usage.md#set-quotas-for-users) sayısına göre kullanılan saat sayısını gösterir. Bu süre, laboratuvar için zamanlanan süreye ek olarak size ayrılan ek süredir. İlerleme çubuğunun rengi ve ilerleme çubuğunun altındaki metin aşağıdaki senaryolara göre değişir:

- Bir sınıf devam ediyorsa (sınıfın zamanlaması içinde), devam çubuğu kota saatlerinin kullanılmadığını temsil edecek şekilde gri renkte dir. 

    ![Gri renkli ilerleme çubuğu](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-class-in-progress.png)
- Bir kota atanmamışsa (sıfır saat), **sınıflar sırasında kullanılabilir** metin yalnızca ilerleme çubuğunun yerine gösterilir. 
    
    ![Kota belirlenmediğinde durum](../media/tutorial-connect-vm-in-classroom-lab/available-during-class.png)
- **Kotanız bittiyse,** ilerleme çubuğunun rengi **kırmızıdır.** 

    ![Kırmızı renkli ilerleme çubuğu](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-red-color.png)
- İlerleme çubuğunun rengi, laboratuar için zamanlanan saatin dışında olduğunda ve kota süresinin bir kısmı kullanıldığında **mavidir.** 

    ![Mavi renkli ilerleme çubuğu](../media/tutorial-connect-vm-in-classroom-lab/progress-bar-blue-color.png)


## <a name="view-all-the-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleme
Laboratuvarlara kaydolduktan sonra aşağıdaki adımları atarak tüm sınıf laboratuvarlarını görüntüleyebilirsiniz: 

1. 'ye [https://labs.azure.com](https://labs.azure.com)gidin. Internet Explorer 11 henüz desteklenmedi. 
2. Laboratuvara kaydolmak için kullandığınız kullanıcı hesabını kullanarak hizmette oturum açın. 
3. Erişebildiğiniz tüm laboratuvarları gördüğünüzden onaylayın. 

    ![Tüm laboratuvarları görüntüleme](../media/how-to-manage-classroom-labs/all-labs.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
 