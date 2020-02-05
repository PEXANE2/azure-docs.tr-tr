---
title: Azure VMware çözümleri (AVS) hızlı başlangıç-hizmet oluşturma
description: AVS hizmetini oluşturma, düğümleri satın alma ve düğümleri ayırma hakkında bilgi edinin
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e7eb414e51ca38f524ab83bfb51f80f771524287
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024441"
---
# <a name="quickstart---create-azure-vmware-solutions-avs-service"></a>Hızlı başlangıç-Azure VMware çözümleri (AVS) hizmeti oluşturma

Başlamak için Azure portal Azure VMware çözümlerini (AVS) oluşturun.

## <a name="vmware-solutions-avs---service-overview"></a>VMware çözümleri (AVS)-hizmete genel bakış

AVS hizmeti, AVS tarafından Azure VMware çözümünü kullanmanıza olanak sağlar. Hizmetin oluşturulması, düğüm sağlamanıza, düğümleri ayırmanızı ve AVS özel bulutları oluşturmanıza olanak sağlar. AVS hizmetini, AVS hizmetinin kullanılabildiği her bir Azure bölgesine eklersiniz. Hizmet, AVS tarafından Azure VMware çözümünün Edge ağını tanımlar. Bu uç ağ, AVS özel Bulutlarınıza VPN, ExpressRoute ve Internet bağlantısı içeren hizmetler için kullanılır.

AVS hizmetini eklemek için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, Edge ağı oluşturulurken kullanılır ve bir/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Şirket içi ağ adresi alanları veya Azure sanal ağ adres alanı ile çakışamaz.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ geçidi alt ağı için bir/28 CıDR bloğu ayırın. Bir ağ geçidi alt ağı her bir AVS hizmeti için gereklidir ve oluşturulduğu bölgeye özeldir. Ağ geçidi alt ağı, AVS uç ağ hizmetleri tarafından Azure VMware çözümü için kullanılır ve bir/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Bu, AVS ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. AVS ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur.

[Ağ önkoşullarını](cloudsimple-network-checklist.md)gözden geçirin. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **AVS hizmeti**için arama yapın.

    ![AVS hizmetinde ara](media/create-cloudsimple-service-search.png)

3. **AVS Hizmetleri**' ni seçin.
4. Yeni bir hizmet oluşturmak için **Ekle** ' ye tıklayın.

    ![AVS hizmeti Ekle](media/create-cloudsimple-service-add.png)

5. AVS hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CıDR girin. Şirket içi alt ağlarınızın, Azure alt ağlarının veya planlı AVS alt ağlarının hiçbiriyle çakışmayacak bir/28 alt ağı belirtin. Hizmet oluşturulduktan sonra CıDR 'yi değiştiremezsiniz.

    ![AVS hizmeti oluşturma](media/create-cloudsimple-service.png)

9. **Tamam**’a tıklayın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="provision-nodes"></a>Sağlama düğümleri

Bir AVS özel bulut ortamı için Kullandıkça Öde kapasitesini ayarlamak için, önce Azure portal düğümleri sağlayın.

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

* [AVS özel bulutu oluşturma ve ortamı yapılandırma](quickstart-create-private-cloud.md)
* [AVS hizmeti](https://docs.azure.cloudsimple.com/cloudsimple-service) hakkında daha fazla bilgi edinin
