---
title: CloudSimple-Azure tarafından VMware çözümü için düğüm sağlama
description: CloudSimple dağıtımıyla VMWare 'nize nasıl düğüm ekleneceğini öğrenin
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5806198968d98fea4c5cbf8731358ca4041f0935
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972880"
---
# <a name="provision-nodes-for-azure-vmware-solution-by-cloudsimple"></a>CloudSimple tarafından Azure VMware çözümü için düğüm sağlama

Azure portal düğümleri sağlayın. Daha sonra, CloudSimple özel bulut ortamınız için Kullandıkça Öde kapasitesini ayarlayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="add-a-node-to-your-cloudsimple-private-cloud"></a>CloudSimple özel buluta bir düğüm ekleyin

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple düğümleri**için arama yapın.

   ![CloudSimple düğümlerinde arama yapın](media/create-cloudsimple-node-search.png)

3. **Cloudsimple düğümlerini**seçin.
4. Düğüm oluşturmak için **Ekle** ' ye tıklayın.

    ![CloudSimple düğümleri ekleme](media/create-cloudsimple-node-add.png)

5. CloudSimple düğümlerini sağlamak istediğiniz aboneliği seçin.
6. Düğümlerin kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Düğümleri tanımlamak için ön eki girin.
8. Düğüm kaynaklarının konumunu seçin.
9. Düğüm kaynaklarını barındıracak ayrılmış konumu seçin.
10. Düğüm türünü seçin. [CS28 veya CS36 seçeneğini](cloudsimple-node.md)belirleyebilirsiniz. İkinci seçenek, en yüksek işlem ve bellek kapasitesini içerir.
11. Sağlanacak düğüm sayısını seçin.
12. **Gözden geçir + oluştur**' u seçin.
13. Ayarları gözden geçirin. Ayarları değiştirmek için **önceki**'ni tıklatın.
14. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulut oluştur](create-private-cloud.md)
