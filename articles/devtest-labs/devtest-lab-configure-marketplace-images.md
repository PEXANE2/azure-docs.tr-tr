---
title: Azure DevTest Labs Azure Marketi görüntü ayarlarını yapılandırma
description: Azure DevTest Labs bir VM oluştururken hangi Azure Market görüntülerinin kullanılabileceğini yapılandırın
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: a5158ed33bf253db1dbe0eb3232bc43d27ce15e7
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512444"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs Azure Marketi görüntü ayarlarını yapılandırma
DevTest Labs, laboratuvarınızda kullanılacak Azure Marketi görüntülerini nasıl yapılandırdığınıza bağlı olarak, Azure Marketi görüntülerini temel alan sanal makineler oluşturmayı destekler. Bu makalede, bir laboratuvarda sanal makine oluştururken Azure Marketi görüntülerinin kullanılabileceği, ne yapılacağını nasıl belirtebileceğiniz gösterilmektedir. Takımınızın yalnızca ihtiyaç duydukları Market görüntülerine erişimi olmasını sağlar. 

## <a name="specify-allowed-images-for-creating-vms"></a>VM 'Ler oluşturmak için izin verilen görüntüleri belirtme
Bir VM oluştururken hangi Azure Marketi görüntülerinin izin verileceğini belirtmek için bu adımları takip edin. 

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden laboratuvarınızı seçin. 
4. Laboratuvar sayfasının giriş sayfasında **yapılandırma ve ilkeler**' i seçin.
5. Laboratuvarın **yapılandırma ve ilkeler** sayfasında, **sanal makine tabanları**altında **Market görüntüleri**' ni seçin.
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


## <a name="troubleshoot"></a>Sorun giderme
Laboratuvar için etkinleştirmek üzere belirli bir görüntü bulamıyorsanız, şu adımları izleyin: 

- İşlem VM 'si oluştururken görüntüyü görüntüleyip görüntüleyegöremiyorsanız emin olun.
- Görüntü, kullandığınız abonelik türünde kullanılamayabilir. Abonelik türü için abonelik yöneticisine başvurun (örneğin: MSDN, ücretsiz, Kullandıkça Öde vb.). 
- DevTest Labs 'de Gen 2 görüntüleri için destek sınırlıdır. Bir görüntü için hem Gen 1 hem de Gen 2 sürümleri varsa, DevTest Labs VM oluştururken görüntünün yalnızca Gen 1 sürümünü gösterir. Geçici çözüm, laboratuvar dışında özel bir gen 2 görüntüsü oluşturmak ve bunu bir VM oluşturmak için kullanmaktır. Görüntünün yalnızca Gen 2 sürümü varsa, bu, DevTest Labs tarafından desteklenir ve listede gösterilir. 
      


## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken Azure Marketi görüntülerinin nasıl izin verileceğini yapılandırdıktan sonra, bir sonraki adım [laboratuvara BIR VM eklemektir](devtest-lab-add-vm.md).

