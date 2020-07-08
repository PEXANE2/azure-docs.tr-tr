---
title: Azure DevTest Labs 'da laboratuvarınızda lisanslı bir görüntüyü etkinleştirin | Microsoft Docs
description: Azure portal kullanarak Azure DevTest Labs lisanslı bir görüntüyü nasıl etkinleştirebileceğinizi öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 6a4023a0107acf5aa4cabc2178d17f7be40301f0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85481026"
---
# <a name="enable-a-licensed-image-in-your-lab-in-azure-devtest-labs"></a>Azure DevTest Labs 'da laboratuvarınızda lisanslı bir görüntüyü etkinleştirin

Azure DevTest Labs, lisanslı bir görüntü, hüküm ve koşulları içeren bir, genellikle üçüncü bir şahıs tarafından, görüntünün laboratuvardaki kullanıcılar tarafından erişilebilmesi için kabul edilmesi gerekir. Aşağıdaki bölümlerde, sanal makineler oluşturmak için kullanılabilir olmaları için lisanslı görüntülerle nasıl çalışılacağı açıklanır.

## <a name="determining-whether-a-licensed-image-is-available-to-users"></a>Lisanslı bir görüntünün kullanıcılar için kullanılabilir olup olmadığını belirleme
Kullanıcıların lisanslı görüntüden VM oluşturmalarına izin vermenin ilk adımı, lisanslı görüntü için hüküm ve koşulların kabul edildiğinden emin olmanızı sağlar. Aşağıdaki adımlarda, lisanslı bir görüntünün teklif durumunu nasıl görüntüleyebileceğiniz gösterilmektedir ve gerekirse hüküm ve koşullarını kabul edebilirsiniz.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

1. Sol bölmede **Ayarlar**' ın altında **yapılandırma ve ilkeler**' i seçin.

1. Sol bölmede, **sanal makıne tabanları**' nın altında **Market görüntüleri**' ni seçin. 

    ![Market görüntüleri menü öğesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-marketplace-images.png)

    Her bir görüntünün **TEKLIF durumu** da dahil olmak üzere tüm kullanılabilir Market görüntülerinin listesi görüntülenir.

    ![Her görüntü için teklif durumunu gösteren Market görüntülerinin listesi](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-offer-status.png)

    Lisanslı bir görüntü, bir teklif durumu gösterir 
    
    - **Kabul edilen koşullar:** lisanslı görüntü, kullanıcıların VM 'ler oluşturmalarına olanak sağlar. 
    - **Koşulların incelenmesi gerekiyor:** lisanslı görüntü şu anda kullanıcılar tarafından kullanılamıyor. Laboratuvar kullanıcılarının VM oluşturmak için kullanabilmesi için önce lisansın hüküm ve koşullarının kabul edilmesi gerekir. 

## <a name="making-a-licensed-image-available-to-lab-users"></a>Lisanslı bir görüntüyü laboratuvar kullanıcıları için kullanılabilir hale getirme
Lisanslı bir görüntünün laboratuvar kullanıcıları tarafından kullanılabilir olduğundan emin olmak için yönetici izinlerine sahip bir laboratuvar sahibi öncelikle bu lisanslı görüntüye ait hüküm ve koşulları kabul etmelidir. Lisanslı bir görüntüyle ilişkili abonelik için programlı dağıtımı etkinleştirmek, söz konusu görüntüye ait yasal hüküm ve gizlilik bildirimlerini otomatik olarak kabul eder. [Azure Resource Manager Market görüntülerle çalışma](https://azure.microsoft.com/blog/working-with-marketplace-images-on-azure-resource-manager/) , Market görüntülerinin programlı dağıtımı hakkında ek bilgiler sağlar.

Aşağıdaki adımları izleyerek, lisanslı bir görüntü için programlı dağıtımı etkinleştirebilirsiniz:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) **Market görüntüleri**listesine gidin.

1. Kullanıcıların erişimine sahip olmasını istediğiniz ancak koşulları kabul edilmeyen bir lisanslı görüntü belirler. Örneğin, **kabul edilen koşulların** veya **koşulların gözden geçirdiğine**ilişkin bir durumu gösteren bir veri bilimi sanal makinesi görebilirsiniz.

    ![Programlı dağıtım penceresini yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-licensed-images.png)

   > [!NOTE]
   > Veri bilimi sanal makineleri, veri analizi, makine öğrenimi ve AI eğitimi için yaygın olarak kullanılan çeşitli popüler araçlarla Azure sanal makine görüntüleridir, önceden yüklenmiş, yapılandırılmıştır ve test edilmiştir. [Linux ve Windows Için Azure veri bilimi sanal makinesi 'ye giriş](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) , DSVMs hakkında harika bilgiler sağlar.
   >
   >

1. Görüntü için **TEKLIF durumu** sütununda, **koşulların incelenmesi gereken**' ı seçin.

1. Programlı dağıtımı Yapılandır penceresinde **Etkinleştir**' i seçin.

    ![Programlı dağıtım penceresini yapılandırma](./media/devtest-lab-create-custom-image-from-licensed-image/devtest-lab-enable-programmatic-deployment.png)

   > [!IMPORTANT]
   > Programsal dağıtım yapılandırma penceresinde birden çok abonelik listelendiğini görebilirsiniz. Yalnızca amaçlanan abonelik için programlı dağıtımı etkinleştirdiğinizden emin olun.
   >
   >


1. **Kaydet**'i seçin. 

    Market görüntüleri listesinde, bu görüntüde artık **kabul edilen şartlar** gösteriliyor ve kullanıcıların sanal makineler oluşturması için kullanılabilir.

> [!NOTE]
> Kullanıcılar, lisanslı bir görüntüden özel bir görüntü oluşturabilir. Daha fazla bilgi için bkz. [VHD dosyasından özel görüntü oluşturma](devtest-lab-create-template.md) .
>
>


## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller mi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [VM’den özel görüntü oluşturma](devtest-lab-create-custom-image-from-vm-using-portal.md)
- [Bir VHD dosyasından özel görüntü oluşturma](devtest-lab-create-template.md)
- [Laboratuvarınızda VM ekleme](devtest-lab-add-vm.md)