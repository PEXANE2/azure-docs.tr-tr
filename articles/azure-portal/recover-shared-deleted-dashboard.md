---
title: Azure portalında silinen bir panoyu kurtarma
description: Azure portal yayımlanmış bir panoyu silerseniz panoyu kurtarabilirsiniz.
ms.date: 01/21/2020
ms.topic: troubleshooting
ms.openlocfilehash: 2d0c8ad9ce93f3812dd810361e10a26232e29685
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104774907"
---
# <a name="recover-a-deleted-dashboard-in-the-azure-portal"></a>Azure portalında silinen bir panoyu kurtarma

Küresel Azure bulutunuz varsa ve Azure portal _yayımlanmış_ bir panoyu silerseniz, bu panoyu silmenin 14 gün içinde kurtarabilirsiniz. Azure Kamu bulutunda çalışıyorsanız veya Pano yayınlanmamışsa, bu dosyayı kurtaramazsınız ve yeniden oluşturmanız gerekir. Pano yayımlama hakkında daha fazla bilgi için bkz. [panoyu Yayımla](azure-portal-dashboard-share-access.md#publish-a-dashboard). Yayımlanan bir panoyu kurtarmak için şu adımları izleyin:

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
