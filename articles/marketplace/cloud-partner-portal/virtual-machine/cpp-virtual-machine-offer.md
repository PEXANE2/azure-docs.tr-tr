---
title: Azure Marketi'nde sanal makine teklifi
description: Azure Marketi'nde bir VM teklifi yayımlama işlemine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: dsindona
ms.openlocfilehash: 0f2ae9fe6f006b5418ebee82b08a44188b7c58d3
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273078"
---
# <a name="virtual-machine-offer"></a>Sanal makine teklifi

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Bu bölümde, Azure Marketi'nde yeni [Azure Marketplace](https://azuremarketplace.microsoft.com)bir sanal makine teklifinin nasıl yayımlanabileceği açıklanmaktadır. Bir işletim sistemi sanal sabit disk (VHD) ve sıfır veya daha fazla veri VHD'leri içeren hem Windows hem de Linux tabanlı sanal makineler için destek sağlanır. | ![sanal makine simgesi](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>Yayımlama genel bakış

Aşağıdaki video olan [Azure Marketinizi Optimize Edin,](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player)bu pazarda nasıl yayınlayacağınızı (sanal makine çözümünün kullanılması), ürün sayfanızla kullanıcı deneyimini nasıl optimize edebileceğiniz ve isteğe bağlı Test Sürüşü deneyimi, kullanıcı müşteri adaylarının nasıl oluşturulduğu ve bunları nasıl tüketebileceğiniz ve müşteri etkileşimini optimize edebileceğiniz dahil olmak üzere Azure Marketi'ne geniş bir genel bakış sunar.

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>VM Yayıncılık proses akışı

Aşağıdaki diyagram, VM teklifini yayımlamadaki üst düzey adımları göstermektedir. 

![VM yayımlama süreci](./media/publishvm_001.png)

1. Teklifi oluşturun - Teklif tanımı, pazarlama malzemeleri, yasal, destek bilgileri ve varlık özellikleri de dahil olmak üzere teklifle ilgili tüm ayrıntılar ve bilgiler yapılandırılır.

2. İş ve teknik varlıkları oluşturun - İlişkili çözüm için iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıkları (burada, VM'ler ve ekli diskler) oluşturun. 

3. SKU'yu oluşturun - Teklifle ilişkili SKU(lar) oluşturun ve bunları gönderin.  Yayınlamayı planladığınız her resim için benzersiz bir SKU gereklidir. 
 
4. Teklifi onaylayın ve yayınlayın - Teklif ve teknik varlıklar tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderim, çözümün test edildiği, doğrulandığı, onaylandığı, daha sonra pazarda "yayına geçtiği" yayımlama sürecini başlatacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce, Microsoft Azure Marketi'nde bir VM yayımlamak için [teknik ve iş gereksinimlerini](./cpp-prerequisites.md) karşılamanız gerekir. 
