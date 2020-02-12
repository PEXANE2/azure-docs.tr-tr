---
title: Silinen bir panoyu Azure portal kurtar | Microsoft Docs
description: Azure portal yayımlanmış bir panoyu silerseniz panoyu kurtarabilirsiniz.
services: azure-portal
author: mgblythe
ms.author: mblythe
ms.date: 01/21/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: af0c72f0bc5dd8f3a3cbae7b82b1ac56447cbb08
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133285"
---
# <a name="recover-a-deleted-dashboard-in-the-azure-portal"></a>Silinen bir panoyu Azure portal kurtarma

Ortak Azure bulutunuz varsa ve Azure portal _yayımlanmış_ bir panoyu silerseniz, bu panoyu silmenin 14 gün içinde kurtarabilirsiniz. Azure Kamu bulutunda çalışıyorsanız veya Pano yayınlanmamışsa, bu dosyayı kurtaramazsınız ve yeniden oluşturmanız gerekir. Pano yayımlama hakkında daha fazla bilgi için bkz. [panoyu Yayımla](azure-portal-dashboard-share-access.md#publish-dashboard). Yayımlanan bir panoyu kurtarmak için şu adımları izleyin:

1. Azure portal menüsünden **kaynak grupları**' nı seçin ve ardından panoyu yayımladığınız kaynak grubunu seçin (varsayılan olarak, **panolar**olarak adlandırılır).

1. **Etkinlik günlüğü**altında **panoyu silme** işlemini genişletin. **Değişiklik geçmişi** sekmesini seçin ve **\<kaynak\>silindi** ' i seçin.

    ![Değişiklik geçmişi sekmesinin ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-tab.png)

1. Sol bölmenin içeriğini seçip kopyalayın ve ardından _. JSON_ dosya uzantısıyla bir metin dosyasına kaydedin. Portal, Panoyu yeniden oluşturmak için JSON dosyasını kullanır.

    ![Değişiklik geçmişi farkı 'nin ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-diff.png)

1. Azure portal menüsünde **panolar**' ı seçin ve ardından **karşıya yükle**' yi seçin.

    ![Pano karşıya yükleme ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-upload.png)

1. Kaydettiğiniz JSON dosyasını seçin. Portal silinen Pano ile aynı ad ve öğelerle Panoyu yeniden oluşturur.

1. Panoyu yayımlamak ve uygun erişim denetimini yeniden kurmak için **Share** ' u seçin.

    ![Pano paylaşımının ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-share.png)
