---
title: Azure Lab Hizmetleri'nde sınıf laboratuvarı şablonu yönetme | Microsoft Dokümanlar
description: Azure Lab Hizmetleri'nde sınıf laboratuvarı şablonu oluşturma ve yönetme yi öğrenin.
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
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: fcf31fcc266358911612c25e0b73a0a9de696b1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77539049"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde sınıf şablonu oluşturma ve yönetme
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Ardından, şablon VM örneklerini laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için şablonu yayımlarsınız. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir.

Bu makalede, Azure Laboratuvar Hizmetleri'nin sınıf laboratuvarında şablon sanal makinenin nasıl oluşturulup yönetilen anlatılmaktadır. 

## <a name="set-or-update-template-title-and-description"></a>Şablon başlığını ve açıklamasını ayarlama veya güncelleştirme
Başlığı ve açıklamayı ilk kez ayarlamak ve daha sonra güncelleştirmek için aşağıdaki adımları kullanın. 

1. **Şablon** sayfasında, laboratuar için yeni **başlığı** girin.  
2. Şablon için yeni **açıklamayı** girin. Odağı metin kutusunun dışına taşıdığınızda, otomatik olarak kaydedilir. 

    ![Şablon adı ve açıklaması](../media/how-to-create-manage-template/template-name-description.png)

## <a name="update-a-template-vm"></a>Şablon VM'yi güncelleştirme
Bir şablon VM'i güncelleştirmek için aşağıdaki adımları kullanın.  

1. Şablon VM başlatılana kadar bekleyin ve ardından şablon VM'ye bağlanmak için araç çubuğundaki **şablona bağlan'ı** seçin ve yönergeleri izleyin. Windows makinesiyse, RDP dosyasını karşıdan yükleme seçeneği görürsünüz. 
1. Şablona bağlandığınızda ve değişiklik yaptığınızda, artık kullanıcılarınız için en son yayınlanan sanal makinelerle aynı kuruluma sahip olmayacaktır. Şablon değişiklikleri, siz yeniden yayımlanınana kadar kullanıcılarınızın mevcut sanal makinelerine yansıtılmayacaktır.

    ![Şablon VM'ye bağlanma](../media/how-to-create-manage-template/connect-template-vm.png)
    
1. Öğrencilerinizin laboratuvarda ihtiyaç duyacağı uygulamaları (Visual Studio, Azure Depolama Gezgini gibi) yükleyin. 
1. Şablon VM bağlantısını kesin (uzak masaüstü oturumunuzu kapatın). 
1. **Durdur şablonu**seçerek şablon VM'yi **durdurun.** 
1. Güncelleştirilmiş vm şablonu **yayımlamak** için sonraki bölümdeki adımları izleyin. 

## <a name="publish-the-template-vm"></a>Şablon VM'yi yayımlama  
Bu adımda, VM şablonu yayımlarsınız. VM şablonu yayımladığınızda, Azure Lab Hizmetleri şablonu kullanarak laboratuvarda VM'ler oluşturur. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir.


1. **Şablon** sayfasında, araç çubuğunda **Yayımla'yı** seçin. 

    ![Şablon düğmesini yayımla](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Yayımlama işlemini geri alamazsınız. 
2. **Şablonu Yayımla** sayfasında, laboratuvarda oluşturmak istediğiniz sanal makine sayısını girin ve ardından **Yayımla'yı**seçin. 

    ![Şablon yayımla - VM sayısı](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
3. Şablonu sayfada **yayımlama durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
4. Yayımlama tamamlanana kadar bekleyin ve sol menüdeki Sanal **makineleri** seçerek veya **Sanal makineler** döşemesini seçerek **Sanal makineler havuzu** sayfasına geçin. Atanmamış durumda olan sanal makineleri gördüğünüzden **onaylayın.** Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)
## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
