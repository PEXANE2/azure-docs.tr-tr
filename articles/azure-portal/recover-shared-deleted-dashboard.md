---
title: Azure portalında silinen bir panokurtarma | Microsoft Dokümanlar
description: Azure portalında yayınlanan bir panonu silerseniz, panoyu kurtarabilirsiniz.
services: azure-portal
author: mgblythe
ms.author: mblythe
ms.date: 01/21/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: af0c72f0bc5dd8f3a3cbae7b82b1ac56447cbb08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77133285"
---
# <a name="recover-a-deleted-dashboard-in-the-azure-portal"></a>Azure portalında silinen bir pano kurtarma

Herkese açık Azure bulutundaysan ve Azure portalında _yayınlanmış_ bir panoyu silerseniz, bu panoyu silmeden sonraki 14 gün içinde kurtarabilirsiniz. Bir Azure devlet bulutundaysanız veya pano yayınlanmıyorsa, onu kurtaramazsınız ve yeniden oluşturmanız gerekir. Pano yayımlama hakkında daha fazla bilgi için [bkz.](azure-portal-dashboard-share-access.md#publish-dashboard) Yayımlanmış bir panoyu kurtarmak için aşağıdaki adımları izleyin:

1. Azure portalı menüsünden **Kaynak gruplarını**seçin ve ardından panoyu yayınladığınız kaynak grubunu seçin (varsayılan olarak **panoadı**verilir).

1. **Etkinlik günlüğü**altında, **Pano Sil** işlemini genişletin. Geçmişi **Değiştir** sekmesini seçin ve ** \<ardından silinen\>kaynağı**seçin.

    ![Değişiklik geçmişi sekmesinin ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-tab.png)

1. Sol bölmenin içeriğini seçin ve kopyalayın, ardından _.json_ dosya uzantısı olan bir metin dosyasına kaydedin. Portal, panoyu yeniden oluşturmak için JSON dosyasını kullanır.

    ![Değişim geçmişi diff ekran görüntüsü](media/recover-shared-deleted-dashboard/change-history-diff.png)

1. Azure portalı menüsünden **Panolar'ı**seçin ve ardından **Yükle'yi**seçin.

    ![Pano yüklemesinin ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-upload.png)

1. Kaydettiğiniz JSON dosyasını seçin. Portal, silinen panoyla aynı ada ve öğelere sahip panoyu yeniden oluşturur.

1. Panoyu yayımlamak ve uygun erişim denetimini yeniden oluşturmak için **Paylaş'ı** seçin.

    ![Pano paylaşımının ekran görüntüsü](media/recover-shared-deleted-dashboard/dashboard-share.png)
