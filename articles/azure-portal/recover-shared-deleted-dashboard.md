---
title: Azure portalında silinen bir panoyu kurtarma
description: Azure portal yayımlanmış bir panoyu silerseniz panoyu kurtarabilirsiniz.
ms.date: 01/21/2020
ms.topic: troubleshooting
ms.openlocfilehash: 095964691a3cb22f8a805af2e8fe37af4c47cb28
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96745630"
---
# <a name="recover-a-deleted-dashboard-in-the-azure-portal"></a>Azure portalında silinen bir panoyu kurtarma

Ortak Azure bulutunuz varsa ve Azure portal _yayımlanmış_ bir panoyu silerseniz, bu panoyu silmenin 14 gün içinde kurtarabilirsiniz. Azure Kamu bulutunda çalışıyorsanız veya Pano yayınlanmamışsa, bu dosyayı kurtaramazsınız ve yeniden oluşturmanız gerekir. Pano yayımlama hakkında daha fazla bilgi için bkz. [panoyu Yayımla](azure-portal-dashboard-share-access.md#publish-dashboard). Yayımlanan bir panoyu kurtarmak için şu adımları izleyin:

1. Azure portal menüsünden **kaynak grupları**' nı seçin ve ardından panoyu yayımladığınız kaynak grubunu seçin (varsayılan olarak, **panolar** olarak adlandırılır).

1. **Etkinlik günlüğü** altında **panoyu silme** işlemini genişletin. **Değişiklik geçmişi** sekmesini seçin ve ardından öğesini seçin **\<deleted resource\>** .

    ![Değişiklik geçmişi sekmesinin ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-tab.png)

1. Sol bölmenin içeriğini seçip kopyalayın ve ardından _. JSON_ dosya uzantısıyla bir metin dosyasına kaydedin. Portal, Panoyu yeniden oluşturmak için JSON dosyasını kullanır.

    ![Değişiklik geçmişi farkı 'nin ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-diff.png)

1. Azure portal menüsünde **panolar**' ı seçin ve ardından **karşıya yükle**' yi seçin.

    ![Pano karşıya yükleme ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-upload.png)

1. Kaydettiğiniz JSON dosyasını seçin. Portal silinen Pano ile aynı ad ve öğelerle Panoyu yeniden oluşturur.

1. Panoyu yayımlamak ve uygun erişim denetimini yeniden kurmak için **Share** ' u seçin.

    ![Pano paylaşımının ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-share.png)
