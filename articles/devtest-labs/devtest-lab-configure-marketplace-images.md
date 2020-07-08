---
title: Azure DevTest Labs Azure Marketi görüntü ayarlarını yapılandırma
description: Azure DevTest Labs bir VM oluştururken hangi Azure Market görüntülerinin kullanılabileceğini yapılandırın
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9fdb4e3a888e876f91b8af2e4854a9c101eea45c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482726"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs Azure Marketi görüntü ayarlarını yapılandırma
DevTest Labs, laboratuvarınızda kullanılacak Azure Marketi görüntülerini nasıl yapılandırdığınıza bağlı olarak, Azure Marketi görüntülerini temel alan sanal makineler oluşturmayı destekler. Bu makalede, bir laboratuvarda sanal makine oluştururken Azure Marketi görüntülerinin kullanılabileceği, ne yapılacağını nasıl belirtebileceğiniz gösterilmektedir. Bu, takımınızın yalnızca ihtiyaç duydukları Market görüntülerine erişimi olmasını sağlar. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>VM oluştururken hangi Azure Market görüntülerine izin verileceğini seçin
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin. 
4. Laboratuvarın dikey penceresinde **yapılandırma ve ilkeler**' i seçin.
5. Laboratuvarın **yapılandırma ve ilkeler** dikey penceresinde, **sanal makine tabanları**altında **Market görüntüleri**' ni seçin.
6. Tüm Azure Marketi görüntülerinin yeni bir VM 'nin temeli olarak kullanılabilmesini isteyip istemediğinizi belirtin. **Evet**' i seçerseniz, laboratuvarda aşağıdaki ölçütlere uyan tüm Azure Market görüntülerine izin verilir:
   
   * Görüntü tek bir VM oluşturur **ve**
   * Görüntü, VM 'Leri sağlamak için Azure Resource Manager kullanır **ve**
   * Görüntü, ek bir lisans planının satın alınmasını gerektirmez
     
     İzin verilmesini istiyorsanız veya hangi görüntülerin kullanılabileceğini belirtmek istiyorsanız **Hayır**' ı seçin.
     
     ![Tüm Market görüntülerinin VM 'Ler için temel görüntü olarak kullanılmasına izin verme seçeneği](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Önceki adımda **Hayır** ' ı seçerseniz, **izin verilen görüntüler/Tümünü Seç** onay kutusu etkin olur. 
   Listede görünen tüm öğeleri hızlıca seçmek veya seçimlerini kaldırmak için bu seçeneği arama kutusuyla birlikte kullanabilirsiniz.
   * Her bir görüntünün karşılık gelen onay kutusunu işaretleyerek sanal makine oluşturmaya yönelik olarak izin vermek istediğiniz Azure Marketi görüntülerini seçin.
   * Laboratuvarda herhangi bir Azure Marketi görüntülerinin kullanılmasına izin vermek istemiyorsanız listeden hiçbir şey seçin.
   
     ![Hangi Azure Marketi görüntülerinin VM 'Ler için temel görüntü olarak kullanılabileceğini belirtebilirsiniz](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken Azure Marketi görüntülerinin nasıl izin verileceğini yapılandırdıktan sonra, bir sonraki adım [laboratuvara BIR VM eklemektir](devtest-lab-add-vm.md).

