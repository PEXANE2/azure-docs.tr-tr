---
title: CloudSimple hızlı başlangıç-Azure VMware çözümü-hizmet oluşturma
description: CloudSimple hizmeti, satın alma düğümleri ve ayrılan düğümleri oluşturma hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b20ff261939dd97a74d27f5ec7f21eae2665f474
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69574564"
---
# <a name="quickstart---create-cloudsimple-service"></a>Hızlı başlangıç-CloudSimple hizmeti oluşturma

Başlamak için Azure portal Azure VMware çözümünü CloudSimple ile oluşturun.

CloudSimple hizmeti, CloudSimple ile Azure VMware çözümünü kullanmanıza olanak sağlar.  Hizmetin oluşturulması, düğüm satın almanızı, düğümleri ayırmanızı ve özel bulutlar oluşturmanızı sağlar.  Cloudsimple hizmetini, CloudSimple hizmetinin kullanılabildiği her bir Azure bölgesine eklersiniz.  Hizmet, CloudSimple tarafından Azure VMware çözümünün Edge ağını tanımlar.  Bu Edge ağı VPN, ExpressRoute ve özel Bulutlarınıza Internet bağlantısı içeren hizmetler için kullanılır.

CloudSimple hizmetini eklemek için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, Edge ağı oluşturulurken kullanılır ve bir/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Şirket içi ağ adresi alanları veya Azure sanal ağ adres alanı ile çakışamaz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="enable-microsoftvmwarecloudsimple-resource-provider"></a>Microsoft. Vmwarechoparlör basit kaynak sağlayıcısını etkinleştir

CloudSimple hizmeti için kaynak sağlayıcısını etkinleştirmek üzere aşağıdaki adımları izleyin.

1. **Tüm Hizmetler**’i seçin.
2. **Abonelik**arayın ve seçin.

    ![Abonelikleri seçin](media/cloudsimple-service-select-subscriptions.png)

3. CloudSimple hizmetini etkinleştirmek istediğiniz aboneliği seçin.
4. Abonelik için **kaynak sağlayıcıları** ' na tıklayın.
5. Kaynak sağlayıcısını filtrelemek için **Microsoft. Vmwarecses Simple** kullanın.
6. **Microsoft. Vmwarechoparlör basit** kaynak sağlayıcısını seçin ve **Kaydet**' e tıklayın.

    ![Kaynak sağlayıcısını kaydet](media/cloudsimple-service-enable-resource-provider.png)

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple hizmeti**için arama yapın.

    ![CloudSimple hizmeti ara](media/create-cloudsimple-service-search.png)

3. **Cloudsimple Hizmetleri**' ni seçin.
4. Yeni bir hizmet oluşturmak için **Ekle** ' ye tıklayın.

    ![CloudSimple hizmeti Ekle](media/create-cloudsimple-service-add.png)

5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CıDR girin. Şirket içi alt ağlarınızın, Azure alt ağlarının veya planlı CloudSimple alt ağlarının hiçbiriyle çakışmayacak bir/28 alt ağı belirtin. Hizmet oluşturulduktan sonra CıDR 'yi değiştiremezsiniz.

    ![CloudSimple hizmeti oluşturma](media/create-cloudsimple-service.png)

9. **Tamam**'ı tıklatın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="purchase-nodes"></a>Düğümleri satın alma

CloudSimple özel bulut ortamı için Kullandıkça Öde kapasitesini ayarlamak için, önce Azure portal düğümleri sağlayın.

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple düğümleri**için arama yapın.

    ![CloudSimple düğümlerinde arama yapın](media/create-cloudsimple-node-search.png)

3. **Cloudsimple düğümlerini**seçin.
4. Düğüm oluşturmak için **Ekle** ' ye tıklayın.

    ![CloudSimple düğümleri ekleme](media/create-cloudsimple-node-add.png)

5. CloudSimple düğümlerini satın almak istediğiniz aboneliği seçin.
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

* [Özel bulut oluşturma ve ortamı yapılandırma](quickstart-create-private-cloud.md)
* [Cloudsimple hizmeti](cloudsimple-service.md) hakkında daha fazla bilgi edinin
