---
title: Azure DevTest Labs'de laboratuvar oluşturma | Microsoft Belgeleri
description: Bu makale, Azure portalını ve Azure DevTest Laboratuvarlarını kullanarak bir laboratuvar oluşturma sürecinde size yol açar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 5cd675823b85e975dcb8dfe152c27b2d30463c1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759746"
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma
Azure DevTest Labs'deki bir laboratuvar, Sanal Makineler (VM'ler) gibi kaynaklardan oluşan grupları kapsayan ve limitlerin yanı sıra kotalar belirleyerek söz konusu kaynakları daha iyi yönetmenizi sağlayan bir altyapıdır. Bu makalede, Azure portalını kullanarak laboratuvar oluşturma işlemi ayrıntılı olarak açıklanmıştır.

## <a name="prerequisites"></a>Ön koşullar
Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma adımları
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. Sol taraftaki ana menüden **Tüm Hizmetler**'i (listenin en üst kısmında) seçin. **DEVOPS** **bölümündeKi DevTest Labs'ın** yanındaki * (yıldız) seçeneğini belirleyin. Bu eylem, bir sonraki sefere kolayca erişebilmeniz için **DevTest Labs'ı** sol navigasyon menüsüne ekler. 

    ![Tüm hizmetler - DevTest Labs'ı seçin](./media/devtest-lab-create-lab/all-services-select.png)
2. Şimdi, sol daki gezinme menüsünden **DevTest Labs'ı** seçin. Araç çubuğunda **Ekle'yi** seçin. 
   
    ![Laboratuvar ekleme](./media/devtest-lab-create-lab/add-lab-button.png)
1. **DevTest Lab oluştur** sayfasında aşağıdaki işlemleri yapın: 
    1. Laboratuvar için bir **ad** girin.
    2. Laboratuvarla ilişkilendirilecek **Abonelik** seçimini yapın.
    3. Laboratuvar **için kaynak grubu için** bir ad girin. 
    4. Laboratuarı depolamak için bir **konum** seçin.
    4. **Otomatik kapatma**’yı seçerek laboratuvarın tüm VM’lerinin otomatik olarak kapatılmasını etkinleştirmek isteyip istemediğinizi belirleyin ve buna yönelik parametreleri tanımlayın. VM'nin otomatik olarak kapatılmasını istediğiniz zamanı belirtmenizi sağlayan otomatik kapanma özelliği temel olarak maliyet tasarrufu sağlayan bir özelliktir. Makalede özetlenen adımları izleyerek laboratuarı oluşturduktan sonra otomatik kapatma ayarlarını değiştirebilirsiniz [Azure DevTest Labs'daki bir laboratuvar için tüm ilkeleri yönet.](./devtest-lab-set-lab-policy.md#set-auto-shutdown)
    1. Laboratuvarda oluşturacağınız tüm kaynaklara eklenecek özel etiketler oluşturmak istiyorsanız **Etiketler** için **AD** ve **DEĞER** bilgilerini girin. Laboratuvarları kategorilere göre yönetmek ve düzenlemek istiyorsanız etiketler faydalı olacaktır. Laboratuvarı oluşturduktan sonra etiket ekleme dahil olmak üzere etiketler hakkında daha fazla bilgi için bkz. [Laboratuvara etiket ekleme](devtest-lab-add-tag.md).
    6. Yapılandırma otomasyonu yönelik Azure Resource Manager şablonlarını almak için **Otomasyon seçenekleri**’ni seçin. 
    7. **Oluştur'u**seçin. **Bildirim** alanını gözlemleyerek laboratuvar oluşturma işleminin durumunu izleyebilirsiniz. 
    
        ![DevTest Labs laboratuvar bölümü oluşturma](./media/devtest-lab-create-lab/create-devtestlab-blade.png)
    8. Tamamlandıktan sonra, bildirimde **kaynağa git'i** seçin. Alternatif olarak, **laboratuvarlar** listesinde yeni oluşturulan laboratuarı görmek için DevTest Labs sayfasını yenileyin.  Listedeki laboratuarı seçin. Laboratuvarınızın ana sayfasını görüyorsunuz. 

        ![Laboratuvar için ana sayfa](./media/devtest-lab-create-lab/lab-home-page.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

