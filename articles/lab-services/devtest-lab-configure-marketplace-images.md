---
title: Azure DevTest Labs'da Azure Marketi görüntü ayarlarını yapılandırın
description: Azure DevTest Labs'da VM oluştururken hangi Azure Marketi görüntülerinin kullanılabileceğini yapılandırın
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 2dc3aa000cefc0e65305b58f8fdce93b94bfd35f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169562"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs'da Azure Marketi görüntü ayarlarını yapılandırın
DevTest Labs, Azure Marketi görüntülerini laboratuvarınızda kullanılacak şekilde nasıl yapılandırdığınıza bağlı olarak Azure Marketi görüntülerine dayalı VM oluşturmayı destekler. Bu makalede, bir laboratuvarda VM oluştururken hangi Azure Marketi görüntülerinin kullanılabileceğini niçin belirtebileceğiniz gösterilmektedir. Bu, ekibinizin yalnızca gereksinim duydukları Market görüntülerine erişmesini sağlar. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>VM oluştururken hangi Azure Marketi görüntülerine izin verilebilenleri seçin
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin. 
4. Laboratuvarın bıçağında Yapılandırma **ve ilkeler'i**seçin.
5. **Sanal Makine Üsleri**altında laboratuvar Yapılandırma ve **politikaları** bıçak, **Pazar görüntüleri**seçin.
6. Tüm nitelikli Azure Marketi görüntülerinin yeni bir VM'nin temeli olarak kullanılabilir olmasını isteyip istemediğinizi belirtin. **Evet'i**seçerseniz, aşağıdaki tüm ölçütleri karşılayan tüm Azure Marketi resimlerine laboratuarda izin verilir:
   
   * Görüntü tek bir VM oluşturur **ve**
   * Görüntü, VM'leri sağlamak için Azure Kaynak Yöneticisi'ni kullanır **ve**
   * Görüntü, ek bir lisans planı satın alma gerektirmez
     
     Hiçbir görüntüye izin verilmemesini veya hangi görüntülerin kullanılabileceğini belirtmek istiyorsanız, **Hayır'ı**seçin.
     
     ![Tüm Market görüntülerinin VM'ler için temel görüntü olarak kullanılmasına izin verme seçeneği](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Önceki adıma **Hayır'ı** seçerseniz, **İzin Verilen görüntüler/Tüm** onay kutusunu seç etkinleştirilir. 
   Listede görüntülenen tüm öğeleri hızla seçmek veya seçmek için arama kutusuyla birlikte bu seçeneği kullanabilirsiniz.
   * Her resmin ilgili onay kutusunu kontrol ederek VM oluşturmaiçin ayrı ayrı izin vermek istediğiniz Azure Marketi görüntülerini seçin.
   * Laboratuvarda azure marketi görüntülerinin kullanılmasına izin vermek istemiyorsanız listeden hiçbir şey seçmeyin.
   
     ![VM'ler için temel resim olarak hangi Azure Marketi görüntülerinin kullanılabileceğini belirtebilirsiniz](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
VM oluştururken Azure Marketi görüntülerine nasıl izin verildiğini yapılandırıldıktan sonra, bir sonraki adım [laboratuvarınıza bir VM eklemektir.](devtest-lab-add-vm.md)

