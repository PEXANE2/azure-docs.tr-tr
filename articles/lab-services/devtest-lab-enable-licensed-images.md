---
title: Azure DevTest Labs'daki laboratuvarınızda lisanslı bir görüntü etkinleştirin | Microsoft Dokümanlar
description: Azure portalını kullanarak Azure DevTest Labs'da lisanslı bir görüntüyü nasıl etkinleştirmenizi öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 221390d2-8d3b-4e1f-b454-43d33f8072b7
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 11b6553fe8aceef0d3d15977998dd870c275128a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61294344"
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Azure DevTest Laboratuvarları'ndaki laboratuvarınızda lisanslı bir görüntü etkinleştirme

Azure DevTest Labs'da lisanslı bir görüntü, genellikle üçüncü bir taraftan gelen hüküm ve koşulları içeren ve görüntünün laboratuvardaki kullanıcılar tarafından erişilebilir hale gelmeden önce kabul edilmesi gereken bir görüntüdür. Aşağıdaki bölümlerde, sanal makineler oluşturmak için kullanılabilen lisanslı görüntülerle nasıl çalışılabildikleri açıklanmaktadır.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Lisanslı bir görüntünün kullanıcılar tarafından kullanılabilip kullanılmadığını belirleme
Kullanıcıların lisanslı bir görüntüden VM oluşturmasına izin vermenin ilk adımı, hüküm ve koşulların lisanslı görüntü için kabul edildiğinden emin olmaktır. Aşağıdaki adımlar, lisanslı bir resmin teklif durumunu nasıl görüntüleyebileceğinizi ve gerekirse şart ve koşullarını nasıl kabul edebileceğinizi gösterir.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

1. **SETTINGS**altındaki sol panelde **Yapılandırma ve ilkeler'i**seçin.

1. **VIRTUAL MACHINE BASES**altındaki sol panelde, **Pazar görüntüleri**seçin. 

    ![Pazar yeri görüntüleri menü öğesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Her görüntü için **TEKLIF DURUMU** da dahil olmak üzere, kullanılabilir tüm pazar yeri görüntülerinin listesi gösterilir.

    ![Her resim için teklif durumunu gösteren pazar görüntüleri listesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Lisanslı bir resim, 
    
    - **Kabul edilen şartlar:** lisanslı resim, Kullanıcılar tarafından VM'ler oluşturmak için kullanılabilir. 
    - **Terimler gözden geçirme gerekli:** lisanslı görüntü şu anda kullanıcılar için kullanılabilir değildir. Laboratuvar kullanıcılarının VM'ler oluşturmak için kullanabilmesi için lisansın hüküm ve koşullarının kabul edilmesi gerekir. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Lisanslı bir görüntüyü laboratuvar kullanıcılarının kullanımına sun
Lisanslı bir görüntünün laboratuvar kullanıcıları tarafından kullanılabildiğinden emin olmak için, yönetici izinlerine sahip bir laboratuvar sahibinin önce bu lisanslı görüntünün hüküm ve koşullarını kabul etmesi gerekir. Lisanslı bir resimle ilişkili abonelik için programatik dağıtımı etkinleştirmek, söz resmin yasal koşullarını ve gizlilik bildirimlerini otomatik olarak kabul eder. [Azure Kaynak Yöneticisi'nde Market Görselleri ile çalışmak,](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) pazar yeri görüntülerinin programatik dağıtımı hakkında ek bilgiler sağlar.

Lisanslı bir görüntü için programatik dağıtımı aşağıdaki adımları izleyerek etkinleştirebilirsiniz:

1. Azure [portalında,](https://go.microsoft.com/fwlink/p/?LinkID=525040) **Market görüntüleri**listesine gidin.

1. Kullanıcıların erişebilmesini istediğiniz ancak şartları kabul edilmeyen lisanslı bir resim tanımlayın. Örneğin, Kabul Edilen Terimler in durumunu veya Gerekli **Terimler** gözden **geçirmesini**gösteren bir Veri Bilimi Sanal Makinesi görebilirsiniz.

    ![Programlı Dağıtım pencereyi yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Veri Bilimi SANAL M'leri, veri analitiği, makine öğrenimi ve Yapay A eğitimi için yaygın olarak kullanılan çeşitli popüler araçlarla önceden yüklenmiş, yapılandırılmış ve test edilmiş Azure Sanal Makine görüntüleridir. [Linux ve Windows için Azure Veri Bilimi Sanal Makine'ye giriş,](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) DSVM'ler hakkında çok sayıda bilgi sağlar.
   >
   >

1. Görüntü için **OFFER STATUS** sütununda, Gerekli Şartlar **gözden geçirmesini**seçin.

1. Programlı Dağıtımı Yapılandır penceresinde **Etkinleştir'i**seçin.

    ![Programlı Dağıtım pencereyi yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > Programlı Dağıtımı Yapılandır penceresinde birden çok abonelik görebilirsiniz. Yalnızca amaçlanan abonelik için programatik dağıtımı etkinleştirdiğinizden emin olun.
   >
   >


1. **Kaydet'i**seçin. 

    Pazar yeri görüntüleri listesinde, bu resim artık **kabul edilen Koşulları** gösterir ve kullanıcıların sanal makineler oluşturması için kullanılabilir.

> [!NOTE]
> Kullanıcılar lisanslı bir görüntüden özel bir görüntü oluşturabilir. Bkz. Daha fazla bilgi için [bir VHD dosyasından özel bir resim oluşturun.](devtest-lab-create-template.md)
>
>


## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında Özel Görüntüler Kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [VM’den özel görüntü oluşturma](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [VHD dosyasından özel görüntü oluşturma](devtest-lab-create-template.md)
- [Laboratuvarınıza VM ekleme](devtest-lab-add-vm.md)