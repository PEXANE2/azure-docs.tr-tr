---
title: VMware çözümleri (AVS) için düğüm Sağlama-Azure
description: AVS dağıtımıyla VMWare 'nize nasıl düğüm ekleneceğini öğrenin
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 94ca681153f1e3ccd9ff628b41a9d0e547be5802
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024815"
---
# <a name="provision-nodes-for-azure-vmware-solutions-avs"></a>Azure VMware çözümleri (AVS) için düğüm sağlama

Azure portal düğümleri sağlayın. Daha sonra, AVS özel bulut ortamınız için Kullandıkça Öde kapasitesini ayarlayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="add-a-node-to-your-avs-private-cloud"></a>AVS özel buluta düğüm ekleme

1. **Tüm Hizmetler**’i seçin.
2. **AVS düğümlerini**arayın.

   ![AVS düğümlerinde ara](media/create-cloudsimple-node-search.png)

3. **AVS düğümlerini**seçin.
4. Düğüm oluşturmak için **Ekle** ' ye tıklayın.

    ![AVS düğümleri Ekle](media/create-cloudsimple-node-add.png)

5. AVS düğümlerini sağlamak istediğiniz aboneliği seçin.
6. Düğümlerin kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Düğümleri tanımlamak için ön eki girin.
8. Düğüm kaynaklarının konumunu seçin.
9. Düğüm kaynaklarını barındıracak ayrılmış konumu seçin.
10. [Düğüm türünü](cloudsimple-node.md)seçin.
11. Sağlanacak düğüm sayısını seçin.
12. **Gözden geçir + oluştur**' u seçin.
13. Ayarları gözden geçirin. Ayarları değiştirmek için **önceki**'ni tıklatın.
14. **Oluştur**'u seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutu oluştur](create-private-cloud.md)
