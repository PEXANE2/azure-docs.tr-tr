---
title: Azure VMware çözümleri (AVS)-AVS hizmeti oluşturma
description: Azure portal AVS hizmetini nasıl oluşturacağınız açıklanmaktadır
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 468ef8751438812422d7eee83d98acc9e3aedb82
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024832"
---
# <a name="create-the-azure-vmware-solutions-avs-service"></a>Azure VMware Solutions (AVS) hizmetini oluşturma

Azure VMware çözümlerini (AVS) kullanmaya başlamak için Azure portal Azure VMware çözümleri (AVS) hizmetini oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ geçidi alt ağı için bir/28 CıDR bloğu ayırın. Bir ağ geçidi alt ağı her bir AVS hizmeti için gereklidir ve oluşturulduğu bölgeye özeldir. Ağ geçidi alt ağı, uç ağ hizmetleri için kullanılır ve/28 CıDR bloğu gerektirir. Ağ geçidi alt ağ adresi alanı benzersiz olmalıdır. Bu, AVS ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. AVS ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-the-service"></a>Hizmeti oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **AVS Hizmetleri**için arama yapın.
    ![arama AVS hizmeti](media/create-cloudsimple-service-search.png)
3. **AVS Hizmetleri**' ni seçin.
4. Yeni bir hizmet oluşturmak için **Ekle** ' ye tıklayın.
    ![AVS hizmeti](media/create-cloudsimple-service-add.png) Ekle
5. AVS hizmetini oluşturmak istediğiniz aboneliği seçin.
6. Hizmet için kaynak grubunu seçin. Yeni bir kaynak grubu eklemek için **Yeni oluştur**' a tıklayın.
7. Hizmeti tanımlamak için ad girin.
8. Hizmet ağ geçidi için CıDR girin. Şirket içi alt ağlarınızın, Azure alt ağlarının veya planlı AVS alt ağlarının hiçbiriyle çakışmayacak bir/28 alt ağı belirtin. Hizmet oluşturulduktan sonra CıDR 'yi değiştiremezsiniz.

    ![AVS hizmeti oluşturma](media/create-cloudsimple-service.png)
9. **Tamam**’a tıklayın.

Hizmet oluşturulur ve hizmetler listesine eklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Düğüm sağlamayı](create-nodes.md) öğrenin
* [AVS özel bulutu oluşturmayı](create-private-cloud.md) öğrenin
* [AVS özel bulut ortamının nasıl yapılandırılacağını](quickstart-create-private-cloud.md) öğrenin
