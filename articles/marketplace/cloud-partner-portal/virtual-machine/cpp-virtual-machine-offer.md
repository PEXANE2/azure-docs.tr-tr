---
title: Azure Marketi 'nde sanal makine teklifi
description: Azure Marketi 'nde VM teklifi yayımlama sürecine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: dsindona
ms.openlocfilehash: b857cf8b7485beb884adf77d99f82d965f55a0ad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142793"
---
# <a name="virtual-machine-offer"></a>Sanal makine teklifi

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Bu bölümde, [Azure Marketi](https://azuremarketplace.microsoft.com)'ne yeni bir sanal makine teklifinin nasıl yayımlanacağı açıklanmaktadır. Bir işletim sistemi sanal sabit diski (VHD) ve sıfır veya daha fazla veri VHD 'si içeren hem Windows tabanlı hem de Linux tabanlı sanal makineler için destek sağlanır. | ![sanal makine simgesi](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>Yayımlamaya genel bakış

Aşağıdaki videoda, [Azure Market teklifinizi en iyileştirin](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player), Azure Market 'te yayımlama (bir sanal makine çözümü kullanılarak), ürün sayfanız ve Isteğe bağlı test sürücüsü deneyiminizle Kullanıcı deneyimini en uygun hale getirmeyi, Kullanıcı liderinizi nasıl kullanabileceğinizi ve müşteri katılımını en uygun hale getirmeyi de kapsayan genel bir bakış sunar.

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>VM yayımlama işlemi akışı

Aşağıdaki diyagramda bir VM teklifi yayımlamanın üst düzey adımları gösterilmektedir. 

![VM yayımlama işlemi](./media/publishvm_001.png)

1. Teklifi oluşturun-teklif açıklaması, pazarlama malzemeleri, yasal, destek bilgileri ve varlık belirtimleri dahil olmak üzere teklifle ilgili tüm ayrıntıları ve bilgileri yapılandırılır.

2. İş ve teknik varlıkları oluşturun-ilgili çözüme yönelik iş varlıklarını (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıkları oluşturun (burada, VM 'Ler ve bağlı diskler). 

3. SKU 'YU oluşturun-teklifle ilişkili ilgili SKU 'ları oluşturun ve bunları iletin.  Yayımlamayı planladığınız her görüntü için benzersiz bir SKU gereklidir. 
 
4. Teklifi onaylama ve yayımlama-teklif ve teknik varlıklar tamamlandığında teklifi gönderebilirsiniz. Bu gönderim, çözümün test edilmiş, doğrulanan, sertifikalı ve sonra Market 'teki "canlı" olduğu yayımlama işlemini başlatacak.  

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları kabul etmeden önce, bir VM 'yi Microsoft Azure Market yayımlamak için [Teknik ve iş gereksinimlerini](./cpp-prerequisites.md) karşılamanız gerekir. 
