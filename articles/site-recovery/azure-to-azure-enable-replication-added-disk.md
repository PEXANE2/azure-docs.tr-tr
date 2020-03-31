---
title: Azure Site Kurtarma'da eklenen bir Azure VM diski için çoğaltmayı etkinleştirme
description: Bu makalede, Azure Site Kurtarma ile olağanüstü durum kurtarma için etkinleştirilen bir Azure VM'ye eklenen bir disk için çoğaltmanın nasıl etkinleştirilen
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/29/2019
ms.openlocfilehash: 6cbbe63d7968816de78256f5a8408517bb8da278
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973806"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Azure VM'ye eklenen bir disk için çoğaltmayı etkinleştirme


Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md)kullanarak başka bir Azure bölgesine olağanüstü durum kurtarma için zaten etkin leştirilmiş bir Azure VM'sine eklenen veri diskleri için çoğaltmanın nasıl etkinleştirilen açıklanmaktadır.

VM'ye eklediğiniz bir disk için çoğaltmayı etkinleştirme, yönetilen disklerle Azure VM'leri için desteklenir.

Başka bir Azure bölgesine çoğalan bir Azure VM'sine yeni bir disk eklediğinizde aşağıdakiler oluşur:

-   VM için çoğaltma durumu bir uyarı gösterir ve portaldaki bir not, bir veya daha fazla diskin korunmak için kullanılabildiğini bildirir.
-   Eklenen diskler için koruma yı etkinleştiriseniz, uyarı diskin ilk çoğaltıldıktan sonra kaybolur.
-   Disk için çoğaltmayı etkinleştirmemeyi seçerseniz, uyarıyı reddetmeyi seçebilirsiniz.

![Yeni disk eklendi](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Başlamadan önce

Bu makalede, diskeklediğiniz VM için olağanüstü durum kurtarma zaten ayarladığınızı varsayar. Yapmadıysanız, [Azure'dan Azure'a](azure-to-azure-tutorial-enable-replication.md)olağanüstü durum kurtarma öğreticisini izleyin.

## <a name="enable-replication-for-an-added-disk"></a>Eklenen disk için çoğaltmayı etkinleştirme

Eklenen bir disk için çoğaltmayı etkinleştirmek için aşağıdakileri yapın:

1. **Çoğaltılan Öğeler**> kasasında, diski eklediğiniz VM'yi tıklatın.
2. **Diskler'i**tıklatın ve ardından çoğaltmayı etkinleştirmek istediğiniz veri diskini seçin (bu disklerin **korumalı olmayan** bir durumu vardır).
3.  **Disk Ayrıntıları'nda**, **çoğaltmayı etkinleştir'i**tıklatın.

    ![Eklenen disk için çoğaltmayı etkinleştirme](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Etkinleştirçoğaltma işi çalışır ve ilk çoğaltma bittikten sonra, disk sorunu için çoğaltma sistem durumu uyarısı kaldırılır.



## <a name="next-steps"></a>Sonraki adımlar

Bir test başarısızlığını çalıştırma hakkında [daha fazla bilgi edinin.](site-recovery-test-failover-to-azure.md)
