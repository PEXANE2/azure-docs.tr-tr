---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple hizmeti oluşturun
description: Azure portalında CloudSimple hizmetini nasıl oluşturabilirsiniz açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8648f2c9cc0175050d4b7642f5235d47159ecfaf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024832"
---
# <a name="create-the-azure-vmware-solution-by-cloudsimple-service"></a>CloudSimple hizmetine göre Azure VMware Çözüm'ü oluşturun

CloudSimple tarafından Azure VMware Solution'a başlamak için Azure portalında CloudSimple hizmetine göre Azure VMware Çözümü oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ geçidi alt ağı için bir /28 CIDR bloğu ayırın. CloudSimple hizmeti başına bir ağ geçidi alt ağı gereklidir ve oluşturulduğu bölgeye özgüdür. Ağ geçidi alt ağı kenar ağ hizmetleri için kullanılır ve bir /28 CIDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuranabilecek herhangi bir ağla örtüşmemelidir. CloudSimple ile iletişim sağlayan ağlar şirket içi ağları ve Azure sanal ağları içerir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Services'ı**arayın.
    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)
3. **CloudSimple Services'ı**seçin.
4. Yeni bir hizmet oluşturmak için **Ekle'yi** tıklatın.
    ![CloudSimple Hizmeti Ekle](media/create-cloudsimple-service-add.png)
5. CloudSimple hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni Oluştur'u**tıklatın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CIDR'yi girin. Şirket içi alt ağların, Azure alt ağların veya planlanan CloudSimple alt ağlarınizle çakışan bir /28 alt ağı belirtin. Hizmet oluşturulduktan sonra CIDR'yi değiştiremezsiniz.

    ![CloudSimple hizmetini oluşturma](media/create-cloudsimple-service.png)
9. **Tamam**'a tıklayın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Düğümleri nasıl [sağlarz öğreneceksin](create-nodes.md)
* [Özel bulut oluşturmayı](create-private-cloud.md) öğrenin
* Özel bulut ortamını nasıl [yapılandırılatırmayı](quickstart-create-private-cloud.md) öğrenin
